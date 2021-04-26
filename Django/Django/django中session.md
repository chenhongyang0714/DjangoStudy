# django中session
###  简介
~~~
首先，第一个用户登录到服务器之后会保存一个key:value值，就是session。
~~~
~~~
这个key，是系统随机生成的一个字符串，用来表示唯一的身份。比如：87234EFFDIDf7234D:{‘id’:1,‘username’:“zhangsan”,‘account’:0001,}。
这个value，就是key中保存的数据，默认字段：[’_auth_user_id’, ‘_auth_user_backend’, ‘_auth_user_hash’]
~~~
~~~
注意:第二个用户登录到服务器之后，key又是一个新的随机字符串。
~~~
~~~
Session的作用就是它在Web服务器上保持用户的状态信息供在任何时间从任何设备上的页面进行访问。因为浏览器不需要存储任何这种信息，所以可以使用任何浏览器。
~~~
### 使用方法
##### Django中默认支持Session，其  _内部_  提供5种类型的Session供开发者使用，下面仅有第一种的介绍
~~~python
# 在settings.py文件中，可以设置session数据的存储方式
* 数据库(默认)
* 缓存
* 文件
* 缓存+数据库
* 加密cookie
~~~
##### 启用session
~~~python
# Django项目默认启用Session。可以在settings.py文件中间件设置中查看，如下所示:
MIDDLEWARE = [
    ***
  'django.contrib.sessions.middleware.SessionMiddleware',
    ***
]
~~~
~~~
如需禁用session，将上面代码中的session中间件注释即可。
~~~
##### 数据库中的session配置
~~~
Django默认支持Session，并且默认是将Session数据存储在数据库中(保证session数据的持久化)，即:django_session表中。包括默认字段[键、值、过期时间]
~~~
~~~Python
# 存储在mysql数据库中，如下设置可以写，也可以不写，这是默认存储方式。
a. 配置settings.py
# 引擎（默认）
SESSION_ENGINE = 'django.contrib.sessions.backends.db'
# Session的cookie保存在浏览器上时的key,即:sessionid = 随机字符串(默认)
SESSION_COOKIE_NAME ＝ "sessionid" 
# Session的cookie保存的路径（默认）
SESSION_COOKIE_PATH ＝ "/"
# Session的cookie保存的域名（默认）
SESSION_COOKIE_DOMAIN = None
# 是否Https传输cookie（默认）
SESSION_COOKIE_SECURE = False 
# 是否Session的cookie只支持http传输（默认）
SESSION_COOKIE_HTTPONLY = True   
# Session的cookie失效日期（2周）（默认）
SESSION_COOKIE_AGE = 1209600 s
# 是否关闭浏览器使得Session过期（默认）
SESSION_EXPIRE_AT_BROWSER_CLOSE = False 
# 是否每次请求都保存Session，默认修改之后才保存（默认）
SESSION_SAVE_EVERY_REQUEST = False
~~~
~~~python
# 如果存储在mysql数据库中，需要在项INSTALLED_APPS中安装Session应用。
INSTALLED_APPS = [
    ***
    'django.contrib.sessions',
    ***
   ]
~~~
##### Session操作
~~~Python
b. 通过HttpRequest对象的session属性进行会话的读写操作。
def index(request):
	# 读取值
	request.session['键']
	request.session.get('键', 默认值)
	# 写入
	request.session['键'] = 值
	request.session.setdefault('键', 默认值) # 存在则不设置
    
	# 删除key为session_key的整条session数据
	del request.session['session_key']
	# 删除当前用户的所有Session数据
	request.session.delete("session_key")
    
    # 清除session缓存数据（不管缓存与数据库的同步）
    request.session.clear()
    # 将session的缓存中的数据与数据库同步
    request.session.flush()
    
	# 所有 键、值、键值对
	request.session.keys()
	request.session.values()
	request.session.items()
	request.session.iterkeys()
	request.session.itervalues()
	request.session.iteritems()
	
	# 用户session的随机字符串
	request.session.session_key
	
	# 将所有Session失效日期小于当前日期的数据删除
	request.session.clear_expired()
	
	# 检查 用户session的随机字符串 在数据库中是否存在
	request.session.exists("session_key")  
	
	request.session.set_expiry(value)
	* 如果value是个整数，session会在value秒数后失效。
	* 如果value是个datatime或timedelta，session就会在这个时间后失效。
	* 如果value是0,那么用户session的Cookie将在用户浏览器关闭时过期。
	* 如果value是None,那么session有效期将采用系统默认值，默认为两周。可以通过在settings.py中设置SESSION_COOKIE_AGE来设置全局默认值。
~~~
#### 友情链接
[Django 使用auth模块登录 如何通过session 获取用户ID](https://blog.csdn.net/weixin_39726347/article/details/88538839)
[在Django中Session的那点事](https://www.cnblogs.com/zhuifeng-mayi/p/9099811.html)
[session.flush()与session.clear()的区别及使用环境](https://blog.csdn.net/leidengyan/article/details/7514484)