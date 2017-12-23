"示例程序5.36
REPORT zrep_cls_016.

DATA:
sflight TYPE sflight.
*object define
DATA:
gi_get_flights TYPE REF TO zif_get_flights,
go_flight_p TYPE REF TO zcl_get_flight_price,
go_flight_d TYPE REF TO zcl_get_flight_dest,
gs_flights TYPE sflight,
gs_sflights TYPE sflights.

PARAMETERS:
s_connid TYPE sflight-connid.

*create object
START-OF-SELECTION.

  CREATE OBJECT go_flight_p.

*call interface method.
  CALL METHOD go_flight_p->zif_get_flights~get_flight_details
    EXPORTING
      iv_flight_no = s_connid
    IMPORTING
      es_sflight   = gs_flights.

*display the price data
  IF sy-subrc = 0.
    cl_demo_output=>display_data( gs_flights ).
  ENDIF.

  "upcasting
  CREATE OBJECT gi_get_flights TYPE zcl_get_flight_dest.

*call interface method.
  CALL METHOD gi_get_flights->get_flight_details
    EXPORTING
      iv_flight_no = s_connid
    IMPORTING
      es_sflights  = gs_sflights.

*display the Departure and Arrival city data
  IF sy-subrc = 0.
    cl_demo_output=>display_data( gs_sflights ).
  ENDIF.

  "Method "PRINT_FLIGHT_DEST(" is unknown or PROTECTED or PRIVATE.
  "CALL METHOD gi_get_flights->print_flight_dest( ).

  "downcasting
  go_flight_d ?= gi_get_flights.

  CALL METHOD go_flight_d->print_flight_dest( ).

