.. _how-exponent-works:

==================
Exponent 是怎么工作的
==================

只是使用 Exponent 的话, 你不需要知道这些, 许多工程师喜欢了解他们的工具究竟是怎么工作的。
我们会介绍一些重要的概念，包括:

- 应用本地开发
- 线上发布, 部署 app
- Exponent 怎么管理 SDK 更新
- 离线打开 Exponent apps

你也可以查看源码, fork, hack 然后给 Exponent 工具提交PR:
`github/@exponentjs <http://github.com/exponentjs>`_.

本地开发服务 Exponent 应用
"""""""""""""""""""""""""""""""""""""""""""""""""

.. image:: img/fetch-app-from-xde.png
  :width: 100%

有两块: Exponent 客户端和 Exponent 开发工具 (XDE 或者 or ``exp`` CLI),
我们这里只简单介绍 XDE 的部分。当你在 XDE 里打开一个 app 的时候，它会启动并管理两个后台服务: Exponent Development server 还有 React Native Packager Server.

.. epigraph::
  **Note:** XDE 同时也会启动一个隧道程序，可以让你不做防火墙修改就允许局域网外的设备访问上面这些服务。想了解更多的话，可以看 `ngrok <https://ngrok.com>`_.   

Exponent Development Server
'''''''''''''''''''''''''''

当你在 Exponent 客户端里输入 URL 的时候，你会向这个服务发起请求。
它的目的是提供 **Exponent Manifest**, 还有作为 XDE UI 和你手机上的 Exponent 客户端
(或者虚拟机) 之间的通信层。

.. _exponent-manifest:
Exponent Manifest
-----------------

下面是一个 XDE 提供的 manifest 例子。你可能立即会发现它和 ``exp.json`` 有很多字段是相同的
(如果你还没看过 exp.json 的话, 文档在: :ref:`Configuration with exp.json <exp>`)。
下面这些字段是直接从 exp.json 获取的 -- Exponent 客户端就是这样获取你的配置的。

.. code-block:: json

  {
    "name":"My New Project",
    "description":"A starter template",
    "slug":"my-new-project",
    "sdkVersion":"8.0.0",
    "version":"1.0.0",
    "orientation":"portrait",
    "primaryColor":"#cccccc",
    "iconUrl":"https://s3.amazonaws.com/exp-brand-assets/ExponentEmptyManifest_192.png",
    "notification":{
      "iconUrl":"https://s3.amazonaws.com/exp-us-standard/placeholder-push-icon.png",
      "color":"#000000"
    },
    "loading":{
      "iconUrl":"https://s3.amazonaws.com/exp-brand-assets/ExponentEmptyManifest_192.png"
    },
    "entryPoint": "main.js",
    "packagerOpts":{
      "hostType":"tunnel",
      "dev":false,
      "strict":false,
      "minify":false,
      "urlType":"exp",
      "urlRandomness":"2v-w3z",
      "lanType":"ip"
    },
    "xde":true,
    "developer":{
      "tool":"xde"
    },
    "bundleUrl":"http://packager.2v-w3z.notbrent.internal.exp.direct:80/apps/new-project-template/main.bundle?platform=ios&dev=false&strict=false&minify=false&hot=false&includeAssetFileHashes=true",
    "debuggerHost":"packager.2v-w3z.notbrent.internal.exp.direct:80",
    "mainModuleName":"main",
    "logUrl":"http://2v-w3z.notbrent.internal.exp.direct:80/logs"
  }

Manifest 上每一个字段都会告诉 Exponent 怎么去运行你的 app。App 会先获取 manifest, 然后
会显示你在 ``exp.json`` 上设置的加载图标, 然后获取你的 app 的 JavaScript (``bundleUrl`` 设定的)
-- 这个 URL 会指向 React Native Packager Server。

为了把日志输出到 XDE, Exponent XDK 拦截了一些调用，比如 ``console.log``, ``console.warn`` 等，
然后把日志提交到 manifest 里 ``logUrl`` 设定的 URL。URL 的入口是 Exponent Development Server。

React Native Packager Server
''''''''''''''''''''''''''''

如果你不用 Exponent 而只用 React Native 的话，你应该在你的项目目录运行 ``react-native start``
来启动 packager。Exponent 会代替你启动 packager, 然后把它的 ``STDOUT`` 输出传给 XDE。这个服务
有两个目的。

第一是服务你 app 里 JavaScript 编译成的单个文件，编译任何和你手机上 JavaScript 引擎不兼容的
JavaScript 代码, 比如说 JSX 不是合法 JavaScript -- 它是一个语言扩展，用来使 React 组件更
方便，它会编译成普通的方法调用 -- ``<HelloWorld />``
 会变成 ``React.createElement(HelloWorld, {}, null)`` (更多信息可以查看 `JSX in Depth
<https://facebook.github.io/react/docs/jsx-in-depth.html>`_ ). 其他语言特性比如
`async/await <https://blog.getexponent.com/react-native-meets-async-functions-3e6f81111173#.4c2517o5m>`_
在大多数引擎上都是不支持的，所以必须先编译成能在你手机上 JavaScript 引擎 (JavaScript Core) 运行的
JavaScript 代码。

第二是服务 assets。当你在 app 里引用一张图片，你会写类似的:
``<Image source={require('./assets/example.png')} />``, 除非你已经缓存过这个
asset, 不然的话你会在 XDE 日志里看到一个类似的请求:
``<START> processing asset request my-proejct/assets/example@3x.png``.
注意它会服务适配你的屏幕 DPI 的 asset, 假设存在的话。

线上发布/部署 Exponent app
""""""""""""""""""""""""""""""""""""""""""""""""""

当你发布一个 Exponent app 的时候，我们会把它编译成一个 标记为 production 的 JavaScript bundle, (最小化，减少运行时开发阶段的检查),
然后上传这个 bundle 还有其他需要的所有 assets (文档: :ref:`Assets <all-about-assets>`) 到 CloudFront。同时我们会上传你的
:ref:`Manifest <exponent-manifest>` (包括大部分你的 ``exp.json``) 到我们的服务器。

发布成功后，我们会返回给你一个URL, 你可以把他发给任何下载 Exponent 客户端的用户。

.. epigraph::
  **Note:** 发布一个 Exponent 应用并不能让它可以被公开搜索或者在其他地方被发现, 取决于你怎么分享链接。

只要发布一完成，所有你已经存在的用户都可以更新最新的代码。只要他们安装的 Exponent 客户端里支持你在 ``exp.json`` 定义的 ``sdkVersion``,
下一次他(她)们打开 app 或者 刷新的时候，就会下载最新的代码。

.. epigraph::
   **Note:** 发布 app 到 Apple App Store 或者 Google Play Store 的话, 请参考 :ref:`Building Standalone Apps<building-standalone-apps>`. 每一次你更新 SDK 版本的话，需要重新 build 你的 二进制程序。

SDK 版本
""""""""""""

一个 Exponent app 里的 ``sdkVersion`` 说明了要用到 Exponent 里边编译好的 ObjC/Java/C
里的某个版本。每一个 ``sdkVersion`` 基本上对应一个 React Native 版本加上 SDK 文档里的
Exponent 库。

Exponent 客户端应用支持多个 Exponent SDK 版本，但是 app 只能同时用一个版本。这样子你可以
今天发布，然后一行代码不改明年照样可以用，即使我们在新的版本修改或者删除某些你的 app 依赖的 API。
可以这样子是因为你的 app 运行的代码，仍然是当初你发布时的相同的编译代码。

如果你的 app 发布了一个新的 ``sdkVersion``, 如果用户还没更新到最新的 Exponent 客户端，
他们可以仍然用之前的 ``sdkVersion``.

.. epigraph::
  **Note:** 看起来最终我们会有一个策略，多长时间保留 sdkVersions, 然后在客户端里删掉非常老的版本。但是目前来说，所有东西都支持向后兼容。

打开一个已经发布的 Exponent app
"""""""""""""""""""""""""""""""

.. image:: img/fetch-app-production.png
  :width: 500

和在开发阶段打开 Exponent app 基本类似，只不过这时候我们先从 Exponent 服务器拿到 manifest, 然后 manifest 会指向 CloudFront, 再获取你的 app 的 JavaScript。     

离线打开 Exponent Apps 
"""""""""""""""""""""""""""""

Exponent 客户端会自动缓存打开过的每个 app 的最新版本。当你打开一个 Exponent app 的时候, 它会
尝试获取最新的版本，但是如果因为各种原因 (可能连不上网络) 失败的话, 它会加载最近缓存的版本。

如果你 build 一个 standalone app, standalone 二进制程序同时会包含一个已经预缓存的 JavaScript
版本，所以它可以在没有联网的情况下瞬间打开。继续往下读关于 standalone apps 的部分。

Standalone Apps
"""""""""""""""

你也可以打包你的 Exponent 应用成一个 standalone 二进制程序, 然后提交给 Apple iTunes Store 或者 Google Play.

实际上，它是在 Exponent 客户端基础上的修改, 只不过设计成转为加载单个 URL (你的 app), 不显示 Exponent 主页或者品牌。
了解更多的话，可以看 :ref:`Building Standalone Apps <building-standalone-apps>`.
