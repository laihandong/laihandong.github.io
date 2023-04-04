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

### LogToHtml类
#### init初始化
```python
    def __init__(self, script_root, log_root="", static_root="", export_dir=None, script_name="", logfile=None, lang="en", plugins=None):
        self.log = []
        self.script_root = script_root
        self.script_name = script_name
        if not self.script_name or os.path.isfile(self.script_root):
            self.script_root, self.script_name = script_dir_name(self.script_root)
        self.log_root = log_root or ST.LOG_DIR or os.path.join(".", DEFAULT_LOG_DIR)
        self.static_root = static_root or STATIC_DIR
        self.test_result = True
        self.run_start = None
        self.run_end = None
        self.export_dir = export_dir
        self.logfile = logfile or getattr(ST, "LOG_FILE", DEFAULT_LOG_FILE)
        self.lang = lang
        self.init_plugin_modules(plugins)
```
plugins的加载是以`__import__()`方式导入的，目前仅支持两个插件`poco.utils.airtest.report 和 airtest_selenium.report`

#### report方法
