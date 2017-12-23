"示例程序7.5
FUNCTION zfm_create_doc_db.
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     REFERENCE(OUTPUT_DEVICE) TYPE  TSP03D-NAME
*"  EXPORTING
*"     REFERENCE(HOST_NAME) TYPE  TSP03D-PAPROSNAME
*"----------------------------------------------------------------------

  IF output_device IS NOT INITIAL.
    SELECT paprosname FROM tsp03d
    INTO (host_name)
      WHERE name = output_device.
    ENDSELECT.
  ENDIF.

  IF sy-subrc = 0.
    WRITE: / '连接到文档数据库:', host_name..
    WRITE: / '创建文件到文档数据库:' , host_name.
    WRITE: / '断开连接:', host_name. 
  ENDIF.

ENDFUNCTION.
