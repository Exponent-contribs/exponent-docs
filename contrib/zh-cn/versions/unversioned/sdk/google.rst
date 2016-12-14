Google
======

Exponent 应用集成 Google。Exponent 提供了比较少的 native API, 因为你可以直接 HTTP 访问 Google 的 `REST APIs
<https://developers.google.com/apis-explorer/>`_ 比如:
`fetch <https://facebook.github.io/react-native/docs/network.html#fetch>`_

.. code-block:: javascript

  // Example of using the Google REST API
  async function getUserInfo(accessToken) {
    let userInfoResponse = await fetch('https://www.googleapis.com/userinfo/v2/me', {
      headers: { Authorization: `Bearer ${accessToken}`},
    });

    return userInfoResponse;
  }

在 Exponent 应用里使用
"""""""""""""""""""""""""""""""""""

在 Exponent 客户端里，你只能用 WebView 方式登录。如果你 build 一个 standalone app, 你可以用该平台的 native 登录，文档最后会详细介绍。

使用 Google 登录，你需要在 Google Developer Console 创建一个项目，然后创建一个 OAuth 2.0 client ID。

- 进入 `Credentials Page <https://console.developers.google.com/apis/credentials>`_
- 如果你还没创建项目的话，新建一个
- Once that is complete, click "Add Credentials" and then "OAuth client ID." You will be prompted to set the product name on the consent screen, go ahead and do that.
- 完成以后，点击 "Add Credentials", 然后 "OAuth client ID"。会提示你设置 product name, 输入就好了。
- 应用类型选择 "Web Application", 你也可以给它一个名字。你不需要提供 'authorized origin'。
- 添加 ``https://oauth.host.exp.com`` 到 "Authorised redirect URIs"。
- 点击 "Create"
- 现在你会看到一个有 client ID 和 secret 的框。
- 你需要这个 client ID 来登录，如下:

.. code-block:: javascript

  import Exponent from 'exponent';

  async function signInWithGoogleAsync() {
    try {
      const result = await Exponent.Google.logInAsync({
        webClientId: YOUR_CLIENT_ID_HERE,
        scopes: ['profile', 'email'],
      });

      if (result.type === 'success') {
        return result.accessToken;
      } else {
        return {cancelled: true};
      }
    } catch(e) {
      return {error: true};
    }
  }

.. function:: Exponent.Google.logInAsync(options)

  提示用户用 Google 登录，并且授予 app 权限来获取该用户 Google 的部分信息

   :param object options:
      参数map。

      * **behavior** (*string*) -- 用什么方式登录, 支持 ``web`` 或者 ``system``。
        本地 (``system``) 只支持 standalone app (按照下面的步骤 build)。默认在 Exponent
        应用里是 ``web``, standalone 里是 ``system``.

      * **scopes** (*array*) -- 当前登录需要 Google 的哪些权限(数组), 权限为字符串, 具体参考: (`<https://gsuite-developers.googleblog.com/2012/01/tips-on-using-apis-discovery-service.html>`_).
        默认权限是 ``['profile', 'email']``。

      * **webClientId** (*string*) -- 这个 app 在 Google 注册的 client ID, 支持 web 登录。

      * **iosClientId** (*string*) -- 这个 app 在 Google 注册的 client ID, 支持 standalone app 本地登录。

   :returns:
      如果用户或者 Google 取消登录，返回 ``{ type: 'cancel' }``.

      否则的话，返回 ``{ type: 'success', accessToken, idToken,
      serverAuthCode, user: {...profileInformation} }``. ``accessToken`` 是一个字符串，用来请求 Google HTTP API。

发布 Android standalone app
""""""""""""""""""""""""""""""""""""""""

如果你想在 standalone app 里支持 native 登录，你可以按照下面的步骤。不需要的话，
你只需要在 ``signInAsync`` options 参数里定义 ``behavior: 'web'``, 跳过下面
这些步骤就好了。

1. 如果你还没有像上面说的创建一个 "Web Application" client ID, 你需要先创建一个，我们后面会用到。
2. build standalone app, 后面会用到。
3. 进入 Google Developer console 你的 app 里 (你可能在步骤 1 或者之前已经创建过).
4. 点击 "Add Credentials" 然后 "API Key".
5. 点击 "Restrict Key".
6. 在 "Key restriction" 里选择 "Android apps", 然后点击 "Add package name and fingerprint".
7. 运行 ``keytool -list -printcert -jarfile growler.apk | grep SHA1 | awk '{ print $2 }'`` (替换 ``growler.apk`` 为第 2 步生成的apk).
8. 把第 7 步的输出填写到 "Signing-certificate fingerprint" 字段。
9. 添加 ``exp.json`` 里的 package name, (比如: ca.brentvatne.growlerprowler) 到 Package name 字段. 保存.
10. 打开 ``exp.json``，添加 client id 到 ``android.config.googleSignIn.apiKey``.
11. 运行 ``keytool -list -printcert -jarfile growler.apk | grep SHA1 | awk '{ print $2 } | sed -e 's/\://g'`` (替换 ``growler.apk`` 为第 2 步生成的apk).
12. 把第 11 步的输出添加到 ``exp.json``, key 为 ``android.config.googleSignIn.certificateHash``.
13. 当你用 ``Exponent.Google.logInAsync(..)`` 的时候, 一定要把第 1 步拿到的 Web Application client ID 作为 ``webClientId`` option 传参。我们也不知道为什么 Google 在 Android 上需要这一步，我们就姑且这么做把。
14. 重新 build 你的 standalone app.

发布 iOS standalone app
""""""""""""""""""""""""""""""""""""

如果你想在 standalone app 里支持 native 登录，你可以按照下面的步骤。不需要的话，
你只需要在 ``signInAsync`` options 参数里定义 ``behavior: 'web'``, 跳过下面
这些步骤就好了。

1. 如果还没添加的话，添加 ``bundleIdentifier`` 到你的 ``exp.json``
2. 在 Google Developer Console 创建一个 app (如果还没为当前项目创建的话)。
3. 点击 "Add Credentials" 然后 "OAuth client ID".
4. "Application Type" 选择 "iOS"。
5. 在 "Bundle ID" 字段填写你的 ``bundleIdentifier``， 然后点击 "Create".
6. 添加给的 "iOS URL scheme" 到你的 ``exp.json``, key 为 ``ios.config.googleSignIn.reservedClientId``.
7. 当你用 ``Exponent.Google.logInAsync`` 的时候, 把 "Client ID" 作为 ``iosClientId`` option, 比如: ``Exponent.Google.logInAsync({iosClientId: YOUR_CLIENT_ID, ...etc});``.
8. 重新 build 你的 standalone app.
