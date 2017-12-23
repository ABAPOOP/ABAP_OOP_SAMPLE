"示例程序9.6
  METHOD zif_data_upload~upload_goods_receipt_number.
    DATA: ls_return TYPE bapiret2,
          le TYPE bapi_mtype.
    "调用SAP提供的Function Module，执行物料移动
    CALL FUNCTION 'BAPI_GOODSMVT_CREATE'
      EXPORTING
        goodsmvt_header = is_goodsmvt_header
        goodsmvt_code   = is_goodsmvt_code
      TABLES
        goodsmvt_item   = ct_goodsmvt_item
        return          = ct_return.
    "查询是否返回任何错误，‘E’开头
    LOOP AT ct_return INTO ls_return.
      IF ls_return-type EQ 'E'.
        le = 'E'.
      ENDIF.
    ENDLOOP.
    "没有错误返回，则执行提交，确认物料移动提交到系统
    IF le IS INITIAL.
      CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'
        EXPORTING
          wait = 'X'.
    ENDIF.
  ENDMETHOD.
