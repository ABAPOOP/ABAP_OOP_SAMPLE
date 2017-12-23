"示例程序9.2 
CLASS zcl_call_report_variants DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.

    TYPES:
      BEGIN OF ty_report_variant,
                     report  TYPE vari-report,
                     variant TYPE vari-variant,
                     desc    TYPE varit-vtext,
                   END OF ty_report_variant .
    TYPES:
      ty_tab_report_variant
      TYPE STANDARD TABLE OF ty_report_variant .

    DATA mv_rep_id TYPE vari-report .
    DATA mv_wait TYPE n .
    DATA mv_variant_desc_c TYPE char20 .

    METHODS constructor
      IMPORTING
        !iv_rep_id TYPE vari-report OPTIONAL
        !iv_wait TYPE n OPTIONAL
        !iv_variant_desc_c TYPE char20 OPTIONAL .
    METHODS get_report_variants .
    METHODS call_report_by_variant .
  PROTECTED SECTION.
  PRIVATE SECTION.

    DATA mi_msglist TYPE REF TO if_reca_message_list .
    DATA ms_msg TYPE recamsg .
    DATA mt_report_variant TYPE ty_tab_report_variant .

    METHODS save_log
      IMPORTING
        !is_msg TYPE recamsg .
ENDCLASS.



CLASS zcl_call_report_variants IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_CALL_REPORT_VARIANTS->CALL_REPORT_BY_VARIANT
* +-------------------------------------------------------------------------------------------------+
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD call_report_by_variant.
    DATA: ls_report_variant TYPE ty_report_variant,
           ls_print_par TYPE pri_params.

    DATA: lv_exref  TYPE REF TO cx_root.
    DATA:
      "设定日志级别 1 – 9， 1表示该日志最为概略，9表示该日志最为细节
      lv_log_level TYPE i,
      lv_system_log_level TYPE i VALUE 9.

    "定义ABAP的宏，方便将消息按格式添加到类对象中
    DEFINE macro_build_msg.
      lv_log_level  = &7.
      "当日志级别大于等于控制打印级时，输出日志，否则不输出，节约系统资源
      IF lv_system_log_level GE lv_log_level.
        clear: ms_msg.
        ms_msg-msgty = &1.
        ms_msg-msgid = 'Z_OOP_MSG'.
        ms_msg-msgno = &2.
        ms_msg-msgv1 = &3.
        ms_msg-msgv2 = &4.
        ms_msg-msgv3 = &5.
        ms_msg-msgv4 = &6.
        ms_msg-detlevel = &7.
      ENDIF.
    END-OF-DEFINITION.

    IF mt_report_variant[] IS NOT INITIAL.
      "循环程序变量内表
      LOOP AT mt_report_variant INTO ls_report_variant.
        TRY.
            "调用SUBMIT方法，设定变量执行的程序
            SUBMIT (mv_rep_id)
                    USING SELECTION-SET ls_report_variant-variant
                    TO SAP-SPOOL
                    SPOOL PARAMETERS ls_print_par WITHOUT SPOOL DYNPRO
                    AND RETURN.
            IF mv_wait IS INITIAL.
              mv_wait = 5.
            ENDIF.
            WAIT UP TO mv_wait SECONDS.
            "调用宏，将不同类型和级别的消息将加到类对象中
            macro_build_msg 'I' '002' 'Program ' mv_rep_id
            'runs with variant ' ls_report_variant-variant '3'.
            CALL METHOD me->save_log( ms_msg ).
          CATCH cx_root INTO lv_exref.
            "调用宏，将不同类型和级别的消息将加到类对象中
            macro_build_msg 'E' '002' 'Program ' mv_rep_id
            'runs with variant ' ls_report_variant-variant '3'.
            CALL METHOD me->save_log( ms_msg ).
        ENDTRY.
      ENDLOOP.
    ENDIF.
  ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_CALL_REPORT_VARIANTS->CONSTRUCTOR
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_REP_ID                      TYPE        VARI-REPORT(optional)
* | [--->] IV_WAIT                        TYPE        N(optional)
* | [--->] IV_VARIANT_DESC_C              TYPE        CHAR20(optional)
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD constructor.
    DATA:
      "定义日志 Object 类型
      lv_object TYPE balobj_d VALUE 'Z_OOP_LOG',
      "定义日志Object子类型
      lv_subobject TYPE balsubobj VALUE 'REPORT',
      "定义外部信息标识ID
      lv_msg_ext TYPE balnrext VALUE 'OO_CALL_REP',
      "定义Message ID信息标识
      lv_msgid TYPE sy-msgid VALUE 'Z_OOP_MSG',
      lv_msg_text    TYPE symsgv.

    "创建日志对象 mi_msglist，传入为日志类型，子类型，外部信息
    CALL METHOD cf_reca_message_list=>create
      EXPORTING
        id_object    = lv_object   "(SLG0)
        id_subobject = lv_subobject  " (SLG0)
        id_extnumber = lv_msg_ext "external number
      RECEIVING
        ro_instance  = mi_msglist.

    mv_rep_id	 = iv_rep_id.
    mv_wait = iv_wait.
    mv_variant_desc_c	= iv_variant_desc_c.
  ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_CALL_REPORT_VARIANTS->GET_REPORT_VARIANTS
* +-------------------------------------------------------------------------------------------------+
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD get_report_variants.
    DATA: ls_report_variant TYPE ty_report_variant.
    "按参数的DESC包含的字段来定义分组信息
    CONCATENATE '%'  mv_variant_desc_c '%' INTO mv_variant_desc_c.
    "查找系统报表变量表，来确定要执行的报表的参数列表
    SELECT vari~report vari~variant varit~vtext
                        INTO TABLE mt_report_variant
                        FROM vari INNER JOIN varit
                        ON ( vari~report = varit~report
                        AND vari~variant = varit~variant )
                        WHERE vari~relid   = 'VA'       AND
                              vari~report  = mv_rep_id   AND
                              varit~langu   = 'E'       AND
                              varit~vtext LIKE mv_variant_desc_c
                        ORDER BY vari~variant.
    "去除参数列表 中的系统冗余记录
    IF sy-subrc EQ 0.
      LOOP AT mt_report_variant INTO ls_report_variant.
        IF ls_report_variant-variant(1) EQ '&'.
          DELETE TABLE mt_report_variant FROM ls_report_variant.
        ENDIF.
      ENDLOOP.
    ENDIF.
  ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Private Method ZCL_CALL_REPORT_VARIANTS->SAVE_LOG
* +-------------------------------------------------------------------------------------------------+
* | [--->] IS_MSG                         TYPE        RECAMSG
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD save_log.

    "调用add方法，将消息添加入对象
    mi_msglist->add( is_message = is_msg ).

    "调用对象方法，保存消息到数据库
    CALL METHOD mi_msglist->store
      EXPORTING
        if_in_update_task = abap_false
      EXCEPTIONS
        error             = 1
        OTHERS            = 2.
    IF sy-subrc <> 0.
      "出错时不处理
    ENDIF.
    "提交到数据库
    cf_reca_storable=>commit( ).

  ENDMETHOD.
ENDCLASS.