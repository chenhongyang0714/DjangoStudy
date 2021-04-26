#### 基于Django实现restful api接口 (规范)

1. 接口开发规范

   ~~~python
   # a: 普通接口开发
   urlpatterns = [
       # url(r'^admin/', admin.site.urls),
       url(r'^get_order/', views.get_order),
       url(r'^add_order/', views.add_order),
       url(r'^del_order/', views.del_order),
       url(r'^update_order/', views.update_order),
   ]
   
   
   def get_order(request):
       return HttpResponse('')
   
   
   def add_order(request):
       return HttpResponse('')
   
   
   def del_order(request):
       return HttpResponse('')
   
   
   def update_order(request):
       return HttpResponse('')
   
   # b: restful 规范
       # 1. 根据method不同做不同的操作，示例：
       # 基于FBV:
       urlpatterns = [
           url(r'^order/', views.order),
       ]
   
       def order(request):
           if request.method == 'GET':
               return HttpResponse('获取订单')
           elif request.method == 'POST':
               return HttpResponse('创建订单')
           elif request.method == 'PUT':
               return HttpResponse('更新订单')
           elif request.method == 'DELETE':
               return HttpResponse('删除订单')
       
       # 基于CBV:
       urlpatterns = [
           url(r'^order/', views.OrderView.as_view()),
       ]
   
       class OrderView(View):
           def get(self,request,*args,**kwargs):
               return HttpResponse('获取订单')
   
           def post(self,request,*args,**kwargs):
               return HttpResponse('创建订单')
   
           def put(self,request,*args,**kwargs):
               return HttpResponse('更新订单')
   
           def delete(self,request,*args,**kwargs):
               return HttpResponse('删除订单') 
           
   ~~~

   