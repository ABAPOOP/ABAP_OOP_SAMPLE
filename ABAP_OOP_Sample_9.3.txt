"示例程序9.3
REPORT zrp_oop_call_rep_vari.

DATA:
"定义报表参数类
  go_call_report_variants TYPE REF TO zcl_call_report_variants.
"定义输入参数
PARAMETERS: p_rep_id TYPE vari-report,
            p_wait   TYPE n LENGTH 3.
PARAMETERS: p_var_c TYPE char20.
START-OF-SELECTION.
"创建类对象
  CREATE OBJECT go_call_report_variants
  EXPORTING
      iv_rep_id = p_rep_id
      iv_wait = p_wait
      iv_variant_desc_c = p_var_c.
  IF go_call_report_variants IS BOUND.
"查找报表参数
    go_call_report_variants->get_report_variants( ).
"逐一按参数执行报表
    go_call_report_variants->call_report_by_variant( ).
  ENDIF.
