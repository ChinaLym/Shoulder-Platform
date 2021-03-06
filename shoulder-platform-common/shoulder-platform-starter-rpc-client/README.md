# shoulder-platform-starter-rpc-client

包含如 `Feign`、`Ribbon` 、统一认证客户端等

## 功能 & 选型

- 服务发现，自动寻址
    - nacos-discovery-client
- 负载均衡
    - Ribbon
- 简化调用
    - Feign
- 服务间调用认证
    - 支持 RestTemplate
    - 支持 Feign
    
- 先控制并发再熔断最后重试

- 缓存（与业务相关，默认不提供）
    - 调用后缓存结果
    - 有缓存或熔断时使用缓存，自己决定
    
## 离群实例摘除

> 集群中部分实例节点异常），此时直接触发服务降级不如针对性地仅对故障实例隔离，摘除服务列表。

场景介绍：
- A为服务提供方，有2个实例。
- B为调用方，当调用A服务50%几率失败时，将自动触发降级。
- 若A服务一台机器出现故障（如磁盘满、线程池满，滚动发布新版本出问题），很可能会导致B服务触发

处理手段
- 自动处理宕机夯死：心跳中断，由注册中心自动将其暂时下线。
- 自动处理高负载，慢响应：监控服务监控其负载、当达到限值触发告警、告警中触发web端点，调用注册中心接口，主动将其下线
- 手动：在注册中心手动将其下线

https://www.jianshu.com/p/304250a65c82