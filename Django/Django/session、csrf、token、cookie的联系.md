# session、csrf、token、cookie的联系
## Session
~~~
session可以用来判断是哪一位用户(通过session_key)。
前后端不分离的话 会自动有session_id 携带在cookie中发请求给后端。
~~~
__架构:客户端代码和服务器代码运行在同一台服务器上，协议、域名、端口号一致。__
* session是服务端存储的一个对象，主要用来存储所有访问过该服务端的客户端的用户信息( 也可以存储其他信息)，从而实现保持用户会话状态。但是服务器重启后，内存会被销毁，存储的用户信息也就随之消失。
* 不同的用户访问服务端的时候会在session对象中存储键值对，'键'用来存储开启这个用户信息的'钥匙'，在登录成功后，'钥匙'通过cookie返回给客户端，客户端存储为sessionid记录在cookie中。当用户再次访问时，会 __默认携带__ cookie中的sessionid来实现会话机制。
* session持久化  
用于解决重启服务器后session就消失的问题。在数据库中存储session，而不是存储在内存中。通过包：express-mysql-session
~~~python
Session失效问题:
在我们每一次的请求数据中，浏览器都会向后台发送一个sessionid,后台会根据这个值自动查找id为sessionid的那个session对象。但是跨域时，‘sessionid的值每次都会变’，后台就会以为是一个新的会话，于是又去创建了一个新的session对象，而原来的session对象就找不到了。
~~~
* __注意:__ 如果我们的cookie被黑客挟持，这将导致CSRF问题。
---
## CSRF
* 跨域请求伪造(英文:Cross-site request forgery),是一种挟持用户在当前已登录的Web应用程序上执行非本意的操作的攻击方法。CSRF利用的是网站对用户网页浏览器的信任。
---
## token
~~~
token可以判断是不是自己人，但是不能判断是哪一个人。
~~~
__适用情况：前后端分离，项目级别__
__架构：前后端代码运行在两台机器上，协议、域名、端口号不一致，session不能用来验证登录。__ 

~~~python
前后端分离不能用session(session失效问题);【因为前后端属于不同的域,导致每次ajax请求服务器都会当做新的用户访问,导致session丢失】
涉及登陆验证 也不需要token;
~~~
* 请求登录时，token与sessionid原理相同，是对key和key对应的用户信息进行加密后的加密字符。登录成功后，会在响应主体中将{token:'字符串'}返回给客户端。客户端对cookie、sessionStorage、localStorage进行存储，再次向服务器请求时 __token不会被默认携带__ ，需要在请求拦截器位置给  _请求头_ 中添加认证字段Authorization携带token信息，服务器端就可以通过token信息查询用户登录状态。
## cookie
~~~python 
1. 当前端使用axios时，默认是不携带cookie的。
2. 由于前后端分离，那就必须要跨域的，但是cookie默认是不跨域的。
~~~
友情链接：
* [token和session的区别](https://www.cnblogs.com/belongs-to-qinghua/p/11353228.html)
* [CSRF百度百科](https://baike.baidu.com/item/%E8%B7%A8%E7%AB%99%E8%AF%B7%E6%B1%82%E4%BC%AA%E9%80%A0/13777878?fromtitle=CSRF&fromid=2735433&fr=aladdin)
