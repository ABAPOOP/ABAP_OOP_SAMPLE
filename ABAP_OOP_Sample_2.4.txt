"示例程序2.4
REPORT zrep_cls_10.

DATA:
    gv_var_value1(50) TYPE c VALUE 'Pedro Paramo',
    gv_var_value2(50) TYPE c VALUE 'The Burning Plain and other Stories',
    gv_var_value3(50) TYPE c VALUE 'One Hundred Years of Solitude'.

DATA gv_var_name(30)  TYPE c.
DATA gv_book_name(50) TYPE c.
FIELD-SYMBOLS : <fs_book_name>.

"用括号操作符(gv_var_name)，将变量 gv_var_value1动态赋给变量 gv_book_name
gv_var_name =  'gv_var_value1'.
WRITE (gv_var_name) TO gv_book_name.
WRITE / gv_book_name.

"用assign，括号操作符，将变量 gv_var_value2动态赋给字段符号 <fs_book_name>
gv_var_name =  'gv_var_value2'.
ASSIGN (gv_var_name) TO <fs_book_name>.
WRITE / <fs_book_name>.

"如果只用assign，不采用括号操作符，
"是不能将变量 gv_var_value2动态赋给字段符号 <fs_book_name>
"只能 ‘gv_var_value2’作为字符串赋给字段符号 <fs_book_name>
gv_var_name =  'gv_var_value2'.
ASSIGN gv_var_name TO <fs_book_name>.
WRITE / <fs_book_name>.

"字符串GV_var_VALUE3和 gv_var_value3作为字符串时因大小写不一致而不相同，
"但在指定变量时，因为ABAP语言变量是大小不敏感的，所以指向相同
"用括号操作符(gv_var_name)，将变量 gv_var_value3动态赋给变量 gv_book_name
gv_var_name =  'GV_var_VALUE3'.
WRITE (gv_var_name) TO gv_book_name.
WRITE / gv_book_name.
