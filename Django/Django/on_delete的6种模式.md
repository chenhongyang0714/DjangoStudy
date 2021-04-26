## on_delete的6种模式
~~~
on_delete 当一个被外键关联的对象被删除时，Django将模仿on_delete参数定义的SQL约束执行相应操作
~~~
~~~python
1. CASCADE: 模拟SQL语句中的ON_DELETE_CASCADE约束，将定义有外键的模型对象同时删除(该操作为当前Django版本的默认操作)

2. PROTECT: 阻止上面的删除操作，但是弹出ProtectedError异常

3. SET_NULL: 将外键字段(子表的字段)设为null,只有当字段设置了null=True、blank=True时,方可使用该值【当父表中的记录被删除的时候，子表对应外键字段的值设为null】

4. SET_DEFAULT: 将外键字段设为默认值。只有当字段设置了default参数时,方可使用(models.ForeignKey(A,on_delete=models.SET_DEFAULT,default=0)

5. DO_NOTHING: 什么也不做

6. SET: 设置为一个传递给SET()的值或者一个回调函数的返回值。注意大小写。
~~~