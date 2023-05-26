---
title: airtest 日志模块源码解析
date: 2023-04-07 08:00:00
categories:
- [Python,第三方库,airtest]
tags: 
- 源码解析
---



airtest除了标准的运行日志，还有一套专门用来记录测试数据并生成报告的日志格式，主要的接口有如下几个：

# `airtest.core.helper`
## 类
仅有一个类，名为`G`
其中有一个用`AirtestLogger`实例化的成员对象，以及一些记录日志相关信息的常量成员，它们是用来记录日志的关键

## 函数
有7个函数
### `set_logdir`
设置日志文件路径
### `log`
接收的参数为:arg,timestamp,desc,snapshot。后三者顾名思义，`arg`则会被讨论是异常还是普通字符串，从而决定是否被标记为不通过。
### `logwrap`
基于类中的`AirtestLogger`实例，调用了`Logwrap`
### `device_platform`
设置类中当前设备属性
### `using`
将指定的路径加入`sys.path`，从而达到脚本间的互相引用
### `import_device_cls`
### `delay_after_operation`

## `log()`接口参考
https://www.cnblogs.com/AirtestProject/p/16983441.html
https://www.cnblogs.com/AirtestProject/p/16964460.html
https://www.cnblogs.com/AirtestProject/p/16223928.html

# `airtest.utils.logger`
## 函数
仅含两个函数，如下：
+ `init_logging()`
基于库`logging`，设置日志的样式和等级
+ `get_logger(name)`
供外部调用，获取日志对象实例

# `airtest.utils.logwraper`
## 常量
`LOGGING`，从`airtest.utils.logger.get_logger(__name__)`获取的日志对象实例

## 类
只有一个类`AirtestLogger`，继承自`object`。该对象生成的实例，也被叫做“logger”
### 变量属性
+ `self.running_stack`
  + 用于保存正在运行的测试函数的信息，以便在测试结束时将其记录到日志文件中
+ `self.logfile`
  + 日志文件的路径（默认为None）
+ `self.logfd`
  + 用于保存日志文件的文件句柄（默认关闭并置为None）
  
### 方法
+ `__init__()`
  + `running_stack = [], logfile = None, logfd = None`
  + 调用方法`set_logfile()`
  + 调用函数`reg_cleanup()`清空给定函数寄存器，将方法`handle_stacked_log`作为参数
+ `log()`
  + 用于将日志信息记录到日志文件中
  + 记录深度`depth`、`tag`、`timestamp`以及`data`等日志信息转为json后写入文件
+ `set_logfile()`
  + 用于设置日志文件的路径，并打开文件句柄
+ `handle_stacked_log()`
  + 用于处理 running_stack 中的测试函数信息，并将其记录到日志文件中
+ `_dumper()`
  + 用于将对象转换为 JSON 格式的字符串
## 函数
只有一个函数`Logwrap()`，是装饰器函数，它用于装饰测试函数，以记录测试函数的执行情况。该函数包含以下属性和方法：
+ `wrapper()`方法
  + 作为测试函数的包装器，用于记录测试函数的执行情况，并将执行情况记录到日志文件中：
    + 这些信息都放在`data`属性，由`name`,`call_args`,`start_time`,`ret`,`end_time`组成，分辨对应函数名、参数情况、开始和结束时间以及函数返回结果
    + 然后再把`data`同`tag`,`depth`,`timestamp`的内容作为参数，调用`log()`函数
  + 除此之外，该修饰器会额外判断是否含有名为`snapshot`的参数名，若有，则在函数执行完毕后调用截图函数`try_log_screen()`
# 简要总结
`AirtestLogger.log()`是最终实现写入文件的函数，可以在`.air`脚本中直接调用

`@Logwrap()`则是负责收集所修饰的函数信息，再调用`AirtestLogger.log()`

`log()`才是专门暴露给用户的接口

+ 最终写入日志的基本格式为`{tag:, depth:, timestamp:, data:{name:, call_args:, start_time:, ret:, end_time:,}}`
  + 若传入的值本身就为字典，那么就可能有更多层的嵌套关系
