# DTO与VO的区别

[DTO与VO的区别 - 剑轩的专栏 - TNBLOG](http://www.tnblog.net/aojiancc2/article/details/2396#:~:text=VO%EF%BC%88View%20Object%EF%BC%89%EF%BC%9A%20%E8%A7%86%E5%9B%BE%E5%AF%B9%E8%B1%A1%EF%BC%8C%E7%94%A8%E4%BA%8E%E5%B1%95%E7%A4%BA%E5%B1%82%EF%BC%8C%E5%AE%83%E7%9A%84%E4%BD%9C%E7%94%A8%E6%98%AF%E6%8A%8A%E6%9F%90%E4%B8%AA%E6%8C%87%E5%AE%9A%E9%A1%B5%E9%9D%A2%EF%BC%88%E6%88%96%E7%BB%84%E4%BB%B6%EF%BC%89%E7%9A%84%E6%89%80%E6%9C%89%E6%95%B0%E6%8D%AE%E5%B0%81%E8%A3%85%E8%B5%B7%E6%9D%A5%E3%80%82,DTO%EF%BC%88Data%20Transfer%20Object%EF%BC%89%EF%BC%9A%20%E6%95%B0%E6%8D%AE%E4%BC%A0%E8%BE%93%E5%AF%B9%E8%B1%A1%EF%BC%8C%E6%B3%9B%E6%8C%87%E7%94%A8%E4%BA%8E%E5%B1%95%E7%A4%BA%E5%B1%82%E4%B8%8E%E6%9C%8D%E5%8A%A1%E5%B1%82%E4%B9%8B%E9%97%B4%E7%9A%84%E6%95%B0%E6%8D%AE%E4%BC%A0%E8%BE%93%E5%AF%B9%E8%B1%A1%E3%80%822019%E5%B9%B44%E6%9C%8826%E6%97%A5)

[一篇文章讲清楚VO，BO，PO，DO，DTO的区别 - 简书 (jianshu.com)](https://www.jianshu.com/p/072304c3dfb7)

## ****概念****

 VO（View Object）：

      视图对象，用于展示层，它的作用是把某个指定页面（或组件）的所有数据封装起来。

 DTO（Data Transfer Object）：

      数据传输对象，泛指用于展示层与服务层之间的数据传输对象。

 PO（Persistent Object）：

      持久化对象,就是和数据库保持一致的对象

‍

BO（ Business Object）：业务对象。 由Service层输出的封装业务逻辑的对象。

DO（ Data Object）：与数据库表结构一一对应，通过DAO层向上传输数据源对象。 == PO  == Entity

DTO（ Data Transfer Object）：数据传输对象，Service或Manager向外传输的对象。

AO（ Application Object）：应用对象。 在Web层与Service层之间抽象的复用对象模型，极为贴近展示层，复用度不高。

POJO（ Plain Ordinary Java Object）：在本手册中， POJO专指只有setter/getter/toString的简单类，包括DO/DTO/BO/VO等。

Query：数据查询对象，各层接收上层的查询请求。 注意超过2个参数的查询封装，禁止使用Map类来传输。

‍
