"示例程序7.16 
REPORT zrep_cls_028.

DATA:
  exc_ref TYPE REF TO cx_root,
  exc_text TYPE string.
DATA:
  go_facade_evaluation TYPE REF TO zcl_facade_evaluation.
TRY.
    "外部程序调用复杂的评估系统时，仅需要调用下面一个方法即可。
    CREATE OBJECT go_facade_evaluation .
    go_facade_evaluation->evaluation( '1297' ).
  CATCH cx_sy_create_object_error INTO exc_ref.
    exc_text = exc_ref->get_text( ).
    MESSAGE exc_text TYPE 'I'.
ENDTRY.
