.. _all-about-assets:

******
Assets
******

一個 *Asset* 泛指某個圖片、字體、影片、音效，以及你的應用程式中除了 Javascript 之外的其他東西。就像是在網頁上一樣，assets 是按照需求從 HTTP 上抓取或串留下來的。這是個與典型 app 的不同點，一般 assets 是跟程式本身打包在一起的。

Exponent 會區別對待兩種 assets，一種是在開發階段時就已經儲存在本地端，可以使用 *require* 匯入的。例如: 

``<Image source={require('./assets/images/example.png')} />``

另外一種則是透過 Web 載入的圖片，例如:

``<Image source={{uri:
'http://yourwebsite.com/logo.png'} />``

OK，由於我們並不管理這些經由 Web 載入的圖片，所以我們不能保證這些圖片來源可靠。而且與本地檔案系統中的圖片相比，我們沒辦法經由 packager 自動取得圖片的 metadata（width, height 之類）。所以，當你從 Web 載入圖片時，你需要指定每一張圖片的 width 和 height 屬性，否則預設會被設置為 0x0。最後，這兩種 assets 的快取行為也會不一樣。


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
