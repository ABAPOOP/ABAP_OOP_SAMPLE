"示例程序5.13
REPORT zrep_cls_010.
DATA :
      "declare super material class
      go_super_material TYPE REF TO zcl_super_class_matl,
      "declare sub material class
      go_sub_material_raw TYPE REF TO zcl_sub_class_matl_raw,

      gv_material_id TYPE mara-matnr.

START-OF-SELECTION.

  PARAMETERS p_matnr TYPE mara-matnr. "material input
  gv_material_id = p_matnr.

  WRITE : /, / '1. 测试子类对象'.
  "create instance for sub class 创建子类对象
  CREATE OBJECT go_sub_material_raw.
  "Call Sub Class Methods 调用子类对象方法
  CALL METHOD go_sub_material_raw->set_material_id( p_matnr ).

  WRITE : /, / '2. 测试父类对象'.
  "create instance for super class 创建父类对象
  CREATE OBJECT go_super_material.
  "Call Super Class Methods 调用父类对象方法
  CALL METHOD go_super_material->set_material_id( p_matnr ).
