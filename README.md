# Chat

一个基于 QT 和 C++ 网络编程的简单的即时通讯程序

## 应用层协议 

<center>登录请求数据包格式（128）</center>

|      | type       | work     | flen         | from      | tlen    | to        | plen    | psw   | ulen    | uid       | size   |
| ---- | ---------- | -------- | ------------ | --------- | ------- | --------- | ------- | ----- | ------- | --------- | :----- |
| 值   | 0          | 1        | 9            | 127.0.0.1 | 9       | 127.0.0.1 | 5       | 11111 | 9       | 111111111 | 39     |
| 解释 | 普通数据包 | 登录请求 | from字段长度 | 发送者IP  | to 长度 | 接收者IP  | psw长度 | psw   | uid长度 | uid       | 总长度 |
| 长度 | 1          | 1        | 1            | 1-15      | 1       | 1-15      | 1       | 1-20  | 1       | 1-20      | 1      |

<center>登录成功响应数据包格式（128）</center>

|      | type       | work     | ulen    | uid  | nlen       | name   | tlen      | token   |
| ---- | ---------- | -------- | ------- | ---- | ---------- | ------ | --------- | ------- |
| 值   | 0          | 1        | 3       | xxx  | 3          | xxx    | 36        | x-x-x-x |
| 解释 | 普通数据包 | 登陆成功 | uid长度 | uid  | 用户名长度 | 用户名 | token长度 | token   |
| 长度 | 1          | 1        | 1       | 1-20 | 1          | 1-20   | 1         | 36      |

<center>登录失败响应数据包格式（128）</center>

|      | type       | work     |
| ---- | ---------- | -------- |
| 值   | 0          | 2        |
| 解释 | 普通数据包 | 登陆失败 |
| 长度 | 1          | 1        |

<center>注册请求数据包格式（128）</center>

|      | type       | work     | flen         | from      | tlen    | to        | plen    | psw   | nlen     | name   | aLen         | aPSW     | size   |
| ---- | ---------- | -------- | ------------ | --------- | ------- | --------- | ------- | ----- | -------- | ------ | ------------ | -------- | :----- |
| 值   | 0          | 1        | 9            | 127.0.0.1 | 9       | 127.0.0.1 | 5       | 11111 | 3        | xxx    | 5            | 11111    | 39     |
| 解释 | 普通数据包 | 登录请求 | from字段长度 | 发送者IP  | to 长度 | 接收者IP  | psw长度 | psw   | name长度 | 用户名 | 重复密码长度 | 重复密码 | 总长度 |
| 长度 | 1          | 1        | 1            | 1-15      | 1       | 1-15      | 1       | 1-20  | 1        | 1-20   | 1            | 1-20     | 1      |

<center>注册成功响应数据包格式（128）</center>

|      | type       | work     | ulen    | uid  | nlen       | name   | tlen      | token   |
| ---- | ---------- | -------- | ------- | ---- | ---------- | ------ | --------- | ------- |
| 值   | 0          | 3        | 3       | xxx  | 3          | xxx    | 36        | x-x-x-x |
| 解释 | 普通数据包 | 注册成功 | uid长度 | uid  | 用户名长度 | 用户名 | token长度 | token   |
| 长度 | 1          | 1        | 1       | 1-20 | 1          | 1-20   | 1         | 36      |

<center>注册失败响应数据包格式（128）</center>

|      | type       | work     |
| ---- | ---------- | -------- |
| 值   | 0          | 4        |
| 解释 | 普通数据包 | 注册失败 |
| 长度 | 1          | 1        |

<center>登出请求（1024）</center>

|      | type       | work     | uLen      | uid  | tLen      | token   |
| ---- | ---------- | -------- | --------- | ---- | --------- | ------- |
| 值   | 0          | 3        | 9         | xxx  | 36        | x-x-x-x |
| 解释 | 普通数据包 | 登出请求 | uid的长度 | uid  | token长度 | token   |
| 长度 | 1          | 1        | 1         | 1-20 | 1         | 36      |

<center>发消息请求（1024）</center>

|      | type       | work         | flen     | fID      | tlen    | tID      | tokLen    | token | mLen        | message  |
| ---- | ---------- | ------------ | -------- | -------- | ------- | -------- | --------- | ----- | ----------- | -------- |
| 值   | 0          | 4            | 9        | xxx      | ９      | xxx      | 36        | x-x-x | 100         | xxxx     |
| 解释 | 普通数据包 | 客户端发消息 | fID 长度 | 发送者ID | tID长度 | 接收者ID | token长度 | token | message长度 | 具体消息 |
| 长度 | 1          | 1            | 1        | 1-20     | 1       | 1-20     | 1         | 36    | 2           | 1-940    |

<center>消息转发数据包（1024）</center>

|      | type       | work         | flen     | fID      | mLen        | message  |
| ---- | ---------- | ------------ | -------- | -------- | ----------- | -------- |
| 值   | 0          | 6            | 9        | xxx      | 100         | xxxx     |
| 解释 | 普通数据包 | 客户端发消息 | fID 长度 | 发送者ID | message长度 | 具体消息 |
| 长度 | 1          | 1            | 1        | 1-20     | 2           | 1-940    |

<center>客户端心跳包</center>

|      | type   | work     | uLen      | uid  | tLen      | token   |
| ---- | ------ | -------- | --------- | ---- | --------- | ------- |
| 值   | 1      | 1        | 9         | xxx  | 36        | x-x-x-x |
| 解释 | 心跳包 | 心跳请求 | uid的长度 | uid  | token长度 | token   |
| 长度 | 1      | 1        | 1         | 1-20 | 1         | 36      |

<center>心跳响应包</center>

|      | type   | work     | uLen      | uid  | frNum    | f1IDLen   | f1ID | f1Statu | ... | size |
| ---- | ------ | -------- | --------- | ---- | -------- | --------- | --------- | --------- | --------- | --------- |
| 值   | 1      | 1        | 9         | xxx  | 10       | 9         | xxxx     | 1/2 | ... | 189 |
| 解释 | 心跳包 | 心跳响应 | uid的长度 | uid  | 好友数量 | f1ID 长度 | 第一个好友的ID | 在线/离线 | ... | 数据包总长 |
| 长度 | 1      | 1        | 1         | 1-20 | 2   | 36        | 1-20    | 1   | ... | 2 |