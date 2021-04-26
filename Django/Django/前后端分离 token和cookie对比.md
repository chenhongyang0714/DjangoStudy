## 前后端分离 token和cookie对比
~~~python
# token、cookie存在的意义
HTTP协议本身是无状态的，所以需要一个标志来对用户身份进行验证
~~~
#### cookie
~~~
用户登录成功后，会在服务器生成一个session对象，同时发送给客户端一个cookie，这个cookie里面有唯一标识该用户的session_id数据，需要客户端和服务器同时存储。用户再次进行请求操作时，需要带上cookie，在服务器进行验证。
cookie是有状态的。
~~~
#### token
~~~
用户进行任何操作时都需要带上一个token，token的存在形式有很多种，这个token只需要存在客户端，服务器在收到数据后，进行解析。
token是无状态的。
~~~
#### token相对cookie的优势
~~~
1. 支持跨域访问，将token置于请求头中，而cookie是不支持跨域访问的。
2. 无状态化，服务端无需存储token，只需要验证token信息是否正确即可;而session需要在服务端存储，一般是通过cookie中的session_id在服务器查询对应的session
3. 无需绑定到一个特殊的身份验证方案（传统的用户名、密码登录），只需要请求的token是符合我们预期设定的即可。
4. 更适合于移动端（ios,小程序等等），这种原生平台不支持cookie，比如说微信小程序，每一次请求都是一次会话，当然我们可以手动添加cookie。
5. 避免CSRF跨站伪造攻击，还是因为不依赖cookie
6. 非常适用于DRF，这样可以与各种后端（java,.net,python）相结合，去耦合
~~~

