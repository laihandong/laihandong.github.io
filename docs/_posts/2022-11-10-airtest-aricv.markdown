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

## 多尺度模板匹配

## 基于特征点的图像识别


