### 概要

### 原型图
![原型图](modules/aukey-supply-chain-invoice/imgs/原型图_20170526191153.png)

### 涉及表

*   `purchase_invoice` 采购入库单记录表.
*   `invoice_record` 录入发票发票记录表.
*   `invoice_assoc` 采购入库单记录 与 录入发票发票记录 关联关系表, 多对多关系.
*   `purchase_sku_invoice_record` 发票与采购单SKU 数量分配对应关系表(分配数量字段`invoice_num`), 多对多关系.
*   `invoice_tax_collect` 总发票明细记录表, 一个发票号一条记录, 与`invoice_record`表通过发票号一对一关系.
*   `invoice_declaration_detail` 采购单SKU申报记录详情表.
*   `declaration_number_invoice_no_assoc` 采购单SKU申报记录详情表 与 发票号 数量分配对应关系表, 多对多关系.

### 流程

1.  仓库模块进行采购入库操作, 生成`purchase_invoice`采购入库单记录表数据.
2.  发票模块进行录入发票
    -   生成`invoice_record`录入发票发票记录.
    -   生成`invoice_assoc`采购入库单记录 与 录入发票发票记录 关联关系.
    -   生成`invoice_tax_collect`总发票明细记录.
    -   生成`purchase_sku_invoice_record`发票与采购单SKU 数量分配对应关系记录.
3.  海关申报模块进行申报, 生成 `invoice_declaration_detail`采购单SKU申报记录详情记录.
4.  如果进行`3步骤`时候根据采购单号与SKU已经存在对应的`purchase_sku_invoice_record`记录并且`invoice_tax_collect`发票总明细表中发票为`审核同意`状态, 则需要生成 `declaration_number_invoice_no_assoc`采购单SKU申报记录与发票号数量分配对应关系记录.



### 总发票明细功能点分析

#### 审核同意
`invoice_tax_collect`字段`review_status_id`为`WAITING`待审核才能进行该操作.

1.  `invoice_tax_collect`字段`review_status_id`设置为`APPROVED`
2.  根据发票号查询`purchase_sku_invoice_record`获取采购单号与SKU, 检查`invoice_declaration_detail`是否存在采购单SKU申报记录, 有则生成 `declaration_number_invoice_no_assoc`采购单SKU申报记录与发票号数量分配对应关系记录.

#### 审核拒绝
`invoice_tax_collect`字段`review_status_id`为`WAITING`待审核才能进行该操作.

1. `invoice_tax_collect`字段`review_status_id`设置为`REJECTED.`
2. `invoice_record`表标记发票为已删除, 修改字段`del_type`为'Y'.
3. 标记发票所匹配的所有`invoice_assoc`采购入库单记录 与 录入发票发票记录 关联关系为已删除, 修改字段`del_type`为'Y'.
4. 删除(物理删除)`purchase_sku_invoice_record`发票与采购单SKU 数量分配对应关系数据, 根据所匹配的数量回滚`purchase_invoice`表中可开发票数量`max_invoice_number`以及修改状态`status`.

#### 反审核

审核同意的逆向操作.

#### 导出模版
![导出模版](modules/aukey-supply-chain-invoice/imgs/原型图-导出Excel_20170526191153.png)
