"示例程序5.34
  METHOD zif_get_flights~get_flight_details.
    "Get Flight Departure and Arrival city
    SELECT SINGLE carrid carrname connid
      countryfr cityfrom airpfrom
      countryto cityto airpto FROM sflights
      INTO corresponding fields of es_sflights
      WHERE connid = iv_flight_no.

  ENDMETHOD.
