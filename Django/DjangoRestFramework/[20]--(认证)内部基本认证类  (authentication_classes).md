### 内部基本认证类
###### 基于authentication_classes的认证配置, 继承内部类BaseAuthentication

![认证继承内部类BaseAuthentication](./pictures/认证继承内部类BaseAuthentication.png)

##### 注意

~~~python
* BaseAuthentication
自定义认证类，建议继承： from rest_framework.authentication import BaseAuthentication

* BasicAuthentication (不常用)
浏览器帮我们把用户账号密码base64加密，放到请求头发送给服务端[有一个浏览器自动弹出的认证对话框]

# 以下内部类均基于Django实现
* SessionAuthentication
* TokenAuthentication
* RemoteUserAuthentication
~~~

------



##### TokenAuthentication 使用步骤

* 首先在 `INSTALLED_APPS` 中添加

~~~python
INSTALLED_APPS = (
    ...
    'rest_framework.authtoken'
)
~~~

然后 `python manage.py mirate` 生成一个和 user 一对一关系的 token 表

![token表](./pictures/token表.png)

* 创建 token

  ~~~python
  # 一般在 每次用户登录的时候 创建token (可以判断 token 是否失效)
  
  from rest_framework.authtoken.models import Token
  
  token = Token.objects.create(user=...)
  print(token.key)
  ~~~

  - 扩展：

    使用信号  `signals` 生成 token

    - 如果你想让每个user都有一个自动生成的Token, 你可以简单的捕捉User的  `post_save`  信号

      ~~~python
      from django.conf import settings
      from django.db.models.signals import post_save
      from django.dispatch import receiver
      from rest_framework.authtoken.models import Token
      
      @receiver(post_save, sender=settings.AUTH_USER_MODEL)
      def create_auth_token(sender, instance=None, created=False, **kwargs):
          """
          sender表示是要保存的实例的Model
          created表示是否是新创建
    	instance表示当前保存的实例对象的引用
          """
          if created:
              Token.objects.create(user=instance)
      ~~~
      
      **注意:** 确保这段代码在一个已经 `installed` 的 model.py 中 （已将这个 models.py 所在的app添加到 settings.py 的 INSTALLED_APPS 中）
      
    - 如果你已经创建了一些user, 你可以对所有已经存在的users生成token, 如下:
    
      ~~~python
      from django.contrib.auth.models import User
      from rest_framework.authtoken.models import Token
      
      for user in User.objects.all():
          Token.objects.get_or_create(user=user)
      ~~~
    
  
* 验证 token

  在客户端的请求头中添加
  `Authorization: Token 9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b`
  
  **注意:** 上面的Token的左右两端都有空格


* request.user 和 request.auth 在后端的返回

​      `request.user` 返回一个 Django 的 user instance

  	`request.auth `会返回一个 `rest_framework.authtoken.models.Token`的 instance

没有验证通过的响应会返回 401 Unauthorized

**注意:** 如果你使用了  `TokenAuthentication`  你必须确保你的API只能用https访问



###### 友情链接

[DRF TokenAuthentication](https://blog.csdn.net/lancegentry/article/details/99936387)

































