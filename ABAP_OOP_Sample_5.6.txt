"示例程序5.6
  METHOD withdraw.
    IF mv_account_balance >= iv_withdraw_amount.
      mv_account_balance = mv_account_balance - iv_withdraw_amount.
      rv_account_balance = mv_account_balance.
    ENDIF.
  ENDMETHOD.
