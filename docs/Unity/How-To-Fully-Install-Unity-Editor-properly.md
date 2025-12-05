# Unity编辑器（国际版本）安装指南   
受一些不可抗力影响，中国用户目前难以获取国际版本的Unity引擎编辑器。在默认情况下，无论是在Unity国际官网（unity.com）还是在UnityHub中尝试下载国际版本的编辑器，都会被重定向至Unity中国（unity.cn）下，从而导致编辑器无法正常下载。   

然而，无论是unity开发从业者还是为使用Unity开发的游戏制作Mod的爱好者，仍有使用国际版Unity引擎的需求，这一现状为我们带来了极大的不便。   

故此，本文收集汇总了一些能够使国内用户正常获取国际版本Unity引擎编辑器和组件的方法，希望能提供一些帮助。   

### 如何分辨安装的Unity编辑器版本   
要区分自己安装的Unity引擎编辑器是国际版还是特供版本，可以根据其版本号是否包含特殊后缀 `c1` 来判断，举一个例子： 

- 国际版本 — `2022.3.62f1`     
- 特供版本 — `2022.3.62f1c1`   

特供版引擎编辑器的版本号一般会包含特殊后缀 `c1` 。这一特点对于UnityHub的版本号同样适用。  

### 编辑器安装
>⚠注意，下面提供的办法都需要在使用非中国地区的网络代理的情况下才会有效   
#### 1. 修改Unity Hub内置代理参数   
有些时候，我们在使用了网络代理服务的情况下，也无法通过UnityHub获取国际版本的Unity引擎编辑器，这是因为UnityHub内置了代理服务，其网络流量不经过我们自己的网络代理服务，导致我们的下载请求仍然被重定向到UnityCN。   

一般这种情况，将我们使用的网络代理调整为`TUN模式`来强制使UnityHub的流量经过我们使用的网络代理服务能解决这个问题。   

但是如果你尝试后无效，或者你使用的网络代理服务不支持`TUN模式`，我们仍然可以通过修改UnityHub内置代理参数的方式，来使UnityHub的网络流量经过我们的网络代理。   

以`Windows`平台为例：   
1. 打开一个文本编辑器，如记事本。输入以下文本，将 `proxy-url` 替换为正确的代理服务器 URL，并根据需要调整 Hub 安装路径   
 
    ```cmd
    @echo off
    set HTTP_PROXY=proxy-url
    set HTTPS_PROXY=proxy-url
    start "" "C:\Program Files\Unity Hub\Unity Hub.exe"
    ```   
    > ⚠如果路径中有空格，则必须在程序路径的两边使用双引号。

2. 将文件保存到易于找到的位置（例如：Windows桌面），并确保文件具有 `.cmd` 后缀（例如 launchUnityHub.cmd），运行脚本命令。   

更多的信息，可以查阅Unity官方文档：   
[Unity Documentation — 解决网络问题](https://docs.unity3d.com/cn/2022.3/Manual/upm-config-network.html)   

#### 2. 在Unity国际官网的编辑器下载存档页面   
除此之外，我们还有最保险的方式：在Unity国际官网上手动下载并安装我们想要版本的Unity引擎编辑器。Unity官网提供了Unity引擎编辑器下载的[存档页面](https://unity.com/releases/editor/archive)，可以在这里查看所有历史发行版本的Unity引擎。   

找到我们想要使用的Unity引擎版本，在该版本的`Release Note`中，提供了编辑器应用和所有组件的安装包下载，我们可以通过这个办法来获取我们想使用的Unity引擎编辑器。   

在成功安装编辑器应用和所有需要的组件后，就可以在UnityHub根据我们安装编辑器的目录，添加已安装的编辑器版本并正常使用了。   

但是，这个办法有一个弊端，一些原本UnityHub会自动配置的工具可能需要我们手动配置。比如，即使安装了Android平台的构建组件，Android SDK、OpenJDK和NDK等工具仍需要我们手动配置，否则我们无法将项目导出为Android平台的安装包。   

虽然我们可以自己手动安装并为Unity编辑器配置这些内容，但我们可能会因为版本不兼容或是其它问题引来更多的麻烦。所幸，我们仍有办法解决这个问题。   

>⚠通过UnityHub安装的Unity编辑器可在UnityHub中一键配置组件和工具   

我们可以通过下面的UnityAPI获取对应版本的发行信息：
```url
https:\\services.api.unity.com/unity/editor/release/v1/releases?version=编辑器版本号
```   
这个API会返回一个Json文本，包含了对应版本编辑器的各种信息。其中，就有编辑器所需组件和工具的相关信息。   

我们以Unity版本`2022.3.62f1`为例，在浏览器地址栏输入下面的url，并访问：   
```url
https:\\services.api.unity.com/unity/editor/release/v1/releases?version=2022.3.62f1
```   
我们将获取一个名为`release.json`的文件，在它的内容中，找到Android组件相关的内容，以OpenJDK为例：   
```json
{
    ...
    "results": [
        {
            "version": "2022.3.62f1",
            "releaseDate": "2025-05-07T06:24:12.533Z",
            ...
            "downloads": [
                {
                    ...
                    "type": "EXE",
                    "platform": "WINDOWS",
                    ...
                    "modules": [
                        ...
                        {
                            "id": "android",
                            ...
                            "subModules": [
                                {
                                    "id": "android-open-jdk-11.0.14.1+1",
                                    ...
                                    "url": "https://download.unity3d.com/download_unity/open-jdk/open-jdk-win-x64/jdk11.0.14.1-1_85218201fea144521d643808d167605d6d46cd4fe44ee4001991a3a4b76fdd64.zip",
                                    ...
                                    "destination": "{UNITY_PATH}/Editor/Data/PlaybackEngines/AndroidPlayer/OpenJDK",
                                    ...
                                },
                                ...
                            ],
                            ...
                        },
                        ...
                    ],
                    ...
                },
                ...
            ],
            ...
        }
    ]
}
```   

其中，**键为`url`字段的值就是该组件的下载链接，而键为`destination`字段的值就是该组件的安装目录，`{UNITY_PATH}` 则是指安装Unity编辑器的根目录**。   

我们根据这两个值，分别下载、并安装所有Android相关组件到目标位置后，启动Unity编辑器。在编辑器的 `Edit` -> `Preferences` -> `External Tools` 页面中与Android相关的部分，勾选选项 `xxx Installed with Unity` 选项。如果没有出现任何警告信息，则代表我们成功安装了原本需要UnityHub为我们配置的组件。   

原理上，这个方法不仅对安卓构建组件有效。所有需要额外安装 `子模块(SubModule)` 的组件都支持这样的安装方式。同样的，像语言包、项目模板以及文档等组件，也可以使用这样的方式手动安装。   

至此，我们成功安装了国际版本的Unity引擎编辑器，并且成功地配置了我们所需要的组件。Enjoy~❤

#### *第三方下载站*   
 如果上面的方法都难以成功，你还可以尝试访问第三方下载站`NoUnityCN`，其提供了Unity多个版本的下载与安装。并且，它是一个**开源项目**。   
 NoUnityCN主页地址：[`nounitycn.top`](https://www.nounitycn.top/)   
 它的Github仓库地址：[`https://github.com/NoUnityCN/NoUnityCN`](https://github.com/NoUnityCN/NoUnityCN)   

<div align="right">

  <br/>
  Purpaca
  <br/>
  2025.12.5

</div>