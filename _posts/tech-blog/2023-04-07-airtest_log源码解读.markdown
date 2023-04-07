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
  + 
+ `self.logfile`
  + 日志文件的路径（默认为None）
+ `self.logfd`
  + 读取文件时的指针（默认关闭并置为None）
  

### 方法
+ `__init__()`
  + `running_stack = [], logfile = None, logfd = None`
  + 调用方法`set_logfile()`
  + 调用函数`reg_cleanup()`，将方法`handle_stacked_log`作为参数
+ `log()`
  + 
+ `set_logfile()`
  + 尝试给`logfile logfd`赋予有效值，否则保持默认
+ `handle_stacked_log()`
  + 尝试把`running_stack`中的所有元素**从尾到头**的顺序作为参数传入`log()`方法进行处理
  + 每调用完一次`log()`就pop尾部元素
+ `_dumper()`

## 函数
只有一个函数`Logwrap()`



## 额外说明
### `reg_cleanup`
这个是airtest封装的用来清空给定函数寄存器的接口
涉及队列、主从线程等

### `log()`接口参考
https://www.cnblogs.com/AirtestProject/p/16983441.html
https://www.cnblogs.com/AirtestProject/p/16964460.html
https://www.cnblogs.com/AirtestProject/p/16223928.html


