## 概述

> 根据库位ID获取库位下SKU相关信息列表

##   接口详细

**接口url**

```text
GET     /warehouse/locator/sku/list
```

**参数列表**

| 参数名称     |      必要       | 样例 | 说明          |
|:------------|:--------------:|:----|:--------------|
| locatorId   |  是 <br/> int  | 111 | 仓库库位ID标识 |

**响应结果JSON**

```json
    {
        "success": true,
        "message": null,
        "data": [
            {
                "sku": "1212121",
                "quantity": 12,
                "lastInStockStamp": "2017-04-18 14:37:10"
            },{
                "sku": "121212122",
                "quantity": 13,
                "lastInStockStamp": "2017-04-18 14:37:10"
            }
            //... ...
        ]
    }
```

> `success` 字段为业务操作是否成功，`true`表示成功，`false` 表示失败。 <br />
> `message` 字段表示失败原因。 <br />
> `data` 为sku数据项数组

`data` 为sku数据项字段说明：

| 字段名称          | 说明         |
|:-----------------|:------------|
| sku              | sku         |
| quantity         | 在库数量     |
| lastInStockStamp | 最近入库时间  |
