"示例程序5.2
  METHOD set_account_number.
    DATA: lv_str TYPE string VALUE '0000000000'.
    lv_str = iv_account_number.
    SHIFT lv_str LEFT DELETING LEADING '0'.
    IF lv_str CP 'CA277*'.
      mv_account_number = iv_account_number.
    ELSE.
      mv_account_number = '0000000000'.
    ENDIF.
  ENDMETHOD.
