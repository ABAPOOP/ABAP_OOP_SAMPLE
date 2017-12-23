"示例程序6.1
REPORT zrep_cls_021.

DATA gv_material_type TYPE mara-mtart.
DATA gv_material TYPE mara-matnr.

PARAMETERS: p_matnr LIKE mara-matnr.

START-OF-SELECTION.

  SELECT SINGLE mtart FROM mara INTO gv_material_type
    WHERE matnr = p_matnr.

  IF sy-subrc = 0.

    "类型判断
    IF ( gv_material_type EQ 'ROH') .
      WRITE: / '此物料为原料，需检查采购合同 '.
    ELSEIF ( gv_material_type EQ 'FERT') .
      WRITE: / '此物料为成品，需检查销售合同 '.
    ENDIF.
  ENDIF.

