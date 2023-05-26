---
title: Unity 转 微信小游戏 插件
date: 2023-04-25 08:00:00
categories:
- [Unity,插件]
tags:
- Unity转微信小游戏
---



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


# 编写c#脚本调用DoExport
由于asset格式的文件只能在unity中修改，所以无法调用其他语言来读取和解析`MiniGameConfig.asset`配置文件。
这是官方代码中`Assets/WX-WASM-SDK/Editor/UnityUtil.cs`的解析配置部分：
```c#
        public static WXEditorScriptObject GetEditorConf()
        {
            var path = "Assets/WX-WASM-SDK/Editor/MiniGameConfig.asset";
            var config = AssetDatabase.LoadAssetAtPath(path, typeof(WXEditorScriptObject)) as WXEditorScriptObject;
            if (config == null)
            {
                AssetDatabase.CreateAsset(EditorWindow.CreateInstance<WXEditorScriptObject>(), path);
                config = AssetDatabase.LoadAssetAtPath(path, typeof(WXEditorScriptObject)) as WXEditorScriptObject;
            }
            return config;
        }
```
对应着官方代码中`Assets/WX-WASM-SDK/Editor/WXEditorWindow`关于配置读取和保存部分：
```c#
    public class WXEditorWindow : EditorWindow
    {

        public static WXEditorScriptObject config;
        //...//
        public void LoadData()
        {

            SDKFilePath = Path.Combine(Application.dataPath, "WX-WASM-SDK", "wechat-default", "unity-sdk", "index.js");
            config = UnityUtil.GetEditorConf();
            projectName = config.ProjectConf.projectName;
            appid = config.ProjectConf.Appid;
            cdn = config.ProjectConf.CDN;
            assetLoadType = config.ProjectConf.assetLoadType;
            videoUrl = config.ProjectConf.VideoUrl;
            orientation = (int) config.ProjectConf.Orientation;
            dst = config.ProjectConf.DST;
            // streamCDN = config.ProjectConf.StreamCDN;
            bundleHashLength = config.ProjectConf.bundleHashLength;
            bundlePathIdentifier = config.ProjectConf.bundlePathIdentifier;
            bundleExcludeExtensions = config.ProjectConf.bundleExcludeExtensions;
            preloadFiles = config.ProjectConf.preloadFiles;
            developBuild = config.CompileOptions.DevelopBuild;
            autoProfile = config.CompileOptions.AutoProfile;
            scriptOnly = config.CompileOptions.ScriptOnly;
            profilingFuncs = config.CompileOptions.profilingFuncs;
            profilingMemory = config.CompileOptions.ProfilingMemory;
            deleteStreamingAssets = config.CompileOptions.DeleteStreamingAssets;
            cleanBuild = config.CompileOptions.CleanBuild;
            customNodePath = config.CompileOptions.CustomNodePath;
            webgl2 = config.CompileOptions.Webgl2;
            useAudioApi = config.SDKOptions.UseAudioApi;
            // audioPrefix = config.ProjectConf.AssetsUrl;
            useFriendRelation = config.SDKOptions.UseFriendRelation;
            bgImageSrc = config.ProjectConf.bgImageSrc;
            tex = AssetDatabase.LoadAssetAtPath<Texture>(bgImageSrc);
            memorySize = config.ProjectConf.MemorySize;
            hideAfterCallMain = config.ProjectConf.HideAfterCallMain;

            // 不常用配置，先只通过MiniGameConfig.assets修改
            dataFileSubPrefix = config.ProjectConf.dataFileSubPrefix;
            maxStorage = config.ProjectConf.maxStorage;
            defaultReleaseSize = config.ProjectConf.defaultReleaseSize;
            texturesHashLength = config.ProjectConf.texturesHashLength;
            texturesPath = config.ProjectConf.texturesPath;
            needCacheTextures = config.ProjectConf.needCacheTextures;
            loadingBarWidth = config.ProjectConf.loadingBarWidth;
            needCheckUpdate = config.ProjectConf.needCheckUpdate;
        }
        
        private void OnLostFocus()
        {

            config.ProjectConf.projectName = projectName;
            config.ProjectConf.Appid = appid;
            config.ProjectConf.CDN = cdn;
            config.ProjectConf.assetLoadType = assetLoadType;
            config.ProjectConf.VideoUrl = videoUrl;
            config.ProjectConf.Orientation = (WXScreenOritation) orientation;
            config.ProjectConf.DST = dst;
            // config.ProjectConf.StreamCDN = streamCDN;
            config.ProjectConf.bundleHashLength = bundleHashLength;
            config.ProjectConf.bundlePathIdentifier = bundlePathIdentifier;
            config.ProjectConf.bundleExcludeExtensions = bundleExcludeExtensions;
            config.ProjectConf.preloadFiles = preloadFiles;
            config.CompileOptions.DevelopBuild = developBuild;
            config.CompileOptions.AutoProfile = autoProfile;
            config.CompileOptions.ScriptOnly = scriptOnly;
            config.CompileOptions.profilingFuncs = profilingFuncs;
            config.CompileOptions.ProfilingMemory = profilingMemory;
            config.CompileOptions.DeleteStreamingAssets = deleteStreamingAssets;
            config.CompileOptions.CleanBuild = cleanBuild;
            config.CompileOptions.CustomNodePath = customNodePath;
            config.CompileOptions.Webgl2 = webgl2;
            config.SDKOptions.UseAudioApi = useAudioApi;
            // config.ProjectConf.AssetsUrl = audioPrefix;
            config.SDKOptions.UseFriendRelation = useFriendRelation;
            config.ProjectConf.bgImageSrc = bgImageSrc;
            config.ProjectConf.MemorySize = memorySize;
            config.ProjectConf.HideAfterCallMain = hideAfterCallMain;

            config.ProjectConf.dataFileSubPrefix = dataFileSubPrefix;
            config.ProjectConf.maxStorage = maxStorage;
            config.ProjectConf.defaultReleaseSize = defaultReleaseSize;
            config.ProjectConf.texturesHashLength = texturesHashLength;
            config.ProjectConf.texturesPath = texturesPath;
            config.ProjectConf.needCacheTextures = needCacheTextures;
            config.ProjectConf.loadingBarWidth = loadingBarWidth;
            config.ProjectConf.needCheckUpdate = needCheckUpdate;
        }
    }
```



