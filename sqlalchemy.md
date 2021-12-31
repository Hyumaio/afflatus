## SQLAlchemy

- 在 `bindparam` 使用多个条件时，通过设置 `postgres -c log_statement=all` 可以看到，数据库中实际上执行的是多条语句，sqlalchemy 里使用了 `do_executemany` 函数完成这一功能，而这一功能不支持 RETURNING，所以 fetch 会失败，这也是为什么当 `bindparam` 只有一个条件时，fetch 可以成功，因为实际上只执行了一条语句
  ```python
  # sqlalchemy/dialects/postgresql/psycopg2.py
  # do_executemany()
  # Currently, SQLAlchemy does not pass "RETURNING" statements
  # into executemany(), since no DBAPI has ever supported that
  # until the introduction of psycopg2's executemany_values, so
  # we are not yet using the fetch=True flag.
  ```