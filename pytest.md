# pytest

## fixture
- pytest 的 tmpdir 本质上是返回一个 py.path 对象，它有一个特质是：当 join 一个文件时，默认采用 lazily 模式，只有当 write 数据进这个文件时，这个文件才真的生成出来。
- conftest 作用于同级以及下级目录所有文件，并可以继承外层的 conftest
- fixture 无论在 conftest 还是自己 module 中，如果名字一样会被覆盖
- 执行顺序大概是 `inner conftest > outer conftest > own`，`session > module > class > function`

## logging
- `--log-level` 参数或者 `log_level` configuration 是全局配置，会覆盖掉代码中设置的 log level，所以如果想要在代码中使用比全局配置更低的 log level，那么就不要设置全局配置，使用 `caplog.set_level/at_level` [代替](https://docs.pytest.org/en/6.2.x/logging.html#:~:text=Log%20levels%20are,prone%20to%20failure.)，另外需要注意的是，代码中设置比全局配置更高的 log level 同样也不会起作用
- `caplog.at_level` 需要在 `with` 代码块下使用，块内记录设置的 level，块外只记录默认的 level
- `log-level` 会覆盖 `log_level`
- 只有 handler level 为 `NOTSET` 的时候才可以在代码内自由修改 log level，当没有全局配置时 handler level 为 `NOTSET`，并且会一直保持为 `NOTSET` 不受影响
- 当没有设置任何 log level 时默认 log level 为 `WANING`