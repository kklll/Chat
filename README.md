# Chat

一个基于 QT 和 C++ 网络编程的简单的即时通讯程序

## 网络协议

### 1. 登录注册协议

<center>登录请求数据包格式（128）</center>

|      | type       | work       | from_len     | from     | to_len       | to       | psw_len  | psw  | uid_len | uid  | size         |
| ---- | ---------- | ---------- | ------------ | -------- | ------------ | -------- | -------- | ---- | ------- | ---- | ------------ |
| 解释 | 数据包类型 | 数据包功能 | 发送者IP长度 | 发送者IP | 接收者IP长度 | 接收者IP | 密码长度 | 密码 | uid长度 | uid  | 数据包总长度 |
| 长度 | 1          | 1          | 1            | 1-15     | 1            | 1-15     | 1        | 1-20 | 1       | 1-20 | 1            |

* **type**: 数据包类型(char),占 1 位
  * ‘0’: 普通数据包
  * ‘1’: 心跳包
* **work**: 表示数据包的具体功能（int）,占用 1 位，因此最多支持 128 种功能
  * 1：表示该数据包用于登录请求
  * 2：表示该数据包用于注册请求
* **from_len**: 表示发送方IP长度（IPV4）用 int存储，占用 1 位
*  **from**: 表示发送方IP地址，用 char 存储，最多占用 15 位
* ......
* **size**: 数据包实际长度，用于服务端校验数据。

<center>登录响应数据包格式（128）</center>

|      | type       | work       | uid_len | uid  | name_len   | name   | token_len | token |
| ---- | ---------- | ---------- | ------- | ---- | ---------- | ------ | --------- | ----- |
| 解释 | 数据包类型 | 数据包功能 | uid长度 | uid  | 用户名长度 | 用户名 | token长度 | token |
| 长度 | 1          | 1          | 1       | 1-20 | 1          | 1-20   | 1         | 1-64  |

* **work**:表示数据包的具体功能（int）,占用 1 位，因此最多支持 128 种功能
  * 1: 登录成功
  * 2：登陆失败（work = 2 时，响应包只有 type 和 work 两项）
* **token**: 用户登录凭证，使用 UUID 产生。 