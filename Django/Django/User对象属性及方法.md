# User对象属性及方法
### 对象属性
~~~python
User实例一般从request.user，或是其他下面即将要讨论到的方法取得，它有很多属性和方法。AnonymousUser对象模拟了部分的接口，但不是全部。在把它当成真正的user对象使用前，你得检查一下user.is_autnecticated()
~~~
字段 | 说明
-|-:
id | int 类型，数据表主键
password | varchar 类型，用户密码，默认使用pbkdf2_sha256方式来存储和管理密码【修改密码的时候，不能直接操作这个属性，有专门的方法】
last_login | datatime 类型，最近一次登录的时间
is_superuser | tinyint 类型，表示该用户是否拥有所有的权限，即是否为超级管理员
username | varchar 类型，用户账号
first_name | varchar 类型，用户的名字
last_name | varchar 类型，用户的姓氏
email | varchar 类型，用户的邮箱
is_staff | 用来判断用户是否可以登录进入Admin后台系统
is_activate | tinyint 类型，用来判断用户的状态是否被激活【与其将用户删除，不如设置该字段为0】
date_joind | datetime 类型，账号创建的时间
### 方法
方法 | 描述
-|-:
is_authenticated() | 对于真实的User对象，返回True。这是一个分辨用户是否已被鉴证的方法。它并不意味着任何权限，也不检查用户是否仍是活动的。它仅说明此用户已被成功鉴证。
is_anonymous() | 对于AnonymousUser 对象返回True （对于真实的User 对象返回False ）。总的来说，比起这个方法，你应该倾向于使用is_authenticated() 方法。
get_full_name() | 返回first_name 加上last_name ，中间插入一个空格。
set_password(passwd) | 设定用户密码为指定字符串（自动处理成哈希串）。 实际上没有保存User对象。
check_password(passwd) | 如果指定的字符串与用户密码匹配则返回True。 比较时会使用密码哈希表。
get_group_permissions() | 返回一个用户通过其所属组获得的权限字符串列表。
get_all_permissions() | 返回一个用户通过其所属组以及自身权限所获得的权限字符串列表。
has_perm(perm) | 如果用户有指定的权限，则返回True ，此时perm 的格式是"package.codename" 。如果用户已不活动，此方法总是返回False 。
has_perms(perm_list) | 如果用户拥有全部 的指定权限，则返回True 。 如果用户是不活动的，这个方法总是返回False 。
has_module_perms(app_label) | 如果用户拥有给定的app_label 中的任何权限，则返回True 。如果用户已不活动，这个方法总是返回False 。
get_and_delete_messages() | 返回一个用户队列中的Message 对象列表，并从队列中将这些消息删除。
email_user(subj, msg) | 向用户发送一封电子邮件。 这封电子邮件是从DEFAULT_FROM_EMAIL 设置的地址发送的。 你还可以传送一个第三参数：from_email ，以覆盖电邮中的发送地址。
### 实例展示
~~~python
# set_password()
user = User.objects.filter(email=email).first()
user.set_password(password)
user.save()
~~~


