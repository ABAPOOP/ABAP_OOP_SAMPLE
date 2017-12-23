"示例程序9.11
REPORT zrep_cls_iot.

"设定对应的生产订单数据
SELECTION-SCREEN BEGIN OF BLOCK block_h1 WITH FRAME TITLE title_h1.
PARAMETERS p_prd TYPE aufnr.
PARAMETERS p_plant TYPE werks.
PARAMETERS p_pmat TYPE matnr.
PARAMETERS p_psloc TYPE lgort_d.
PARAMETERS p_batch TYPE charg_d.
SELECTION-SCREEN END OF BLOCK block_h1.

"设定计数传感器1的数据，模拟从流数据处理类获取的收货数据
SELECTION-SCREEN BEGIN OF BLOCK block_h2 WITH FRAME TITLE title_h2.
PARAMETERS p_p1sor TYPE char10.
PARAMETERS p_p1num TYPE erfmg.
PARAMETERS p_p1uit TYPE erfme.
SELECTION-SCREEN END OF BLOCK block_h2.
"设定计数传感器2的数据，模拟从流数据处理类获取的收货数据
SELECTION-SCREEN BEGIN OF BLOCK block_h3 WITH FRAME TITLE title_h3.
PARAMETERS p_p2sor TYPE char10.
PARAMETERS p_p2num TYPE erfmg.
PARAMETERS p_p2uit TYPE erfme.
SELECTION-SCREEN END OF BLOCK block_h3.
"设定计数传感器3的数据，模拟从流数据处理类获取的收货数据
SELECTION-SCREEN BEGIN OF BLOCK block_h4 WITH FRAME TITLE title_h4.
PARAMETERS p_p3sor TYPE char10.
PARAMETERS p_p3num TYPE erfmg.
PARAMETERS p_p3uit TYPE erfme.
SELECTION-SCREEN END OF BLOCK block_h4.

"设定计数传感器内表类型
TYPES:
BEGIN OF ty_sensor,
  sensor TYPE REF TO zcl_sensor ,
  gr_number TYPE erfmg,
  gr_unit TYPE erfme,
END OF ty_sensor.
TYPES:
  ty_tab_sensor TYPE STANDARD TABLE OF ty_sensor .

DATA:
  exc_ref TYPE REF TO cx_root,
  exc_text TYPE string.
"设定采样处理类和计数传感器类对象
DATA:
  go_sampler  TYPE REF TO zcl_sampler,
  go_num_sensor_01 TYPE REF TO zcl_num_sensor,
  go_num_sensor_02 TYPE REF TO zcl_num_sensor,
  go_num_sensor_03 TYPE REF TO zcl_num_sensor,
  go_prd_order    TYPE REF TO zcl_prd_order,
  gs_sensor TYPE ty_sensor.


TRY.
    "创建采样处理类对象
    CREATE OBJECT go_sampler.
    "采样处理类管理多个传感器类
    CREATE OBJECT go_num_sensor_01.
    go_num_sensor_01->mv_sensor_id = p_p1sor.
    gs_sensor-sensor = go_num_sensor_01.
    gs_sensor-gr_number = p_p1num.
    gs_sensor-gr_unit = p_p1uit.
    INSERT gs_sensor INTO TABLE go_sampler->mt_sensor.

    CREATE OBJECT go_num_sensor_02.
    go_num_sensor_02->mv_sensor_id = p_p2sor.
    gs_sensor-sensor = go_num_sensor_02.
    gs_sensor-gr_number = p_p2num.
    gs_sensor-gr_unit = p_p2uit.
    INSERT gs_sensor INTO TABLE go_sampler->mt_sensor.

    CREATE OBJECT go_num_sensor_03.
    go_num_sensor_03->mv_sensor_id = p_p3sor.
    gs_sensor-sensor = go_num_sensor_03.
    gs_sensor-gr_number = p_p3num.
    gs_sensor-gr_unit = p_p3uit.
    INSERT gs_sensor INTO TABLE go_sampler->mt_sensor.

    WRITE: / '生产订单:', p_prd.

    "采样处理上传收货数量，每个传感器各自上传读取的收货数量
    go_sampler->upload_goods_receipt_number(
                iv_gr_plant = p_plant
                iv_order_number = p_prd
                iv_gr_material = p_pmat
                iv_gr_sloc = p_psloc
                iv_gr_batch = p_batch ).
    SKIP 1.

    "采样处理计算工时
    go_sampler->cal_work_hour( p_prd ).

  CATCH cx_sy_create_object_error INTO exc_ref.
    exc_text = exc_ref->get_text( ).
    MESSAGE exc_text TYPE 'I'.
ENDTRY.
