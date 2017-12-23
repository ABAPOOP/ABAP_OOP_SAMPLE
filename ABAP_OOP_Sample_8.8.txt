"示例程序8.8
REPORT zrep_cls_c10.
"定义整数变量
DATA: gv_i_1 TYPE i VALUE 10,
      gv_i_2 TYPE i VALUE 20,
      gv_i_3 TYPE i.
"定义浮点数变量
DATA: gv_f_1 TYPE f VALUE '16.8',
      gv_f_2 TYPE f VALUE '22.7',
      gv_f_3 TYPE f .
"定义字符变量
DATA: gv_c_1 TYPE c VALUE 'A',
      gv_c_2 TYPE c VALUE 'S',
      gv_c_3 TYPE c .

"调用通用对比方法，比较整数变量
PERFORM max_g USING gv_i_1 gv_i_2 CHANGING gv_i_3.
WRITE: / gv_i_3.
"调用通用对比方法，比较浮点数变量
PERFORM max_g USING gv_f_1 gv_f_2 CHANGING gv_f_3.
WRITE: / gv_f_3.
"调用通用对比方法，比较字符变量
PERFORM max_g USING gv_c_1 gv_c_2 CHANGING gv_c_3.
WRITE: / gv_c_3.

"该方法只能支持整型数据对比
FORM max_i USING p_i1 TYPE i p_i2 TYPE i CHANGING p_i3 TYPE i.
  IF p_i1 > p_i2.
    p_i3 = p_i1.
  ELSE.
    p_i3 = p_i2.
  ENDIF.
ENDFORM.

"该方法只能支持浮点型数据对比
FORM max_f USING p_f1 TYPE f p_f2 TYPE f CHANGING p_f3 TYPE f.
  IF p_f1 > p_f2.
    p_f3 = p_f1.
  ELSE.
    p_f3 = p_f2.
  ENDIF.
ENDFORM.

"该方法能支持通用数据类型的大小对比
FORM max_g USING p_g1 TYPE any p_g2 TYPE any CHANGING p_g3 TYPE any.
  IF p_g1 > p_g2.
    p_g3 = p_g1.
  ELSE.
    p_g3 = p_g2.
  ENDIF.
ENDFORM.
