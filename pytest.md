# pytest
- pytest 的 tmpdir 本质上是返回一个 py.path 对象，它有一个特质是：当 join 一个文件时，默认采用 lazily 模式，只有当 write 数据进这个文件时，这个文件才真的生成出来。
- conftest 作用于同级以及下级目录所有文件，并可以继承外层的 conftest
- fixture 无论在 conftest 还是自己 module 中，如果名字一样会被覆盖
- 执行顺序大概是 `inner conftest > outer conftest > own`，`session > module > class > function`
