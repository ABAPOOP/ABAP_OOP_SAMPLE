"示例程序8.14 
REPORT zrep_cls_040.

"GRID ALV （OOALV）

"定义内表，定义GRID ALV 的类CL_GUI_ALV_GRID的对象变量
DATA: go_alv TYPE REF TO cl_gui_alv_grid,
      gt_sflight TYPE STANDARD TABLE OF sflight.

SELECT * FROM sflight INTO TABLE gt_sflight.
"创建类变量，指定ALV的容器container
CREATE OBJECT go_alv
    EXPORTING i_parent = cl_gui_container=>screen0.
"调用类方法显示GRID ALV
CALL METHOD go_alv->set_table_for_first_display
    EXPORTING
       i_structure_name = 'SFLIGHT'
    CHANGING
       it_outtab = gt_sflight.

"调用屏幕100，屏幕需要
CALL SCREEN 100.

"定义状态栏按钮的对应逻辑
MODULE user_command_0100 INPUT.
  CASE sy-ucomm.
    WHEN 'BACK'.
      LEAVE TO SCREEN 0.
  ENDCASE.
ENDMODULE.

"定义显示逻辑
MODULE status_0100 OUTPUT.
  SET PF-STATUS 'SCREEN0'.
  SET TITLEBAR 'OO GRID ALV'.
ENDMODULE.                 " STATUS_0100  OUTPUT
 
