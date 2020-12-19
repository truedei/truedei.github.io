# TrueDei
这是一个Linux监控平台

利用Python进行采集数据，将采集的数据使用消息中间件RabbitMQ进行暂时存储，
最终使用java的SpringBoot框架编写的web端，进行展现出来丰富的图片，表格等形式展现给用户。

# 一、说明

版本：

```xml
TrueDeiService-version   0.1

TrueDeiClient-version   0.1
```

总体结构流程图如下图所示：

![image-20200301134033440](img\image-20200301134033440.png)

因为是借助RabbitMQ做的消息中间件，所以需要配置一台RabiitMQ服务，此处为了方便，我是在Docker中部署的。所以就算你把RabbitMQ或者客户端部署到Docker容器上也是轻而易举的。不用担心部署麻烦的问题。当然了，我也做了详细的部署方案说明。



已经测试通过的Linux服务器版本：Centos7.x，其他服务器暂未测试。

# 二、解决方案

## 1、单服务器监控方案

### 1、需求&场景:

这种方案比较适用于监控个人服务器。

例如：我自己有一台云服务器或者私有的服务器，跑着的时候，可能随时出现问题，服务崩溃了？web服务器访问慢了？自己可能不是专门的运维人员，可能不能立马诊断问题，然后去网络搜索各种资料，最终也没能解决。自己就无可下手了。

那么这套解决方案整适合你。需要简单的部署即可完美解决你的问题。

![image-20200301135820848](img\image-20200301135820848.png)

如上图所示：

​	有一台Linux服务器需要监控，那么就需要下载TrueDeiService安装部署在需要监控的Linux服务器上。

​    然后把RabbitMQ部署在任意一台服务器上，然后把TrueDeiClient服务器在任意一台服务器上。当然了只要全部都可以通信就好。也可以部署在一台上。

TrueDeiService的部署很简单，需要把TrueDeiService下载后，解压到Linux服务器上。然后需要使用Python3启动。

需要修改下RabiitMQ的IP或者账号密码

在conf/truedei.conf文件中:

```yml
#RabbitMQ相关的配置信息
#你的RabbitMQ的地址
RabbitMQHost 监听的Ip
#RabbitMQ端口号
RabbitMQPost 5672
#创建的账号，当然了也可以使用默认的guest账号，密码也是guest
RabbitMQUsername admin
#账号的密码
RabbitMQPassword 123456
```



运行如下图所示：

![image-20200301140728040](img\image-20200301140728040.png)



TrueDeiClient的部署需要把项目加载到SpringBoot的开发工具中，这里我推荐的是IDEA。

需要修改：rabbitMq 服务器的地址、端口、账号、密码。

```yml
  #配置rabbitMq 服务器
  rabbitmq:
    host: 自己的rabbitMq 服务器IP地址或者域名
    port: 5672
    username: admin
    password: 123456
```

然后把此项目利用Maven打包成jar包。

然后把这个jar包发布到任何一台计算机上即可。也可以发布到自己的个人电脑。

利用java -jar truedeiclient.java即可运行。



访问:http://127.0.0.1:8080/admin

就可以看到如下界面：此处我设计了开关，最初是关闭的。需要打开后就可以自动拉取数据了

![image-20200301141214252](img\image-20200301141214252.png)

