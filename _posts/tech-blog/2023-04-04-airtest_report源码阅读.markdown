# airtest_report源码阅读

## 入口函数
路径：`airtest/report/report.py`
1. 使用python内置模块argparse，给当前python文件添加一系列命令行可选参数
```cmd
report [script] [--outfile] [--static_root] [--log_root] [--recort] [--export] [--lang] [--plugins] [--report]
```
2. 然后传入给主函数main()


## 主函数
1. 解析传入的命令行参数的具体的值，传入LogToHtml类，进行初始化
2. 调用LogToHtml类中的report方法，生成html报告
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
+ `nl2br`
+ `simple_report`
+ `main`
+ `timefmt`
### 全局类
+ `LogToHtml`
+ `__init__`
    + `log`
    + `script_root`
    + `script_name`
    + `log_root`
    + `static_root
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

