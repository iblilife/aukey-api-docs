##   概述

> 消息被消费后需要进行ACK确认，确认后消息会在消息服务中间件移除，未确认消息将会被重试（重新发送到消息队列），到达一定次数（3次）依然没有被ACK将会被标记死亡消息。

> `由于同一消息可能会被多次推送到消息队列，消费端会重复收到相同操作消息，请保证消费端业务接口的幂等性!`

##   接口描述

**url**

```text
POST    /msg/ack
```

**参数列表**

| 参数名称   |    必要     | 样例                                  | 说明          |
|:----------|:----------:|:-------------------------------------|:-------------|
| msgId     | 是  String | 01bb4b5e-d64b-430f-a685-466b7f179fc5 | ACK消息消息ID |
| appId     | 是  String |                                      | 模块ID        |
| timestamp | 是  String | 1492161281                           | 时间戳        |
| sign      | 是  String |                                      | 签名          |

> [appId获取，sign如何计算？](modules/aukey-ecmq-server/sign_build)

**错误响应结果**

```json
    {
        "success": false,
        "message": "未授权，appId[adfadfdas]。"
    }
```

**成功响应结果**
```json
    {
        "success": true,
        "message": null
    }
```