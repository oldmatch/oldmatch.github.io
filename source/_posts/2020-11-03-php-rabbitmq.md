---
thumbnail: https://oldmatch-1302978637.cos.ap-guangzhou.myqcloud.com/banner/sky.jpg
title: RabbitMQ 基础使用
date: 2020-11-03 18:07
tags:
  - blog
  - hexo
categories:
  - 分享
---

# RabbitMQ 基础使用

### RabbitMQ介绍

* MQ是 message queue 的简称，是应用程序和应用程序之间通信的方法。

* RabbitMQ是一个由erlang语言编写的、开源的、在AMQP基础上完整的、可复用的企业消息系统。支持多种语言，包括java、Python、ruby、PHP、C/C++等。

* AMQP：advanced message queuing protocol ，一个提供统一消息服务的应用层标准高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计。基于此协议的客户端与消息中间件可传递消息并不受客户端/中间件不同产品、不同开发语言等条件的限制。

* 实用优点：应用解耦，流量削峰，异步处理


## RabbitMQ安装(docker安装)
* https://hub.docker.com/_/rabbitmq

> tips: management版本是带web管理工具的

docker-compose.yml

    version: '2'
    services:
    rabbitmq:
    image: rabbitmq:management
    container_name: rabbitmq
    hostname: myrabbitmq
    ports:
      - 15672:15672
      - 5672:5672
    environment:
      - RABBITMQ_DEFAULT_USER=guest
      - RABBITMQ_DEFAULT_PASS=guest
    restart: always
    # 里面的/var/lib/rabbitmq/.erlang.cookie 需要600权限
    volumes:
      - ./data:/var/lib/rabbitmq

## 启动
    docker-compose up -d

1. docker ps -a 查看状态
2. 访问ip:15762查看web界面 ps:记得打开端口可以访问


## 卸载
    docker-compose down

## 简单使用(单发送单接收：简单的发送与接收，没有特别的处理

![](https://www.rabbitmq.com/img/tutorials/python-one.png)


引入包

    composer require php-amqplib/php-amqplib


发送

    // 连接RabbitMQ
    $connection = new \PhpAmqpLib\Connection\AMQPStreamConnection('ip', 5672, 'guest', 'guest');
    // 创建一个通道
    $channel = $connection->channel();
    // 声明一个队列
    $channel->queue_declare('hello', false, false, false, false);
    // 发送的内容
    $msg = new \PhpAmqpLib\Message\AMQPMessage('Hello World');
    $channel->basic_publish($msg, '', 'hello');
    
    echo " [x] Sent 'Hello World!'\n";
    // 关闭通道
    $channel->close();
    // 关闭连接
    $connection->close();
    
    
消费

    // 连接RabbitMQ
    $connection = new \PhpAmqpLib\Connection\AMQPStreamConnection('ip', 5672, 'guest', 'guest');
    // 创建一个通道
    $channel = $connection->channel();
    // 声明一个队列
    $channel->queue_declare('hello', false, false, false, false);
    
    echo ' [*] Waiting for messages. To exit press CTRL+C', "\n";
    
    
    // 当调用basic_consume，我们的代码会阻塞。当我们收到消息时，我们的回调函数将通过接受到返回的消息传递
    $callback = function ($msg) {
        echo " [x] Received ", $msg->body, "\n";
    };
    
    $channel->basic_consume('hello', '', false, true, false, false, $callback);
    
    while (count($channel->callbacks)) {
        $channel->wait();
    }
    
    
发送测试
     
    [x] Sent 'Hello World!'
    
消费测试

    [*] Waiting for messages. To exit press CTRL+C
    [x] Received Hello World


## 致谢
感谢[海清博客](https://blog.lihq.xyz/2020/08/13/rabbitmq-first-experience)的分享并指导