# aircv简介
aircv作为airtest的图像识别模块，封装了多种图像识别算法，可分为模板匹配和基于特征点的图像识别两类，各有优缺点，旨在根据不同场合使用不同算法。

+ 模板匹配：
  + 无法跨分辨率识别 
  + 一定有相对较佳的结果（即使目标不存在） 
  + 方法名：tpl

+ 基于特征点的图像识别：
  + 能跨分辨率识别 
  + 不一定有匹配结果 （特征点过少时）
  + 方法名：kaze、brisk、akaze、orb、sift、surf、brief


+ aircv图像识别模块下包含11个脚本：
  + `aircv.py`：
    +  包含基于opencv封装的基本的图片处理函数
    
  + `cal_confidence.py`：
    + 封装了两个计算2张图片相似度的通用函数
    
  + `error.py`：
    + 基于Exception，封装了自定义错误类
    
  + `keypoint_base.py`：<font color=red size=4>重要</font>
    + 基于KAZE算法思想
    + 封装了基类KeypointMatching
    
  + `keypoint_matching.py`：
    + 继承KeypointMatching
    + 继续封装了4个类KAZE/AKAZE/BRISK/ORB
    + 基于opencv，无需opencv-contrib module
    
  + `keypoint_matching_contrib.py`：
    + 继承KeypointMatching
    + 封装了3个类BRIEF/SIFT/SURF
    + 需要opencv-contrib

  + `multiscale_template_matching.py`：<font color=red size=4>重要</font>
    + 基于多尺度模板匹配思想
    + 封装了基类MultiScaleTemplateMatching
    + 继承MultiScaleTemplateMatching，封装了类MultiScaleTemplateMatching**Pre**（基于截图预设条件）

  + `sift.py`：<font color=red size=4>重要</font>
    + 基于sift算法思想
    + 封装了一系列基于特征点的图像识别算法

  + `template.py`：
    + 以**函数**形式封装模板匹配算法的一般流程

  + `template_matching.py`：
    + 以**类**的形式封装模板匹配算法的一般流程

  + `utils.py`：
    + 封装了图像预处理和工具类的公共函数
    + 包含打印运行时间的修饰器函数、检查图像识别的输入的函数、图片数据互转sting/cv2格式/np的函数以及压缩图片质量保存的函数

# 图像识别算法剖析
该部分将详细解析aircv图像识别模块下的算法的详细结构，以及对其进行合理评估和改进

## 模板匹配
在`template.py`和`template_matching.py`中，airtest对模板匹配的流程进行了代码实现，两者的流程基本一致，只不过后者在前者基础上，封装成了`TemplateMatching`类

### 脚本结构
```txt
|-template.py
|--def find_template(im_source, im_search, threshold, rgb)
|--def find_all_template(im_source, im_search, threshold, rgb, max_count)
|--def _get_confidence_from_matrix(im_source, im_search, max_loc, max_val, w, h, rgb)
|--def _get_template_result_matrix(im_source, im_search)
|--def _get_target_rectangle(left_top_pos, w, h)


|-template_matching.py
|--class TemplateMatching(属性: im_source, im_search, threshold, rgb )
|----def find_all_results(self)
|----def find_best_result(self)
|----def _get_confidence_from_matrix(self, max_loc, max_val, w, h)
|----def _get_template_result_matrix(self)
|----def _get_target_rectangle(self, left_top_pos, w, h)
```

### template.py

```python
""" 功能：在源图像上以模板匹配方式，求得目标图像结果
    参数：
      源图像：屏幕代理（默认minicap）的截图的格式
      目标图像：cv2的图片处理格式
      置信度阈值：float 筛选阈值，默认为0.8，即置信度超过0.8的结果会被认为是目标结果
      彩色模式：bool 可以进行彩色权识别
    主流程-1（find_template）：
      检验图像输入
      计算模板匹配的结果矩阵
      依次获取匹配结果
      求取可信度
      求取识别位置      
    主流程-2（find_all_template）：
      和主流程-1类似，只不过将第4、第5步重复执行，获取多个匹配结果
      貌似源代码中将屏蔽已经去除的最优结果这步给注释掉了
      同样的，
"""    
```
#### 主流程
+ 1.检验图像输入
  + 参数：
    + 源图像
    + 目标图像
  + 流程：
    + 对比两者的宽、高
    + 目标图像的宽、高，均不得超过源图像 

+ 2.计算模板匹配的结果矩阵
  + 参数：
    + 源图像
    + 目标图像
  + 流程：
    + 断言源、目标图像为np.ndarray格式
    + 将源、目标图像的颜色空间由BGR转为GRAY
    + 使用**cv2.matchTemplate**进行模板匹配
    + 返回模板匹配的结果矩阵

+ 3.依次获取匹配结果
  + 参数：
    + 上一步的结果矩阵
  + 流程：
    + 使用**cv2.minMaxLoc**获取 最值以及最值的索引

+ 4.求取可信度
  + 参数：
    + 源图像
    + 目标图像
    + 结果矩阵的最大值
    + 结果矩阵的最大值的索引
    + 目标图像的宽
    + 目标图像的高
    + 彩色模式
  + 流程：
    + 若开启彩色模式：
      + 以最大值的索引为左上角，在源图像上裁取和目标图像同大小的矩阵
      + 使用**np.clip cv2.cvtColor cv2.copyMakeBorder cv2.split**对图像进行预处理，得到三张不同通道的图片
      + 分别使用**cv2.matchTemplate**进行模板匹配，比较三个结果矩阵中的最大值，返回最小值
    + 若不开启：
      + 返回结果矩阵的最大值（没错，直接返回了传进来的参数/笑哭）

+ 5.求取识别位置  
  + 参数：
    + 结果矩阵的最大值的索引
    + 目标图像的宽
    + 目标图像的高
  + 流程：
    + 以最大值的索引为左上角
    + 以目标图像的宽、高为识别结果的矩形框的宽、高
    + 返回识别结果的矩形框的四个角点和中心点


#### cv2.matchTemplate


## 多尺度模板匹配

## 基于特征点的图像识别


