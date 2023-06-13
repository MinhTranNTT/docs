# 说说Soring Boot的起步依赖

‍

解题思路

得分点 starter配置,约定大于配置 

‍

标准回答 

Spring Boot 将日常企业应用研发中的各种场景都抽取出来,做成一个个的 starter（启动器）,starter 中整合了该场景下各种可能用到的依赖,用户只需要在 Maven 中引入 starter 依赖,SpringBoot 就能自动扫描到要加载的信息并启动相应的默认配置。

starter 提供了大量的自动配置,让用户摆脱了处理各种依赖和配置的困扰。

所有这些 starter 都遵循着约定成俗的默认配置,并允许用户调整这些配置,即遵循“约定大于配置”的原则。 那么我们看构建的项目的 pom.xml 文件中的starter配置。

```xml
<dependency>
    <groupid>org.springframework.boot</groupid>
    <artifactid>spring-boot-starter-web</artifactid>
</dependency>
```

 以 `spring-boot-starter-web`​ 为例,它能够为提供 Web 开发场景所需要的几乎所有依赖,因此在使用 Spring Boot 开发 Web 项目时,只需要引入该 Starter 即可,而不需要额外导入 Web 服务器和其他的 Web 依赖。

‍

加分回答 

有时在引入starter时,我们并不需要指明版本（version）,这是因为starter版本信息是由 `spring-boot-starter-parent`​（版本仲裁中心） 统一控制的。

‍
