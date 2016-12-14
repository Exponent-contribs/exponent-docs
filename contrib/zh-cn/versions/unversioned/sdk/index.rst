.. _exponent-sdk:

SDK API Reference
=======================

Exponent SDK 提供一些系统功能，比如联系人，相机，社交登录等。npm 包:
`exponent <https://www.npmjs.com/package/exponent>`_. 可以通过在项目根目录
运行 ``npm install --save exponent`` 来安装包。然后你可以如下在你的 Javascript
代码里 import SDK 库:

.. code-block:: javascript

  import { Contacts } from 'exponent';

You can also import all Exponent SDK modules:

.. code-block:: javascript

  import * as Exponent from 'exponent';

然后你就可以写比如: :func:`Exponent.Contacts.getContactsAsync`.

.. raw:: html

    <h2>Modules</h2>

.. toctree::
   :maxdepth: 2

   amplitude
   app-loading
   asset
   bar-code-scanner
   blur-view
   constants
   contacts
   facebook
   font
   gl-view
   google
   imagepicker
   linear-gradient
   location
   map-view
   permissions
   segment
   svg
   util
   video
