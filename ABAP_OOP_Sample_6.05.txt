"示例程序6.5
CLASS zcl_rectangle DEFINITION
  PUBLIC
  CREATE PUBLIC .
  PUBLIC SECTION.
    DATA mv_width TYPE i .
    DATA mv_height TYPE i .
    METHODS set_width
      IMPORTING
        !iv_width TYPE i .
    METHODS set_height
      IMPORTING
        !iv_height TYPE i .
    METHODS get_area
      RETURNING
        VALUE(rv_area) TYPE i .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_rectangle IMPLEMENTATION.
  METHOD get_area.
    rv_area = mv_width * mv_height.
  ENDMETHOD.

  METHOD set_height.
    mv_height = iv_height.
  ENDMETHOD.

  METHOD set_width.
    mv_width = iv_width.
  ENDMETHOD.
ENDCLASS.
