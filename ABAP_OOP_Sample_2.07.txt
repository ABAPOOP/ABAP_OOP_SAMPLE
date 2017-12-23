"示例程序2.7
REPORT zrep_cls_27.

TYPES:
BEGIN OF ty_t001,
       client_id TYPE mandt,
       company_code TYPE bukrs,
       company_name TYPE butxt,
       city  TYPE ort01,
END OF ty_t001.

DATA gs_t001 TYPE ty_t001.
DATA gt_t001 TYPE STANDARD TABLE OF ty_t001.

PARAMETERS p_bukrs TYPE t001-bukrs.

"1. 用EXEC 执行Native SQL ，可以取得跨client的数据，
"语法必须符合具体的数据库的要求，如表名为 ECC.ecc.T001
EXEC
  SQL PERFORMING print_Record.
  SELECT MANDT, BUKRS, BUTXT, ORT01
  INTO   :gs_t001
  FROM   ECC.ecc.T001
  WHERE  BUKRS = :p_bukrs
ENDEXEC.

"2. 打印内表数据
FORM print_record.
  INSERT gs_t001 INTO TABLE gt_t001.
  WRITE: / gs_t001-client_id, gs_t001-company_code, gs_t001-company_name, gs_t001-city.
ENDFORM.
