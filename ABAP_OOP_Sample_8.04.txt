"示例程序8.4

REPORT zrep_cls_d04.

DATA: go_value TYPE REF TO zcl_values.
"创建类对象
CREATE OBJECT go_value.
"设定类对象属性值
go_value->m_value_1 = 1356. 
"调用程序 ZREP_CLS_D05中的方法 CALL_REF
PERFORM call_ref IN PROGRAM zrep_cls_d05.

