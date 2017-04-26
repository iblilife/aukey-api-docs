## 概述
> 根据ID获取供应商结算方式详情

##   接口详细

**接口url**

```text
GET     /settlement/getDesWithIds
```

**参数列表**

| 参数名称             |      必要       | 样例          | 说明                                     |
|:--------------------|:--------------:|:-------------|:----------------------------------------|
| settlementMethodIds | 是<br/> String | 111,112,1113 | 供应商结算方式ID, 多个请用英文逗号(`,`)隔开  |

**响应结果JSON**

```json
{
  "success": true,
  "message": null,
  "data": [
    {
        "paymentMethodId": 1,
        "method": "预付25%,剩余半月结",
        "prepay": "25",
        "accountperiod": "75",
        "paymentDurationDays": 15,
        "createUser": 111,
        "createTime": "2017-04-19 12:29:52"
    },{
        "paymentMethodId": 1,
        "method": "预付25%,剩余半月结",
        "prepay": "25",
        "accountperiod": "75",
        "paymentDurationDays": 15,
        "createUser": 111,
        "createTime": "2017-04-19 12:29:52"
     }
    //... ...
  ]
}
```

**响应结果JSON`data`项字段说明**

| 字段名称             | 说明                                 |
|:--------------------|:------------------------------------|
| paymentMethodId     | 供应商结算方式ID，唯一                 |
| method              | 结算方式简短描述（名称）               |
| prepay              | 预付                                 |
| accountperiod       | 账期                                 |
| paymentDurationDays | 付款期间                             |
| createUser          | 创建人用户ID                         |
| createTime          | 创建时间，格式：`yyyy-MM-dd HH:mm:ss` |

