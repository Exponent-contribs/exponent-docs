.. _development-mode:

================
Development Mode
================

React Native 為開發階段提供了很多非常有用的工具，例如：Chrome 的 JavaScript 遠端除錯、live reload、host reloading，還有類似 Chrome 裡你常用的元素檢查工具inspector。當你的 app 執行時它也會做很多檢查，舉例來說，你是否用了已經廢棄的屬性、或是忘記傳遞必要的屬性給某個元件。

.. figure:: img/development-mode.png
  :width: 100%
  :alt: Screenshots of development mode in action

不過，**這些功能會帶來一些代價：在開發模式（development mode），你的 app 執行時會相對比較慢一點。**好消息是，你可以從 XDE 切換開發模式的開啟或關閉。當你切換模式時，記得要關閉再重啟你的 app，以讓這個切換生效。

**不管任何時候，當你想要測試 app 效能時，請記得關閉開發模式。**

在 XDE 中切換模式
""""""""""""""""""""""""""""""""

下面這張圖展示如何切換開發模式（development mode）。

.. image:: img/toggle-development-mode.png
  :width: 100%
