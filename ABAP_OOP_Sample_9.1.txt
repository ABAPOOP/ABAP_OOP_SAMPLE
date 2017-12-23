"示例程序9.1
CLASS zcl_call_report_variants IMPLEMENTATION.
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
            "调用SUBMIT方法，动态设定变量执行的程序和参数
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
