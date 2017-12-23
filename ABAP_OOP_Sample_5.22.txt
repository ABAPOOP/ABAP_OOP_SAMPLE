"示例程序5.22
  METHOD print_material_desc.
    CALL METHOD super->print_material_desc.
    WRITE: /'批次号码 ' , 'MF01FIN001'.
    WRITE: /'生产日期 ', SY-DATUM.
  ENDMETHOD.
