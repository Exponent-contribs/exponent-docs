ImagePicker
===========

提供从库里选择图片，或者拍照的系统 UI。

.. function:: Exponent.ImagePicker.launchImageLibraryAsync(options)

   显示从库里选择图片的系统 UI。

   :param object options:
      参数 map:

      * **allowsEditing** (*boolean*) -- 在选中后是否显示编辑图片的 UI。在 Android
        上用户可以裁切或者翻转图片, 在 iOS 上只支持裁切。默认为 ``false``.

      * **aspect** (*array*) -- 如果用户可以编辑图片的话 (传参: ``allowsEditing: true``),
        一个 ``[x, y]`` 数组, 定义需要维持的宽高比。只有 Android 支持，因为 iOS 上裁切永远是一个正方形。

   :returns:
      如果用户取消选择图片的话，返回 ``{ cancelled: true }``.

      否则的话, 返回 ``{ cancelled: false, uri, width, height }``,  ``uri`` 代表本地图片的
      URI (可以在 React Native ``Image`` 组件里使用), ``width, height`` 是图片的纬度。

.. function:: Exponent.ImagePicker.launchCameraAsync(options)

   Display the system UI for taking a photo with the camera.

   :param object options:
      参数map:

      * **allowsEditing** (*boolean*) -- 在选中后是否显示编辑图片的 UI。在 Android
        上用户可以裁切或者翻转图片, 在 iOS 上只支持裁切。默认为 ``false``.

      * **aspect** (*array*) -- 如果用户可以编辑图片的话 (传参: ``allowsEditing: true``),
        一个 ``[x, y]`` 数组, 定义需要维持的宽高比。只有 Android 支持，因为 iOS 上裁切永远是一个正方形。

   :returns:
      如果用户取消拍照的话，返回 ``{ cancelled: true }``.

      否则的话, 返回 ``{ cancelled: false, uri, width, height }``,  ``uri`` 代表本地图片的
      URI (可以在 React Native ``Image`` 组件里使用), ``width, height`` 是图片的纬度。
