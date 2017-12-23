"示例程序8.3
REPORT ZREP_CLS_C08.
"数值类型的字段符号测试
FIELD-SYMBOLS: <fs1> TYPE i.
DATA: gv_data TYPE i VALUE 1000 .
"字段符号的赋值
ASSIGN gv_data TO <fs1> .
WRITE:/ <fs1> .

"结构体类型的字段符号测试
DATA: BEGIN OF ty_struc,
         id(20) TYPE c VALUE ' 1930 ',
         name(20) TYPE c VALUE ' Heinz ',
       END OF ty_struc.
FIELD-SYMBOLS <fs2> LIKE ty_struc.
"结构体字段符号的赋值
ASSIGN ty_struc TO <fs2>.
WRITE / <fs2>-id.
"字段符号的赋值
MOVE <fs2>-name TO <fs2>-id.
WRITE / <fs2>-id.

"内表类型的字段符号测试
TYPES: BEGIN OF ty_line,
         col1 TYPE c,
         col2 TYPE string,
       END OF ty_line.
DATA: wa TYPE ty_line,
      itab TYPE HASHED TABLE OF ty_line WITH UNIQUE KEY col1,
      key(4) TYPE c VALUE 'COL1'.
FIELD-SYMBOLS <fs3> TYPE ANY TABLE.
"内表类型的字段符号的赋值
ASSIGN itab TO <fs3>.
wa-col1 = 'X'.
wa-col2 = ' Carl '.
"内表类型的字段符号的操作
INSERT wa INTO TABLE <fs3>.
READ TABLE <fs3> WITH TABLE KEY (key) = 'X' INTO wa.
WRITE / wa-col1.
WRITE / wa-col2.
