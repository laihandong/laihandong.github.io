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
      同样的，循环主体把匹配结果矩阵放在了外面，又不做最优结果的屏蔽，那么每次的最优结果append进临时存储列表，最终列表中会出现全部相同的值了
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
    + 返回**识别结果**的矩形框的四个角点和中心点


#### 关键函数
```python
# cv2.matchTemplate 只能处理灰度图
MatchTemplate(InputArray image, InputArray templ, OutputArray result, int method);

        image：输入一个待匹配的图像，支持8U或者32F，大小(H, W)，用I表示

        templ：输入一个模板图像，与image相同类型，大小(h, w)，用T表示

        result：输出保存结果的矩阵，32F类型，大小是(H-h+1, W-w+1)

        method：要使用的数据比较方法，有六种
        
        

np.clip 
cv2.cvtColor 
cv2.copyMakeBorder 
cv2.split
```
+ cv2.matchTemplate常用的数据比较方法：
  + 方法匹配方法：TM_SQDIFF
  + 归一化方差匹配方法：TM_SQDIFF_NORMED,
  + 相关性匹配方法：TM_CCORR,
  + 归一化的互相关匹配方法：TM_CCORR_NOMED,
  + 相关系数匹配方法：TM_CCOEFF,
  + 归一化的相关系数匹配方法：TM_CCOEFF_NORMED
$$R=\frac{\sum_{x^{'},y^{'}}T^{'}(x^{'},y^{'})·I^{'}(x+x^{'},y+y^{'})}{\sqrt{\sum_{x^{'},y^{'}}T^{'}(x^{'},y^{'})^2·\sum_{x^{'},y^{'}}I^{'}(x+x^{'},y+y^{'})^2}}$$


+ np.clip
+ cv2.cvtColor 
+ cv2.copyMakeBorder 
+ cv2.split

### template_matching.py
其实就是把template.py的方法，用类封装管理起来了
find_best_result()写的代码文档中，什么“基于kaze进行图像识别xxxx”，扯淡。一个模板匹配怎么和基于特征点混在一起，再说也没看到代码有相关内容


## 多尺度模板匹配
cv2的matchTemplate模板匹配应用范围很局限，多尺度的模板匹配可以考虑到图像旋转变形等多种情况，在脚本`multiscale_template_matching.py`中对此进行了实现



### 脚本结构
```txt
|-class MultiScaleTemplateMatching（属性：im_source, im_search, threshold, rgb, record_pos, resolution, scale_max, scale_step）
|--def find_all_results
|--def find_best_result
|--def _get_confidence_from_matrix(self, max_loc, w, h)
|--def _get_target_rectangle(self, left_top_pos, w, h)
|--def _resize_by_ratio(src, templ, ratio, templ_min, src_max)
|--def _org_size(max_loc, w, h, tr, sr)
|--def multi_scale_search(self, org_src, org_templ, templ_min, src_max, ratio_min, ratio_max, step, threshold, time_out)

|-class MultiScaleTemplateMatching**Pre**（继承 MultiScaleTemplateMatching）
|--重写 def find_best_result
|--新增 def _get_ratio_scope(self, src, templ, resolution)
|--新增 def get_predict_point(self, record_pos, screen_resolution)
|--新增 def _get_area_scope(self, src, templ, record_pos, resolution)

```

```python
""" 功能：在源图像上以多尺度模板匹配方式，求得最适合的目标图像结果
    参数：
      源图像：屏幕代理（默认minicap）的截图的格式
      目标图像：cv2的图片处理格式
      置信度阈值：float 筛选阈值，默认为0.8，即置信度超过0.8的结果会被认为是目标结果
      彩色模式：bool 可以进行彩色权识别
      record_pos：记录点
      resolution：分辨率 基于分辨率的多尺度
      scale_max：源图像的预设最大尺寸
      scale_step：步长
    主流程-1（MultiScaleTemplateMatching.find_best_result）：
      检验图像输入
      计算模板匹配的结果矩阵
      求取识别位置      
    主流程-2（MultiScaleTemplateMatching**Pre**.find_best_result）：
      检查截图分辨率（没有则立即返回None）（整体和主流程-1类似，不过基于分辨率做了更多处理）
      检验图像输入
      计算模板匹配的结果矩阵
      求取识别位置
"""    
```
### class MultiScaleTemplateMatching
#### 主流程

+ 1.检验图像输入：
  + 参数：
    + 源图像
    + 目标图像
  + 流程：
    + 对比两者的宽、高
    + 目标图像的宽、高，均不得超过源图像 


+ 2.计算模板匹配的结果矩阵：
  + 参数：
    + 源图像
    + 目标图像
    + 最小比例
    + 最大比例
    + 目标图像最小尺寸  
    + 源图像预设最大尺寸
    + 步长
    + 识别阈值
    + 最大等待时间
  + 流程：
    +  将源图像、目标图像处理成灰度图
    +  进行多尺度搜索（参数：源图像的灰度图，目标图像的灰度图，最小比例，最大比例，目标图像预设最小尺寸，源图像预设最大尺寸，步长，识别阈值，等待时间上限）
      ```python
      主流程：
        新建临时存储变量（最大值、最大值对应的信息、初始化比例=最小比例、开始时间）
        
        循环（当前比例 < 最大比例）：
          1.按比例缩放源、目标图像：
              根据源图像预设最大尺寸求`源图像的缩放比例` = min(预设最大尺寸/源图像尺寸的长边, 1.0) ，使用**cv2.resize**缩放源图像
              获取缩放后的源图像、目标图像的高、宽
              比较对应边的比例 （目标图像的高/源图像的高 > 目标图像的宽/源图像的宽 ？）
              根据 传入参数`比例`和上一步的比较结果，求取目标图像的缩放比例 = （源图像的高或宽*比例）/目标图像的高或宽，使用**cv2.resize**缩放目标图像
              返回（缩放后的源图像、缩放后的目标图像、源图像的缩放比例、目标图像的缩放比例）
          2.如果缩放后的目标图像的短边 > 目标图像预设最小尺寸：
              使用cv2.TM_CCOEFF_NORMED标准和cv2.resize在源图像进行模板匹配
              更新临时存储变量：最大值 = 匹配结果矩阵中的最大值、 最大值对应的信息 = （当前比例， 结果矩阵最大值， 结果矩阵最大值的索引， 当前目标图像的宽和高，当前源图像的缩放比例，当前目标图像的缩放比例）
              如果 （达到时间上限） 并且 （当前结果矩阵最大值 > 识别阈值）：
                根据源图像缩放的比例，获取结果矩阵最大值对应原来的索引，以及源图像原来的宽、高 
                根据TM_CCOEFF_NORMED标准，求取置信度
                如果（置信度 > 识别阈值），返回（置信度，最大值的索引，源图像的原始宽和高，当前比例）
          3.比例 = 比例 + 步长
        
        如果 （临时存储变量的信息仍为空）：
          返回 空空空（代表无结果）
        重复循环主题步骤2.最后条件分支里的所有内容
      ```
+ 3.求取识别位置
  + 参数：
    + 结果矩阵的最大值的索引
    + 源图像的宽
    + 源图像的高
    + 置信度
  + 流程：
    + 获取目标区域（矩形框）
    + 拼接返回匹配结果的格式（矩形框中心坐标，矩形框（4个角点），置信度）
    + 比较置信度和识别阈值，决定返回None或结果


#### 关键函数
```python
cv2.resize
TM_CCOEFF_NORMED标准计算置信度

```

### class MultiScaleTemplateMatching**Pre**
与基类区别在于，针对源图像的分辨率做了特殊处理
#### 主流程

+ 1.检验图像输入
  + 参数：
    + 源图像
    + 目标图像
    + 分辨率
  + 流程：
    + 获取源图像、目标图像的宽高
    + 对应比较两者的宽、高，目标图像的宽、高均不能超过源图像
    + 比较分辨率、目标图像的宽、高
    + 对应比较两者的宽、高，分辨率的宽、高均不能小于目标图像

+ 2.计算模板匹配的结果矩阵
  + 参数：
    + 源图像
    + 目标图像
    + 记录点
    + 分辨率
  + 流程： 
    + 设置临时变量 偏差量
    + 预测搜索区域：
      ```python
      流程：
        获取源图像、目标图像、分辨率的宽、高
        获取 预测的目标点：
          依据 记录点 和 分辨率，获得预测的目标点（策略是个一元一次方程，系数为记录点，自变量为分辨率，因变量就是预测点）
        以预测的目标点为圆心，获取 预测的范围：
          策略是 范围半径 = （源图像宽、高 乘以 目标图像宽、高）/分辨率宽、高
                范围半径 = 预设的偏差量
                以两者最大的为准
          依据范围半径，返回预测范围的左上角点、右下角点、预测范围的分辨率
      ```
    + 裁剪源图像的预测区域：
      ```python
      流程：
        获取源图像的高、宽
        获取在源图像中实际的有效区域：没有负数、源图像有足够空间
        返回剪切后的图像（函数代码文档里写了还要返回截取偏移，但没有）
      ```
    + 再次检查裁剪后的源图像的尺寸是否大于目标图像
    + 获取预测缩放比的范围
      ```python
      流程：
        获取源图像、目标图像、分辨率的高、宽
        策略：
          比例1： 取最值 = 裁剪的源图像的宽、高 / 预测范围的分辨率的宽、高
          比例2： 取最大值 = 目标图像的宽、高 / 裁剪的源图像的宽、高
          最终缩放范围 = 比例1 乘以 比例2
          返回 最终范围 与 [步长, 0.99] 的交集
      ```
    + 将裁剪后的源图像、目标图转为灰度图
    + 多尺度匹配搜索（裁剪后的源图像的灰度图，目标图的灰度图，缩放范围，步长，阈值，等待时间=1s），对返回的最大值的索引加上预测区域的左上角点坐标
    + 获取最终识别区域框
    + 拼接识别结果
    + 比较置信度和识别阈值，返回None或结果
+ 3.求取识别位置

#### 关键函数
```python
分辨率：宽x高
图像：高，宽，[通道]
```


## 基于特征点的图像识别
在`keypoint_base.py`,`keypoint_matching.py`,`keypoint_matching_contrib.py`中，airtest基于KAZE的思想，用代码对其进行了基本实现，并封装成了一个`KeypointMatching`基类。在此基础上，以不同改进的迭代器，继承封装了多个方法类。


### 脚本结构
```txt
|-keypoint_base.py（对KAZE算法用代码实现，封装为类 Keypointmatching）
|--def find_best_result(self)
|--def show_match_image(self)
|--def _cal_confidence(self, resize_img)
|--def init_detector(self)
|--def get_keypoints_and_descriptors(self, image)
|--def match_keypoints(self, des_sch, des_src)
|--def _get_key_points(self)
|--def _handle_two_good_points(self, kp_sch, kp_src, good)
|--def _handle_three_good_points(self, kp_sch, kp_src, good)
|--def _many_good_pts(self, kp_sch, kp_src, good)
|--def _get_origin_result_with_two_points(self, pts_sch1, pts_sch2, pts_src1, pts_src2)
|--def _find_homography(self, sch_pts, src_pts)
|--def _target_error_check(self, w_h_range)

|-keypoint_matching.py（继承`Keypointmatching`基类，封装了四个类）
|--类 KAZEMatching：原封不动的继承Keypointmatching
|--类 BRISKMatching：仅将detector改为了`cv2.BRISK_create`
|--类 AKAZEMatching：仅将detector改为了`cv2.AKAZE_create`
|--类 ORBMatching：仅将detector改为了`cv2.ORB_create`


|-keypoint_matching_contrib.py（继承`Keypointmatching`基类，封装了三个类（用到了opencv的拓展包，所以要对opencv做版本检查））
|--类 BRIEFMatching：仅新增了star_detector、brief_extractor
|--类 SIFTmatching：仅修改了detector，新增了matcher
|--类 SUFMatching：仅修改了detector，新增了matcher
```

### Keypointmatching类解析
+ 类属性
  + im_source 源图像
  + im_search 目标图像
  + threshold 识别阈值
  + rgb 彩色识别模式
+ 主流程：
  + 
+ 其他函数：
