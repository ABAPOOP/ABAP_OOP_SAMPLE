"示例程序8.2
REPORT zrep_cls_020.
CLASS zcl_product DEFINITION
  CREATE PUBLIC .
  PUBLIC SECTION.
    METHODS print_desc.
ENDCLASS.
CLASS zcl_product IMPLEMENTATION.
  METHOD print_desc.
    WRITE: / 'Print PRODUCT'.
  ENDMETHOD.
ENDCLASS.
START-OF-SELECTION.
  DATA:
     exc_ref TYPE REF TO cx_root,
     exc_text TYPE string,
     lv_type_name TYPE string,
     lv_method_name TYPE string.
  DATA lo_product TYPE REF TO zcl_product.
  lv_type_name = 'ZCL_PRODUCT'. "类名必须大写
  lv_method_name = 'PRINT_DESC'. "方法名必须大写
  TRY.
      CREATE OBJECT lo_product TYPE  (lv_type_name).
      CALL METHOD lo_product->(lv_method_name).
    CATCH cx_sy_create_object_error INTO exc_ref.
      exc_text = exc_ref->get_text( ).
      MESSAGE exc_text TYPE 'I'.
  ENDTRY.
