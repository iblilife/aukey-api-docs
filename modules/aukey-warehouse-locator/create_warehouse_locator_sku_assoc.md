### 概述

创建`SKU`与`仓库库位`关联关系。

###  接口详细

> 已经存在关联关系，重复调用该创建关系接口将直接返回成功。

#### URL
```text
POST     /warehouse/locator/createSKUAssoc
```

#### Parameters
带`*`为必填字段, `删除线`为废弃字段。

| NAME       |  TYPE  | DESCRIPTION |
|:-----------|:------:|:------------|
| skuCode*   | string | SKU编码      |
| locatorId* | number | 库位ID       |

#### Response JSON

```json
    {
        "success": true,
        "message": null
    }
```

`success` 字段为业务操作是否成功，`true`表示成功，`false` 表示失败。 <br />
`message` 字段表示失败原因。 <br />