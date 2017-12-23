"示例程序8.17（HANA）
"本程序为SAP HANA 1610版本
REPORT zrep_cls_hana06.

"定义父类，商品类
CLASS
  zcl_goods DEFINITION INHERITING FROM object ABSTRACT.
  PUBLIC SECTION.
    DATA mv_name TYPE string.
    DATA mv_price TYPE p DECIMALS 2.
ENDCLASS.


"定义子类，平板电脑类
CLASS
  zcl_pad DEFINITION INHERITING FROM zcl_goods.
  PUBLIC SECTION.
    METHODS print_os_desc.
ENDCLASS.

CLASS zcl_pad IMPLEMENTATION.
  METHOD print_os_desc.
    WRITE:  ' Pad OS: Mobile OS'.
  ENDMETHOD.
ENDCLASS.

"定义子类，电话类
CLASS
  zcl_phone DEFINITION INHERITING FROM zcl_goods.
  PUBLIC SECTION.
    METHODS print_phone_mode.
ENDCLASS.

CLASS zcl_phone IMPLEMENTATION.

  METHOD print_phone_mode.
    WRITE:  ' Phone Mode: 4G'.
  ENDMETHOD.
ENDCLASS.

"定义购物车类
CLASS
  zcl_shop_cart DEFINITION .
  PUBLIC SECTION.
    TYPES:
      ty_tab_goods TYPE STANDARD TABLE OF REF TO zcl_goods .
    DATA mt_object TYPE ty_tab_goods .
    METHODS add_item
      IMPORTING
        !io_object TYPE REF TO zcl_goods .
    METHODS remove_item
      IMPORTING
        !iv_index TYPE sy-tabix.
    METHODS get_count
      EXPORTING
        VALUE(ev_count) TYPE i .
    METHODS get_price
      EXPORTING
        VALUE(ev_price) TYPE p.
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_shop_cart IMPLEMENTATION.
  METHOD add_item.
    IF io_object IS BOUND.
      INSERT io_object INTO TABLE mt_object.
    ENDIF.
  ENDMETHOD.

  METHOD remove_item.
    IF iv_index IS NOT INITIAL.
      DELETE mt_object INDEX iv_index.
    ENDIF.
  ENDMETHOD.

  METHOD get_count.
    DATA lv_count TYPE i VALUE 0.
    DESCRIBE TABLE mt_object LINES lv_count.
    ev_count = lv_count.
  ENDMETHOD.

  METHOD get_price.
    DATA lv_price TYPE p.
    DATA lv_object TYPE REF TO zcl_goods.
    LOOP AT mt_object INTO lv_object.
      lv_price = lv_price + lv_object->mv_price.
    ENDLOOP.
    ev_price = lv_price.
  ENDMETHOD.
ENDCLASS.

START-OF-SELECTION.
"定义对象变量
  DATA go_cart TYPE REF TO zcl_shop_cart.
  DATA go_goods TYPE REF TO zcl_goods.
  DATA go_pad TYPE REF TO zcl_pad.
  DATA go_phone TYPE REF TO zcl_phone.

  DATA l_i TYPE i.
  DATA l_p TYPE p DECIMALS 2.
"创建购物车对象
  go_cart = NEW zcl_shop_cart( ).

"模拟从采购订单中获取记录，创建对应对象，
"插入到购物车的商品列表中,此处为自动上转型
  go_pad = NEW zcl_pad( ).
  go_pad->mv_name = 'Pad - MS Surface Pro 4'.
  go_pad->mv_price = 7000.
  go_cart->add_item( go_pad ).
  FREE go_pad.

  go_pad = NEW zcl_pad( ).
  go_pad->mv_name = 'Pad - Apple iPad Air 2'.
  go_pad->mv_price = 6000.
  go_cart->add_item( go_pad ).
  FREE go_pad.

  go_phone = NEW zcl_phone( ).
  go_phone->mv_name = 'Phone - Apple iPhone 7'.
  go_phone->mv_price = 7000.
  go_cart->add_item( go_phone ).
"添加一个商品
  go_cart->add_item( go_phone ).
"再删除这个商品
  go_cart->remove_item( 4 ).
  FREE go_phone.

"调用购物车方法，获取订单中的商品数量和总价
  CALL METHOD go_cart->get_count
    IMPORTING
      ev_count = l_i.
  WRITE: / '选取商品数量 ', l_i.

  CALL METHOD go_cart->get_price
    IMPORTING
      ev_price = l_p.
  WRITE: / '选取商品总价 ', l_p.


"获取订单中的每一个商品，
"根据商品的类型向下强转型，然后才能重新调用子类的方法和特有属性
   WRITE: /,/ ' 商品名称         ', '            商品价格  ', '  商品特有信息  '.
  LOOP AT go_cart->mt_object INTO go_goods.
" IS INSTANCE OF 操作符在 ABAP 7.5 可用
   IF ( go_goods IS INSTANCE OF zcl_phone ) .
    TRY.
        go_phone ?= go_goods.
        WRITE: / go_phone->mv_name , go_phone->mv_price.
        go_phone->print_phone_mode( ).
      CATCH cx_sy_move_cast_error.
        WRITE: / 'phone wrong cast'.
    ENDTRY.
   ENDIF.
   IF ( go_goods IS INSTANCE OF zcl_pad ) .
    TRY.
        go_pad ?= go_goods.
        WRITE: / go_pad->mv_name , go_pad->mv_price.
        go_pad->print_os_desc( ).
        CATCH cx_sy_move_cast_error.
          WRITE: / 'pad wrong cast'.
    ENDTRY.
   ENDIF.
  ENDLOOP.
