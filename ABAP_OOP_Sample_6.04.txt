"示例程序6.4
"抽象的物料类，具体的物料类，物料对象生成工厂类的code based 代码 
 
CLASS zcl_material DEFINITION
  PUBLIC
  ABSTRACT
  CREATE PUBLIC .

  PUBLIC SECTION.

    METHODS print_insp_desc .
  PROTECTED SECTION.

    DATA mv_material TYPE mara-matnr .
  PRIVATE SECTION.
ENDCLASS.



CLASS zcl_material IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_MATERIAL->PRINT_INSP_DESC
* +-------------------------------------------------------------------------------------------------+
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD print_insp_desc.
  ENDMETHOD.
ENDCLASS.




CLASS zcl_material_fin DEFINITION
  PUBLIC
  INHERITING FROM zcl_material
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.

    METHODS show_material_fin .

    METHODS print_insp_desc
      REDEFINITION .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS zcl_material_fin IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_MATERIAL_FIN->PRINT_INSP_DESC
* +-------------------------------------------------------------------------------------------------+
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD print_insp_desc.
*CALL METHOD SUPER->PRINT_INSP_DESC
*    .

    WRITE: / '此物料为成品，需检查销售合同 '.

  ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_MATERIAL_FIN->SHOW_MATERIAL_FIN
* +-------------------------------------------------------------------------------------------------+
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD show_material_fin.

    WRITE: / '成品类新增的方法.'.
  ENDMETHOD.
ENDCLASS.



CLASS zcl_material_raw DEFINITION
  PUBLIC
  INHERITING FROM zcl_material
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.

    METHODS print_insp_desc
      REDEFINITION .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS zcl_material_raw IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_MATERIAL_RAW->PRINT_INSP_DESC
* +-------------------------------------------------------------------------------------------------+
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD print_insp_desc.
*CALL METHOD SUPER->PRINT_INSP_DESC
*    .
    WRITE: / '此物料为原料，需检查采购合同 '.
  ENDMETHOD.
ENDCLASS.



CLASS zcl_material_semi DEFINITION
  PUBLIC
  INHERITING FROM zcl_material
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.

    METHODS print_insp_desc
      REDEFINITION .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS zcl_material_semi IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_MATERIAL_SEMI->PRINT_INSP_DESC
* +-------------------------------------------------------------------------------------------------+
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD print_insp_desc.
*CALL METHOD SUPER->PRINT_INSP_DESC
*    .
    WRITE: / '此物料为半成品，需要打印生产过程及其库存信息。 '.
  ENDMETHOD.
ENDCLASS.



CLASS zcl_matl_spl_factory DEFINITION
  PUBLIC
  CREATE PUBLIC .

  PUBLIC SECTION.

    CLASS-METHODS get_material_obj
      IMPORTING
        !iv_material_id TYPE mara-matnr
      RETURNING
        VALUE(ro_material) TYPE REF TO zcl_material .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS zcl_matl_spl_factory IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_MATL_SPL_FACTORY=>GET_MATERIAL_OBJ
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_MATERIAL_ID                 TYPE        MARA-MATNR
* | [<-()] RO_MATERIAL                    TYPE REF TO ZCL_MATERIAL
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD get_material_obj.

    DATA: lv_material_type TYPE mara-mtart,
     lv_material TYPE mara-matnr,
     lv_class TYPE char30.

    DATA:
       exc_ref TYPE REF TO cx_root,
       exc_text TYPE string,
       lv_type_name TYPE string,
       lv_method_name TYPE string.

    "先根据物料号码查询物料类型
    SELECT SINGLE mtart FROM mara INTO lv_material_type
      WHERE matnr = iv_material_id.
    IF sy-subrc = 0.
      "在根据物料类型查询注册表中对应的物料类的类型
      SELECT SINGLE class_type FROM zinsp_desc_t INTO lv_class
            WHERE material_type = lv_material_type.
    ENDIF.
    "如果有查询记录则开始动态创建类
    IF sy-subrc = 0.
      lv_type_name = lv_class. "类名必须大写
      TRANSLATE lv_type_name TO UPPER CASE.
      TRY.
          "动态创建类 ro_material中，类型为注册表中的类类型，
          CREATE OBJECT ro_material TYPE (lv_type_name).
        CATCH cx_sy_create_object_error INTO exc_ref.
          exc_text = exc_ref->get_text( ).
          MESSAGE exc_text TYPE 'I'.
      ENDTRY.
    ENDIF.

  ENDMETHOD.
ENDCLASS.








