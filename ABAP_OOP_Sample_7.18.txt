"示例程序7.18  
  METHOD attach.
    DATA: ms_purchasing_group TYPE ty_purchasing_group.
    IF ii_purchasing_group IS BOUND.
      ms_purchasing_group-group_id = ii_purchasing_group->group_id.
      ms_purchasing_group-group = ii_purchasing_group.
      INSERT ms_purchasing_group INTO TABLE mt_purchasing_group.
    ENDIF.

  ENDMETHOD.
