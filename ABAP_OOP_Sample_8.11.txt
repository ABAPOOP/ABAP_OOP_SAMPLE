"示例程序8.11
REPORT zrep_cls_c05.

DATA:
"定义消息变量
  gs_msg          TYPE recamsg,
"定义日志类对象
  go_slg1_log      TYPE REF TO cl_cux_appl_log.

DATA:
  gs_log         TYPE bal_s_log,
  gs_log_msg  TYPE bal_s_msg,
  gt_log_handle   TYPE bal_t_logh,
  gv_log_handle   TYPE balloghndl,
"定义消息号码
gv_msgno_000    TYPE sy-msgno   VALUE 000,
  gv_msgno_001    TYPE sy-msgno   VALUE 001,
  gv_msgno_002    TYPE sy-msgno   VALUE 002,
  gv_msg_text    TYPE symsgv.

START-OF-SELECTION.
"创建日志对象实例，传入定义的SLG0的对象，子对象和外部信息
  CREATE OBJECT go_slg1_log
  EXPORTING
    "iv_handle = gv_log_handle
    iv_log_name = 'ABAP_OO_LOG_TEST_1'
    iv_object = 'Z_OOP_LOG'
    iv_subobject = 'REPORT'.
    "iv_default_msgid = 'Z_OOP_MSG'.

gv_msg_text = syst-uzeit.

"调用日志对象实例方法，传入消息类型S，消息类型名称，消息号码，参数等信息
  CALL METHOD go_slg1_log->add_message
    EXPORTING
      iv_msgty = 'S'
      iv_msgid = 'Z_OOP_MSG'
      iv_msgno = '002'
      iv_msgv1 = 'Log'
      iv_msgv2 = 'is printed'
      iv_msgv3 = 'at'
      iv_msgv4 = gv_msg_text.

"调用日志对象实例方法，保存日志
  CALL METHOD go_slg1_log->save_messages.
  IF sy-subrc <> 0.
"出错时不处理
  ENDIF. 

