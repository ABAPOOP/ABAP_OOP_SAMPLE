"示例程序5.8
REPORT zrep_cls_27_1.

DATA:
      go_account_balance TYPE REF TO zcl_account_balance.

CREATE OBJECT: go_account_balance.

PARAMETERS:
p_numbr TYPE string10,
p_blnce TYPE dmbtr,
p_dpst TYPE dmbtr,
p_wdrw TYPE dmbtr.

DATA:
gv_balance TYPE dmbtr.

* Public attribute could be access directly.
*go_account_balance->mv_account_number = '12111111232'.

*OOP support this format for calling method   
CALL METHOD go_account_balance->set_account_number
  EXPORTING
    iv_account_number = p_numbr.

p_numbr = go_account_balance->get_account_number(  ).
WRITE:/ 'Account ', p_numbr ,' Account Balance: ', p_blnce.

CALL METHOD go_account_balance->set_account_balance( p_blnce ).
WRITE:/ 'Set Account balance to ', p_blnce.

*OOP support this format for calling method -
CALL METHOD go_account_balance->deposit
  EXPORTING
    iv_deposit_amount   = p_dpst
  RECEIVING
    rv_account_balance = gv_balance.

*New format of calling method.
gv_balance = go_account_balance->get_account_balance( ).
WRITE:/ 'Deposited ', p_dpst ,'RMB, now making balance to ',
gv_balance.

gv_balance = go_account_balance->withdraw( p_wdrw ).

gv_balance = go_account_balance->get_account_balance( ).
WRITE:/ 'Withdrew ', p_wdrw ,'RMB, now making balance to ',
gv_balance.
