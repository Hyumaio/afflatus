- `Combine` 与 `Group` 的区别：
  - `Combine` 会组合匹配的结果返回，`Group` 会将整个匹配结果作为一个列表
  - `Combine` 不支持句中的空格
```python
pattern = Optional(Word('+-')).suppress() + nums + '.' + nums

Combine(pattern).parseString('+3.14') == ['3.14']
Group(pattern).parseString('+3.14') == [['3', '.', '14']]
```

- `Or`(`^`) 与 `MatchFirst`(`|`) 的区别：
  - `Or` 会尝试所有匹配结果，返回最长的那一个，`MatchFirst` 从左到右匹配，匹配到即返回
```python
pattern_or = Word(nums) ^ Word('12bar') ^ Word(alphanums)
pattern_matchfirst = Word(nums) | Word('12bar') | Word(alphanums)

pattern_or.parseString('21arbc') == ['21arbc']  # 尝试所有匹配，第一组匹配出 21，第二组 21arb，第三组 21arbc，第三组最长，返回第三组
pattern_matchfirst.parseString('21arbc') == ['21']  # 从左到右匹配，匹配到第一组 21 符合，即返回第一组
```

- `infixNotation` 的 `opList`：
  - 优先级为倒序
  - 类似 `MatchFirst`，匹配到即返回，不会去尝试所有可能，所以当一个 op 包含另一个 op 时，一定要保证被包含的 op 放在后面，比如 `?` 和 `?>` 两个 op，如果 `?` 放在前面就会导致无论如何都匹配不到 `?>`
  - `Op.AssoRight`：当操作数为 1 时，取右边元素，当操作数大于 1 时，在重复 op 的时候，会先计算右边
