### 概述

> 仓库库位基础数据管理

**项目svn地址**
```text
https://svn.aukeyit.com/svn/Code/aukey-warehouse-locator/trunk
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
| CREATER_USER_ID     |     int     | 创建人用户ID                                                         |
| CREATED_STAMP       |  datetime   | 创建时间                                                             |
| LAST_EDITOR_USER_ID |     int     | 最后修改人用户ID                                                      |
| LAST_UPDATED_STAMP  |  datetime   | 最后修改时间                                                          |


**sql**

```sql
  CREATE TABLE `warehouse_locator` (
      `LOCATOR_ID`  int NOT NULL AUTO_INCREMENT ,
      `LOCATOR_CODE`  varchar(50) NULL ,
      `LOCATOR_TYPE_ID`  varchar(50) NULL ,
      `WAREHOUSE_ID`  int NULL ,
      `REMOVED`  char(1) NULL ,
      `CREATER_USER_ID`  int NULL ,
      `CREATED_STAMP`  datetime NULL ,
      `LAST_EDITOR_USER_ID`  int NULL ,
      `LAST_UPDATED_STAMP`  datetime NULL ON UPDATE CURRENT_TIMESTAMP ,
      PRIMARY KEY (`LOCATOR_ID`)
  );
```