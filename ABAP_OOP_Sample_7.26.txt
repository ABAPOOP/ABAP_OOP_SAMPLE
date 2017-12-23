"示例程序7.26
  METHOD make_plan.
    IF mo_strategy IS BOUND.
      mo_strategy->call_algorithm( ).
    ENDIF.
  ENDMETHOD.
