# RabbitMQ 怎么实现延迟消息队列？

延迟队列的实现有两种方式：

* 通过消息过期后进入死信交换器，再由交换器转发到延迟消费队列，实现延迟功能；
* 使用 `RabbitMQ-delayed-message-exchange`​ 插件实现延迟功能。

‍
