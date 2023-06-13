# hibernate 实体类可以被定义为 final 吗？

实体类可以定义为 final 类，但这样的话就不能使用 hibernate 代理模式下的延迟关联提供性能了，所以不建议定义实体类为 final。

‍
