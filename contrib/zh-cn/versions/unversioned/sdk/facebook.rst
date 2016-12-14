Facebook
==============

Exponent 应用集成 Facebook。Exponent 提供了比较少的 native API, 因为你可以直接 HTTP 访问 Facebook 的 `Graph API <https://developers.facebook.com/docs/graph-api>`_ 比如:
`fetch <https://facebook.github.io/react-native/docs/network.html#fetch>`_

请先阅读 `Facebook's developer documentation <https://developers.facebook.com/docs/apps/register>`_ 注册一个 Facebook API 应用，然后获取这个应用 ID。iOS 的话，需要添加 `host.exp.Exponent` 作为 'Bundle ID'。Android 的话，需要添加 key hash ``rRW++LUjmZZ+58EbN5DVhGAnkX4=``. 你的应用设置里 "Settings > Basic" 应该类似:

.. image:: img/facebook-app-settings.png
  :width: 95%
  :align: center

为了不影响其他用户登录，你可能需要把 app 从 'development mode' 切换到 'public mode'。

.. function:: Exponent.Facebook.logInWithReadPermissionsAsync(appId, options)

   提示用户用 Facebook 登录，并且授予 app 权限来获取该用户 Facebook 的部分信息。

   :param string appId:
      你的 Facebook 应用 ID. 具体可以看: `Facebook's developer documentation
      <https://developers.facebook.com/docs/apps/register>`_

   :param object options:
      参数map。

      * **permissions** (*array*) -- 当前登录需要 Facebook 的哪些权限(数组), 权限为字符串，具体参考: `Facebook API documentation
        <https://developers.facebook.com/docs/facebook-login/permissions>`_. 默认的权限是 ``['public_profile', 'email', 'user_friends']``.
      * **behavior** (*string*) -- 用什么方式登录。当前该参数只支持 iOS, 必须为以下几种：

        * ``'web'`` (default) -- 弹出登录 modal ``UIWebView``.
        * ``'native'`` -- 尝试用本地 Facebook 应用登录。只支持 standalone apps。
        * ``'browser'`` -- 尝试用 Safari 或者 ``SFSafariViewController`` 登录。只支持 standalone apps。
        * ``'system'`` -- 尝试用系统登录过的 Facebook 账号登录。

   :returns:
      如果用户或者 Facebook 取消登录，返回 ``{ type: 'cancel' }``.

      否则的话，返回 ``{ type: 'success', token, expires }``. ``token`` 是 access token 字符串，用来请求 Facebook HTTP API。
      ``expires`` 是 token 过期时间，从 epoch 开始按秒计数。你可以存储 access token 到，比如说: ``AsyncStorage``, 然后一直使用到过期时间为止。

   :example:
      .. code-block:: javascript

        async function logIn() {
          const { type, token } = await Exponent.Facebook.logInWithReadPermissionsAsync(
            '<APP_ID>', {
              permissions: ['public_profile'],
            });
          if (type === 'success') {
            // Get the user's name using Facebook's Graph API
            const response = await fetch(
              `https://graph.facebook.com/me?access_token=${token}`);
            Alert.alert(
              'Logged in!',
              `Hi ${(await response.json()).name}!`,
            );
          }
        }

      把 ``<APP_ID>`` 替换成有效的 Facebook 应用 ID, 上面的代码会提示用户用 Facebook
      登录，然后显示出用户的名字。这段代码使用了 React Native 的 `fetch
      <https://facebook.github.io/react-native/docs/network.html#fetch>`_ 来查询
      Facebook 的 `Graph API
      <https://developers.facebook.com/docs/graph-api>`_.

发布 Android  standalone 应用
""""""""""""""""""""""""""""""""""""""""

1. build standalone app
2. 运行 ``keytool -list -printcert -jarfile growler.apk | grep SHA1 | awk '{ print $2 }' | xxd -r -p | openssl base64`` (替换 ``growler.apk`` 为第 1 步生成的apk).
3. 把第 2 步输出添加到你的 Facebook 开发 app 页面的 ``Key Hashes`` 选项，在 Basic Settings 下面。保存就好了。
