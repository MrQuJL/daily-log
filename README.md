# work-journey

## 记录工作中遇到的一些bug，学习到的一些经验，以及新的技术

1. 不要轻易删除数据库中数据，与该表相关联的业务极有可能收到影响，例：某程序员误删了用户表中某用户的记录，但在订单表中仍然存储着该用户的订单记录，导致查询订单时，从订单表中获取用户的详细信息时报空指针异常。

2. 不要为了写优雅的代码轻易使用极易出现空指针的链式调用写法，导致后台报空指针时排查困难。

    反例：

    ```
    order.setMsg(userMap.get(order.userId()).getUserInfo());
    ```

    若从userMap中获取了null，则调用getUserInfo方法时就会报空指针异常，此类小错看起来可笑，但是却很常见。

3. 在实现一个需求的时候，需要事先与前台约定好数据的格式，不要一意孤行。

4. 重写Comparator的compare方法时，第一个值大于第二个值，返回1表示升序，-1降序；第一个值小于第二个值，返回1表示降序，返回-1表示升序。

5. 慎用复制粘贴：

    反例：

    ```
    int s1 = o1.getShopCount().intValue();
    int s2 = o1.getShopCount().intValue();
    ```

    这种错误IDE不会报错，很难发现。

6. 在向session中缓存数据的时候，对应的实体类必须实现Serializable接口，否则会导致序列化失败

7. 查询商品列表的同时计算每件商品的昨日销量，过去7天销量，过去30天销量需要频繁查询订单明细表中的数据，造成性能低下。

    > 解决：一次性将过去30天内已经付款的订单加载到内存，在内存里计算。（此种方式仅限于订单量较小的情况）

8. 使用```@GeneratedValue```注解的字段，必须是自增长的字段，比如int，如果是varchar类型，不要使用该注解，否则在用jpa的save方法保存对象的时候会报错：

    Filed id dosen't have a default value

9. 使用Java8的parallelStream（并行流）可以实现并行化流操作，但是要想达到最大的效果，流中元素的个数必须很大，CPU的核心数必须很多。

10. 在使用Java8的StreamAPI来进行编码时可以使用 ```peek``` 方法来进行断点调试，例如在下方代码的 ```peek``` 那一行点上断点，然后以```debug``` 模式运行就可以断点调试，也建议使用下面那样的调用方式（调用的方法排成一列，这样就方便断点调试）：

    ```java
    public class LambdaPeek {

        public static void main(String[] args) {
            List<String> list = Stream.of("ABC", "DEF", "Ghi")
                                      .peek(e -> System.out.println(e.toLowerCase()))
                                      .collect(Collectors.toList());
        }
    }
    ```

11. 什么是web1.0，web2.0，web3.0？

    web1.0：Yahoo，AOL，新浪，搜狐等。**关注点是是网络的连接**。
    web2.0：博客，互动论坛，今日头条，拼多多，大众点评，社交网络等。**关注点是信息的互动**。
    web3.0：Steemit ，趣头条，区块链等。**关注点是利益共享，激励**。

    Web 1.0是**B2C**模式，由商家直接提供服务，是单向的行为，受中心化控制。

    Web 2.0则是**C2B**，中心相对变弱，用户的影响力增强，平台规模急速扩大，用户受制于平台控制。

    Web 3.0相当于**C2C**，在这里，每一个C都可以更加顺畅、更加简单、更低成本地跟其他C产生交易，完全不依赖于平台，不受平台掌控，共同分享整个平台的收益。 

12. Navicat中表的记录一页只显示1000条，多出的记录在下一页，但是不会主动显示有第二页，坑。

13. @Transactional注解需要放到所有注解的最上面才起作用。

14. jpa的Repository接口的抽象方法上也是可以添加@Transactional注解的





