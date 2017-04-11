##   概述

> 通过采购单号获取该采购单号下的开票记录信息。

##   接口描述


**url**

```text
GET    /supply_invoice_access_api/invoice_history
```


**参数列表**

| 参数名称           |      必要       | 样例            | 说明                   |
|:------------------|:--------------:|:---------------|:----------------------|
| purchase_order_id | 是<br/> String | ACG17031368161 | 采购订单单号，必填参数。 |


**响应结果**

```json
{
	"success": true,
	"code": null,
	"message": "",
	"data": [
	    {
            "status_id": "FINISHED_INVOICED",
            "date": "2017-04-13 16:07:19",
            "invoice_number": "543434345321545",
            "invoice_amount": 100,
            "entered_amount": 1000,
            "wait_enter_amount": 0
        },{
			"status_id": "PART_INVOICED",
            "date": "2017-04-12 16:07:19",
            "invoice_number": "546465465321545",
            "invoice_amount": 100,
            "entered_amount": 1000,
            "wait_enter_amount": 1200
		},
		/*
		  ... ...
		*/
		{
			"status_id": "WAITING_INVOICED",
            "date": "2017-04-11 16:07:19"
		}
	]
}
```

**响应结果DATA项数据说明**

> 数据DATA项信息按照开票时间`date`进行降序排列 <br />
> 采购订单号无开票记录DATA项为空数组`data:[]`

| 字段名称           | 类型    | 说明                                                                                                             |
|:------------------|:-------|:----------------------------------------------------------------------------------------------------------------|
| status_id         | String | 采购单开票状态：<br /> `WAITING_INVOICED` 未开票 <br /> `PART_INVOICED` 部分开票 <br /> `FINISHED_INVOICED` 全部开票 |
| date              | String | 开票日期，`yyyy-MM-dd HH:mm:ss` 格式                                                                              |
| invoice_number    | String | 发票号                                                                                                           |
| invoice_amount    | Number | 发票金额                                                                                                         |
| entered_amount    | Number | 已开票金额                                                                                                       |
| wait_enter_amount | Number | 剩余未开票金额                                                                                                    |

