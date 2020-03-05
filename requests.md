# requests

+ post 支持两种参数传递：
	+ **json**: 以 `application/json` 方式传递，可以不显式使用 json 字符串，直接使用 dict 即可，底层会自动实现转换。
	+ **data**: 以 `application/x-www-form-urlencoded` 方式传递，即 formdata 方式。

+ 以 data 方式 post 字典时，不支持多层字典嵌套的格式，似乎是因为 formdata 不支持。建议是所有只传输数据的接口，都以 json 方式传递。

