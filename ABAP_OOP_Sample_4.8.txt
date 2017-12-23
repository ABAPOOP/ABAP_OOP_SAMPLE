"示例程序4.8
REPORT zrep_cls_003.

DATA : gv_matnr TYPE mara-matnr.
DATA : gt_mara TYPE TABLE OF mara.
DATA : gs_mara TYPE mara.
DATA : go_inventory TYPE REF TO zcl_inventory.
CREATE OBJECT go_inventory.

PARAMETERS : p_mtart TYPE mara-matnr.

"Register event handler method
SET HANDLER go_inventory->handle_material_not_found FOR go_inventory.

START-OF-SELECTION.

  CALL METHOD go_inventory->check_material
    EXPORTING
      iv_material_id = p_mtart
    IMPORTING
      et_material    = gt_mara.

  LOOP AT gt_mara INTO gs_mara.
    WRITE :/ gs_mara-matnr, gs_mara-mtart, gs_mara-meins, gs_mara-ersda.
  ENDLOOP.
