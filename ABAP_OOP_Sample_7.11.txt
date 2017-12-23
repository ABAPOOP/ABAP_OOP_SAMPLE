"示例程序7.11
  METHOD print_order_document.
    WRITE: / '执行PP02类型的生产订单打印'.
    IF ii_prod_order_output IS BOUND.
      ii_prod_order_output->print_output( ).
    ENDIF.
  ENDMETHOD. 
