### Django 查询 --- id vs pk
###### 当编写 django 查询时，可以使用 主键/pk 作为查询参数
~~~python
# 当表中 id 为主键时
Object.objects.get(id=1)
Object.objects.get(pk=1)
OR 
# 当表中 uid 为主键时 
>>> Repair_Man.objects.get(uid=11)
<Repair_Man: Repair_Man object (11)>
>>> Repair_Man.objects.get(pk=11)
<Repair_Man: Repair_Man object (11)>
~~~
###### pk 代表主键 （primary key）,  pk 更加独立于实际的主键字段名称，即您不必关心主键字段是否被称为 id 或 任何。
###### 如果具有不同主键字段的模型，pk 的使用可以提高代码的一致性。

