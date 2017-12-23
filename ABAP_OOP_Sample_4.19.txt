"示例程序4.19
REPORT zrep_cls_008.
"declare class objects
DATA : go_master TYPE REF TO zcl_master.
DATA : go_master_new TYPE REF TO zcl_master.
"create objects
CREATE OBJECT go_master.
*call class instance
go_master->display_public( ).
"go_master->DISPLAY_PRIVATE( ).
"go_master->DISPLAY_PROTECTED( ).

NEW zcl_master( )->display_public( ).
