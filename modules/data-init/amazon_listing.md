#### 概述
佰易系统同步E登亚马逊listing相关数据, 功能位于`产品中心模块(aukey-product)`。
首次为佰易系统调用E登数据库同步所有基础数据，后续E登系统数据变动需要调用佰易系统接口进行数据同步。

#### 佰易产品中心表设计（MYSQL）
```sql
  -- ----------------------------
  -- Table structure for amazon_listing
  -- ----------------------------
  CREATE TABLE `amazon_listing` (
    `ID` int(11) NOT NULL,
    `ACCOUNT_ID` int(11) DEFAULT NULL,
    `SITE_ID` int(11) DEFAULT NULL,
    `SKU` varchar(255) DEFAULT NULL,
    `SELLER_SKU` varchar(255) DEFAULT NULL,
    `SITE_SKU_TITLE` text,
    `ASIN` varchar(255) DEFAULT NULL,
    `PRICE` decimal(18,6) DEFAULT NULL,
    `CURRENCY` varchar(50) DEFAULT NULL,
    `QUANTITY` decimal(10,0) DEFAULT NULL,
    `ASIN_MIN_PRICE` decimal(18,6) DEFAULT NULL,
    `ASIN_MIN_PRICE_FLAG` char(1) DEFAULT NULL,
    `ASIN_SECOND_MIN_PRICE` decimal(18,6) DEFAULT NULL,
    `ASIN_THIRD_MIN_PRICE` decimal(18,6) DEFAULT NULL,
    `ASIN_PRICE_RANKING_LIST` int(11) DEFAULT NULL,
    `ASIN_SAME_PRICE_COUNT` int(11) DEFAULT NULL,
    `ASIN_SELLER_COUNT` int(11) DEFAULT NULL,
    `FOLLOW_LISTING_FLAG` char(1) DEFAULT NULL,
    `STOP_SALE_FLAG` char(1) DEFAULT NULL,
    `STOP_SALE_TIME_STAMP` datetime DEFAULT NULL,
    `SAME_CODE_LISTING_COUNT` int(11) DEFAULT NULL,
    `SHIPPING_AMOUNT` decimal(18,6) DEFAULT NULL,
    `MARK_DOWN_PRICE` decimal(18,6) DEFAULT NULL,
    `OPEN_TIME_STAMP` datetime DEFAULT NULL,
    `AMAZON_STATUS` varchar(50) DEFAULT NULL,
    `CREATED_TIME_STAMP` datetime DEFAULT NULL,
    `LAST_UPDATED_TIME_STAMP` datetime DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP,
    PRIMARY KEY (`ID`)
  )
```

#### 佰易表与E登表字段对应参考

> 佰易系统产品模块表`product_ms.amazon_listing`与E登系统表`AukeysADS.dbo.AmazonSellingList`对应

| amazon_listing表字段     | AmazonSellingList表字段 | 说明 |
|:------------------------|:-----------------------|:-----|
| ID                      | AmazonSellingListID    |      |
| ACCOUNT_ID              | AccountID              |      |
| SITE_ID                 | SiteInfoID             |      |
| SKU                     | Sku                    |      |
| SELLER_SKU              | MarketSku              |      |
| SITE_SKU_TITLE          | ForeignName            |      |
| ASIN                    | Asin                   |      |
| PRICE                   | Price                  |      |
| CURRENCY                | Currency               |      |
| QUANTITY                | Quantity               |      |
| ASIN_MIN_PRICE          | ASINMinPrice           |      |
| ASIN_MIN_PRICE_FLAG     | IsASINMinPrice         |      |
| ASIN_SECOND_MIN_PRICE   | ASINSencodMinPrice     |      |
| ASIN_THIRD_MIN_PRICE    | ASINThirdMinPrice      |      |
| ASIN_PRICE_RANKING_LIST | ASINPriceRankingList   |      |
| ASIN_SAME_PRICE_COUNT   | ASINSamePriceCount     |      |
| ASIN_SELLER_COUNT       | ASINSellerCount        |      |
| FOLLOW_LISTING_FLAG     | IsFollowListing        |      |
| STOP_SALE_FLAG          | IsStopSale             |      |
| STOP_SALE_TIME_STAMP    | StopSaleTime           |      |
| SAME_CODE_LISTING_COUNT | SameCodeListingCount   |      |
| SHIPPING_AMOUNT         | ShippingAmount         |      |
| MARK_DOWN_PRICE         | MarkDownPrice          |      |
| OPEN_TIME_STAMP         | OpenDate               |      |
| AMAZON_STATUS           | AmazonStatus           | -    |


#### E登listing数据变动调用佰易接口

__url__

```text
POST     http://product.qa.aukeyit.com/amazon/listing/sync
```

__参数列表__

| 参数                                                                                                             | 必要 | 详细说明                                                                                                                                                      |
|:-----------------------------------------------------------------------------------------------------------------|:-----|:-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [佰易表与E登表字段对应参考](/modules/data-init/amazon_listing?id=佰易表与E登表字段对应参考)中所有`amazon_listing表字段` |      | 数据参数与[佰易表与E登表字段对应参考](/modules/data-init/amazon_listing?id=佰易表与E登表字段对应参考)中一致，请求参数名称为对应`amazon_listing表字段`，当中`ID`为必填项。 |
| sign                                                                                                             | 是   | 数据签名                                                                                                                                                      |
| timestamp                                                                                                        | 是   | 当前时间戳(毫秒)                                                                                                                                                    |

> __sign计算__

> 将所有请求参数以key=value的形式通过字符‘&’进行拼接，`sign参数不参与拼接,值为null的参数不参与拼接`，key以自然排序进行排序(从小到大)。 <br />
> 最后拼接上密钥（盐值）`secret`进行MD5得到的值`转换大写`即为sign参数的值。<br />
> 密钥（盐值）`secret`获取：开发约定，此处不提及。

__签名计算代码示例-JAVA__
```java
    String secret = getSecret();
    
    //收集所有参数
    Map<String, String> args = new HashMap<>();
    args.put("ID", id);
    args.put("ACCOUNT_ID", accountId);
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
```

__响应成功结果JSON__
```json
    {
        "success": true,
        "message": null
    }
```
`success` 字段为业务操作是否成功，`true`表示成功，`false` 表示失败。 <br />
`message` 字段表示失败原因。