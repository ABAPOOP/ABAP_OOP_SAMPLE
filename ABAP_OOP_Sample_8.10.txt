"示例程序8.10
REPORT zrep_cls_c07.
"定义局部类，包含两个方法和两个属性
CLASS zlcl_cl01 DEFINITION.
  PUBLIC SECTION.
    DATA: mv_string TYPE string VALUE 'String of Class Attribute'.
    DATA: mv_date TYPE string VALUE '2017.10.21'.
    METHODS: print_string.
    METHODS: print_date.
ENDCLASS.
"定义局部类的方法
CLASS zlcl_cl01 IMPLEMENTATION.
  METHOD:print_string.
    WRITE:/ 'print_string:', mv_string.
  ENDMETHOD.
  METHOD:print_date.
    WRITE:/ 'print_date:', mv_date.
  ENDMETHOD.
ENDCLASS.

START-OF-SELECTION.
  "定义 go_oref_l 为本地类对象
  DATA: go_oref_l TYPE REF TO zlcl_cl01.
  "定义 go_oref go_obj 为ABAP根类对象，创建对象时必须指定类名
  DATA: go_oref TYPE REF TO object.
  DATA: go_obj TYPE REF TO object.
  "定义SAP标准类CL_ABAP_CLASSDESCR对象
  DATA: go_class_desc TYPE REF TO cl_abap_classdescr.

  "1. 以下为类的字段符号的应用
  "定义任意类型的字段符号，后面不指定类型，相当于 TYPE any
  FIELD-SYMBOLS <obj>.

  "根据类名创建实例对象
  CREATE OBJECT go_oref_l TYPE zlcl_cl01.
  "设定go_oref_l的属性值
  go_oref_l->mv_string = 'String of Object go_oref_l '.
  "将对象go_oref_l赋予字段类型
  ASSIGN go_oref_l TO <obj>.
  "将字段类型再赋予临时对象 go_obj
  go_obj = <obj>.
  "对象 go_obj是由字段类型在程序运行时指定的类型，
  "所以编译器在编译阶段是无法知道其具体类型的，
  "以下注释掉的属性赋值语句是不可编译的
  "go_obj->mv_string = 'String of Object go_oref '.

  "而对象 go_obj动态访问方法是可行的，
  CALL METHOD go_obj->('PRINT_STRING').

  "根据参数类名动态创建实例对象
  CREATE OBJECT go_oref TYPE ('ZLCL_CL01').
  "将对象go_oref 赋予字段类型
  ASSIGN go_oref TO <obj>.
  "将字段类型再赋予临时对象 go_obj
  go_obj = <obj>.
  "对象 go_obj动态访问方法是可行的，
  CALL METHOD go_obj->('PRINT_DATE').

  SKIP 2.
  "2. 以下为类反射的实现
  "调用SAP标准类CL_ABAP_CLASSDESCR方法获取类属性和方法。
  go_class_desc ?=
  cl_abap_classdescr=>describe_by_object_ref( go_oref ).

  DATA:
  "定义类中的属性内表和结构
  gt_attr_desc TYPE abap_attrdescr_tab,
  gs_attr_desc TYPE abap_attrdescr,

  "定义类中的方法内表和结构
  gt_meth_desc TYPE abap_methdescr_tab,
  gs_meth_desc TYPE abap_methdescr.

  "定义字段符号，类型为任意类型，用于存储类的方法名称
  FIELD-SYMBOLS <fs_meth> TYPE any.
  "获取类的方法列表到内表中
  gt_meth_desc[] = go_class_desc->methods.

  "动态打印类中的方法名称
  LOOP AT gt_meth_desc INTO gs_meth_desc.
    "将类的方法名称赋予字段符号
    ASSIGN  gs_meth_desc-name TO <fs_meth>.
    WRITE: / <fs_meth>.
  ENDLOOP.

  "定义字段符号，类型为任意类型，用于存储类的属性名称和属性值
  FIELD-SYMBOLS <fs_attr> TYPE any.
  "获取类的属性名称内表中
  gt_attr_desc[] = go_class_desc->attributes.

  "动态访问类中的属性
  LOOP AT gt_attr_desc INTO gs_attr_desc.
    "将类的属性名称赋予字段符号
    ASSIGN  gs_attr_desc-name TO <fs_attr>.
    WRITE: / <fs_attr>.
    "将类的属性值赋予字段符号
    ASSIGN  go_oref->(gs_attr_desc-name) TO <fs_attr>.
    WRITE: <fs_attr>.
  ENDLOOP.

  SKIP 2.
  "动态访问类中的方法
  LOOP AT gt_meth_desc INTO gs_meth_desc.
    "动态调用类中的方法
    CALL METHOD go_oref->(gs_meth_desc-name).
  ENDLOOP.
