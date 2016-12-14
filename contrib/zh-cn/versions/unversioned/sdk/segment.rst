Segment
========

提供 https://segment.com/ 应用分析. 在 Segment `iOS
<https://segment.com/docs/sources/mobile/ios/>`_ 和 `Android
<https://segment.com/docs/sources/mobile/android/>`_ SDKs 上的包装.

注意: 会话跟踪在 Exponent 应用上可能不能正常工作。Standalone(Store 下载) 应用的话没问题。

.. function:: Exponent.Segment.initializeIOS(writeKey)

   Segment 针对 iOS 和 Android 需要不同的 write keys。这个 write key 是你的 Segment iOS source 的 write key。

   :param string writeKey:
      iOS source 的 write key。

.. function:: Exponent.Segment.initializeAndroid(writeKey)
   Segment 针对 iOS 和 Android 需要不同的 write keys。这个 write key 是你的 Segment Android source 的 write key。

   :param string writeKey:
      Android source 的 write key。

.. function:: Exponent.Segment.identify(userId)

   给当前用户关联一个 user ID。 在 :func:`Exponent.Segment.initializeIOS` 和 :func:`Exponent.Segment.initializeAndroid` 之后其他方法之前调用这个方法。文档看: https://segment.com/docs/spec/identify/.

   :param string writeKey:
      当前用户的 user ID。

.. function:: Exponent.Segment.identifyWithTraits(userId, traits)

   给当前用户关联一个 user ID 和一些 metadata。 在 :func:`Exponent.Segment.initializeIOS` 和 :func:`Exponent.Segment.initializeAndroid` 之后其他方法之前调用这个方法。文档看: https://segment.com/docs/spec/identify/.

   :param string writeKey:
      当前用户的 user ID。

   :param object traits
      自定义属性 map。

.. function:: Exponent.Segment.track(event)

   在 Segment 记录一个事件，文档: https://segment.com/docs/spec/track/.

   :param string event:
      事件名称。

.. function:: Exponent.Segment.trackWithProperties(event, properties)

   在 Segment 上记录一个事件和自定义属性. 文档: https://segment.com/docs/spec/track/.

   :param string event:
      事件名称。

   :param object properties:
      自定义属性 map

.. function:: Exponent.Segment.flush()

   手动 flush 事件队列。通常情况下你不需要调用这个方法。
