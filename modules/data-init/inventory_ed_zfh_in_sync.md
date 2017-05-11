#### 概述
E登做自发货（国内自发货仓库）入库同步给佰易系统接口，接口根据参数中`流水号`与`SKU`幂等。<br />
__`调用方` E登，`接收方`：佰易__

#### 接口详细

__url__

```text
POST     http://data-sync-api.qa.aukeyit.com/warehouse/sync/ed_zfh_in
```

__参数列表__

| 参数            | 名称        | 样例     | 详细说明                                                                                 |
|:---------------|:-----------|:---------|:----------------------------------------------------------------------------------------|
| ~~sign~~       | 数据签名    |          | 数据签名，用于验证数据有效性 [查看sign计算](/modules/data-init/sign_build)                  |
| ~~timestamp~~  | 时间戳      |          | 调用接口当前时间戳(毫秒)                                                                   |
| no             | 流水号      | 1        | 流水号，`流水号`与`SKU`唯一                                                               |
| type           | 类型        | `IN_P`   | 入库类型，可选值：`IN_P` 采购入库、`IN_O` 其他入库                                          |
| t_number       | 单号        | 12312321 | 单号，根据类型不同给到不到单号, `IN_O` 其他入库单号不传值。例如：类型采购入库，单号则为`质检单号`  |
| in_number      | 入库单号    |          | 入库单号                                                                                 |
| sku            | SKU        | TEST_SKU | 产品SKU编码                                                                              |
| quantity       | 数量        | 10       | 入库数量                                                                                 |
| warehouse_id   | 仓库ID      |          | 入库仓库ID                                                                               |
| operator_email | 入库员Email |          | 入库员Email，必填字段，用于在佰易系统对应相关操作人员ID                                       |


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

#### 开发需求分析
> 开发/维护人员文档，接口调用无需关注

__项目svn地址__
```text
https://10.1.1.132/svn/Code/aukey-by-ed-api/trunk
```

__接口代码位置__
```text
com.aukey.by.ed.api.web.WarehouseApiController.syncEDWithZFHStockInput(request)
```


__数据存放位置__

涉及数据库：`supply_sign`<br/>

涉及表：<br/>
`storage`   采购入库记录表 <br/>
`rejects`  不良品记录表   <br/>
`stock_record` 其他入库记录表 <br/>

__采购入库类型__

根据质检单号查询质检表`qc_quality_control`获取`良品`和`不良品`数量，`良品`入库记录存放在`storage`表，`不良品`记录存放在`rejects`表。 <br />

__其他入库类型__

数据存放在`stock_record`表。


#### warehouse_inout_sync_ed_history
新增数据表`warehouse_inout_sync_ed_history` E登同步出入库到佰易系统记录，用于实现出入库接口做`幂等性`判断依据。
E登推送出入库记录成功会在该表记录一条信息，已经存在与该表的`流水号`与`SKU`记录，E登再次同步会忽略不做处理。

__warehouse_inout_sync_ed_history 表结构__
```sql
CREATE TABLE `warehouse_inout_sync_ed_history` (
    `SKU_SEQ`  int NOT NULL ,
    `SKU`  varchar(50) NOT NULL ,
    `TYPE`  char(10) NULL ,
    PRIMARY KEY (`SKU_SEQ`, `SKU`)
);
```

| 字段     | 类型         | 说明                       |
|:--------|:------------|:--------------------------|
| SKU_SEQ | int         | 根据sku出入库流水号         |
| SKU     | varchar(50) | SKU                       |
| TYPE    | char(10)    | 类型：`IN` 入库，`OUT` 出库 |

TODO: 未完待续...