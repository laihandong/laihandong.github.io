如有开发者想把基于Unity开发的游戏工程想直接转为微信小游戏快速上线，非常推荐使用minigame-unity-webgl-transform这个插件。本文主要总结分析如何将该插件集成到平时的测试和发版流程。

这是官方文档的原话：
```c#
/// 脚本调用的话可修改 Assets/WX-WASM-SDK/Editor/MiniGameConfig.asset配置，然后调用WXEditorWindow 的 DoExport方法导出小游戏
```
我们就分析`MiniGameConfig.asset`配置文件和类`WXEditorWindow.DoExport`，来尝试将其集成。

# MiniGameConfig.asset
这是官方为了将unity工程转为webgl资源和小游戏工程所设计的一个配置文件，几乎包含了所有转换过程中会用到的配置：
+ Project Conf
+ SDK Options
+ Compile Options
+ Compress Texture
+ Player Prefs Keys
就我目前使用情况来看，每次发版只需要修改其中的2~3个值，其余的保持默认就已经能满足集成了

插件本身也有一个在菜单栏的工具UI入口，可以让人用鼠标点点点就可以发版了，但本文暂不讨论该方式，因为其本质也是调用了`DoExport`函数

# WXEditorWindow.DoExport
该方法只接受一个`bool`参数，`true`代表“导出WEBGL并转换为小游戏”，`false`代表“将WEBGL转换为小游戏”，**前者更为常用**。

