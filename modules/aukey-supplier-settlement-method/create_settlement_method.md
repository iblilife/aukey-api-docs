## 概述

>新增供应商结算方式


##   接口详细

**接口url**

```text
POST     /settlement/create
```

**参数列表**

| 参数名称             |      必要       | 样例              | 说明                                                  |
|:--------------------|:--------------:|:-----------------|:-----------------------------------------------------|
| method              | 是<br/> String | 预付25%,剩余半月结 | 结算方式简短描述（名称）                                |
| prepay              | 是<br/> Number | 25               | 预付, `预付与账期相加必须为100`                         |
| accountperiod       | 是<br/> Number | 75               | 账期, `预付与账期相加必须为100`                         |
| paymentDurationDays | 是<br/> Number | 15               | 付款期间，付款期间的可选值：`0`, `15`, `30`, `60`, `90`  |


**响应成功结果JSON**

```json
    {
        "success": true,
        "message": null
    }
```

**响应失败结果JSON**

```json
    {
        "success": false,
        "message": "付款期间不能为空。"
    }
```

> `success` 字段为业务操作是否成功，`true`表示成功，`false` 表示失败。 <br />
> `message` 字段表示失败原因。
