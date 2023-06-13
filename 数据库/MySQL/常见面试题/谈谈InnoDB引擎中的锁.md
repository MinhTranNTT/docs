# 谈谈InnoDB引擎中的锁 

[这次终于懂了，InnoDB的七种锁（收藏） - 腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1822439#:~:text=%E6%80%BB%E7%9A%84%E6%9D%A5%E8%AF%B4%EF%BC%8CInnoDB%E5%85%B1%E6%9C%89%20%E4%B8%83%E7%A7%8D%E7%B1%BB%E5%9E%8B%E7%9A%84%E9%94%81%20%EF%BC%9A%20%EF%BC%881%EF%BC%89%E8%87%AA%E5%A2%9E%E9%94%81%20%28Auto-inc%20Locks%29%EF%BC%9B%20%EF%BC%882%EF%BC%89%E5%85%B1%E4%BA%AB%2F%E6%8E%92%E5%AE%83%E9%94%81,%28Shared%20and%20Exclusive%20Locks%29%EF%BC%9B%20%EF%BC%883%EF%BC%89%E6%84%8F%E5%90%91%E9%94%81%20%28Intention%20Locks%29%EF%BC%9B)

‍
