# pytest
- pytest 的 tmpdir 本质上是返回一个 py.path 对象，它有一个特质是：当 join 一个文件时，默认采用 lazily 模式，只有当 write 数据进这个文件时，这个文件才真的生成出来。
