#### 概述
E登获取佰易系统海外仓掉不入库记录数据接口。<br/>
__`调用方` E登，`接收方`：佰易__

#### 接口详细

__url__

```text
GET     http://data-sync-api.qa.aukeyit.com/warehouse/sync/overseas_allot_in
```

__参数列表__
> 带`*`为必填字段, `删除线`为废弃字段

| 参数           | 名称           | 样例                  | 详细说明                                                                        |
|:--------------|:--------------|:----------------------|:-------------------------------------------------------------------------------|
| ~~sign~~      | 数据签名       |                       | 数据签名，用于验证数据有效性, 必填项。[查看sign计算](/modules/data-init/sign_build) |
| ~~timestamp~~ | 时间戳         |                       | 调用接口当前时间戳(毫秒)。                                                        |
| sku           | SKU           | TEST_SKU              | sku条件                                                                        |
| warehouse_id  | 仓库ID         |                       | 仓库ID                                                                         |
| from_time     | 起始时间(包含)  | 2017-05-10 18: 59:56  | 格式：`yyyy-MM-dd HH:mm:ss`                                                    |
| thru_time     | 截至时间(包含)  |                       | 格式：`yyyy-MM-dd HH:mm:ss`                                                    |

__响应结果JSON__

```json
{
    "success": true,
    "message": null,
    "data": [
        {
            "allot_num": "DB17051100007",
            "warehouse_id": 10,
            "sku": "100002_3",
            "quantity": 10
        },{
            "allot_num": "DB17051100002",
            "warehouse_id": 10,
            "sku": "100002_1",
            "quantity": 11
        }
        //......
    ]
}
```

__`data`数据项字段说明__

| 参数          | 名称     | 详细说明 |
|:-------------|:--------|:--------|
| allot_num    | 调拨单号 |         |
| warehouse_id | 仓库ID   |         |
| sku          | SKU     |         |
| quantity     | 数量     |  　       |

`success` 字段为业务操作是否成功，`true`表示成功，`false` 表示失败。 <br />
`message` 字段表示失败原因。




#### 开发需求分析
> 开发/维护人员文档，接口调用无需关注

__项目svn地址__
```text
https://10.1.1.132/svn/Code/aukey-by-ed-api/trunk
```

__接口代码位置__
```text
com.aukey.by.ed.api.web.WarehouseApiController.getWarehouseOverseasAllotInput(request)
```


__数据库相关__

涉及数据库：
- `supply_delivery`

涉及表：
- `supply_delivery.transfer_slip` 调拨主表
- `supply_delivery.transfer_detail` 调拨详细表

数据SQL:
```sql
SELECT 
ts.transfer_no AS transferNo, td.sku AS sku, 
SUM(td.box_count) AS quantity, ts.warehouse_id AS warehouseId
FROM transfer_detail td LEFT JOIN transfer_slip ts ON(td.transfer_id = ts.transfer_id)
WHERE ts.data_status = '1' AND ts.transfer_status = '2'
GROUP BY ts.transfer_no, td.sku
```