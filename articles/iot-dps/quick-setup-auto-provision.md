---
title: "Настройка подготовки устройств на портале Azure | Документация Майкрософт"
description: "Краткое руководство Azure. Настройка службы подготовки устройств для Центра Интернета вещей Azure на портале Azure"
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2017
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: e444e46b9044d822731683781112be83c8c6db04
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2018
---
# <a name="set-up-the-iot-hub-device-provisioning-service-with-the-azure-portal"></a>Настройка службы подготовки устройств для Центра Интернета вещей на портале Azure

В этих инструкциях показано, как настроить облачные ресурсы Azure на портале для подготовки устройств. Эта настройка включает создание Центра Интернета вещей, создание новой службы подготовки устройств для Центра Интернета вещей и связывание двух служб. 

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.


## <a name="log-in-to-the-azure-portal"></a>Войдите на портал Azure.

Войдите на [портал Azure](https://portal.azure.com/).

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

1. Нажмите кнопку **Создать ресурс** в верхнем левом углу окна портала Azure.

2. Выберите **Интернет вещей**, **Центр Интернета вещей** и нажмите кнопку **Создать**. 

3. **Присвойте имя** Центру Интернета вещей. Выберите один из доступных вариантов цены, укажите число [единиц Центра Интернета вещей](https://azure.microsoft.com/pricing/details/iot-hub/), выберите число разделов для сообщений, отправляемых из устройства в облако, а также подписку, которую будет использовать этот ресурс. Введите имя новой или существующей группы ресурсов и выберите расположение. Затем нажмите кнопку **Создать**.

    ![Ввод основных сведений о Центре Интернета вещей в колонке портала](./media/quick-setup-auto-provision/create-iot-hub-portal.png)  

4. После успешного развертывания Центра Интернета вещей автоматически откроется колонка сводки Центра.


## <a name="create-a-new-instance-for-the-iot-hub-device-provisioning-service"></a>Создание экземпляра службы подготовки устройств Центра Интернета вещей

1. Нажмите кнопку **Создать ресурс** в верхнем левом углу окна портала Azure.

2. *Выполните в Marketplace поиск* по запросу **Device provisioning service**. Выберите **Служба подготовки устройств Центра Интернета вещей** и нажмите кнопку **Создать**. 

3. **Присвойте имя** экземпляру службы подготовки устройств. Выберите подписку, которая будет использоваться для этого экземпляра, и присвойте имя новой или существующей группе ресурсов. Выберите расположение. Затем нажмите кнопку **Создать**.

    ![Ввод основных сведений об экземпляре службы подготовки устройств в колонке портала](./media/quick-setup-auto-provision/create-iot-dps-portal.png)  

4. После успешного развертывания службы автоматически откроется колонка сводки службы.


## <a name="link-the-iot-hub-and-your-device-provisioning-service"></a>Связывание Центра Интернета вещей со службой подготовки устройств

1. Нажмите кнопку **Все ресурсы** в меню слева на портале Azure. Выберите созданный выше экземпляр службы подготовки устройств.  

2. В колонке сводки службы подготовки устройств выберите **Связанные Центры Интернета вещей**. Нажмите кнопку **Добавить** сверху. 

3. На портале в колонке **Добавление ссылки на Центр Интернета вещей** выберите текущую подписку или введите имя и строку подключения для другой подписки. В раскрывающемся списке выберите имя концентратора. Затем нажмите кнопку **Сохранить**. 

    ![Связывание имени центра с экземпляром службы подготовки устройств в колонке портала](./media/quick-setup-auto-provision/link-iot-hub-to-dps-portal.png)  

3. Теперь вы выбранный центр будет отображается в колонке **Связанные Центры Интернета вещей**. Может потребоваться щелкнуть **Обновить**, чтобы **связанные Центры Интернета вещей** отобразились.



## <a name="clean-up-resources"></a>Очистка ресурсов

Другие краткие руководства в этой коллекции созданы на основе этого документа. Если вы планируете продолжать работу с последующими краткими руководствами или обычными руководствами, не удаляйте созданные ресурсы. В противном случае удалите все созданные ресурсы, выполнив на портале Azure следующие действия.

1. В меню слева на портале Azure нажмите кнопку **Все ресурсы** и откройте службу подготовки устройств. В верхней части колонки **Все ресурсы** щелкните **Удалить**.  
2. В меню слева на портале Azure нажмите кнопку **Все ресурсы** и выберите свой Центр Интернета вещей. В верхней части колонки **Все ресурсы** щелкните **Удалить**.  

## <a name="next-steps"></a>Дополнительная информация

Вы развернули Центр Интернета вещей и экземпляр службы подготовки устройств, а затем связали эти два ресурса. Чтобы узнать, как подготовить виртуальное устройство, см. руководство по созданию виртуального устройства.

> [!div class="nextstepaction"]
> [Краткое руководство по созданию виртуального устройства](./quick-create-simulated-device.md)
