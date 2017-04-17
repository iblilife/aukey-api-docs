##  概述
> 预创建消息确认适用预创建消息，预创建消息后如果业务操作成功最后调用预创建消息确认进行消息确认，确认成功的消息将被投递到消息队列。确认失败的消息将直接删除。

> 预创建消息如果一直没有被确认将会触发超时回调（默认预创建消息5分钟后没有被确认）

##   接口描述

**url**

```text
POST    /msg/pre_msg/ack
```

**参数列表**

| 参数名称   |    必要     | 样例                                  | 说明                                                                      |
|:----------|:----------:|:-------------------------------------|:-------------------------------------------------------------------------|
| msgId     | 是  String | 01bb4b5e-d64b-430f-a685-466b7f179fc5 | ACK消息消息ID                                                             |
| appId     | 是  String |                                      | 模块ID                                                                    |
| timestamp | 是  String | 1492161281                           | 时间戳                                                                    |
| sign      | 是  String |                                      | 签名                                                                      |
| ack       | 是  String | Y                                    | 确认消息可选值`Y`确认消息成功消息将被投递到消息队列,`N`确认消息失败消息将被丢弃。  |

> [appId获取，sign如何计算？](modules/aukey-ecmq-server/sign_build)

**成功响应结果**

```json
    {
        "success": true,
        "message": null
    }
```