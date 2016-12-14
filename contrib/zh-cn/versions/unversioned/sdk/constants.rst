Constants
=========

App 运行时的常量系统信息。

.. attribute:: Exponent.Constants.appOwnership

   返回 ``exponent``, ``standalone``, 或者 ``guest``. 如果 ``exponent``,
   app 是运行在 Exponent 客户端里。 如果
   ``standalone``, 就是 :ref:`standalone app <building-standalone-apps>`.
   如果 ``guest``, 就在从 standalone app 里的链接打开的。

.. attribute:: Exponent.Constants.exponentVersion

   当前运行的 Exponent 客户端版本。

.. attribute:: Exponent.Constants.deviceId

   当前设备还有设备上安装的 Exponent 客户端的唯一标识符。

.. attribute:: Exponent.Constants.deviceName

   设备类型，易读的。

.. attribute:: Exponent.Constants.deviceYearClass

   设备 `device year class <https://github.com/facebook/device-year-class>`_

.. attribute:: Exponent.Constants.isDevice

   如果 app 运行在设备上的话，值为 ``true``, 模拟器的话就是 ``false``.

.. attribute:: Exponent.Constants.platform

    .. attribute:: ios

        .. attribute:: platform

           当前设备的 Apple 内部 model 标识符, 比如: ``iPhone1,1``.
        .. attribute:: model

           当前设备 model 名称, 比如: ``iPhone 7 Plus``.
        .. attribute:: userInterfaceIdiom

           用户是用哪一种设备。比如: app 是运行在 iPhone 还是 iPad 上。当前支持 ``handset`` 和 ``tablet``。Apple TV 和 CarPlay 显示为 ``unsupported``.

.. attribute:: Exponent.Constants.sessionId

   当前会话唯一标识(字符串)。不同应用，同一个应用每次启动的标识都是不一样的。

.. attribute:: Exponent.Constants.statusBarHeight

   设备默认 status bar 高度。当位置跟踪在使用或者打电话的时候，不计入考虑。

.. attribute:: Exponent.Constants.systemFonts

   当前设备上所有能用的系统字体。
   A list of the system font names available on the current device.

.. attribute:: Exponent.Constants.manifest

   当前设备的 :ref:`manifest <exponent-manifest>`.

.. attribute:: Exponent.Constants.linkingUri

   当 app 是通过 deep link 打开的话，是 URI 里边没有 deep link 的前缀。这个值和 ``Exponent.Constants.appOwnership`` 有关:
   在 ``standalone`` 和 Exponent 客户端里运行可能会不一样。
