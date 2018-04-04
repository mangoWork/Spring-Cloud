### Spring Cloud Sleuth整合Zipkin

#### Zipkin简介

Zipkin是Twitter的一个开源项目，它基于Google Dapper实现。我们可以使用它来收集各个服务器上请求链路的跟踪数据，并通过它提供的REST API接口来辅助我们查询跟踪数据以实现对分布式系统的监控程序，从而及时地发现系统中出现的延迟升高问题并找出系统性能瓶颈的根源。除了面向开发的API接口之外，它也提供了方便的UI组件来帮助我们直观的搜索跟踪信息和分析请求链路明细，比如：可以查询某段时间内各用户请求的处理时间等。

##### 结构图
![](image/zipkin-architecture.png)

##### 核心组件

- Collector：收集器组件，它主要用于处理从外部系统发送过来的跟踪信息，将这些信息转换为Zipkin内部处理的Span格式，以支持后续的存储、分析、展示等功能。
- Storage：存储组件，它主要对处理收集器接收到的跟踪信息，默认会将这些信息存储在内存中，我们也可以修改此存储策略，通过使用其他存储组件将跟踪信息存储到数据库中。
- RESTful API：API组件，它主要用来提供外部访问接口。比如给客户端展示跟踪信息，或是外接系统访问以实现监控等。
- Web UI：UI组件，基于API组件实现的上层应用。通过UI组件用户可以方便而有直观地查询和分析跟踪信息。

#### 创建Zipkin Server

- 1 . 创建Spring Boot 项目，pom.xml中引入以下依赖

```
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.10.RELEASE</version>
    <relativePath/>
  </parent>


  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
      <groupId>io.zipkin.java</groupId>
      <artifactId>zipkin-server</artifactId>
      <version>1.23.2</version>
    </dependency>
    <dependency>
      <groupId>io.zipkin.java</groupId>
      <artifactId>zipkin-autoconfigure-ui</artifactId>
      <version>1.23.2</version>
    </dependency>

  </dependencies>
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Dalston.SR5</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>
```

- 2 . 在启动类中添加注解

```
@SpringBootApplication(exclude={DataSourceAutoConfiguration.class,HibernateJpaAutoConfiguration.class})
@EnableZipkinServer
public class App 
{
    public static void main( String[] args )
    {
        SpringApplication.run(App.class, args);
    }
}
```

- 3 . 在application.properties文件中添加配置信息

```
spring.application.name=zipkin-server
server.port=9411
```

- 4 . 启动项目，浏览器打开localhost:9411，可见Zipkin管理界面

![](image/zipkin-ui.png)

#### trace-1和trace-2引入和配置Zipkin服务

- 1 . 在trace-1和trace-2项目中增加以下依赖

```
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-sleuth-zipkin</artifactId>
</dependency>
```

- 2 . 在trace-1和trace-2的application.properties中增加Zipkin Server的配置信息

```
spring.zipkin.base-url=http://localhost:9411
```

- 3 . 重新启动trace-1和trace-2项目，请求http://localhost:9101/trace-1，多次直到日志信息第四个值为true时，刷新zipkin管理页面进行搜索

![](image/zipkin-trace-1.png)
