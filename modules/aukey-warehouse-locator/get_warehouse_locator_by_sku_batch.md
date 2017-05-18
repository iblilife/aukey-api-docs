### 概述

根据`SKU Code`获取该仓库下该SKU已关联的库位信息。

###  接口详细
#### URL
```text
GET     /warehouse/locator/getDesWithBatchSKU
```

#### Parameters
带`*`为必填字段, `删除线`为废弃字段。

| NAME      |  TYPE  | DESCRIPTION                        |
|:----------|:------:|:-----------------------------------|
| skuCodes* | string | SKU Code, 多个使用英文逗号`,`进行分割 |

#### Response JSON
```json
{
    "success": true,
    "message": null,
	"data":{
		"SKU_CODE_1": "LOCATOR_CODE_1",
		"SKU_CODE_2": "LOCATOR_CODE_2",
		"SKU_CODE_3": "LOCATOR_CODE_3",
		"SKU_CODE_4": null
		//... ...
	}
}
```

`ROOT -> data` `key`为SKU Code, `value`为所对应库位编码，如果SKU Code没有查询到对应的库位那么所对应`value`值为`null`。

