"示例程序8.15 
REPORT zrep_cls_041.

"SALV （OOALV）

"定义内表，定义SALV 的类CL_GUI_SALV_TABLE的对象变量
DATA gt_itab_salv TYPE TABLE OF sflight.
DATA go_table TYPE REF TO cl_salv_table.
DATA go_functions TYPE REF TO cl_salv_functions_list.

SELECT * FROM sflight INTO CORRESPONDING FIELDS OF TABLE gt_itab_salv.
"调用类方法创建类对象
CALL METHOD cl_salv_table=>factory
IMPORTING
  r_salv_table = go_table
  CHANGING
    t_table = gt_itab_salv.

"调用类方法显示SALV
go_functions = go_table->get_functions( ).
go_functions->set_all( abap_true ).
go_table->display( ).
