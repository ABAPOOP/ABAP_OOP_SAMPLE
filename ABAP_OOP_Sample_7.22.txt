"示例程序7.22 
REPORT zrep_cls_029.

DATA:
  exc_ref TYPE REF TO cx_root,
  exc_text TYPE string.
DATA:
  go_inv_mgmt_pl01  TYPE REF TO zcl_inventory_mgmt_pl01,
  go_purch_group_pl01 TYPE REF TO zcl_purchasing_group_pl01,
  gv_group_id    TYPE string,
  gt_group_id TYPE STANDARD TABLE OF string.

TRY.
    "模拟从数据库表中读取采购组信息。
    gv_group_id = '1001'.
    INSERT gv_group_id INTO TABLE gt_group_id.
    gv_group_id = '3023'.
    INSERT gv_group_id INTO TABLE gt_group_id.
    gv_group_id = '4045'.
    INSERT gv_group_id INTO TABLE gt_group_id.
    CREATE OBJECT go_inv_mgmt_pl01.
    "循环创建采购组类对象，注册到库存管理被观察者对象中
    LOOP AT gt_group_id INTO gv_group_id.
      CREATE OBJECT go_purch_group_pl01.
      go_purch_group_pl01->zif_purchasing_group~group_id = gv_group_id.
      go_inv_mgmt_pl01->attach( go_purch_group_pl01 ).
      FREE go_purch_group_pl01.
    ENDLOOP.
    "库存发生变化，通知相关的观察者处理
    go_inv_mgmt_pl01->notify_reorder( 'Re-order 2017/09/30' ).
    SKIP 2. "打印2个空行
    "删除一个观察者
    go_inv_mgmt_pl01->detach( '3023' ).
    SKIP 2.
    "库存再次发生变化，通知相关的观察者处理
    go_inv_mgmt_pl01->notify_reorder( 'Re-order 2017/10/10' ).
  CATCH cx_sy_create_object_error INTO exc_ref.
    exc_text = exc_ref->get_text( ).
    MESSAGE exc_text TYPE 'I'.
ENDTRY.
