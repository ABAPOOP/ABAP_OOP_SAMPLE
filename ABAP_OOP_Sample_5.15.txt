REPORT zrep_cls_015.
" 定义素食类
CLASS
  zcl_vegetarian_food DEFINITION.
ENDCLASS.
" 定义水果类，继承自素食类
CLASS
  zcl_fruit DEFINITION INHERITING FROM zcl_vegetarian_food.
ENDCLASS.

" 定义苹果类，继承自水果类
CLASS
  zcl_apple DEFINITION INHERITING FROM zcl_fruit.
ENDCLASS.

" 定义梨子类，继承自水果类
CLASS
  zcl_pear DEFINITION INHERITING FROM zcl_apple.
ENDCLASS.
" 定义类变量
DATA go_vegetarian_food TYPE REF TO zcl_vegetarian_food.
DATA go_fruit TYPE REF TO zcl_fruit.
DATA go_apple TYPE REF TO zcl_apple.
DATA go_pear TYPE REF TO zcl_pear.


"第1种向下转型，如果子类经过向上转型到父类，则父类对象可以直接向下强转型。
"向下强转型，采用 “?=”进行赋值

" 先创建对象
CREATE OBJECT go_vegetarian_food TYPE zcl_vegetarian_food .
CREATE OBJECT go_apple TYPE zcl_apple.

" 向上转型
go_vegetarian_food = go_apple.

" 然后用“?=”下转型
TRY.
    go_apple ?= go_vegetarian_food.
    WRITE: / 'go_apple ?= go_vegetarian_food - Correct'.
  CATCH cx_sy_move_cast_error.
    WRITE: / 'go_apple ?= go_vegetarian_food - Error'.
ENDTRY.

"第2种向下转型，如果子类经过向上转型到父类，则父类对象可以直接向下强转型
"向下强转型，采用 “MOVE ？TO”进行赋值

" 先创建对象
CREATE OBJECT go_vegetarian_food TYPE zcl_vegetarian_food .
CREATE OBJECT go_pear TYPE zcl_pear.

" 向上转型
go_vegetarian_food = go_pear.

" 然后用“MOVE ？TO”下转型
TRY.
    MOVE go_vegetarian_food ?TO go_pear.
    WRITE: / 'MOVE go_vegetarian_food ?TO go_pear - Correct'.
  CATCH cx_sy_move_cast_error.
    WRITE: / 'MOVE go_vegetarian_food ?TO go_pear - Error'.
ENDTRY.

"第3种向下转型，如果子类经过向上转型到父类，则父类对象可以直接向下强转型
"向下强转型，采用 “CAST”进行赋值

" 先创建对象
CREATE OBJECT go_vegetarian_food TYPE zcl_vegetarian_food.
CREATE OBJECT go_pear TYPE zcl_pear.

" 向上转型
go_vegetarian_food = go_pear.

" 然后用“CAST”下转型
TRY.
    CAST zcl_pear( go_vegetarian_food  ).
    WRITE: / 'CAST zcl_pear( go_vegetarian_food  ) - Correct'.
  CATCH cx_sy_move_cast_error.
    WRITE: / 'CAST zcl_pear( go_vegetarian_food  ) - Error'.
ENDTRY.


"第4种向下转型，如果父类对象没有经过子类的向上转型，直接进行向下强转型，会引发错误。

" 先创建对象
CREATE OBJECT go_vegetarian_food TYPE zcl_vegetarian_food.
CREATE OBJECT go_pear TYPE zcl_pear.

" 没有经过子类的向上转型
"go_vegetarian_food = go_pear.

" 此时用“CAST”下转型,会引发错误。
TRY.
    CAST zcl_pear( go_vegetarian_food  ).
    WRITE: / 'CAST zcl_pear( go_vegetarian_food  ) - Correct'.
  CATCH cx_sy_move_cast_error.
    WRITE: / 'CAST zcl_pear( go_vegetarian_food  ) - Error'.
ENDTRY.
