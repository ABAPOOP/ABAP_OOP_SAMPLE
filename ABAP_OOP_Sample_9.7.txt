"示例程序9.7
  METHOD get_order_instance.

    "定义类中的属性内表和结构
    DATA:
    gt_attr_desc TYPE abap_attrdescr_tab,
    gs_attr_desc TYPE abap_attrdescr,
    lv_type_name TYPE char30,
    exc_ref TYPE REF TO cx_root,
    exc_text TYPE string.

    CREATE OBJECT mo_prd_type.
    "调用SAP标准类CL_ABAP_CLASSDESCR方法获取类属性和方法
    mo_class_desc ?= cl_abap_classdescr=>describe_by_object_ref( mo_prd_type ).
    "定义字段符号，类型为任意类型，用于存储类的属性名称和属性值
    FIELD-SYMBOLS <fs_attr> TYPE any.
    "获取类的属性
    gt_attr_desc[] = mo_class_desc->attributes.
    "动态访问类中的属性
    LOOP AT gt_attr_desc INTO gs_attr_desc.
      "如果有查询记录则开始动态创建类
      IF gs_attr_desc-name EQ iv_order_type.
        "将类的属性值赋予字段符号
        ASSIGN  mo_prd_type->(gs_attr_desc-name) TO <fs_attr>.
        lv_type_name = <fs_attr>. "类名必须大写
        TRANSLATE lv_type_name TO UPPER CASE.
        TRY.
            "动态创建类对象 RO_PRD_ORDER ，类型为注册表中的类的类型，
            CREATE OBJECT RO_PRD_ORDER TYPE (lv_type_name).
            MO_PRD_ORDER = RO_PRD_ORDER.
          CATCH cx_sy_create_object_error INTO exc_ref.
            exc_text = exc_ref->get_text( ).
            MESSAGE exc_text TYPE 'I'.
        ENDTRY.
        EXIT.
      ENDIF.
    ENDLOOP.
  ENDMETHOD.

