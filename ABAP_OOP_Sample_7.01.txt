"示例程序7.1 
  METHOD GET_INSTANCE.
    IF mo_log IS NOT BOUND.
      CREATE OBJECT mo_log.
    ENDIF.
      ro_log = mo_log.
  ENDMETHOD.
