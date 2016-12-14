.. _logging:

************
查看日志
************

在 Exponent 应用里写日志和浏览器里是一样的: 使用 ``console.log``, ``console.warn`` 还有 ``console.error``.
注意: 暂时在 remote debugging mode 外我们不支持 ``console.table``.

推荐: 用 Exponent 工具查看日志
==========================================

当你打开一个 XDE 或者 exp 服务的 app, 这个 app 会发送日志到 server, 然后你就可以方便的查看。这就意味着你不需要非得设备连接在你的电脑上才可以看到日志 -- 实际上，即使地球另外一端的某个家伙打开了应用，你也可以看到他们设备上你的应用日志。

.. epigraph::
  **Note:** 没有看到日志? 确保你使用至少 Exponent sdkVersion 7.0.0, 安装了 ``exponent`` npm 包, 而且已经导入 (比如: 在你的 main JS 顶部有 ``import * as Exponent from 'exponent'``)。
   
XDE 日志窗口
^^^^^^^^^^^^^^^^

.. figure:: img/xde-logs.png
  :width: 100%
  :alt: XDE window with logs

  在 XDE 里你会注意到，当你打开了一个 sdkVersion >= 7.0.0 的 app, 日志窗口会一分为二。你的 app 日志在右边，packager 日志在左边。

.. figure:: img/xde-logs-device-picker.png
  :width: 100%
  :alt: XDE window with device picker selected

  XDE 也让你可以在打开这个 app 的不同设备间切换。

exp logs
^^^^^^^^

如果你用我们的命令行 ``exp`` 的话，你也可以用 ``exp logs`` 来查看日志 (确保已经启动你的服务, 在项目目录 ``exp start``)。

.. figure:: img/exp-logs.png
  :width: 100%
  :alt: Terminal output from running xde logs

  所有连接设备的Packager 日志还有 app 日志都会显示在这个屏幕, ``CTRL+C`` 退出。

可选: 手动获取设备日志
=====================================

通常不是必须的，但是如果你需要查看设备上所有日志，甚至包括其他 app 还有系统本身的日志, 你可以用下面的一些方法。

查看 iOS 虚拟机日志
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

第 1 种方式: 使用 GUI log
""""""""""""""""""""""

* 在虚拟机里, 敲 ``⌘ + /``, *或者* 点击 ``Debug -> Open System Log`` -- 这两种方式都会打开一个日志窗口，显示你设备里的所有日志，包含你的 Exponent app 的日志。

第 2 种方式: 在终端打开
""""""""""""""""""""""""""""""

* 运行 ``instruments -s devices``
* 找到你在用的虚拟机的设备 / OS 版本， 比如: ``iPhone 6s (9.2) [5083E2F9-29B4-421C-BDB5-893952F2B780]``
* 后面方括号里的是设备代码， 接下来你需要做的是: ``tail -f ~/Library/Logs/CoreSimulator/DEVICE_CODE/system.log``, 比如: ``tail -f ~/Library/Logs/CoreSimulator/5083E2F9-29B4-421C-BDB5-893952F2B780/system.log``

查看你的 iPhone 日志
^^^^^^^^^^^^^^^^^^^^^^^^^^^

* ``brew install libimobiledevice``
* 插上你的手机
* ``idevicepair pair``
* 点击接受
* 运行 ``idevicesyslog``

查看 Android 设备日志或者虚拟机
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* 确保已安装 Android SDK
* 确保 `设备上 USB 调试已打开 <https://developer.android.com/studio/run/device.html#device-developer-options>`_ (对虚拟机来说没必要).
* 运行 ``adb logcat``
