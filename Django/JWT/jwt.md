## JWT

#### JWT  认证流程

~~~
json web token, 一般用于用户认证(前后端分离、微信小程序、app开发)
~~~

* 基于传统的 token 认证

  ~~~
  用户登录
  服务端返回 token， 并将 token 保存在数据库
  用户再次访问需要携带 token
  服务端获取 token， 去数据库校验 token
  ~~~
  

![普通token认证](./pictures/普通token认证.png)

* jwt 

  ~~~
  用户登录
  服务端返回 token (数据库不保存 token )
  用户再次访问需要携带 token
  服务端获取 token， 再做 token 的校验 (算法校验)
  
  优势: 相较于传统的token相比，无需在数据库保存token.
  ~~~

![jwt认证](./pictures/jwt认证.png)



#### JWT 实现过程

##### jwt创建token

* 用户提交用户名和密码给服务端。如果登录成功，使用 jwt 创建一个 token， 并返回给用户。

  ~~~python
  # token
  eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
  
  # 组成：
  第一部分称为头部 (header)
  第二部分称为载荷 (payload)
  第三部分是签证 (signature)
  # 注意： jwt 生成的 token 由三段字符串组成，并且用 . 连接起来
  ~~~

  * header，  json转化为字符串，然后做 base64url 加密

    ~~~python
    {
      'typ': 'JWT',  # 加密算法
      'alg': 'HS256'  # token 类型
    }
    ~~~

  * payload， 自定义的值,  json转化为字符串，然后做 base64url 加密

    ~~~python
    {
      "sub": "1234567890",
      "name": "John Doe",
      "admin": true
    }
    ~~~

  * signature

    ~~~python
    # 第一步: 第1,2部分密文拼接起来
    eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9
    # 第二步: 对前2部分密文进行HS256加密(不能反解) + 加盐  [组合加密]
    # 第三步: 对第二步的密文再做base64url加密
    
    
    # 盐 是存放在服务器的私钥，在任何场景都不应该泄露。不然客户端就可以自我签发 jwt啦
    # 注意: base64url加密是先做base64加密，然后将 - 替代 + 以及 _ 替代 /
    ~~~

###### 代码实现

~~~python
import jwt
import datetime
from jwt import exceptions
SALT = 'iv%x6xo7l7_u9bf_u!9#g#m*)*=ej@bek5)(@u3kh*72+unjv='

def create_token():
    # 构造header
    headers = {
        'typ': 'jwt',
        'alg': 'HS256'
    }
    # 构造payload
    payload = {
        'user_id': 1, # 自定义用户ID
        'username': 'wupeiqi', # 自定义用户名
        'exp': datetime.datetime.utcnow() + datetime.timedelta(minutes=5) # 超时时间
    }
    result = jwt.encode(payload=payload, key=SALT, algorithm="HS256", headers=headers).decode('utf-8')
    return result

if __name__ == '__main__':
    token = create_token()
    print(token)
~~~



##### jwt校验token

*   以后用户再来访问的时候，需要携带 token，后端需要对 token 进行校验

   * 获取 token

   * 第一步：对 token 进行切割

   * 第二步：对 payload 进行 base64url 解密，获取 payload 信息,检测token是否已经超时？

     ~~~python
     {
       "sub": "1234567890",
       "name": "John Doe",
       "admin": true
     } 
     ~~~

  * 第三步：把1,2段拼接  再次执行 HS256 加密

    ~~~python
    # 第一步: 第1,2部分密文拼接起来
    eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9
    # 第二步: 对前2部分密文进行HS256加密(不能反解) + 加盐secret, 获取密文
    
    判断 密文 是否== base64url解密(TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ) 
    如果相等，表示token未被修改.(认证通过)
    ~~~

###### 代码实现

1. 将token分割成 `header_segment`、`payload_segment`、`crypto_segment` 三部分

~~~python
jwt_token = "a.b.c"
signing_input, crypto_segment = jwt_token.rsplit(b'.', 1)  # 从右边切割
header_segment, payload_segment = signing_input.split(b'.', 1)
~~~

- 对第一部分`header_segment`进行base64url解密，得到`header`

- 对第二部分`payload_segment`进行base64url解密，得到`payload`

- 对第三部分`crypto_segment`进行base64url解密，得到`signature`

- 对第三部分`signature`部分数据进行合法性校验

  - 拼接前两段密文，即：`signing_input`

  - 从第一段明文中获取加密算法，默认：`HS256`

  - 使用 算法+盐 对`signing_input` 进行加密，将得到的结果和`signature`密文进行比较。

~~~python
import jwt
import datetime
from jwt import exceptions
def get_payload(token):
    """
    根据token获取payload
    """
    # 从token中获取payload【不校验合法性】
    # unverified_payload = jwt.decode(token, None, False)
    # print(unverified_payload)
        
    # 从token中获取payload【校验合法性】
    verified_payload = jwt.decode(token, SALT, True)
        return verified_payload
    except exceptions.ExpiredSignatureError:
        print('token已失效')
    except jwt.DecodeError:
        print('token认证失败')
    except jwt.InvalidTokenError:
        print('非法的token')
        
if __name__ == '__main__':
    token = "a.b.c"
    payload = get_payload(token)
~~~

#### 应用

* 安装

  ~~~python
  python install pyjwt
  ~~~

* 使用

  ~~~python
  jwt.encode  生成token
  
  jwt.decode  token解密
  ~~~



#### 安全相关

* 不应该在jwt的payload部分存放敏感信息，因为该部分客户端可解密
* 保护好 secret 私钥，该私钥非常重要
* 如果可以，使用 https 协议



#### 扩展

~~~python
pip3 install djangorestframework-jwt
~~~

djangorestframework-jwt 本质是调用 pyjwt 实现的

#### 友情链接

[武沛齐老师笔记](https://pythonav.com/wiki/detail/6/67/)