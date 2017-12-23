"示例程序5.11
    DATA: ls_mara TYPE mara.

    SELECT SINGLE mtart FROM mara
    INTO (ls_mara-mtart)
    WHERE matnr = iv_material_id.
    IF sy-subrc = 0.
      rv_material_type = ls_mara-mtart.
    ELSE.
      rv_material_type = ''.
    ENDIF.

    WRITE :/ 'MATERIAL TYPE:', rv_material_type .
