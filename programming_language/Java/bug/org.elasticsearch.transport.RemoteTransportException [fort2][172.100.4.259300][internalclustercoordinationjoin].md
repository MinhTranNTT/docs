# org.elasticsearch.transport.RemoteTransportException [fort2][172.100.4.259300][internalclustercoordinationjoin]

elasticsearch报错

<details>
<summary>点击查看代码</summary>

```
[2022-08-06T23:00:05,943][INFO ][o.e.c.c.JoinHelper       ] [fort1] failed to join {fort2}{nR7UstreQIe_yKXlxpo-Ew}{XRdOsMHwTnafWK9SD943Gg}{172.100.4.25}{172.100.4.25:9300}{dilmrt}{ml.machine_memory=16703619072, ml.max_open_jobs=20, xpack.installed=true, transform.node=true} with JoinRequest{sourceNode={fort1}{nR7UstreQIe_yKXlxpo-Ew}{EOnAahggRuykCw11eBy6PA}{172.100.4.9}{172.100.4.9:9300}{dilmrt}{ml.machine_memory=16703619072, xpack.installed=true, transform.node=true, ml.max_open_jobs=20}, minimumTerm=190, optionalJoin=Optional.empty}
org.elasticsearch.transport.RemoteTransportException: [fort2][172.100.4.25:9300][internal:cluster/coordination/join]
Caused by: org.elasticsearch.cluster.coordination.CoordinationStateRejectedException: received a newer join from {fort1}{nR7UstreQIe_yKXlxpo-Ew}{EOnAahggRuykCw11eBy6PA}{172.100.4.9}{172.100.4.9:9300}{dilmrt}{ml.machine_memory=16703619072, ml.max_open_jobs=20, xpack.installed=true, transform.node=true}
	at org.elasticsearch.cluster.coordination.JoinHelper$CandidateJoinAccumulator.handleJoinRequest(JoinHelper.java:447) ~[elasticsearch-7.8.1.jar:7.8.1]
	at org.elasticsearch.cluster.coordination.Coordinator.processJoinRequest(Coordinator.java:526) ~[elasticsearch-7.8.1.jar:7.8.1]
	at org.elasticsearch.cluster.coordination.Coordinator.lambda$handleJoinRequest$7(Coordinator.java:489) ~[elasticsearch-7.8.1.jar:7.8.1]
	at org.elasticsearch.action.ActionListener$1.onResponse(ActionListener.java:63) ~[elasticsearch-7.8.1.jar:7.8.1]
	at org.elasticsearch.transport.ClusterConnectionManager.connectToNode(ClusterConnectionManager.java:120) ~[elasticsearch-7.8.1.jar:7.8.1]
	at org.elasticsearch.transport.TransportService.connectToNode(TransportService.java:379) ~[elasticsearch-7.8.1.jar:7.8.1]
	at org.elasticsearch.transport.TransportService.connectToNode(TransportService.java:363) ~[elasticsearch-7.8.1.jar:7.8.1]
	at org.elasticsearch.cluster.coordination.Coordinator.handleJoinRequest(Coordinator.java:476) ~[elasticsearch-7.8.1.jar:7.8.1]
	at org.elasticsearch.cluster.coordination.JoinHelper.lambda$new$0(JoinHelper.java:129) ~[elasticsearch-7.8.1.jar:7.8.1]
	at org.elasticsearch.xpack.security.transport.SecurityServerTransportInterceptor$ProfileSecuredRequestHandler$1.doRun(SecurityServerTransportInterceptor.java:257) ~[?:?]
	at org.elasticsearch.common.util.concurrent.AbstractRunnable.run(AbstractRunnable.java:37) ~[elasticsearch-7.8.1.jar:7.8.1]
	at org.elasticsearch.xpack.security.transport.SecurityServerTransportInterceptor$ProfileSecuredRequestHandler.messageReceived(SecurityServerTransportInterceptor.java:315) ~[?:?]
	at org.elasticsearch.transport.RequestHandlerRegistry.processMessageReceived(RequestHandlerRegistry.java:63) ~[elasticsearch-7.8.1.jar:7.8.1]
	at org.elasticsearch.transport.InboundHandler$RequestHandler.doRun(InboundHandler.java:263) ~[elasticsearch-7.8.1.jar:7.8.1]
	at org.elasticsearch.common.util.concurrent.ThreadContext$ContextPreservingAbstractRunnable.doRun(ThreadContext.java:695) ~[elasticsearch-7.8.1.jar:7.8.1]
	at org.elasticsearch.common.util.concurrent.AbstractRunnable.run(AbstractRunnable.java:37) ~[elasticsearch-7.8.1.jar:7.8.1]
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1130) ~[?:?]
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:630) ~[?:?]
	at java.lang.Thread.run(Thread.java:832) [?:?]
****
```

</details>
elasticsearch 集群失效，大致原因，es内部存在脏数据，

‍
