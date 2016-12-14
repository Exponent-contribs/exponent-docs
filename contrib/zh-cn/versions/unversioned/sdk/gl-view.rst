GLView
======

.. py:class:: Exponent.GLView

   一个 ``View``, 作为 OpenGL ES 渲染对象。在加载的时候，一个 OpenGL ES 环境会创建。
   每一帧它的 drawing buffer 都会当作这个 ``View`` 的内容。

   除了一般的 ``View`` 属性，比如 layout 还有 触摸操作， 还有下面的这些属性：

   .. py:attribute:: onContextCreate

      当 OpenGL ES 环境创建的时候，会调用这个方法。这个方法需要一个参数，用来代表底层的 OpenGL ES 环境:
      :ref:`gl <gl-object>`.

.. _gl-object:

``gl`` 对象
-----------------

一旦组件加载，OpenGL ES 环境创建成功，传入属性 :any:`onContextCreate
<Exponent.GLView.onContextCreate>` 的 `gl` 对象就成为 OpenGL ES 环境的接口，提供
类似 WebGL 的 API。类似 WebGL 1 规范，`WebGLRenderingContext
<https://www.khronos.org/registry/webgl/specs/latest/1.0/#5.14>`_. 还有一个方法
`endFrameExp` 可以通知当前环境当前帧可以显示了。类似其他 OpenGL 平台上的 'swap buffers'
API 调用。

SDK 11.0.0 还没有实现所有的 WebGL 功能。我们会在接下来的 SDK 版本里不断完善 API。

下面是还没有实现的 `WebGLRenderContext` 方法。

* Buffer

  * ``isBuffer``

* Texture

  * ``compressedTexImage2D``
  * ``compressedTexSubImage2D``
  * ``copyTexImage2D``
  * ``copyTexSubImage2D``
  * ``getTexParameter``
  * ``isTexture``
  * ``texSubImage2D``

* Program an shaders

  * ``bindAttribLocation``
  * ``getAttachedShaders``
  * ``isProgram``
  * ``isShader``

* Uniform an attributes

  * ``getUniform``
  * ``getVertexAttrib``
  * ``getVertexAttribOffset``
  * ``vertexAttrib1fv``
  * ``vertexAttrib2fv``
  * ``vertexAttrib3fv``
  * ``vertexAttrib4fv``

* Misc

  * ``finish``
  * ``getSupportedExtensions``

* Framebuffer

  * ``checkFramebufferStatus``
  * ``createFramebuffer``
  * ``deleteFramebuffer``
  * ``framebufferRenderbuffer``
  * ``framebufferTexture2D``
  * ``getFramebufferAttachmentParameter``
  * ``isFramebuffer``

* Renderbuffer

  * ``bindRenderbuffer``
  * ``createRenderbuffer``
  * ``deleteRenderbuffer``
  * ``getRenderbufferParameter``
  * ``isRenderbuffer``
  * ``renderbufferStorage``

``readPixels`` 当前只支持 Android。

``texImage2D`` 只支持 9 个参数的形式。最后一个参数要么是一个 `ArrayBuffer` (WebGL 规范里的 texture 数据),
要么是一个 `Exponent.Asset` (作为 texture source 的一张图片)。

为了性能，当前这些方法对参数不做类型或者边界检查。所以如果传入的参数不合法，就会导致 native 崩溃。
之后的 SDK 版本我们计划加入参数检查。
