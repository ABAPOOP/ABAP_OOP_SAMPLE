"示例程序5.19
CLASS zcl_sub_matl_raw DEFINITION
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

CLASS zcl_sub_matl_raw IMPLEMENTATION.

  METHOD print_material_desc.
    CALL METHOD super->print_material_desc.
    WRITE: /'供应商号码 ', 'VEN0127'.
    WRITE: /'批次号码 ', 'MF01RAW209'.
    WRITE: /'采购日期 ', sy-datum.
  ENDMETHOD.
ENDCLASS.
