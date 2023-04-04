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
