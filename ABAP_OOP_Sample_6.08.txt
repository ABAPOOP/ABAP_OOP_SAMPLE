"示例程序6.8
REPORT zrep_cls_023.
DATA:
   lv_area TYPE i.
DATA go_rectangle TYPE REF TO zcl_rectangle.
DATA go_square TYPE REF TO zcl_square. 
START-OF-SELECTION.
  CREATE OBJECT go_square.
  go_square->set_width( 3 ).
  go_square->set_height( 6 ).
  lv_area = go_square->get_area( ).
  WRITE: / 'Area is:', lv_area.
  ASSERT lv_area = 18.
