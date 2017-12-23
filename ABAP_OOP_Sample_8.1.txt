"示例程序8.1
REPORT zrep_cls_c06.
"定义本地类
CLASS zlcl_class DEFINITION.
  PUBLIC SECTION.
    DATA: mv_value TYPE string.
    METHODS:
    set_value IMPORTING iv_value TYPE string,
    display_value.
ENDCLASS.
CLASS zlcl_class IMPLEMENTATION.
  METHOD display_value.
    WRITE:/ 'Class Instance Attribute:', mv_value.
  ENDMETHOD.
  METHOD set_value.
    mv_value = iv_value.
  ENDMETHOD.
ENDCLASS.
 
START-OF-SELECTION.
  TYPES: ty_i TYPE i.
  DATA: gv_dref TYPE REF TO ty_i .
  DATA: go_oref TYPE REF TO zlcl_class.

  DATA:
  exc_ref TYPE REF TO cx_root,
  exc_text TYPE string.

  "根据基本类型名动态创建数据
  CREATE DATA gv_dref TYPE ('ty_i').
  gv_dref->* = 1.
  WRITE: / gv_dref->*." 1

  TRY.
      "根据类名动态创建实例对象ZLCL_CLASS， 类名必须大写
      CREATE OBJECT go_oref TYPE ('ZLCL_CLASS').
      go_oref->set_value(' Test String ').
      go_oref->display_value( ).
    CATCH cx_sy_create_object_error INTO exc_ref.
      exc_text = exc_ref->get_text( ).
      MESSAGE exc_text TYPE 'I'.
  ENDTRY.
