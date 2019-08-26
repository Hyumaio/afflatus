# python2

* 保留两位小数：`'%.2f' % <num>`
* float 除法需要 from \_\_future__ import division，否则像 6/13 这样的整数除法只能得到结果 0

## 编码问题
* 直接 print dict，显示的是 unicode，要想显示中文，可以使用 `json.dumps(\<dict\>, ensure_acsii=False, encoding="utf8")` 将其转换为 json 字符串再打印，encoding 参数可以省略，因为默认为 utf8
* python2 中 str 对象实际上存储的是 bytes，unicode 对象实际上存储的是 code points(即 \u 后面的 XXXX)。
* python3 中 str 对象实际上存储的是 code points 即 unicode，bytes 对象实际上存储的是 bytes。
* 即 python2 中的 unicode 在 python3 中为 str，python2 中的 str 在 python3 中为 bytes。
* unicode.encode('utf8') \=\= bytes, bytes.decode('utf8') \=\= unicode