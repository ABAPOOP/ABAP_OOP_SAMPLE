"示例程序2.8(HANA)
"本程序为SAP HANA 1610版本
REPORT zrep_cls_hana03.

TYPES:
  "设定内表和工作区结构体的类型
  BEGIN OF ty_airline,
    airline_id  TYPE scarr-carrid,
    airline_name TYPE scarr-carrname,
    airline_currency TYPE scarr-currcode,
    url TYPE scarr-url,
    url2 TYPE string,
  END OF ty_airline.

"设定承载SQL语句的变量，返回结果变量，条件变量
DATA:
  lv_statement TYPE string,
  lt_airline TYPE TABLE OF ty_airline,
  lr_result TYPE REF TO data,
  lv_currency TYPE scarr-currcode,

"设定ADBC相关类的连接对象，SQL对象，结果对象，异常对象的变量
  lo_conn TYPE REF TO cl_sql_connection,
  lo_statement TYPE REF TO cl_sql_statement,
  lo_result_set TYPE REF TO cl_sql_result_set,
  lx_sql TYPE REF TO cx_sql_exception.

"关联内表到结果对象变量中
GET REFERENCE OF lt_airline INTO lr_result.

"设定条件变量的值
lv_currency = 'EUR'.

" 拼接HANA NATIVE SQL，该语法是 HANA 的 SQL Script支持的语法
" 也可以调用SQL Script的函数如 SUBSTRING 
 lv_statement = | SELECT CARRID, CARRNAME, CURRCODE, URL,  |
&& | SUBSTRING (URL,8,LENGTH(URL)) AS URL2 |
&& | FROM scarr WHERE currcode = '{ lv_currency }' and |
&& | CARRID <> 'CA' |.

TRY.
  " 调用SQL-Connection方法，连接到数据库
  lo_conn = cl_sql_connection=>get_connection( ).
  " 调用SQL-Statement方法，创建SQL对话
  lo_statement = lo_conn->create_statement( ).
  " 调用SQL-query 方法，执行SQL语句
  lo_result_set = lo_statement->execute_query( lv_statement ).
  " 调用SQL-set_param_table 方法，指定用那个内表来记录返回结果
  lo_result_set->set_param_table( lr_result ).
  " 读取数据集的下一组数据到内表
  lo_result_set->next_package( ).
  " 得到结果后，关闭数据集
  lo_result_set->close( ).

"打印内表
cl_demo_output=>display_data( lt_airline ).
"错误处理，如SQL有误，会在此处报出log，而不会short dump
  CATCH cx_sql_exception INTO lx_sql.
  WRITE: lx_sql->get_text( ).
ENDTRY.
