"示例程序7.4
REPORT zrep_cls_c02.

START-OF-SELECTION.
  "定义两个类变量，类型都为ZCL_LOG
  DATA: go_zcl_log_1 TYPE REF TO zcl_log.
  DATA: go_zcl_log_2 TYPE REF TO zcl_log. 

  "采用类ZCL_LOG 的 =>形式的调用，调用静态方法GET_INSTANCE
  go_zcl_log_1 = zcl_log=>get_instance( ).
  "打印字符串属性
  go_zcl_log_1->print_msg( ).
  "为类对象字符串属性设定值
  go_zcl_log_1->set_msg( 'Instance object 0001 message' ).
  "打印字符串属性
  go_zcl_log_1->print_msg( ).
  "再次采用类ZCL_LOG 的 =>形式的调用，调用静态方法GET_INSTANCE
  go_zcl_log_2 = zcl_log=>get_instance( ).
  "打印字符串属性，打印的是一个字符串，即GET_INSTANCE方法返回的是同一个实例。
  go_zcl_log_2->print_msg( ).

  "判断两个类实例是否相同
  IF go_zcl_log_1 EQ go_zcl_log_2.
    WRITE: / 'Equal'.
  ELSE.
    WRITE: / 'Not Equal'.
  ENDIF.

