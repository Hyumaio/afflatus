# Python

- 将 postgres date 转换为 timestamp
```python
def to_unix_timestamp(expression: ColumnElement,
                      label: str = None) -> ColumnElement:
  """turn a datetime column into unix timestamp, from db"""
  element = func.extract('epoch', expression).cast(Integer)
  if label is not None:
    return element.label(label)
  return element
  
# >>> to_unix_timestamp(table.c.create_time, 'ts')
# CAST(EXTRACT(EPOCH FROM create_time) AS INTEGER) AS 'ts'
```