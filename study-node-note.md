# Nodejs stream学习记录

> 目前记录 stream 、pipe、event 

根据 stream 引申出了推、拉模型，推模型：服务器主动推送数据给接收者，拉模型接收者主动拉取数据。

在 stream 中，推模型对应的是流动模式，拉模型对应暂停模式。

所有[可读流](http://nodejs.cn/api/stream.html#stream_class_stream_readable)都开始于暂停模式，可以通过以下方式：

#### 流动模式（Flowing）

- 在流动模式中，数据自动从底层系统读取，并通过 [`EventEmitter`](http://nodejs.cn/api/events.html#events_class_eventemitter) 接口的事件尽可能快地被提供给应用程序。
- 切换到流动模式
  - 添加 [`'data'`](http://nodejs.cn/api/stream.html#stream_event_data) 事件句柄。
  - 调用 [`stream.resume()`](http://nodejs.cn/api/stream.html#stream_readable_resume) 方法。
  - 调用 [`stream.pipe()`](http://nodejs.cn/api/stream.html#stream_readable_pipe_destination_options) 方法将数据发送到[可写流](http://nodejs.cn/api/stream.html#stream_class_stream_writable)。

#### 暂停模式（Paused）

- 在暂停模式中，必须显式调用 [`stream.read()`](http://nodejs.cn/api/stream.html#stream_readable_read_size) 读取数据块。
- 切换回暂停模式
  - 如果没有管道目标，则调用 [`stream.pause()`](http://nodejs.cn/api/stream.html#stream_readable_pause)。
  - 如果有管道目标，则移除所有管道目标。调用 [`stream.unpipe()`](http://nodejs.cn/api/stream.html#stream_readable_unpipe_destination) 可以移除多个管道目标。