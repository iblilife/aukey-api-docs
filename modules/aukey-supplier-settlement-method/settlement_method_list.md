## 概述

>获取供应商结算方式列表数据


##   接口详细

**接口url**

```text
GET     /settlement/list
```

**参数列表**

| 参数名称     |      必要       | 样例       | 说明                  |
|:------------|:--------------:|:----------|:---------------------|
| pageSize    | 是<br/> Number | 50        | 分页每页数据大小       |
| pageIndex   | 是<br/> Number | 1         | 分页当前页页码，从1开始 |


**响应结果JSON**

```json
{
  "total": 1000,
  "rows": [
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

> `total`表示数据总条数，`rows`为当前页数据内容。

**响应结果JSON`rows`项字段说明**

| 字段名称             | 说明                                 |
|:--------------------|:------------------------------------|
| paymentMethodId     | 供应商结算方式ID，唯一                 |
| method              | 结算方式简短描述（名称）               |
| prepay              | 预付                                 |
| accountperiod       | 账期                                 |
| paymentDurationDays | 付款期间                             |
| createUser          | 创建人用户ID                         |
| createTime          | 创建时间，格式：`yyyy-MM-dd HH:mm:ss` |
