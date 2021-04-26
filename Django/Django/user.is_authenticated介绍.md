# user.is_authenticated介绍
##### 引出问题
~~~python
#这是我的一个view函数
def user_info(request):
	response = HttpResponse()
	user = request.user
	user_id = user.id
	if user.is_authenticated():
		is_login = 1
	else:
        is_login = 0
        
    response.write('{"is_login":%s}' % str(is_login))
    return response
~~~
我虽然已经登录，但是返回的 is_login=0 , 也就是没有登录。问题出在哪里呢？
#### A1
~~~
.is_authenticated()原理:
如果你使用is_authenticated()判断用户是否登录，那么意味着你采用了django的auth系统。那么你的登录方式最好使用django.contrib.auth中的login方法。该方法会将user_id以及user_backend放入session中存储，.is_authenticated()通过判断session中是否有user_id[或者]user_backend来判断用户是否登录。
~~~
~~~
简介login()方法:
如果通过authenticate()进行身份认证后,则此方法对request.session中简单的添加两个键值:
(1)"_auth_user_id"就是user.id
(2)"_auth_user_backend"就是user.backend
~~~
~~~
出错原因:
如果，采用自己的登录方法，那么可能没将user_id或user_backend放入session中保存。所以你的user被django认为没有登录，虽然你已经登录了。
~~~
~~~
解决办法:
最好的办法就是使用django自己的登录方法，结合该方法，判断用户是否登录，从而决定用户的行为。
~~~
#### A2
~~~
如果你要用is_authenticated()来判断用户是否登录，那么登录你也得用django.contrib.auth来处理，包括登录、登出和权限验证。
~~~
#### 应用
~~~
我自己的话，一般在session中加以标识，后面的请求每次过来都验证一下session，即可判断登录状态，session也比较好控制过期时长。
~~~
~~~python
#代码实现
def VerifyLogin(request):
	try:
		if request.session['user_id']:
        # 或
        # if request.session['user_backend']
			return True
	except:
		return False
~~~
~~~
后面的处理请求方法中，调用一下 VerifyLogin 函数即可验证状态。
~~~

##### 友情链接
[一个简单的django user.is_authenticated问题](https://www.cnblogs.com/robinunix/p/7911429.html)

