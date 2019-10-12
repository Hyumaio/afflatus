# xlrd

+ 基本使用

```python
# 打开 excel 为一个 workbook 对象
book = open_workbook(excel)

# 打开默认 sheet（下标从 0 开始）
sheet = book.sheet_by_index(0)

# 以下三种取值方式都可以取到同一个位置的数据
print sheet.row(1)[0]  # row 0 为表头
print sheet.col(0)[1]
print sheet.cell(1, 0) # cell(rowx, colx)
# 取到的数据是一个 Cell 对象，需要 .value 才能取到值
print type(sheet.cell(1, 0))
print sheet.cell(1, 0).value

# 使用 xldate_as_datetime(xldate_obj, book.datemode)可以将类型为 xldate 的 Cell 对象转为 python 的 datetime 类型
# 需要注意的是 <xldate object>.value 取出来之后在 python 里是 float 类型，所以任意 float/int 都可以使用此函数转换为 datetime
# 测试发现小数点前每增加 1 等于增加一天，从 1900-01-01 开始计算，小数点后面的为当天时分秒毫秒
upload_time = xldate_as_datetime(sheet.row(1)[0].value, book.datemode)
print upload_time

# sheet.get_rows() 可以拿到一个 rows 的生成器
rows_gen = sheet.get_rows()
print rows_gen

# 比较数据是否相等
print next(rows_gen)  # 表头
print next(rows_gen)  # 第一行数据
# s = [row for idx, row in enumerate(rows_gen) if idx != 0]  # 生成器在上面已经用过了，所以在这里会少数据
# 正确方式，重新构建一个生成器
s = [row for idx, row in enumerate(sheet.get_rows()) if idx != 0]
print s[0]  # 第一行数据
print len(s)

# 是否需要跳过表头
skip_headers = True
if skip_headers:
    rows = (row for idx, row in enumerate(sheet.get_rows()) if idx != 0)
else:
    rows = sheet.get_rows()
for i in rows:
    print i[-1].value
    print type(i[-1].value)  # number 类型在 python 里是 float 类型
    break
```