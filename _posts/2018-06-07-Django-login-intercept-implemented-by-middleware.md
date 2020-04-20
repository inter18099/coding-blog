---
title: 中间件实现django登录拦截验证
category: Python
---

### 1，新建一个文件夹，并把中间件py文件放入

在yourproject/yourapp/目录下新建一个文件夹middleware。
新建文件middleware.py。

### 2，编辑middleware.py
```
from django.http import HttpResponseRedirect
from django.utils.deprecation import MiddlewareMixin
 
class LoginMiddleware(MiddlewareMixin):
    def process_request(self, request):
        path = request.path_info.lstrip('/')
        if (not request.user.is_authenticated()):
            print(path, type(path))
            if path[:7] == 'backend':
                return HttpResponseRedirect('/login')
```
可以看到LoginMiddleware是继承MiddlewareMixin的。
 
request.user.is_authenticated()是判断用户是否已登录的。用户指的是auth_user表中的用户。这个表是用
```
python manage.py makemigrations 
python manage.py migrate 
```
系统生成的。
 
### 3，防止出现死循环
```
if path[:7] == 'backend':
                return HttpResponseRedirect('/login’)
```
这2句意思是如果请求url中含有backend（且用户没登录），就跳转到login页面。这里需要注意，如果你不用if把login排除掉，那么会引起无限循环，游览器会报重定向次数过多错误。也就是说/login不能重定向到/login。
 
死循环图：
 
![infinite loops](http://xiaozhangzaici.com/image/django登录拦截验证-中间件实现1.jpg)
 
正确的流程：
 
![right implementation](http://xiaozhangzaici.com/image/django登录拦截验证-中间件实现2.jpg)



