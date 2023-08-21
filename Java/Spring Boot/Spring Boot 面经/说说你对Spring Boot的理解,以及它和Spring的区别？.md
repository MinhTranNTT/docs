# 说说你对Spring Boot的理解,以及它和Spring的区别？

‍

解题思路

得分点 Spring Boot与Spring 的关系 、Spring Boot的主要功能 、Spring Boot的优点 

标准回答 

其实从本质上来说,Spring Boot就是Spring,它帮你完成了一些Spring Bean配置。Spring Boot使用“习惯优于配置”的理念让你的项目快速地运行起来,使用Spring Boot能很快的创建一个能独立运行、准生产级别、基于Spring框架的项目。 

但Spring Boot本身不提供Spring的核心功能,而是作为Spring的脚手架框架,达到快速构建项目,预设第三方配置,开箱即用的目的。

Spring Boot有很多优点,具体如下： 

- 可以快速构建项目 

- 可以对主流开发框架的无配置集成 

- 项目可独立运行,无需外部依赖Servlet容器 

- 提供运行时的应用监控 

- 可以极大地提高开发、部署效率 

- 可以与云计算天然集成 

‍

加分回答 

Spring Boot 的核心功能： 

1. 自动配置 针对很多Spring应用程序常见的应用功能,Spring Boot能自动提供相关配置。 

2. 起步依赖 Spring Boot通过起步依赖为项目的依赖管理提供帮助。起步依赖其实就是特殊的Maven依赖和Gradle依赖,利用了传递依赖解析,把常用库聚合在一起,组成了几个为特定功能而定制的依赖。 

3. 端点监控 Spring Boot 可以对正在运行的项目提供监控。

‍
