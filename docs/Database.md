# 数据库规划

> 注:所有数据表如果以`ID`字段开头则是使用数据库创建主键作为ID,反之则是忽略数据库主键

> 总数据库只有一个

> 组织数据库是一个组织一个

> 会议数据库是一个会议一个

> 用户数据库是一个用户一个

## 总数据库

> 唯一性数据存放

### 数据表

#### UserInfo

> 用户信息表

| ID                                  | UserInfo                           |
| ----------------------------------- | ---------------------------------- |
| 用户ID(数据库自动创建 跨数据库唯一) | 用户信息 (可能是一组数据 需要展开) |

#### Login

> 登陆表(用于登陆验证)

| UserID | Username | Password | OpenID             |
| ------ | -------- | -------- | ------------------ |
| 用户ID | 用户名   | 密码     | 用户唯一标识(微信) |

#### Token

> 登陆创建的有时效令牌表(用于请求验证)

> 可能使用jwt以精简掉此表

| UserID | Token           | Time              |
| ------ | --------------- | ----------------- |
| 用户ID | Token(登陆获得) | 负责控制Token时效 |

#### GroupInfo

> 组织信息表

| ID                                  | GroupCode                                                           |
| ----------------------------------- | ------------------------------------------------------------------- |
| 组织ID(数据库自动创建 跨数据库唯一) | 用户可见的组织Code(用于手动加入组织 可能会用这个Code生成组织二维码) |

#### CreateGroupRequest

> 创建组织请求表

> 用于存放申请部门/组织信息的数据库

| ID     | UserID | Reason   | GroupName | GroupCode                                                           | GroupDescription |
| ------ | ------ | -------- | --------- | ------------------------------------------------------------------- | ---------------- |
| 请求ID | 用户ID | 申请原因 | 组织名    | 用户可见的组织Code(用于手动加入组织 可能会用这个Code生成组织二维码) | 组织描述         |

---

## 单个部门数据库

> 负责控制部门内成员权限等

### 数据表

#### MemberInfo

> 记录成员在组织内的权限等

| UserID | Permissions                        |
| ------ | ---------------------------------- |
| 用户ID | 用户在组织的权限(这块可能需要展开) |

#### MeetingInfo

> 组织内会议(记录组织有开过什么会议)

| ID     | BeginAt  | EndAt    | MeetingDescription |
| ------ | -------- | -------- | ------------------ |
| 会议ID | 开始时间 | 结束时间 | 会议描述           |

---

## 单个会议数据库

> 负责记录参会情况

### 数据表

#### MettingParticipants

> 记录参会人

| UserID | ParticipationTime |
| ------ | ----------------- |
| 用户ID | 参会时间          |

#### Sign

> 记录签到信息

> 因为可能会有多次签到,所以需要一个ID来区分

| ID                                            | BeginAt  | EndAt    |
| --------------------------------------------- | -------- | -------- |
| 签到ID(由数据库自动创建 单个会议数据库内唯一) | 开始时间 | 结束时间 |

#### SignatureBook

> 记录签到情况

| UserID | SignID |
| ------ | ------ |
| 用户ID | 签到ID |

--

## 每个用户的用户数据库

### 数据表

#### MemberOf

> 记录用户加了哪些组织(用来避免搜索用户在哪些组织时的大量搜索)

| GroupID | Permissions |
| ------- | ----------- |
| 组织ID  | 权限        |
