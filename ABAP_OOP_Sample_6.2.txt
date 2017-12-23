"示例程序6.2
  METHOD get_material_obj.

    DATA: lv_material_type TYPE mara-mtart,
     lv_material TYPE mara-matnr,
     lv_class TYPE char30.

    DATA:
       exc_ref TYPE REF TO cx_root,
       exc_text TYPE string,
       lv_type_name TYPE string,
       lv_method_name TYPE string.

    "先根据物料号码查询物料类型
    SELECT SINGLE mtart FROM mara INTO lv_material_type
      WHERE matnr = iv_material_id.
    IF sy-subrc = 0.
      "在根据物料类型查询注册表中对应的物料类的类型
      SELECT SINGLE class_type FROM zinsp_desc_t INTO lv_class
            WHERE material_type = lv_material_type.
    ENDIF.
    "如果有查询记录则开始动态创建类
    IF sy-subrc = 0.
      lv_type_name = lv_class. "类名必须大写
      TRANSLATE lv_type_name TO UPPER CASE.
      TRY.
          "动态创建类对象 ro_material，类型为注册表中的类的类型，
          CREATE OBJECT ro_material TYPE (lv_type_name).
        CATCH cx_sy_create_object_error INTO exc_ref.
          exc_text = exc_ref->get_text( ).
          MESSAGE exc_text TYPE 'I'.
      ENDTRY.
    ENDIF.

  ENDMETHOD.
