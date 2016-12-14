.. _routing-and-navigation:

********************
路由 & 导航
********************

一个网页上的 "单页应用" 不是一个只有一个页面的应用，不然的话大多数情况下都是没啥用的；实际上
这个应用不会请求浏览器为每个新的页面跳转到对应的URL。实际上一个 "单页应用" 会用它自己的路由
系统 (比如: react-router), 这样子可以解藕页面和对应的地址栏。通常它也会更新地址栏，但是直接
覆盖会导致浏览器重新加载整个页面。目的就是让体验更流畅，还有更像 app。

这点同样适用于本地手机应用。当你跳转到新的页面，与其刷新整个 app 然后从那个页面重新开始，
这个页面会 push 到一个导航栈，然后动画过渡到对应配置里的 view。

Exponent 里我们推荐的路由 & 导航库是 `ExNavigation <https://github.com/exponentjs/ex-navigation>`_.
你可以看 `full documentation for ExNavigation on Github. <https://github.com/exponentjs/ex-navigation>`_.

试试看
^^^^^^^^^^

熟悉 ExNavigation 最好的办法是先试试 `ExNavigation example Exponent app
<https://getexponent.com/@community/ex-navigation-example>`_. 然后回来这里，继续看下面的!

一点介绍: 最简单的路由配置
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

你可以复制下面的代码到一个新的 Exponent 项目里边的 ``main.js``, 然后运行:
``npm install @exponent/ex-navigation --save``.

.. code-block:: javascript

  import Exponent from 'exponent';
  import React from 'react';
  import {
    AppRegistry,
    Text,
    View,
  } from 'react-native';

  import {
    createRouter,
    NavigationProvider,
    StackNavigation,
  } from '@exponent/ex-navigation';

  const Router = createRouter(() => ({
    home: () => HomeScreen,
  }));

  class HomeScreen extends React.Component {
    static route = {
      navigationBar: {
        title: 'Home',
      }
    }

    render() {
      return (
        <View style={{alignItems: 'center', justifyContent: 'center', flex: 1}}>
          <Text onPress={this._handlePress}>HomeScreen!</Text>
        </View>
      )
    }

    _handlePress = () => {
      this.props.navigator.push('home');
    }
  }

首先我们创建一个 router, keys 对应不同的页面. ``HomeScreen`` 是我们第一个 **route component**,
我们在它上面设置一个 ``static route`` 对象属性来明确。然后我们配置这个路由的不同属性，比如过渡的样式，
导航栏上显示什么样的按钮。注册为 routes 的组件会有一个特殊的 prop ``navigator`` 传入，navigator 允许
你做一些路由操作，比如 ``push`` 还有 ``pop``. 这里我们 push home screens 到最顶端。但是为了让它工作，
我们需要先把 ``home`` 路由添加到 ``StackNavigation``, 所以我们可以有一个路由栈，可以 push, pop。 

我们通过在 app 的 root 设置一个 ``NavigationProvider`` 来初始化 ExNavigation。然后我们
显示一个 ``StackNavigation`` child, 设置它的 ``initialRoute`` 为我们之前在 ``createRouter``
里定义的 ``home`` route。

.. code-block:: javascript

  class App extends React.Component {
    render() {
      return (
        <NavigationProvider router={Router}>
          <StackNavigation initialRoute="home" />
        </NavigationProvider>
      );
    }
  }

  Exponent.registerRootComponent(App);

了解 tab 模版
^^^^^^^^^^^^^^^^^^^^^^^^^^

你可能不想每次创建一个新的项目，都重新开始。Tab 就是 Exponent 提供的一个模版可以让你更快开发你的
app. ``@exponent/ex-navigation`` 默认包含它，所以你可以直接用 tab 路由。

让我们来看看 tab 模版和路由相关的项目结构把。你不需要完全遵照这个风格，只不过我们发现它很适合我们。

.. code-block:: text

  ├── main.js
  ├── navigation
  │   ├── RootNavigation.js
  │   └── Router.js
  ├── screens
  │   ├── HomeScreen.js
  │   ├── LinksScreen.js
  │   └── SettingsScreen.js

main.js
-------

在 Exponent 应用里，这个文件通常是你 app 注册的 root 组件。在 root 组件里，你通常
包含所有高级的 ``Provider`` 组件，比如 ``react-redux Provider``, 还有
ExNavigation ``NavigationProvider``. 正如上面例子里你看到的，通常我们也会渲染我们的
``StackNavigation`` 组件。大部分应用包含很多嵌套的 stacks, 我们会看到的。

screens/*Screen.js
------------------

我把所有代表 app 里页面的 root 组件都在在一个 ``Screens`` 目录里 (一个页面不一定非得在哪里定义过，
只要你认为它是合适的 -- 对我来说，通常是任何东西我认为可以 ``push`` 或者 ``pop`` 的)。

navigation/Router.js
--------------------

在上面的这个简单例子里，我们在 ``main.js`` 只用一行就定义了我们的 Router -- 刚开始这样子可以，但是
最终随着页面的增多为了清晰我们会把它抽出来放到单独的文件。也有其他的场景你需要直接导入这个 Router.

navigation/RootNavigation.js
----------------------------

这个组件用来渲染我们的根路由布局 -- 在这个项目里，我们用的 tabs。你也可能会在 Android 里用抽屉布局，
或者其他的布局。在这个模版里，我们在 ``main.js`` 渲染的 ``StackNavigation`` 只会指向
``RootNavigation`` screen, 每个 tab 会渲染它对应的 ``StackNavigation`` 组件。

我们赋予它另外一个功能是订阅推送通知，这样子如果收到或者选择新的通知，我们可以响应切换到新的路由。

了解更多关于路由 & 导航
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``ExNavigation`` 不是唯一的路由库，它是我们推荐的方式，而且我们可能不能回答关于其他库的问题。
你可以了解更多 `on the Github repository <https://github.com/exponentjs/ex-navigation>`_,
或者看一些用 ``ExNavigation`` 创建的一些 app, 比如
`Growler Prowler <https://github.com/brentvatne/growler-prowler>`_, `React Native Playground <https://github.com/exponentjs/rnplay>`_,
还有 `ExNavigation example app <https://github.com/exponentjs/ex-navigation/tree/master/example>`_.
