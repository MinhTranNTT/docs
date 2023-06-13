#  spring boot 核心配置文件是什么？

spring boot 核心的两个配置文件：

bootstrap (. yml 或者 . properties)：boostrap 由父 ApplicationContext 加载的，比 applicaton 优先加载，且 boostrap 里面的属性不能被覆盖；  
application (. yml 或者 . properties)：用于 spring boot 项目的自动化配置。  

‍
