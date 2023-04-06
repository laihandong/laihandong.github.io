# airtest_report源码阅读

airtest的报告组成：
```dir
大概的结构如下：
|-<sript_name_without_ext>.log
    |-log
        |-<timestamp>.jpg
        |-log.txt
    |-static
        |-css
        |-fonts
        |-image
        |-js
    |-<sript_name_without_ext>.py
    |-log.html
    |-<tpl+timestamp>.png
    
具体的例子
|-测试购买.log
    |-log                         这一部分就是完整的airtest的log日志
        |-1674877913337.jpg
        |-1674877913337_small.jpg   这是脚本运行时所截的图
        |-log.txt
    |-static                      这是html报告用到的完整的静态资源
        |-css
        |-fonts
        |-image
        |-js
    |-测试购买.py                 这是测试脚本文件
    |-log.html                    这是最终导出的html格式的报告
    |-tpl1674121024.png           这是脚本内所识别的目标图
```

## report.py脚本组成分析
### 全局变量/常量
+ `_paragraph_re`
+ `ap`
+ `args`
+ `DEFAULT_LOG_DIR`
+ `DEFAULT_LOG_FILE`
+ `STATIC_DIR`
+ `HTML_FILE`
+ `HTML_TPL`
+ `LOGGING`
### 全局函数
+ `get_parger`
    1. 接收唯一参数，类型为`argparse.ArgumentParser()`
    2. 使用python内置模块argparse，可给当前python文件添加一系列命令行可选参数，返回修改后的参数本身
    ```shell
    report [script] [--outfile] [--static_root] [--log_root] [--recort] [--export] [--lang] [--plugins] [--report]
    ```
+ `main`
    1. 接收唯一参数，类型为`argparse.ArgumentParser().parse_args()`。
    2. 解析传入的命令行参数的具体的值，传入`LogToHtml`类，进行初始化
    3. 调用`LogToHtml`类中的`report`方法，生成html报告

+ `nl2br`
+ `simple_report`（重点）
    1. 接受四个参数：
        1. `filepath`
        2. `logpath`
        3. `logfile`
        4. `output`
    2. 对参数进行检验，必要时重新赋值，传入`LogToHtml`类，进行初始化
    3. 调用`LogToHtml`类中的`report`方法，生成html报告
+ `timefmt`
### 全局类`LogToHtml`
+ `__init__`
    + `log`
    + `script_root`
    + `script_name`
    + `log_root`
    + `static_root`
    + `test_result`
    + `run_start`
    + `run_end`
    + `export_dir`
    + `logfile`
    + `lang`
+ `init_plugin_modules` plugins的加载是以`__import__()`方式导入的，目前仅支持两个插件`poco.utils.airtest.report 和 airtest_selenium.report`    
+ `_load`
+ `_analyse`
+ `_translate_step`
+ `_translate_title`
+ `_translate_code`
+ `_translate_desc`
+ `_translate_screen`
+ `_translate_info`
+ `_translate_assertion`
+ `get_thumbnail`
+ `get_small_name`
+ `is_pos`
+ `div_rect`
+ `_render`
+ `copy_tree`
+ `_make_export_dir`
    1. 设置**报告文件夹**的名字为<script name>.log
    2. 在导出路径`export_dir`下新建这个**报告文件夹**
    3. 无视错误，`shutil.rmtree`删除**报告文件夹**下的所有文件
    4. 复制`script_root`下的目录树到**报告文件夹**
    5. 复制`log_root`下的目录树到**报告文件夹**
    6. 复制`static_root`下的css/fonts/image/js四个目录到**报告文件夹**，如果不是http开头的话
    7. 返回**报告文件夹**路径、**报告文件夹**下的日志路径
+ `get_relative_log`
+ `get_console`
+ `readFile`
+ `report_data`
+ `report`
    + 接受四个参数
        + self 类实例引用
        + `template_name` 
        + `output_file`
        + `record_list`
    + 涉及5个类变量属性：
        + `script_root`
        + `script_name`
        + `log_root`
        + `static_root`
        + `export_dir`
    + 涉及3个类方法属性：
        + `_render`
        + `_make_export_dir`
        + `report_data`
    代码内容：
    1. 生成报告页面，可以加入自定义数据并且重写
    2. 根据`sript_root`拆分成路径和`sript_name`
    3. 如果`export_dir`不为空：
        1. 调用方法`_make_export_dir()`准备导出的路径文件夹和相关资源
        2. 设置`output_file`路径
        3. `static_root`不是http开头的话，设置为"static/"
    4. 如果`record_list`不为空，将`log_root`下的`mp4`文件的路径全部保存到列表
    5. 调用方法`report_data`
    
    
    
