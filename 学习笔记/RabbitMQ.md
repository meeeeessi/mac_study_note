# RabbitMQ

## 安装配置

### 安装教程

[Mac安装MQ教程](https://www.jianshu.com/p/60c358235705)

本机安装位置：cd /opt/Homebrew/Cellar

本机MQ版本：**3.8.16**

启动rabbitMQ：**在/opt/Homebrew/Cellar。。。/3.8.16目录下 输入sudo sbin/rabbitmq-server指令启动**，启动成功后，此时在浏览器输入http://localhost:15672即可进入rabbitmq控制终端登录页面，默认用户名和密码为 guest/guest。

## RabbitMQ 的第一个程序

**生产者：**

```java
package com.wang;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import org.junit.Test;

import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.util.concurrent.TimeoutException;

public class Provider {

    //生产消息
    @Test
    public void testSendMessage() throws IOException, TimeoutException {

        //创建mq连接工厂对象
        ConnectionFactory connectionFactory=new ConnectionFactory();
        //设置连接rabbitmq主机
        connectionFactory.setHost("127.0.0.1");
        //设置端口号
        connectionFactory.setPort(5672);
        //设置连接虚拟主机
        connectionFactory.setVirtualHost("/ems");
        //设置访问虚拟主机的用户名和密码
        connectionFactory.setUsername("ems");
        connectionFactory.setPassword("123");

        //获取连接对象
        Connection connection=connectionFactory.newConnection();

        //获取连接中通道
        Channel channel=connection.createChannel();

        /*
        通道绑定对应消息队列
        参数1：队列名称，如果队列不存在自动创建
        参数2：用来定义队列特性是否要持久化 true持久化队列 false 不持久化
        参数3：是否独占队列 true独占队列 false不独占
        参数4：是否在消费完所有消息后自动删除队列 true自动删除 false不自动删除
        参数5：额外附加参数
         */
        channel.queueDeclare("hello",false,false,false,null);


        /*
        发布消息
        参数1：交换机名称
        参数2：队列名称
        参数3：传递消息额外设置
        参数4：消息的具体内容
         */
        channel.basicPublish("","hello",null,"hello rabbitmq".getBytes(StandardCharsets.UTF_8));

        channel.close();
        connection.close();

    }

}
```

**消费者：**

```java
package com.wang;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

public class Customer {

    public static void main(String[] args) throws IOException, TimeoutException {

        //创建mq连接工厂对象
        ConnectionFactory connectionFactory=new ConnectionFactory();
        //设置连接rabbitmq主机
        connectionFactory.setHost("127.0.0.1");
        //设置端口号
        connectionFactory.setPort(5672);
        //设置连接虚拟主机
        connectionFactory.setVirtualHost("/ems");
        //设置访问虚拟主机的用户名和密码
        connectionFactory.setUsername("ems");
        connectionFactory.setPassword("123");

        //获取连接对象
        Connection connection=connectionFactory.newConnection();

        //获取连接中通道
        Channel channel=connection.createChannel();

        /*
        通道绑定对应消息队列
        参数1：队列名称，如果队列不存在自动创建
        参数2：用来定义队列特性是否要持久化 true持久化队列 false 不持久化
        参数3：是否独占队列 true独占队列 false不独占
        参数4：是否在消费完所有消息后自动删除队列 true自动删除 false不自动删除
        参数5：额外附加参数
         */
        channel.queueDeclare("hello",false,false,false,null);

        /*
        消费消息
        参数1：消费那个队列的消息 队列名称
        参数2：开始消息的自动确认机制 true消费者自动向rabbitmq确认消息消费 false不会确认消费，即使确认消费了在队列中也会标记未被消费
        参数3：消费时的回调借口
         */
        channel.basicConsume("hello",true,new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                super.handleDelivery(consumerTag, envelope, properties, body);
                System.out.println("=================="+new String(body));
            }
        });

        //消费者通道需要一直打开，如果关闭channel，connection，其他线程还没运行完主线程就已经结束将通道关闭，导致其他线程无法输出。
//        channel.close();
//        connection.close();

    }

}
```

**将消息设置为持久化：MessageProperties.PERSISTENT_TEXT_PLAIN**

```java
channel.basicPublish("","aa", MessageProperties.PERSISTENT_TEXT_PLAIN,"hello rabbitmq".getBytes(StandardCharsets.UTF_8));
```

