#### 概述
E登获取佰易系统海外仓掉不入库记录数据接口。<br/>
__`调用方` E登，`接收方`：佰易__

#### 接口详细

__url__

```text
GET     http://data-sync-api.qa.aukeyit.com/warehouse/sync/overseas_allot_in
```

__参数列表__

| 参数       | 名称     | 样例                  | 详细说明                                                     |
|:----------|:--------|:---------------------|:------------------------------------------------------------|
| sign      | 数据签名 |                      | 数据签名，用于验证数据有效性, 必填项。[查看sign计算](/modules/data-init/sign_build)                           |
| timestamp | 时间戳   |                      | 调用接口当前时间戳(毫秒), 必填项。                              |
| sku       | SKU     | TEST_SKU             | sku条件与起始时间条件必需给其一。                             |
| from_time | 起始时间 | 2017-05-10 18: 59:56 | 格式：`yyyy-MM-dd HH:mm:ss`，sku条件与起始时间条件必需给其一。 |
| thru_time | 截至时间 |                      | 格式：`yyyy-MM-dd HH:mm:ss`, 选填条件                         |

__响应结果JSON__

```json
{
    "success": true,
    "message": null,
    "data": [
        {
            "id": 11000,
            "warehouse_id": 10,
            "sku": "100002_3",
            "quantity": 10
        },{
            "id": 11002,
            "warehouse_id": 10,
            "sku": "100002_1",
            "quantity": 11
        }
        //......
    ]
}
```

__`data`数据项字段说明__

| 参数          | 名称         | 详细说明            |
|:-------------|:------------|:-------------------|
| id           | 出入库记录ID | 出入库记录ID，唯一。  |
| warehouse_id | 仓库ID       |                    |
| sku          | SKU         |                    |
| quantity     | 数量         | 调拨入库数量         |

`success` 字段为业务操作是否成功，`true`表示成功，`false` 表示失败。 <br />
`message` 字段表示失败原因。




#### 开发需求分析
> 开发/维护人员文档，接口调用无需关注

TODO: 未完待续...