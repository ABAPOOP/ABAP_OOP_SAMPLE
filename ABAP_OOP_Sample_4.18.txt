"示例程序4.18
  METHOD DISPLAY.
    WRITE: / 'DISPLAY '.

    DATA lo_master TYPE REF TO zcl_master.
    CREATE OBJECT lo_master.

    lo_master->mv_public = 'caller set value : public'.
    lo_master->mv_private = 'caller set value : private'.
    lo_master->mv_protected = 'caller set value : protected'.

    CALL METHOD lo_master->display_public.
    CALL METHOD lo_master->display_private.
    CALL METHOD lo_master->display_protected.
  ENDMETHOD.
