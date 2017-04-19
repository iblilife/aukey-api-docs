## 概述

>提供仓库ID和库位编码返回该仓库下库位编码是否存在。

##   接口详细

**接口url**

```text
GET     /warehouse/locator/exists
```

**参数列表**

| 参数名称     |      必要       | 样例       | 说明                  |
|:------------|:--------------:|:----------|:---------------------|
| locatorCode | 是<br/> String | 00-200-99 | 库位编码              |
| warehouseId | 是<br/> Number | 111       | 仓库ID                |

**响应结果JSON**

```json
    {
        "success": true,
        "message": null,
        "data": {
          "exists": true,
          "locatorId": 123
        }
    }
```

> `success` 字段为业务操作是否成功，`true`表示成功，`false` 表示失败。 <br />
> `message` 字段表示失败原因。 <br />
> `data` -> `exists` 值为`true`表示该仓库下库位存在, `false`表示该仓库下库位不存在。 <br />
> `data` -> `locatorId` 值为库位ID