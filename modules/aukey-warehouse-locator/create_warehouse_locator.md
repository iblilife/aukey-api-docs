## 概述

> 用于仓库库位创建

##   接口详细

**接口url**

```text
POST     /warehouse/locator/create
```

**参数列表**

| 参数名称       |      必要       | 样例           | 说明                                                  |
|:--------------|:--------------:|:--------------|:-----------------------------------------------------|
| locatorCode   | 是<br/> String | 00-200-99     | 库位编码                                              |
| locatorTypeId | 是<br/> String | `DYNAMICALLY` | 库位类型，可选值`DYNAMICALLY`(动态)或`STATICALLY`(静态)  |
| warehouseId   | 是<br/> Number | 111           | 仓库ID                                                |


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
