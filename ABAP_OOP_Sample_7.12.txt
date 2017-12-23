"示例程序7.12
REPORT zrep_cls_027.

DATA:
  exc_ref TYPE REF TO cx_root,
  exc_text TYPE string,
  lv_order_type1 TYPE string,
  lv_order_type2 TYPE string,
  lv_doc_class1 TYPE string,
  lv_doc_class2 TYPE string.
DATA:
  go_prod_order           TYPE REF TO zcl_prod_order,
  gi_prod_order_output    TYPE REF TO zif_prod_order_output.
"类名必须大写
lv_order_type1 = 'ZCL_PROD_ORDER_PP01'.
lv_order_type2 = 'ZCL_PROD_ORDER_PP02'.
lv_doc_class1 = 'ZCL_PICK_LIST'.
lv_doc_class2 = 'ZCL_OPER_CONTROL_TICKET'.
TRY.
    "动态创建类，对生产订单类型和输出文档类型做灵活的组合
    "生产订单PP01与两种输出类型组合打印
    CREATE OBJECT go_prod_order TYPE (lv_order_type1).
    CREATE OBJECT gi_prod_order_output TYPE (lv_doc_class1).
    go_prod_order->print_order_document( gi_prod_order_output ).
    CREATE OBJECT gi_prod_order_output TYPE (lv_doc_class2).
    go_prod_order->print_order_document( gi_prod_order_output ).
    WRITE: /.
    "生产订单PP02与一种输出类型组合打印
    CREATE OBJECT go_prod_order TYPE (lv_order_type2).
    CREATE OBJECT gi_prod_order_output TYPE (lv_doc_class2).
    go_prod_order->print_order_document( gi_prod_order_output ).
  CATCH cx_sy_create_object_error INTO exc_ref.
    exc_text = exc_ref->get_text( ).
    MESSAGE exc_text TYPE 'I'.
ENDTRY.
