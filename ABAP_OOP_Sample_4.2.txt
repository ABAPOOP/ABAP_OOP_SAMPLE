"示例程序4.2
REPORT zrep_cls_002.

"declare class objects
DATA: go_class_matl1 TYPE REF TO zcl_class_matl.

"declare data
DATA: gv_i_name TYPE char30 VALUE 'importing into class'.
DATA: gv_e_name TYPE char30 VALUE 'exporting from class'.
DATA: gv_c_name TYPE char30 VALUE 'changing in class'.
DATA: gv_r_name TYPE char30 VALUE 'returning from class'.

"create objects
CREATE OBJECT go_class_matl1.

*call class instance
"assign value to objects
CALL METHOD go_class_matl1->test_parameter
  EXPORTING
    iv_name = gv_i_name "method importing parameter
  IMPORTING
    ev_name = gv_e_name "method exporting parameter
  CHANGING
    cv_name = gv_c_name "method changing parameter
  RECEIVING
    rv_name = gv_r_name. "method returning parameter

"Print parameters
WRITE :/ 'gv_i_name', gv_i_name.
WRITE :/ 'gv_e_name', gv_e_name.
WRITE :/ 'gv_c_name', gv_c_name.
WRITE :/ 'gv_r_name', gv_r_name.
