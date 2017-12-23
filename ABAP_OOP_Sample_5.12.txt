"示例程序5.12
CLASS zcl_sub_class_matl_raw DEFINITION
  PUBLIC
  INHERITING FROM zcl_super_class_matl
  FINAL
  CREATE PUBLIC .
  PUBLIC SECTION.
    METHODS get_material_type
      IMPORTING
        !iv_material_id TYPE mara-matnr
      RETURNING
        VALUE(rv_material_type) TYPE mara-mtart .

    METHODS set_material_id
      REDEFINITION .
  PROTECTED SECTION.
  PRIVATE SECTION.

    DATA mv_material_type TYPE mara-mtart .
ENDCLASS.

CLASS zcl_sub_class_matl_raw IMPLEMENTATION.
  METHOD get_material_type.
    DATA: ls_mara TYPE mara.
    SELECT SINGLE mtart FROM mara
    INTO (ls_mara-mtart)
    WHERE matnr = iv_material_id.
    IF sy-subrc = 0.
      rv_material_type = ls_mara-mtart.
    ELSE.
      rv_material_type = ''.
    ENDIF.
    WRITE :/ 'MATERIAL TYPE:', rv_material_type .
  ENDMETHOD.

  METHOD set_material_id.
*CALL METHOD SUPER->SET_MATERIAL_ID
*  EXPORTING
*    IV_MATERIAL_ID =
*    .
    mv_material_id = iv_material_id.
    WRITE: / 'Raw Material: ',mv_material_id .
  ENDMETHOD.
ENDCLASS.
