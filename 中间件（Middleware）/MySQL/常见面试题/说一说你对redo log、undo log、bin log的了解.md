# 说一说你对redo log、undo log、bin log的了解 

bin log是在服务层实现的；redo log和undo log是在引擎层实现的，且是innodb引擎独有的，主要和事务相关。

bin log中记录的是整个mysql数据库的操作内容，对所有的引擎都适用，包括执行DDL、DML，可以用来进行数据库的恢复及控制。

redo log中记录的是要更新的数据，比如一条数据已提交成功，并不会立即同步到磁盘，而是记录到redo log中，等待合适的时机再刷盘，为了实现事务的持久性。

undo log中记录的是当前操作的相反操作，如一条insert语句在undo log中会对应一条delete语句，在任务回滚时会用到undo log,实现事务的原子性，同时会用在MVCC中，undo会有一条记录的多个版本，用在快照读中。

‍
