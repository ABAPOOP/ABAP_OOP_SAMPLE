"示例程序9.9
  METHOD upload_goods_receipt_number.
    DATA: ls_sensor TYPE ty_sensor.
    TYPES:
      ty_tab_goodsmvt_item TYPE STANDARD TABLE OF bapi2017_gm_item_create .
    TYPES:
      ty_tab_return TYPE STANDARD TABLE OF  bapiret2 .

    DATA: ls_goodsmvt_header TYPE bapi2017_gm_head_01,
      ls_goodsmvt_code TYPE bapi2017_gm_code,
      ls_goodsmvt_item TYPE bapi2017_gm_item_create,
      lt_goodsmvt_item TYPE ty_tab_goodsmvt_item,
      lt_return TYPE ty_tab_return .

" 循环读取传感器内表
    LOOP AT me->mt_sensor INTO ls_sensor.
      TRY.
          REFRESH lt_goodsmvt_item.
" 模拟流数据读取输入，设定读取信息
          ls_goodsmvt_header-pstng_date = sy-datum.
          ls_goodsmvt_header-doc_date = sy-datum.
          ls_goodsmvt_code-gm_code = '02'.
          ls_goodsmvt_item-material = iv_gr_material.
          ls_goodsmvt_item-plant = iv_gr_plant.
          ls_goodsmvt_item-stge_loc = iv_gr_sloc.
          ls_goodsmvt_item-batch = iv_gr_batch.
          ls_goodsmvt_item-move_type = '101'. "101移动类型，用于生产订单收货
          ls_goodsmvt_item-entry_qnt = ls_sensor-gr_number.
          ls_goodsmvt_item-entry_uom = ls_sensor-gr_unit.
          ls_goodsmvt_item-orderid = iv_order_number.
          ls_goodsmvt_item-mvt_ind = 'F'. "库存移动标识为生产订单类型
          ls_goodsmvt_item-item_text = ls_sensor-sensor->mv_sensor_id.
"将读入信息存入SAP标准功能模块内表
          INSERT ls_goodsmvt_item INTO TABLE lt_goodsmvt_item.
"获取具体的传感器实例（此处为计数传感器）
          mo_num_sensor ?= ls_sensor-sensor.
"调用计数传感器的数据上传方法
          CALL METHOD mo_num_sensor->zif_data_upload~upload_goods_receipt_number
            EXPORTING
              is_goodsmvt_header = ls_goodsmvt_header
              is_goodsmvt_code   = ls_goodsmvt_code
            CHANGING
              ct_goodsmvt_item   = lt_goodsmvt_item
              ct_return          = lt_return.
          WRITE: / '传感器:', ls_sensor-sensor->mv_sensor_id, ',生产收货数据上传完成.'.
        CATCH cx_sy_move_cast_error.
      ENDTRY.
    ENDLOOP.
  ENDMETHOD.
