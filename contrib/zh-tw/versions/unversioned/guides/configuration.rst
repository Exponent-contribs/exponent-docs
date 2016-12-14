.. _exp:

exp.json 設定值
========

.. 此檔案為自動產生，千萬不要手動修改它！ --
詳細請見 scripts/generate-exp-docs.js

``exp.json`` 包含了你 app 中除了程式碼之外的所有設定值。下面列出所有支援的屬性：

.. attribute:: name

 **Required**. Exponent 使用者端中 app 的名字，以及 standalone app 主畫面中的名字。

.. attribute:: description

 一段簡短描述，告訴大家為什麼你的 app 超棒！

.. attribute:: slug

 **Required**. 發布後易讀的 url name, 比如: ``exp.host/@your-username/slug``.

.. attribute:: sdkVersion

 **Required**. 本 App 用到的 Exponent sdkVersion。應該與 package.json 中定義的版本一致。

.. attribute:: version

 你的 app 版本，你可以使用任何你喜歡的規範。

.. attribute:: orientation
 鎖定 app 方向：``portrait`` 或者 ``landscape``。預設值為不鎖定，可以使用 ``default``、``portrait``、 ``landscape。``

.. attribute:: primaryColor
 此為在 Android 上，顯示多工選單時你的 app 標題欄的顏色。目前不支援 iOS，但以後可能會用到。使用 6 位數的 16 進制色碼，例如： ``'#000000'``

.. attribute:: iconUrl

 你的 App 圖示連結。我們建議你使用一張含有透明圖層的 512x512 PNG 格式圖片。它會顯示在手機的主畫面上，以及会 Exponent app 裡。

.. attribute:: notification

 推播通知設定值。

   .. attribute:: iconUrl

    顯示在推播通知上的圖示連結。格式為灰階透明、尺寸為 48x48 的 PNG 格式圖片。

   .. attribute:: color

    推播通知圖示的著色背景色。使用 6 位數的 16 進制色碼，例如： ``'#000000'``

   .. attribute:: androidMode

    Android 的推播通知顯示設定。獨立顯示每次的推播通知 (``default``)，或是整合收摺成同一個通知 (``collapse``).
    預設為 ``collapse``。

   .. attribute:: androidCollapsedTitle

    當 ``androidMode`` 設定為 ``collapse`` 時，需要設定此欄位來顯示折疊通知的標題。 例如：``'#{unread_notifications} new interactions'``。

.. attribute:: loading

 當使用者打開 app 時的載入（拉取、快取 Javascript bundle 还有 assets) 畫面設定值。

   .. attribute:: iconUrl

    每次啟動 app 時顯示的圖示。不限制大小以及比例，但是必須是 .png。

   .. attribute:: exponentIconColor

    如果沒有提供啟動 app 時顯示的圖示，預設會顯示 Exponent logo。你可以選擇替該 logo 上色， ``white`` 或者 ``blue``.

   .. attribute:: exponentIconGrayscale

    用途類似 ``exponentIconColor``，但用於指明單色 (``1``) 或者不是單色 (``0``)。

   .. attribute:: backgroundImageUrl

    App 載入畫面的背景圖片。不限制大小以及比例，但是必須是 .png。

   .. attribute:: backgroundColor

    App 載入畫面的背景色彩。使用 6 位數的 16 進制色碼，例如： ``'#000000'``

   .. attribute:: hideExponentText

    Exponent 預設會在載入頁面底部顯示一些文字，設定為 ``true`` 可以不顯示。

.. attribute:: appKey
 Exponent 會尋找你在 AppRegistry 處註冊 application，預設應該是 ``main`` 。如果需要的話，你可以在這個屬性變更成特定的名稱。

.. attribute:: androidStatusBarColor

  Android 的標題欄色彩。使用 6 位數的 16 進制色碼，例如： ``'#000000'``

.. attribute:: androidHideExponentNotificationInShellApp

 Exponent 預設會替你的 app 建立一個狀態欄通知，含有更新（refresh）按鈕、除錯資訊等。設定成 ``true`` 可以關閉該通知。

.. attribute:: scheme

 **Standalone Apps Only**. 跟系統註冊關聯到你的 app 的 url scheme。例如，如果設定成 ``'rnplay'`` 的話，使用者點擊任何 ``rnplay://`` 開頭的連結就會啟動你的 app。

.. attribute:: entryPoint

 你的 main Javascript bundle 檔案的相對路徑。

.. attribute:: extra

 你想要傳給 experience 的額外參數。

.. attribute:: rnCliPath


.. attribute:: packagerOpts


.. attribute:: ignoreNodeModulesValidation


.. attribute:: nodeModulesPath


.. attribute:: ios

 **Standalone Apps Only**. iOS standalone app 的相關設定。

   .. attribute:: bundleIdentifier

    你的 iOS standalone app 的 bundle identifier，它必須是 App Store 中唯一存在的。可以看 `這個 StackOverflow question <http://stackoverflow.com/questions/11347470/what-does-bundle-identifier-mean-in-the-ios-project>`_來瞭解更多。

    iOS bundle identifier 的命名，例如：

      ``host.exp.exponent``

    `exp.host` 是你的網域名稱，而 Exponent 則是 app 名稱。

   .. attribute:: buildNumber

    你的 iOS standalone app 的 Build number。

   .. attribute:: config

       .. attribute:: fabric

        `Twitter Fabric <https://get.fabric.io/>`_ keys，用來支援 Crashlytics 與其他服務。

           .. attribute:: apiKey

            你的 Fabric API key。

           .. attribute:: buildSecret

            你的 Fabric build secret。

       .. attribute:: googleSignIn

        你的 `Google Sign-In iOS SDK <https://developers.google.com/identity/sign-in/ios/start-integrating>`_ keys，這是 standalone app 需要的。

           .. attribute:: reservedClientId

            保留的 client id url scheme。 可以在 `GoogeService-Info.plist` 裡找到。

.. attribute:: android

 **Standalone Apps Only**. Android standalone app 的相關設定。

   .. attribute:: package

    你的 Android standalone app 的 package name。必須確保是 Play Store 上唯一存在的名稱。可以看：`這個  StackOverflow question <http://stackoverflow.com/questions/6273892/android-package-name-convention>`_.

    反向的你的 DNS 作為 package name。例如：

      ``host.exp.exponent``

    `exp.host` 是你的網域名稱，而 Exponent 則是 app 名稱。

   .. attribute:: versionCode

    Google Play 所需要的版本號，每次發佈都要遞增 1。詳細請看： https://developer.android.com/studio/publish/versioning.html.

   .. attribute:: config

       .. attribute:: fabric

        `Twitter Fabric <https://get.fabric.io/>`_ keys，用來支援 Crashlytics 與其他服務。

           .. attribute:: apiKey

            你的 Fabric API key。

           .. attribute:: buildSecret

            你的 Fabric build secret。

       .. attribute:: googleMaps

        你的 `Google Maps Android SDK <https://developers.google.com/maps/documentation/android-api/signup>`_ key， standalone app 需要。

           .. attribute:: apiKey

            你的 Google Maps Android SDK API key。

       .. attribute:: googleSignIn

        你的 `Google Sign-In Android SDK <https://developers.google.com/identity/sign-in/android/start-integrating>`_ keys， standalone app 需要。

           .. attribute:: apiKey

            Android API key。你可以在 developer console 的 credentials 裡或是目錄下 `google-services.json` 檔案中找到。

           .. attribute:: certificateHash

            用來建置 apk 簽章的 SHA-1 雜湊碼，不能包含 `:`符號。
            你可以在 `google-services.json` 中找到。詳細請看： https://developers.google.com/android/guides/client-auth
