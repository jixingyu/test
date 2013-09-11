Message Module
==============

This is the Message module for Pi.

User private messages and the system notification are provided.

Note
====
- You should deny guest to access this module by editing permissions in backend.

For consumer
============
1. Client register
  - <host>/oauth/client/register
      - bbb
  - abc

2. Get authorization code
  - Url: <host>/oauth/authorize/index
  - method: GET
  - parameters
    || *client_id* || Client ID assigned after register the client ||
    || *response_type* || Response type, the value is "code" fixed. ||
    || *redirect_uri* || Callback uri after authorization ||
    || *state* || State of client, it will be sent back after authorization ||
    || *scope(optional)* || Uncompleted ||

front-end
========

#### 1. Client management
* 1.1 **Register:** Web application register a client, then a client_id and client_secret will be generated. Registration needs client name, address, redirect uri, description, logo.
* 1.2 **Client list:** List the clients that registered before.
* 1.3 **Show details and edit client:** Client id and client secret can not be changed.
* 1.4 **Submit for verification:**  Need to submit for verification after register a client. If the client is not verified, functions are limited.
* 1.5 **Apply for scope:** The new registered client has the scope with base functions. The application for more scopes is provided after the client is virefied.

#### 2. Authorization api for consumer
* 1.1 **Authorize api**
  * Used to get the url that provide the authorization service. 提供用户授权页面，用户确认授权后，浏览器跳转到回调地址上，并携带响应参数。
  * Access url：http://\<host\>/oauth/authorize/index?response_type=\<code\>&client_id=\<client_id\>&redirect_uri=\<redirect_uri\>&state=\<rand_string\>
  * Parameter：
      * response_type: It's "code" if grant type is authorization_code, or "token" if grant type is implicit.
      * client_id: Client id, used to identify a client.
      * redirect_uri: The uri after 客户端接收授权码，发起token请求的地址,需要经过两次URL转码 
      * scope ：本次授权申请需要用户授权的范围，为空时则使用客户端申请的全部scope
      * state：随机字符串，由客户端产生，响应时作为参数原样返回 
  * Redirect uri after authorization
      * authorization_code：\<redirect_uri\>?code=\<code\>&state=\<rand_string\>
      * implicit: ：\<redirect_uri\>?token=\<token\>&state=\<rand_string\>
* 1.2 **Grant api**
  * Used to get the access token. The submit method must be "post".
  * Access url：http://\<host\>/oauth/token/index?client_id=\<client_id\>&client_secret=\<client_secret\>&grant_type=authorizecode&redirect_uri=\<redirect_uri\>&code=\<authorization_code\>
  * Return json string
```
    {
      "access_token":"681e1d98fe25f973a41a011e5a784ba1",
      "expires_in":"3600",
      "scope":null
    }
```


back-end
========
#### 1. 客户端相关后台
* 1.1 **列表:** 查看所有客户端信息，显示客户端名称，简介，创建者。
* 1.2 **管理:** 对客户端进行管理，包括：删除客户端，停止客户端服务
* 1.3 **审核:** 处理客户端的审核申请，如果无法通过申请，需要填写相关原因

#### 2. scope相关后台
* 1.1 **添加:** 添加scope信息，包括：scope名称，scope简介
* 1.2 **列表:** 查看当前已有的scope
* 1.3 **删除、修改:** 对已有的scope进行管理，（修改功能尚未实现）
* 1.4 **审核:** 审核客户端对scope的申请

#### 3. config设置
* 1.1 **模块功能角色:** 设置当前模块作为consumer还是provider
* 1.2 **授权方式设置:** 设置provider提供的授权方式，默认授权方式为AuthorizationCode方式
* 1.3 **授权信息设置:** 设置授权过程中，相关数据的条件，包括：code、token的长度和有效期
* 1.4 **基本scope设置:** 设置模块支持的基本scope，凡是注册的客户端都具有这些scope权限

