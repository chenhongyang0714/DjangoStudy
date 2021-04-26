## Django 测试、生产环境下无法加载媒体资源问题
###### 在项目的 setting.py 添加
~~~python
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
~~~
###### 在 urls.py 添加
~~~python
from django.urls import re_path
from django.conf import settings
from django.views.static import serve
re_path('media/(?P<path>.*)', serve, {'document_root': settings.MEDIA_ROOT}, name='media'),
~~~
###### 参考文献
[解决Django生产环境无法加载静态文件问题的解决	](http://www.cppcns.com/jiaoben/python/257529.html)
