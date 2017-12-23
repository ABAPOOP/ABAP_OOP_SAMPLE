"示例程序7.14
  METHOD evaluation.
    WRITE: / '2. 质量： ', IV_VENDOR_ID.
    WRITE: / '2.1收货检验'.
    WRITE: / '2.2质量审计	 '.
    WRITE: / '2.3抱怨/拒绝水平  '.
  ENDMETHOD.

  METHOD evaluation.
    WRITE: / '3. 服务采购： ', IV_VENDOR_ID.
    WRITE: / '3.1可靠性'.
    WRITE: / '3.2持续改善的服务   '.
    WRITE: / '3.3服务执行时间的灵活性   '.
  ENDMETHOD.

  METHOD evaluation.
    WRITE: / '4.交货准确度评估 ', IV_VENDOR_ID.
    WRITE: / '4.1按时交货的表现 '.
    WRITE: / '4.2数量可靠性   '.
    WRITE: / '4.3对装运须知的遵守情况   '.
    WRITE: / '4.4确认日期 '.
  ENDMETHOD.

  METHOD calculation.
    WRITE:/ '加权平均：' , IV_VENDOR_ID.
    WRITE:/ '价格40%'.
    WRITE:/ '质量30%'.
    WRITE:/ '服务20%'.
    WRITE:/ '交货10%'.
  ENDMETHOD. 
