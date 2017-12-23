"示例程序2.2
REPORT zrep_cls_08.

DATA:    gv_counter  TYPE i VALUE 0.

"此处不符合命名规则，应该命名为gc_team1, gc_team2
CONSTANTS: team1(30) TYPE c     VALUE 'Atlanta Hawks',
           team2     LIKE team1 VALUE 'Boston Celtics'.
"作用域为全局有效
WRITE: / gv_counter, 0, team1, team2.

DO 5 TIMES.
  PERFORM add_value.
ENDDO.

"作用域继续为全局有效
WRITE: / gv_counter, 0, team1, team2.

FORM add_value.
  "子程序中, 变量会每次都重新定义并初始化为0
  DATA:    lv_counter  TYPE i VALUE 0.

  "子程序中作用域被局部变量替代
  "此处不符合命名规则，应该命名为lc_team1, lc_team2
  "所以采用良好的命名规则也是避免这种值替代情况发生的好办法
  CONSTANTS: team1(30) TYPE c     VALUE 'Philadelphia 76ers',
             team2     LIKE team1 VALUE 'Chicago Bulls'.
  gv_counter = gv_counter + 1.
  lv_counter = lv_counter + 1.
  WRITE: / gv_counter, lv_counter, team1, team2.
ENDFORM.                    "add_value
