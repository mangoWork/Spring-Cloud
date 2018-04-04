## 简介

尽管Spring Cloud带有“Cloud”字样，但它并不是云计算解决方案，而是在Spring Boot基础上构建的，用于快速构建分布式系统的通用模式的工具集。
使用Spring Cloud开发的应用程序非常适合在Docker或者Paas上部署，所以又叫做云原生应用（Cloud Native Application），云原生（Cloud Native）可以简单接力位面向云环境的软件架构

## 特点

- 以Spring Boot 为基础构建的，所以约定优于配置
- 适用于各种环境。开发、部署在PC Server或各种云环境（例如阿里云、AWS等）均可
- 隐藏了组件的复杂性，并提供声明式、无xml的配置方式
- 开箱即用，快速启动
- 轻量级的组件。Spring Cloud整合的组件大多比较轻量。例如Eureka、Zuul，等等，都是各自领域轻量级的实现
- 组件丰富，功能齐全。Spring Cloud为微服务架构提供了非常完整的支持
- 选型中立、丰富
- 灵活。Spring Cloud的组成部分都是解耦的，开发人员可按需灵活挑选技术选型

## Spring Cloud的技术储备

Spring Cloud并不是面向零基础开发人员的，他有一定的学习曲线


- 语言基础：Spring Cloud是一个基于java语言的工具套件，所以学习它需要一定的java基础
- Spring Boot：Spring Cloud是基于Spring Boot构建的，因此它延续了Spring Boot的契约模式以及开发方式
- 项目管理与构建工具：目前业界比较主流的项目管理与构建工具有Maven和Gradle等