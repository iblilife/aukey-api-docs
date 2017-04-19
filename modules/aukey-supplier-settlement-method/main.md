### 概述

> 供应商结算方式基础数据管理

**项目svn地址**
```text
https://svn.aukeyit.com/svn/Code/aukey-supplier-settlement-method/trunk
```

**测试环境数据库**
```text
//数据库
supplier2
```

### 表设计

**base_payment_method** 供应商结算方式表。

> `该表在开发该模块已经存在，设计上存在问题（命名规则混乱，账期类型字段无用，字段类型错误）。`
> `由于现有功能使用较多未作优化调整。`

| 字段名称                      |     类型      | 说明                     |
|:-----------------------------|:-------------:|:-------------------------|
| payment_method_id            |      int      | 标识                     |
| method                       | varchar(200)  | 结算方式简短描述（名称）    |
| prepay                       |  varchar(20)  | 预付                     |
| accountPeriod                |  varchar(20)  | 账期                     |
| ~~paymentDay~~ `弃用`        |     enum      | 账期类型                  |
| payment_duration_days `新增` |      int      | 付款期间（天）            |
| removed `新增`               |    char(1)    | 是否已删除，可选值`Y`或`N` |
| create_user                  |      int      | 创建人用户ID              |
| create_time                  |   datetime    | 创建时间                  |
| update_user                  |      int      | 最后修改人用户ID          |
| update_time                  |   datetime    | 最后修改时间              |
