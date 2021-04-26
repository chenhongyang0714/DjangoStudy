### 内部基本节流类及源码分析

###### 基于 throttle_classes 的配置, 继承内部类 SimpleRateThrottle

~~~python
from rest_framework.throttling import BaseThrottle, SimpleRateThrottle
~~~





#### 源码分析

~~~python
# 源码分析顺序

import BaseThrottle -> SimpleRateThrottle -> __init__(self) -> get_rate()、self.parse_rate() -> allow_request() -> get_cache_key() -> self.cache.get()
~~~



###### 内部类

~~~python
# SimpleRateThrottle
一般我们自定义时 继承它

# AnonRateThrottle
匿名用户的限制

# UserRateThrottle
用户登录的限制

# ScopedRateThrottle
应用在局部视图上的 [不推荐， 见 autofull项目]

~~~

