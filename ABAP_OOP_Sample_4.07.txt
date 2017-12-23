"示例程序4.7
  METHOD check_material.
    "Select records from MARA table according to the material ID.
    SELECT mandt matnr ersda
           ernam laeda aenam mtart
    FROM mara
    INTO TABLE et_material
    WHERE matnr LIKE iv_material_id.

    "If no records are found, raise event, and print warning.
    IF sy-subrc NE 0.
      RAISE EVENT material_not_found.
    ENDIF.
  ENDMETHOD.
