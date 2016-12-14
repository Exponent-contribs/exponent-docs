.. _blur-view

********
BlurView
********

一个 React 组件，在 iOS 上渲染一个 native blur view(毛玻璃效果), 在 Android 生成类似半透明的 view。通常用在导航栏或者 tab bars，比如:

.. image:: img/nav-bar-blur.png
  :width: 400

Example: tintEffect
'''''''''''''''''''

.. image:: img/tint-effect-example.png
  :width: 100%

.. code-block:: javascript

  import React from 'react';
  import {
    AppRegistry,
    Image,
    StyleSheet,
    View,
  } from 'react-native';
  import Exponent, {
    Components
  } from 'exponent';

  class BlurViewExample extends React.Component {
    render() {
      const uri = 'https://s3.amazonaws.com/exp-brand-assets/ExponentEmptyManifest_192.png';

      return (
        <View>
          <Image style={{width: 192, height: 192}} source={{uri}} />

          { /* Change tintEffect here to reproduce the above image */ }
          <Components.BlurView tintEffect="light" style={StyleSheet.absoluteFill}>
            <Image style={{width: 96, height: 96}} source={{uri}} />
          </Components.BlurView>
        </View>
      );
    }
  }

  Exponent.registerRootComponent(BlurViewExample);

Example: blur strength with opacity
"""""""""""""""""""""""""""""""""""

You can change ``opacity`` style on the component to change the strength of the
blur.

.. image:: img/blur-opacity-example.gif
   :width: 400

.. code-block:: javascript

  import React from 'react';
  import {
    Animated,
    AppRegistry,
    Image,
    StyleSheet,
    View,
  } from 'react-native';
  import {
    Components,
  } from 'exponent';

  const AnimatedBlurView = Animated.createAnimatedComponent(Components.BlurView);

  class BlurViewExample extends React.Component {
    state = {
      opacity: new Animated.Value(0),
    }

    componentDidMount() {
      this._animate();
    }

    _animate = () => {
      let { opacity } = this.state;
      Animated.timing(opacity, {duration: 2500, toValue: 1}).start((value) => {
        Animated.timing(opacity, {duration: 2500, toValue: 0}).start(this._animate);
      });
    }

    render() {
      const uri = 'https://s3.amazonaws.com/exp-brand-assets/ExponentEmptyManifest_192.png';

      return (
        <View style={{flex: 1, margin: 30}}>
          <View style={{flex: 1, padding: 55, paddingTop: 60}}>
            <Image style={{width: 180, height: 180}} source={{uri}} />

            <AnimatedBlurView
              tintEffect="default"
              style={[StyleSheet.absoluteFill, {opacity: this.state.opacity}]} />
          </View>
        </View>
      );
    }
  }

  AppRegistry.registerComponent('main', () => BlurViewExample);

props
'''''

.. attribute:: tintEffect

   一个字符串: ``light``, ``default``, 或者 ``dark``.
