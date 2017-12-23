"示例程序4.12
  METHOD RAISE_EXCEPTION.
    IF cv_value = 1.
      RAISE action_not_supported.
    ELSEIF cv_value = 2.
      RAISE cancelled.
    ELSEIF cv_value = 3.
      RAISE failed.
    ELSE.
      RAISE get_result_failed.
    ENDIF.
  ENDMETHOD.
