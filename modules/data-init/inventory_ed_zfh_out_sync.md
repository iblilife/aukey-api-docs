#### 概述
E登出库数据同步给佰易系统接口，出库记录业务操作根据参数中`流水号`与`SKU`幂等。<br />
__`调用方` E登，`接收方`：佰易__

#### 接口详细

__url__

```text
POST     http://data-sync-api.qa.aukeyit.com/warehouse/sync/ed_zfh_out
```

__参数列表__

| 参数           | 名称     | 样例  | 详细说明                                                                |
|:--------------|:--------|:-----|:-----------------------------------------------------------------------|
| ~~sign~~      | 数据签名 |      | 数据签名，用于验证数据有效性 [查看sign计算](/modules/data-init/sign_build) |
| ~~timestamp~~ | 时间戳   |      | 调用接口当前时间戳(毫秒)                                                 |
| data          | 出库记录 |      | 出库记录JSON字符串                                                      |

__参数出库记录`data`样例__

```json
[
      {
        "no": 12121,
        "type": "OUT_S",
        "t_number": "",
        "sku": "TEST_SKU",
        "quantity": 10,
        "warehouse_id": 10,
        "cost": 14.55
      },{
        "no": 12122,
        "type": "OUT_O",
        "t_number": "",
        "sku": "TEST_SKU1",
        "quantity": 11,
        "warehouse_id": 10,
        "cost": 18.55
      }
      //......
]
```

__参数出库记录`data`JSON字符串字段详细__

| 参数          | 名称     | 数据类型   | 详细说明                                                                                         |
|:-------------|:--------|:----------|:------------------------------------------------------------------------------------------------|
| no           | 流水号   | `String`  | `流水号`与`SKU`唯一。                                                                            |
| type         | 类型     | `String`  | 出库类型，可选值：`OUT_S` 订单出库、`OUT_O` 其他出库                                                |
| t_number     | 单号     | `String`  | 单号，根据类型不同给到不同单号。<br />`OUT_O`其他出库，单号不传值<br />`OUT_S`订单出库，单号则为销售单号 |
| sku          | SKU     | `Number`  | 产品SKU编码                                                                                      |
| quantity     | 数量     | `Number`  | 出库数量                                                                                         |
| warehouse_id | 仓库ID   | `Number`  | 出库仓库ID                                                                                       |
| sku_cost     | sku成本  | `Number`  | 单个SKU成本金额                                                                                  |

__响应结果JSON__

```json
{
    "success": true,
    "message": null
}
```
`success` 字段为业务操作是否成功，`true`表示成功，`false` 表示失败。 <br />
`message` 字段表示失败原因。


<br /><br /><br />

---

#### 开发文档
> 开发/维护人员文档，接口调用无需关注

__项目svn地址__
```text
https://10.1.1.132/svn/Code/aukey-by-ed-api/trunk
```

__接口代码位置__
```text
com.aukey.by.ed.api.web.WarehouseApiController.syncEDWithZFHStockOutput(request)
```

__处理流程__

- 判断出库记录是否已处理 `supply_sign.warehouse_inout_sync_ed_history`
- 新增出库记录 `supply_sign.stock_record`
- 扣减库存 `supply_sign.stock`
