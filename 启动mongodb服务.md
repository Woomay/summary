## 启动mongodb服务

* 打开cmd窗口cd到mongodb的安装目录，如我的安装目录是`D:\mongodb`,在进入bin目录下面；执行以下命令：

  ```
  mongod --dbpath d:\mongodb\data\db   (如果不行，可能要.\mongod...)
  ```

  出现27017字样则表示已经开启成功。

  * 如果报错出现`now exiting` 字样报错，则可以去到目录`D:\mongodb\data\db`下删除`mongod.lock` 文件，再重新执行上面那条命令，即可。

* 另启一个cmd窗口cd到相同的路径下，即`D:\mongodb\bin`;执行命令：

  ```
  mongo (.\mongo)
  ```

  此时会出现connecting to : mogodb://127.0.0.1:27017字样。可在此界面执行以下命令，进行简单的mongodb操作：

  * `show dbs` ----查看mongodb
  * `use a` ----创建数据库a,切换数据库a
  * `db.a.insertOne({"key1": "value1","key2": "value2"})` ----插入一条数据
  * `db.a.find()`----查找数据库中的所有数据
  * `db.a.drop()` ----删除mongodb的数据库