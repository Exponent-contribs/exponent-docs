*********
Debugging
*********

在虚拟机上
=============================

**替代不了真实设备上的体验还有性能**, 但是调试的时候虚拟机会更加方便。
using an emulator/simulator.

iOS
^^^

iOS 的话，可以用Xcode 自带的虚拟机，如果还没安装的话，你可以在
`Mac App Store <https://itunes.apple.com/us/app/xcode/id497799835?mt=12>`_
上安装 Xcode。

Android
^^^^^^^

Android 的话，与其标准的虚拟机, 我们推荐 Genymotion 虚拟机 -- 我们发现功能更多，更快，
用起来也更简单。

`下载 Genymotion <https://www.genymotion.com/fun-zone/>`_ (free version), 然后按照 `Genymotion installation guide <https://docs.genymotion.com/Content/01_Get_Started/Installation.htm>`_ 安装. 安装成功后, 创建一个虚拟设备 - 我们推荐 Nexus 5, 你可以指定你的 Android 版本. 准备好以后启动这个虚拟设备。

调试 Javascript
====================

你可以用 Chrome debugger tools 来调试 Exponent apps。与其在你的手机上运行 app
的 Javascript, 它会在 Chrome 的一个 webworker 来运行。你可以接着设置断点，查看变量，
运行代码，等等，就像你调试 web app 一样。

- 为保证最好的开发体验，请先在 XDE 上修改 host type 为 ``LAN`` 或者 ``localhost``. 调试用
  ``Tunnel`` 的话，可能会比较慢甚至导致你的 app 不可用。同时，确保勾选 ``Development Mode``.
  
  .. image:: img/debugging-host.png
    :width: 100%

- 如果你在用 ``LAN``, 确保你的设备和你的开发机在同一个 wifi 里。可能在某些共有网络有些问题。
  要支持 ``localhost`` 的话，iOS 必须用虚拟机，Android 则必须设备通过 usb 连接到机器。
- 在设备上打开 app 的话，可以晃动 (或者在 Mac 虚拟机上按 `Ctrl-Cmd-Z`) 来打开开发菜单。
  点击 ``Debug JS Remotely``, 会打开一个 Chrome tab, 链接是: ``http://localhost:19001/debugger-ui``.
  然后，你可以在 Javascript console 里设置断点，或者其他交互。关闭的话，可以晃动设备，
  关掉 Chrome debugging。
- ``console.log`` 的代码行数在 Chrome debugging 默认不起作用。你需要打开 Chrome Dev Tools settings,
  切换到 "Blackboxing" tab, 确保已勾选 "Blackbox content scripts", 然后选中 "Blackbox",
  添加 pattern ``exponent/src/Logs.js``.


本地调试问题解决
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

当你在 XDE 里打开一个项目，然后点击 ``Open on Android`` 的时候，XDE 会自动告诉你的设备，
``localhost:19000`` 和 ``19001`` 会连接到你的开发机器。只要你的设备一直插入，或者虚拟机
一直在运行。如果你是用 ``localhost`` 在调试，但是不管用的话，关掉 app, 重新用
``Open on Android`` 来打开 app。或者你可以手动转发端口，如果你安装了 Android developer tools
的话，可以 ``adb reverse tcp:19000 tcp:19000`` - ``adb reverse tcp:19001 tcp:19001``

Source maps 还有 async 方法
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Source maps 和 async 方法不是 100% 可靠。React Native 和 Chrome 的 source mapping 有时候
还是有些问题，所以如果你想确保你在正确的地方设置断点，你应该直接在你的代码里使用 ``debugger`` 调用。

调试 HTTP
==============

调试 app 的 HTTP 请求的话，你需要用到一个代理。下面这几个都可以:

- `Charles Proxy <https://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration/>`_ ($50 刀, 我们推荐用这个)
- `mitmproxy <https://medium.com/@rotxed/how-to-debug-http-s-traffic-on-android-7fbe5d2a34#.hnhanhyoz>`_
- `Fiddler <http://www.telerik.com/fiddler>`_

在 Android 上, `Proxy Settings <https://play.google.com/store/apps/details?id=com.lechucksoftware.proxy.proxysettings>`_
app 在 debug 和 non-debug 模式间切换很有帮助。不过很可惜还不支持 Android M。
doesn't work with Android M yet.

React Native 也有一些努力在 Chrome DevTools 上显示网络请求:
`future work <https://github.com/facebook/react-native/issues/934>`_

Hot Reloading 和 Live Reloading
================================
`Hot Module Reloading <http://facebook.github.io/react-native/blog/2016/03/24/introducing-hot-reloading.html>`_
可以重新加载代码改变，而不会丢失页面或者导航栈里的 state。激活的话，只需要晃动设备 (或者在 Mac 虚拟机上点击 `Ctrl-Cmd-Z`),
然后点击 "Enable Hot Reloading"。Live Reload 会重新加载整个 JS 环境，Hot
Module Reloading 相对来说让你的开发流程更快些。你需要确保没同时打开这两个选项，因为这是不支持的。
