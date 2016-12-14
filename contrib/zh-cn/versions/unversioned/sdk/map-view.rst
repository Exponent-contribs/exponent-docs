MapView
=======

一个 Map 组件，在 iOS 上用 Apple Maps, Android 上用 Google Maps。由 Airbnb 创建: `airbnb/react-native-maps <https://github.com/airbnb/react-native-maps>`_. 在 Exponent app 里用不需要任何设置，在 iOS standalone app 里用也不需要。下面有具体步骤详细说明怎么发布 Android standalone app。

.. image:: img/maps.png
  :width: 100%

.. code-block:: javascript

  import React from 'react';
  import { Components } from 'exponent';

  export default class HomeScreen extends React.Component {
    static route = {
      navigationBar: {
        visible: false,
      },
    }

    render() {
      return (
        <Components.MapView
          style={{flex: 1}}
          initialRegion={{
            latitude: 37.78825,
            longitude: -122.4324,
            latitudeDelta: 0.0922,
            longitudeDelta: 0.0421,
          }}
        />
      );
    }
  }

.. attribute:: Exponent.Components.MapView

   完整文档在: `airbnb/react-native-maps <https://github.com/airbnb/react-native-maps>`_.

发布 Android standalone app
""""""""""""""""""""""""""""""""""""""""

1. 创建你的 app, 注意你的 Android package name (比如: ca.brentvatne.growlerprowler)
2. 打开 https://console.developers.google.com/apis/credentials, 创建一个新的项目.
3. 创建成功后，在 project 里激活 Google Maps Android API。
4. 点击 Go to Credentials
5. 创建一个 key, 点击 Restrict Key
6. key restriction 选择 Android apps, 给 key 一个你想要的名字。
7. 点击 Add package name and fingerprint
8. 运行 ``keytool -list -printcert -jarfile growler.apk | grep SHA1 | awk '{ print $2 }'`` (替换 ``growler.apk`` 为第 1 步生成的apk).
9. 把第 8 步的输出填写到 "SHA-1 certificate fingerprint" 字段。
10. 添加 package name (比如: ca.brentvatne.growlerprowler) 到 Package name 字段. 点击 save.
11. 打开 ``exp.json``, 添加 api key 到 ``android.config.googleMaps.apiKey`` 字段. `See an example diff <https://github.com/brentvatne/growler-prowler/commit/3496e69b14adb21eb2025ef9e0719c2edbef2aa2>`_.
12. 和第 1 步一样重新 build 你的 standalone app。

发布 iOS standalone app
""""""""""""""""""""""""""""""""""""

不需要特殊的配置。
