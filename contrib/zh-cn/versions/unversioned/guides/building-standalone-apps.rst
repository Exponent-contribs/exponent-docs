.. _building-standalone-apps:

************************
创建 Standalone Apps
************************

不是所有人都想让他们的顾客或者朋友先下载 Exponent 然后再用他们的 app 的; 你希望把你的 app 发布到 App Store 或者 Play Store。我们称之为 "shell apps" 或者 "standalone apps"。这篇文章会详细说明怎么创建 iOS 和 Android standalone app。

创建 iOS standalone app 的话，你需要一个 Apple 开发账户。而创建 Android standalone app 的话，你不需要 Google Play 开发账户。如果你要发布到这两个平台任一的话，你当然需要对应的开发账号。

.. epigraph::
  **Warning:** Standalone apps 还是 beta 阶段! 尽管 Android 部分已经在 `li.st <https://li.st/>`_ 上反复测试过，iOS 还有我们的自动化构建流程相对来说还比较新，可能你会遇到某些问题。如果遇到问题的话，一定要在 Slack 或者 Twitter 上告诉我们，然后我们会尽快修复，非常感谢!

1. 安装 exp
""""""""""""""

XDE 暂时还不支持 build  standalone app, 我们需要 ``exp``, 运行 ``npm install -g exp`` 来安装。

你之前没用过 ``exp`` 的话，你需要做的第一件事就是用你的 Exponent 账号登录: ``exp login``.

2. 配置 exp.json
"""""""""""""""""""""

你应用的 ``exp.json`` 必须包含下面这些字段，对比下如果有缺失的话，就需要更改了。

  .. code-block:: javascript

      {
        name: "Playground",
        iconUrl: "https://s3.amazonaws.com/exp-us-standard/rnplay/app-icon.png",
        version: "2.0.0",
        slug: "rnplay",
        sdkVersion: "8.0.0",
        ios: {
          bundleIdentifier: "org.rnplay.exp",
        },
        android: {
          package: "org.rnplay.exp",
        }
      }

  iOS 的 ``bundleIdentifier`` 还有 Android 的 ``package`` 字段用的是 DNS 倒置标记，不需要是真正的域名。
  在这里我用的是 ``org.rnplay.exp``, 因为这个 app 的网站是 rnplay.org。你可以用比如: ``com.yourcompany.appname``.

  你可能知道为什么需要 ``name``, ``iconUrl`` 还有 ``version``, 但是如果你不是很熟悉 Exponent 的话，
  你可能不太清楚 ``slug`` 还有 ``sdkVersion``. ``slug`` 是你 app 的 Javascript 的发布 url 名,
  比如 ``exp.host/@notbrent/rnplay``, notbrent 是我的名字, ``rnplay`` 就是 slug。
  ``sdkVersion`` 告诉 Exponent 用哪个运行时版本，对应的是某个 React Native 版本。

``exp.json`` 还有一些其他你可能需要的属性。我们只介绍了一些必须的。完整文档可以查看:
:ref:`Configuration with exp.json <exp>`

3. 开始 build 
""""""""""""""""""

- 在 app 目录运行 ``exp start`` 来启动 Exponent packager。
  这一步是必须的，因为为了确保是最新的版本，在 build 过程里边你的 app 必须 republish。
- App 启动以后，运行 ``exp build:android`` 或者 ``exp build:ios``.

如果 build  Android 的话
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

第一次 build 的时候你需要选择是自己上传一个 keystore 还是让我们来替你做这块儿。如果你不知道 keystore 是什么的话，就交给我们吧。或者你也可以上传你自己的 keystore。
.. code-block:: none

  [exp] No currently active or previous builds for this project.

  Would you like to upload a keystore or have us generate one for you?
  If you don't know what this means, let us handle it! :)

    1) Let Exponent handle the process!
    2) I want to upload my own keystore!

If you choose to build for iOS
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

第一次 build 的时候你需要输入你开发账号的 Apple ID, 密码 还有 你的 Apple Team ID。
我们需要这些信息来管理证书，还有管理 provisioning profiles, 然后我们可以 build 或者推送通知。

.. code-block:: none

  [exp] No currently active or previous builds for this project.

  We need your Apple ID/password to manage certificates and provisioning
  profiles from your Apple Developer account.

  What's your Apple ID? example@gmail.com
  Password? ******************
  What is your Apple Team ID (you can find that on this page:
  https://developer.apple.com/account/#/membership)? XY1234567

.. epigraph::
  **Note:** 我们暂时不支持 Apple 的双重认证。所以为了用 exp build, 你暂时需要关闭 2FA。我们在 Github 上创建了一个 2FA 支持的 issue: `#17 <https://github.com/exponentjs/exp/issues/17>`_.

下一步你需要回答是否需要我们来管理你的发布证书。和 Android keystore 一样，如果你不知道
发布证书是什么的话，就交给我们负责。

4. 等待 building 完成
"""""""""""""""""""""""""""""""""

通常需要几分钟，你可以运行 ``exp build:status`` 查看状态。当完成的时候，你会看到一个 ``.apk`` (Android)
或者 ``.ipa`` (iOS) 文件的链接。复制粘贴到你的浏览器位置栏去下载 -- ``curl`` 或者 ``wget`` 会失败，除非你
刚好知道怎么从 S3 下载 gzipped 文件。我们会修复掉这个问题。

.. epigraph::
   **Note:** iOS 我们会激活 bitcode, 所以生成的 ``.ipa`` 会比最终 App Store 上的大很多。 想了解更多的话, 可以看 `App Thinning <https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/AppThinning/AppThinning.html>`_.

5. 在设备或者虚拟机上测试
""""""""""""""""""""""""""""""""""""""

- 你可以拖拽 ``.apk`` 到你的 Android 虚拟机。
  这是测试 build 是否成功最简单的办法, 但是通常你还是要在设备上测试。
- **在 Android 设备上测试**, 确保你安装了 Android platform tools 还有 ``adb``, 然后连接你的设备，运行
  ``adb install app-filename.apk``.
- **在 iOS 设备上测试**, 你需要多做一些 :( 我们在朝更简单的生成模拟器 builds 方向努力。但是目前你需要用 Apple TestFlight。在 iTunes connect 创建一个新的 app, 选择你的 bundle identifier。然后上传你的 build, 添加测试人员, 推荐用 `pilot <https://github.com/fastlane/fastlane/tree/master/pilot>`_

6. 提交到对应商店
"""""""""""""""""""""""""""""""""""""

我们目前还没自动化这一步, 暂时你仍然需要按照 Apple 和 Google 的文档来提交你的 standalone binary 到对应商店。

.. epigraph::
   **Note:** 在提交 iTunes Store 时，你会被问是否使用 advertising identifier (IDFA)。因为 Exponent 依赖 Segment 分析，答案是是的，
   而且你必须在 Apple 提交表单上勾选几个选项，具体查看 Segement 文档: `Segment's Guide <https://segment.com/docs/sources/mobile/ios/quickstart/#step-5-submitting-to-the-app-store>`_ 

7. 应用更新
""""""""""""""""""

当你想更新应用的时候，只需要从 XDE 或者 ``exp`` publish 就可以了! 只要你不修改 ``exp.json`` 里的 ``sdkVersion``, 你的 standalone app 就会在用户下次打开 app 的时候，升级到最新代码。
如果你想修改图标或者 app 名字的话，你必须重新提交 app 到每一个商店。

如果过程中遇到什么问题的话，我们非常荣幸可以帮助你!
加入我们的 Slack, 随时问我们问题。
If you run into problems during this process, we're more than happy to help out!

.. epigraph::
  **Note:** 好奇为什么这样子可以? 我们在 app 里内嵌了一个 Exponent 运行时，而且让它永远指向你的 app 的 published URL。

  我们在这里提到了一些必须属性, 但是你可以配置更多属性，比如推送通知图标，deep-linking url scheme (参考: :ref:`the guide on exp.json <configuration>`), 我们负责 build, 所以你完全不需要打开 Xcode 或者 Android Studio。
