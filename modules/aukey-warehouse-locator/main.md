### 概述

> 仓库库位基础数据管理

**项目svn地址**
```text
https://svn.aukeyit.com/svn/Code/aukey-warehouse-locator/trunk
```

**测试环境地址**
```text
http://warehouse-locator.qa.aukeyit.com
```

**测试环境数据库**
```text
//数据库
warehouse_locator

```

### 表设计

**warehouse_locator** 仓库库位表

| 字段名称             |     类型     | 说明                                                                 |
|:--------------------|:-----------:|:--------------------------------------------------------------------|
| LOCATOR_ID          |     int     | 标识                                                                 |
| LOCATOR_CODE        | varchar(50) | 库位编码                                                             |
| LOCATOR_TYPE_ID     | varchar(50) | 库位类型动态/静态，可选值`DYNAMICALLY`或`STATICALLY`，默认`DYNAMICALLY` |
| WAREHOUSE_ID        |     int     | 仓库ID                                                               |
| REMOVED             |   char(1)   | 是否已删除，可选值`Y`或`N`, 默认`N`                                    |
| CREATOR_USER_ID     |     int     | 创建人用户ID                                                         |
| CREATED_STAMP       |  datetime   | 创建时间                                                             |
| LAST_EDITOR_USER_ID |     int     | 最后修改人用户ID                                                      |
| LAST_UPDATED_STAMP  |  datetime   | 最后修改时间                                                          |

**warehouse_locator_sku_assoc** 仓库库位与SKU关联关系表

| 字段名称                |     类型     | 说明                                       |
|:-----------------------|:-----------:|:------------------------------------------|
| ASSOC_ID               |     int     | 标识                                       |
| LOCATOR_ID             |     int     | 库位ID                                     |
| SKU_CODE               | varchar(50) | SKU Code                                  |
| FROM_DATE_STAMP        |  datetime   | 起始时间                                   |
| THRU_DATE_STAMP        |  datetime   | 截止时间                                   |
| STATUS_ID              | varchar(50) | 状态，可选值`CREATED`已创建、`REMOVED`已删除 |
| CREATOR_USER_ID        |     int     | 创建人USER ID                              |
| CREATE_DATE_STAMP      |  datetime   | 创建时间                                   |
| LAST_UPDATE_DATA_STAMP |  datetime   | 最后修改时间                                |
