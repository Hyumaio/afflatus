# python2

* 直接 print dict，显示的是 unicode，要想显示中文，可以使用 `json.dumps(\<dict\>, ensure_acsii=False, encoding="utf8")` 将其转换为 json 字符串再打印，encoding 参数可以省略，因为默认为 utf8
* 保留两位小数：`'%.2f' % <num>`
* float 除法需要 from \_\_future__ import division，否则像 6/13 这样的整数除法只能得到结果 0