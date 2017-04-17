##   概述
> 预创建消息需要结合确认消息进行使用。进行业务操作前调用消息服务预创建消息，业务操作执行成功后调用确认消息。消息确认后会被投递到消息队列，消息消费端进行消费。
> 预创建消息如果超时没有被确认，消息服务会根据预创建消息方提供的超时回调URL检查业务是否执行成功，从而决定是投递消息还是丢弃消息。

> 预创建消息使用场景：假设有A，B，C三个微服务，A中有一个业务操作调用了B的接口并且调用完B成功后需要通过消息服务投递消息到C执行相关的业务操作。
> `为社么需要预创建再确认两步, 直接一步创建消息并确认完事了么？` 假如是一步操作，如果在最后调用消息服务投递消息的时候失败而这时调用B已经成功，消息没有被投递C服务业务操作无法被执行，这并不是我们想要的结果。

> 如果没有如上所述请使用创建即投递消息接口[创建消息](modules/aukey-ecmq-server/create_message)。

##   接口描述

**url**

```text
POST    /msg/pre_create
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
| msgConfirmUrl | 是  String | http://sample.com/callback      | 消息确认超时回调URL                                                                                |
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