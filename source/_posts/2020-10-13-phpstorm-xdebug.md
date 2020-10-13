---
thumbnail: https://oldmatch-1302978637.cos.ap-guangzhou.myqcloud.com/banner/u%3D2477873881%2C3222657671%26fm%3D26%26gp%3D0.png
title: phpstorm 断点调试vagrant虚拟机中php项目
date: 2020-10-13 17:36
tags:
  - blog
  - hexo
categories:
  - 分享
---

# phpstorm 断点调试vagrant虚拟机中php项目

### 开发环境
代码开发环境window10,开发工具phpstorm

php项目运行环境：vagrant虚拟机中，操作系统 centos7.2, php 运行环境 LNMP, php 版本 7.3。

虚拟机ip：192.168.56.10

## 步骤一：给php安装xdebug扩展 
(省略。。。。如果是宝塔很简单啦)


## 步骤二：在php.ini文件中加上

    [xdebug]
    zend_extension=/alidata/server/php7.3/lib/php/extensions/no-debug-non-zts-20180731/xdebug.so  (如果安装完拓展就有的话，不用这句)
    xdebug.idekey=PHPSTORM
    ;如果开启此，将忽略下面的 xdebug.remote_host 的参数
    xdebug.remote_connect_back = 1
    ;注意这里是，客户端的ip<即IDE的机器的ip,不是你的web server>
    xdebug.remote_host=127.0.0.1
    xdebug.remote_enable=1
    xdebug.remote_port=9001
    xdebug.remote_handler=dbgp
    xdebug.remote_autostart=1
    xdebug.default_enable=1
    
## 步骤三：phpstorm配置

1.填写debug port
![](https://oldmatch-1302978637.cos.ap-guangzhou.myqcloud.com/debug/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20201013172411.png)

DBGp Proxy 中 不需要设置！！！因为这可能是其他调试方式中需要配置的，本案例不需要！！！

2.设置 Servers
![](https://oldmatch-1302978637.cos.ap-guangzhou.myqcloud.com/debug/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20201013172631.png)

注意域名 一定要在真机 hosts文件 中加上

注意 Use path mapping 一定 加上，并且mac 真机 和  work  中 php 项目路径一定对应正确！！！

## 步骤四：创建 Run/Debug  Configurations
1.选择 Edit Configurations

2.对话框中左上角 “+ ”号 选择 “PHP Web Page”，选择刚刚创建的server

3.设置 /路由 等

![](https://oldmatch-1302978637.cos.ap-guangzhou.myqcloud.com/debug/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20201013172835.png)


## 结果
在代码打上断点，在浏览器等地方访问你项目访问你的代码，就可打断点成功了。
![](https://oldmatch-1302978637.cos.ap-guangzhou.myqcloud.com/debug/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20201013173202.png)