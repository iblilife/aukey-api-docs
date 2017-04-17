##   概述
> 消息创建接口，创建消息并投递消息队列交由消费端。

##   接口描述

**url**

```text
POST    /msg/create
```

**参数列表**

| 参数名称       |    必要     | 样例                             | 说明                                                                                             |
|:--------------|:----------:|:--------------------------------|:-------------------------------------------------------------------------------------------------|
| appId         | 是  String |                                 | 模块ID                                                                                           |
| timestamp     | 是  String | 1492161281                      | 时间戳                                                                                           |
| sign          | 是  String |                                 | 签名                                                                                             |
| routingId     |   String   | 1                               | 路由ID, 如果appId对应类型为第三方应用`THIRD_PARTY`, 则该字段为必填，需要联系管理人员在消费服务管理后端添加 |
| exchangeId    |   String   | DEFAULT_EXCHANGE                | 队列交换机ID，如果appId对应类型为内部应用`PARTY`, 则该字段为必填, `exchangeId`,`routingKey`决定消息投递的队列 |
| routingKey    |   String   | AK.ECMQSERVER.TEST              | 路由KEY, 如果appId对应类型为内部应用`PARTY`, 则该字段为必填, `exchangeId`,`routingKey`决定消息投递的队列  |
| msgDataType   | 是  String | JSON                            | 消息体数据类型, 可选值：`JSON` `XML` 暂只支持`JSON`                                                 |
| msgBody       | 是  String | {"name"："iblilife", "age": 88}  | 消息提数据, JSON字符串                                                                             |
| remark        | 否  String |                                 | 备注                                                                                             |
| extraField1   | 否  String |                                 | 预留扩展字段1                                                                                     |
| extraField2   | 否  String |                                 | 预留扩展字段2                                                                                     |
| extraField3   | 否  String |                                 | 预留扩展字段3                                                                                     |

> [appId获取，sign如何计算？](modules/aukey-ecmq-server/sign_build)

**成功响应结果**

```json
    {
        "success": true,
        "message": null,
        "data": "01bb4b5e-d64b-430f-a685-466b7f179fc5"
    }
```
`data`数据为msgId（消息ID）

**错误响应结果**

```json
    {
        "success": false,
        "message": "未授权，appId[adfadfdas]。"
    }
```