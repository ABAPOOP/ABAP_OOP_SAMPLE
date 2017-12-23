"示例程序5.21
CLASS zcl_sub_matl_semi DEFINITION
  PUBLIC
  INHERITING FROM zcl_super_matl
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.

    METHODS print_material_desc
      REDEFINITION .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_sub_matl_semi IMPLEMENTATION.

  METHOD print_material_desc.
    CALL METHOD super->print_material_desc.
    WRITE: /'批次号码 ', 'MF01SEM537'.
    WRITE: /'重检日期 ', sy-datum.
  ENDMETHOD.
ENDCLASS.
