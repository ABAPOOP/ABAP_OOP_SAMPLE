"示例程序8.6
"定义整数比较方法
FORM max_i USING p_i1 TYPE i p_i2 TYPE i CHANGING p_i3 TYPE i.
  IF p_i1 > p_i2.
    p_i3 = p_i1.
  ELSE.
    p_i3 = p_i2.
  ENDIF.
ENDFORM.
