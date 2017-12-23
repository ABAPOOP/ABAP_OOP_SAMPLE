"示例程序6.3
REPORT zrep_cls_022.

DATA:
   lv_exref  TYPE REF TO cx_root,
   lv_msgtxt TYPE string.
DATA go_material TYPE REF TO zcl_material.
"输入物料号码
PARAMETERS: p_matnr LIKE mara-matnr.
START-OF-SELECTION.
  TRY.
      "不用创建调用工厂类的实例,通过类名调用静态方法
      "GET_MATERIAL_OBJ，取得子类的对象实例
      go_material = zcl_matl_spl_factory=>get_material_obj( p_matnr ).
      IF go_material IS BOUND.
        "调用类的方法PRINT_INSP_DESC ，实现子类具体的业务处理
        go_material->print_insp_desc( ).
      ELSE.
        WRITE: / '对应物料类型未维护打印信息.'.
      ENDIF.
    CATCH cx_root INTO lv_exref.
      lv_msgtxt = lv_exref->get_text( ).
      WRITE: / lv_msgtxt.
    CLEANUP.
  ENDTRY.
