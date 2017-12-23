"示例程序5.20
  METHOD print_material_desc.
    CALL METHOD super->print_material_desc.
    WRITE: /'批次号码 ', 'MF01SEM537'.
    WRITE: /'重检日期 ', sy-datum.
  ENDMETHOD.
