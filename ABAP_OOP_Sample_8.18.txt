"示例程序8.18
REPORT zrpt_persist_mara_guid.

"指定读入，创建，删除操作
SELECTION-SCREEN BEGIN OF BLOCK block_h1 WITH FRAME TITLE title_h1.
PARAMETERS:
get RADIOBUTTON GROUP radp,
create  RADIOBUTTON GROUP radp,
delete RADIOBUTTON GROUP radp.
SELECTION-SCREEN END OF BLOCK block_h1.

"设定界面选择框
SELECTION-SCREEN BEGIN OF BLOCK block_h2 WITH FRAME TITLE title_h2.
PARAMETERS:
  lv_matr LIKE ztb_mara_key-matnr,
  lv_desc LIKE ztb_mara_key-maktx,
  lv_cdate  LIKE ztb_mara_key-ersda,
  lv_type LIKE ztb_mara_key-mtart,
  lv_gtype LIKE ztb_mara_key-matkl.
SELECTION-SCREEN END OF BLOCK block_h2.

"设定操作类和需要将属性进行持久化的类对象变量
DATA:
      go_material_agent TYPE REF TO zca_persist_mara_key,
      go_result_obj TYPE REF TO object,
      go_material_key TYPE REF TO zcl_persist_mara_key.

START-OF-SELECTION.

  "创建操作类对象
  go_material_agent = zca_persist_mara_key=>agent.

"如果是从数据库中读取记录初始化类
IF get EQ 'X'.
    TRY.
"调用操作类，读取持久化记录，初始化持久类并对属性赋值
        CALL METHOD go_material_agent->get_persistent
        EXPORTING
           i_matnr = lv_matr
        RECEIVING
           result  = go_result_obj.
        go_material_key ?= go_result_obj.
        lv_desc = go_material_key->get_maktx( ).
        lv_cdate = go_material_key->get_ersda( ).
        lv_type = go_material_key->get_mtart( ).
        WRITE:/ lv_matr, lv_desc, lv_cdate, lv_type.
      CATCH cx_os_object_not_found .
        MESSAGE 'Object does not exist' TYPE 'I' DISPLAY LIKE 'I'.
    ENDTRY.
"如果是将初始化类存入数据库，创建持久数据
  ELSEIF create EQ 'X'.
    TRY.
"调用操作类，创建持久化类的属性和主键记录到数据库保存
        CALL METHOD go_material_agent->create_persistent
        EXPORTING
            i_matnr = lv_matr
            i_maktx = lv_desc
            i_ersda = lv_cdate
            i_mtart = lv_type
            i_matkl = lv_gtype
        RECEIVING
            result  = go_material_key.
        COMMIT WORK.
        WRITE 'Object created'.
      CATCH cx_os_object_existing .
        MESSAGE 'Object already exists' TYPE'I' DISPLAY LIKE 'I'.
    ENDTRY.
"如果是将初始化类从数据库中删除
  ELSEIF delete EQ 'X'.
    TRY.
"调用操作类，首先查询持久化记录是否在数据库中存在
        CALL METHOD go_material_agent->get_persistent
        EXPORTING
           i_matnr = lv_matr
        RECEIVING
           result  = go_result_obj.
      CATCH cx_os_object_not_found .
        MESSAGE 'Object does not exist' TYPE 'I' DISPLAY LIKE 'I'.
    ENDTRY.
    go_material_key ?= go_result_obj.
    TRY.
"调用操作类，按主键删除持久化记录
        CALL METHOD go_material_agent->delete_persistent
        EXPORTING
          i_matnr = lv_matr .
        COMMIT WORK.
        WRITE 'Object Deleted'.
      CATCH cx_os_object_not_existing .
        MESSAGE 'Object does not exist' TYPE 'I' DISPLAY LIKE 'I'.
    ENDTRY.
  ENDIF.
