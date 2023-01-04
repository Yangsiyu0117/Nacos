# Nacos

[TOC]

## <u>文档标识</u>

| 文档名称 | Nacos    |
| -------- | -------- |
| 版本号   | <V1.0.0> |

## <u>文档修订历史</u>

| 版本   | 日期       | 描述   | 文档所有者 |
| ------ | ---------- | ------ | ---------- |
| V1.0.0 | 2022.11.23 | create | 杨丝雨     |
|        |            |        |            |
|        |            |        |            |

## <u>路径规划</u>

| 路径        | 描述         | remarks |
| ----------- | ------------ | ------- |
| /nacos/bin  | 二进制目录   |         |
| /nacos/conf | 配置文件目录 |         |
| /nacos/logs | 日志目录     |         |

## <u>端口规划</u>

| 端口 | 协议 | remrks                                                       | **与主端口偏移量** |
| ---- | ---- | ------------------------------------------------------------ | ------------------ |
| 8848 | TCP  | 默认主端口                                                   |                    |
| 9848 | TCP  | 客户端gRPC请求服务端端口，用于客户端服务发起连接和请求，nacos2.0版本相比1.x新增了gRPC的通信方式 | 1000               |
| 9849 | TCP  | 客户端gRPC请求服务端端口，用于服务间同步等，nacos2.0版本相比1.x新增了gRPC的通信方式 | 1001               |

## <u>相关文档参考</u>

[Nacos官网]: https://nacos.io/zh-cn/
[Nacos官方文档]: https://nacos.io/zh-cn/docs/v2/what-is-nacos.html
[Nacos官方下载地址]: https://github.com/alibaba/nacos/releases



## 什么是 Nacos

Nacos /nɑ:kəʊs/ 是 Dynamic Naming and Configuration Service的首字母简称，一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。

Nacos 致力于帮助您发现、配置和管理微服务。Nacos 提供了一组简单易用的特性集，帮助您快速实现动态服务发现、服务配置、服务元数据及流量管理。

Nacos 帮助您更敏捷和容易地构建、交付和管理微服务平台。 Nacos 是构建以“服务”为中心的现代应用架构 (例如微服务范式、云原生范式) 的服务基础设施。

## Nacos 架构

![nacos](https://nacos.io/img/nacosMap.jpg)

## Nacos概念

- ## 地域

------

物理的数据中心，资源创建成功后不能更换。

- ## 可用区

------

同一地域内，电力和网络互相独立的物理区域。同一可用区内，实例的网络延迟较低。

- ## 接入点

------

地域的某个服务的入口域名。

- ## 命名空间

------

用于进行租户粒度的配置隔离。不同的命名空间下，可以存在相同的 Group 或 Data ID 的配置。Namespace 的常用场景之一是不同环境的配置的区分隔离，例如开发测试环境和生产环境的资源（如配置、服务）隔离等。

- ## 配置

------

在系统开发过程中，开发者通常会将一些需要变更的参数、变量等从代码中分离出来独立管理，以独立的配置文件的形式存在。目的是让静态的系统工件或者交付物（如 WAR，JAR 包等）更好地和实际的物理运行环境进行适配。配置管理一般包含在系统部署的过程中，由系统管理员或者运维人员完成。配置变更是调整系统运行时的行为的有效手段。

- ## 配置管理

------

系统配置的编辑、存储、分发、变更管理、历史版本管理、变更审计等所有与配置相关的活动。

- ## 配置项

------

一个具体的可配置的参数与其值域，通常以 param-key=param-value 的形式存在。例如我们常配置系统的日志输出级别（logLevel=INFO|WARN|ERROR） 就是一个配置项。

- ## 配置集

------

一组相关或者不相关的配置项的集合称为配置集。在系统中，一个配置文件通常就是一个配置集，包含了系统各个方面的配置。例如，一个配置集可能包含了数据源、线程池、日志级别等配置项。

- ## 配置集 ID

------

Nacos 中的某个配置集的 ID。配置集 ID 是组织划分配置的维度之一。Data ID 通常用于组织划分系统的配置集。一个系统或者应用可以包含多个配置集，每个配置集都可以被一个有意义的名称标识。Data ID 通常采用类 Java 包（如 com.taobao.tc.refund.log.level）的命名规则保证全局唯一性。此命名规则非强制。

- ## 配置分组

------

Nacos 中的一组配置集，是组织配置的维度之一。通过一个有意义的字符串（如 Buy 或 Trade ）对配置集进行分组，从而区分 Data ID 相同的配置集。当您在 Nacos 上创建一个配置时，如果未填写配置分组的名称，则配置分组的名称默认采用 DEFAULT_GROUP 。配置分组的常见场景：不同的应用或组件使用了相同的配置类型，如 database_url 配置和 MQ_topic 配置。

- ## 配置快照

------

Nacos 的客户端 SDK 会在本地生成配置的快照。当客户端无法连接到 Nacos Server 时，可以使用配置快照显示系统的整体容灾能力。配置快照类似于 Git 中的本地 commit，也类似于缓存，会在适当的时机更新，但是并没有缓存过期（expiration）的概念。

- ## 服务

------

通过预定义接口网络访问的提供给客户端的软件功能。

- ## 服务名

------

服务提供的标识，通过该标识可以唯一确定其指代的服务。

- ## 服务注册中心

------

存储服务实例和服务负载均衡策略的数据库。

- ## 服务发现

------

在计算机网络上，（通常使用服务名）对服务下的实例的地址和元数据进行探测，并以预先定义的接口提供给客户端进行查询。

- ## 元信息

------

Nacos数据（如配置和服务）描述信息，如服务版本、权重、容灾策略、负载均衡策略、鉴权配置、各种自定义标签 (label)，从作用范围来看，分为服务级别的元信息、集群的元信息及实例的元信息。

- ## 应用

------

用于标识服务提供方的服务的属性。

- ## 服务分组

------

不同的服务可以归类到同一分组。

- ## 虚拟集群

------

同一个服务下的所有服务实例组成一个默认集群, 集群可以被进一步按需求划分，划分的单位可以是虚拟集群。

- ## 实例

------

提供一个或多个服务的具有可访问网络地址（IP:Port）的进程。

- ## 权重

------

实例级别的配置。权重为浮点数。权重越大，分配给该实例的流量越大。

- ## 健康检查

------

以指定方式检查服务下挂载的实例 (Instance) 的健康度，从而确认该实例 (Instance) 是否能提供服务。根据检查结果，实例 (Instance) 会被判断为健康或不健康。对服务发起解析请求时，不健康的实例 (Instance) 不会返回给客户端。

- ## 健康保护阈值

------

为了防止因过多实例 (Instance) 不健康导致流量全部流向健康实例 (Instance) ，继而造成流量压力把健康实例 (Instance) 压垮并形成雪崩效应，应将健康保护阈值定义为一个 0 到 1 之间的浮点数。当域名健康实例数 (Instance) 占总服务实例数 (Instance) 的比例小于该值时，无论实例 (Instance) 是否健康，都会将这个实例 (Instance) 返回给客户端。这样做虽然损失了一部分流量，但是保证了集群中剩余健康实例 (Instance) 能正常工作。

## 基本架构及概念

![nacos_arch.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/338441/1561217892717-1418fb9b-7faa-4324-87b9-f1740329f564.jpeg)

- ### 服务 (Service)

------

服务是指一个或一组软件功能（例如特定信息的检索或一组操作的执行），其目的是不同的客户端可以为不同的目的重用（例如通过跨进程的网络调用）。Nacos 支持主流的服务生态，如 Kubernetes Service、gRPC|Dubbo RPC Service 或者 Spring Cloud RESTful Service。

- ### 服务注册中心 (Service Registry)

------

服务注册中心，它是服务，其实例及元数据的数据库。服务实例在启动时注册到服务注册表，并在关闭时注销。服务和路由器的客户端查询服务注册表以查找服务的可用实例。服务注册中心可能会调用服务实例的健康检查 API 来验证它是否能够处理请求。

- ### 服务元数据 (Service Metadata)

------

服务元数据是指包括服务端点(endpoints)、服务标签、服务版本号、服务实例权重、路由规则、安全策略等描述服务的数据。

- ### 服务提供方 (Service Provider)

------

是指提供可复用和可调用服务的应用方。

- ### 服务消费方 (Service Consumer)

------

是指会发起对某个服务调用的应用方。

- ### 配置 (Configuration)

------

在系统开发过程中通常会将一些需要变更的参数、变量等从代码中分离出来独立管理，以独立的配置文件的形式存在。目的是让静态的系统工件或者交付物（如 WAR，JAR 包等）更好地和实际的物理运行环境进行适配。配置管理一般包含在系统部署的过程中，由系统管理员或者运维人员完成这个步骤。配置变更是调整系统运行时的行为的有效手段之一。

- ### 配置管理 (Configuration Management)

------

在数据中心中，系统中所有配置的编辑、存储、分发、变更管理、历史版本管理、变更审计等所有与配置相关的活动统称为配置管理。

- ### 名字服务 (Naming Service)

------

提供分布式系统中所有对象(Object)、实体(Entity)的“名字”到关联的元数据之间的映射管理服务，例如 ServiceName -> Endpoints Info, Distributed Lock Name -> Lock Owner/Status Info, DNS Domain Name -> IP List, 服务发现和 DNS 就是名字服务的2大场景。

- ### 配置服务 (Configuration Service)

------

在服务或者应用运行过程中，提供动态配置或者元数据以及配置管理的服务提供者。

## Nacos支持三种部署模式

- 单机模式 - 用于测试和单机试用。
- 集群模式 - 用于生产环境，确保高可用。
- 多集群模式 - 用于多数据中心场景。

## Nacos单机部署

### 预备环境准备

64 bit JDK 1.8+；

1、下载JDK安装包

```shell
wget https://repo.huaweicloud.com/java/jdk/8u202-b08/jdk-8u202-linux-x64.tar.gz
```

2、建立JDK目录

```shell
mkdir /usr/local/java
cp jdk-8u202-linux-x64.tar.gz /usr/local/java/
cd /usr/local/java/
```

3、解压

```shell
tar xvf jdk-8u202-linux-x64.tar.gz
```

4、配置环境变量

```shell
vim /etc/profile
# 添加到末尾
export   JAVA_HOME=/usr/local/java/jdk1.8.0_202
export   CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export  PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
export   JRE_HOME=$JAVA_HOME/jre
```

5、验证

```shell
# 刷新环境变量
source /etc/profile
```

6、验证环境变量

```shell
# 输入
echo $JAVA_HOME
# 输出
/usr/local/java/jdk1.8.0_202
```

7、验证JDK

```shell
# 输入
java -version
# 输出
java version "1.8.0_202"
Java(TM) SE Runtime Environment (build 1.8.0_202-b08)
Java HotSpot(TM) 64-Bit Server VM (build 25.202-b08, mixed mode)
```

### 下载编译后压缩包方式

您可以从 [最新稳定版本](https://github.com/alibaba/nacos/releases) 下载 `nacos-server-$version.zip` 包。

```shell
unzip nacos-server-$version.zip 或者 tar -xvf nacos-server-$version.tar.gz
cd nacos/bin
```

### 启动服务器

- 注：Nacos的运行需要以至少2C4g60g*3的机器配置下运行。

启动命令(standalone代表着单机模式运行，非集群模式):

```shell
sh startup.sh -m standalone
```

### 配置开机启动文件

```shell
vim /lib/systemd/system/nacos.service
[Unit]
Description=nacos
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/src/nacos/bin/startup.sh -m standalone
ExecReload=/usr/local/src/nacos/bin/shutdown.sh
ExecStop=/usr/local/src/nacos/bin/shutdown.sh
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

```shell
# 重载所有服务
systemctl daemon-reload
# 设置开机启动
systemctl enable nacos.service
# 查看开机启动状态
systemctl is-enabled nacos.service
# 启动
systemctl start nacos.service
# 停止
systemctl stop nacos.service
# 重启
systemctl restart nacos.service
```

### 问题解决

问题一：nacos自动启动服务报错
解决链接：[解决nacos启动报错问题](https://blog.csdn.net/JesusMak/article/details/106919995)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623114424846.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0plc3VzTWFr,size_16,color_FFFFFF,t_70)

### 服务注册&发现和配置管理

#### 服务注册

```shell
curl -X POST 'http://127.0.0.1:8848/nacos/v1/ns/instance?serviceName=nacos.naming.serviceName&ip=20.18.7.10&port=8080'
```

#### 服务发现

```shell
curl -X GET 'http://127.0.0.1:8848/nacos/v1/ns/instance/list?serviceName=nacos.naming.serviceName'
```

#### 发布配置

```shell
curl -X POST "http://127.0.0.1:8848/nacos/v1/cs/configs?dataId=nacos.cfg.dataId&group=test&content=HelloWorld"
```

#### 获取配置

```shell
curl -X GET "http://127.0.0.1:8848/nacos/v1/cs/configs?dataId=nacos.cfg.dataId&group=test"
```

### 单机模式支持mysql

在0.7版本之前，在单机模式时nacos使用嵌入式数据库实现数据的存储，不方便观察数据存储的基本情况。0.7版本增加了支持mysql数据源能力，具体的操作步骤：

- 1.安装数据库，版本要求：5.6.5+
- 2.初始化mysql数据库，数据库初始化文件：mysql-schema.sql
- 3.修改conf/application.properties文件，增加支持mysql数据源配置（目前只支持mysql），添加mysql数据源的url、用户名和密码。

```
spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://11.162.196.16:3306/nacos_devtest?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=nacos_devtest
db.password=youdontknow
```

再以单机模式启动nacos，nacos所有写嵌入式数据库的数据都写到了mysql

### 验证

> http://192.168.10.168:8848/nacos