"示例程序8.16 
REPORT zrep_ooalv_stock.

*********************************************************************
*使用方法：
*输入股票号码，获得新浪财经API实时沪深两市股票数据，双击可显示个股K线
*代码移植：
*在SAP SE38中创建报表ZREP_OOALV_STOCK，粘贴代码
*需要双击屏幕号码 Screen 100并激活
*双击状态'OOALV1'，设置'BACK'按钮，激活
*声明：
*程序仅供参考,不能作为投资依据. Aaron Hao - 2017/11/11
*********************************************************************


"定义股票信息结构体
TYPES: BEGIN OF ty_stock_live,
         sid    TYPE char10,  sname  TYPE char10,
         topen  TYPE char10,  yclose TYPE char10,
         cprice TYPE char10,  thigh  TYPE char10,
         tlow   TYPE char10,  buy    TYPE char10,
         sale   TYPE char10,  number TYPE char20,
         amount TYPE char20,  buy11  TYPE char10,
         buy12  TYPE char10,  buy21  TYPE char10,
         buy22  TYPE char10,  buy31  TYPE char10,
         buy32  TYPE char10,  buy41  TYPE char10,
         buy42  TYPE char10,  buy51  TYPE char10,
         buy52  TYPE char10,  sale11 TYPE char10,
         sale12 TYPE char10,  sale21 TYPE char10,
         sale22 TYPE char10,  sale31 TYPE char10,
         sale32 TYPE char10,  sale41 TYPE char10,
         sale42 TYPE char10,  sale51 TYPE char10,
         sale52 TYPE char10,  date   TYPE char15,
         time   TYPE char15,  color  TYPE lvc_t_scol, "Color.
       END OF ty_stock_live.

TYPES: BEGIN OF ty_stock_id,
         stock_id TYPE char6,
       END OF ty_stock_id.

TYPES ty_tab_stock_id TYPE STANDARD TABLE OF ty_stock_id.

TYPES ty_tab_stock_live TYPE STANDARD TABLE OF ty_stock_live.

DATA: gv_stock_id(6) TYPE c.
DATA: tmp_num(6) TYPE n,
      idx        TYPE i,
      idx_tmp    TYPE i.

"定义内表，定义SALV 的类CL_GUI_SALV_TABLE的对象变量
DATA gt_stock_id TYPE ty_tab_stock_id.
DATA gs_stock_id TYPE ty_stock_id.

"定义股票信息内表
DATA gt_stock TYPE ty_tab_stock_live .
DATA gs_stock TYPE  ty_stock_live.
DATA: gt_stock_pub_list TYPE TABLE OF string.

"定义股票信息OOALV相关类
DATA go_table TYPE REF TO cl_salv_table.
DATA go_functions TYPE REF TO cl_salv_functions_list.
DATA: go_columns TYPE REF TO cl_salv_columns_table.
DATA: go_column TYPE REF TO cl_salv_column_table.
DATA: go_column_list TYPE REF TO cl_salv_column_list.
DATA:go_container TYPE REF TO cl_gui_custom_container.
DATA: go_dock TYPE REF TO cl_gui_docking_container,
      go_picture   TYPE REF TO cl_gui_picture.
DATA: gv_url        TYPE char256,
      gt_data           TYPE STANDARD TABLE OF x255.

"本地类用于获得股票信息
CLASS lcl_stock DEFINITION DEFERRED.
"本地类用于处理双击事件
CLASS lcl_event_handler DEFINITION DEFERRED.

DATA go_stock TYPE REF TO lcl_stock.
DATA: go_timer TYPE REF TO cl_gui_timer,
      go_evt_hndl TYPE REF TO lcl_event_handler.

"定义本地类
CLASS lcl_stock DEFINITION.
  PUBLIC SECTION.
    TYPES:
      BEGIN OF ty_stock_live,
             sid    TYPE char10, "股票ID
             sname  TYPE char10, "股票名字
             topen  TYPE char10, "今日开盘价
             yclose TYPE char10, "昨日收盘价；
             cprice TYPE char10, "当前价格;
             thigh  TYPE char10, "当日最高价
             tlow   TYPE char10, "今日最低价
             buy    TYPE char10, "竞买价，即“买一”报价；
             sale   TYPE char10, "竞卖价，即“卖一”报价；
             number TYPE char20, "成交的股票数
             amount TYPE char20, "成交金额
             buy11  TYPE char10, "买一,数量
             buy12  TYPE char10, "买一,报价
             buy21  TYPE char10, "买二,数量
             buy22  TYPE char10, "买二,报价
             buy31  TYPE char10, "买三,数量
             buy32  TYPE char10, "买三,报价
             buy41  TYPE char10, "买四,数量
             buy42  TYPE char10, "买四,报价
             buy51  TYPE char10, "买五,数量
             buy52  TYPE char10, "买五,报价
             sale11 TYPE char10, "卖一,数量
             sale12 TYPE char10, "卖一,报价
             sale21 TYPE char10, "卖二,数量
             sale22 TYPE char10, "卖二,报价
             sale31 TYPE char10, "卖三,数量
             sale32 TYPE char10, "卖三,报价
             sale41 TYPE char10, "卖四,数量
             sale42 TYPE char10, "卖四,报价
             sale51 TYPE char10, "卖五,数量
             sale52 TYPE char10, "卖五,报价
             date   TYPE char15, "日期
             time   TYPE char15, "时间
             color  TYPE lvc_t_scol, "Color.
           END OF ty_stock_live .
    TYPES:
      ty_tab_stock_live TYPE STANDARD TABLE OF ty_stock_live .
    TYPES:
      BEGIN OF ty_stock_id,
             stock_id TYPE char6,
           END OF ty_stock_id .
    TYPES:
      ty_tab_stock_id TYPE STANDARD TABLE OF ty_stock_id .

    DATA gv_stock_id TYPE char6 .
    DATA mt_stock TYPE ty_tab_stock_live .
    DATA ms_stock TYPE ty_stock_live .
    DATA mt_stock_id TYPE ty_tab_stock_id .
    DATA ms_stock_id TYPE ty_stock_id .
    DATA:
      mt_stock_pub_list TYPE TABLE OF string .

    METHODS get_live_stock_info
      IMPORTING
        !it_stock_id TYPE ty_tab_stock_id .
    METHODS fill_alv
      CHANGING
        VALUE(ct_stock) TYPE ty_tab_stock_live .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS lcl_stock IMPLEMENTATION.
  "定义本地类方法用于获得新浪财经实时股票信息
  METHOD get_live_stock_info.
    DATA: lo_abap_conv     TYPE REF TO cl_abap_conv_in_ce,
          li_http_client   TYPE REF TO if_http_client,
         lv_stock_pub_url TYPE string VALUE 'http://hq.sinajs.cn/list=',
          abap_encoding    TYPE abap_encoding,
          l_str            TYPE string,
          l_xstr           TYPE xstring,
          l_tmp            TYPE string,
          l_url            TYPE string.
    DATA: BEGIN OF wa_num,
            num(6) TYPE c,
          END OF wa_num.
    DATA: BEGIN OF wa_lin,
            str TYPE string,
          END OF wa_lin.
    DATA: it_num    LIKE TABLE OF wa_num,
          it_restab LIKE TABLE OF wa_num,
          it_res    LIKE TABLE OF wa_lin.
    IF it_stock_id[] IS INITIAL.
      RETURN.
    ENDIF.
    CLEAR:it_res.
    REFRESH:it_res.
    LOOP AT it_stock_id INTO ms_stock_id.
      CLEAR l_tmp.
      IF ms_stock_id-stock_id(1) < 6.
        CONCATENATE 'sz' ms_stock_id-stock_id INTO l_tmp.
      ENDIF.
      IF ms_stock_id-stock_id(1) = '6'.
        CONCATENATE 'sh' ms_stock_id-stock_id INTO l_tmp.
      ENDIF.
      CONCATENATE l_str l_tmp INTO l_str SEPARATED BY ','.
    ENDLOOP.
    IF l_str(1) = ','.
      SHIFT l_str LEFT DELETING LEADING ','.
    ENDIF.
    CONCATENATE lv_stock_pub_url l_str INTO l_url.
    CALL METHOD cl_http_client=>create_by_url
      EXPORTING
        url                = l_url
      IMPORTING
        client             = li_http_client
      EXCEPTIONS
        argument_not_found = 1
        plugin_not_active  = 2
        internal_error     = 3
        OTHERS             = 4.
    CALL METHOD li_http_client->request->set_header_field
      EXPORTING
        name  = 'Content-Type'
        value = 'text/html;charset=utf-8'. " utf-8
    CALL METHOD li_http_client->send
      EXCEPTIONS
        http_communication_failure = 1
        http_invalid_state         = 2.
    CALL METHOD li_http_client->receive
      EXCEPTIONS
        http_communication_failure = 1
        http_invalid_state         = 2
        http_processing_failed     = 3.
    l_xstr = li_http_client->response->get_data( ).
    IF sy-subrc = 0 .
      li_http_client->close( ).
    ENDIF.
    CALL METHOD cl_abap_conv_in_ce=>create
      EXPORTING
        input       = l_xstr
        encoding    = '8400'
        replacement = '?'
        ignore_cerr = abap_true
      RECEIVING
        conv        = lo_abap_conv.
    TRY.
        CALL METHOD lo_abap_conv->read
          IMPORTING
            data = l_str.
      CATCH cx_sy_conversion_codepage.
      CATCH cx_sy_codepage_converter_init.
      CATCH cx_parameter_invalid_type.
      CATCH cx_parameter_invalid_range.
    ENDTRY.
    REFRESH mt_stock_pub_list.
    SPLIT l_str AT cl_abap_char_utilities=>newline
    INTO TABLE mt_stock_pub_list.

  ENDMETHOD.
  "定义本地类方法用于填充股票信息内表
  METHOD fill_alv.
    DATA lt_res TYPE TABLE OF string.
    DATA l_str TYPE string.
    DATA l_str_field TYPE string.
    FIELD-SYMBOLS: <fs_value> TYPE any.

    FIELD-SYMBOLS: <fs_stock> TYPE ty_stock_live.
    DATA: ls_color               TYPE lvc_s_scol.
    DATA: tmp_num(6) TYPE n,
          idx        TYPE i,
          idx_tmp    TYPE i.

    REFRESH lt_res.
    REFRESH mt_stock.
    LOOP AT mt_stock_pub_list INTO l_str.
      IF strlen( l_str ) < 30.
        CONTINUE.
      ENDIF.
      SPLIT l_str AT ',' INTO TABLE lt_res.
      READ TABLE lt_res INDEX 1 INTO l_str_field.
      ms_stock-sid = l_str_field+13(6).
      ms_stock-sname = l_str_field+21.

      idx = 2.
      WHILE idx < 33.
        READ TABLE lt_res INDEX idx INTO l_str_field.
        idx = idx + 1.
        ASSIGN COMPONENT idx OF STRUCTURE ms_stock TO <fs_value>.
        <fs_value> = l_str_field.
      ENDWHILE.

      INSERT ms_stock INTO TABLE mt_stock.
    ENDLOOP.

    "填充股票信息内表的颜色列
    LOOP AT mt_stock ASSIGNING <fs_stock>.
      REFRESH <fs_stock>-color.
      IF <fs_stock>-cprice > <fs_stock>-topen.
        ls_color-color-col = 6. "上涨，红色
        ls_color-color-int = 1.
        ls_color-color-inv = 0.
        APPEND ls_color TO <fs_stock>-color.
      ELSE.
        IF <fs_stock>-cprice < <fs_stock>-topen.
          ls_color-color-col = 5." 下跌，绿色
          ls_color-color-int = 0.
          ls_color-color-inv = 0.
          APPEND ls_color TO <fs_stock>-color.
        ENDIF.
      ENDIF.
    ENDLOOP.

    ct_stock = mt_stock.

  ENDMETHOD.


ENDCLASS.



"定义ALV双击事件类
CLASS lcl_handle_events DEFINITION.
  PUBLIC SECTION.
    METHODS: on_double_click FOR EVENT double_click OF
cl_salv_events_table
      IMPORTING
          row "事件触发所在的行号
          column."事件触发所在的列名
    METHODS: on_user_command FOR EVENT added_function OF
cl_salv_events_table
      IMPORTING e_salv_function.
ENDCLASS.

CLASS lcl_handle_events IMPLEMENTATION.
  METHOD on_double_click.
    PERFORM show_stock_details
    USING row column.
  ENDMETHOD.
  METHOD on_user_command.
    PERFORM handle_user_command USING e_salv_function.
  ENDMETHOD.
ENDCLASS.

"程序开始

START-OF-SELECTION.
  SELECT-OPTIONS:
  s_sid FOR gv_stock_id.
  PERFORM init_timer.
  PERFORM show_grid.

  "获得输入的股票号码
FORM get_input_stock_id.
  LOOP AT s_sid.
    CLEAR gs_stock_id.
    IF s_sid-option = 'EQ' .
      IF s_sid-low <> '' .
        gs_stock_id-stock_id = s_sid-low.
        INSERT gs_stock_id INTO TABLE gt_stock_id.
      ENDIF.
      IF s_sid-high <> '' .
        gs_stock_id-stock_id = s_sid-high.
        INSERT gs_stock_id INTO TABLE gt_stock_id.
      ENDIF.
    ENDIF.
    IF s_sid-option = 'BT'.
      tmp_num = s_sid-low.
      WHILE tmp_num <= s_sid-high.
        gs_stock_id-stock_id = tmp_num.
        INSERT gs_stock_id INTO TABLE gt_stock_id.
        tmp_num = tmp_num + 1.
      ENDWHILE.
    ENDIF.
  ENDLOOP.
ENDFORM.


"创建OOALV表格表头等信息
FORM build_alv.
  DEFINE init_alv.
    go_column->set_long_text( &1 ).
    go_column->set_medium_text( &1 ).
    go_column->set_short_text( &1 ).
  END-OF-DEFINITION.


  TRY.
      "调用类方法创建类对象
      CALL METHOD cl_salv_table=>factory
        IMPORTING
          r_salv_table = go_table
        CHANGING
          t_table      = gt_stock.
    CATCH cx_salv_msg.
  ENDTRY.


  TRY.
      go_columns = go_table->get_columns( ).
      go_column ?= go_columns->get_column('SID').
      init_alv '股票号码'.
      go_column ?= go_columns->get_column('SNAME').
      init_alv '股票名称'.
      go_column ?= go_columns->get_column('TOPEN').
      init_alv '开盘价'.
      go_column ?= go_columns->get_column('YCLOSE').
      init_alv '昨收价'.
      go_column ?= go_columns->get_column('CPRICE').
      init_alv '现价'.
      go_column ?= go_columns->get_column('THIGH').
      init_alv '最高价'  .
      go_column ?= go_columns->get_column('TLOW').
      init_alv '最低价'   .
      go_column ?= go_columns->get_column('BUY').
      init_alv '买一价'.
      go_column ?= go_columns->get_column('SALE').
      init_alv '卖一价'.
      go_column ?= go_columns->get_column('NUMBER').
      init_alv '成交数量'.
      go_column ?= go_columns->get_column('AMOUNT').
      init_alv '成交金额'.
      go_column ?= go_columns->get_column('BUY11').
      init_alv '买一数量'.
      go_column ?= go_columns->get_column('BUY12').
      init_alv '买一金额'.
      go_column ?= go_columns->get_column('BUY21').
      init_alv '买二数量'.
      go_column ?= go_columns->get_column('BUY22').
      init_alv '买二金额'.
      go_column ?= go_columns->get_column('BUY31').
      init_alv '买三数量'.
      go_column ?= go_columns->get_column('BUY32').
      init_alv '买三金额'.
      go_column ?= go_columns->get_column('BUY41').
      init_alv '买四数量'.
      go_column ?= go_columns->get_column('BUY42').
      init_alv '买四金额'.
      go_column ?= go_columns->get_column('BUY51').
      init_alv '买五数量'.
      go_column ?= go_columns->get_column('BUY52').
      init_alv '买五金额'.
      go_column ?= go_columns->get_column('SALE11').
      init_alv '卖一数量'.
      go_column ?= go_columns->get_column('SALE12').
      init_alv '卖一金额'.
      go_column ?= go_columns->get_column('SALE21').
      init_alv '卖二数量'.
      go_column ?= go_columns->get_column('SALE22').
      init_alv '卖二金额'.
      go_column ?= go_columns->get_column('SALE31').
      init_alv '卖三数量'.
      go_column ?= go_columns->get_column('SALE32').
      init_alv '卖三金额'.
      go_column ?= go_columns->get_column('SALE41').
      init_alv '卖四数量'.
      go_column ?= go_columns->get_column('SALE42').
      init_alv '卖四金额'.
      go_column ?= go_columns->get_column('SALE51').
      init_alv '卖五数量'.
      go_column ?= go_columns->get_column('SALE52').
      init_alv '卖五金额'.
      go_column ?= go_columns->get_column('DATE').
      init_alv   '日期'  .
      go_column ?= go_columns->get_column('TIME').
      init_alv   '时间'.

    CATCH cx_salv_not_found.                            "#EC NO_HANDLER
  ENDTRY.


  "为内表设置一个特殊的列， COLOR，然后为该列赋值即可控制每一行的颜色
  TRY.
      CALL METHOD go_columns->set_color_column
        EXPORTING
          value = 'COLOR'.
    CATCH cx_salv_data_error.
  ENDTRY.

  "调用类方法显示SALV
  go_functions = go_table->get_functions( ).
  go_functions->set_all( abap_true ).

  TRY.
      "设置热点列
      "go_columns = go_table->get_columns( ).
      "go_column ?= go_columns->get_column( 'SID').
      "go_column->set_cell_type( if_salv_c_cell_type=>hotspot ).

      "=====获取事件对象
      DATA: lr_event TYPE REF TO cl_salv_events_table.
      lr_event = go_table->get_event( ).

      "=====事件注册
      DATA: mo_handle_event TYPE REF TO lcl_handle_events.
      CREATE OBJECT mo_handle_event.
      SET HANDLER mo_handle_event->on_double_click FOR lr_event.
    CATCH cx_salv_not_found.
  ENDTRY.
ENDFORM .


"显示股票详细信息K线图
FORM show_stock_details USING p_row TYPE i
                          p_column TYPE lvc_fname.

  DATA: l_row TYPE char10.
  WRITE p_row TO l_row LEFT-JUSTIFIED.

  READ TABLE gt_stock INDEX l_row INTO gs_stock.
  IF sy-subrc = 0.
    gv_stock_id = gs_stock-sid.
    "调用屏幕,显示图片
    CALL SCREEN 100.
  ENDIF.

ENDFORM.

"刷新，暂时不用
FORM handle_user_command USING p_function TYPE salv_de_function.
  CASE p_function.
    WHEN 'REFRESH'.
      go_table->refresh( ).
  ENDCASE.
ENDFORM.

"调用上述方法，显示ALV结果
FORM show_grid.

  "获取输入的股票号码列表
  PERFORM get_input_stock_id.

  "创建本地类实例，获取实时股票信息，转化为内表格式
  CREATE OBJECT go_stock.
  go_stock->get_live_stock_info( gt_stock_id ).
  CALL METHOD go_stock->fill_alv
    CHANGING
      ct_stock = gt_stock.

  "创建OOALV，因为有SCREEN ，只能采用面向对象与面向过程的ABAP混编
  PERFORM build_alv.
  "显示OOALV
  go_table->display( ).

ENDFORM.

"获得新浪财经API的个股K线图
FORM show_picture USING i_stock_id.

  DATA: l_tmp TYPE string.

  CLEAR l_tmp.
  IF i_stock_id(1) < 6.
    CONCATENATE 'http://image.sinajs.cn/newchart/daily/n/'
    'sz' i_stock_id  '.gif' INTO l_tmp.
  ENDIF.
  IF i_stock_id(1) = '6'.
    CONCATENATE 'http://image.sinajs.cn/newchart/daily/n/'
    'sh' i_stock_id '.gif' INTO l_tmp.
  ENDIF.
  gv_url = l_tmp.

  "创建容器
  CREATE OBJECT go_dock
    EXPORTING
      repid     = sy-repid
      dynnr     = sy-dynnr
      side      = go_dock->dock_at_left
      extension = 620.
  IF sy-subrc NE 0 .
    "Instantiate container Error!
  ENDIF.

  "创建图片
  IF go_dock IS NOT INITIAL.
    CREATE OBJECT go_picture
      EXPORTING
        parent = go_dock
      EXCEPTIONS
        OTHERS = 1.
    IF sy-subrc NE 0 .
      "Instantiate Picture Error!
    ENDIF.
  ENDIF.

  "获取URL
  CALL FUNCTION 'DP_CREATE_URL'
    EXPORTING
      type                 = 'JPG/GIF'
      subtype              = cndp_sap_tab_unknown
    TABLES
      data                 = gt_data
    CHANGING
      url                  = gv_url
    EXCEPTIONS
      dp_invalid_parameter = 1
      dp_error_put_table   = 2
      dp_error_general     = 3
      OTHERS               = 4.
  IF sy-subrc <> 0.
    "Get URL Error!
  ENDIF.

  "显示图片
  CALL METHOD go_picture->load_picture_from_url
    EXPORTING
      url = gv_url.

*  CALL METHOD go_picture->set_display_mode
*    EXPORTING
*      display_mode = 3.


ENDFORM.



"定时器设定
CLASS lcl_event_handler DEFINITION.
  PUBLIC SECTION.
    METHODS:
      handle_timer FOR EVENT finished OF cl_gui_timer.
ENDCLASS.

CLASS lcl_event_handler IMPLEMENTATION.
  METHOD handle_timer.
    "重新连接API，获取最新数据
    go_stock->get_live_stock_info( gt_stock_id ).
    "填充内表
    CALL METHOD go_stock->fill_alv
      CHANGING
        ct_stock = gt_stock.
    "刷新ALV组件
    go_table->refresh( ).

    CALL METHOD go_timer->run
      EXCEPTIONS
        OTHERS = 9.
  ENDMETHOD.
ENDCLASS.


FORM init_timer.
  IF go_timer IS NOT BOUND.
    CREATE OBJECT go_timer
      EXCEPTIONS
        OTHERS = 9.
    CREATE OBJECT go_evt_hndl.
  ENDIF.
  SET HANDLER go_evt_hndl->handle_timer FOR go_timer.
  "10秒钟刷新一次
  go_timer->interval = 10.
  CALL METHOD go_timer->run
    EXCEPTIONS
      OTHERS = 9.
ENDFORM.

"屏幕100是为K线图片专用的
MODULE status_0100 OUTPUT.
  SET PF-STATUS 'ALV1'.
  "屏幕显示后，调用图片显示
  PERFORM show_picture USING gv_stock_id.
ENDMODULE.

"屏幕100是为K线图片专用的
MODULE user_command_0100 INPUT.
  CASE sy-ucomm.
    WHEN 'BACK'.
      "返回前一个界面，先删除容器组件，再删除图片组件
      CALL METHOD go_picture->free( ).
      CALL METHOD go_dock->free( ).
      "返回ALV屏幕
      LEAVE TO SCREEN 0.
    WHEN OTHERS.
  ENDCASE.
ENDMODULE.                 " USER_COMMAND_0100  INPUT