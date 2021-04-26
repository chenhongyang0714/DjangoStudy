#### CBV的csrf

1. csrf使用情况

   ~~~python
   MIDDLEWARE = [
       'django.middleware.security.SecurityMiddleware',
       'django.contrib.sessions.middleware.SessionMiddleware',
       'django.middleware.common.CommonMiddleware',
       'django.middleware.csrf.CsrfViewMiddleware', # 全站使用csrf认证
       'django.contrib.auth.middleware.AuthenticationMiddleware',
       'django.contrib.messages.middleware.MessageMiddleware',
       'django.middleware.clickjacking.XFrameOptionsMiddleware',
   ]
   ~~~

   免除csrf认证一：

   ~~~python
   # 注意： 在dispatch方法中添加（单独加在某一个方法无效）
   
   # 方式一：
   from django.views.decorators.csrf import csrf_exempt,csrf_protect
   from django.utils.decorators import method_decorator
   class StudentsView(View):
   
       @method_decorator(csrf_exempt)
       def dispatch(self, request, *args, **kwargs):
           return super(StudentsView,self).dispatch(request, *args, **kwargs)
   
       def get(self,request,*args,**kwargs):
           print('get方法')
           return HttpResponse('GET')
   
       def post(self, request, *args, **kwargs):
           return HttpResponse('POST')
   
       def put(self, request, *args, **kwargs):
           return HttpResponse('PUT')
   
       def delete(self, request, *args, **kwargs):
           return HttpResponse('DELETE')
   
   # 方式二：
   from django.views.decorators.csrf import csrf_exempt,csrf_protect
   from django.utils.decorators import method_decorator
   
   @method_decorator(csrf_exempt,name='dispatch')
   class StudentsView(View):
   
       def get(self,request,*args,**kwargs):
           print('get方法')
           return HttpResponse('GET')
   
       def post(self, request, *args, **kwargs):
           return HttpResponse('POST')
   
       def put(self, request, *args, **kwargs):
           return HttpResponse('PUT')
   
       def delete(self, request, *args, **kwargs):
           return HttpResponse('DELETE')
   
   ~~~

   