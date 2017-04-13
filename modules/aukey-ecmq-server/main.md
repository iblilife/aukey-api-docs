### 可靠消息服务

> 分布式事务解决方案-可靠消息最终一致性。<br /><br />
> 借助消息队列，在处理业务逻辑的地方发送消息，业务逻辑处理成功后，提交消息，确保消息是发送成功的，之后消息队列投递来进行处理，如果成功，则结束，如果没有成功，则重试，直到成功，不过仅仅适用业务逻辑中，第一阶段成功，第二阶段必须成功的场景。 <br /><br />
> 第二阶段重试可能出现一直不成功情况，其实在大多数的逻辑中，第二阶段失败的概率比较小，所以可以单独独立补偿任务表出来，可以更加清晰，能够比较明确的知道当前多少任务是失败的。

**相关资料**

- [分布式系统常用思想和技术总结](http://blog.hebiace.net/other/428.html)
- [分布式系统的事务处理](http://coolshell.cn/articles/10910.html)
- [保证分布式系统数据一致性的6种方案](http://www.cnblogs.com/soundcode/p/5590710.html)
- [RabbitMQ相关](http://www.cnblogs.com/zhangweizhong/category/855479.html)
- [SpringBoot RabbitMQ详解](http://www.cnblogs.com/ityouknow/p/6120544.html)

**RabbitMQ安装**
- 安装 [Erlang](http://www.erlang.org/downloads)
- 设置Erlang环境变量 `set ERLANG_HOME=C:\Program Files\erl8.0`
- 安装 [RabbitMQ](http://www.rabbitmq.com/download.html)

**RabbitMQ相关设置**
-   使用RabbitMQ管理插件 `rabbitmq-plugins.bat  enable  rabbitmq_management`
-   添加用户 `rabbitmqctl.bat add_user iblilife iblilife123`
-   设置管理员 `rabbitmqctl.bat set_user_tags iblilife administrator`
-   设置权限 `rabbitmqctl.bat set_permissions -p / iblilife ".*" ".*" ".*"`
-   查看用户列表 `rabbitmqctl.bat list_users`

```text
C:\setups\RabbitMQ Server\rabbitmq_server-3.6.9\sbin>rabbitmq-plugins.bat  enable  rabbitmq_management
The following plugins have been enabled:
  amqp_client
  cowlib
  cowboy
  rabbitmq_web_dispatch
  rabbitmq_management_agent
  rabbitmq_management

Applying plugin configuration to rabbit@DESKTOP-V4JJ7TF... started 6 plugins.

C:\setups\RabbitMQ Server\rabbitmq_server-3.6.9\sbin>rabbitmqctl.bat add_user iblilife iblilife123
Creating user "iblilife" ...

C:\setups\RabbitMQ Server\rabbitmq_server-3.6.9\sbin>rabbitmqctl.bat set_user_tags iblilife administrator
Setting tags for user "iblilife" to [administrator] ...

C:\setups\RabbitMQ Server\rabbitmq_server-3.6.9\sbin>rabbitmqctl.bat set_permissions -p / iblilife ".*" ".*" ".*"
Setting permissions for user "iblilife" in vhost "/" ...

C:\setups\RabbitMQ Server\rabbitmq_server-3.6.9\sbin>rabbitmqctl.bat list_users
Listing users ...
iblilife        [administrator]
guest   [administrator]

C:\setups\RabbitMQ Server\rabbitmq_server-3.6.9\sbin>
```