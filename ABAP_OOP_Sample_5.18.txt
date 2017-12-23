"示例程序5.18
  METHOD print_material_desc.
    CALL METHOD super->print_material_desc.
    WRITE: /'供应商号码 ', 'VEN0127'.
    WRITE: /'批次号码 ', 'MF01RAW209'.
    WRITE: /'采购日期 ', sy-datum.
  ENDMETHOD.
