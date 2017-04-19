## 概述

> 用于仓库库位列表数据获取

##   接口详细

**接口url**

```text
GET     /warehouse/locator/list
```

**参数列表**

| 参数名称     |      必要       | 样例       | 说明                  |
|:------------|:--------------:|:----------|:---------------------|
| pageSize    | 是<br/> Number | 50        | 分页每页数据大小       |
| pageIndex   | 是<br/> Number | 1         | 分页当前页页码，从1开始 |
| locatorCode | 否<br/> String | 00-200-99 | 库位编码条件           |
| warehouseId | 否<br/> Number | 111       | 仓库ID条件            |

**响应结果JSON**

```json
{
  "total": 1000,
  "rows": [
    {
      "locatorId": 1,
      "locatorCode": "00-200-99",
      "locatorTypeId": "DYNAMICALLY",
      "warehouseId": 11,
      "warehouseName": "HK-4PX澳洲仓-903827",
      "removed": "N",
      "creatorUserId": 111,
      "createdStamp": "2017-04-18 12:06:19",
      "lastEditorUserId": 122,
      "lastUpdatedStamp": "2017-04-18 12:07:05"
    },{
      "locatorId": 2,
      "locatorCode": "22-200-99",
      "locatorTypeId": "DYNAMICALLY",
      "warehouseId": 22,
      "warehouseName": "Amazon美国仓",
      "removed": "N",
      "creatorUserId": 222,
      "createdStamp": "2017-04-18 12:06:19",
      "lastEditorUserId": 2223,
      "lastUpdatedStamp": "2017-04-18 12:07:05"
    }
    //... ...
  ]
}
```
**响应结果JSON`rows`项字段说明**

| 字段名称          | 说明                                                  |
|:-----------------|:-----------------------------------------------------|
| locatorId        | 库位ID，唯一                                          |
| locatorCode      | 库位编码                                              |
| locatorTypeId    | 库位类型，可选值`DYNAMICALLY`(动态)或`STATICALLY`(静态) |
| warehouseId      | 仓库ID                                                |
| warehouseName    | 仓库名称                                              |
| removed          | 是否已删除，可选值`Y`（是）或`N`（否）                   |
| creatorUserId    | 创建人UserId                                          |
| createdStamp     | 创建时间，格式：`yyyy-MM-dd HH:mm:ss`                  |
| lastEditorUserId | 最后修改人UserId                                      |
| lastUpdatedStamp | 最后修改时间，格式：`yyyy-MM-dd HH:mm:ss`               |
