---
title: Flask中的SQLAlchemy使用教程
layout: post
category: Python
date: 2017-12-19
---

小张注：这是我转的一篇flask配合sqlachemy的文章，网上好多文章都是写flask-sqlachemy，是辅助flask和sqlachemy运行的一个插件，用了它写法就不一样了，我因为走了弯路，找到这篇文章，就记下来。

Flask 是一个 python web micro framework。所谓微框架，主要是 flask 简洁与轻巧，自定义程度高。相比 django 更加轻量级。

之前一直折腾 django，得益于django  的 ORM 模式很好用，上手简单，使用方便。Flask里面没有原生的 orm，需要用到第三方的库，大名顶顶的 SQLALchemy正是一类 实现ORM的库。
下面简单介绍一下，Flask中如何使用sqlchemy (注意，这个和 flask-sqlalchemy 的使用还是有差别的，至于 flask-sqlalchemy 用法下一篇博客介绍。)。参考官方文档[ 在 Flask 中使用 SQLAlchemy](https://link.jianshu.com?t=http://docs.torriacg.org/docs/flask/patterns/sqlalchemy.html)

一 安装 SQLAlchemy 扩展
在 shell 命令下输入

```
pip install sqlchemy
```

检测是否安装成功:

```
import sqlchemy
sqlchemy.__version__ # 0.10.1
```

如果没有报错，则安装成功

二 显式调用
所谓显示调用，就是免去操作 sqlchemy 和 python class 的映射操作，而是由代码直接完成。关于 sqlchemy的详细使用，再另外一篇博客介绍。移步 sqlchemy的使用教程。
新建一个 flask项目。项目结构如下：

```
(env)ghost@ghost-H61M-S2V-B3:~/project/flask/fsqlauto$ tree
.
├── app.py
├── database.py
├── models.py
├── static
└── templates
                                  
2 directories, 3 files

```

我们先定义数据库连接,操作 database.py

```
# -*- coding: utf-8 -*-
                                                                
from sqlalchemy import create_engine
from sqlalchemy.orm import scoped_session, sessionmaker
from sqlalchemy.ext.declarative import declarative_base
                                                                
engine = create_engine('sqlite:///./test.db', convert_unicode=True) # 创建数据库引擎( 当前目录下保存数据库文件) 
db_session = scoped_session(sessionmaker(autocommit=False,
                                         autoflush=False,
                                         bind=engine))
Base = declarative_base()
Base.query = db_session.query_property()
                                                                
def init_db():
    # 在这里导入所有的可能与定义模型有关的模块，这样他们才会合适地
    # 在 metadata 中注册。否则，您将不得不在第一次执行 init_db() 时
    # 先导入他们。
    import models
    Base.metadata.create_all(bind=engine)

```

init_db 方法创建数据库
然后定义我们的 models.py

```
# -*- coding: utf-8 -*-
                                                              
from sqlalchemy import Column, Integer, String
from database import Base
                                                              
class User(Base):
    __tablename__ = 'users'
                                                              
    id = Column(Integer, primary_key=True)
    name = Column(String(50), unique=True)
    email = Column(String(120), unique=True)
                                                              
    def __init__(self, name=None, email=None):
        self.name = name
        self.email = email
                                                              
    def __repr__(self):
        return '%s (%r, %r)' % (self.__class__.__name__, self.name, self.email)

```

定义了一个 python class ,实际上是映射了 users 表里.
最后就是我们的应用程序主入口

```
# -*- coding: utf-8 -*-
                                                            
from flask import Flask
from database import init_db, db_session
from models import User
                                                            
app = Flask(__name__)
                                                            
@app.teardown_request
def shutdown_session(exception=None):
    db_session.remove()
                                                            
                                                            
@app.route('/')
def index():
    return 'hello world flask'
                                                            
@app.route('/add/<name>/<email>')
def add(name, email):
    u = User(name=name, email=email)
    try:
        db_session.add(u)
        db_session.commit()
    except Exception, e:
        return 'wrong'
    return 'Add %s user successfully' % name
                                                            
@app.route('/get/<name>')
def get(name):
    try:
        u = User.query.filter(User.name==name).first()
    except Exception, e:
        return 'there isnot %s' % name
    return 'hello %s' % u.name
                                                            
if __name__ == '__main__':
    init_db()
    app.debug = True
    app.run()

```

需要注意,定义了一个 @app.teardown_request 装饰器.用查询完毕后关闭数据库,具体可以参考flask文档 数据库.
这里的主方法中 init_db 主要是初始化数据库,如果数据库存在就链接读取.
add 是数据库添加记录方法, get 正好是数据库查询,更多例子请参考 sqlchemy教程

三 手动实现 ORM
上面是自动实现 orm,如果是手动,相应的文件修改如下:
database.py

```
# -*- coding: utf-8 -*-
                           
from sqlalchemy import create_engine, MetaData
from sqlalchemy.orm import scoped_session, sessionmaker
                           
                           
engine = create_engine('sqlite:///./test.db', convert_unicode=True)
metadata = MetaData()
db_session = scoped_session(sessionmaker(autocommit=False,
                                        autoflush=False,
                                        bind=engine))
                           
                           
def init_db():
    metadata.create_all(bind=engine)

```

models.py

```
from sqlalchemy import Table, Column, Integer, String
from sqlalchemy.orm import mapper
from database import metadata, db_session
                          
class User(object):
                          
    query = db_session.query_property()
                          
    def __init__(self, name=None, email=None):
        self.name = name
        self.email = email
                          
    def __repr__(self):
        return '<User %s>' % (self.name)
                          
users = Table('users', metadata,
    Column('id', Integer, primary_key=True),
    Column('name', String(50), unique=True),
    Column('email', String(120), unique=True)
)
                          
mapper(User, users)

```

model 需要手动定义 table 并指定映射
app.py

```
# -*- coding: utf-8 -*-
                    
import sqlite3
from flask import Flask
from database import *
from models import *
                    
app = Flask(__name__)
                    
                    
@app.teardown_request
def teardown_request(exception=None):
    db_session.remove()
                    
@app.route('/')
def index():
    return 'hello'
                    
@app.route('/add/<name>/<email>')
def add(name, email):
    u = User(name, email)
    try:
        db_session.add(u)    
        db_session.commit()  
    except Exception, e:
        return 'wrong'
                          
    return '%s add successful' % name
                    
@app.route('/get/<name>')
def get(name):
    try:
        u = User.query.filter(User.name==name).first()
    except Exception, e:
        return 'there isnot %s' % name
    return 'hello %s' % u.name
                                                   
                    
if __name__ == '__main__':
    init_db()
    app.debug = True
    app.run()

```

更底层的操作就是 在 flask app 里面写 sql 查询，具体和 sqlalchemy 使用方法一致