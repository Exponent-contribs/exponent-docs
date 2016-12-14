Amplitude
==========

提供 https://amplitude.com/ 应用分析。在 Amplitude `iOS
<https://github.com/amplitude/Amplitude-iOS>`_ 和 `Android
<https://github.com/amplitude/Amplitude-Android>`_ SDKs 上的包装.

注意: 会话跟踪在 Exponent 应用上可能不能正常工作。Standalone(Store 下载) 应用的话没问题。

.. function:: Exponent.Amplitude.initialize(apiKey)

   用你的 Amplitude API key 初始化 Amplitude。找到你的 API key: `see
   <https://amplitude.zendesk.com/hc/en-us/articles/206728448-Where-can-I-find-my-app-s-API-Key-or-Secret-Key->`_.

   :param string apiKey:
      你的 Amplitude 应用 API key.

.. function:: Exponent.Amplitude.setUserId(userId)

   给当前用户设置一个 user ID。如果你的系统里没有 user IDs 你不需要调用这个方法。See https://amplitude.zendesk.com/hc/en-us/articles/206404628-Step-2-Assign-User-IDs-and-Identify-your-Users。

   :param string userId:
      当前用户的 user ID。

.. function:: Exponent.Amplitude.setUserProperties(userProperties)

   给当前用户设置自定义属性。 See https://amplitude.zendesk.com/hc/en-us/articles/207108327-Step-4-Set-User-Properties-and-Event-Properties.

   :param object userProperties:
      一个自定义属性 map.

.. function:: Exponent.Amplitude.clearUserProperties()

   清空调用 :func:`Exponent.Amplitude.setUserProperties` 生成的属性.

.. function:: Exponent.Amplitude.logEvent(eventName)

   在 Amplitude 上记录一个事件. https://amplitude.zendesk.com/hc/en-us/articles/206404698-Step-3-Track-Events-and-Understand-the-Actions-Users-Take 有介绍具体需要跟踪哪些事件。

   :param string eventName:
      事件名称。

.. function:: Exponent.Amplitude.logEventWithProperties(eventName, properties)

   在 Amplitude 上记录一个事件和自定义属性. https://amplitude.zendesk.com/hc/en-us/articles/206404698-Step-3-Track-Events-and-Understand-the-Actions-Users-Take 有介绍具体需要跟踪哪些事件。

   :param string eventName:
      时间名称.

   :param object properties:
      一个自定义属性 map.

.. function:: Exponent.Amplitude.setGroup(groupType, groupNames)

   添加当前用户到一个 group。 See https://github.com/amplitude/Amplitude-iOS#setting-groups 还有 https://github.com/amplitude/Amplitude-Android#setting-groups.

   :param string groupType:
      group 名称, 比如: "sports"。

   :param object groupNames:
      一个 group 名称的数组, 比如 ["tennis", "soccer"]。注意: iOS 和 Android Amplitude SDKs 允许同时用字符串或者字符串数组。我们只支持字符串数组。如果你只需要一个group name, 你可以创建只包含一个 group name 的数组
