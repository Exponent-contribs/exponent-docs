*****
Asset
*****

这个模块提供 Exponent asset 系统的接口。一个asset是应用运行需要的除了源代码之外的东西，比如图片，文字，音频等。Exponent 的 asset 系统和 React Native 集成，所以你可以 ``require('path/to/file')`` 来引用文件。比如你可以在React Native  ``Image`` 组件里中用这种方式来引用静态图片。更多信息可以查看 React Native 的静态图片资源文档: <https://facebook.github.io/react-native/docs/images.html#static-image-resources>`_ , Exponent 直接支持这种方式。

.. py:class:: Exponent.Asset

   这个类代表你应用里的 asset。它提供一些 metadata, 比如 asset 的名称，类型，还有加载 asset 据的方法。

   .. py:attribute:: name

      Asset 文件的名称，不包含后缀。也不包含文件名里 ``@`` 到后缀之间的字符 (用来声明图片的比例尺寸)。

   .. py:attribute:: type

      Asset 文件的类型。

   .. py:attribute:: hash

      Asset 数据的 MD5 hash。

   .. py:attribute:: uri

      一个指向远端服务器上 asset 数据的 URI。在线上运行的时候，它代表 Exponent asset 服务器上的位置，
      而在开发环境的时候，它指向 在你电脑上运行的 XDE 服务, 本地磁盘负责提供 asset。

   .. py:attribute:: localUri

      如果 asset 已经下载好 (调用 :any:`downloadAsync() <Exponent.Asset.downloadAsync>`), ``file://`` URI
      指向设备上包含 asset 数据的本地文件。

   .. py:attribute:: width

      如果 asset 是一张图片的话，代表这张图片的 width 除以 比例系数(scale factor)。比例系数是文件名里紧跟 ``@`` 后面的数字，
      没有的话默认为 ``1``。

   .. py:attribute:: height

      如果 asset 是一张图片的话，代表这张图片的 height 除以 比例系数(scale factor)。比例系数是文件名里紧跟 ``@`` 后面的数字，
      没有的话默认为 ``1``。

   .. py:method:: downloadAsync

      下载 asset 数据到设备缓存目录里的一个本地文件。只要返回的 promise 没有错误，这个 asset 的 :any:`localUri
      <Exponent.Asset.localUri>` 属性会指向一个本地文件。只有在这个asset有更新时才会重新下载。

.. function:: Exponent.Asset.fromModule(module)

   根据参数 module 返回一个 :py:class:`Exponent.Asset` 实例，代表一个 asset。
   module

   :param number module:
      ``require('path/to/file')`` 返回的 asset 数值。

   :returns:
      :py:class:`Exponent.Asset` 实例，代表这个 asset。

   :example:
      .. code-block:: javascript

        const imageURI = Exponent.Asset.fromModule(require('./images/hello.jpg')).uri;

      在运行这段代码的时候, ``imageURI`` 是 ``images/hello.jpg`` 的远端 URI。这个路径是这段代码的文件所在位置的相对路径。
