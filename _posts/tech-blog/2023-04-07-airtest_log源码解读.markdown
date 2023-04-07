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
只有一个类`AirtestLogger`，继承自`object`
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
  + 调用函数`reg_cleanup()`，将方法`handle_stacked_log`作为参数
+ `log()`
  + 用于将日志信息记录到日志文件中
+ `set_logfile()`
  + 用于设置日志文件的路径，并打开文件句柄
+ `handle_stacked_log()`
  + 用于处理 running_stack 中的测试函数信息，并将其记录到日志文件中
+ `_dumper()`
  + 用于将对象转换为 JSON 格式的字符串
## 函数
只有一个函数`Logwrap()`，是装饰器函数，它用于装饰测试函数，以记录测试函数的执行情况。该函数包含以下属性和方法：
+ `wrapper()`方法
  + 作为测试函数的包装器，用于记录测试函数的执行情况，并将执行情况记录到日志文件中


## 额外说明
### `reg_cleanup`
这个是airtest封装的用来清空给定函数寄存器的接口
涉及队列、主从线程等

### `log()`接口参考
https://www.cnblogs.com/AirtestProject/p/16983441.html
https://www.cnblogs.com/AirtestProject/p/16964460.html
https://www.cnblogs.com/AirtestProject/p/16223928.html

 
