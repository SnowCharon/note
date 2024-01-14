# 前置知识

我们使用Tomcat来运行后端项目，无论是在JavaWeb还是在SpringBoot中——SpringBoot内置集成了Tomcat。Tomcat 是一个HTTP 服务器 + Servlet 容器，首先需要Socket 监听客户端请求连接，返回响应数据；还要加载和管理Servlet，以及具体处理请求。Tomcat分别使用Connector（连接器）和Container（容器）来实现上述两个功能。

![](assets/Pasted%20image%2020240114165657.png) 

如图，每个Tomcat实例是一个Server,一个Server有多个Service,一个Service有多个Connector和一个Container,Connector接收HTTP请求报文，解析成标准的ServletRequest 和 ServletResponse后与Container进行通信

# Tomcat启动

Tomcat启动时就是创建了一个实例，即一个Server，会首先初始化一个Connector和一个Container,最后会来到GenericServlet——因为该类实现了Servlet接口的init方法，结合ServletConfig，对Servlet进行初始化等等

示例项目配置为：[http://localhost:8080](http://localhost:8080)

实例Servlet的访问路径为：

“/see”对应SeeServlet——也是测试的主要Servlet

“/other”对应OtherServlet——作为参照Servlet

Jdk-version:17


所用依赖包：Servlet-api.jar、catalina.jar、tomcat-coyote.jar、tomcat-util-scan.jar皆来自于Tomcat的lib文件夹下
![](assets/Pasted%20image%2020240114165709.png)
如上，在执行完init方法后，原本为null的context被初始化，包括名称、classCache等等

## Connector

连接器的主要功能：

1. 监听服务器端口，读取客户端请求

2. 将请求数据按照具体的协议解析(HTTP/AJP)生成统一的请求对象

3. 调用 Servlet 容器，进行业务处理

4. 得到响应对象，返回客户端

核心组件：ProtocolHandler和Adapter
![](assets/Pasted%20image%2020240114165728.png)
### ProtocolHandler（协议处理器）

Tomcat支持多协议(HTTP/AJP)以及多种I/O方式(BIO/NIO)。由于 I/O 模型和应用层协议可以自由组合，比如 NIO + HTTP 或者 NIO2 + AJP，因此使用了 ProtocolHandler 的接口来封装这两种变化点

![](assets/Pasted%20image%2020240114165746.png)

它还包含了2个重要部件：EndPoint 和 Processor

###### EndPoint

EndPoint是一个接口，对应的抽象实现类是AbstractEndpoint。用于启动Socket监听,该接口按照I/O方式进行分类实现,如Nio2Endpoint表示非阻塞式Socket I/O。这有两个重要的子组件： Acceptor 和 SocketProcessor。

其中 Acceptor 用于监听 Socket 连接请求。SocketProcessor 用于处理接收到的 Socket 提交到线程池（Executor)来执行。调用协议处理组件 Processor 进行处理。

###### Processor

Processor用于按照指定协议读取数据,并将请求交由容器处理,如Http11Processor实现了HTTP 1.1协议的解析方法和请求处理方式。
![](assets/Pasted%20image%2020240114165759.png)

###### Adapter组件

由于协议不同，客户端发过来的请求信息也不尽相同，Tomcat 定义了自己的 Request 类来存放这些请求信息。通过 Processor调用 CoyoteAdapter 的 Sevice 方法，将 Tomcat Request 转成ServletRequest
###### 小结

连接器用 ProtocolHandler 接口来封装通信协议和I/O 模型的差异，ProtocolHandler 内部又分为 EndPoint 和 Processor 模块，EndPoint 负责底层 Socket通信，Proccesor 负责应用层协议解析。连接器通过适配器 Adapter 调用容器

![](assets/Pasted%20image%2020240114165815.png)

## Container（容器）

Tomcat 设计了 4 种容器，分别是 Engine、Host、Context 和 Wrapper，且它们是父子关系。

![](assets/Pasted%20image%2020240114165824.png)

1. 根据协议和端口号选择Service和Engine

2. 根据域名选定 Host,通过 URL 中的域名去查找相应的 Host 容器

3. 根据 URL 的路径来匹配相应的 Web 应用的路径,找到了Context 容器

4. 根据 URL 路径找到 Wrapper（Servlet）

# 请求处理全过程源码解析

![](assets/Pasted%20image%2020240114165834.png)

 1. NioEndpoint的doRun方法——进行握手连接,即开启Socket连接
	![](assets/Pasted%20image%2020240114165848.png)
	在该方法，会调用AbstractProtocol类的public AbstractEndpoint.Handler.SocketState process(SocketWrapperBase wrapper, SocketEvent status)方法，获取Socket状态。
	之后会传参SocketWrapper调用到Processor的实现类AbstractProcessorLight的process方法，因为Socket的状态是OPEN，则调用AbstractProcessorLight的子类Http11Processor的Service方法
	如下图，Http11Processor类的实例的Request此时还为null、
![](assets/Pasted%20image%2020240114165856.png)

2. 解析请求

在Http11Processor的Service方法中又调用Http11InputBuffer的parseRequestLine方法对byteBuffer进行解析——初始byteBuffer全为0，调用同类的fill方法，从SockeWrapper中读取进byteBuffer,然后进行解析

 nRead = socketWrapper.read(block, this.byteBuffer);

该语句调用SocketWrapper的read方法获取到请求的编码。

再在parseRequestLine中：

![](assets/Pasted%20image%2020240114171316.png)

如上图的byteBuffer中的数字，根据ASCII码对应过来如下（前半部分）：
```
GET /test/see HTTP/1.1

User-Agent: Apache/1.0.0 (https://apifox.com)

Accept: */*

Host: localhost
```

即，在Http11InputBuffer类的parseRequestLine方法中对请求报文进行解析，转化为字符串。

![](assets/Pasted%20image%2020240114171410.png)

如上图，在Http11Processor的实例中，Request已经被设置为我们的真实请求路径

在获取RequestInfo对象后——解析后将请求封装的对象，在Http11Processor的prepareRequest方法中还需要在处理请求前做一些预处理

![](assets/Pasted%20image%2020240114171416.png)

随后还要对插入过滤器做一些预设置。

Adapter

还是在Http11Processor的Service方法中，执行如下代码：

 this.getAdapter().service(this.request, this.response);

调用CoyoteAdapter类的Service方法，将Request和Response转换成ServletRequest和ServletResponse

![](assets/Pasted%20image%2020240114171439.png)

3. 容器传递调用

在CoyoteAdapter类的Service方法中有如下代码：

this.connector.getService().getContainer().getPipeline().getFirst().invoke(request, response);

它会调用Valve类的invok方法

![](assets/Pasted%20image%2020240114171448.png)

然后找到在StandardEngineValve类中的实现方法

![](assets/Pasted%20image%2020240114171456.png)

如上图可以看到，在对request简单设置后又调用了Valve的Invoke方法

在做了简单日志记录后，**Request和Response参数一路在Engine**、Host、Context、Wrapper调用下去。

StandardHostValve.invoke方法：

![](assets/Pasted%20image%2020240114171503.png)

StandardContextValve.invoke方法

![](assets/Pasted%20image%2020240114171509.png)

StandardWrapperValve.invoke方法

![](assets/Pasted%20image%2020240114171514.png)

在Wrapper的invoke方法中，设置了servlet的变量，随后将wrapper对象的Servlet赋值给该servlet对象：

![](assets/Pasted%20image%2020240114171525.png)

可以看到对应请求路径的Servlet的信息已经展示出来——包括Servlet的Mapping

Wrapper.accocate()方法是调用了StandardWrapper类的allocate方法

![](assets/Pasted%20image%2020240114171649.png)

![](assets/Pasted%20image%2020240114171654.png)

可以看到在执行完该语句后instance已经有了实例对象——即在这之前只有Servlet的信息，但是没有对应Servlet的实例，这里是将实例创建出来。

4. 构造FilterChain

ApplicationFilterChain filterChain = ApplicationFilterFactory.createFilterChain(request, wrapper, servlet);

该函数用于创建一个应用过滤器链，并根据请求的路径和servlet名称添加相应的过滤器配置——根据过滤器配置，添加相应的过滤器到过滤器链，没有自定义过滤器会添加ApplicationFilter实例为过滤器

![](assets/Pasted%20image%2020240114171704.png)

![](assets/Pasted%20image%2020240114171708.png)

5. doFilter并执行Servlet的Service方法

在构建FilterChain后返回到StandardWrapperValve.invoke方法中，最终执行filterChain.doFilter方法——经过条件判断后会到internalDoFilter方法

![](assets/Pasted%20image%2020240114171714.png)

显示执行FilterChain的每个Filter的doFilter方法，随后再执行获取到的Servlet的service方法

![](assets/Pasted%20image%2020240114171720.png)

在HTTPServlet类中的Service，先是将ServletRequest和ServletResponse转换为HttpRequest和HttpResponse，然后再调用本类中的重载service方法——根据请求方法不同调用不同的方法——此时调用的就是自定义Servlet的重写方法

![](assets/Pasted%20image%2020240114171725.png)

6. 结束

回到ApplicationFilterChain的internalDoFilter方法，代表已经执行完毕Servlet的Service方法

随后回到StandardWrapperValve类的invoke方法中，将FilterChain、Servlet释放，再随后都是对资源释放和对Response进行完善等等

返回到Context、Host、Engine容器，再返回调用它们的方法……

至此请求的处理结束

有必要一提的是在进入该类的invoke方法时记录下了当前时间

*long t1 = System.currentTimeMillis();*

在该方法结束时又记录下时间，相差求得Request的处理时间