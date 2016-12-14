.. _permissions:

***********
Permissions
***********

当你需要添加一些可能需要用户设备敏感信息的功能的时候，比如地理位置，或者发送一些用户可能
不想要的推送通知，你需要首先征求用户的权限。如果你已经征求过了，那就没需要了。

.. function:: Exponent.Permissions.getAsync(type)

   判断你的 app 是否给予这种权限。

   :param string type:
      权限类型。

   :returns:
      返回一个 ``Promise``, 包含权限的一些信息，比如状态，过期时间，还有 scope (是否应用这种权限类型) 。

   :example:
      .. code-block:: javascript

        async function alertIfRemoteNotificationsDisabledAsync() {
          const { Permissions } = Exponent;
          const { status } = await Permissions.getAsync(Permissions.REMOTE_NOTIFICATIONS);

          if (status !== 'granted') {
            alert('Hey! You might want to enable notifications for my app, they are good.');
          }
        }

.. function:: Exponent.Permissions.askAsync(type)

   征求用户权限。如果用户已经授予权限，返回成功。

   :param string type:
      权限名称。

   :returns:
      返回一个 ``Promise``, 包含权限的一些信息，比如状态，过期时间，还有 scope (是否应用这种权限类型) 。

   :example:
      .. code-block:: javascript

        async function getLocationAsync() {
          const { Location, Permissions } = Exponent;
          const { status } = await Permissions.askAsync(Permissions.LOCATION);

          if (status === 'granted') {
            return Location.getCurrentPositionAsync({enableHighAccuracy: true});
          } else {
            throw new Error('Location permission not granted');
          }
        }

.. attribute:: Exponent.Permissions.REMOTE_NOTIFICATIONS

   消息推送权限类型。

   .. epigraph::
      **注意:** 在 iOS 上, ``undetermined`` 和 ``denied`` 不能区别开来，所以只会返回 ``granted`` 或者 ``undetermined``. 这是底层 native API实现方式导致的。

.. attribute:: Exponent.Permissions.LOCATION

   地理位置权限类型。
