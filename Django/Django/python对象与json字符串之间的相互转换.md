# python对象与json字符串之间的相互转换
## python对象转换成json字符串
Python | JSON
- | -:
dict | object
list,tuple | array
str,unicode | string
int,long,float | number
True | true
False | false
None | null
#### eg. 字典转换为json
~~~python
#字典转json数据
import json
jsonInfo = json.dumps(dictInfo)
~~~
## json字符串转换成python对象
JSON | Python
- | -:
object | dict
array | list
string | unicode
number(int) | int,long
number(real) | float
true True | 00
false | False
null | None
#### eg. json转换为字典
~~~python
#json数据转字典
import json
dictInfo = json.loads(jsonInfo)
id = dictInfo['id']
~~~


