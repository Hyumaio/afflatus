# Python

+ 二进制格式的数据 json 不支持，并且其中含有大量转义字符和反斜杠，不能调用 json.dumps，解决方法一，将二进制数据转换为 raw 字符串（可以使用 pickle.dumps），解决方法二，base64 encode 一下（推荐）。
+ `pip install --target=<path> <package>` 可以将包安装到指定路径下。
+ `python3 -m http.server <port> -b <address> -d <directory>` 可以实现一个简单的文件列表服务器，默认使用 80 端口，绑定 0.0.0.0，在当前文件夹生成。python2 可使用 SimpleHTTPServer 代替。如果目录下有 index.html，会自动渲染。