# 模型文档

## 概述

本项目采用django ORM机制sqlite数据库进行数据存储。

根据功能具体分为:

- User(用户表单)
- Groupfriend(好友分组)
- ChatSingle(私聊会话)
- SingleMessage(私聊消息)
- ChatGroup(群聊会话)
- GroupMessage(群聊消息)
- Invite(好友邀请)
- ChatInfo(群聊信息)

## 模型解析

### User(用户表单)

每个用户对应的表单。

| 名称         | 类型                 | 中文名     | 说明           |
| ------------ | -------------------- | ---------- | -------------- |
| id           | BigAutoField         | 用户编号   | 主键           |
| name         | CharField            | 用户名     | 唯一值         |
| password     | CharField            | 密码       | none           |
| created_time | FloatField           | 创建时间   | 默认为当前时间 |
| image_id     | PositiveIntegerField | 头像ID     | 默认为0        |
| email        | TextField            | 邮箱       | 默认为空字符串 |
| phonenum     | CharField            | 手机号码   | 默认为空字符串 |
| self_sign    | TextField            | 个性签名   | 可为空         |
| friendlist   | ManyToManyField      | 好友列表   | none           |
| unpassed     | ManyToManyField      | 未通过用户 | none           |
| topass       | ManyToManyField      | 待通过用户 | none           |
| groupfriends | ManyToManyField      | 群组好友   | none           |
| chats        | ManyToManyField      | 单聊列表   | none           |
| chatg        | ManyToManyField      | 群聊列表   | none           |
| chatinfo     | ManyToManyField      | 聊天信息   | none           |

### Groupfriend(好友分组)

| 名称         | 类型            | 中文名   | 额外说明       |
| ------------ | --------------- | -------- | -------------- |
| id           | BigAutoField    | 编号     | 主键           |
| name         | CharField       | 名称     | 唯一值         |
| created_time | FloatField      | 创建时间 | 默认为当前时间 |
| users        | ManyToManyField | 好友列表 | none           |

`Groupfriend` 模型具有一个 `serialize` 方法，用于将对象序列化为字典。该方法返回一个包含 `id`、`name`、`createdAt` 和 `members` 的字典。`members` 列表包含了该群组的所有成员的名称。

### ChatSingle(私聊会话)

| 名称      | 类型                 | 中文名     | 额外说明 |
| --------- | -------------------- | ---------- | -------- |
| id        | BigAutoField         | 编号       | 主键     |
| fromname  | CharField            | 发送者名称 | none     |
| toname    | CharField            | 接收者名称 | none     |
| messages  | ManyToManyField      | 消息列表   | 可为空   |
| unreadnum | PositiveIntegerField | 未读消息数 | 默认为0  |

`ChatSingle` 模型代表了单个聊天会话，其中包含了发送者和接收者的名称，以及相关联的消息列表。`unreadnum` 字段用于追踪未读消息的数量。

### SingleMessage(私聊消息)

| 名称         | 类型                 | 中文名     | 额外说明       |
| ------------ | -------------------- | ---------- | -------------- |
| id           | BigAutoField         | 编号       | 主键           |
| created_time | FloatField           | 创建时间   | 默认为当前时间 |
| sendtime     | TextField            | 发送时间   | 可为空         |
| text         | TextField            | 消息内容   | 可为空         |
| sender       | TextField            | 发送者     | 可为空         |
| replyto      | TextField            | 回复对象   | 可为空         |
| read         | BooleanField         | 是否已读   | 默认为False    |
| isreply      | BooleanField         | 是否为回复 | 默认为False    |
| replynum     | PositiveIntegerField | 回复数量   | 默认为0        |

`SingleMessage` 模型表示单个聊天消息，其中包含了消息的创建时间、发送时间、文本内容、发送者、回复对象等信息。

### ChatGroup(群聊会话)

| 名称         | 类型                 | 中文名     | 额外说明 |
| ------------ | -------------------- | ---------- | -------- |
| id           | BigAutoField         | 编号       | 主键     |
| image_id     | PositiveIntegerField | 图像ID     | 默认为0  |
| users        | ManyToManyField      | 用户列表   | none     |
| leader       | CharField            | 群组领导   | none     |
| managers     | ManyToManyField      | 管理员列表 | none     |
| name         | CharField            | 群组名称   | none     |
| invite       | ManyToManyField      | 邀请列表   | none     |
| statenow     | TextField            | 当前状态   | 可为空   |
| statehistory | TextField            | 历史状态   | 可为空   |

`ChatGroup` 模型表示群组聊天，其中包含了群组的图像ID、用户列表、群组领导、管理员、群组名称、邀请列表以及当前和历史状态。如果您还有其他问题或需要更多详细信息，请随时告知我！ 😊

### GroupMessage(群聊消息)          

| 名称         | 类型                 | 中文名       | 额外说明       |
| ------------ | -------------------- | ------------ | -------------- |
| id           | BigAutoField         | 编号         | 主键           |
| created_time | FloatField           | 创建时间     | 默认为当前时间 |
| sendtime     | TextField            | 发送时间     | 可为空         |
| text         | TextField            | 消息内容     | 可为空         |
| sender       | TextField            | 发送者       | 可为空         |
| replyto      | TextField            | 回复对象     | 可为空         |
| isreply      | BooleanField         | 是否为回复   | 默认为False    |
| replynum     | PositiveIntegerField | 回复数量     | 默认为0        |
| unreadlist   | ManyToManyField      | 未读用户列表 | 可为空         |

`GroupMessage` 模型表示群组聊天中的消息，其中包含了消息的创建时间、发送时间、文本内容、发送者、回复对象、是否为回复、回复数量以及未读用户列表。

### Invite(好友邀请)

| 名称    | 类型            | 中文名   | 额外说明 |
| ------- | --------------- | -------- | -------- |
| id      | BigAutoField    | 编号     | 主键     |
| invitor | ManyToManyField | 邀请人   | none     |
| invited | ManyToManyField | 被邀请人 | none     |
| content | TextField       | 邀请信息 | 可为空   |

`Invite` 模型表示邀请，其中包含了邀请人列表、被邀请人列表以及邀请信息。

### ChatInfo(群聊信息)

| 名称      | 类型                 | 中文名     | 额外说明 |
| --------- | -------------------- | ---------- | -------- |
| id        | BigAutoField         | 编号       | 主键     |
| unreadnum | PositiveIntegerField | 未读消息数 | 默认为0  |
| fromname  | CharField            | 发送者名称 | none     |
| groupname | CharField            | 群名       | none     |
| messages  | ManyToManyField      | 消息列表   | 可为空   |

`ChatInfo` 模型表示群组聊天的信息，其中包含了未读消息数、发送者名称、群名及相关联的消息列表。

​                                   

###       

###                           

​              