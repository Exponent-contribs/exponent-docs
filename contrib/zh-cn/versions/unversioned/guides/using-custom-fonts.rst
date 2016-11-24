.. 使用自定义字体s:

******************
使用自定义字体
******************

iOS 和 Android 系统都有他们自带的平台字体，但如果你想给你的 app 增加更多的商标特性，一种精心挑选的字体会帮大忙。在这份指南中，我们会指导你如何在你的Exponent app 中添加自定义字体。我们将会使用谷歌字体<https://fonts.google.com/>`_中的 Open Sans <https://fonts.google.com/specimen/Open+Sans>`_作为示例，其他任意字体的添加过程也是一样的，所以请放心地在你的 use case 中添加这个字体。开始之前，让我们前往这个网站并下载好 Open Sans <https://fonts.google.com/specimen/Open+Sans>`_。

初始代码
=============

首先我们从一个以 ”Hello world!” 为基础的 app开始。在 XDE/exp 中新建一个项目并以以下示例修改 main.js：

.. code-block:: javascript

  import {
    Text,
    View,
  } from 'react-native';

  import React from 'react';
  import Exponent from 'exponent';

  class App extends React.Component {
    render() {
      return (
        <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
          <Text style={{ fontSize: 56 }}>
            Hello, world!
          </Text>
        </View>
      );
    }
  }

  Exponent.registerRootComponent(App);

尝试用让这个 app 在没有加入 Open Sans 之前运行起来，这样你可以避免一些基础的设置问题。

下载字体
====================

打开你下载的 Open Sans 压缩文件，解压它并将其中的``OpenSans-Bold.ttf``复制到你的资源地址中。我们推荐的位置是``your-project/assets/fonts``。

.. .. epigraph::
..   **Note:** We don't *have to* download the font, we could alternatively load it from the web. We recommend it, though, so that it doesn't just disappear on you like things on the web sometimes do.

载入字体
============================

我们将会通过:ref:`Exponent SDK <exponent-sdk>` 组件载入并使用字体，这个组件在你新建一个新的 Exponent 项目时已经预安装好了。但是如果出于某些奇怪的原因，你可能还没安装好它，这时候你可以通过``npm install --save exponent``将其安装在你的项目地址中。添加下列的``import``在你的应用代码中：
.. code-block:: javascript

   import { Font } from 'exponent';

``exponent``库中提供了一个在你的 JaveScript 代码中接入设备原生功能的 API。``Font``是一个用来处理字体相关任务的模块。首先，我们需要以下功能 :func:`Exponent.Font.loadAsync` 从资源地址中载入字体。我们可以用``App``组件中的生命周期方式 componentDidMount() <https://facebook.github.io/react/docs/component-specs.html#mounting-componentdidmount>`_来实现这个。在``App``中添加如下代码所示的方法。现在我们已经把字体文件存储在磁盘中而且 Font SDK 也已经导入，代码如下：

.. code-block:: javascript

      class App extends React.Component {
        componentDidMount() {
          Font.loadAsync({
            'open-sans-bold': require('./assets/fonts/OpenSans-Bold.ttf'),
          });
        }

        // ...
      }

这会载入 Font Awesome 并且在 Exponent 中的字体映射中与``'open-sans-bold'``链接起来。现在我们只需要在``Text``组件中将其调取。

.. epigraph::
  **说明:** 在 Exponent 载入的字体目前不支持字体高度（``fontWeight``）与风格（``fontStyle``）等特性――你需要载入这些字体的变式并将他们以名称来指定，正如我们在这里对 bold 的操作。

使用字体
======================================

你或许还记得在 React Native 中使用``fontFamily``特性来在``Text``部分中指定字体。因为跟踪你载入的不同的字体文件有时会引起混淆，Exponent 提供了函数:func:`Exponent.Font.style` 用来返回你指定名字的字体的风格特性（包括``fontFamily``），所以你需要依照如下的代码来改变你的``Text``元素。

.. code-block:: javascript

          <Text style={{ fontFamily: 'open-sans-bold', fontSize: 56 }}>
            Hello, world!
          </Text>

之后刷新 App 似乎字体还不是以 Open Sans Bold 显示，你会看到它还是以系统默认字体显示。这个问题是因为 :func:`Exponent.Font.loadAsync`  是一个异步请求并且需要一定的时间去完成。在它完成之前，``Text``部分已经在默认字体上渲染了，因为它找不到``'open-sans-bold'``（因为还没有被载入）

.. epigraph::
  **说明:** 如果你感到好奇你可以在你的代码中加入``console.log(Font.style('open-sans-bold'));``; 然后你可以看到它的值为 {fontFamily: 'some-long-id-open-sans-bold'}。为了避免因为通过 Exponent 打开的多个应用的字体崩溃，我们用session id 预设 family name。

在渲染之前等待字体载入
=============================================

我们需要在字体加载完毕之后重新渲染``Text``部分。可以在``App``部分的 state 里创建一个 boolean 值: ``fontLoaded``, 用来跟踪字体是否已加载完毕。只有在``fontLoaded``变为``true``之后我们才渲染``Text``部分。

首先在``App``类构造时我们初始化``fontLoaded``为 false：

.. code-block:: javascript

    class App extends React.Component {
      state = {
        fontLoaded: false,
      };

      // ...
    }

接着，当字体完成载入时，我们必须把``fontLoaded``设为``true``。当字体成功加载和可以使用时，:func:`Exponent.Font.loadAsync`会返回一个值：``Promise``。所以当我们用``componentDidMount()``来使用`async/await <https://blog.getexponent.com/react-native-meets-async-functions-3e6f81111173>`_并等待直到字体成功加载，然后更新状态。

.. code-block:: javascript

      class App extends React.Component {
        async componentDidMount() {
          await Font.loadAsync({
            'open-sans-bold': require('./assets/fonts/OpenSans-Bold.ttf'),
          });

          this.setState({ fontLoaded: true });
        }

        // ...
      }

最后，我们只想渲染``Text``部分如果``fontLoaded``的值为``true``，我们可以通过用以下操作取代``Text``元素：

.. code-block:: javascript

          <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
            {
              this.state.fontLoaded ? (
                <Text style={{ fontFamily: 'open-sans-bold', fontSize: 56 }}>
                  Hello, world!
                </Text>
              ) : null
            }
          </View>
React Native 会简单地无视值为``null``的子元素，所以当``fontLoaded``的值为``false``时，这会跳过渲染``Text``部分。所以现在刷新此app你会看到它渲染了Font Awesome的玻璃图标！

.. epigraph::
  **说明:** 通常你希望想要在app显示前载入默认字体来在字体载入之后避免文字闪烁。一个推荐的方法是移动``Font.loadAsync``命令到顶层组件。
