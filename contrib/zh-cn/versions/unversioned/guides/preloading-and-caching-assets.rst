.. _all-about-assets:

***************************
预加载 & 缓存 Assets 
***************************

在缓存 assets 的时候，我们需要一直显示加载页面，我们会只渲染
:ref:`Exponent.Components.AppLoading <app-loading>`, 直到所有东西都加载完毕。

对于需要保存到本地文件系统的图片，我们可以用 ``Exponent.Asset.fromModule(image).downloadAsync()``
来下载，缓存图片。而网络图片的话，我们可以用 ``Image.prefetch(image)``.

字体通过 ``Exponent.Font.loadAsync(font)`` 预加载。``font`` 参数在这里是一个对象，类似:
``{OpenSans: require('./assets/fonts/OpenSans.ttf}``. ``@exponent/vector-icons``
对这种对象提供一个方便的方法, 你可以在下面代码里看到，就是 ``FontAwesome.font``.

.. code-block:: javascript

  import Exponent from 'Exponent';

  function cacheImages(images) {
    return images.map(image => {
      if (typeof image === 'string') {
        return Image.prefetch(image);
      } else {
        return Exponent.Asset.fromModule(image).downloadAsync();
      }
    });
  }

  function cacheFonts(fonts) {
    return fonts.map(font => Exponent.Font.loadAsync(font));
  }

  class AppContainer extends React.Component {
    state = {
      appIsReady: false,
    }

    componentWillMount() {
      this._loadAssetsAsync();
    }

    render() {
      if (!this.state.appIsReady) {
        return <Components.AppLoading />;
      }

      return <MyApp />;
    }

    async _loadAssetsAsync() {
      const imageAssets = cacheImages([
        require('./assets/images/exponent-wordmark.png'),
        'http://www.google.com/logo.png',
      ]);

      const fontAssets = cacheFonts([
        FontAwesome.font,
      ]);

      await Promise.all([
        ...imageAssets,
        ...fontAssets,
      ]);

      this.setState({appIsReady: true});
    }
  }

完整例子可以看: `github/exponentjs/new-project-template <https://github.com/exponentjs/new-project-template/blob/9c5f99efa9afcbefdadefe752ea350cc378c0f0d/main.js>`_.
