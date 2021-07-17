日志分类

框架日志分为 TRACE、DEBUG、INFO、WARN、ERROR 和 SILENT 6 个级别，别在不同的场景下使用：

- logger.trace(msg): 输出一个堆栈跟踪
- logger.debug(msg) : 输出一个调试日志
- logger.info(msg): 输出一个信息日志
- logger.warn(msg): 输出一个警告日志
- logger.error(msg): 输出一个错误日志

更多可看： https://github.com/pimterry/loglevel