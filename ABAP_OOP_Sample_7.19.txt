"示例程序7.19 
  METHOD detach. 
    IF iv_group_id IS NOT INITIAL.
      DELETE me->mt_purchasing_group WHERE group_id = iv_group_id.
    ENDIF.
  ENDMETHOD.
