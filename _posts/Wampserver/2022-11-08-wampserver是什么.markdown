---
title: Wampserver 概念篇
date: 2022-11-08 08:00:00
categories: 
- [Wampserver]
---



# 下载安装

需要翻墙才能下载软件，否则只能浏览网页而已（下载那一步会一直失败）
全部next就行了（最好选个好记的目录就行）
# 配置第一个网站

## 1 新建一个文件夹
    这个文件夹就是用来存放你的网站的资源、代码等等一切可以通过你的网址访问到的东西
    记住路径！（下文以"d:/Websites/Blog"为例）

## 2 修改3个文件：
+ httpd.conf
    ```
    文件在wampserver安装路径/bin/apache/apache-xx.xx/conf
    搜索DocumentRoot，修改这两个路径为存放网站的根路径，示例如下：
        DocumentRoot "d:/Websites/Blog"
        Directory "d:/Websites/Blog/"
        (注意看，后者要多个斜杠)
        
    搜索Listen，仿照你所看到的这两行，紧接其后写下：
        Listen 0.0.0.0:8080
        Listen [::0]:8080
    ```
+ httpd-vhosts.conf
    ```
    文件在wampserver安装路径/bin/apache/apache-xx.xx/conf/extra
    把已有的<VirtualHost *:80>...</VirtualHost>这些内容，复制一份接着粘贴到其下面，但需要做三处修改。
    同样的是都要修改两处，示例如下：
        DocumentRoot "d:/Websites/Blog"
        Directory "d:/Websites/Blog/"
        (注意看，后者要多个斜杠)
    不同的是，还要修改host，示例如下：
        VirtualHost *:8080
        （不要与上面的80端口重复，且不要被当前计算的应用占用的端口即可，亲测还有8081、8082都可以）
    ```
+ hosts
    ```
    文件在C:/Windows/System32/drivers/etc
    文件末尾加上两行，示例如下：
        127.0.0.1 localhost
        ::1 localhost
        （至于为什么要加这两行，我现在还不能说得太明白，继续学习吧。有的话就不用重复了）
    ```
    
## 3 新建一个php文件
```
在"d:/Websites/Blog"下新建你的第一个php文件（比如叫“test.php”），写下如下代码：
    <?php
    echo "success!"
    ?>
```

## 4 success!
保存以上所有的文件，重启一遍wampserver所有服务，在浏览器中输入`localhost:8080/test.php`，看到以下内容说明成功了！恭喜你有了第一个网站！

```
success!
```

# 其他
这只是一种方法，还有更简单的。在之后的学习中再慢慢掌握吧

# 多站点

还记得建立第一个站点时修改的httpd-vhostsconf吗？没错！继续把8080端口那块内容多复制几份，让网址。。。我还没试过，在看这篇[文章](https://blog.csdn.net/baidu_41327283/article/details/82668757)，之后再总结。（遇到问题，重启一下wampserver所有服务）

# 注意事项
php需要连接数据库时的host就是，本地创建连接时需要填的 主机 的名称，一般我们是填localhost或者是项目名称

就像这次我在工作的时候填连接名是warmserver，但这不是dbname（即，不是数据库名），数据库名是unit-platform。 那么PHP在连接数据库时，需要填的dbname就应该是unit- platform

（这是一次22年10月末的经历）