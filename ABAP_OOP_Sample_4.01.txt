"示例程序4.1
REPORT zrep_class_local_01.

* CLASS class_local DEFINITION
CLASS zlcl_class_local DEFINITION.
  PUBLIC SECTION.
    DATA:
    mv_value TYPE string. "Instance Attribute
    CLASS-DATA:
    gv_cl_value TYPE string. "Static Attribute
    METHODS:
    set_value IMPORTING iv_value TYPE string,
    display_value.
ENDCLASS.   "class_local DEFINITION


* CLASS class_local IMPLEMENTATION
CLASS zlcl_class_local IMPLEMENTATION.
  METHOD display_value.
    WRITE:/ 'Class Instance Attribute:', mv_value.
    WRITE:/ 'Class Static Attribute:', gv_cl_value.
  ENDMETHOD.                    "class_local IMPLEMENTATION

  METHOD set_value.
    mv_value = iv_value.
  ENDMETHOD.

ENDCLASS.



START-OF-SELECTION.
  DATA : lo_class_local TYPE REF TO zlcl_class_local. "DECLARE CLASS

  CREATE OBJECT lo_class_local. "CREATE OBJECT

  CALL METHOD lo_class_local->set_value
    EXPORTING
      iv_value = '<SAP ABAP OOP>'.

  zlcl_class_local=>gv_cl_value = 'IT Book Press'.
  CALL METHOD lo_class_local->display_value.
