"示例程序5.17
class ZCL_SUPER_MATL definition
  public
  create public .

public section.

  data MV_MATERIAL_ID type STRING .

methods PRINT_MATERIAL_DESC .
protected section.
private section.
ENDCLASS.

CLASS ZCL_SUPER_MATL IMPLEMENTATION.

  METHOD print_material_desc.
    WRITE: / '物料号码 ',  mv_material_id.
  ENDMETHOD.
ENDCLASS.
