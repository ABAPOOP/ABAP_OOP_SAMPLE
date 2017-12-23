"示例程序7.20  
  METHOD notify_reorder.
    DATA: ms_purchasing_group TYPE ty_purchasing_group.
    LOOP AT me->mt_purchasing_group INTO ms_purchasing_group.
      WRITE: / '采购组:', ms_purchasing_group-group->group_id.
      ms_purchasing_group-group->update_task_status( iv_status ).
    ENDLOOP.
  ENDMETHOD.
