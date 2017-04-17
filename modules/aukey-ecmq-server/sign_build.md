##  概述

> 接口调用签名生成

##  详细

**appId与secret数据**

appId表示一个模块或者第三方，在消息服务模块进行注册，同时生成对应的secret数据。

**数据签名计算**

> 将所有请求参数以key=value的形式通过字符‘&’进行拼接，`sign参数不参与拼接,值为null的参数不参与拼接`，key以自然排序进行排序(从小到大)。 <br />
> 最后拼接上secret进行MD5得到的值`转换大写`

**签名代码示例-JAVA**
```java
    String secret = getSecret();
    
    //收集所有参数
    Map<String, String> args = new HashMap<>();
    args.put("appId", appId);
    args.put("msgId", msgId);
    args.put("timestamp", timestamp);

    //按照key1=val&key2=val...的形式拼接字符串
    String params = args.entrySet().stream()
        .filter(e -> e.getValue() != null)
        .sorted(Comparator.comparing(Map.Entry::getKey))
        .map(e -> e.getKey() + "=" + e.getValue())
        .collect(Collectors.joining("&"));
    
    //拼接secret，md5hex获取签名，注意转换大写
    String sign = DigestUtils.md5Hex( (params + secret).getBytes("UTF-8") ).toUpperCase();
```