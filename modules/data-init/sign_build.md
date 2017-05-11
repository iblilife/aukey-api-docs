####  概述

接口调用数据签名生成

####  详细

> __sign计算__

> 将所有请求参数以key=value的形式通过字符‘&’进行拼接，`sign参数不参与拼接,值为null的参数不参与拼接`，key以自然排序进行排序(从小到大)。 <br />
> 最后拼接上密钥（盐值）`secret`进行MD5（32位）得到的值`转换大写`即为sign参数的值。<br />
> 密钥（盐值）`secret`获取：开发约定，此处不提及。

__签名计算代码示例-JAVA__
```java
    //填写约定的secret
    String secret = "xxxxxxxxxxxxxxx";
    
    //收集所有参数
    Map<String, String> args = new HashMap<>();
    args.put("params1", val1);
    args.put("params2", val2);
    //......
    args.put("timestamp", timestamp);
    
    //按照key1=val&key2=val...的形式拼接字符串
    String params = args.entrySet().stream()
        .filter(e -> e.getValue() != null)      //值为null不参与拼接
        .sorted(Comparator.comparing(Map.Entry::getKey))
        .map(e -> e.getKey() + "=" + e.getValue())
        .collect(Collectors.joining("&"));
    
    //拼接secret，md5获取签名，注意转换大写
    String sign = DigestUtils.md5Hex( (params + secret).getBytes("UTF-8") ).toUpperCase();
    
    //打印sign
    //202CB962AC59075B964B07152D234B70
```

__签名不正确响应结果JSON__
```json
{
    "success": false,
    "message": "sign数据签名错误！"
}
```