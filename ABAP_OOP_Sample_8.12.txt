"示例程序8.12
REPORT zrep_cls_c03.

DATA:
"定义消息变量
  gs_msg          TYPE recamsg,
"定义日志类对象
  gi_msglist      TYPE REF TO if_reca_message_list.
DATA:
  "定义日志 Object 类型
  gv_object TYPE balobj_d VALUE 'Z_OOP_LOG',
  "定义日志Object子类型
  gv_subobject TYPE balsubobj VALUE 'REPORT',
  "定义外部信息标识ID
  gv_msg_ext TYPE balnrext VALUE 'OO_Log_Test_2',
  "定义Message ID信息标识
  gv_msgid TYPE sy-msgid VALUE 'Z_OOP_MSG',
  "定义Message ID信息号码
  gv_msgno_000    TYPE sy-msgno   VALUE 000,
  gv_msgno_001    TYPE sy-msgno   VALUE 001,
  gv_msgno_002    TYPE sy-msgno   VALUE 002,
  gv_msg_text    TYPE symsgv,
  "设定日志级别 1 – 9， 1表示该日志最为概略，9表示该日志最为细节
  gv_log_level TYPE i,
  "设定日志控制级别，模拟该参数从自定义数据表中读取，用于控制打印级别
"1打印最概略的日志，9打印最细节的日志
  gv_system_log_level TYPE i VALUE 9.


"定义ABAP的宏，方便将消息按格式添加到类对象中
DEFINE macro_build_msg.
  gv_log_level  = &7.
  "当日志级别大于等于控制打印级时，输出日志，否则不输出，节约系统资源
  IF gv_system_log_level GE gv_log_level.
    clear: gs_msg.
    gs_msg-msgty = &1.
    gs_msg-msgid = gv_msgid.
    gs_msg-msgno = &2.
    gs_msg-msgv1 = &3.
    gs_msg-msgv2 = &4.
    gs_msg-msgv3 = &5.
    gs_msg-msgv4 = &6.
    gs_msg-detlevel = &7.
    "调用add方法，将消息添加入对象
    gi_msglist->add( is_message = gs_msg ).
  ENDIF.
END-OF-DEFINITION.

START-OF-SELECTION.
  "创建日志对象 gi_msglist，传入为日志类型，子类型，外部信息
  CALL METHOD cf_reca_message_list=>create
    EXPORTING
       id_object    = gv_object   "(SLG0)
       id_subobject = gv_subobject  " (SLG0)
       id_extnumber = gv_msg_ext "external number
    RECEIVING
      ro_instance  = gi_msglist.

  "调用宏，将不同类型和级别的消息将加到类对象中
  macro_build_msg 'A' '002' 'Log ' syst-uname syst-datum syst-uzeit '1'.
  macro_build_msg 'E' '002' 'Get variant ' sy-cprog space space '2' .
  macro_build_msg 'I' '000' space space space space '3'.
  macro_build_msg 'S' '001' 'get_stock' space space space '4'.
  macro_build_msg 'W' '000' space space space space '5'.
  macro_build_msg 'X' '000' space space space space '6'.

  "调用对象方法，保存消息到数据库
  CALL METHOD gi_msglist->store
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
