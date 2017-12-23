"示例程序7.23
REPORT zrep_cls_032.

DATA:
  go_subject   TYPE REF TO zcl_subject,
  go_observer1 TYPE REF TO zcl_observer,
  go_observer2 TYPE REF TO zcl_observer.
"创建被观察对象
CREATE OBJECT go_subject.
"创建观察者1
CREATE OBJECT go_observer1.
"创建观察者2
CREATE OBJECT go_observer2.

"设定观察者1的事件句柄指向被观察者
SET HANDLER go_observer1->action_on_subject_changed FOR go_subject.
"设定观察者2的事件句柄指向被观察者
SET HANDLER go_observer2->action_on_subject_changed FOR go_subject.

"被观察者状态修改，触发自身事件，然后触发事件处理者（Event handler  Method）
go_subject->change_status( ).
