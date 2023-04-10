airtest调用了原生jinja2模板来渲染html，下面仅讨论被jinja2模板控制的区域部分

> 由于liquid语法限制，block相关的语法块都会被jekkly识别到

# head部分
## 脚本
`{{`static_root`}}`
## title
`{{`info.title`}}`
## 数据
`{{`data|safe`}}`
## 语言
`{{`lang`}}`

# body部分
## container-fluid
### row
#### 主要内容 main
**标题：**
`{{info.title}}`
**副标题：**
`{% if not steps %}`
log不存在的提示语
`{% endif %}`
**summary:**
主要是报告抬头的一些基本信息、比如运行时间、日志链接、脚本作者等等
涉及`{{ info.desc }} {{ if console }} {{ if log }} {{ info.author }}  {{ info.name }} `
**自定义模块：**
`{{ extra_block|safe }}`

**steps:**
`{%` if steps|length >0` %}`
表头（分左右两部分）：顺序 耗时 状态    |     跳至错误步骤 筛选（全部 成功 失败 断言）
内容steps-content（左边）：没有jinja2控制的模板内容
+ step-list：
+ pageTool：
内容steps-content（右边）：没有jinja2控制的模板内容
`{%` endif `%}`

#### footer
```html
`{%` block footer `%}`
```
放一些有关网易airtest的跳转链接和图标



### 录屏 row gif-wrap show
#### menu
三个按钮：放大、缩小和关闭
#### col-md-6
`{%` if records `%}`
  `{%` for r in records `%}`
    放一个按钮：在新窗口打开
    放视频本身
  `{%` endfor `%}`
`{%` endif `%}`

### mask hide
没有jinja2控制的模板内容

## 脚本
`{{`static_root`}}`

由此可看出，由模板渲染的内容仍然有限。类似点击跳转、切换列表视图等功能都写在js文件中
