# java找不到符号

报错:

```
D:\IDE\IdeaProjects\gulimall\renren-fast\src\main\java\io\renren\common\aspect\SysLogAspect.java:69:19
java: 找不到符号
  符号:   方法 setOperation(java.lang.String)
  位置: 类型为io.renren.modules.sys.entity.SysLogEntity的变量 sysLog
D:\IDE\IdeaProjects\gulimall\renren-fast\src\main\java\io\renren\common\aspect\SysLogAspect.java:75:15
java: 找不到符号
  符号:   方法 setMethod(java.lang.String)
  位置: 类型为io.renren.modules.sys.entity.SysLogEntity的变量 sysLog
D:\IDE\IdeaProjects\gulimall\renren-fast\src\main\java\io\renren\common\aspect\SysLogAspect.java:81:19
java: 找不到符号
  符号:   方法 setParams(java.lang.String)
  位置: 类型为io.renren.modules.sys.entity.SysLogEntity的变量 sysLog
D:\IDE\IdeaProjects\gulimall\renren-fast\src\main\java\io\renren\common\aspect\SysLogAspect.java:89:15
java: 找不到符号
  符号:   方法 setIp(java.lang.String)
  位置: 类型为io.renren.modules.sys.entity.SysLogEntity的变量 sysLog
D:\IDE\IdeaProjects\gulimall\renren-fast\src\main\java\io\renren\common\aspect\SysLogAspect.java:92:86
java: 找不到符号
  符号:   方法 getUsername()
  位置: 类 io.renren.modules.sys.entity.SysUserEntity
D:\IDE\IdeaProjects\gulimall\renren-fast\src\main\java\io\renren\common\aspect\SysLogAspect.java:93:15
java: 找不到符号
  符号:   方法 setUsername(java.lang.String)
  位置: 类型为io.renren.modules.sys.entity.SysLogEntity的变量 sysLog
D:\IDE\IdeaProjects\gulimall\renren-fast\src\main\java\io\renren\common\aspect\SysLogAspect.java:95:15
java: 找不到符号
  符号:   方法 setTime(long)
  位置: 类型为io.renren.modules.sys.entity.SysLogEntity的变量 sysLog
D:\IDE\IdeaProjects\gulimall\renren-fast\src\main\java\io\renren\common\aspect\SysLogAspect.java:96:15
java: 找不到符号
  符号:   方法 setCreateDate(java.util.Date)
  位置: 类型为io.renren.modules.sys.entity.SysLogEntity的变量 sysLog
```

问题：Lombok依赖注解@Data失效，暂不知什么问题导致

![img](https://img2022.cnblogs.com/blog/2402369/202211/2402369-20221111183007237-812685115.png)

解决：
升级版本

‍
