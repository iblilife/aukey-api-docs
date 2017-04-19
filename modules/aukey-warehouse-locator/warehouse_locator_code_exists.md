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
        "message": null
    }
```

`success`值为`true`表示存在`false`表示不存在。