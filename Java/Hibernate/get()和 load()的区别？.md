# get()和 load()的区别？

* 数据查询时，没有 OID 指定的对象，get() 返回 null；load() 返回一个代理对象。
* load()支持延迟加载；get() 不支持延迟加载。

‍
