##   概述

> 消息被消费后需要进行ACK确认，确认后消息会在消息服务中间件移除，未确认消息将会被重试（重新发送到消息队列），到达一定次数（3次）依然没有被ACK将会被标记死亡消息。

##   接口描述


**url**

```text
GET    /msg/ack
```

**参数列表**

| 参数名称   |    必要     | 样例                                  | 说明          |
|:----------|:----------:|:-------------------------------------|:-------------|
| msgId     | 是  String | 01bb4b5e-d64b-430f-a685-466b7f179fc5 | ACK消息消息ID |
| appId     | 是  String |                                      | 模块ID        |
| timestamp | 是  String | 1492161281                           | 时间戳        |
| sign      | 是  String |                                      | 签名          |

**数据签名计算**

> 将所有参数已key=value的形式通过字符‘&’进行拼接，sign不参与，key以自然排序进行排序。 <br />
> 最后拼接上secret进行MD5得到的值转换大写

**签名代码示例-JAVA**
```java
    String secret = getSecret();

    Map<String, String> args = new HashMap<>();
    args.put("appId", appId);
    args.put("msgId", msgId);
    args.put("timestamp", timestamp);

    String params = args.entrySet().stream()
        .sorted(Comparator.comparing(Map.Entry::getKey))
        .map(e -> e.getKey() + "=" + e.getValue())
        .collect(Collectors.joining("&"));
    
    String sign = DigestUtils.md5Hex( (params + secret).getBytes("UTF-8") ).toUpperCase();
```

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