"示例程序7.6
METHOD zif_target~create_doc.
  CALL FUNCTION 'ZFM_CREATE_DOC_DB'
 EXPORTING
   output_device   = iv_output_device
 IMPORTING
   host_name = rv_host_name .
ENDMETHOD.
