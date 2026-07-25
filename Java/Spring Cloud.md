## **<font style="color:rgb(51,51,51);">什么是SpringCloud？</font>**
Spring Cloud 是一套基于 Spring Boot 的微服务开发框架，它提供了构建分布式系统所需的一系列组件，用于解决微服务之间的通信、服务注册与发现、配置管理、负载均衡、网关、熔断限流等问题。

Spring Boot 更侧重于开发单个微服务，而 Spring Cloud 更侧重于管理多个微服务之间的协作。在实际项目中，我们通常会使用 Nacos 作为注册中心和配置中心，OpenFeign 实现服务间调用，Spring Cloud Gateway 作为网关，Spring Cloud LoadBalancer 实现客户端负载均衡，并结合 Sentinel 或 Resilience4j 提供熔断和限流能力，从而构建一个稳定、可扩展的微服务系统。  



常见的功能：

| 功能 | 作用 | 常见组件 |
| :---: | :---: | :---: |
| 服务注册与发现 | 找到其他微服务 | Nacos |
| 服务调用 | 微服务之间调用 | OpenFeign |
| 负载均衡 | 自动选择服务器 |  Spring Cloud LoadBalancer  |
| 熔断降级 | 防止雪崩 |  Sentinel 或 Resilience4j   |
| 限流 | 防止流量过大 | Spring Cloud Gateway |
| 配置中心 | 配置统一管理 | Nacos |
| 网关 | 所有请求统一入口 |  Spring Cloud Gateway    |
| 链路追踪 | 查看请求经过哪些服务 |  Micrometer Tracing   |






## **<font style="color:rgb(51,51,51);">SpringBoot和SpringCloud的区别？</font>**
Spring Boot 和 Spring Cloud 并不是替代关系，而是**基础框架和微服务框架的关系**。

+ **Spring Boot** 主要用于**快速开发单个 Spring 应用**，它通过自动配置（Auto Configuration）和 Starter 机制，简化了 Spring 项目的搭建和开发。 
+ **Spring Cloud** 则是在 Spring Boot 的基础上，为微服务架构提供了一整套解决方案，例如服务注册与发现、配置中心、服务调用、网关、熔断限流、链路追踪等。

| **对比项** | **Spring Boot** | **Spring Cloud** |
| --- | --- | --- |
| **定位** | 快速开发单个 Spring 应用 | 微服务治理框架 |
| **解决问题** | 简化开发和配置 | 管理多个微服务之间的通信与协作 |
| **核心功能** | 自动配置、Starter、内嵌服务器 | 服务注册、配置中心、负载均衡、网关、熔断等 |
| **使用场景** | 单体应用、单个微服务 | 大型分布式微服务系统 |
| **两者关系** | 可独立使用 | 基于 Spring Boot，不能脱离 Spring Boot |








## Spring Cloud 和 Spring Cloud Alibaba 有什么区别？  
Spring Cloud 是 Spring 官方提供的微服务框架，定义了微服务治理的一整套规范，包括服务注册与发现、服务调用、配置管理、网关、负载均衡等能力。

Spring Cloud Alibaba 则是在 Spring Cloud 基础上，由阿里巴巴提供的一套微服务组件实现，它遵循 Spring Cloud 的开发规范，但将部分组件替换为了阿里生态的实现。例如，使用 Nacos 作为注册中心和配置中心，使用 Sentinel 实现限流和熔断，并提供 RocketMQ、Seata 等组件支持。相比之下，Spring Cloud Alibaba 更符合国内企业的使用习惯，社区活跃、文档完善，因此在国内项目中应用非常广泛，而 Spring Cloud 本身则更偏向于提供统一的微服务框架和标准。  





## 什么是微服务？它和单体架构有什么区别？
微服务是一种架构风格，它将一个大型应用按照业务功能拆分成多个独立的小服务，每个服务都可以独立开发、部署和扩展，服务之间通过 HTTP、RPC 或消息队列等方式进行通信。相比单体架构，微服务具有高内聚、低耦的特点，更适合大型项目和多人协作开发。

单体架构是将所有业务模块打包成一个应用统一部署，结构简单、开发和运维成本低，适合小型项目；但随着系统规模增大，会出现代码耦合高、部署影响范围大、无法按需扩容等问题。微服务虽然提高了系统的可扩展性和维护性，但也带来了服务治理、分布式事务、配置管理、监控和运维等新的挑战，因此通常会结合 Spring Cloud、Nacos、Gateway、OpenFeign 等组件来解决这些问题。

| 对比项 | 单体架构 | 微服务架构 |
| :---: | :---: | :---: |
| 项目结构 | 一个项目 | 多个项目 |
| 部署方式 | 一起部署 | 独立部署 |
| 扩容方式 | 整个系统扩容 | 按服务扩容 |
| 数据库 | 一般共享一个数据库 | 通常每个服务独立数据库 |
| 开发效率 | 小项目效率高 | 大项目协作更好 |
| 服务通信 | 方法调用 | HTTP、RPC、消息队列等 |
| 技术栈 | 一般统一 | 可以根据需要选择（需权衡维护成本） |
| 故障影响 | 一个模块可能影响整个系统 | 单个服务故障影响相对可控（需配合熔断、降级等治理） |
| 运维复杂度 | 简单 | 较复杂，需要服务治理 |






## **<font style="color:rgb(51,51,51);">SpringCloud有什么优势？</font>**
Spring Cloud 的优势主要体现在它为微服务提供了一整套成熟的解决方案。

首先，它提供了服务注册与发现、服务调用、负载均衡、配置中心、API 网关以及熔断限流等组件，帮助开发者快速搭建微服务架构。其次，它与 Spring Boot 无缝集成，开发和配置都比较简单，上手成本较低。

此外，Spring Cloud 支持通过服务名进行调用，避免了硬编码 IP 地址；通过 Spring Cloud LoadBalancer 实现负载均衡，提高系统的并发能力；结合 Sentinel 或 Resilience4j 实现熔断和降级，提升系统的稳定性；再配合 Nacos 配置中心，可以实现配置的集中管理和动态刷新。因此，Spring Cloud 能够显著降低微服务开发和治理的复杂度，提高系统的可扩展性、可维护性和高可用性。  

| 优势 | 说明 |
| :---: | :---: |
| 微服务治理完善 | 提供注册中心、网关、配置中心等组件 |
| 开发效率高 | 与 Spring Boot 集成，配置简单 |
| 服务发现 | 通过服务名调用，无需硬编码 IP |
| 负载均衡 | 自动选择服务实例 |
| 高可用 | 支持熔断、限流、降级 |
| 配置集中管理 | 配置统一维护，支持动态刷新 |
| 网关能力 | 路由、认证、限流等统一处理 |
| 易扩展 | 组件可按需替换和组合 |




## Spring Cloud 的工作流程  
Spring Cloud 的工作流程一般是：

首先，各个微服务启动后会将自己的服务名、IP、端口等信息注册到 Nacos 注册中心。

客户端发起请求后，首先进入 Spring Cloud Gateway，Gateway 根据路由规则从 Nacos 获取目标服务实例并转发请求。

如果订单服务需要调用商品服务，则通过 OpenFeign 发起远程调用，OpenFeign 再从 Nacos 获取商品服务的所有实例列表，由 Spring Cloud LoadBalancer 选择一个实例完成访问。

商品服务处理完成后返回结果给订单服务，最终再返回给客户端。

如果调用过程中服务异常，可以通过 Sentinel 或 Resilience4j 实现限流、熔断和降级，防止故障扩散。

同时，各微服务的配置可以统一存放在 Nacos 配置中心，实现集中管理和动态刷新。

整个流程体现了 Spring Cloud 对微服务通信、服务发现、负载均衡、网关和服务治理的支持。  

<!-- 这是一个文本绘图，源码为：flowchart TD

A[用户请求] --> B[Spring Cloud Gateway网关]

B --> C{权限认证}
C -->|通过| D[路由转发]
C -->|失败| E[返回错误]

D --> F[服务消费者 Consumer]

F --> G[服务发现 Discovery]

G --> H[注册中心 Nacos/Eureka]

H --> I[获取服务实例列表]

I --> J[Spring Cloud LoadBalancer负载均衡]

J --> K[选择服务实例]

K --> L[Feign远程调用]

L --> M[服务提供者 Provider]

M --> N[执行业务逻辑]

N --> O[(数据库)]

O --> N

N --> P[返回结果]

P --> L

L --> F

F --> B

B --> A


L -.调用失败.-> Q[Sentinel/Resilience4j熔断降级]

Q --> R[返回降级结果]


S[配置中心 Nacos Config] --> F
S --> M


M --> T[Seata事务协调]

T --> U[多个微服务事务管理] -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/62d7c112775477d6a0eeb60ca59c0c00.svg)

| 阶段 | 组件 | 作用 |
| --- | --- | --- |
| 1. 服务启动 | Spring Boot | 启动微服务应用 |
| 2. 服务注册 | Nacos / Eureka | 将服务名称、IP、端口注册到注册中心 |
| 3. 服务发现 | Nacos Discovery | 获取目标服务地址 |
| 4. 请求入口 | Gateway | 统一接收请求、路由、鉴权 |
| 5. 负载均衡 | Spring Cloud LoadBalancer | 从多个服务实例中选择一个 |
| 6. 服务调用 | Feign | 简化 HTTP/RPC 调用 |
| 7. 业务处理 | Provider服务 | 执行业务逻辑 |
| 8. 数据访问 | MyBatis/JPA | 操作数据库 |
| 9. 返回结果 | Feign + Gateway | 将结果返回给客户端 |
| 10. 服务保护 | Sentinel/Resilience4j | 熔断、限流、降级 |
| 11. 配置管理 | Nacos Config | 统一管理配置 |
| 12. 分布式事务 | Seata | 保证跨服务事务一致性 |








## 什么是 Spring Cloud Gateway？  
Spring Cloud Gateway 是 Spring Cloud 官方推出的 API 网关组件，用于微服务架构中的统一入口管理。它基于 Spring WebFlux 和 Reactor 异步非阻塞模型构建，主要负责请求路由、负载均衡、权限认证、限流、熔断、日志监控等功能。

在微服务架构中，客户端不需要直接访问多个微服务，而是先请求 Gateway，由 Gateway 根据请求路径、请求参数、请求 Header 等规则，将请求转发到对应的微服务。同时 Gateway 可以在请求到达服务之前和响应返回之后进行统一处理，例如 Token 校验、请求限流、日志记录等，从而实现服务治理和安全控制。

相比传统的 Servlet 网关 Zuul 1.x，Spring Cloud Gateway 采用响应式编程模型，性能更高，扩展性更好，目前已经成为 Spring Cloud 微服务体系中推荐使用的网关方案。

**核心功能：**

| 功能 | 作用 | 示例 |
| --- | --- | --- |
| 路由(Route) | 请求转发 | /user/** → 用户服务 |
| 断言(Predicate) | 匹配请求规则 | 路径、请求方法、Header |
| 过滤器(Filter) | 修改请求或响应 | 添加Token、日志 |
| 负载均衡 | 选择服务实例 | 调用 user-service |
| 权限认证 | 统一鉴权 | JWT Token验证 |
| 限流 | 保护服务 | 限制接口访问频率 |
| 熔断降级 | 防止服务雪崩 | 服务异常返回默认结果 |




### Gateway主要组件  
| 组件 | 作用 |
| --- | --- |
| RouteLocator | 保存路由信息 |
| RoutePredicateHandlerMapping | 寻找匹配路由 |
| FilteringWebHandler | 执行过滤器 |
| GlobalFilter | 全局过滤 |
| GatewayFilter | 局部过滤 |
| NettyRoutingFilter | 发送HTTP请求 |




### Gateway 如何实现权限认证？  
在微服务架构中，Spring Cloud Gateway 通常通过**过滤器**（Filter）实现权限认证。

客户端请求首先经过 Gateway，Gateway 中的全局过滤器会拦截请求，从请求头中获取 Token（例如 JWT），然后验证 Token 的合法性，包括 Token 是否存在、是否过期、签名是否正确等。如果认证通过，Gateway 会解析 Token 中的用户信息（如用户 ID、角色、权限），然后将用户信息传递给后端微服务；如果认证失败，则直接返回 401 或 403，不再转发请求。Spring Cloud Gateway 本身支持通过 Filter 对请求进行前置处理，也可以使用 OAuth2 的 Token Relay 将认证 Token 转发给下游服务。  

<!-- 这是一个文本绘图，源码为：flowchart TD

A[客户端请求] --> B[Spring Cloud Gateway]

B --> C[Gateway过滤器 GlobalFilter]

C --> D{是否需要认证}

D -->|否| E[直接路由转发]

D -->|是| F[获取请求Header中的Token]

F --> G{Token是否存在}

G -->|不存在| H[返回401 未认证]

G -->|存在| I[解析Token]

I --> J[验证Token合法性]

J --> K{Token是否有效}

K -->|无效| L[返回401 Token失效]

K -->|有效| M[获取用户信息]

M --> N[添加用户信息到请求Header]

N --> O[转发到业务服务]

O --> P[微服务执行业务逻辑]

P --> Q[返回结果]

Q --> A


%% Token校验
I --> R[认证服务/Auth Service]

R --> J -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/85d5693f8a61ff146aef003b812d4e49.svg)



```java
@Component
public class AuthGlobalFilter 
        implements GlobalFilter {


@Override
public Mono<Void> filter(
        ServerWebExchange exchange,
        GatewayFilterChain chain) {


    String token =
    exchange.getRequest()
    .getHeaders()
    .getFirst("Authorization");


    // 校验token

    return chain.filter(exchange);
}

}
```





### Spring Cloud Gateway 和 Nginx区别  
Spring Cloud Gateway 和 Nginx 都可以作为系统入口的网关，负责请求转发和流量控制，但是它们定位不同。

Nginx 更偏向基础设施层面的高性能反向代理服务器，主要负责网络层面的请求转发、负载均衡、静态资源处理以及高并发连接处理。

Spring Cloud Gateway 是微服务体系中的 API 网关，主要负责微服务场景下的业务路由、权限认证、限流、熔断、服务发现以及请求过滤。

> Nginx 负责最外层流量入口和高性能转发，Spring Cloud Gateway 负责微服务内部的业务网关能力。
>

| 区别 | Gateway | Nginx |
| --- | --- | --- |
| 定位 | 微服务网关 | 反向代理服务器 |
| 协议 | HTTP/WebSocket | HTTP/TCP |
| 服务发现 | 支持Nacos/Eureka | 需要额外配置 |
| 业务逻辑 | 强 | 弱 |
| 认证授权 | 支持 | 较弱 |
| 限流 | 支持 | 支持 |
| 动态路由 | 支持 | 较弱 |




### Gateway 的执行流程  
<!-- 这是一个文本绘图，源码为：flowchart TD
    %% 节点定义
    Client([1. 客户端 Client])
    GatewayIn[2. Gateway 接收请求]
    Mapping[3. Handler Mapping]
    Predicate{4. Predicate 路由断言}
    PreFilter[5. Pre Filter Chain 前置过滤器]
    LB[6. LoadBalancer 负载均衡]
    HttpClient[7. HTTP Client 转发]
    MicroService[8. 微服务 Service Provider]
    ServiceResp[9. 微服务返回响应]
    PostFilter[10. Post Filter Chain 后置过滤器]
    ClientEnd([11. 客户端接收响应])

    %% 流程连接
    Client -->|发送 HTTP 请求| GatewayIn
    GatewayIn --> Mapping
    Mapping -->|查找匹配 Route| Predicate
    
    Predicate -->|匹配成功| PreFilter
    Predicate -->|匹配失败| ClientEnd

    PreFilter -->|鉴权/限流/修改请求| LB
    LB -->|服务发现与节点选择| HttpClient
    HttpClient -->|转发请求| MicroService
    
    MicroService -->|执行业务并返回| ServiceResp
    ServiceResp --> PostFilter
    PostFilter -->|处理响应头/记录耗时| ClientEnd

    %% 子图分组展现层次结构
    subgraph Spring Cloud Gateway 核心流程
        GatewayIn
        Mapping
        Predicate
        PreFilter
        LB
        HttpClient
        PostFilter
    end -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/d3de0a8f4a745f9e4af0a50da296e18e.svg)

<!-- 这是一个文本绘图，源码为：sequenceDiagram
    autonumber
    actor Client as 客户端
    participant Gateway as Gateway 接收入口
    participant Mapping as Handler Mapping
    participant Predicate as Predicate 断言
    participant Filter as Filter Chain 过滤器
    participant LB as LoadBalancer
    participant HttpClient as HTTP Client
    participant Service as 目标微服务

    Client->>Gateway: 1-2. 发送 HTTP 请求 (/order/create)
    Gateway->>Mapping: 3. 查找匹配 Route
    Mapping->>Predicate: 4. 判断规则 (Path/Method/Header)
    
    alt 匹配成功
        Predicate->>Filter: 5. 执行前置过滤器 (认证/限流/日志)
        Filter->>LB: 6. 选择服务实例 (Nacos/Eureka)
        LB->>HttpClient: 获取具体实例 IP:Port
        HttpClient->>Service: 7-8. 转发请求并执行业务逻辑
        Service-->>HttpClient: 9. 返回处理结果
        HttpClient-->>Filter: 传递响应
        Filter->>Filter: 10. 执行后置过滤器 (耗时记录/修改 Header)
        Filter-->>Client: 11. 返回最终结果
    else 匹配失败
        Predicate-->>Client: 返回 404 Not Found
    end -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/c2c1d489793dc3073b2b2d48a6e5f5cf.svg)

| 执行步骤 | 核心组件 | 执行动作 | 作用 |
| --- | --- | --- | --- |
| 1 | 客户端（Client） | 发送 HTTP 请求 | 用户访问 Gateway，例如请求 `/order/create` |
| 2 | Gateway 接收请求 | 接收客户端请求 | 作为微服务统一入口，接收所有外部请求 |
| 3 | Handler Mapping（RoutePredicateHandlerMapping） | 查找匹配的 Route | 根据请求信息寻找对应的路由规则 |
| 4 | Predicate（路由断言） | 判断请求是否满足条件 | 根据 Path、Method、Header 等规则判断是否匹配路由 |
| 5 | Filter Chain（过滤器链） | 执行过滤器 | 执行认证、日志、限流、参数修改等前置处理 |
| 6 | LoadBalancer（Spring Cloud LoadBalancer） | 选择服务实例 | 从 Nacos/Eureka 获取服务列表，并选择一个实例 |
| 7 | HTTP Client（Reactor Netty） | 转发 HTTP 请求 | 将请求发送到目标微服务 |
| 8 | 微服务（Service Provider） | 执行业务逻辑 | 处理请求，例如查询数据库、调用其他服务 |
| 9 | 微服务返回响应 | 返回处理结果 | 将业务处理结果返回给 Gateway |
| 10 | Gateway Filter Chain（后置过滤器） | 执行响应过滤 | 对响应进行处理，例如添加 Header、记录耗时 |
| 11 | 客户端（Client） | 接收响应 | 获取最终返回结果 |




### Gateway 和 Feign 的区别  
| 对比 | Spring Cloud Gateway | Feign |
| --- | --- | --- |
| 定位 | API 网关 | 服务调用客户端 |
| 主要作用 | 统一接收外部请求 | 微服务之间远程调用 |
| 调用方向 | 用户 → 微服务 | 微服务 → 微服务 |
| 所处位置 | 系统入口层 | 业务服务内部 |
| 是否暴露给用户 | 是 | 否 |
| 是否负责路由 | 是 | 否 |
| 是否负责负载均衡 | 是（结合 LoadBalancer） | 是（结合 LoadBalancer） |
| 是否支持认证 | 支持 | 一般不负责 |
| 是否支持限流 | 支持 | 不负责 |
| 通信方式 | HTTP请求转发 | HTTP接口调用 |
| 典型场景 | 网关、鉴权、限流 | 服务间调用 |


 **底层实现区别：**

|  | Gateway | Feign |
| --- | --- | --- |
| 核心技术 | Spring WebFlux | 动态代理 |
| 网络模型 | 异步非阻塞 | 同步HTTP调用 |
| 底层客户端 | Reactor Netty | HTTP Client |
| 核心组件 | Route、Predicate、Filter | FeignClient、Encoder、Decoder |


<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/70336156/1784864568000-3633c157-7dfa-438e-96ba-faa05c1ddba6.png)



### Gateway 和 Spring Security 如何整合？  
Spring Cloud Gateway 和 Spring Security 整合，主要是让 Gateway 作为统一认证入口，通过 Spring Security 实现身份认证和权限校验。通常采用 OAuth2 Resource Server + JWT 的方案。

具体流程是：用户登录认证服务后获取 JWT Token，之后访问微服务时携带 Token 请求 Gateway。Gateway 集成 Spring Security 后，会在过滤器链中拦截请求，Spring Security 的认证过滤器会解析请求中的 JWT，并验证 Token 的合法性，包括签名、过期时间等。验证成功后，将用户信息封装成 Authentication 对象保存到 SecurityContext 中，后续 Gateway 可以根据用户角色或权限进行访问控制。如果认证失败，则直接由 Gateway 返回 401 或 403，不会转发到后端微服务。



添加依赖：

```xml
//安全框架
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

//支持JWT Token验证
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-resource-server</artifactId>
</dependency>
```

 配置 JWT 校验：

```yaml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: http://auth-server
```

 配置 SecurityWebFilterChain：

```java
@Configuration
@EnableWebFluxSecurity
public class SecurityConfig {


@Bean
SecurityWebFilterChain securityWebFilterChain(
        ServerHttpSecurity http) {


    return http
        .csrf(ServerHttpSecurity.CsrfSpec::disable)

        .authorizeExchange(exchange -> exchange

            // 放行登录接口
            .pathMatchers("/login")
            .permitAll()

            // 其他接口必须认证
            .anyExchange()
            .authenticated()
        )

        // JWT认证
        .oauth2ResourceServer(
            oauth2 ->
            oauth2.jwt()
        )

        .build();
}

}
```





## CAP 理论
+ **C（Consistency - 强一致性）：** 任何时刻，所有节点上的数据都是完全相同的。
+ **A（Availability - 高可用性）：** 客户端发起的每次请求都能得到非错误的响应（但不保证拿到的是最新数据）。
+ **P（Partition Tolerance - 分区容错性）：** 当节点间网络发生故障、形成网络隔离（分区）时，系统仍能继续运行。

| 组件 | CAP选择 | 原因 |
| --- | --- | --- |
| Nacos服务注册 | AP | 服务发现更关注可用 |
| Nacos配置中心 | CP | 配置必须准确 |
| Eureka | AP | 保证服务持续发现 |
| Zookeeper | CP | 保证节点一致 |
| Redis集群 | AP倾向 | 提高可用性 |
| MySQL主从复制 | AP倾向 | 允许延迟 |
| Seata事务 | CP倾向 | 保证事务一致 |






## BASE理论
BASE 理论是分布式系统中用于解决数据一致性问题的一种思想，它是对 CAP 理论中 AP 方案的延伸。

BASE 认为，在分布式系统中，由于网络分区不可避免，无法同时保证强一致性和高可用性，所以可以牺牲强一致性，采用最终一致性的方案。

BASE 由三个部分组成：**Basically Available**（基本可用）、**Soft State**（软状态）、**Eventually Consistent**（最终一致性）。

在实际业务中，系统不要求数据立即一致，而是允许短时间的数据不一致，通过消息重试、补偿机制等方式，最终达到数据一致。

例如电商下单场景，订单创建成功后，库存扣减可能通过消息异步完成，短时间内订单和库存可能不一致，但经过重试最终会保持一致。

**Basically Available： **系统出现故障时，允许损失部分可用性，但核心功能仍然可以使用。**  **

**Soft State：** 系统中的数据状态可以存在中间状态，而不是必须立即一致。**  **

**Eventually Consistent：** 经过一段时间后，所有节点的数据最终达到一致。  





## BASE 和 CAP 的关系  
|  | CAP | BASE |
| --- | --- | --- |
| 解决问题 | 分布式系统理论限制 | 实际工程实现 |
| 关注点 | 一致性和可用性选择 | 如何实现最终一致 |
| 提出内容 | 不能三者兼得 | 允许暂时不一致 |
| 关系 | 理论基础 | 工程方案 |






## BASE 和 ACID 的区别  
|  | ACID | BASE |
| --- | --- | --- |
| 应用场景 | 单机数据库事务 | 分布式系统 |
| 一致性 | 强一致 | 最终一致 |
| 事务范围 | 单数据库 | 多个服务 |
| 性能 | 较低 | 较高 |
| 实现方式 | 数据库事务 | 补偿、消息、重试 |






## 解释 Nacos 和 Eureka 的区别
Eureka 和 Nacos 都可以作为微服务的注册中心，实现服务注册与发现。

它们最大的区别在于，Eureka 主要只提供注册中心功能，而 Nacos 同时提供注册中心和配置中心功能，可以统一管理服务和配置。在一致性方面，Eureka 采用 AP 模式，更强调服务可用性；Nacos 同时支持 AP 和 CP 两种模式，可以根据业务场景选择。在生态方面，Eureka 属于 Netflix，近年来维护较少，而 Nacos 由阿里巴巴持续维护，与 Spring Cloud Alibaba 集成度高，支持动态配置、命名空间、集群管理等更多特性。因此，目前国内大多数新建 Spring Cloud 项目都会优先选择 Nacos。 

| 对比项 | Eureka | Nacos（临时实例） |
| --- | --- | --- |
| 心跳异常 | 进入自我保护，不立即删除 | 超时后直接删除 |
| 是否有自我保护 | ✅ 有 | ❌ 没有（临时实例） |
| 设计理念 | AP，优先保证可用性 | 临时实例偏 AP，但不采用 Eureka 的自我保护策略 |
| 数据一致性 | 允许短时间存在脏数据 | 更快剔除失效实例 |






### **<font style="color:rgb(51,51,51);">eureka自我保护机制是什么?</font>**
Eureka 自我保护机制是为了防止因网络抖动或短暂故障导致大量健康服务被误剔除。

当 Eureka 检测到最近一分钟内实际收到的心跳数低于预期阈值（默认约 85%）时，会进入自我保护模式。在该模式下，即使部分实例长时间没有发送心跳，也不会立即将其从注册中心剔除，而是继续保留实例信息，等待网络恢复。这体现了 Eureka **AP（可用性优先）** 的设计思想，代价是可能短时间内保留已经失效的实例信息。  

自我保护机制只能应对**短时间的网络异常**，如果长时间无法恢复，就需要人工介入或恢复网络，否则服务发现的准确性会越来越差。  

```yaml
eureka:
  server:
    enable-self-preservation: false
```



### Nacos 的 AP 和 CP 模式是如何实现的？什么时候选择 AP，什么时候选择 CP？  
在分布式系统中，由于必须保证分区容错（P），因此需要在一致性（C）和可用性（A）之间做权衡。

对于**服务注册**，Nacos 默认将服务注册为临时实例，采用 AP 模式，实例通过心跳维持状态，即使发生网络分区，也会优先保证服务可用，允许短时间内的数据不一致。

而对于**永久实例或配置管理**，Nacos 采用 CP 模式，底层基于 Raft 一致性协议，只有数据同步到多数节点后才认为写入成功，从而保证数据的一致性。因此，在实际应用中，服务注册更适合选择 AP，因为微服务更关注系统持续可用；而配置中心更适合选择 CP，因为配置必须保持一致，不能出现不同节点读取到不同配置的情况。  



### 什么是临时实例和永久实例  ？
 Nacos 中的实例分为临时实例和永久实例。

**临时实例**需要定时向 Nacos 发送心跳来维持存活状态，如果长时间没有收到心跳，Nacos 会自动将该实例从注册中心剔除。它采用 AP 模式，更强调系统的可用性，因此 Spring Cloud Alibaba 默认注册的都是临时实例。

**永久实例**则不依赖心跳，即使服务停止也不会自动删除，需要手动或通过接口移除。为了保证数据的一致性，永久实例采用 CP 模式，底层通过 Raft 一致性协议保证集群中的数据同步。在实际项目中，微服务注册通常使用临时实例，而配置管理等需要保证数据一致性的场景则更适合采用 CP 模式。  



### 如果 Nacos 宕机了，已经启动的微服务还能继续调用吗？为什么？  
如果 Nacos 宕机，已经启动的微服务通常仍然可以继续调用。

因为服务消费者在启动后，会从 Nacos 获取服务实例列表，并**缓存在本地**，后续 OpenFeign 或 Spring Cloud LoadBalancer 会优先使用本地缓存进行服务调用，而不是每次请求都访问 Nacos。因此，在 Nacos 短时间不可用的情况下，已有服务之间一般还能正常通信。

但是，这种能力是有限的。如果 Nacos 长时间不可用，新启动的服务无法获取服务列表，新注册的服务也无法注册到注册中心；同时，服务实例上下线的信息无法同步，本地缓存会逐渐过期，可能导致调用已经下线的实例。此外，依赖 Nacos 的配置动态刷新等功能也会失效。因此，生产环境通常会部署 Nacos 集群，提高注册中心的高可用性。



### Nacos 本地缓存保存在哪里?
 Nacos 客户端的本地缓存主要包括**内存缓存**和**磁盘缓存**两部分。

微服务启动后，会从 Nacos 获取服务实例列表，并将其保存在 JVM 内存中，后续 OpenFeign 或 Spring Cloud LoadBalancer 进行服务调用时，通常直接使用内存中的服务列表，而不是每次都访问 Nacos。同时，Nacos Client 还会将服务信息和配置快照保存到本地磁盘，形成 Snapshot 缓存。当 Nacos 短时间不可用或者客户端重启时，可以读取本地快照继续工作，提高系统的可用性。不过，本地缓存只是临时方案，Nacos 恢复后，客户端会重新同步最新的服务信息，避免长期使用过期数据。  

**磁盘缓存**：windows保存在`C:\Users\用户名\nacos\`；Linux保存在`${user.home}/nacos/`



### 如果缓存中的服务实例已经失效，如何避免一直调用失败？  
 如果本地缓存中的服务实例已经失效，为了避免持续调用失败，Spring Cloud 会通过多种机制共同解决。

首先，Nacos 会根据心跳或健康检查及时发现异常实例，并更新服务列表，客户端同步后会刷新本地缓存，不再选择失效实例。其次，Spring Cloud LoadBalancer 会从最新的可用实例中进行负载均衡；如果开启了 Spring Retry 或底层 HTTP 客户端的重试机制，调用失败后还可以自动切换到其他实例重试。此外，在实例持续不可用时，还可以结合 Sentinel 或 Resilience4j 实现熔断和降级，避免不断请求故障节点，防止故障扩散。因此，在实际生产环境中，通常会将**服务发现、负载均衡、重试、健康检查和熔断降级**结合起来，共同保证服务调用的稳定性。  





### 负载均衡的意义什么？
负载均衡就是将客户端请求按照一定策略分发到多台服务器，而不是集中到一台服务器处理。它的主要意义有几个方面：第一，**提高系统吞吐量**，多台服务器共同处理请求可以支撑更大的并发；第二，**防止单台服务器压力过大，提高资源利用率**；第三，**提高系统的高可用性**，当某个实例发生故障时，可以自动将请求转发到其他正常实例；第四，**支持横向扩容**，当业务增长时，只需要增加服务实例即可，不需要修改客户端代码；**第五，可以降低响应时间**，通过合理的调度算法把请求分配到负载较低的服务器。在微服务项目中，我一般会结合 Nacos 做服务注册发现，再使用 Spring Cloud LoadBalancer 或 Nginx 实现负载均衡。  



### 常见负载均衡算法
| 算法 | 特点 | 适用场景 |
| --- | --- | --- |
| **轮询** | 按顺序依次分配请求 | 服务器配置一致、请求处理时间相近的场景 |
| **加权轮询** | 根据服务器权重分配请求，性能好的服务器处理更多请求 | 服务器性能不同，如新旧服务器混合部署 |
| **随机** | 随机选择服务器处理请求 | 节点数量较多、对请求均衡要求不高的场景 |
| **加权随机** | 按权重随机选择服务器，性能好的服务器被选中的概率更高 | 云平台、服务器性能存在差异的场景 |
| **最少连接** | 优先选择当前连接数最少的服务器 | 长连接、WebSocket、文件下载、视频直播等请求耗时差异较大的场景 |
| **加权最少连接** | 综合考虑服务器权重和当前连接数分配请求 | 集群中服务器性能差异较大且连接数不均衡的场景 |
| **IP Hash** | 根据客户端 IP 计算 Hash，同一客户端固定访问同一台服务器 | Session 未共享、需要会话保持（Session Sticky）的场景 |
| **一致性哈希** | 相同请求尽量落到同一台服务器，节点变化时数据迁移少 | 分布式缓存（Redis）、CDN、分布式存储等需要减少数据迁移的场景 |
| **最短响应时间** | 优先选择响应时间最短的服务器 | API 网关、高性能 Web 服务、对响应速度要求高的场景 |


Spring Cloud LoadBalancer 默认使用的是：轮询算法。



### RoundRobinLoadBalancer 的轮询原理是什么？是否线程安全？
RoundRobinLoadBalancer 的轮询原理是维护一个全局递增的 AtomicInteger 计数器，每次请求先进行原子递增，再通过** position % 实例数** 计算目标实例下标，实现依次访问各个服务实例。由于使用 AtomicInteger 的 CAS 原子操作保证了计数器递增的线程安全，因此在高并发环境下也能正确完成轮询。同时，服务实例列表采用只读访问和替换更新的方式，避免了并发修改问题，因此整个负载均衡过程是线程安全的





###  如何修改 Spring Cloud LoadBalancer 的负载均衡算法？  
####  方法一：修改为随机算法  
```java
@Configuration
public class LoadBalancerConfig {

    @Bean
    ReactorLoadBalancer<ServiceInstance> randomLoadBalancer(
            Environment environment,
            LoadBalancerClientFactory factory) {

        String serviceId = environment.getProperty(
                LoadBalancerClientFactory.PROPERTY_NAME);

        return new RandomLoadBalancer(
                factory.getLazyProvider(serviceId,
                        ServiceInstanceListSupplier.class),
                serviceId);
    }
}
```

 然后指定某个服务使用该配置：  

```java
@Configuration
@LoadBalancerClient(
    name = "order-service",
    configuration = LoadBalancerConfig.class
)
public class OrderServiceConfig {
}
```

####  方法二：实现自己的负载均衡算法  
 只需要实现接口：  `ReactorServiceInstanceLoadBalancer`

```java
public class MyLoadBalancer
        implements ReactorServiceInstanceLoadBalancer {

    @Override
    public Mono<Response<ServiceInstance>> choose(
            Request request) {

        // 获取所有实例

        // 自定义选择逻辑

        return new DefaultResponse(instance);
    }
}
```

```java
@Bean
public ReactorLoadBalancer<ServiceInstance> myLoadBalancer(...) {

    return new MyLoadBalancer(...);
}
```

这样以后所有请求都会走： `MyLoadBalancer.choose()`

####  方法三：根据服务单独配置  
```java
@LoadBalancerClient(
        name = "pay-service",
        configuration = PayLoadBalancerConfig.class
)
```







###  Spring Cloud LoadBalancer 为什么要替代 Ribbon？  
 Ribbon 是 Netflix 提供的客户端负载均衡组件，但由于 Netflix 已停止维护 Ribbon，Spring Cloud 从 2020.x 版本开始将其移除，改用 Spring Cloud LoadBalancer。相比 Ribbon，Spring Cloud LoadBalancer 由 Spring 官方维护，能够更好地适配 Spring Boot 和 Spring Cloud，原生支持 WebClient 和响应式编程，扩展方式更加符合 Spring 的设计理念，因此成为新的官方默认负载均衡方案。  







## **<font style="color:rgb(51,51,51);">什么是服务熔断？什么是服务降级？</font>**
服务熔断和服务降级都是微服务中的容错机制。

**服务熔断**是指当某个下游服务连续失败、超时或异常率过高时，熔断器会暂时停止对该服务的调用，直接快速返回错误或执行备用逻辑，防止故障扩散，保护整个系统。熔断器通常有关闭（Closed）、打开（Open）和半开（Half Open）三种状态，并能自动探测服务是否恢复。

**服务降级**则是在服务不可用或系统压力较大时，主动关闭非核心功能或返回默认值、缓存数据、静态页面等简化结果，以保证核心业务能够正常运行。例如推荐服务不可用时返回热门商品，短信发送失败时先完成订单，稍后补发短信。

可以理解为：**熔断决定"不再调用"，降级决定"返回什么"**。两者通常配合使用，熔断发生后执行降级逻辑，从而提升系统的稳定性和可用性。

###  熔断器三个状态  
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/70336156/1784681398544-4d576943-0302-473a-bf08-9d3a5ae99d00.png)



###  熔断器为什么能防止雪崩？ 
熔断器能够防止雪崩，是因为它采用了**快速失败**（Fail Fast）机制。

当下游服务连续超时或失败达到设定阈值时，熔断器会进入打开（Open）状态，后续请求不再继续调用故障务，而是立即返回错误或降级结果。这样可以避免大量线程长期阻塞等待，防止线程池耗尽，从而阻止故障沿调用链传播，引发整个系统雪崩。当经过一段时间后，熔断器会进入半开（Half-Open）状态，尝试少量请求检测服务是否恢复，如果恢复正常则关闭熔断，否则重新打开熔断。这种机制既保护了系统资源，又提高了整个微服务系统的稳定性。  



### 熔断、降级、限流有什么区别？  
**限流**是在流量过大时限制请求进入系统，防止系统被高并发压垮，保护的是当前服务；

**熔断**是在下游服务连续失败或超时达到阈值后，暂时停止调用该服务，采用快速失败机制，避免故障扩散和线程资源耗尽，保护的是整个调用链；

**降级**是在服务不可用或资源紧张时，主动关闭非核心功能或返回默认值、缓存数据等备用结果，保证核心业务仍然可用，提升用户体验。实际项目中，这三者通常会配合使用：高并发时先限流，下游异常时触发熔断，再通过降级返回兜底结果，从而提升系统的可用性和稳定性。  

| 对比项 | **限流（Rate Limiting）** | **熔断（CircuitBreaker）** | **降级（Fallback / Degradation）** |
| --- | --- | --- | --- |
| **目的** | 防止流量过大压垮系统 | 防止故障扩散，避免雪崩 | 保证系统仍能提供基本服务 |
| **触发条件** | 请求量超过设定阈值 | 下游服务连续失败、超时或失败率过高 | 服务异常、熔断、资源不足或系统压力过大 |
| **作用对象** | 当前服务 | 下游服务及整个调用链 | 用户体验和核心业务 |
| **处理方式** | 拒绝请求、排队、等待 | 暂停调用故障服务，快速失败 | 返回默认值、缓存数据或关闭非核心功能 |
| **是否调用下游服务** | 可以调用（只是限制进入系统的请求数） | 不调用（熔断打开后直接返回） | 一般不调用，直接返回兜底结果 |
| **解决的问题** | 高并发导致系统过载 | 服务故障导致线程耗尽、系统雪崩 | 服务异常导致用户完全无法使用 |
| **典型应用场景** | 秒杀、双十一、大促活动 | 支付服务故障、数据库超时、RPC调用失败 | 评论服务不可用、推荐服务异常、优惠券服务故障 |
| **典型实现** | Sentinel、Guava RateLimiter、Nginx、Gateway | Sentinel、Resilience4j、Hystrix（已停更） | Sentinel Fallback、Resilience4j Fallback、自定义兜底逻辑 |


| 技术 | 一句话解释 | 关键词 |
| --- | --- | --- |
| **限流** | 控制请求数量，防止系统被高并发压垮。 | **限制流量** |
| **熔断** | 服务故障时停止调用，防止故障扩散和雪崩。 | **快速失败** |
| **降级** | 服务异常时返回兜底结果，保证核心业务可用。 | **备用方案** |




###  降级有哪些实现方式？  
| 降级方式 | 实现方式 | 适用场景 | 示例 |
| --- | --- | --- | --- |
| **返回默认值** | 返回固定数据 | 非核心业务 | 评论服务异常，返回"暂无评论" |
| **返回缓存数据** | Redis、本地缓存 | 数据变化不频繁 | 商品详情、配置数据 |
| **调用备用服务** | 调用备用接口或备用机房 | 高可用系统 | 主支付接口异常，切换备用支付接口 |
| **关闭非核心功能** | 暂停部分业务 | 系统压力过大 | 关闭推荐、排行榜、评论功能 |
| **异步处理** | MQ、任务队列 | 对实时性要求不高 | 下单成功后异步发送短信 |
| **页面友好提示** | 返回错误提示页 | 用户体验优化 | "系统繁忙，请稍后重试" |


 **Sentinel 中的降级实现： **主要通过 Fallback（兜底） 来实现  

```java
@SentinelResource(
    value = "getUser",
    fallback = "fallback"
)
public User getUser(Long id) {
    return remoteService.getUser(id);
}

public User fallback(Long id, Throwable e) {
    return new User(-1L, "默认用户");
}
```

 Resilience4j 中的降级：

```java
@CircuitBreaker(name = "userService", fallbackMethod = "fallback")
public User getUser(Long id) {
    return remoteService.getUser(id);
}

public User fallback(Long id, Exception e) {
    return new User(-1L, "默认用户");
}
```



### Sentinel 和 Resilience4j 有什么区别？  
Sentinel 和 Resilience4j 都是微服务容错框架，但定位有所不同。

**Sentinel** 是阿里巴巴开源的流量治理框架，除了提供熔断、降级、限流外，还支持热点参数限流、系统自适应保护、授权规则以及 Dashboard 实时监控，并且可以结合 Nacos 实现动态规则管理，因此更适合 Spring Cloud Alibaba 体系。

**Resilience4j** 是轻量级容错框架，提供熔断、重试、限流、隔离、超时等模块化能力，与 Spring Cloud 官方生态集成更紧密，但没有官方 Dashboard，也不支持热点参数限流和系统保护等高级功能。因此，如果项目使用 Spring Cloud Alibaba，一般选择 Sentinel；如果使用 Spring Cloud 官方体系，则通常选择 Resilience4j。  

| 对比项 | Sentinel | Resilience4j |
| --- | --- | --- |
| **定位** | 流量治理框架 | 容错（Fault Tolerance）框架 |
| **适用生态** | Spring Cloud Alibaba | Spring Cloud（官方推荐） |
| **核心功能** | 限流、熔断、降级、热点参数限流、系统保护 | 熔断、重试、限流、线程隔离、超时 |
| **特色功能** | Dashboard、热点参数限流、系统自适应保护、动态规则 | Retry、Bulkhead（线程隔离）、TimeLimiter（超时控制） |
| **监控能力** | 自带 Dashboard，可实时监控和在线修改规则 | 无官方 Dashboard，通常结合 Micrometer + Prometheus + Grafana |
| **规则管理** | 支持 Nacos、Apollo 等动态配置 | 主要通过配置文件或 Spring Cloud Config 管理 |
| **适用场景** | 需要完善流量治理和可视化监控的大型微服务 | 需要轻量级容错、采用 Spring Cloud 官方生态的项目 |




### 项目中哪些场景适合使用熔断和降级？  
在项目中，**熔断**主要用于下游服务出现连续超时、异常或宕机等情况，例如支付服务、库存服务、数据库或第三方接口故障。当失败率达到阈值后，通过快速失败停止继续调用故障服务，避免线程资源耗尽和服务雪崩。**降级**则主要用于保证核心业务可用，当非核心功能异常时，可以返回默认值、缓存数据、关闭部分功能或采用异步处理。例如评论、推荐、排行榜等功能不可用时，可以返回空数据；短信、邮件发送失败时，可以改为异步重试；支付服务异常时，可以提示用户稍后重试。实际项目中，熔断和降级通常配合使用：**先通过熔断隔离故障，再通过降级提供兜底方案，从而提升系统的稳定性和可用性。**

| **场景** | **是否适合熔断** | **是否适合降级** | **原因** |
| --- | --- | --- | --- |
| 下游服务连续超时或宕机 | ✔ | ✔ | 熔断避免继续调用故障服务，降级返回默认结果。 |
| 数据库响应过慢 | ✔ | ✔ | 熔断防止线程长期阻塞，降级返回缓存数据。 |
| Redis 故障 | ✔ | ✔ | 熔断避免不断访问故障缓存，降级查询数据库或返回默认值。 |
| 第三方支付接口异常 | ✔ | ✔ | 熔断停止调用异常接口，降级提示"支付系统繁忙，请稍后重试"。 |
| 短信、邮件服务异常 | **✘（一般不需要）** | ✔ | 非核心业务，可异步重试或忽略，不影响主流程。 |
| 推荐、评论、排行榜服务异常 | **✘（一般不需要）** | ✔ | 属于非核心功能，可直接隐藏或返回空数据。 |
| 大促、秒杀期间系统压力过大 | ✔ | ✔ | 熔断异常服务，降级关闭非核心功能，优先保障核心业务。 |
| 第三方地图、天气等接口异常 | ✔ | ✔ | 可熔断后切换备用接口或返回缓存数据。 |






## **<font style="color:rgb(51,51,51);">说说 RPC 的实现原理</font>**
RPC  （Remote Procedure Call，远程过程调用）  的实现原理是利用**动态代理**将本地方法调用转换为远程调用。客户端调用代理对象时，代理会将服务名、方法名、参数等封装为 RPC 请求，并进行序列化，通过 TCP 或 HTTP 等协议发送到服务端。服务端反序列化后，利用反射调用真正的业务方法，再将执行结果序列化返回给客户端，客户端反序列化后返回最终结果。整个过程中通常还会结合服务注册与发现、负载均衡、超时控制以及熔断降级等机制，共同保证 RPC 调用的高可用和高性能。  

**涉及到的技术：**

| 技术 | 作用 |
| --- | --- |
| 动态代理 | 屏蔽远程调用细节，让调用方式像本地方法 |
| 序列化 / 反序列化 | 在对象和字节流之间转换 |
| 网络通信 | 将请求发送到远程服务并接收响应 |
| 服务注册与发现 | 找到目标服务实例（如 Nacos、Eureka） |
| 负载均衡 | 多实例时选择一个实例（如 RoundRobin） |
| 超时控制 | 避免请求长时间阻塞 |
| 熔断与降级 | 防止故障扩散（如 Sentinel、Resilience4j） |






## RPC 和 HTTP 有什么区别？  
RPC（Remote Procedure Call，远程过程调用）是一种调用方式，它的目标是让远程方法调用像本地方法调用一样简单；HTTP 则是一种应用层通信协议，主要用于浏览器与服务器或服务之间的数据传输。

RPC 可以基于 HTTP 实现，也可以基于 TCP、HTTP/2 等协议实现，因此 RPC 和 HTTP 并不是同一级别的概念。一般来说，RPC 更适合微服务内部调用，因为序列化效率高、通信开销小、性能更好；HTTP 更适合对外提供开放接口，兼容性和通用性更强。例如，我们的前端调用后端接口通常使用 HTTP，而微服务之间则常使Dubbo、gRPC 等 RPC 框架进行通信。  

| 对比项 | RPC | HTTP |
| --- | --- | --- |
| 本质 | 一种远程调用方式 | 一种应用层通信协议 |
| 调用方式 | 像调用本地方法 | 通过 URL 请求资源 |
| 面向对象 | 面向方法（Method） | 面向资源（Resource） |
| 底层协议 | TCP、HTTP/2、QUIC 等 | HTTP/1.1、HTTP/2、HTTP/3 |
| 数据格式 | Protobuf、Hessian、Kryo 等 | JSON、XML、表单等 |
| 性能 | 较高 | 相对较低 |
| 易用性 | 需要客户端 Stub、IDL 等 | 浏览器即可调用 |
| 适用场景 | 微服务内部调用 | 前后端交互、开放 API |






## RPC 一次调用经过了哪些步骤？  
业务调用 → 动态代理 → 服务发现 → 负载均衡 → 序列化 → 网络通信 → 反序列化 → 反射调用 → 执行业务 → 序列化返回 → 客户端反序列化 → 返回结果

<!-- 这是一个文本绘图，源码为：flowchart TD
    subgraph 客户端 [Client / 消费者]
        A[1. 业务调用] --> B[2. 动态代理]
        B --> C[3. 服务发现]
        C --> D[4. 负载均衡]
        D --> E[5. 序列化]
        E --> F[6. 网络通信 - 发送请求]
        L[11. 客户端反序列化] --> M[12. 返回结果]
    end

    subgraph 注册中心 [Registry]
        REG[(服务注册中心)]
    end

    subgraph 服务端 [Server / 提供者]
        F --> G[6. 网络通信 - 接收请求]
        G --> H[7. 反序列化]
        H --> I[8. 反射调用]
        I --> J[9. 执行业务]
        J --> K[10. 序列化返回]
    end

    %% 服务发现过程交互
    C -.-|查询可用节点| REG

    %% 服务端响应网络回传
    K -->|6. 网络通信 - 发送响应| L -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/3a01a67785f16129e2850787c1c787d1.svg)

| RPC 调用流程 | 作用 |
| --- | --- |
| 业务调用 | 开发者调用远程服务接口，代码形式与本地方法调用一致，例如 `stockService.reduce()`。 |
| 动态代理 | 拦截本地方法调用，将接口名、方法名、参数等封装成 RPC 请求，屏蔽远程调用细节。 |
| 服务发现 | 从注册中心（如 Nacos、Eureka）获取目标服务的可用实例地址（IP 和端口）。 |
| 负载均衡 | 当服务有多个实例时，根据负载均衡算法（如随机、轮询、一致性哈希等）选择一个实例进行调用。 |
| 序列化 | 将 `RpcRequest` 等 Java 对象转换为字节流，便于通过网络传输。 |
| 网络通信 | 使用 TCP、HTTP 或 HTTP/2 等协议，将请求发送到目标服务，并接收响应。 |
| 反序列化 | 服务端将收到的字节流还原为 `RpcRequest`对象，解析出接口、方法和参数。 |
| 反射调用 | 根据接口名、方法名和参数，通过反射调用真正的业务方法。 |
| 执行业务 | 执行业务逻辑，处理请求，并得到返回结果或异常。 |
| 序列化返回 | 将执行结果封装为 `RpcResponse`，再序列化成字节流返回客户端。 |
| 客户端反序列化 | 客户端将收到的字节流反序列化为 `RpcResponse`，解析返回值或异常。 |
| 返回结果 | 将最终结果返回给业务代码，使调用方感觉就像执行了一次本地方法调用。 |






## 什么是 feigin ？它的优点是什么？
OpenFeign 是 Spring Cloud 提供的声明式 HTTP 客户端。开发者只需定义接口并使用 `@FeignClient` 声明远程服务，就可以像调用本地方法一样完成 HTTP 调用。它底层通过动态代理生成请求，结合 Spring Cloud LoadBalancer 实现客户端负载均衡，通过 Nacos 等注册中心完成服务发现，并支持与 Sentinel 或 Resilience4j 集成实现熔断降级。相比 RestTemplate，OpenFeign 代码更简洁、开发效率更高、可维护性更好。  

| 优点 | 说明 |
| --- | --- |
| **声明式调用** | 只需要定义接口和注解，无需手写 HTTP 请求代码。 |
| **开发效率高** | 不需要自己使用 RestTemplate、HttpClient 等发送请求。 |
| **与 Spring MVC 无缝集成** | 支持 `@GetMapping`、`@PostMapping`、`@RequestParam`、`@PathVariable` 等注解。 |
| **支持负载均衡** | 与 Spring Cloud LoadBalancer 集成，可自动选择服务实例。 |
| **支持服务发现** | 配合 Nacos、Eureka 等注册中心，通过服务名调用服务。 |
| **支持熔断降级** | 可结合 Sentinel 或 Resilience4j 实现熔断、限流、降级。 |
| **支持请求拦截** | 可以统一添加 Token、请求头、日志等。 |






## Feign 的底层实现原理是什么？
OpenFeign 的底层核心是 **JDK 动态代理**。

项目启动时，Spring 会扫描 `@FeignClient` 接口，并为它创建一个代理对象。当我们调用 Feign 接口的方法时，实际上调用的是代理对象。代理对象会解析方法上的 `@GetMapping`、`@PostMapping` 等注解，构建 HTTP 请求，然后通过 Spring Cloud LoadBalancer 从注册中心选择一个服务实例，最后由底层 HTTP 客户端（如 OkHttp、Apache HttpClient 或 JDK HttpURLConnection）发送请求。服务返回结果后，Feign 再利用 Decoder 将响应数据反序列化为 Java 对象返回给调用方，因此开发者感觉就像在调用本地方法一样。  

| 流程 | 核心组件 | 作用 |
| --- | --- | --- |
| **1. 定义 Feign 接口** | @FeignClient | 声明远程服务接口，指定要调用的服务名称。 |
| **2. 创建代理对象** | **JDK 动态代理** | Spring 启动时扫描 `@FeignClient`，为接口创建代理对象，开发者调用接口实际上调用的是代理对象。 |
| **3. 拦截方法调用** | InvocationHandler | 拦截接口方法调用，开始执行 Feign 的远程调用流程。 |
| **4. 解析接口注解** | Contract | 解析 `@GetMapping`、`@PostMapping`、`@RequestParam`、`@PathVariable` 等 Spring MVC 注解，获取请求方式、URL、参数等信息。 |
| **5. 构建 HTTP 请求** | RequestTemplate、Encoder | 将方法参数封装为 HTTP 请求，请求体对象通过 `Encoder` 序列化（如 JSON）。 |
| **6. 服务发现** | Eureka / Nacos | 根据 `@FeignClient` 中配置的服务名，从注册中心获取所有可用服务实例。 |
| **7. 负载均衡** | Spring Cloud LoadBalancer | 根据负载均衡算法（默认轮询）从多个实例中选择一个目标实例。 |
| **8. 发送 HTTP 请求** | Client（OkHttp、Apache HttpClient、HttpURLConnection） | 将构建好的 HTTP 请求发送到目标服务。Feign 本身不负责网络通信，而是委托底层 HTTP 客户端完成。 |
| **9. 服务端处理请求** | Spring MVC Controller | 服务提供者接收请求，执行业务逻辑，并返回 HTTP 响应。 |
| **10. 接收 HTTP 响应** | Client | 接收服务端返回的响应数据（通常为 JSON）。 |
| **11. 响应反序列化** | Decoder | 将 HTTP 响应数据反序列化为 Java 对象。 |
| **12. 返回结果** | Feign 代理对象 | 将最终的 Java 对象返回给调用方，整个过程看起来就像调用本地方法一样。 |






## Feign 和 RestTemplate 有什么区别？  
Feign 和 RestTemplate 都可以用于微服务之间的 HTTP 调用，但它们的设计理念不同。RestTemplate 是 Spring 提供的 HTTP 客户端，需要开发者自己拼接 URL、封装请求、解析响应；而 Feign 是一个声明式 HTTP 客户端，只需要定义一个接口并添加注解即可完成远程调用。在 Spring Cloud 中，Feign 通常会结合 Spring Cloud LoadBalancer 实现客户端负载均衡，并可以方便地集成熔断、重试、日志等功能，因此在微服务项目中通常优先选择 Feign，而 RestTemplate 更多用于普通 HTTP 调用或一些特殊场景。  

```java
String result = restTemplate.getForObject(
    "http://user-service/user/1",
    String.class
);
```

```java
@FeignClient("user-service")
public interface UserFeign {

    @GetMapping("/user/{id}")
    User getById(@PathVariable Long id);

}
```





## Feign 如何传递请求头（如 Token）？  
Feign 默认支持传递请求头，可以通过 `**@RequestHeader**` 显式传递，也可以通过 `**RequestInterceptor**`**（请求拦截器）** 自动为所有请求添加统一的请求头，例如 Token、Authorization、TraceId 等。在实际项目中，如果只是个别接口需要传递请求头，通常使用 `@RequestHeader`；如果需要给所有 Feign 请求统一添加 Token、用户信息或链路追踪信息，则更推荐使用 `RequestInterceptor`，这样无需在每个接口中重复编写代码，可维护性更高。  



###  方法一：@RequestHeader（适合少量接口）  
```java
@FeignClient("user-service")
public interface UserFeign {

    @GetMapping("/user/{id}")
    User getUser(
        @PathVariable Long id,
        @RequestHeader("Authorization") String token
    );

}
```



###  方法二：RequestInterceptor  
```java
@Component
public class FeignTokenInterceptor
        implements RequestInterceptor {

    @Override
    public void apply(RequestTemplate template) {

        template.header(
            "Authorization",
            "Bearer xxxxx"
        );

    }
}
```









## Token 为什么不建议写在 @RequestHeader 参数里？
一般不建议把 Token 写在 @RequestHeader 参数里，因为这样会导致业务代码和认证逻辑耦合，每个接口都需要手动传递 Token，代码重复且容易遗漏。更好的方式是使用 Feign 的 RequestInterceptor 或 Spring MVC 的拦截器，在请求发送前统一将 Token 放入请求头，这样既降低了代码耦合度，也方便后续 Token 的统一管理和维护。

推荐做法：Feign RequestInterceptor

```java
@Component
public class TokenInterceptor implements RequestInterceptor {

    @Override
    public void apply(RequestTemplate template) {
        String token = getToken();
        template.header("Authorization", token);
    }
}
```

| 优点 | 说明 |
| --- | --- |
| **统一管理** | 所有 Feign 请求自动携带 Token，无需每个接口单独编写。 |
| **降低耦合** | 业务接口只关注业务参数，不关心认证信息。 |
| **减少重复代码** | 避免在每个接口和调用处重复传递 Token。 |
| **便于维护** | Token 名称、格式或获取方式变化时，只需修改拦截器。 |
| **降低遗漏风险** | 不容易因为忘记添加请求头导致认证失败。 |






## RequestTemplate 和RestTemplate 
RestTemplate 是 Spring 提供的 HTTP 客户端，用于主动发送 HTTP 请求；RequestTemplate 是 OpenFeign 内部的请求模板对象，用于保存和构建请求数据，本身并不发送请求。Feign 会先创建 RequestTemplate，再经过编码器转换为真正的 HTTP 请求，由底层 HTTP Client 发出。





## 什么是分布式事务？为什么会产生分布式事务  
分布式事务是指在分布式系统中，多个服务或者多个数据库节点共同完成一个业务操作时，需要保证这些操作要么全部成功，要么全部失败的一种事务机制。



### 分布式事务解决方案  
**① 两阶段提交（2PC）**  第一阶段： 准备提交  

   第二阶段： 确认提交  

**② 消息最终一致性（常用）  **

**③ Seata  **

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/70336156/1784959863602-ffec472f-4975-40e3-a170-82bd12383d43.png)

| 角色 |  作用   |
| --- | --- |
|  TC   |  事务协调器，负责管理全局事务状态   |
|  TM   |  事务管理器，负责开启、提交、回滚全局事务   |
|  RM   |  资源管理器，负责管理数据库分支事务   |








###  Seata 的执行流程是什么？  
<!-- 这是一个文本绘图，源码为：flowchart TD

    A[用户请求] --> B[服务A（TM）]

    B --> C[开启全局事务]
    
    C --> D[TC事务协调器]
    
    D --> E[返回 XID]

    E --> F[XID传播]

    F --> G[服务B（RM）]
    F --> H[服务C（RM）]

    G --> G1[开启本地事务]
    G1 --> G2[写业务表]
    G2 --> G3[记录 undo_log]
    G3 --> G4[注册分支事务]

    H --> H1[开启本地事务]
    H1 --> H2[写业务表]
    H2 --> H3[记录 undo_log]
    H3 --> H4[注册分支事务]

    G4 --> I[TC收集结果]
    H4 --> I

    I --> J{事务执行结果}

    J -->|全部成功| K[全局提交]
    J -->|存在失败| L[全局回滚]

    K --> M[RM提交事务]

    L --> N[RM根据 undo_log 恢复数据] -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/1f78a43bc3284b5d9ae833a03a36419e.svg)

<!-- 这是一个文本绘图，源码为：flowchart LR

TM[TM<br/>事务发起者]
TC[TC<br/>事务协调器]
RM1[RM<br/>订单服务]
RM2[RM<br/>库存服务]

TM -->|开启全局事务| TC

TC -->|返回XID| TM

TM -->|携带XID调用| RM1
TM -->|携带XID调用| RM2

RM1 -->|注册分支事务| TC
RM2 -->|注册分支事务| TC

TC -->|成功| COMMIT[提交所有分支事务]
TC -->|失败| ROLLBACK[根据undo_log回滚] -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/8451f31eac527d4b9064ef687f9d14e3.svg)



###  Seata 四种模式区别  
| 模式 | 原理 | 特点 | 使用场景 |
| --- | --- | --- | --- |
| AT | 自动生成undo_log补偿 | 无侵入，最常用 | MySQL业务系统 |
| TCC | Try Confirm Cancel | 手写补偿逻辑 | 高性能场景 |
| SAGA | 长事务状态机 | 异步执行 | 工作流 |
| XA | 数据库二阶段提交 | 强一致 | 数据库支持XA |




### Seata AT 模式为什么需要 undo_log？  
Seata AT 模式需要 undo_log，主要是因为 AT 模式采用的是自动补偿机制。在分布式事务执行过程中，各个分支事务会先提交本地数据库事务，如果后续全局事务失败，需要能够根据之前保存的数据恢复到事务开始前的状态。

undo_log 就是用来记录 SQL 执行前后的数据快照。当业务 SQL 执行时，Seata 会自动生成对应的回滚日志，例如更新前的数据和更新后的数据，并保存到 undo_log 表中。如果全局事务提交成功，undo_log 会被删除；如果事务失败，Seata 根据 undo_log 中保存的数据生成反向 SQL，将数据库恢复到原来的状态，从而实现分布式事务回滚。

**undo_log 如何保证不会误回滚？ ** 

 ① 全局事务ID XID  

 ② 回滚前数据校验  



### Seata AT模式和2PC有什么区别?
2PC 流程：

<!-- 这是一个文本绘图，源码为：sequenceDiagram
    autonumber
    participant C as 协调者 (Coordinator)
    participant P1 as 参与者 1
    participant P2 as 参与者 2

    rect rgb(240, 248, 255)
    note over C,P2: 第一阶段：准备阶段 (Prepare Phase)
    C->>P1: 发送 Prepare 请求
    C->>P2: 发送 Prepare 请求
    P1->>P1: 执行本地事务 (写 Undo/Redo 日志，锁定资源)
    P2->>P2: 执行本地事务 (写 Undo/Redo 日志，锁定资源)
    P1-->>C: 响应 VOTE_COMMIT (同意)
    P2-->>C: 响应 VOTE_COMMIT (同意)
    end

    rect rgb(240, 255, 240)
    note over C,P2: 第二阶段：提交阶段 (Commit Phase)
    C->>P1: 发送 Global Commit 请求
    C->>P2: 发送 Global Commit 请求
    P1->>P1: 正式提交事务，释放锁
    P2->>P2: 正式提交事务，释放锁
    P1-->>C: 响应 ACK
    P2-->>C: 响应 ACK
    end -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/9ecd1505cefed029bd13b90ef296c919.svg)

2PC 是基于数据库原生事务的强一致方案，而 Seata AT 模式是在 2PC 思想基础上进行了优化，通过 undo_log 实现自动补偿，从而降低资源锁定时间，提高性能。

2PC 在第一阶段会让所有参与者执行事务但不提交，进入准备状态，第二阶段由协调者统一决定提交或者回滚，因此整个过程会长期占用数据库资源。

而 Seata AT 模式中，各分支事务会先提交自己的本地事务，同时记录 undo_log，然后由 Seata TC 协调全局事务。如果失败，再根据 undo_log 进行反向补偿。

| 对比项 | 2PC | Seata AT |
| --- | --- | --- |
| **思想** | 两阶段提交 | 两阶段提交优化 |
| **一致性** | 强一致 | 最终一致（默认） |
| **事务提交方式** | 协调者统一提交 | 本地事务先提交 |
| **资源锁定** | 长时间锁定 | 短时间锁定 |
| **回滚方式** | 数据库事务rollback | undo_log补偿 |
| **性能** | 较低 | 较高 |
| **侵入性** | 需要数据库支持 | 业务代码基本无侵入 |
| **适用场景** | 强一致场景 | 互联网业务场景 |


