"示例程序9.5
  METHOD test_get_value.
    DATA: lv_result TYPE i.
    CREATE OBJECT mo_calculate.

    IF mo_calculate IS BOUND.
      lv_result = mo_calculate->get_value( 6 ).
      cl_aunit_assert=>assert_equals(
          exp                  = 600
          act                  = lv_result
          msg                  = 'result is wrong'
          ).
    ENDIF.
  ENDMETHOD. 
