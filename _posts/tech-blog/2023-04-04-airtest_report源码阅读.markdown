# airtest_report源码阅读

## 入口函数
路径：`airtest/report/report.py`
1. 
```cmd
report [script] [--outfile] [--static_root] [--log_root] [--recort] [--export] [--lang] [--plugins] [--report]
```
2. 然后传入给主函数main()

## 组成分析
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
### 全局类
+ `LogToHtml`
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
+ `get_relative_log`
+ `get_console`
+ `readFile`
+ `report_data`
+ `report`
    1. 接受四个参数
        1. self 类实例引用
        2. `template_name`
        3. `output_file`
        4. `record_list`
    2. 涉及5个类变量属性：
        1. `script_root`
        2. `script_name`
        3. `log_root`
        4. `static_root`
        5. `export_dir`
    4. 涉及3个类方法属性：
        1. `_render`
        2. `_make_export_dir`
        3. `report_data`
