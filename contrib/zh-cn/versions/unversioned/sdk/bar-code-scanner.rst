.. _bar-code-scanner

**************
BarCodeScanner
**************

用来渲染取景器(前置或者后置摄像头)的 React 组件, 可以在当前frame扫码。

需要 ``Permissions.CAMERA``.

支持的格式
"""""""""""""""""

+------------------------+------+----------+
| Bar code format        | iOS  | Android  |
+========================+======+==========+
| aztec                  | Yes  | Yes      |
+------------------------+------+----------+
| codabar                | No   | Yes      |
+------------------------+------+----------+
| code39                 | Yes  | Yes      |
+------------------------+------+----------+
| code93                 | No   | Yes      |
+------------------------+------+----------+
| code128                | Yes  | Yes      |
+------------------------+------+----------+
| code39mod43            | Yes  | No       |
+------------------------+------+----------+
| code93                 | Yes  | Yes      |
+------------------------+------+----------+
| datamatrix             | Yes  | Yes      |
+------------------------+------+----------+
| ean13                  | Yes  | Yes      |
+------------------------+------+----------+
| ean8                   | Yes  | Yes      |
+------------------------+------+----------+
| interleaved2of5        | Yes  | Yes      |
+------------------------+------+----------+
| itf14                  | Yes  | Yes      |
+------------------------+------+----------+
| pdf417                 | Yes  | Yes      |
+------------------------+------+----------+
| upc-a                  | Yes  | Yes      |
+------------------------+------+----------+
| upc-e                  | Yes  | Yes      |
+------------------------+------+----------+
| upc-ean                | No   | Yes      |
+------------------------+------+----------+
| qr                     | Yes  | Yes      |
+------------------------+------+----------+

Example
'''''''

.. code-block:: javascript

  import React from 'react';
  import { Text, View } from 'react-native';
  import Exponent, { Components, Permissions } from 'exponent';

  export default class BarcodeScannerExample extends React.Component {
    state = {
      hasCameraPermission: null,
    }

    async componentWillMount() {
      const { status } = await Permissions.askAsync(Permissions.CAMERA);
      this.setState({hasCameraPermission: status === 'granted'});
    }

    render() {
      const { hasCameraPermission } = this.state;
      if (typeof hasCameraPermission === 'null') {
        return <View />;
      } else if (hasCameraPermission === false) {
        return <Text>No access to camera</Text>;
      } else {
        return (
          <View style={{flex: 1}}>
            <Components.BarCodeScanner
              onBarCodeRead={this._handleBarCodeRead}
              style={StyleSheet.absoluteFill}
            />
          </View>
        );
      }
    }

    _handleBarCodeRead = (data) => {
      alert(JSON.stringify(data));
    }
  }

  Exponent.registerRootComponent(BarcodeScannerExample);

props
'''''

.. attribute:: type

   当 type 值为 ``'front'`` 的时候，用前置摄像头, ``'back'`` 的话，用后置摄像头。默认为 ``'back'``。

.. attribute:: torchMode

   当 touchMode 值为 ``'on'`` 的时候，闪光灯会打开, ``'off'`` 的话，会关掉。默认为 ``'off'``。

.. attribute:: barCodeTypes

   条码类型数组, ``BarCodeScanner.BarCodeType`` 是具体平台和设备支持的类型。默认为所有支持的条码类型。
