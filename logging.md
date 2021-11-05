# Logging

- `logging.<info/debug...>(msg)` 会初始化一个 root logger, level 为 warning
- 当 root logger 已经存在时，`logging.basicConfig` 将不起任何作用。所以最佳实践应该是先使用 `basicConfig` 设置一个 logger，然后使用 `logger.xxx()`，而不是直接使用 `logging.xxx()`