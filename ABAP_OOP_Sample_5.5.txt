"示例程序5.5
  METHOD deposit.
    IF mv_account_number NE '0000000000' AND iv_deposit_amount > 0.
      mv_account_balance = mv_account_balance + iv_deposit_amount.
      rv_account_balance = mv_account_balance.
    ENDIF.
  ENDMETHOD.
