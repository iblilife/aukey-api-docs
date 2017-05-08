#### 概述
通过sku，店铺ID，站点ID获取seller SKU和店铺下title。

__接口url__
```text
GET     /amazon/listing/basic_info
```

__参数列表__

| 参数名称    |      必要       | 说明   |
|:-----------|:--------------:|:------|
| sku        | 是<br/> String | SKU   |
| account_id | 是<br/> Number | 店铺ID |
| site_id    | 是<br/> Number | 站点ID |

**响应结果JSON**
```json
{
    "success": true,
    "message": null,
    "data": [
      {
        "sellerSku": "123123",
        "title": "Sony PCGA-AC16V6 Notebook Replacement 16V 4A Black..."
      },{
        "sellerSku": "123123",
        "title": "Sony PCGA-AC16V6 Notebook Replacement 16V 4A Black..."
      }
      //......
    ]
}
```
`success` 字段为业务操作是否成功，`true`表示成功，`false` 表示失败。 <br />
`message` 字段表示失败原因。