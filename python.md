# Python

+ 二进制格式的数据 json 不支持，并且其中含有大量转义字符和反斜杠，不能调用 `json.dumps`，解决方法一，将二进制数据转换为 raw 字符串（可以使用 `pickle.dumps`），解决方法二，base64 encode 一下（推荐）。
+ `pip install --target=<path> <package>` 可以将包安装到指定路径下。
+ `python3 -m http.server <port> -b <address> -d <directory>` 可以实现一个简单的文件列表服务器，默认使用 80 端口，绑定 0.0.0.0，在当前文件夹生成。python2 可使用 SimpleHTTPServer 代替。如果目录下有 index.html，会自动渲染。
+ 命令行执行 python 程序时，会将当前路径加入到 sys.path 中，所以如果在子文件夹中导入父文件夹中的内容会导致 `attempted relative import beyond top-level package` 错误，需要将主文件夹的绝对路径加入到 sys.path 中（最好不要使用相对路径 `os.path.abspath('./')` 添加，因为在不同文件夹执行 os.path 相关操作时，`./` 是基于当前所在文件夹的；但 sys,path[0] 一定是脚本所在文件夹的路径，所以也可以使用相对路径 `os.path.join(sys.path[0], '..')`导入），同时注意避免有同样的命名，否则在 sys.path 前面的包会被导入，导致后面同名的包无法成功导入。
+ 如果一个实例方法的目的是生成自己的返回值————需要用到其他实例属性————而不是去修改它时，最好只是在其中引用这些实例属性；特别是当实例属性为 mutable 时更需要注意，如果不需要修改，一定要使用 deepcopy

### for 循环 list 时动态删除 list 中的元素会导致循环出现问题

通过打印即可得知，for 循环实际上是循环列表的下标，当列表在循环中被动态改变时，每一次循环的下标仍然加 1，但列表的长度发生了改变，则可能会导致循环出现问题

- 某些元素没有被循环掉就跳过了（当删除时），例如下面这段代码的结果是 `['b', 'd']`，而不是期望中的 `[]`
```python
l = list('abcde')
for idx, i in enumerate(l):
  print(f'idx: {idx}, i: {i}')
  print(f'before if-else: {l}')
  l.remove(i)
  print(f'after if-else: {l}')
print(l)
```

- 或者导致某些元素被循环多次（当增添时），例如下面这段代码会导致无限循环，而不是期望中的 `list('a11111bcde')`
```python
l = list('abcde')
for idx, i in enumerate(l):
  l.insert(idx + 1, '1')
  print(l)
```

- 解决方法是通过 `deepcopy` 拷贝出一份原列表，循环时用拷贝列表，修改时直接操作原列表；这种方法也有局限，比如 for 循环里如果在 if 中