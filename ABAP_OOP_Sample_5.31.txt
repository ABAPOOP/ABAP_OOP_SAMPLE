"示例程序5.31
"父类与子类的全局类code based 代码
"使用方法：如创建zcl_beichao_civilian全局类，
"用SE24创建全局类zcl_beichao_civilian，然后点击code based按钮，切换至代码模式，
"复制以下类的DEFINITION和IMPLEMENTATION代码，全部粘贴到类的代码模式中，激活即可
"返回Form Based模式，即可查看类的属性和方法。

CLASS zcl_beichao_civilian DEFINITION
  PUBLIC
  ABSTRACT
  CREATE PUBLIC .

  PUBLIC SECTION.

    DATA mv_name TYPE string READ-ONLY .
    DATA mv_gender TYPE string READ-ONLY .

    METHODS setup_profile
      IMPORTING
        !iv_name TYPE string
        !iv_gender TYPE string .
    METHODS show_capability
    ABSTRACT .
    METHODS self_introduce .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS zcl_beichao_civilian IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_BEICHAO_CIVILIAN->SELF_INTRODUCE
* +-------------------------------------------------------------------------------------------------+
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD self_introduce.
    WRITE:/ '我是 ',  mv_name, '性别 ', mv_gender.
  ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_BEICHAO_CIVILIAN->SETUP_PROFILE
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_NAME                        TYPE        STRING
* | [--->] IV_GENDER                      TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD setup_profile.
    mv_name = iv_name.
    mv_gender = iv_gender.
  ENDMETHOD.
ENDCLASS.



CLASS zcl_huahu DEFINITION
  PUBLIC
  INHERITING FROM zcl_beichao_civilian
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.

    METHODS show_capability
      REDEFINITION .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS zcl_huahu IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_HUAHU->SHOW_CAPABILITY
* +-------------------------------------------------------------------------------------------------+
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD show_capability.
    WRITE: / '年老体衰，耕读持家'.
  ENDMETHOD.
ENDCLASS.



CLASS zcl_huamulan DEFINITION
  PUBLIC
  INHERITING FROM zcl_beichao_civilian
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.

    METHODS make_up .

    METHODS show_capability
      REDEFINITION .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS zcl_huamulan IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_HUAMULAN->MAKE_UP
* +-------------------------------------------------------------------------------------------------+
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD make_up.
    WRITE: / '当窗理云鬓，对镜贴花黄'.
  ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_HUAMULAN->SHOW_CAPABILITY
* +-------------------------------------------------------------------------------------------------+
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD show_capability.
    WRITE: / '年轻力壮，冲锋陷阵'.
  ENDMETHOD.
ENDCLASS.




