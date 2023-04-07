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
    + 读取`log_path`日志文件，将每行的日志文本存入`log`
+ `_analyse`
    1. 准备`steps , children_steps`两个空列表
    2. 读取`log`，补充这两个空列表（log文件的组成详见[airtest_log源码解读]()）
        1. `log['depth']`                             -> `depth`
        2. `log['data']['start_time'] or log['time']` -> `run_start`
        3. `log['time']`                              -> `run_end`
        4. `depth==0`                                 -> `steps.append(log)`
        5. `depth==1`                                 -> `step['__children__'] = children_steps , steps.append(deepcopy(log)) , children_steps = []`
        6. `depth==其它`                              -> `children_steps.insert(0, log)`
    3. 读取`steps`，迭代并传入方法`_translated_step()`，将结果存入`translated_steps`列表
    4. 校验`translated_steps`最后一个元素的调用栈属性`traceback`并修改测试状态属性`test_result`后返回该列表
+ `_translate_step`
    1. 初始化7个变量：
        1. `name`       <- `step['data']['name']`
        2. `title`      <- `_translate_title(name, step)`
        3. `code`       <- `_translate_code(step)`
        4. `desc`       <- `_translate_desc(step, code)`
        5. `screen`     <- `_translate_screen(step, code)`
        6. `info`       <- `_translate_info(step)`
        7. `assertion`  <- `_translate_assertion(step)`
    2. 将变量所得的值存入`translated`字典并返回
+ `_translate_title`
    1. 维护一个`title`字典，将`name`属性转换为合适的`title`属性再返回，比如`touch` 对应 `Touch`
+ `_translate_code`
    1. 若`step['tag'] != 'function'`，立即返回`None`
    2. 初始化最终的返回值`code = {"name":step['data']['name'], "args":[]}`
    3. 将`step['data']['call_args']`的键值对全加进`args`
    4. `for _, arg in enumerate(args)`（`arg`须符合`arg['value']` 为 `dict`，且`arg['value'].get['__class__'] == 'Template'`）:
        1. 若`export_dir`不为空，则把`arg['value']['filename']`对应的图片复制到`script_root`下，并设置`arg['image']为该图片的路径
        2. 尝试获取`filename`对应的图片的分辨率并赋值给'arg[resolution']`
    5. 返回`code`
+ `_translate_desc`
    1. 维护两个字典`desc , desc_zh`，将`name`属性转换为合适的描述文本再返回，比如`exists` 对应 `'断言目标图片不存在'`
    2. 当`step['tag'] != 'function'`时，立即返回`None`
+ `_translate_screen`
    1. 若`step['tag'] not in ['function', 'info']`或者`step.get('__children__')`，立即返回`None`
    2. 初始化最终的返回值`screen = { 'src':None, 'rect':[], 'pos':[], 'vector':[], 'confidence':None }`
    3. 中途视情况(`try_log_screen , _cv_match , touch , assert_exists , wait , exists , swipe`)增加多个属性`resolution, _filepath, thumbnail`，并完善`screen`
    4. 返回`screen`
+ `_translate_info`
    + 尝试获取`step`中的`traceback , log`信息并返回
+ `_translate_assertion`
    + 尝试获取`step['data']['call_args']['msg']`信息并返回
+ `get_thumbnail`
+ `get_small_name`
+ `is_pos`
+ `div_rect`
+ `_render`
    用`jinja2`渲染html[详见如何使用jinja2渲染html]()，并将渲染后的内容存入变量`html`并返回[详见airtest-html报告模板源码解读]()
    
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
    1. 接受两个参数`output_file , record_list`
    2. 调用方法`_load()`，将`log_path`的日志内容存储到`log`列表中
    3. 调用方法`_analyse()`，将`log`解析为可渲染的`dict`，存入变量`steps`
    4. 调用`get_script_info(script_path)`获取脚本内容存入变量`info`[详见airtest-cli模块源码解读]()
    5. 当录像列表不为空，视情况将录像分配给指定的导出路径或默认日志目录，并将拼接好的路径存入变量`records`
    6. 处理`static_root`的分隔符使之合法，比如`\\ --> /`、如有必要则末尾添加`/`
    7. 设置`output_file`为默认或者指定的传入参数
    8. 新建一个字典变量`data`，它包含13个键，赋值了对应的值后，`return data`
        1. `'steps' : steps`
        2. `'name' : self.script_root`
        3. `'scale' : self.scale`
        4. `'test_result':self.test_result`
        5. `'run_end' : self.run_end`
        6. `'run_start' : self.run_start`
        7. `'static_root' : self.static_root`
        8. `'lang' : self.lang`
        9. `'records' : records`
        10. `'info' : info`
        11. `'log' : self.get_relative_log(output_file)`
        12. `'console' : self.get_console(output_file)`
        13. `'data' : json.dumps(data).replace("<", "{"),replace(">", "}")`
    > 注意到`data`的`log console data`这三个键有所不同，说明如下：
    > 1. `log`，日志的相对路径
    > 2. `console`，尝试读取与导出报告同目录下的`console.txt`文件里的内容并返回
    > 3. `data`，将以上包含12个键的字典中的`"<>"`对应替换为`"{}"`，避免被认为是特殊用法
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
    5. 调用方法`report_data()`，将返回的值存入`data`
    6. 调用方法`_render()`，将html模板`template_name`、`output_file`和`**data`作为参数传入，返回方法返回的值
    
    
    
