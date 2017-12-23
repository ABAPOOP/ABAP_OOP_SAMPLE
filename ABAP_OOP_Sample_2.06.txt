"示例程序2.6(HANA)
"本程序为SAP HANA 1610版本
REPORT zrep_cls_hana01.

"1. 定义变量
DATA gv_carrid TYPE scarr-carrid.
gv_carrid = 'LH'.

"2. 在内联定义内表 gt_scarr，传入参数为gv_carrid，前面标以@号表示为内联定义
SELECT *
       FROM scarr
       INTO TABLE @DATA(gt_scarr)
       WHERE carrid = @gv_carrid.

"3. 打印内表
cl_demo_output=>display_data( gt_scarr ).
IF line_exists( gt_scarr[ 1 ] ).
   cl_demo_output=>display_data( gt_scarr[ 1 ]-carrname ).
ENDIF.
