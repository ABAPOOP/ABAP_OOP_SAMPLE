"示例程序5.14
REPORT zrep_cls_0151.

"父类水果，从ABAP根类object继承
CLASS
  zcl_fruit DEFINITION INHERITING FROM object.
ENDCLASS.
"子类苹果，继承自水果类
CLASS
  zcl_apple DEFINITION INHERITING FROM zcl_fruit.
ENDCLASS.
"子类梨子，继承自水果类
CLASS
  zcl_pear DEFINITION INHERITING FROM zcl_apple.
ENDCLASS.
"定义类对象
DATA go_fruit TYPE REF TO zcl_fruit.
DATA go_apple TYPE REF TO zcl_apple.
DATA go_pear TYPE REF TO zcl_pear.

"第1种上转型，按子类类型创建父类对象
"父类是可创建对象实例的类，或者抽象类和接口
CREATE OBJECT go_fruit TYPE zcl_apple.
"或者使用NEW语句
go_fruit = NEW zcl_apple( ).

"第2种上转型，如果父类是可创建对象实例的类，
"分别创建子类和父类对象后，采用一般赋值语句
CREATE OBJECT go_fruit.
CREATE OBJECT go_apple.
go_fruit = go_apple.

"第3种上转型，如果父类是可创建对象实例的类，
"分别创建子类和父类对象后，采用MOVE TO 语句
CREATE OBJECT go_fruit.
CREATE OBJECT go_apple.
MOVE go_apple TO go_fruit.
