## HTTP状态码
#### 简介
~~~
当浏览者访问一个网页时，浏览者的浏览器会向网页所在服务器发出请求。当浏览器接收并显示网页前，此网页所在的服务器会返回一个包含HTTP状态码的信息头（server header）用以响应浏览器的请求。
HTTP状态码由三个十进制数字组成，第一个十进制数字定义了状态码的类型，后两个数字没有分类的作用。
~~~

#### 网页状态码的获取

~~~python
# 使用 requests 模块
import requests

# url=http://localhost:8080/userList?page=1&page_size=10
code = requests.get(url).status_code  # url 为完整路径
print code 
~~~
#### HTTP状态码分类
分类 | 分类描述
- |-:
1** | 信息，服务器收到请求，需要请求者继续执行操作
2** | 成功，操作被成功接收并处理
3** | 重定向，需要进一步的操作以完成请求
4** | 客户端错误，请求包含语法错误或无法完成请求
5** | 服务器错误，服务器在处理请求的过程中发生了错误

#### 友情链接
[python接口http网络请求 返回常见statusCode(状态码)解释](https://blog.csdn.net/helloxiaozhe/article/details/82623918)

















