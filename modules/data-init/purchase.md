#### 概述

佰易涉及表：`requirement`（采购需求表-需求模块）、`purchase_demand`（采购需求表-采购模块）、`purchase_order`（采购订单表）。
新建需求将在`requirement`中插入一条记录即可，新建采购单需要再`purchase_demand`和`purchase_order`同时插入记录。

E登涉及表：`PurchaseOrder`

#### 表结构字段

**requirement** _采购需求表-需求模块_

| 字段                | 描述         | 说明                                                                            |
|:-------------------|:------------|:--------------------------------------------------------------------------------|
| requirement_id     | 主键自增     |                                                                                 |
| requirement_no     | 需求单号     |                                                                                 |
| dept_id            | 需求部门     |                                                                                 |
| sku_code           | sku编码      |                                                                                 |
| category_id        | 品类id       |                                                                                 |
| quantity           | 采购数量     |                                                                                 |
| supplier_id        | 供应商id     |                                                                                 |
| subject_id         | 法人主体id   |                                                                                 |
| remark             | 备注         |                                                                                 |
| warehouse_id       | 目标仓       |                                                                                 |
| enter_warehouse_id | 入库仓       |                                                                                 |
| order_status       | 订单状态     | 0 异常,1 正常                                                                    |
| requirement_status | 需求状态     | 0 未审批,1 指派采购,2 生成订单,3 采购审批,4 产品入库,5 发货计划, 6 需求完结,7 需求拒绝 |
| transport_type     | 运输方式     | 0 空运,1 海运                                                                    |
| create_date        | 单据时间     |                                                                                 |
| update_date        | 单据修改时间  |                                                                                 |
| create_user        | 需求创建人   |                                                                                 |
| update_user        | 需求修改人   |                                                                                 |
| purchase_user      | 采购人员     |                                                                                 |
| data_status        | 数据状态     | 0 已删除,1 未删除,2 待完成                                                        |

**purchase_demand** _采购需求表-采购模块_

| 字段                | 描述                          | 说明                                    |
|:-------------------|:-----------------------------|:---------------------------------------|
| id                 | 主键自增                      |                                        |
| purchase_demand_id | 需求模块-需求表ID              | 引用`requirement`表字段`requirement_id` |
| sku_code           | sku编码                       |                                        |
| quantity           | 数量                          |                                        |
| warehouse_id       | 目标仓id                      |                                        |
| unit_price         | 单价                          |                                        |
| money              | 金额                          |                                        |
| remark             | 备注                          |                                        |
| status             | 需求状态                      |                                        |
| purchase_order_id  | 采购订单编号                   |                                        |
| tax_unit_price     | 含税单价                      |                                        |
| tax_money          | 税额                          |                                        |
| tax_money_total    | 价税合计                      |                                        |
| create_date        | 单据创建时间                   |                                        |
| update_date        | 单据修改时间                   |                                        |
| create_user        | 需求创建人                    |                                        |
| update_user        | 需求修改人                    |                                        |
| data_status        | 数据状态                      | 0 已删除,1 未删除                        |
| tax_rate           | 税率                          |                                        |
| tax_price          | 含税金额（单价乘数量，不是税额） |                                        |
| cal_tax_unit_price | 含税的单价计算(和含税单价不同)   |                                        |
| delivery_date      | 交货日期                      | 　                                     |
