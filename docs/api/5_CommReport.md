---
sort: 5
---

# 通用数据上报

## 基本信息

**Path：** /api/v1/CommReport

**Method：** POST

**接口描述：**


## 请求参数

**Headers**

| 参数名称          | 参数值              | 是否必须 | 示例 | 备注 |
|---------------|------------------|------|----|----|
| Content-Type  | application/json | 是    |    |    |
| Authorization | token            | 是    |    |    |

**Body**

| 名称           | 类型        | 是否必须 | 默认值 | 备注                             | 其他信息                       |
|--------------|-----------|------|-----|--------------------------------|----------------------------|
| ts           | integer   | 必须   |     | 时间戳                            |                            |
| nonce        | string    | 必须   |     | 随机字符串                          |                            |
| data_list    | object [] | 必须   |     | 上报数据内容,每次请求最多10条        | [{},{}]                    |
| ├─ oper_id   | string    | 必须   |     | 操作ID                           | 操作ID官方提供                   |
| ├─ gid       | integer   | 非必须  |     | 房间ID                           |                            |
| ├─ target_id | string    | 非必须  |     | 频道ID                           |                            |
| ├─ to_uid    | integer   | 非必须  |     | 接收者uid                         | 群聊为at_msg中uid，私聊为target_id |
| ├─ scope     | string    | 非必须  |     | 消息范围 channel:频道消息,private:私聊信息 | 默认channel                  |
| ├─ ext1      | string    | 非必须  |     | 扩展字段1                          |                            |
| ├─ ext2      | string    | 非必须  |     | 扩展字段2                          |                            |
| ├─ ext3      | string    | 非必须  |     | 扩展字段3                          |                            |
| ├─ ext4      | string    | 非必须  |     | 扩展字段4                          |                            |
| ├─ ext5      | string    | 非必须  |     | 扩展字段5                          |                            |
| ├─ ext6      | string    | 非必须  |     | 扩展字段6                          |                            |
| ├─ ext7      | string    | 非必须  |     | 扩展字段7                          |                            |
| ├─ ext8      | string    | 非必须  |     | 扩展字段8                          |                            |
| ├─ ext9      | string    | 非必须  |     | 扩展字段9                          |                            |
| ├─ ext10     | string    | 非必须  |     | 扩展字段10                         |                            |
| ├─ msg_seq     | integer    | 非必须  |     | 消息序号                         |                            |


## 返回数据

| 名称  | 类型      | 是否必须 | 默认值 | 备注                   | 其他信息 |
|-----|---------|------|-----|----------------------|------|
| ret | integer | 必须   | 0   |                      |      |
| msg | string  | 必须   | ok  |                      |      |

## YTB数据需求

### 1. 批量上报

后台每日 23：00 定时、全量上报当前bot被添加的房间信息。

必填字段如下：

| 字段      | 值             |
|---------|---------------|
| oper_id | 1502007320601 |
| gid     | 房间ID          |
| ext1    | 该群订阅UP主或频道数量  |
| ext2    | 订阅途径（1=指令，2=UI） |

### 2. 消息上报

Bot每发送一条消息均上报一条数据。

需要区分主动消息/被动消息：

- 主动消息为Bot推送YTB订阅内容等。

- 被动消息为Bot回复用户指令等，需要正确填写to_uid字段。

必填字段如下：

| 字段        | 值                     |
|-----------|-----------------------|
| oper_id   | 主动:1503000120601 / 被动:1503000120602        |
| gid       | 房间ID                  |
| target_id | 频道ID                  |
| to_uid    | 接收者uid ，群聊为at_msg中uid |
| scope     | channel               |
| ext1      | youtube               |
| ext2      | 订阅up主/频道名             |
| ext3      | 消息类型（1=推荐订阅提醒，2=推送内容）   |
| msg_seq      | 消息序号    |

## 投票数据需求

### 1. 批量上报

后台每日 23：00 定时、全量上报当前bot被添加的房间信息。

必填字段如下：

| 字段      | 值             |
|---------|---------------|
| oper_id | 1502007320605 |
| gid     | 房间ID          |
| ext1    | 投票个数  |
| ext2    | 投票ID，多个用【-】串连  |

### 2. 消息上报

Bot每发送一条消息均上报一条数据。

需要区分主动消息/被动消息：

- 主动消息为：不经过用户触发，主动发送消息。

- 被动消息为：用户触发指令后，机器人发送的消息。

必填字段如下：

| 字段        | 值                     |
|-----------|-----------------------|
| oper_id   | 主动:1503000120601 / 被动:1503000120602        |
| gid       | 房间ID                  |
| target_id | 频道ID                  |
| to_uid    | 接收者uid ，群聊为at_msg中uid |
| scope     | channel               |
| msg_seq      | 消息序号             |

## 抽奖数据需求

### 1. 批量上报

后台每日 23：00 定时、全量上报当前bot被添加的房间信息。

必填字段如下：

| 字段      | 值             |
|---------|---------------|
| oper_id | 1502007320604 |
| gid     | 房间ID          |
| ext1    | 抽奖个数  |
| ext2    | 抽奖ID，多个用【-】串连  |

### 2. 消息上报

Bot每发送一条消息均上报一条数据。

需要区分主动消息/被动消息：

- 主动消息为：不经过用户触发，主动发送消息。

- 被动消息为：用户触发指令后，机器人发送的消息。

必填字段如下：

| 字段        | 值                     |
|-----------|-----------------------|
| oper_id   | 主动:1503000120601 / 被动:1503000120602        |
| gid       | 房间ID                  |
| target_id | 频道ID                  |
| to_uid    | 接收者uid ，群聊为at_msg中uid |
| scope     | channel               |
| msg_seq      | 消息序号       |