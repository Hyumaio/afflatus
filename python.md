# Python

+ 二进制格式的数据 json 不支持，并且其中含有大量转义字符和反斜杠，不能调用 `json.dumps`，解决方法一，将二进制数据转换为 raw 字符串（可以使用 `pickle.dumps`），解决方法二，base64 encode 一下（推荐）。
+ `pip install --target=<path> <package>` 可以将包安装到指定路径下。
+ `python3 -m http.server <port> -b <address> -d <directory>` 可以实现一个简单的文件列表服务器，默认使用 80 端口，绑定 0.0.0.0，在当前文件夹生成。python2 可使用 SimpleHTTPServer 代替。如果目录下有 index.html，会自动渲染。
+ 命令行执行 python 程序时，会将当前路径加入到 sys.path 中，所以如果在子文件夹中导入父文件夹中的内容会导致 `attempted relative import beyond top-level package` 错误，需要将主文件夹的绝对路径加入到 sys.path 中（最好不要使用相对路径 `os.path.abspath('./')` 添加，因为在不同文件夹执行 os.path 相关操作时，`./` 是基于当前所在文件夹的；但 sys,path[0] 一定是脚本所在文件夹的路径，所以也可以使用相对路径 `os.path.join(sys.path[0], '..')`导入），同时注意避免有同样的命名，否则在 sys.path 前面的包会被导入，导致后面同名的包无法成功导入。