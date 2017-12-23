"示例程序7.27 
REPORT zrep_cls_033.
DATA:
  exc_ref TYPE REF TO cx_root,
  exc_text TYPE string.
DATA:
  go_mrp  TYPE REF TO zcl_mrp,
  go_strategy TYPE REF TO zcl_strategy,
  gv_strategy_id TYPE string.
TRY.
    "创建MRP类
    CREATE OBJECT go_mrp.
    "模拟从数据库表中读取策略信息，动态调用不同的策略算法
    gv_strategy_id = 'ZCL_STRATEGY_Z1'.
    CREATE OBJECT go_strategy TYPE (gv_strategy_id).
    go_mrp->set_strategy( go_strategy ).
    go_mrp->make_plan( ).

    "模拟从数据库表中读取策略信息，动态调用不同的策略算法
    gv_strategy_id = 'ZCL_STRATEGY_Z2'.
    CREATE OBJECT go_strategy TYPE (gv_strategy_id).
    go_mrp->set_strategy( go_strategy ).
    go_mrp->make_plan( ).

    "模拟从数据库表中读取策略信息，动态调用不同的策略算法
    gv_strategy_id = 'ZCL_STRATEGY_Z3'.
    CREATE OBJECT go_strategy TYPE (gv_strategy_id).
    go_mrp->set_strategy( go_strategy ).
    go_mrp->make_plan( ).

  CATCH cx_sy_create_object_error INTO exc_ref.
    exc_text = exc_ref->get_text( ).
    MESSAGE exc_text TYPE 'I'.
ENDTRY.
