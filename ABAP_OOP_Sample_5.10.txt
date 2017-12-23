"示例程序5.10
  METHOD set_material_id.
*CALL METHOD SUPER->SET_MATERIAL_ID
*  EXPORTING
*    IV_MATERIAL_ID =
*    .
    mv_material_id = iv_material_id.
    WRITE: / 'Raw Material: ',mv_material_id .

  ENDMETHOD.
