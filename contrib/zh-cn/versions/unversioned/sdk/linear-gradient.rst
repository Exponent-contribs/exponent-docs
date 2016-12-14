.. _linear-gradient:

**************
LinearGradient
**************

一个线性渐变(颜色)的 native view 组件。
A React component that renders a native gradient view.

Example button
''''''''''''''

.. image:: img/gradient-button-example.png
  :width: 400

.. code-block:: javascript

  import React from 'react';
  import {
    Text,
    StyleSheet,
  } from 'react-native';
  import {
    Components
  } from 'exponent';

  export default class FacebookButton extends React.Component {
    render() {
      return (
        <Components.LinearGradient
          colors={['#4c669f', '#3b5998', '#192f6a']}
          style={{padding: 15, alignItems: 'center', borderRadius: 5}}>
          <Text style={{backgroundColor: 'transparent', fontSize: 15, color: '#fff'}}>
            Sign in with Facebook
          </Text>
        </Components.LinearGradient>
      );
    }
  }

Example with transparency
'''''''''''''''''''''''''

.. image:: img/gradient-transparency-example.png
  :width: 400

.. code-block:: javascript

  import React from 'react';
  import { Components } from 'exponent';

  export default class BlackFade extends React.Component {
    render() {
      return (
        <Components.LinearGradient
          colors={['rgba(0,0,0,0.4)', 'transparent']}
          style={{position: 'absolute', left: 0, right: 0, top: 0, height: 20}} />
      );
    }
  }



props
'''''

.. attribute:: colors

   一个颜色数组代表渐变里的不同变化。最少需要两种颜色 (不然的话就不是渐变而是充满了)。

.. attribute:: start

   数组 ``[x, y]``, x 和 y 都是 floats. 他们代表 gradient 的起始位置, 作为 gradient 范围整体的比例。比如 ``[0.1, 0.1]`` 就代表 gradient 从 10% 高 和 10% 左 开始。

.. attribute:: end

   和 start 一样，但是定义 gradient 的 end.

.. attribute:: locations

   一个和 ``colors`` 长度一样的数组, 每一个元素是一个和 ``start``, ``end`` 含义一样的 float, 不同的是它们定义的是对应 index 的 color 的位置。
