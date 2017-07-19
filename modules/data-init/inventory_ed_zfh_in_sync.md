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

| 参数             | 名称        | 样例                | 详细说明                                                                                                                                                                                               |
|:----------------|:-----------|:--------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ~~sign~~        | 数据签名    |                     | 数据签名，用于验证数据有效性 [查看sign计算](/modules/data-init/sign_build)                                                                                                                                |
| ~~timestamp~~   | 时间戳      |                     | 调用接口当前时间戳(毫秒)                                                                                                                                                                                 |
| no*             | 流水号      | 1                   | 流水号，`流水号`与`SKU`唯一                                                                                                                                                                             |
| type*           | 类型        | `PURCHASE_IN`       | 入库类型，参考[E登与佰易 出入库类型对照表](/modules/data-init/inventory_ed_zfh_in_sync?id=e%e7%99%bb%e4%b8%8e%e4%bd%b0%e6%98%93-%e5%87%ba%e5%85%a5%e5%ba%93%e7%b1%bb%e5%9e%8b%e5%af%b9%e7%85%a7%e8%a1%a8)  |
| t_number*       | 单号        |                     | 单号，根据类型不同给到不到单号。<br/>采购入库，单号则为`质检单号` <br/>调拨入库，单号则为`调拨单号`                                                                                                             |
| in_number*      | 入库单号    |                     | E登入库单号(用于记录, 如果出现数据异常用于回查)                                                                                                                                                            |
| sku*            | SKU        | TEST_SKU            | 产品SKU编码                                                                                                                                                                                            |
| quantity*       | 数量        | 10                  | 入库数量                                                                                                                                                                                               |
| warehouse_id*   | 仓库ID      |                     | 入库仓库ID                                                                                                                                                                                             |
| operator_email* | 入库员Email |                     | 入库员Email，必填字段，用于在佰易系统对应相关操作人员ID                                                                                                                                                     |
| create_time*    | 入库时间    | 2017-07-19 15:47:25 | 入库时间, 字符串, 格式: yyyy:MM:dd HH:mm:ss                                                                                                                                                             |


__响应结果JSON__

```json
{
    "success": true,
    "message": null
}
```
`success` 字段为业务操作是否成功，`true`表示成功，`false` 表示失败。 <br />
`message` 字段表示失败原因。

#### E登与佰易 出入库类型对照表

| 易登                              | 佰易                                                | 佰易接口类型字符         |
|:---------------------------------|:----------------------------------------------------|:-----------------------|
| _以下为入库类型_                   |                                                     |                        |
| 采购入库                          | 根据质检对应:<br/>1. 质检良品入库<br/>2. 质检不良品入库  | `PURCHASE_IN`          |
| 内采入库                          | 内部采购入库                                         | `WITHIN_PURCHASE_IN`   |
| 盘盈入库                          | 盘盈入库                                             | `INV_PROFIT_IN`        |
| 转换SKU入库                       | 转换入库                                             | `CONVERT_SKU_IN`       |
| 样品入库                          | 还样入库                                             | `SAMPLE_IN`            |
| 其他入库                          | 其他入库                                             | `OTHER_IN`             |
| 退货入库                          | 销售退货入库                                         | `RETURN_IN`            |
| 虚拟入库                          | 冲销入库                                             | `VIRTUAL_IN`           |
| 调整入库                          | 冲销入库                                             | `ADJUST_IN`            |
| 调拨(调仓)入库                     | 调拨入库                                             | `ALLOT_IN`             |
| 红字冲销                          | 冲销入库                                             | `WRITE_OFF_IN`         |
| ~~不良品入库~~`通过采购入库质检处理` | ~~不良品入库~~                                       | ~~`FAIL_SKU_IN`~~      |
|                                  |                                                     |                        |
| _以下为出库类型_                   |                                                     |                        |
| 调整出库                          | 冲销出库                                             | `ADJUST_OUT`           |
| 调拨(调仓)出库                     | 调拨出库                                             | `ALLOT_OUT`            |
| 虚拟出库                          | 冲销出库                                             | `VIRTUAL_OUT`          |
| 其他出库                          | 其他出库                                             | `OTHER_OUT`            |
| 转换SKU出库                       | 转换出库                                             | `CONVERT_SKU_OUT`      |
| 盘亏出库                          | 盘亏出库                                             | `INV_LOSS_OUT`         |
| 报损出库                          | 报损出库                                             | `SCRAP_OUT`            |
| 内销出库                          | 内部销售出库                                         | `WITHIN_SALE_OUT`      |
| 订单出库                          | 销售订单出库                                         | `SALE_ORDER_OUT`       |
| 不良品出库                         | 不良品出库                                           | `FAIL_SKU_OUT`         |
| 采购退货                          | 采购退货出库                                         | `PURCHASE_RETURN_OUT`  |

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
1.  根据仓库ID，SKU获取库存记录，存在记录累加`stock`表`quantity_available`字段数据，不存在新建记录。

2.  采购入库类型特殊处理, 数据来源逻辑[处理逻辑-良品数量处理](/modules/data-init/inventory_ed_zfh_in_sync?id=处理逻辑),
存在记录累加`supply_sign.stock_request_inquiry`表`quantity_available`字段数据. 不存在新建记录. 这里只记录良品数量.

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
根据良品数量/不良品数量分摊到每条采购需求记录，
分摊数量不能超过采购需求记录的`quantity`字段的值减去所对应的`supply_sign.storage_requirement`表`supply_quantity`已分摊数量（根据采购需求单号匹配）。
1.  良品数量处理 <br />
    优先匹配`update_date`（处理时间）最早（小）的采购订单需求。<br />
    分摊数量记录插入到数据表`supply_sign.storage_requirement`中`type`为`1`表示良品。

2.  不良品数量处理(大致处理逻辑与良品一致) <br />
    优先匹配`update_date`（处理时间）最晚（大）的采购订单需求。<br />
    分摊数量记录插入到数据表`supply_sign.storage_requirement`中`type`为`2`表示不良品。

---


##### 状态变更

1.  需求单状态
    特殊仓库处理, 仓库ID为 `36`, `64`, `7`, `28`, `77`.<br/>
    `supply_sign.storage_requirement`表字段`supply_quantity`入库数量等于`supply_chain.requirement`表的`quantity`则修改状态为 需求完结, 否则不变化.
    `supply_chain.requirement`-`requirement_status`->`6`:需求完结
    <br/>

    其它仓库<br/>
    `supply_chain.requirement`-`requirement_status`->`5`:发货计划 <br/>

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


