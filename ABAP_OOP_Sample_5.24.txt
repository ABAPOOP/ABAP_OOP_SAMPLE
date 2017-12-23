"示例程序5.24
CLASS zcl_sub_matl_fin DEFINITION
  PUBLIC
  INHERITING FROM zcl_super_matl
  CREATE PUBLIC .

  PUBLIC SECTION.

    METHODS print_material_type .

    METHODS print_material_desc
      REDEFINITION .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_sub_matl_fin IMPLEMENTATION.

  METHOD print_material_desc.
    CALL METHOD super->print_material_desc.
    WRITE: /'批次号码 ' , 'MF01FIN001'.
    WRITE: /'生产日期 ', sy-datum.
  ENDMETHOD.

  METHOD print_material_type.
    WRITE: /'物料类型 ' , 'FIN'.
  ENDMETHOD.
ENDCLASS.
