"示例程序7.10
  METHOD print_order_document.
    WRITE: / 'PP01类型生产订单执行文档输出 '.
    IF ii_prod_order_output IS BOUND.
      ii_prod_order_output->print_output( ).
    ENDIF.
  ENDMETHOD.
