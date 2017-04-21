## 概述

> 根据仓库ID获取仓库下库位。

##   接口详细

**接口url**

```text
GET     /warehouse/locator/getAllWithWarehouseId
```

**参数列表**

| 参数名称     |      必要       | 样例 | 说明   |
|:------------|:--------------:|:----|:-------|
| warehouseId | 是<br/> String | 111 | 仓库ID |

**响应结果JSON**

```json
    {
        "success": true,
        "message": null,
        "data": [
            {
                "locatorCode": "001-01",
                "locatorId": 111
            },{
                "locatorCode": "001-01",
                "locatorId": 111
            }
            //... ...
        ]
    }
```

**响应结果JSON`data`项字段说明**

| 字段名称     | 说明     |
|:------------|:--------|
| locatorId   | 库位ID   |
| locatorCode | 库位编码 |
