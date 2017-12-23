"示例程序7.17
CLASS zcl_inventory_mgmt DEFINITION
  PUBLIC
  ABSTRACT
  CREATE PUBLIC .

  PUBLIC SECTION.

    TYPES:
    BEGIN OF ty_purchasing_group,
      group_id TYPE string,
      gi_group TYPE REF TO zif_purchasing_group,
    END OF ty_purchasing_group.

    TYPES:
    ty_tab_purchasing_group TYPE STANDARD TABLE OF ty_purchasing_group
    WITH KEY group_id.


    DATA mt_purchasing_group TYPE ty_tab_purchasing_group.

    METHODS attach .
    METHODS detach .
    METHODS notify_reorder .
