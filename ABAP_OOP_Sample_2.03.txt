"示例程序2.3
REPORT zrep_cls_09.

DATA:    gv_counter  TYPE i VALUE 0.
WRITE: / '     gv_counter', '  lv_counter', '  lv_static'.
DO 3 TIMES.
  PERFORM add_value.
  PERFORM sub_value.
ENDDO.
WRITE: / 'gv_counter', gv_counter.
FORM add_value.
  "局部变量，子程序运行完后即丢失值
  DATA: lv_counter  TYPE i VALUE 1.
  "静态局部变量，可以一直保持值
  STATICS: lv_static TYPE i VALUE 0.
  gv_counter  = gv_counter  + 1.
  lv_counter  = lv_counter  + 1.
  lv_static = lv_static + 1.
  WRITE: / 'add+', gv_counter, lv_counter, lv_static.
ENDFORM.                    "add_value
FORM sub_value.
  "局部变量，子程序运行完后即丢失值
  DATA: lv_counter  TYPE i VALUE 1.
  "静态局部变量，可以一直保持值
  STATICS: lv_static TYPE i VALUE 9.
  gv_counter  = gv_counter  - 1.
  lv_counter  = lv_counter  - 1.
  lv_static = lv_static - 1.
  WRITE: / 'sub-', gv_counter, lv_counter, lv_static.
ENDFORM.                    "sub_value
