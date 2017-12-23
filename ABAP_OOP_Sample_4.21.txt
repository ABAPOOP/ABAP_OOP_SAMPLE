"示例程序4.21
REPORT zrep_cls_t01.
DATA:
  go_obj TYPE REF TO cl_material_details_list.
"打印程序初始内存使用量
cl_abap_memory_utilities=>get_total_used_size( IMPORTING size = DATA(lv_init_size) ).
WRITE: / |内存初始用量: { lv_init_size } { 'Bytes' }| COLOR COL_POSITIVE.

DO 1000 TIMES.
  CREATE OBJECT go_obj.
  FREE go_obj.
ENDDO.

cl_abap_memory_utilities=>get_total_used_size( IMPORTING size = DATA(lv_before_size) ).
WRITE: / |创建并FREE类变量后内存用量: { lv_before_size } { 'Bytes' }| COLOR COL_GROUP.

cl_abap_memory_utilities=>do_garbage_collection( ).

cl_abap_memory_utilities=>get_total_used_size( IMPORTING size = DATA(lv_after_size) ).
WRITE: / |使用垃圾回收后内存用量: { lv_after_size } { 'Bytes' }| COLOR COL_TOTAL.
