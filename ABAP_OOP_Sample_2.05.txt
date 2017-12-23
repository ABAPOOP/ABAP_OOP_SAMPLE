"示例程序2.5
REPORT zrep_cls_22.

"一 内表的创建
"1. 定义结构体类型，包含了三个字段，这就是表的每一行的基本类型
TYPES: BEGIN OF ty_flight,
         airline_id TYPE i,
         airline_name(20) TYPE c,
         flight_number TYPE i,
       END OF ty_flight.

"2. 依照结构体类型，定义标准表类型，并定义不唯一的默认主键 airline_id，即主键值相同的行可以存在
TYPES ty_tab_flight TYPE STANDARD TABLE OF ty_flight WITH NON-UNIQUE KEY airline_id.

"3. 依照结构体类型，定义内表变量
DATA gt_flight TYPE ty_tab_flight.

"4. 依照结构体类型，定义结构体工作区变量
DATA gs_flight TYPE ty_flight.

"5. 初始化内表工作区结构体
CLEAR gs_flight.

"6. 初始化内表
REFRESH gt_flight.

