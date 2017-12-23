"示例程序7.7
REPORT zrep_cls_026.

"基于接口声明类，而基于子类创建类实例
DATA: go_target TYPE REF TO zif_target.

"创Target接口对象，类型按子类Adapter类来创建
CREATE OBJECT go_target TYPE zcl_adapter.

IF go_target IS NOT INITIAL.
  "调用Adapter中的方法，传入文档数据库名称，连接到具体的Host Server
  go_target->create_doc( 'DOC_BASE_01' ).
ENDIF.
