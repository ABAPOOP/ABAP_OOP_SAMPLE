"示例程序8.19
REPORT zrep_cls_51.

PARAMETERS p_ctry TYPE land1.

DATA: gv_str TYPE string VALUE 'Input: '.
"声明变量，对应的是BADI的接口
DATA: go_ref TYPE REF TO zif_ex_badi_test_001.
"调用BADI的handler接口方法，获得对应的类实例
CALL METHOD cl_exithandler=>get_instance
  EXPORTING
    exit_name                     = 'ZBADI_TEST_001'
    null_instance_accepted        = 'X'
  CHANGING
    instance                      = go_ref
  EXCEPTIONS
    no_reference                  = 1
    no_interface_reference        = 2
    no_exit_interface             = 3
    class_not_implement_interface = 4
    single_exit_multiply_active   = 5
    cast_error                    = 6
    exit_not_existing             = 7
    data_incons_in_exit_managem   = 8
    OTHERS                        = 9.
"根据传入的国家代码参数，调用BADI的具体实现类的方法
IF sy-subrc IS INITIAL.
  CALL METHOD go_ref->get_value
    EXPORTING
      iv_input = gv_str
      flt_val  = p_ctry.
ENDIF.
