# Style Guide

写日志方法应该命名为相应的 level ，比如 `debug` 、`info` 等，并接受四个参数：

- `message` *(String)* ：日志消息，应为固定字符串，中间不要拼接任何变量（应该放到 `context` 里）。使用英文时应注意时态且首字母应大写：

    ```js
    // 描述接下来要做什么或正在做什么，应该用现在进行时
    logger.info('Sending emails');
    sendEmails();
    // 描述什么事情做完了，应该用一般过去时，并且没必要加 successfully 等字眼，不是 failed 自然就是成功
    logger.info('Emails sent')
    // 描述失败的情况：
    logger.info('Failed to send emails') // 或者，Sending emails failed
    ```

- `context` *(Map, optional)* ：可选，日志上下文，以 kv 形式存放日志产生时的一些关键变量等。
- `error` *(Error, optional)* ：可选，错误或异常对象。

# Logging Levels

- `debug` 级别可用于记录代码执行细节信息，应该仅用于本地开发环境，或者完全不用。
- `info` 级别可用于记录代码运行主干流程，比如记录 HTTP server 启动成功。
- `warning` 级别可用于记录预期内的、但比较重要或需要留意的信息，出现后不要求立即采取措施，比如记录危险的删除操作、错误的请求（HTTP 4xx）等。
- `error` 级别的应该用于所有非预期错误，出现时要求立即采取措施，要么修复，要么密切观察是否为偶发，要么调低级别，比如调用关键第三方服务失败、未捕获的异常等。
- 建议本地开发环境开启 `debug` 级别，staging 和 production 环境开启 `info` 级别。

# Special Notes

- 请正确使用日志级别，尤其不要滥用 `error` ，否则会淹没关键信息，干扰系统调试。经常出现但无需干预的，都不应该是 `error` ，`error` 意味着需要干预，一个健康的系统里，其数量应该非常有限。
- `message` 应尽量直白，说出问题的本质，比如不要仅是说什么操作失败了，要说为什么失败，DNS 解析超时、连接超时还是响应错误。
- 应提供尽量完整的 `context` ，比如 HTTP 请求的 url 、参数、响应状态码、响应体，等等，或者配合 `X-Request-Id` 实现完整的调用链路追踪。
