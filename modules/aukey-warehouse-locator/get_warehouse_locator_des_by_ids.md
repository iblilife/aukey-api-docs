## 概述

> 根据库位ID获取详细信息, 可批量ID通过字符英文逗号(`,`)隔开。

##   接口详细

**接口url**

```text
GET     /warehouse/locator/getDesWithIds
```

**参数列表**

| 参数名称    |      必要       | 样例         | 说明                                     |
|:-----------|:--------------:|:------------|:----------------------------------------|
| locatorIds | 是<br/> String | 11,22,33,44 | 库位ID, 可批量ID通过字符英文逗号(`,`)隔开。 |

**响应结果JSON**

```json
    {
        "success": true,
        "message": null,
        "data": [
            {
                "locatorId": 1,
                "locatorCode": "00-200-99",
                "locatorTypeId": "DYNAMICALLY",
                "warehouseId": 11,
                "creatorUserId": 111,
                "createdStamp": "2017-04-18 12:06:19",
                "lastEditorUserId": 122,
                "lastUpdatedStamp": "2017-04-18 12:07:05"
            },{
                "locatorId": 2,
                "locatorCode": "22-200-99",
                "locatorTypeId": "DYNAMICALLY",
                "warehouseId": 22,
                "creatorUserId": 222,
                "createdStamp": "2017-04-18 12:06:19",
                "lastEditorUserId": 2223,
                "lastUpdatedStamp": "2017-04-18 12:07:05"
            }
            //... ...
        ]
    }
```

**响应结果JSON`data`项字段说明**

| 字段名称          | 说明                                                  |
|:-----------------|:-----------------------------------------------------|
| locatorId        | 库位ID，唯一                                          |
| locatorCode      | 库位编码                                              |
| locatorTypeId    | 库位类型，可选值`DYNAMICALLY`(动态)或`STATICALLY`(静态) |
| warehouseId      | 仓库ID                                                |
| creatorUserId    | 创建人UserId                                          |
| createdStamp     | 创建时间，格式：`yyyy-MM-dd HH:mm:ss`                  |
| lastEditorUserId | 最后修改人UserId                                      |
| lastUpdatedStamp | 最后修改时间，格式：`yyyy-MM-dd HH:mm:ss`               |