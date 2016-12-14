Video
=====


.. attribute:: Exponent.Components.Video

   在你的 app 里其他 React Native UI 组件里可以嵌入一个视频组件。
   在屏幕上显示的尺寸和位置都可以用普通的 React Native 样式来设置。

   支持下面这些属性:

   .. py:attribute:: source

      要显示的视频源。支持这些类型:

      - 一个网络 URL 指向网上的某个视频文件。
      - ``require('path/to/file')``, 源码包含的视频文件。

      `iOS developer documentation
      <https://developer.apple.com/library/ios/documentation/Miscellaneous/Conceptual/iPhoneOSTechOverview/MediaLayer/MediaLayer.html>`_
      列出 iOS 里支持的视频格式。

      `Android developer documentation
      <https://developer.android.com/guide/appendix/media-formats.html#formats-table>`_
      列出 Android 里支持的视频格式。

   .. py:attribute:: fullscreen

      是否全屏。

   .. py:attribute:: resizeMode

      在组件范围区间里怎么缩放, 必须是下面其中一种:

      - ``Exponent.Components.Video.RESIZE_MODE_STRETCH`` -- 拉伸图片且不维持宽高比，直到宽高都刚好填满容器。
      - ``Exponent.Components.Video.RESIZE_MODE_CONTAIN`` -- 在保持图片宽高比的前提下缩放图片，直到宽度和高度都小于等于容器视图的尺寸（如果容器有padding内衬的话，则相应减去）。译注：这样图片完全被包裹在容器中，容器中可能留有空白
      - ``Exponent.Components.Video.RESIZE_MODE_COVER`` -- 在保持图片宽高比的前提下缩放图片，直到宽度和高度都大于等于容器视图的尺寸（如果容器有padding内衬的话，则相应减去）。译注：这样图片完全覆盖甚至超出容器，容器中不留任何空白。

   .. py:attribute:: repeat

      如果为 true, 视频会循环播放。不然的话，只播放一次。

   .. py:attribute:: paused

      如果为 true 的话，视频会停止在上次这个属性从 false 到 true的那一点，不然的话继续播放。

   .. py:attribute:: volume

      音量大小，从 0 到 1。

   .. py:attribute:: mute

      是否静音. ``volume`` 值在切换静音时保持不变。

   .. py:attribute:: rate

      播放速度。0 停止播放，1 正常速度。其他速度可以用来放慢，加快或者往后播放如果
      :ref:`onLoad <video-on-load>` 的参数支持这些方式。

   .. py:attribute:: onLoadStart

      当视频数据从网络上开始加载时候，这个方法会调用。需要传入参数 ``{ uri }``, ``uri``
      是视频的URI。

   .. _video-on-load:
   .. py:attribute:: onLoad

      当视频数据已经被加载，这个方法会调用。因为数据是流式的，可能还没完全加载，只是满足
      第一帧渲染。这个方法需要传入一个对象，包含下面的属性:

      - ``duration`` -- 视频长度 (秒为单位)。
      - ``currentTime`` -- 当前播放位置 (秒为单位)。
      - ``canPlayReverse`` -- 是否支持正常速度向后播放。
      - ``canPlayFastForward`` -- 是否支持快速向前播放。
      - ``canPlaySlowForward`` -- 是否支持慢速向前播放。
      - ``canPlaySlowReverse`` -- 是否支持慢速向后播放。
      - ``canStepBackward`` -- 是否支持向后拖。
      - ``canStepForward`` -- 是否支持向前拖。
      - ``naturalSize`` -- 一个有这些属性的对象: ``{ width, height, orientation
        }``, ``orientation`` 不是 ``'landscape'`` 就是 ``'portrait'``

   .. py:attribute:: onError

      如果加载或者播放失败，这个方法会调用。iOS 上需要参数类型为: ``{ error: { code, domain }``,
      Android 上是 ``{ error: {what, extra }``.

   .. py:attribute:: onProgress

      每次播放进行当中，会调用这个方法。最多每隔 250 ms 调用一次。接受参数类型为:
      ``{ currentTime, playableDuration }``, ``currentTime`` 是当前播放的时间 (秒为单位)，
      ``playableDuration`` 是已缓冲的视频长度 (秒为单位)。

   .. py:attribute:: onSeek

      当你每次拖视频 (比如说调用这个组件 ref 的 ``.seek()`` 方法) 的时候这个方法会调用。
      接受参数类型为: ``{ currentTime, seekTime }``, ``currentTime`` 是当前播放的时间 (秒为单位)，
      ``seekTime`` 是要拖到的位置。

   .. py:attribute:: onEnd

      当播放结束的时候，会调用这个方法。不需要传参。

   组件的 ref 包含下面这些方法:

   .. py:method:: seek(time)

      播放移动到传入的时间(秒为单位)。:ref:`onLoad <video-on-load>` 的参数说明是否支持
      往前或者往后拖。

   .. py:method:: presentFullscreenPlayer()

      切换到全屏。

   .. py:method:: dismissFullscreenPlayer()

      退出全屏。
