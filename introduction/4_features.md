# Actor (actor)

## 特性
- 使用函数/方法创建actor
- 使用对象工厂创建actor
- 使用自动的/带前缀的/指定的名称生成actor

# Props (配置)

## 设置
- Actor生产者
- Mailbox生产者
- Supervisor策略
- Dispatcher(分发器)
- Actor
- Middleware（中间件）

# Context (上下文)

## 数据
- Parent PID (父actor的PID)
- Self PID (自己的PID)
- Sender PID（发送者的PID）
- Children PIDs（子actor的PID）
- Current message（当前处理的消息）
- Current Actor（当前处理的actor）

## 特性
- 向sender发送响应
- Stash current message pending restart
- 使用自动的/带前缀的/指定的名称生成子actor
- 停止/重启/恢复子actor
- 设置/push/pop actor 行为(become/unbecome)
- Watch/unwatch actors
- 接收消息超时处理

# Process注册

- 根据PID获取Process
- 增加带ID的本地Process
- 根据PID移除Process
- 生成下一个Process ID

# Process

- 向process发送用户消息
- 向process发送系统消息
- 停止Process

# Supervision(监管)

## Directives(指示)
- Resume(恢复)
- Restart(重启)
- Stop(停止)
- Escalate(升级)
## Strategies(策略)
- OneForOneStrategy：应用于失败的子actor的指示

# PID

## 特性
- 存放地址（nonhost或远程的地址）和ID
- 发送用户消息
- 发送系统消息
- Request
- Request future
- 停止

# Future process

用于提供包含请求响应的可等待future的辅助process

# Dead letter process

辅助process，用于收集发送到不存在process的消息

# Routers（路由器）

- Group routers将消息路由到一组指定的路由
- Pool routers将消息路由到一组指定大小的路由，并由路由器创建/管理
- Broadcast routers将消息路由到所有路由
- Random routers将消息路由到随机路由
- Consistent-hash routers可确定地将消息路由到路由
- Round-robin routers以循环方式将消息路由到路由

## Consistent hash routers(一致性哈希路由)
路由是确定性的，基于消息计算出的哈希值

使用散列环，这样如果一个路由不可用，则会自动选择另一个路由，当路由返回时，它将再次使用

## Broadcast routers

当路由是远程时，使用特殊消息RouterBroadcastMessage来更有效地路由消息

## Management messages

- 添加路由
- 获取路由
- 移除路由

# Remote

- Remote.Start() 在给定地址上启动远程actor服务器
- RemoteProcess代表远程actor服务器上的actor
- 可以watch远程actor
- EndpointWriter邮箱收集批量消息以优化发送