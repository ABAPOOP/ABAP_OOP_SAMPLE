"示例程序6.6
CLASS zcl_square DEFINITION
  PUBLIC
  INHERITING FROM zcl_rectangle
  FINAL
  CREATE PUBLIC .
  PUBLIC SECTION.
    METHODS set_width
      REDEFINITION .
    METHODS set_height
      REDEFINITION .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_square IMPLEMENTATION.
  METHOD set_height.
    mv_height = iv_height.
    mv_width = iv_height.
  ENDMETHOD.

  METHOD set_width.
    mv_width = iv_width.
    mv_height = iv_width.
  ENDMETHOD.
ENDCLASS.
