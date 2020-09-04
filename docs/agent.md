
agent只提供代理的能力，不做任何数据的处理

1. node, container等资源监控
2. http
3. tcp
4. log获取
5. command执行 bash python

agent主动注册到registry，agent的注册UUID可配置。当前UUID格式：
针对daemonset模式：
0. uuidVersion。不同的version，UUID的构成模式不同
1. clusterName
2. nodeIP
3. agentPort。一个node上可以运行多个agent(横向扩容，提高吞吐量)，通过agentPort来区分
4. 设备UUID。比如主板UUID
agentUUID不能依赖于设备UUID来保证唯一性，设备UUID只是为了记录设备的变更情况。注册时需要对agentUUID的唯一性和合法性进行校验。
心跳保持存活状态，提供本身状态详细信息。详细信息存储schema+json字段

针对sidecar模式
0. uuidVersion
1. targetInfo
3. targetUUID


agent部署在被监控集群，可能会存在网络问题。比如：外部服务无法主动访问agent，但是agent可以访问外部服务。

worker服务会通过agent获取指定目标的数据，agent需要知道应该和哪些worker节点主动建立连接。如何知道
agent通过registry或者master节点获取 本agent相关任务分配在哪些worker 然后与worker主动建连。
worker的IP在外网不一定能够访问，增加一个网关来路由到指定的worker么？

agent可以增加一个标识，标志agent与worker网络是否能互通

是否要支持两种网络模式？网络ok 则worker主动建连agent。网络不ok 则agent主动建连worker。还是不用 只保留一种模式
像公有云这种模式 还是主动push比较合理。或者worker本身就应该运行在vpc内部，并提供API供数据消费方来pull数据或者主动push数据到server。
这个模式会影响调度么？
