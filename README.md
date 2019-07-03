# Crawler (知至课堂)
知至课堂 Flask版本



#安装 mysqlclient 1.3.12 之前需要安装python开发包

sudo apt-get install libpython2.7-dev


### Flask-Migrate的介绍与安装：
1. 介绍：因为采用`db.create_all`在后期修改字段的时候，不会自动的映射到数据库中，必须删除表，然后重新运行`db.craete_all`才会重新映射，这样不符合我们的需求。因此flask-migrate就是为了解决这个问题，她可以在每次修改模型后，可以将修改的东西映射到数据库中。
2. 首先进入到你的虚拟环境中，然后使用`pip install flask-migrate`进行安装就可以了。
3. 使用`flask_migrate`必须借助`flask_scripts`，这个包的`MigrateCommand`中包含了所有和数据库相关的命令。
4. `flask_migrate`相关的命令：
    * `python manage.py db init`：初始化一个迁移脚本的环境，只需要执行一次。
    * `python manage.py db migrate`：将模型生成迁移文件，只要模型更改了，就需要执行一遍这个命令。
    * `python manage.py db upgrade`：将迁移文件真正的映射到数据库中。每次运行了`migrate`命令后，就记得要运行这个命令。
5. 注意点：需要将你想要映射到数据库中的模型，都要导入到`manage.py`文件中，如果没有导入进去，就不会映射到数据库中。
6. `manage.py`的相关代码：
    ```
    from flask_script import Manager
    from migrate_demo import app
    from flask_migrate import Migrate,MigrateCommand
    from exts import db
    from models import Article

    # init
    # migrate
    # upgrade
    # 模型  ->  迁移文件  ->  表

    manager = Manager(app)

    # 1. 要使用flask_migrate，必须绑定app和db
    migrate = Migrate(app,db)

    # 2. 把MigrateCommand命令添加到manager中
    manager.add_command('db',MigrateCommand)

    if __name__ == '__main__':
        manager.run()
    ```



需要先创建数据库
create database wsyu charset='utf8mb4'
映射数据库
python manage.py db init
python manage.py db migrate
python manage.py db upgrade
