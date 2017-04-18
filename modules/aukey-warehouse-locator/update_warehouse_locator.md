## 概述

> 修改库位编码

##   接口详细

**接口url**

```text
POST     /warehouse/locator/update
```

**参数列表**

| 参数名称        |      必要       | 样例       | 说明            |
|:---------------|:--------------:|:----------|:---------------|
| newLocatorCode | 是<br/> String | 00-200-99 | 修改后新库位编码 |
| locatorId      |  是<br/> int   | 11        | 库位ID标识          |


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
        "message": "库位编码在该仓库下已经存在。"
    }
```

> `success` 字段为业务操作是否成功，`true`表示成功，`false` 表示失败。 <br />
> `message` 字段表示失败原因。
