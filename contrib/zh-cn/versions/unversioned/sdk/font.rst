Font
====

允许网络加载字体，然后在 React Native 组件中使用。

.. function:: Exponent.Font.loadAsync(name, url)

   在网络加载一个字体，并且赋予它一个 name。

   :param string name:
      字体的标识。你可以取任意名称。

   :returns:
      不返回，直到该字体可用。

.. function:: Exponent.Font.loadAsync(map)

   让 :func:`Exponent.Font.loadAsync` 更方便，可以一次加载多种字体。

   :param object map:
      字体 name 到 url 的 map, 参考 :func:`Exponent.Font.loadAsync`.

   :returns:
      不返回，直到所有字体都可用。

   :example:
      .. code-block:: javascript

        Exponent.Font.loadAsync({
          title: 'http://url/to/font1.ttf',
          cursive: 'http://url/to/font2.ttf',
        });

      相当于对每组 name 和 URL 调用 :func:`Exponent.Font.loadAsync`.

.. function:: Exponent.Font.style(name)

   返回字体样式属性，然后可以在 ``Text`` 或者其他 React Native 组件里用。即使在未调用
   :func:`Exponent.Font.loadAsync` 的情况下，调用 style 方法也是安全的; 仍然会返回
   正确的样式属性。这样子你就可以用 ``StyleSheet.create()`` 和这个方法了。

   :param string name:
      :func:`Exponent.Font.loadAsync` 传入的 name。

   :returns:
      一个包含样式属性的对象，然后就可以在 ``Text`` 或者其他组件里用了。

   :example:
      .. code-block:: javascript

        <Text style={{ ...Exponent.Font.style('cursive'), color: 'red' }}>
          Hello world!
        </Text>

      在这个组件渲染之前，字体必须通过调用 ``Exponent.Font.loadAsync('cursive', 'http://url/to/font.ttf')`` 提前加载好。
