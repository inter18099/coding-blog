---
title: Flask搭配SQLAlchemy配置
layout: post
category: Python
date: 2017-12-20
---


# 首先安装 SQLAlchemy



> 很多人用 Flask-SQLAlchemy插件 ，这个教程并不涉及。

使用 pip 安装（没安装pip的需要要先安装pip）

```
pip install SQLAlchemy
```

安装完毕后检查一下是否安装成功

```
>>> import sqlalchemy
>>> sqlalchemy.__version__ 
1.1.0
```



# 安装 MySQL 的驱动

## 安装驱动

这个选择很多，根据[文档](http://docs.sqlalchemy.org/en/latest/dialects/mysql.html#unicode-encoding-decoding) 列出来的有：

* MySQL-Python
* PyMySQL
* MySQL Connector/Python
* CyMySQL
* OurSQL
* Google Cloud SQL
* PyODBC
* zxjdbc for Jython

我们这里使用 MySQL 官方提供的 MySQL Connector/Python。

```
pip install mysql-connector-python --allow-external mysql-connector-python
```

使用 —allow-external 来使 pip 从外部获取包。



## 测试驱动

1，打开Mysql

2，进去Python命令行模式

```
# import驱动:
>>> import mysql.connector
# 替换下面命令中的账号、密码和数据库名字:
>>> conn = mysql.connector.connect(user='root', password='password', database='yourdatabase')
>>> cursor = conn.cursor()
# 创建user表:
>>> cursor.execute('create table user (id varchar(20) primary key, name varchar(20))')
# 插入一行记录，注意MySQL的占位符是%s:
>>> cursor.execute('insert into user (id, name) values (%s, %s)', ['1', 'Micheal'])
>>> cursor.rowcount
1
# 提交事务:
>>> conn.commit()
>>> cursor.close()
# 运行查询:
>>> cursor = conn.cursor()
>>> cursor.execute('select * from user where id = %s', ('1',))
>>> values = cursor.fetchall()
>>> values
[('1', 'Michael')]
# 关闭Cursor和Connection:
>>> cursor.close()
True
>>> conn.close()
```



# 编写代码，整合 FLask、MySQL和SQLAlchemy

## 目录结构

├── app.py<br>
├── database.py<br>
├── main.py<br>
├── models.py<br>
├── static<br>
└── templates<br>



## main.py

```
from flask import jsonify
from flask import render_template
from flask import Flask, session, redirect, url_for, escape, request
import models
import database
from app import app

@app.route('/add/<name>/<email>')
def add(name, email):
    u = models.User(name=name, email=email)
    try:
        database.db_session.add(u)
        database.db_session.commit()
    except Exception as e:
        return 'wrong'
    return 'Add %s user successfully' % name

if __name__ == '__main__':
    database.init_db()
    app.debug = True
    app.run()
```



## models.py

```
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



## database.py

```
from sqlalchemy import create_engine
from sqlalchemy.orm import scoped_session, sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from app import app

# 替换 root, password, databasename 为你自己的)
engine = create_engine("mysql+mysqlconnector://root:password@localhost:3306/databasename?charset=utf8")

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



## app.py

```
from flask import Flask

app = Flask(__name__, instance_relative_config=True)

# 读取./instance/config.py的代码
app.config.from_pyfile('config.py')

#app.config.from_envvar('APP_CONFIG_FILE')
```



# 运行

最后运行代码，访问localhost:5000/add/abc/edf，出现一行字successfully，就算成功了。



> 参考:
>
> [廖雪峰 python 教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014320107391860b39da6901ed41a296e574ed37104752000) 
>
> [Flask 中的 SQLAlchemy 使用教程](https://inter18099.github.io/Flask%E4%B8%AD%E7%9A%84SQLAlchemy%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.html) 