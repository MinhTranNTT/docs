# hibernate 对象有哪些状态？

临时/瞬时状态：直接 new 出来的对象，该对象还没被持久化（没保存在数据库中），不受 Session 管理。  
持久化状态：当调用 Session 的 save/saveOrupdate/get/load/list 等方法的时候，对象就是持久化状态。  
游离状态：Session 关闭之后对象就是游离状态。  

‍
