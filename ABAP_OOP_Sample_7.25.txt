"示例程序7.25
  METHOD set_strategy.
    IF io_strategy IS BOUND.
      me->mo_strategy = io_strategy.
    ENDIF.
  ENDMETHOD.  
 
