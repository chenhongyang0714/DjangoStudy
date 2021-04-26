### 内部基本授权认证类
###### 基于permission_classes 的认证配置, 继承内部类 BasePermission

##### 注意

~~~python
* BaseAuthentication(常用)
自定义权限认证类，建议继承： from rest_framework.permissions import BasePermission

* AllowAny(不常用)
允许所有人授权认证通过

# 以下内部类均基于 Django 实现
* IsAuthenticated  # autofull项目中使用到了
* IsAdminUser
* IsAuthenticatedOrReadOnly
~~~

