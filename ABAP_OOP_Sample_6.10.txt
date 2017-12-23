"示例程序6.10
REPORT zrep_cls_025.

DATA: gi_read_order            TYPE REF TO zif_read_order,
      go_order_operator        TYPE REF TO zcl_order_operator,
      go_read_order_db    TYPE REF TO zcl_read_order_db,
      go_read_order_xml   TYPE REF TO zcl_read_order_xml.

"创建订单操作类和数据库读取订单类的对象实例
CREATE OBJECT go_order_operator.
CREATE OBJECT go_read_order_db.

"创建订单操作类和数据库读取订单类的对象实例
IF go_order_operator IS NOT INITIAL
  AND go_read_order_db IS NOT INITIAL.
  "调用订单操作类的方法，传入参数为实现接口的数据库读取订单类的实例
  CALL METHOD go_order_operator->read_order( go_read_order_db ).
  CALL METHOD go_order_operator->process_order( ).
ENDIF.

CREATE OBJECT go_read_order_xml.

IF go_order_operator IS NOT INITIAL
  AND go_read_order_xml IS NOT INITIAL.
  "如果底层功能修改，不用去修改订单操作类，仅对调用代码简单修改即可 
  "调用订单操作类的方法，传入参数为实现接口的数据库读取订单类的实例
  CALL METHOD go_order_operator->read_order( go_read_order_xml ).
  CALL METHOD go_order_operator->process_order( ).
ENDIF.
