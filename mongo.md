## mongo
- 默认使用 `localhost:27017/test` 作为 db_address
- mongo 的 user 是绑定 database 的，所以需要指定创建 user 时的 database 进行验证，验证成功后可以访问所有数据库；也可使用`--authenticationDatabase`选项指定进行 user 验证的数据库，这时 db_address 就可以填其他的数据库
- 例如`mongo localhost:27017/test -u root -p passwd123 --authenticationDatabase admin`的意思是：
  + 连接`localhost:27017/test`这个数据库
  + 账户密码为 root:passwd123
  + 使用 admin 数据库验证此账户（因为 root 创建时指定了 admin 为验证数据库）

## pymongo
- 默认使用 admin 数据库，也就是说 auth 也会默认使用admin，MongoClient 初始化时可传入 authSource 改变这一行为：[详情](https://pymongo.readthedocs.io/en/stable/examples/authentication.html)
- `collection.remove()`只会清空 collection 中的数据，保留 collection；`collection.drop()`会直接删除 collection
