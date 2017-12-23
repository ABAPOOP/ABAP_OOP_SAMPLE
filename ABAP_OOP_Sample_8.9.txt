"示例程序8.9
REPORT zrep_cls_c11.

PARAMETERS dbtab TYPE c LENGTH 20.

"定义通用类型
DATA gt_table TYPE REF TO data.
"定义字段类型
FIELD-SYMBOLS <fs_table> TYPE STANDARD TABLE.
"定义字段类型，<struc>代表结构体；<comp>代表结构体的数据列
FIELD-SYMBOLS: <struc> TYPE any,
                  <comp>  TYPE any.
TRY.
    "创建内表的引用数据类型
    CREATE DATA gt_table TYPE TABLE OF (dbtab).
    "将引用的内表赋予字段类型
    ASSIGN gt_table->* TO <fs_table>.
    "动态查询数据库表(dbtab)，存储于内表类型的字段符号<fs_table>
    SELECT *
          FROM (dbtab)
          INTO TABLE <fs_table>.
    "循环内表<fs_table>，获取工作区结构体赋予字段类型<line1>
    LOOP AT <fs_table> ASSIGNING FIELD-SYMBOL(<line1>).
      "将工作区结构体<line1>的第2个列的值取出，赋予<comp>
      ASSIGN COMPONENT 3 OF STRUCTURE <line1> TO <comp> .
      "打印<comp>的值
      WRITE: / <comp>.
    ENDLOOP.
    SKIP 2.
    "判断内表<fs_table>的第1条记录是否存在
    IF line_exists( <fs_table>[ 1 ] ).
      ""将工作区结构体<line1>的第6列的值取出，赋予<comp>
      ASSIGN COMPONENT 6 OF STRUCTURE <fs_table>[ 1 ] TO <comp> .
      "打印<comp>的值
      WRITE: / <comp>.
    ENDIF.

  CATCH cx_sy_create_data_error cx_sy_dynamic_osql_error.
ENDTRY.
