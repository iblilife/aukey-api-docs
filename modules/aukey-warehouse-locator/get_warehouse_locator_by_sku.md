### 概述

根据`仓库ID`与`SKU Code`获取该仓库下该SKU已关联的库位信息，如果没有关联信息则返回该仓库下所有可用库位。

###  接口详细
#### URL
```text
GET     /warehouse/locator/getAssocWithSKU
```

#### Parameters
带`*`为必填字段, `删除线`为废弃字段。

| NAME        |  TYPE  | DESCRIPTION |
|:------------|:------:|:------------|
| skuCode*     | string | SKU编码      |
| warehouseId* | number | 仓库ID       |

#### Response JSON
_存在已关联的库位_
```json
{
    "success": true,
    "message": null,
	"data":{
		"existsAssoc": true,
		"warehouseLocator":{
			"locatorId": 1000,
			"locatorCode": "G-2-2",
			"locatorTypeId": "STATICALLY",
			"warehouseId": 111
		},
		"warehouseLocators": null
	}
}
```
_不存在已关联的库位_
```json
{
    "success": true,
    "message": null,
	"data":{
		"existsAssoc": false,
		"warehouseLocator": null,
		"warehouseLocators": [
			{
				"locatorId": 1000,
				"locatorCode": "G-2-2",
				"locatorTypeId": "STATICALLY",
				"warehouseId": 111
			},{
				"locatorId": 1001,
				"locatorCode": "G-2-3",
				"locatorTypeId": "STATICALLY",
				"warehouseId": 111
			}
			//......
		]
	}
}
```

`ROOT -> data` 字段说明

| NAME              |  TYPE   | DESCRIPTION                                                       |
|:------------------|:-------:|:------------------------------------------------------------------|
| existsAssoc       | boolean | 是否存在关联关系，存在：`true` 不存在：`false`                        |
| warehouseLocator  | object  | 关联的库位信息，字段说明见[附录-1](#附录-1)                           |
| warehouseLocators |  array  | 不存在关联关系返回该仓库下所有库位信息，数据项字段说明见[附录-1](#附录-1) |

#### 附录-1
| NAME          |  TYPE  | DESCRIPTION                                         |
|:--------------|:------:|:----------------------------------------------------|
| locatorId     | number | 库位ID                                               |
| locatorCode   | string | 库位编码                                             |
| locatorTypeId | string | 库位类型，动态/静态，可选值`DYNAMICALLY`或`STATICALLY`  |
| warehouseId   | number | 仓库ID                                               |
