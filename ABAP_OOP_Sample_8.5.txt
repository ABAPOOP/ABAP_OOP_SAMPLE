"示例程序8.5
REPORT zrep_cls_d05.
"定义子过程 CALL_REF
FORM call_ref.
  "定义字段符号，不指定任何类型，即支持任意类型
  FIELD-SYMBOLS: <ref>.
  DATA: lo_refobj TYPE REF TO object.
  DATA: lv_v1 TYPE i VALUE 2009.
  "将代码 ZREP_CLS_D04中的类对象 go_value的属性 m_value_1赋予字段符号
  ASSIGN ('(ZREP_CLS_D04)go_value->m_value_1') TO <ref>.
  WRITE : / <ref>.
  "再将代码 ZREP_CLS_D04中的类对象 go_value本身赋予字段符号
  ASSIGN ('(ZREP_CLS_D04)go_value') TO <ref>.
  IF sy-subrc = 0.
    "将字段符号指向的对象赋予本地对象实例
    lo_refobj = <ref>.
    "调用对象的方法 SET_VALUE，传入值
    CALL METHOD lo_refobj->('SET_VALUE')
      EXPORTING
        iv_v1 = lv_v1.
  ENDIF.
ENDFORM.
