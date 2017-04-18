## 概述

> 删除仓库下指定库位，如果库位下有SKU将该SKU关联到新的库位上。

##   接口详细

**接口url**

```text
POST     /warehouse/locator/remove
```

**参数列表**

| 参数名称        |      必要       | 样例       | 说明                                     |
|:---------------|:--------------:|:----------|:----------------------------------------|
| newLocatorCode | 是<br/> String | 00-200-99 | 如果删除的该库位下有SKU，SKU转移到新的库位编码 |
| locatorId      |  是<br/> int   | 1         | 删除的库位ID标识                          |

**响应成功结果JSON**

```json
    {
        "success": true,
        "message": null,
        "data": {
            "updatedSkuLocatorCodeCount": 11
        }
    }
```

> `success` 字段为业务操作是否成功，`true`表示成功，`false` 表示失败。 <br />
> `message` 字段表示失败原因。 <br />
> `data`中`updatedSkuLocatorCodeCount`表示删除该库位，库位中已有SKU转移到新库位的SKU个量。

**响应失败结果JSON**

```json
    {
        "success": false,
        "message": "删除失败，转移到新库位库位编码不存在。"
    }
```