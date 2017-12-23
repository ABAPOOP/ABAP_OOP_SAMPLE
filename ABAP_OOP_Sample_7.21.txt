"示例程序7.21
  METHOD notify_reorder.
    write: / 'PL01 工厂库存管理通知: '.
    CALL METHOD super->notify_reorder
      EXPORTING
        iv_status = iv_status.
  ENDMETHOD.
