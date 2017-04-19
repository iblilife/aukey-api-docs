## 概述

> 获取查询条件仓库下拉框数据

**下拉框要求有`分组效果`如下**

仓库：<select>
  <optgroup label="样品仓">
    <option selected>Amazon美国仓</option>
    <option>HK-4PX澳洲仓-903827</option>
    <option>HK-4PX德国3仓-903827</option>
  </optgroup>
  <optgroup label="不良品仓">
    <option>HK-4PX加拿大仓-9038274</option>
    <option>HK-4PX加拿大仓-9038275</option>
    <option>HK-4PX加拿大仓-9038276</option>
  </optgroup>
  <optgroup label="保税仓">
    <option>HK-4PX加拿大仓-9038277</option>
    <option>HK-4PX加拿大仓-9038278</option>
    <option>HK-4PX加拿大仓-9038279</option>
  </optgroup>
</select>

```html
<select>
  <optgroup label="样品仓">
    <option selected>Amazon美国仓</option>
    <option>HK-4PX澳洲仓-903827</option>
    <option>HK-4PX德国3仓-903827</option>
  </optgroup>
  <optgroup label="不良品仓">
    <option>HK-4PX加拿大仓-9038274</option>
    <option>HK-4PX加拿大仓-9038275</option>
    <option>HK-4PX加拿大仓-9038276</option>
  </optgroup>
  <optgroup label="保税仓">
    <option>HK-4PX加拿大仓-9038277</option>
    <option>HK-4PX加拿大仓-9038278</option>
    <option>HK-4PX加拿大仓-9038279</option>
  </optgroup>
</select>
```
> **`以上为HTML举例代码，实际效果请以现在项目风格为准。`**

##   接口详细

**接口url**

```text
GET     /warehouse/data/warehouseGroupByTypeList
```

**参数列表**

_无_

**响应结果JSON**

```json
    {
        "success": true,
        "message": null,
        "data": [
            {
                "warehouseTypeId": "7",
                "warehouseTypeName": "样品仓",
                "warehouses":[
                    {
                        "warehouseId": 31,
                        "warehouseName": "Amazon美国仓"
                    },{
                        "warehouseId": 32,
                        "warehouseName": "HK-4PX澳洲仓-903827"
                    },{
                         "warehouseId": 33,
                         "warehouseName": "HK-4PX德国3仓-903827"
                    }
                    //... ...
                ]
            },{
                "warehouseTypeId": "8",
                "warehouseTypeName": "不良品仓",
                "warehouses":[
                  {
                      "warehouseId": 313,
                      "warehouseName": "xxxxx1"
                  },{
                      "warehouseId": 323,
                      "warehouseName": "xxxxx2"
                  },{
                       "warehouseId": 333,
                       "warehouseName": "xxxxx3"
                  }
                  //... ...
                ]
            }
            //... ...
        ]
    }
```

**响应结果JSON字段说明**

_`json root`_ -> `data` 数组项字段说明

| 字段名称           | 说明              |
|:------------------|:-----------------|
| warehouseTypeId   | 仓库类型ID        |
| warehouseTypeName | 仓库类型名称       |
| warehouses        | 仓库类型下仓库列表 |

_`json root`_ -> `data` ->  `warehouses` 数组项字段说明

| 字段名称       | 说明     |
|:--------------|:--------|
| warehouseId   | 仓库ID   |
| warehouseName | 仓库名称 |
