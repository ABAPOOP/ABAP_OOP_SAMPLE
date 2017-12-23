"示例程序5.9
CLASS zcl_super_class_matl DEFINITION
  PUBLIC
  CREATE PUBLIC .
  PUBLIC SECTION.
    DATA mv_material_id TYPE mara-matnr .
    METHODS set_material_id
      IMPORTING
        !iv_material_id TYPE mara-matnr .
  PROTECTED SECTION.

    DATA mv_material_name TYPE makt-maktx .
    METHODS get_material_name
      RETURNING
        VALUE(rv_material_name) TYPE makt-maktx .
  PRIVATE SECTION.

    DATA mv_material_desc TYPE string .
    METHODS get_material_desc
      RETURNING
        VALUE(rv_material_desc) TYPE string .
ENDCLASS.

CLASS zcl_super_class_matl IMPLEMENTATION.
  METHOD get_material_desc.
    rv_material_desc = mv_material_desc.
  ENDMETHOD.

  METHOD get_material_name.
    rv_material_name = mv_material_name.
  ENDMETHOD.

  METHOD set_material_id.
    mv_material_id = iv_material_id.
    WRITE: / 'Material: ',mv_material_id .
  ENDMETHOD.
ENDCLASS.
