"示例程序5.33
  METHOD zif_get_flights~get_flight_details.
*Get flight price from sflight table
    SELECT SINGLE carrid connid fldate
      price currency FROM sflight
      INTO CORRESPONDING FIELDS OF es_sflight
      WHERE connid = iv_flight_no.

  ENDMETHOD.
