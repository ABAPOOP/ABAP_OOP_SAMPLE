"示例程序4.14
REPORT zrep_cls_005.

PARAMETERS : p_ivalue TYPE i.
DATA: lo_cx_root TYPE REF TO cx_root.
DATA: go_exception TYPE REF TO zcl_exception.

CREATE OBJECT go_exception.

*--------------------------------------------------------------
* Try and Catch method Exception
*--------------------------------------------------------------
TRY.
    CALL METHOD go_exception->test_exception
      CHANGING
        cv_value = p_ivalue.
  CATCH cx_root INTO lo_cx_root.
    MESSAGE  lo_cx_root->get_text( ) TYPE 'I'.
ENDTRY.

*--------------------------------------------------------------
* Call method and return Exception
*--------------------------------------------------------------
CALL METHOD go_exception->raise_exception
  CHANGING
    cv_value             = p_ivalue
  EXCEPTIONS
    action_not_supported = 1
    cancelled            = 2
    failed               = 3
    OTHERS               = 4. "GET_RESULT_FAILED

IF sy-subrc EQ 1.
  MESSAGE 'action_not_supported' TYPE 'I'.
ELSEIF sy-subrc EQ 2.
  MESSAGE 'cancelled' TYPE 'I'.
ELSEIF sy-subrc EQ 3.
  MESSAGE 'failed' TYPE 'I'.
ELSEIF sy-subrc EQ 4.
  MESSAGE 'GET_RESULT_FAILED' TYPE 'I'.
ENDIF.
