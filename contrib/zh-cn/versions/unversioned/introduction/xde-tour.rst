.. _xde-tour:

********
XDE 浏览
********

登录
^^^^^^^^^^^^^^

.. figure:: img/xde-signin.png
  :width: 100%
  :alt: XDE sign in

  你第一次打开 XDE 的时候，你看到的会是这个页面。如果你已经有一个账户的话，填进去然后登录。没有的话，填写你想要的用户名还有密码然后登录。如果用户名可用的话，我们就会为你创建这个账户。

主页
^^^^^^^^^^^

.. figure:: img/xde-signin-success.png
  :width: 100%
  :alt: XDE home

  成功登录进来了! 现在你可能想创建一个新的项目，或者打开已经存在的项目。为了方便我们会列举你最近打开的一些项目。

项目对话框
^^^^^^^^^^^^^^

.. figure:: img/xde-project-dialog.png
  :width: 100%
  :alt: XDE home project dialog

  点击 *Project*, 你可以看到几个操作。当然你不可以 *Close project* 或者 *Show in Finder* 等，因为你还没有打开任何项目。

退出，需要的话
^^^^^^^^^^^^^^^^^^^^^

.. figure:: img/xde-signout.png
  :width: 100%
  :alt: XDE sign out

  任何时候你都可以点右上角你的用户名退出。说这个是不是有点多余, :)

项目
^^^^^^^^^^^^^^

.. figure:: img/xde-project-opened.png
  :width: 100%
  :alt: XDE project

  我们已经打开一个项目，左边是 React Packager, 你可以在 :ref:`Up and Running <up-and-running>` 和 :ref:`How Exponent Works <how-exponent-works>` 了解更多。右侧是设备 logs, 你可以在 :ref:`Viewing Logs <logging>` 了解更多。

发送链接
^^^^^^^^^

.. figure:: img/xde-send-link.png
  :width: 100%
  :alt: XDE send link

发送你的应用链接给任何有网络链接的人。你也可以在手机上获取这个链接如果手机不能连接或者访问你的电脑。

在设备上打开
^^^^^^^^^^^^^^^^^^^

.. figure:: img/xde-device.png
  :width: 100%
  :alt: XDE open on device

  点击 *device* 可以快速在你设备或者虚拟机上打开你的应用。了解更多: :ref:`Up and Running <up-and-running>`。

.. _xde-development-mode:
Development mode
^^^^^^^^^^^^^^^^

.. figure:: img/xde-development-mode.png
  :width: 100%
  :alt: XDE project development mode

通常来说你都想在 *development mode* 下开发。这样子会稍微慢点，因为它添加了一些运行时的检测来提示你可能存在的问题，但是它也提供了 live reloading, hot reloading, remote debugging 还有 element inspector。如果你想测试性能，你需要关闭 *development mode* 然后重新加载你的应用。

项目对话框 (项目打开的时候)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. figure:: img/xde-project-opened.png
  :width: 100%
  :alt: XDE project dialog in open project

  除了主页提供的那些选项，项目打开的时候我们提供更多的选项比如在 finder 里打开对应的 project 等。

Publish
^^^^^^^

.. figure:: img/xde-publish.png
  :width: 100%
  :alt: XDE publish

  当你点击了 *publish*, 你需要确认你想让公众看到你的app。点击 *yes* 会上传所有你的 Javascript 代码还有 assets 到我们的服务器。然后用户可以随时通过 ``exp.host/@your-username/your-app-slug`` 来访问你的 app。更多关于 *slug* 你可以看: :ref:`Configuration with exp.json <exp>`, 关于 *publish* 是怎么工作的，你可以看: :ref:`How Exponent Works <how-exponent-works>`。
