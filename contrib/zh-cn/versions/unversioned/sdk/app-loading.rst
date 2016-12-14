.. _app-loading:

**********
AppLoading
**********

一个 React 组件, 告诉 Exponent 如果这个是 app 要显示的第一个而且唯一的组件, 就一直停留在这个 app 加载中的画面。当这个组件移除，画面会消失，然后你的 app 就会显示出来。

这点对提升用户体验非常有用，因为在用户使用你的app之前，已经下载和缓存字体，logo, 图标，图片或者其他 assets。

Example
'''''''

.. code-block:: javascript

  import React from 'react';
  import {
    AppRegistry,
    Image,
    Text,
    View,
  } from 'react-native';
  import {
    Asset,
    Components,
  } from 'exponent';

  class App extends React.Component {
    state = {
      isReady: false,
    };

    componentWillMount() {
      this._cacheResourcesAsync();
    }

    render() {
      if (!this.state.isReady) {
        return <Components.AppLoading />;
      }

      return (
        <View>
          <Image source={require('./assets/images/exponent-icon.png')} />
          <Image source={require('./assets/images/slack-icon.png')} />
        </View>
      );
    }

    async _cacheResourcesAsync() {
      const images = [
        require('./assets/images/exponent-icon.png'),
        require('./assets/images/slack-icon.png'),
      ];

      for (let image of images) {
        await Asset.fromModule(image).downloadAsync();
      }

      this.setState({isReady: true});
    }
  }

  AppRegistry.registerComponent('main', () => App);
