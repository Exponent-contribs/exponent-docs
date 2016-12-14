Contacts
========

提供手机上的联系人。

.. function:: Exponent.Contacts.getContactsAsync(fields)

   获取所有联系人。返回每个联系人的姓名，也可能包含手机号和邮箱。

   :param array fields:
      获取字段数组。必须是 ``Exponent.Contacts.PHONE_NUMBER`` 或者 ``Exponent.Contacts.EMAIL``.
   :returns:
      一个数组，元素格式是 ``{ id, name, phoneNumber, email }``, ``phoneNumber`` 和 ``email`` 只有在
      ``fields`` 参数里包含才会返回。

   :example:
      .. code-block:: javascript

        async function showFirstContactAsync() {
          const contacts = await Exponent.Contacts.getContactsAsync([
            Exponent.Contacts.PHONE_NUMBER,
            Exponent.Contacts.EMAIL,
          ]);
          if (contacts.length > 0) {
            Alert.alert(
              'Your first contact is...',
              `Name: ${contacts[0].name}\n` +
              `Phone: ${contacts[0].phoneNumber}\n` +
              `Email: ${contacts[0].email}`
            );
          }
        }

      这个方法会显示用户第一个联系人信息。
