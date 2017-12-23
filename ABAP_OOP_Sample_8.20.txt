"示例程序8.20
REPORT zrep_cls_52.

PARAMETERS p_mty TYPE mara-mtart.
DATA: gv_material_type TYPE string.
gv_material_type = p_mty.
"声明变量，对应的是BADI
DATA: go_badi_handler TYPE REF TO zbadi_def_001.
"采用固定的格式，获得BADI的实例
GET BADI go_badi_handler
  FILTERS
    mater_type = gv_material_type.
"采用固定的格式，调用BADI的实现类方法
IF sy-subrc = 0.
  CALL BADI go_badi_handler->get_prod_order_info.
ENDIF.
