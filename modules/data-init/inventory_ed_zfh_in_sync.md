#### 概述
E登做自发货（国内自发货仓库）入库同步给佰易系统接口，接口根据参数中`流水号`与`SKU`幂等。<br />
__`调用方` E登，`接收方`：佰易__

#### 接口详细

__url__

```text
POST     http://data-sync-api.qa.aukeyit.com/warehouse/sync/ed_zfh_in
```

__参数列表__
> 带`*`为必填字段, `删除线`为废弃字段

| 参数             | 名称        | 样例     | 详细说明                                                                                                            |
|:----------------|:-----------|:---------|:-------------------------------------------------------------------------------------------------------------------|
| ~~sign~~        | 数据签名    |          | 数据签名，用于验证数据有效性 [查看sign计算](/modules/data-init/sign_build)                                             |
| ~~timestamp~~   | 时间戳      |          | 调用接口当前时间戳(毫秒)                                                                                              |
| no*             | 流水号      | 1        | 流水号，`流水号`与`SKU`唯一                                                                                          |
| type*           | 类型        | `IN_P`   | 入库类型，可选值：`IN_P` 采购入库、`IN_A` 调拨入库、`IN_O` 其他入库                                                      |
| t_number*       | 单号        |          | 单号，根据类型不同给到不到单号, `IN_O` 其他入库单号不传值。<br/>采购入库，单号则为`质检单号` <br/>调拨入库，单号则为`调拨单号`  |
| in_number*      | 入库单号    |          | 入库单号                                                                                                            |
| sku*            | SKU        | TEST_SKU | 产品SKU编码                                                                                                         |
| quantity*       | 数量        | 10       | 入库数量                                                                                                            |
| warehouse_id*   | 仓库ID      |          | 入库仓库ID                                                                                                          |
| operator_email* | 入库员Email |          | 入库员Email，必填字段，用于在佰易系统对应相关操作人员ID                                                                  |
| sku_cost*       | sku成本     | 8.88     | 单个SKU成本金额                                                                                                     |


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
com.aukey.by.ed.api.web.WarehouseApiController.syncEDWithZFHStockInput(request)
```


__数据库相关__

涉及数据库：
- `supply_sign`
- `supply_delivery`
- `supply_chain`

涉及表：
- `storage`   采购入库记录表
- `rejects`  不良品记录表
- `stock_record` 其他入库记录表
- `stock` 库存表
- `supply_delivery.transfer_slip` 调拨主表，`supply_delivery.transfer_detail` 调拨详细表

##### 不同入库类型分别处理
- 采购入库类型 <br />
    根据质检单号查询质检表`qc_quality_control`获取`良品`和`不良品`数量，`良品`入库记录存放在`storage`表，`不良品`记录存放在`rejects`表。 <br />
- 调拨入库类型 <br />
    根据调拨单号与sku修改`supply_delivery.transfer_detail`表处理状态`post_status`为`1`。<br/>
    插入数据`stock_record`表, `type`为`3`表示调拨入库。
- 其他入库类型 <br />
    插入数据`stock_record`表, `type`为`1`表示其他入库。

##### 库存累加
根据仓库ID，SKU获取库存记录，存在记录累加`stock`表`quantity_available`字段数据，不存在新建记录。

##### 数据有效性

根据质检单号查询质检表`qc_quality_control`获取`良品数量`、`不良品数量`、`采购单ID`。 <br />

`{采购需求数量}`：根据`采购单ID`与`SKU`查询`supply_chain.purchase_demand`采购需求表（多条记录）合计`quantity`字段值。 <br />

`{采购需求单号}`: 根据`采购单ID`与`SKU`查询`supply_chain.purchase_demand`采购需求表（多条记录）的所有`purchase_demand_id`字段值。 <br />

`{已处理数量}`：根据`{采购需求单号}`查询`supply_sign.storage_requirement`（多条记录）合计`supply_quantity`字段值。 <br />

__如果`{已处理数量}`+`良品数量`+`不良品数量` > `{采购需求数量}`则该条入库数据有误，不予处理并抛出异常__

---

##### 处理逻辑
根据`采购单ID`与`SKU`查询`supply_chain.purchase_demand`采购需求表（多条记录）。<br />
```sql
    SELECT * FROM purchase_demand WHERE sku_code = :skuCode 
    AND purchase_order_id = :purchaseOrderId ORDER BY update_date ASC --不良品为：DESC
```
根据良品数量/不良品数量分摊到每条采购需求记录，分摊数量不能超过采购需求记录的`quantity`字段的值减去所对应的`supply_sign.storage_requirement`表`supply_quantity`已分摊数量（根据采购需求单号匹配）。
1.  良品数量处理 <br />
    优先匹配`update_date`（处理时间）最早（小）的采购订单需求。<br />
    分摊数量记录插入到数据表`supply_sign.storage_requirement`中`type`为`1`表示良品。

2.  不良品数量处理(大致处理逻辑与良品一致) <br />
    优先匹配`update_date`（处理时间）最晚（大）的采购订单需求。<br />
    分摊数量记录插入到数据表`supply_sign.storage_requirement`中`type`为`2`表示不良品。

---


##### 状态变更

1.  需求单状态
    `supply_chain.requirement`-`requirement_status`->`5`:发货计划
2.  采购单状态
    `supply_chain.purchase_order`-`order_status`->`3`:部分入库、`4`:全部入库
3.  质检单状态
    `supply_sign.qc_quality_control.qc_status`->`4`

---


#### warehouse_inout_sync_ed_history
新增数据表`warehouse_inout_sync_ed_history` E登同步出入库到佰易系统记录，用于实现出入库接口做`幂等性`判断依据。
E登推送出入库记录成功会在该表记录一条信息，已经存在与该表的`流水号`与`SKU`记录，E登再次同步会忽略不做处理。

__warehouse_inout_sync_ed_history 表结构__
```sql
CREATE TABLE `warehouse_inout_sync_ed_history` (
    `HISTORY_ID` varchar(100) NOT NULL ,
    `SKU_SEQ`  int NOT NULL ,
    `SKU`  varchar(50) NOT NULL ,
    `TYPE`  char(10) NULL ,
    PRIMARY KEY (`SKU_SEQ`, `SKU`)
);
```

| 字段        | 类型          | 说明                                      |
|:-----------|:-------------|:-----------------------------------------|
| HISTORY_ID | varchar(100) | 主键由字段 `SKU_SEQ` + `#` + `SKU`拼接而成 |
| SKU_SEQ    | int          | 根据sku出入库流水号                        |
| SKU        | varchar(50)  | SKU                                      |
| TYPE       | char(10)     | 类型：`IN` 入库，`OUT` 出库                |


