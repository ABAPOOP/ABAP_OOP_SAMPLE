"示例程序8.13
REPORT zrep_cls_039.
"Function ALV
"设置内表
DATA gt_itab TYPE TABLE OF sflight.
SELECT * FROM sflight INTO TABLE gt_itab UP TO 10 ROWS.

"调用Funciton ALV 功能模块将数据在ALV控件中显示
CALL FUNCTION 'REUSE_ALV_LIST_DISPLAY'
EXPORTING
  i_structure_name = 'SFLIGHT'
TABLES
  t_outtab = gt_itab.

