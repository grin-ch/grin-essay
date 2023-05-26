[toc]
##### 告警规则(rules)

- 它们定义了应该如何触发指标警报,应该考虑什么条件

##### 网络钩子(webhook)
-  Web 服务端点列表,应在触发警报后调用

##### gRPCHook
- 远程 gRPC 方法的主机和端口,应在警报触发后调用

##### 实体名称(Entity name)

- **Service**: 服务名称 (Service name)
- **Instance**: 实例名称 {Instance name} of {Service name}
- **Endpoint**: 端点名称 {Endpoint name} in {Service name}
- **Database**: 数据库服务名称 (Database service name)
- **Service Relation**: 服务关系 {Source service name} to {Dest service name}
- **Instance Relation**: 实例关系 {Source instance name} of {Source service name} to {Dest instance name} of {Dest service name}
- **Endpoint Relation**: 端点关系 {Source endpoint name} in {Source Service name} to {Dest endpoint name} in {Dest service name}

##### 规则(Rules)

- 有两种类型的规则，单独规则和复合规则，复合规则是单独规则的组合

- **单独规则(Individual rules)**:报警规则由以下几个键构成

  - **Rule name**:唯一名称，显示在报警信息中,必须以`_rule` 结尾

  - **[Metrics name](./指标列表(metrics_name_list).md)**: 指标名称,仅支持 long/double/int 类型

  - **Include names**: 定义包含的实体名称

  - **Exclude names**:定义排除的实体名称

  - **Include names regex**:提供正则表达式以包含实体名称

  - **Exclude names regex**:提供正则表达式以排除实体名称

  - **Include labels**:定义包含的标签

  - **Exclude labels**:定义排除的标签

  - **Include labels regex**:提供正则表达式以包含标签

  - **Exclude labels regex**:提供正则表达式以排除标签

    *标签的设置是`Meter-system`需要的，它打算存储来自标签系统平台的指标，就像Prometheus，Micrometer等。
    支持以上四种设置的函数应该实现`LabeledValueHolder`。*

  - **Threshold**:目标值

    对于多个值指标，例如 **percentile**，阈值是一个数组。描述为`value1, value2, value3, value4, value5`,
      每个值可以是每个度量值的阈值。如果不想通过此值或某些值触发警报，请将值设置为`-`

  - **OP**: 运算符，支持`>`,`>=`,`<`,`<=`,`=`,`!=`

  - **Period**:检查报警规则的周期

  - **Count**:触发告警的阈值,在周期窗口中,如果**value**的数量超过阈值(通过OP),达到计数时,应该发送告警

  - **Only as condition**:指定规则是否可以发送通知或仅作为复合规则的条件

  - **Silence period**:默认与**Period**相同,表示在一个时间段内,相同的告警(同一个ID指标名称)将被触发一次

- **复合规则(Composite rules)**:

  复合规则仅适用于针对同一实体级别的告警规则，例如:

  `service_percent_rule && service_resp_time_percentile_rule`. 

  您不应该编写不同实体级别的警报规则. 例如服务度量的一个警报规则与端点度量的另一规则

  - **Rule name**:唯一名称,显示在报警信息中.必须以`_rule` 结尾
  - **Expression**:指定如何编写规则,支持 `&&`, `||`, `()`
  - **Message**:指定触发规则时的通知消息

##### 示例(.yml)

```yaml
rules:
  # Rule unique name, must be ended with `_rule`.
  endpoint_percent_rule:
    # metrics 的值必须是long,double,int中的一种
    metrics-name: endpoint_percent
    threshold: 75
    op: "<"
    # 10分钟检查一次
    period: 10
    # 触发指标次数的阈值,达到阈值时触发告警
    count: 3
    # 触发告警后的10分钟内不会再次触发,默认和period相同
    silence-period: 10
    # 指定此规则是否可以发送通知或仅作为复合规则的条件
    only-as-condition: false
    
  service_percent_rule:
    metrics-name: service_percent
    # 默认匹配所有指标中的所有服务
    include-names:
      - service_a
      - service_b
    # 排除service_c
    exclude-names:
      - service_c
    threshold: 85
    op: "<"
    period: 10
    count: 4
    only-as-condition: false
    
  service_resp_time_percentile_rule:
    metrics-name: service_percentile
    op: ">"
    # 多个价值指标阈值。 50%、75%、90%、95%、99% 的阈值(1000ms)
    threshold: 1000,1000,1000,1000,1000
    period: 10
    count: 3
    silence-period: 5
    message: Percentile response time of service {name} alarm in 3 minutes of last 10 minutes, due to more than one condition of p50 > 1000, p75 > 1000, p90 > 1000, p95 > 1000, p99 > 1000
    only-as-condition: false
    
  meter_service_status_code_rule:
    metrics-name: meter_status_code
    # 排除标签(http状态码200)
    exclude-labels:
      - "200"
    op: ">"
    threshold: 10
    period: 10
    count: 3
    silence-period: 5
    message: The request number of entity {name} non-200 status is more than expected.
    only-as-condition: false
    
composite-rules:
  comp_rule:
    # 必须满足service_percent_rule和service_resp_time_percentile_rule
    expression: service_percent_rule && service_resp_time_percentile_rule
    message: Service {name} successful rate is less than 80% and P50 of response time is over 1000ms
```



##### webhook

Webhook 要求 peer 是一个 web 容器。警报消息将通过 HTTP(post)以`application/json` 格式发送。 JSON 格式基于`List<org.apache.skywalking.oap.server.core.alarm.AlarmMessage>`,包含以下关键信息

- **scopeId**:**scope**,所有**scope**都在 org.apache.skywalking.oap.server.core.source.DefaultScopeDefine 中定义。
- **name**:Target scope entity name. Please follow [Entity name define](#entity-name).
- **id0**:The ID of the scope entity matched the name. When using relation scope, it is the source entity ID.
- **id1**: When using relation scope, it will be the dest entity ID. Otherwise, it is empty.
- **ruleName**:The rule name you configured in `alarm-settings.yml`.
- **alarmMessage**:Alarm text message.
- **startTime**:Alarm time measured in milliseconds, between the current time and midnight, January 1, 1970 UTC.

##### 示例

```json
[{
    "scopeId": 1, 
    "scope": "SERVICE",
    "name": "serviceA", 
    "id0": "12",  
    "id1": "",  
    "ruleName": "service_resp_time_rule",
    "alarmMessage": "alarmMessage xxxx",
    "startTime": 1560524171000
}, {
	"scopeId": 1,
	"scope": "SERVICE",
	"name": "serviceB",
	"id0": "23",
	"id1": "",
    "ruleName": "service_resp_time_rule",
	"alarmMessage": "alarmMessage yyy",
	"startTime": 1560524171000
}]
```



##### 动态更新设置

从 6.5.0 开始，报警设置可以在运行时通过[动态配置](dynamic-config.md)动态更新，
这将覆盖 `alarm-settings.yml` 中的设置。

SkyWalking为了判断是否触发了报警规则，需要缓存一个时间窗口的metrics
每个警报规则，如果规则的任何属性（`metrics-name`、`op`、`threshold`、`period`、`count` 等）发生变化，
滑动窗口将被销毁并重新创建，导致此特定规则的警报再次重新启动。