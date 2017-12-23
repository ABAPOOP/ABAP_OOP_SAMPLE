"示例程序6.9
REPORT zrep_cls_024.

DATA: go_order_operator           TYPE REF TO zcl_order_operator_1,
      go_read_order_from_db    TYPE REF TO zcl_read_order_db_1,
      gv_exref  TYPE REF TO cx_root,
      gv_msgtxt TYPE string.

"创建订单操作类和数据库读取订单类的对象实例
CREATE OBJECT go_order_operator.
CREATE OBJECT go_read_order_from_db.

IF go_order_operator IS NOT INITIAL AND
  go_read_order_from_db IS NOT INITIAL.
  TRY.
    "调用订单操作类的方法，传入参数为数据库读取订单类的实例
      CALL METHOD
      go_order_operator->read_order( go_read_order_from_db ).
      go_order_operator->process_order( ).
    CATCH cx_sy_dyn_call_error .
  ENDTRY.
ENDIF.
