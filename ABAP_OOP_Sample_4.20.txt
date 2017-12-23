"示例程序4.20
REPORT zrep_cls_009.

"定义调用类，定义类静态方法
CLASS zcl_main_report DEFINITION.
  PUBLIC SECTION.
    CLASS-METHODS: run.
ENDCLASS.

"实现调用类方法，定义类静态方法
CLASS zcl_main_report IMPLEMENTATION.
  METHOD run.
    "创建对象 调用对象的方法
    NEW zcl_master( )->display_public( ).
  ENDMETHOD.
ENDCLASS.

"定义程序可执行块调用对象的静态方法

START-OF-SELECTION.
  zcl_main_report=>run( ).
