*********
Debugging
*********

使用模擬器
=============================

**實體手機有不可取代的高效能以及使用體驗**，但是測試、除錯時，虛擬機可能會更方便。

iOS
^^^

Xcode 內建的 iOS 模擬器非常不錯，如果你還沒有安裝 Xcode 的話，可以透過`Mac App Store <https://itunes.apple.com/us/app/xcode/id497799835?mt=12>`_取得。

Android
^^^^^^^

我們建議使用 Genymotion 取代 Android SDK 提供的標準模擬器，它更快、更容易使用，而且功能也更完備。

`下載 Genymotion <https://www.genymotion.com/fun-zone/>`_ (free version)，然後按照ˋ `Genymotion installation guide <https://docs.genymotion.com/Content/01_Get_Started/Installation.htm>`_ 安裝。當你安裝完成之後就可以建立你的模擬器 --- 我們建議使用 Nexus 5，Android 版本則視你的需求決定。模擬器準備好之後請按 start 來啟動它。

Javascript 除錯
====================

你可以用 Chrome debugger tools 來為你的 Exponent app 除錯。相較在你的手機上執行 app 的 Javascript 程式碼，debugger tool 則是會把這些程式碼放在 Chrome 的一個 webworker 中執行。你可以設定中斷點、查看變數，執行程式碼...諸如此類，就像你平常替你的 web app 除錯一樣。

- 為了確保不受干擾的除錯體驗，首先把你的 XDE HOST 類型修改為 ``LAN`` 或是 ``localhost``。如果你在除錯時打開 ``Tunnel`` 的話，延遲可能會大到 app 難以使用。同時，確保有勾選 ``Development Mode``。

  .. image:: img/debugging-host.png
    :width: 100%

- 如果你使用 ``LAN``，記得確保你的設備與你的開發機處於同一個 WIFI 內。有些公用網路可能會有點問題或無法運作。至於設定成 ``localhost`` 選項時，iOS 必須使用模擬器才可以使用，而 Android 則必須使用 USB 連線。

- 打開你的 app，然後輕輕搖動你的設備（或是在 MAC 模擬器中按下 `Ctrl-Cmd-Z`）來打開除錯選單。點擊 ``Debug JS Remotely``，隨即會打開一個 連結到  ``http://localhost:19001/debugger-ui`` 的 Chrome 分頁。經由 Javascript console，你可以從這邊進行設定中斷點之類的操作。再次輕搖設備或是按下組合鍵，可以 關閉Chrome debugging。

- ``console.log`` 的程式碼行數顯示功能預設是無效的，你得進去 `Chrome Dev Tools settings` 中尋找 `Blackboxing` 分頁，然後勾選 `Blackbox content scripts`，最後選取 `Blackbox` 並且加入 pattern ``exponent/src/Logs.js``。


問題排除
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

當你在 XDE 打開專案然後按下 ``Open on Android`` 時，XDE 會自動告訴你的設備連結到 ``localhost:19000``、``19001`` 以跟你的開發機通訊 --- 只要你沒有拔除設備、或是模擬器沒有關閉。如果你使用 ``localhost`` 模式除錯，但卻運作不正常時，試試看關掉 app 並再次用 ``Open on Android`` 啟動它。或是直接手動來連結 app 與開發機，如果你已經安裝了 Android SDK，請輸入以下指令：
``adb reverse tcp:19000 tcp:19000`` - ``adb reverse tcp:19001 tcp:19001``

Source maps 还有 async 方法
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Source maps 和 async 方法並非 100% 可靠。世事總是不如人意，React Native 和 Chrome 的 source mapping 有時候還是會有些狀況，所以如果你想確保中斷點會在正確的地方發生作用，請直接在程式碼內呼叫 ``debugger``。

HTTP 除錯
==============

要除錯 app 的 HTTP 請求，你會需要一個代理工具。例如以下幾個：

- `Charles Proxy <https://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration/>`_ （＄50 美金，我們的偏愛工具）
- `mitmproxy <https://medium.com/@rotxed/how-to-debug-http-s-traffic-on-android-7fbe5d2a34#.hnhanhyoz>`_
- `Fiddler <http://www.telerik.com/fiddler>`_

在 Android 上，這個 `Proxy Settings <https://play.google.com/store/apps/details?id=com.lechucksoftware.proxy.proxysettings>`_
app 在 debug 和 non-debug 模式之間切換很有幫助。但可惜的是仍不支援 Android M。

React Native 開發團隊也有一些未來可以在 Chrome DevTools 上顯示網路請求的計畫：
`future work <https://github.com/facebook/react-native/issues/934>`_。

Hot Reloading 和 Live Reloading
================================
`Hot Module Reloading <http://facebook.github.io/react-native/blog/2016/03/24/introducing-hot-reloading.html>`_ 是一種快速重新載入程式碼變更而不會丟失你目前螢幕或是導航線狀態的方式。如果要啟動它，只需要輕搖你的設備（或是在 Mac 模擬器上按下 `Ctrl-Cmd-Z`），然後點擊 `Enable Hot Reloading`。相較之下，`Live Reload` 會重新載入整個 JS 環境，而 `Hot Module Reloading` 會讓你的除錯週期更短一些。

無論如何，你都要確保沒有同時打開兩者。
