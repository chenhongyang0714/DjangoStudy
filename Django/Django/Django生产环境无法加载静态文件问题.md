## Django 生产环境无法加载静态文件问题
#### 起步
~~~
线上部署时因设置了 settings.DEBUG=False 会导致静态文件都是 404 的情况。主要原因是因为关闭 DEBUG 模式后，Django 便不提供静态文件服务了。
~~~
#### runserver 的启动
~~~
如果运行是通过 runserver 的方式, 那简单, 在启动 runserver 指令后追加 --insecure 选项能强制 Django 处理静态文件。
~~~
#### 其他方式启动
~~~
但如果是通过 uwsgi 或 daphne 等启动的话, 追加选项的方式就不管用了。要解决这个问题, 我们要手动去使用静态文件服务, 这种处理方式是比较推荐的, 因为它同时也支持了 runserver 的运行方式。
~~~
~~~python
# 情景一 (适应于项目的所有静态文件都在一个static文件夹下时)
1. 在项目的 setting.py 中添加
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
并执行 python manage.py collectstatic 复制项目的所有静态文件到 static 文件夹下
2. 在 urls.py 中导入包
from django.urls import re_path
from django.conf import settings
from django.views.static import serve
3. 在 urlpatterns 中添加指向静态文件的路由规则
re_path('static/(?P<path>.*)', serve, {'document_root': settings.STATIC_ROOT}, name='static'),
~~~
~~~python
# 情景二 (适应于通过 vue 生成dist时)
1. 在项目的 setting.py 中添加
STATIC_ROOT = os.path.join(BASE_DIR, 'dist/static')
2. 在 urls.py 中导入包
from django.urls import re_path
from django.conf import settings
from django.views.static import serve
3. 在 urlpatterns 中添加指向静态文件的路由规则
re_path('static/(?P<path>.*)', serve, {'document_root': settings.STATIC_ROOT}, name='static'),
4. 修改 settings.py 中的 TEMPLATES 配置信息
'DIRS': [os.path.join(BASE_DIR, 'dist')]
5. 在 views.py 中添加
def index(request):
    """
    用于访问 index.html
    """
    return render(request, 'index.html')
~~~
###### 参考文献
[解决Django生产环境无法加载静态文件问题的解决	](http://www.cppcns.com/jiaoben/python/257529.html)










