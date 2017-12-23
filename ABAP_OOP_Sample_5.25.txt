"示例程序5.25
REPORT zrep_cls_013.
"定义类对象
DATA go_super_matl TYPE REF TO zcl_super_matl.
DATA go_sub_matl_raw TYPE REF TO zcl_sub_matl_raw.
DATA go_sub_matl_semi TYPE REF TO zcl_sub_matl_semi.
DATA go_sub_matl_fin TYPE REF TO zcl_sub_matl_fin.

"创建类对象
CREATE OBJECT go_super_matl.
CREATE OBJECT go_sub_matl_raw.
CREATE OBJECT go_sub_matl_semi.
CREATE OBJECT go_sub_matl_fin.

"为各个类对象设定物料号码
go_super_matl->mv_material_id = 'MATL001'.
go_sub_matl_raw->mv_material_id = 'RAW001'.
go_sub_matl_semi->mv_material_id = 'SEMI001'.
go_sub_matl_fin->mv_material_id = 'FIN001'.

"测试物料类
WRITE: / '物料:'.
go_super_matl->print_material_desc( ).

"向上转型，测试多态下的原料类
WRITE: /,/ '原料:'.
go_super_matl = go_sub_matl_raw.
go_super_matl->print_material_desc( ).

"向上转型，测试多态下的半成品类
WRITE: /,/ '半成品:'.
go_super_matl = go_sub_matl_semi.
go_super_matl->print_material_desc( ).

"向上转型，测试多态下的成品类
WRITE: /,/ '成品:'.
go_super_matl = go_sub_matl_fin.
go_super_matl->print_material_desc( ).

"向下强转型，测试成品类自身方法
WRITE: /,/ '成品:'.
go_sub_matl_fin ?= go_super_matl.
go_sub_matl_fin->print_material_desc( ).
go_sub_matl_fin->print_material_type( ).
