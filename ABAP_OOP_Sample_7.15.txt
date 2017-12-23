"示例程序7.15
  METHOD evaluation.
    CREATE OBJECT mo_price_evaluation.
    CREATE OBJECT mo_quality_evaluation.
    CREATE OBJECT mo_service_evaluation.
    CREATE OBJECT mo_delivery_evaluation.
    CREATE OBJECT mo_evaluation_calculation.
    mo_price_evaluation->evaluation( iv_vendor_id ).
    mo_quality_evaluation->evaluation( iv_vendor_id ).
    mo_service_evaluation->evaluation( iv_vendor_id ).
    mo_delivery_evaluation->evaluation( iv_vendor_id ).
    mo_evaluation_calculation->calculation( iv_vendor_id ).
  ENDMETHOD.
