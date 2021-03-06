---
title: Использование Azure CDN с SAS | Документация Майкрософт
description: Azure CDN поддерживает использование подписанных URL-адресов (SAS) для предоставления ограниченного доступа к частным контейнерам хранилища.
services: cdn
documentationcenter: ''
author: dksimpson
manager: ''
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: v-deasim
ms.openlocfilehash: 44c28f45b7be8fbaa47a16d8ab07892ab146c39e
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/19/2018
---
# <a name="using-azure-cdn-with-sas"></a>Использование Azure CDN с SAS

При настройке учетной записи хранения для кэширования содержимого сети доставки содержимого (CDN) Azure по умолчанию любой пользователь, знающий URL-адреса контейнеров хранилища, может получить доступ к переданным вами файлам. Чтобы защитить файлы в своей учетной записи хранения, можно изменить параметры доступа к контейнерам хранилища, сделав их частными, а не общедоступными. Однако в этом случае никто не сможет получить доступ к вашим файлам. 

Если вы хотите предоставить ограниченный доступ к частным контейнерам хранилища, то можете использовать функцию подписанных URL-адресов (SAS) учетной записи хранения Azure. SAS — это URI, который предоставляет ограниченные права доступа к ресурсам хранилища Azure, не отображая ключ вашей учетной записи. Вы можете предоставить SAS клиентам, которым вы не доверяете ключ учетной записи хранения, но которым вы хотите делегировать доступ к определенным ресурсам учетной записи. Распределяя URI подписи общего доступа к этим клиентам, вы предоставляете им доступ к ресурсу в течение определенного периода времени.
 
SAS дает возможность определить различные параметры доступа к большому двоичному объекту, такие как время запуска и срок действия, разрешения (на чтение и запись) и диапазоны IP-адресов. В этой статье описывается использование SAS вместе с Azure CDN. Дополнительные сведения о SAS, включая данные о его создании и настройке параметров, см. в статье [Использование подписанных URL-адресов (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Настройка Azure CDN для работы с SAS хранилища
Для использования SAS с Azure CDN рекомендуются следующие три варианта. Предполагается, что вы уже создали рабочий SAS (см. предварительные требования). 
 
### <a name="prerequisites"></a>предварительным требованиям
Для начала создайте учетную запись хранения, а затем создайте SAS для ресурса. Вы можете создать два типа подписанных URL-адресов: SAS службы или учетной записи. Дополнительные сведения см. в разделе [Типы подписанных URL-адресов](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures).

После создания маркера SAS можно обращаться к файлу в хранилище BLOB-объектов, добавляя `?sv=<SAS token>` в URL-адрес. Этот URL-адрес имеет следующий формат. 

`https://<account name>.blob.core.windows.net/<container>/<file>?sv=<SAS token>`
 
Например: 
 ```
https://democdnstorage1.blob.core.windows.net/container1/demo.jpg?sv=2017-04-17&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Дополнительные сведения о параметрах см. в разделах [Сведения о параметрах SAS](#sas-parameter-considerations) и [Параметры подписанного URL-адреса](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#shared-access-signature-parameters).

![Параметры SAS CDN](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-azure-cdn"></a>Вариант 1. Использование SAS с передачей в хранилище BLOB-объектов из Azure CDN

Этот вариант является самым простым и использует только один маркер SAS, который передается из Azure CDN на сервер-источник. Он поддерживается **Azure CDN от Verizon** и **Azure CDN от Akamai**. 
 
1. Выберите конечную точку, щелкните **Правила кеширования**, затем выберите **Кэшировать каждый уникальный URL-адрес** из списка **Query string caching** (Кэширование строк запросов).

    ![Правила кэширования CDN](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. После настройки SAS в вашей учетной записи хранения используйте маркер SAS с URL-адресом Azure CDN для доступа к файлу. 
   
   Полученный SAS имеет следующий формат: `https://<endpoint hostname>.azureedge.net/<container>/<file>?sv=<SAS token>`

   Например:    
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg/?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. Точная настройка длительности кэширования выполняется либо с помощью правил кеширования, либо путем добавления заголовков `Cache-Control` на сервер-источник. Так как Azure CDN рассматривает маркер SAS как обычную строку запроса, необходимо настроить длительность кэширования, которая истекает при истечении срока действия SAS или раньше. В противном случае, если файл кэшируется дольше, чем активен SAS, файл может остаться доступным с сервера-источника Azure CDN по истечении срока действия SAS. В этом случае, если вы хотите сделать кэшированный файл недоступным, выполните операцию очистки кэша в файле. Сведения о настройке длительности кэширования для Azure CDN см. в статье [Управление поведением кэширования сети доставки содержимого Azure с помощью правил кэширования](cdn-caching-rules.md).

### <a name="option-2-hidden-cdn-security-token-using-a-rewrite-rule"></a>Вариант 2. Скрытый маркер безопасности CDN с использованием правила переопределения
 
С помощью этого варианта вы можете защитить исходное хранилище BLOB-объектов, не требуя от пользователя Azure CDN указывать маркер SAS в URL-адресе. Его можно использовать, если вам не нужны определенные ограничения доступа к файлу, но вы хотите, чтобы пользователи не могли напрямую обращаться к источнику хранения, чтобы улучшить время разгрузки Azure CDN. Этот параметр доступен только для профилей **Azure CDN уровня "Премиум" от Verizon**. 
 
1. Используйте [обработчик правил](cdn-rules-engine.md) для создания правила переопределения URL-адресов. Новые правила распространяются в течение 90 минут.

   ![Кнопка управления CDN](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![Кнопка обработчика правил Azure CDN](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   Ниже приведен пример правила переопределения URL-адресов, в котором используется шаблон регулярного выражения с группой записи и конечной точкой *storagedemo*.
   
   Источник:   
   `(/test/*.)`
   
   Назначение:   
   ```
   $1sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```

   ![Правило переопределения URL-адресов CDN](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-2.png)

2. После активации нового правила к файлу в Azure CDN можно обращаться без указания маркера SAS в URL-адресе, используя следующий формат: `https://<endpoint hostname>.azureedge.net/<container>/<file>`.
 
   Например:    
   `https://demoendpoint.azureedge.net/container1/demo.jpg`
       
   Обратите внимание на то, что доступ к файлам на конечной точке CDN можно получить независимо от использования маркера SAS. 

3. Точная настройка длительности кэширования выполняется либо с помощью правил кеширования, либо путем добавления заголовков `Cache-Control` на сервер-источник. Так как Azure CDN рассматривает маркер SAS как обычную строку запроса, необходимо настроить длительность кэширования, которая истекает при истечении срока действия SAS или раньше. В противном случае, если файл кэшируется дольше, чем активен SAS, файл может остаться доступным с сервера-источника Azure CDN по истечении срока действия SAS. В этом случае, если вы хотите сделать кэшированный файл недоступным, выполните операцию очистки кэша в файле. Сведения о настройке длительности кэширования для Azure CDN см. в статье [Управление поведением кэширования сети доставки содержимого Azure с помощью правил кэширования](cdn-caching-rules.md).

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Вариант 3. Использование аутентификации маркера безопасности CDN с правилом подстановки

Этот вариант является наиболее безопасным и настраиваемым. Для использования аутентификации на основе маркера безопасности Azure CDN требуется профиль **Azure CDN уровня "Премиум" от Verizon**. Клиентский доступ основан на параметрах безопасности, установленных для маркера безопасности. Но если позже SAS станет недействительным, Azure CDN не сможет повторно проверить содержимое с сервера-источника.

1. [Создайте маркер безопасности Azure CDN](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication) и активируйте его, используя обработчик правил для конечной точки CDN и путь, с помощью которого пользователи смогут получить доступ к файлу.

   URL-адрес маркера безопасности имеет следующий формат.   
   `https://<endpoint hostname>.azureedge.net/<container>/<file>?<security_token>`
 
   Например:    
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg?a4fbc3710fd3449a7c99986bkquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   Параметры аутентификации на основе маркера безопасности отличаются от параметров маркера SAS. Если вы решите использовать срок действия при создании маркера безопасности, установите для него то же значение, что и для маркера SAS. Это обеспечит прогнозируемость срока окончания действия. 
 
2. Используйте [обработчик правил](cdn-rules-engine.md), чтобы создать правило переопределения URL-адресов и разрешить доступ с маркером SAS ко всем большим двоичным объектам в контейнере. Новые правила распространяются в течение 90 минут.

   Ниже приведен пример правила переопределения URL-адресов, в котором используется шаблон регулярного выражения с группой записи и конечной точкой *storagedemo*.
   
   Источник:   
   `(/test/*.)`
   
   Назначение:   
   ```
   $1&sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```

   ![Правило переопределения URL-адресов CDN](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-3.png)

3. При обновлении SAS обновите правило переопределения URL-адресов, чтобы использовать новый маркер SAS. 

## <a name="sas-parameter-considerations"></a>Рекомендации по настройке параметров SAS

Так как параметры SAS не отображаются в Azure CDN, Azure CDN не может изменить свой режим доставки на их основе. Определенные ограничения параметров применяются только к запросам, которые Azure CDN отправляет на сервер-источник, а не к запросам клиента к Azure CDN. Это различие следует учитывать при настройке параметров SAS. Если необходимы эти дополнительные возможности и вы используете [вариант 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule), установите соответствующие ограничения в маркере безопасности Azure CDN.

| Имя параметра SAS | ОПИСАНИЕ |
| --- | --- |
| Начало | Время, когда Azure CDN сможет обращаться к файлу большого двоичного объекта. Из-за разницы в показаниях часов (когда сигнал времени поступает в разное время для различных компонентов) установите время на 15 минут раньше, если вы хотите, чтобы ресурс был доступен немедленно. |
| End | Время, после которого Azure CDN больше не сможет обращаться к файлу большого двоичного объекта. Ранее кэшированные файлы в Azure CDN по-прежнему будут доступны. Чтобы управлять временем окончания срока действия файла, установите соответствующее время для маркера безопасности Azure CDN либо очистите ресурс. |
| Разрешенные IP-адреса | Необязательный элемент. Если вы используете **Azure CDN от Verizon**, примените параметр диапазонов, указанных на странице [Azure CDN from Verizon Edge Server IP Ranges](https://msdn.microsoft.com/library/mt757330.aspx) (Диапазоны IP-адресов пограничного сервера Azure CDN от Verizon). Если вы используете **Azure CDN от Akamai**, нельзя задать параметр диапазонов IP-адресов, так как они не являются статичными.|
| Разрешенные протоколы | Протоколы, разрешенные для запроса, сделанного с помощью SAS учетной записи. Рекомендуется использовать параметр HTTPS.|

## <a name="see-also"></a>См. также
- [Использование подписанных URL-адресов (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Подписанные URL-адреса. Часть 2: создание и использование подписанного URL-адреса в службе BLOB-объектов](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)
- [Защита ресурсов сети доставки содержимого Azure с помощью аутентификации на основе маркеров](https://docs.microsoft.com/azure/cdn/cdn-token-auth)
