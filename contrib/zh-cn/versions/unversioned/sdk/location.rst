Location
========

这个模块允许从设备里访问地理位置信息。
你的 app 可以轮询当前位置或者订阅位置更新事件。

.. function:: Exponent.Location.getCurrentPositionAsync(options)

   Get the current position of the device.

   :param object options:
      参数map:

      * **enableHighAccuracy** (*boolean*) -- 是否激活高精度模式。如果低精度的话，
        可以防止一些地理位置提供商浪费过多的电池 (比如 GPS)。

   :returns:
      返回一个有下面这些属性的对象:

      * **coords** (*object*) -- 位置坐标，包含以下 fields:

        * **latitude** (*number*) -- 纬度。

        * **longitude** (*number*) -- 经度。

        * **altitude** (*number*) -- 海拔基于 WGS 84 reference ellipsoid (单位为米)。

        * **accuracy** (*number*) -- 位置的不错定半径 (单位为米)。

        * **altitudeAccuracy** (*number*) -- 海拔数值的精确度，(单位为米，只支持 iOS)。

        * **heading** (*number*) -- 设备水平方向移动，从北面开始在指南针上按顺时针方向以度数测量。
          北是 0 度, 东是 90 度，南是 180 度，以此类推。Horizontal direction of travel of this

        * **speed** (*number*) -- 设备的即时速度(多少米每秒)。

      * **timestamp** (*number*) -- 位置信息是什么时候获取的，从 epoch 开始算，单位为 milliseconds。

.. function:: Exponent.Location.watchPositionAsync(options, callback)

   订阅位置更新事件。

   :param object options:
      参数 map:

      * **enableHighAccuracy** (*boolean*) -- 是否激活高精度模式。如果低精度的话，
        可以防止一些地理位置提供商浪费过多的电池 (比如 GPS)。

      * **timeInterval** (*number*) -- 等待下一次更新的最少时间, 单位为 milliseconds。

      * **distanceInterval** (*number*) -- 只在移动了超过这个距离 (单位为米) 才接收更新。

   :param function callback:
      每次位置变化都会调用这个方法，只有一个参数: 一个有下面这些属性的对象:

      * **coords** (*object*) -- 位置坐标，包含以下 fields:

        * **latitude** (*number*) -- 纬度。

        * **longitude** (*number*) -- 经度。

        * **altitude** (*number*) -- 海拔基于 WGS 84 reference ellipsoid (单位为米)。

        * **accuracy** (*number*) -- 位置的不错定半径 (单位为米)。

        * **altitudeAccuracy** (*number*) -- 海拔数值的精确度，(单位为米，只支持 iOS)。

        * **heading** (*number*) -- 设备水平方向移动，从北面开始在指南针上按顺时针方向以度数测量。
          北是 0 度, 东是 90 度，南是 180 度，以此类推。Horizontal direction of travel of this

        * **speed** (*number*) -- 设备的即时速度(多少米每秒)。

      * **timestamp** (*number*) -- 位置信息是什么时候获取的，从 epoch 开始算，单位为 milliseconds。


   :returns:
      返回一个订阅对象, 包含一个属性:

      * **remove** (*function*) -- 无参数调用这个方法可以撤销订阅。
        之后的位置更改就不会再调用这个 callback 方法。
