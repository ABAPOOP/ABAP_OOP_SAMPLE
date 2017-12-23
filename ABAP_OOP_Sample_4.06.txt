"示例程序4.6
*OOP support this format for calling method -
  l_balance = lo_account_balance->deposit( 
  EXPORTING
    iv_name = l_param
  IMPORTING
    ev_name = l_param
  CHANGING
    cv_name = l_param
).
