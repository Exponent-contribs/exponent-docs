.. _all-about-assets:

******
Assets
******

*Asset* 包含图片，字体，视频，音频还有应用里除了 Javascript 其他的东西。和 Web 上一样，assets 是按需在 HTTP 上拉取下来。而通常的手机应用，assets 是直接和程序打包在一起的。

Exponent 会区别对待两种 assets, 一种是开发阶段已经在本地你可以 *require* 的, 比如: ``<Image source={require('./assets/images/example.png')} />``, 另一种是Web加载的图片，比如: ``<Image source={{uri:
'http://yourwebsite.com/logo.png'} />``. 因为我们不托管 Web 加载的图片, 所以我们没法保证这些图片的可靠性。而且和本地图片相比，我们不能拿到图片的 metadata (width, height 之类的)。所以当你从 Web 加载图片的时候，必须声明它的 width 和 height, 否则的话默认是 0x0。最后，待会儿你会看到，这两种的缓存行为也不一样。

下面我们详细介绍下第一种 asset: 开发阶段你本地的 asset。至于 Web 加载的，我们假设你可以上传图片到可访问的网络。

Assets 不同阶段
"""""""""""""""""

开发
''''''''''''''

当你在本地开发项目的时候，本地磁盘负责提供 assets，而且 Javascript module 系统会集成它们。然后比如说我想添加一个图片，我可以 ``require``, 在 Javacript 里: ``require('./assets/images/example.png')``. 唯一的区别是我们需要指定它的后缀 -- 没有后缀的话，module 系统会认为它是一个 Javascript 文件。这句代码在编译时会变为一个包含 asset metadata 的对象，然后 ``Image`` 组件就可以用这个对象来拉取，显示: ``<Image source={require('./assets/images/example.png')} />``.

线上
'''''''''''''

每一次你发布 app 的时候，Exponent 会上传你的 assets 到 Amazon Cloudfront, 一个超级快的 CDN 。Exponent 用一个聪明的方法来保证你的发布速度: 如果你的 asset 和最近一次发布没有变化，就跳过它。你不需要做任何东西，Exponent 自动支持。

性能
"""""""""""

有些 assets 几乎是必备的，比如字体。在 Web 上字体加载问题甚至有几个首字母缩略词: FOUT, FOIT,
还有 FOFT, 分别代表 ``Flash of Unstyled Text``, ``Flash of Invisible Text`` 还有 ``Flash of Faux Text`` (`了解更多 <https://css-tricks.com/fout-foit-foft/>`_).

:ref:`@exponent/vector-icons <icons>` 提供icon fonts, icons第一次加载是 FOIT, 之后的加载字体都会自动缓存。用户对手机比 Web 有更高的要求，所以你可能需要更进一步，在初始加载的时候，预加载字体和重要的图片。

:ref:`了解更多关于预加载和缓存assets <preloading-and-caching-assets>`_.
