"示例程序5.32
REPORT zrep_cls_012.
DATA :
      "declare super class
      go_civi_solider TYPE REF TO zcl_beichao_civilian,
      "declare sub class
      go_hua_hu TYPE REF TO zcl_huahu,
      "declare sub class
      go_hua_mulan TYPE REF TO zcl_huamulan.

START-OF-SELECTION.

  "1.创建花木兰的对象实例
  CREATE OBJECT go_hua_mulan.

  WRITE : /, / '1. 测试子类对象花木兰:'.
  "自我介绍用的是花木兰的信息，我是花木兰，女
  CALL METHOD go_hua_mulan->setup_profile
    EXPORTING
      iv_name      = '花木兰'
      iv_gender    = '女'.

  CALL METHOD go_hua_mulan->self_introduce.
  "北朝人尚武，女孩子也会武术
  CALL METHOD go_hua_mulan->show_capability.
  "当然女孩也能化妆打扮
  CALL METHOD go_hua_mulan->make_up.

  "2. 创建花弧的对象实例
  CREATE OBJECT go_hua_hu.
  WRITE : /, / '2. 测试子类对象花弧:'.
  "调用的是父类的自我介绍：我是花弧，男

  CALL METHOD go_hua_hu->setup_profile
    EXPORTING
      iv_name      = '花弧'
      iv_gender    = '男'.

  CALL METHOD go_hua_hu->self_introduce.
  "无奈年老体衰，力不从心
  CALL METHOD go_hua_hu->show_capability.
  "父类没有子类的打扮能力
  "CALL METHOD go_hua_hu->make_up.

  "3. 测试多态下的花弧的能力
  WRITE : /, / '3. 测试子类对象向上转型到父类对象，多态 - 父亲无法打仗:'.

  "父类是虚拟类，不能直接创建对象, 但可以依据实现所有方法的子类类型创建对象
  "CREATE OBJECT go_civi_solider TYPE zcl_huahu.

  "虚拟类对象变量不经创建，直接由子类对象赋值也可以
  go_civi_solider = go_hua_hu.

  "向上转型，对象是go_civi_solider，
  "此时是查看子类对象花弧的能力
  go_civi_solider = go_hua_hu.

  "在军中报名: 调用的是父类的自我介绍：我是花弧，男
  CALL METHOD go_civi_solider->self_introduce.
  "如果花弧在军中打仗: 花弧不能负担作战任务，只能充当牺牲品
  CALL METHOD go_civi_solider->show_capability.

  "4. 木兰决定代父从军
  WRITE : /, / '4. 木兰决定代父从军'.
  "子类花木兰声称自己就是花弧，准备以父亲的名义参军
  CALL METHOD go_hua_mulan->setup_profile
    EXPORTING
      iv_name      = '花弧'
      iv_gender    = '男'.

  " 5.将花木兰对象实例向上转型
  WRITE : /, / '5. 测试子类对象向上转型到父类对象，多态 - 代父从军:'.
  "向上转型，对象是go_civi_solider，名义上是花弧在参军，其实是花木兰女扮男装代父从军
  "向上转型后，对象go_hua_mulan和go_civi_solider是指向同一个内存区域
  go_civi_solider = go_hua_mulan.

  "在军中报名: 调用的是重新声明过的自我介绍：我是花弧，男
  CALL METHOD go_civi_solider->self_introduce.
  "在军中打仗: 打仗的时候，其实上阵打仗的是花木兰，打仗的功夫也是花木兰自己的能力
  CALL METHOD go_civi_solider->show_capability.
  "多态的代价，不能调用子类新增加的方法，军中也不允许花木兰使用自己新增加的方法
  "CALL METHOD go_civi_solider->make_up.


  " 6.花木兰回乡团聚
  WRITE : / , /'6. 测试父类对象向下强转型到子类对象, 花木兰重新做自己:'.
  "不必按子类类型重新创建父类对象, 因为子类曾经向上转型，向下强转型可以直接进行
  "向下强转型，花木兰自己回家重新过女儿的生活
  go_hua_mulan ?= go_civi_solider.
  "花木兰回家，重新声明自己其实是女儿的花木兰
  CALL METHOD go_hua_mulan->setup_profile
    EXPORTING
      iv_name      = '花木兰'
      iv_gender    = '女'.
  "自我介绍用的是十年后的花木兰的信息
  CALL METHOD go_hua_mulan->self_introduce.
  "花木兰依然具有能冲锋打仗的能力, 如有战事依然能够上阵
  CALL METHOD go_hua_mulan->show_capability.
  "向下强转型后，可以重新调用子类新增加的方法
  "花木兰终于可以开始使用自己的化妆打扮的能力
  CALL METHOD go_hua_mulan->make_up.
