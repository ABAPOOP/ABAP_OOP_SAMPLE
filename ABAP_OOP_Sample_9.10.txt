"示例程序9.10
  METHOD cal_work_hour. 
    DATA: lv_prod_type TYPE aufk-auart,
     iv_prod_id TYPE aufk-aufnr,
     lv_class TYPE char30.
"根据生产订单号码，获取生产订单类型
    SELECT SINGLE auart FROM aufk INTO lv_prod_type
      WHERE aufnr = IV_ORDER_NUMBER.
"根据生产订单类型，调用工厂类获取对应的生产订单处理类对象实例
    IF sy-subrc = 0.
      mo_prd_order = mo_factory_prd->get_order_instance( lv_prod_type ).
"调用返回的生产订单类实例，调用对应的工时计算方法
      mo_prd_order->cal_work_hour( ).
    ENDIF.
  ENDMETHOD.
