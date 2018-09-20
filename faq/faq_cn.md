**Q: EMQ X是什么？**

EMQ X是一个开源的MQTT消息服务器（message broker，也有翻译成消息代理），用于支持各种接入标准MQTT协议的设备，实现从设备端到服务器端的消息传递，以及从服务器端到设备端的设备控制消息转发。从而实现物联网设备的数据采集，和对设备的操作和控制。



**Q: EMQ X与物联网平台的关系是什么？**

典型的物联网平台包括设备硬件、数据采集、数据存储、分析、Web/移动应用等。EMQ X位于数据采集这一层，分别与硬件和数据存储、分析进行交互，是物联网平台的核心：前端的硬件通过MQTT协议与位于数据采集层的EMQ X交互，通过EMQ X将数据采集后，通过EMQ X提供的数据接口，将数据保存到后台的持久化平台中（各种关系型数据库和NOSQL数据库），或者流式数据处理框架等，上层应用通过这些数据分析后得到的结果呈现给最终用户。



**Q: EMQ X有哪些产品？**

EMQ X现在[有3个产品](https://www.emqx.io/products)，主要体现在支持的连接数目、功能和服务等方面的区别：

- EMQ X Broker：EMQ开源版，提供MQTT协议、CoAP和LwM2M等常见物联网协议的支持；支持10万级连接
- EMQ X Enterprise：EMQ企业版，在开源版基础上，增加了数据持久化、与别的消息Broker桥接、LoRaWAN、监控、Kubernates部署等方面的支持；支持百万级别连接
- EMQ X Platform：EMQ大客户版，在企业版基础上，支持千万级的连接和跨数据中心的解决方案，提供物联网平台全生命周期中需要的各种服务（咨询、架构设计、开发、测试与运维等服务）



**Q: EMQ X企业版（Enterprise）和开源版（Broker）的区别是什么？**

企业版基于开源版，包含了开源版的所有功能，与开源版相比，主要有以下方面的区别，

- 接入的设备量级：开源版的稳定接入为10万，而企业版为100万
- 数据持久化：企业版支持将消息转储到各类持久化数据库中，包括流行的关系型数据库，比如MySQL、PostgresSQL；内存数据库Redis；非关系型数据库MongoDB等
- 与流式数据处理引擎Kafka对接：内置支持跟Kafka消息处理的对接，用户可以通过消费Kafka消息来实现实时流式数据的处理
- 与RabbitMQ的桥接：支持与RabbitMQ的对接，将MQTT消息转发给RabbitMQ，别的应用可以通过消费RabbitMQ消息来实现可能的异构系统的集成
- 监控（EMQ Control Center）
  - 支持对EMQ X的监控，包括连接、主题、消息和对话（session）等
  - Erlang虚拟机：Erlang虚拟机的进程、线程、数据库和锁的使用等
  - 机器监控：CPU、内存、磁盘、网络和操作系统等各类指标
- 高安全：可以通过配置基于TLS、DTLS的安全连接（证书）等来保证安全的连接传输



**Q: EMQ X与NB-IoT、LoRAWAN的关系是什么？**

EMQ X是一个开源的MQTT消息服务器，并且MQTT是一个TCP协议栈上位于应用层的协议；而NB-IoT和LoRAWAN在TCP协议层处于物理层，负责物理信号的传输。因此两者在TCP协议栈的不同层次上，实现不同的功能。



**Q: 怎么样才能使用EMQ X？**

EMQ X提供的开源版可以免费下载使用，用户可以在[这里](https://www.emqx.io/downloads/emq/broker?osType=Linux)下载开源版。

EMQ X提供的企业版用户也可以进行下载，用户可以在[这里](https://www.emqx.io/downloads/emq/enterprise?osType=Linux)下载企业版后再[申请试用license](https://www.emqx.io/account?tab=login)之后就可以进行企业版软件的试用。

另外用户还可以通过使用公有云来直接创建企业版的EMQ服务，比如通过[阿里云](https://market.aliyun.com/products/56014009/cmjj029182.html?spm=5176.730005.productlist.d_cmjj029182.53cf3524sCmfnp)，[青云](https://appcenter.qingcloud.com/search/category/iot)等平台来创建。



**Q: EMQ X可以提供方案咨询服务吗？**

可以。EMQ X在为客户搭建物联网平台的咨询方面有丰富的经验，包括为互联网客户和电信运营商搭建千万级物联网平台的实践。包括如何搭建负载均衡、集群、安全策略、数据存储和分析方案等方面可以根据客户的需求制定方案，满足业务发展的需求。



**Q: EMQ X推荐的生产部署环境的操作系统是什么？**

EMQ X在CentOS、Ubuntu、Debian等Linux发行版上都可以运行，生产系统推荐CentOS Linux 7.2+的版本。



**Q: EMQ X支持Windows操作系统吗？**

支持。中文读者可以参考[文章](https://www.jianshu.com/p/e5cf0c1fd55c).



**Q: EMQ X可以对私有协议进行扩展吗？如果可以的话，应该如何实现？**





**Q: EMQ X如何预估资源的使用？**

EMQ X对资源的使用主要有以下的影响因素，每个因素都会对计算和存储资源的使用产生影响，

- 连接数：对于每一个MQTT长连接，EMQ X会生成一个对应的连接和会话管理Erlang进程，每个进程都会耗费一定的资源。连接数越高，所需的资源越多。
- 平均吞吐量：值的是每秒pub和sub的消息数量。吞吐量越高，EMQ X的路由处理和消息转发处理就需要更多的资源。
- 消息体大小：消息体越大，在EMQ X中处理消息转发的时候在内存中进行数据存储和处理，所需的资源就越多。
- 主题数目：如果主题数越多，在EMQ X中的路由表会相应增长，因此所需的资源就越多
- QoS：消息的QoS越高，EMQ X服务器端所处理的逻辑会更多，因此会耗费更多的资源

另外，如果设备直接通过TLS（加密的连接）来连接EMQ X，EMQ X会需要额外的资源（主要是CPU资源），因此推荐的做法是在EMQ X前面加入负载均衡，该负载均衡节点附带做加密连接处理，而负载均衡与后面的EMQ X节点的通信则采用非加密的网络传输方式，实现职责分离。

您可以参考[这里](https://www.emqx.io)来预估计算资源的使用；如果您想在公有云端快速创建符合最佳实践的EMQ X部署实例，请参考[这里](https://www.emqx.io)。



**Q: MQTT协议与HTTP协议相比，有何优点和弱点？ **

HTTP协议是一个无状态的协议，每个HTTP请求为TCP短连接，每次请求都需要重新创建一个TCP连接（可以通过keep-alive属性来优化TCP连接的使用，多个HTTP请求可以共享该TCP连接）；而MQTT协议为长连接协议，每个客户端都会保持一个长连接。与HTTP协议相比优势在于，

- MQTT的长连接可以用于实现从设备端到服务器端的消息传送之外，还可以实现从服务器端到设备端的实时控制消息发送，而HTTP协议要实现此功能只能通过轮询的方式，效率相对来说比较低
- MQTT协议在维护连接的时候会发送心跳包，因此协议以最小代价内置支持设备“探活”的功能，而HTTP协议要实现此功能的话需要单独发出HTTP请求，实现的代价会更高
- 低带宽、低功耗。MQTT在传输报文的大小上与HTTP相比有巨大的优势，因为MQTT协议在连接建立之后，由于避免了建立连接所需要的额外的资源消耗，发送实际数据的时候报文传输所需带宽与HTTP相比有很大的优势，参考网上[有人做的测评](https://medium.com/@flespi/http-vs-mqtt-performance-tests-f9adde693b5f )，发送一样大小的数据，MQTT比HTTP少近50倍的网络传输数据，而且速度快了将近20倍。在网上有人做的[另外一个评测显示](http://stephendnicholas.com/posts/power-profiling-mqtt-vs-https )，接收消息的场景，MQTT协议的耗电量为HTTP协议的百分之一，而发送数据的时候MQTT协议的耗电量为HTTP协议的十分之一
- MQTT提供消息质量控制（QoS），消息质量等级越高，消息交付的质量就越有保障，在物联网的应用场景下，用户可以根据不同的使用场景来设定不同的消息质量等级



**Q: 什么是认证鉴权？使用场景是什么？**

认证鉴权指的是当一个客户端连接到MQTT服务器的时候，通过服务器端的配置来控制客户端连接服务器的权限。EMQ的认证机制包含了有三种，

- 用户名密码：针对每个MQTT客户端的连接，可以在服务器端进行配置，用于设定用户名和密码，只有在用户名和密码匹配的情况下才可以让客户端进行连接
- ClientID：每个MQTT客户端在连接到服务器的时候都会有个唯一的ClientID，可以在服务器中配置可以连接该服务器的ClientID列表，这些ClientID的列表
- 匿名：允许匿名访问

通过用户名密码、ClientID认证的方式除了通过配置文件之外，还可以通过各类数据库和外部应用来配置，比如MySQL、PostgreSQL、Redis、MongoDB、HTTP和LDAP等。



**Q: 我可以捕获设备上下线的事件吗？该如何使用？**

EMQ X可以捕获设备的上下线的事件，并将其保存到数据库中（支持的数据库包括Redis、MySQL、PostgreSQL、MongoDB和Cassandra）。用户可以通过



**Q: 什么是hook？使用场景是什么？**



**Q: 什么是WebSocket？什么情况下我需要通过WebSocket去调用EMQ X的服务？**



**我想限定某些主题只为特定的客户端所使用，EMQ X该如何进行配置？**



**什么是共享订阅？有何使用场景？**



**EMQ X能做流量控制吗？**



**什么是离线消息？**



**什么是代理订阅？使用场景是什么？**



**EMQ X是如何实现支持大规模并发和高可用的？**



**EMQ X能把接入的消息保存到数据库吗？**



**EMQ X能把接入的消息发布到流式大数据处理引擎吗？**



**EMQ X支持集群吗？有哪些实现方式？**



**我可以把消息从EMQ X转到别的消息中间件上吗？比如说RabbitMQ？**



**我可以把消息从EMQ X转到公有云MQTT服务上吗？比如AWS或者Azure的IoT Hub？**



**EMQ X可以接入别的MQTT Broker（比如Mosquito）的消息吗？**



**系统主题有何用处？都有哪些系统主题？**



**我想跟踪特定消息的发布和订阅过程，应该如何做？**



**为什么我做压力测试的时候，连接数目和吞吐量老是上不去，有系统调优指南吗？**



**我的连接数目并不大，EMQ X生产环境部署需要多节点吗？**



**EMQ X支持做加密连接吗？推荐的部署方案是什么？**
