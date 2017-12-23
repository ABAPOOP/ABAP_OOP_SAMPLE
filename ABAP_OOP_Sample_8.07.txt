"示例程序8.7
"定义浮点数比较方法
FORM max_f USING p_f1 TYPE f p_f2 TYPE f CHANGING p_f3 TYPE f.
  IF p_f1 > p_f2.
    p_f3 = p_f1.
  ELSE.
    p_f3 = p_f2.
  ENDIF.
ENDFORM. 
