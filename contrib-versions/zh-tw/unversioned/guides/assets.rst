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

接下來我們會詳細介紹兩種 asset：
- 在開發階段存在你本地端的 asset。
- 從 Web 載入的 asset，我們假設你把上傳到可以任意存取的網路。

Assets 的不同階段
"""""""""""""""""

開發階段
''''''''''''''

當你在本地端開發專案時，你的檔案系統會負責提供這些 assets 讓 Javascript 的 module 系統來整合。

例如，假設我想加入一張圖片，我可以使用 ``require``，在 Javacript 中，程式碼大概是長這個樣子：
``require('./assets/images/example.png')``
唯一的區別是我們需要指定它的副檔名 --- 沒有副檔名的話，module 系統會認為它是一個 Javascript 程式碼檔案。這段程式碼會在編譯時轉變為一個包含該 asset metadata 的物件，然後 ``Image`` 組件就可以使用這個物件來拉取，然後渲染它。例如以下程式碼：
``<Image source={require('./assets/images/example.png')} />``

上線
'''''''''''''

每次你發布你的 app 時，Exponent 都會把你的 assets 上傳到 Amazon Cloudfront，一個超級快的 CDN。Exponent 用了一個聰明的方法來保證你的發布速度：如果你這次的 assets 和上一次的發佈沒有變化，那就跳過上傳的這個動作。

這些過程你不需要做任何處理，Exponent 都幫你搞定了。

效能
"""""""""""

有些 assets 你的 app 沒有是不行的，例如：字體。在 Web 上的字體載入問題甚至知名到有幾個字首縮寫：FOUT、FOIT，以及 FOFT；分別代表的是：
- ``Flash of Unstyled Text``
- ``Flash of Invisible Text``
- ``Flash of Faux Text``
（`了解更多 <https://css-tricks.com/fout-foit-foft/>`_）

Exponent 整合的 :ref:`@exponent/vector-icons <icons>` 組件提供 icon fonts；這些 icons 第一次載入時是是 FOIT，後續需要載入字體時都會自動快取。使用者對手機 app 會比對 Web 有更高的要求，所以你可能得更進一步的搶在 app 剛啟動的初始化畫面時預先載入字體以及後續會用到的重要圖片。

:ref:`瞭解更多關於預先載入以及快取 assets 的相關資訊 <preloading-and-caching-assets>`_.
