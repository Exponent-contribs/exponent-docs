.. _all-about-assets:

***************************
預先載入 & 快取 Assets
***************************

為了確保在 app 啟動時的載入畫面會在 assets 載入時保持顯示，我們會先只顯示
:ref:`Exponent.Components.AppLoading <app-loading>`，直到全部東西都載入完畢。

對於需要保存在本地檔案系統的圖片，我們可以使用  ``Exponent.Asset.fromModule(image).downloadAsync()`` 來下載、並且快取圖片；而網路圖片的部份，我們可以使用 ``Image.prefetch(image)``。

字體的部分則是透過 ``Exponent.Font.loadAsync(font)`` 來預先載入。其中的 ``font`` 參數是一個類似下列的物件：

 ``{OpenSans: require('./assets/fonts/OpenSans.ttf}``

``@exponent/vector-icons`` 組件對這種物件提供方便的使用方式，你可在下面的程式碼中看見，就是 ``FontAwesome.font``。

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

完整範例請見： `github/exponentjs/new-project-template <https://github.com/exponentjs/new-project-template/blob/9c5f99efa9afcbefdadefe752ea350cc378c0f0d/main.js>`_.
