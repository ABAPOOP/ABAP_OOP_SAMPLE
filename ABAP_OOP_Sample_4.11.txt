"示例程序4.11
REPORT zrep_cls_004.

START-OF-SELECTION.

  DATA go_constructor TYPE REF TO zcl_constructor.
  CREATE OBJECT go_constructor
    EXPORTING
      iv_value = 'CONSTRUCTOR'.

  WRITE:/ 'CONSTRUCTOR TEST. '.
