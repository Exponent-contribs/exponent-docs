.. _logging:

************
閱讀日誌（Logs）
************

在 Exponent app 裡寫 Log 跟 Web 開發是一樣的：使用 ``console.log``、``console.warn``，以及 ``console.error``。

注意：除了遠端除錯模式（remote debugging mode）之外，我們暫時不支援``console.table``。

建議：用 Exponent 工具查看日誌
==========================================

當你打開一個由 XDE 或者 exp 提供執行環境的 app，它會傳送 logs 到 server 以方便讓你查看。這表示就算你的設備沒跟電腦連線，你也能看到這些 logs --- 實際上，如果有地球上另一端的某個人打開該 app，你也能從他們的設備中看到 log。

.. epigraph::
  **Note:** 沒看到 log？ 確保你至少是使用 Exponent sdkVersion 7.0.0、有安装 ``exponent`` 的 npm 套件，而且已經正確引入 (例如：在你的 main JS
  開頭處有 ``import * as Exponent from 'exponent'``)。

XDE 日誌視窗
^^^^^^^^^^^^^^^^

.. figure:: img/xde-logs.png
  :width: 100%
  :alt: XDE window with logs

  在 XDE 裡你會注意到，當你打開一個 sdkVersion >= 7.0.0 的 Exponent app 時，日誌視窗會一分為二。你的 app 日誌會在右邊，而 packager 日誌則是在左邊。

.. figure:: img/xde-logs-device-picker.png
  :width: 100%
  :alt: XDE window with device picker selected

  XDE 也可以讓你在所有開啟你的 app 的不同設備所傳回的日誌間切換。

exp logs
^^^^^^^^

如果你是使用我們的 CLI 指令 ``exp`` 的話，你也可以用 ``exp logs`` 來查看日誌（請確保已經啟動你的 server，在專案目錄輸入：``exp start``）。

.. figure:: img/exp-logs.png
  :width: 100%
  :alt: Terminal output from running xde logs

  除非你按了 ``CTRL+C`` 退出，否則 Packager 日誌以及 app 日誌都會顯示在這個視窗。

選用：手動存取裝置日誌
=====================================

通常這不是必須的，如果你想要了解所有發生在該設備上的事，包含其他 app 甚至是系統本身的日誌，你可以使用接下來要介紹的方法。

查看 iOS 模擬器日誌
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

選項 1 ：使用 GUI log
""""""""""""""""""""""

* 在模擬器中，按下 ``⌘ + /``，*或者*，點選 ``Debug -> Open System Log`` -- 這兩種方式都會打開一個日誌視窗，顯示你設備中發生的所有日誌，包含你的 Exponent app 的日誌。

選項 2：從終端機中打開
""""""""""""""""""""""""""""""

* 輸入 ``instruments -s devices``
* 找到你正在使用的設備 / OS 版本，例如：``iPhone 6s (9.2) [5083E2F9-29B4-421C-BDB5-893952F2B780]``
* 後面``[..]``中的是設備號碼，接下来你需要做的是: ``tail -f ~/Library/Logs/CoreSimulator/[設備號碼]/system.log``，例如：``tail -f ~/Library/Logs/CoreSimulator/5083E2F9-29B4-421C-BDB5-893952F2B780/system.log``

查看你的 iPhone 日誌
^^^^^^^^^^^^^^^^^^^^^^^^^^^

* 輸入``brew install libimobiledevice``
* 插上你的手機
* ``idevicepair pair``
* 點選 ``accept`` 接受
* 執行 ``idevicesyslog``

查看 Android 設備或模擬器的日誌
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* 確定已經安裝 Android SDK
* 確定 `設備上的 USB 除錯開關已經打開 <https://developer.android.com/studio/run/device.html#device-developer-options>`_ (模擬器應不必要).
* 輸入``adb logcat``
