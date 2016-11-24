.. _up-and-running:

**************
创建和运行
**************

第一个指南的目标是创建一个 Exponent 应用并尽可能快地运行起来。

关于这个，我们应该在开发机器上安装好了 XDE 和在 iOS 或者 Android （设备或虚拟机）上安装好了 Exponent 服务。如果没有，在继续之前请回去 `Installation <../introduction/installation.html>`_ 指南。

好了，让我们开始吧！

创建一个项目
"""""""""""""""""

打开 XDE 时你会被要求输入用户名和密码。用你想要的用户名和密码填好表格并按下继续 continue 按钮——如果用户名没被占用，我们会自动为你创建一个账户。

项目的创建
""""""""""""""""""""

按下 ``Project`` 并选择 ``New Project``，然后选择 ``Tab Navigation`` 选项因为这会是一个很好的开始，然后在弹出的对话窗口中输入你的项目名。在这里，我把它命名为 ``first-project``，然后按下创建。

然后，选择项目的存储位置，我把我所有的有趣的项目都保存在 ``~/coding``中，所以我找到了那个目录并按下点开。

XDE 现在已经被初始化而且选定了一个地址：它复制了基础的组件和安装了 ``react``, ``react-native`` 和 ``exponent``。

当项目已经初始化并且准备好是，你会看到 XDE 记录中出现"React packager ready"的消息。

"React packager"是一个用 `Babel <https://babeljs.io/>`_ 来编译 JavaScript 代码并提供到 Exponent 的简单 HTTP 服务。

.. epigraph::
  **注意:** 如果你使用的是 MacOS 系统而且 XDE 卡在了"Waiting for packager and tunnel to start"，你可能需要 `install watchman on your machine <https://facebook.github.io/watchman/docs/install.html#build-install>`_。最简单的方法是使用 `Homebrew <http://brew.sh/>`_, ``brew install watchman``。

在你的手机或虚拟机中打开 app。
"""""""""""""""""""""""""""""""""""""""

你会看见 XDE 显示一条像 ``http://4v-9wa.notbrent.mynewproject.exp.direct:80`` 的链接——放心地打开这个，你会看到它提供一些 JSON，这是 Exponent 提供的 manifest。
在地址栏中输入这个链接后，我们打开 Exponent 后就能打开我们的应用了。或者，点击``Send Link``，输入你的手机号码，然后再点击一次``Send Link``。打开手机的信息然后点击链接并在 Exponent 中打开。
你可以分享这个地址给任何装了 Exponent 的人，但它只有在你将项目在 XDE 打开时才有效。

想在虚拟机中打开这个 app，你可以点击 ``Device`` 按钮然后选择 ``Open on iOS Simulator`` （仅限 macOS）。
至于安卓，可以先boot，然后点击 ``Device`` 按钮然后选择 ``Open on Android``。

做出你的第一个修改
""""""""""""""""""""""""

在你的新项目中打开 ``screens/HomeScreen.js``然后改变 ``render()`` 的任意代码，你应该会看到 app 载入了这些修改。

.. _live-reload-help:
看不见你的修改？
^^^^^^^^^^^^^^^^^^^^^^^
实时修改是默认开启的，但我们还是确认一下以防某些原因它没有启用。

- 首先，确认你有了:ref:`development mode enabled in XDE <development-mode>`.
- 然后，关闭 app 然后重新打开。
- app 再次打开时，摇动你的设备来打开开发者菜单，如果你是在虚拟机上，iOS请按下 ``⌘+d`` ，安卓请按下 ``ctrl+m`` 。
- 如果你看到``Enable Live Reload``，点击它然后你的应用会重新加载，如果你看到 ``Disable Live Reload`` ，退出开发者菜单并尝试其他修改。

  .. figure:: img/developer-menu.png
    :width: 70%
    :alt: In-app developer menu

手动重载 app
-------------------------
- 如果你做完了上面的步奏但是实时加载**还是**没生效。按下 XDE 的右下角的按钮并给我们发送支持请求。
  知道我们解决这个问题，你仍然可以摇动设备然后点击 ``Reload`` 按钮，或者使用如下的如下的工具，无论是不是开发模式。

  .. figure:: img/exponent-refresh.png
    :width: 90%
    :alt: Refresh using Exponent buttons

祝贺！
----------------

你创建了一个 Exponent 项目，做了修改，并更新了它。

下一步
----------

- :ref:`Additional Resources <additional-resources>` 页面有好几个开源的 Exponent 项目，所以你可以看到一些实例。
- 阅读:ref:`Exponent SDK <exponent-sdk>` 来学习一些我们提供的现成的 API。
- 阅读我们的其他指南，例如如何实现
  :ref:`Push Notifications <push-notifications>`, 我们如何为你留意
  :ref:`Assets <all-about-assets>`，或者如何开发你可以交到 Apple 或者 Google的
  :ref:`Standalone Apps <building-standalone-apps>`。
- 在 slack 联系我们并解决你的问题。
