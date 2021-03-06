---
title: "Управление доступом к служебной шине Azure на основе ролей (RBAC) (предварительная версия) | Документация Майкрософт"
description: "Управление доступом к служебной шине Azure на основе ролей"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 729d6db6b2fc6495ffb0f4fbe4d545d7ad953cef
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/20/2017
---
# <a name="active-directory-role-based-access-control-preview"></a>Управление доступом к служебной шине Azure на основе ролей (предварительная версия)

Microsoft Azure предоставляет интегрированную систему управления доступом к ресурсам и приложениям на основе Azure Active Directory (Azure AD). С помощью Azure AD вы можете осуществлять управление учетными записями пользователей и приложениями специально для приложений на основе Azure или же объединить в федерацию имеющуюся инфраструктуру Active Directory с Azure AD для обеспечения единого входа на всех компьютерах компании, а также для ресурсов и приложений в Azure. Затем можно назначить эти удостоверения пользователя и приложения Azure AD для глобальных ролей и ролей определенных служб, чтобы предоставить доступ к ресурсам Azure.

Для служебной шины Azure управление пространствами имен и всеми связанными ресурсами через портал Azure и API управления ресурсами Azure уже защищено с помощью модели *управления доступом на основе ролей* (RBAC). Сейчас компонент RBAC для операций среды выполнения доступен в общедоступной предварительной версии. 

В приложении, использующем RBAC Azure AD, не нужно обрабатывать правила SAS и ключи, а также любые другие маркеры доступа, относящиеся к служебной шине. Клиентское приложение получает маркер доступа для служебной шины и взаимодействует с Azure AD, чтобы установить контекст аутентификации. С помощью учетных записей пользователей домена, требующих интерактивного входа в систему, приложение не обрабатывает учетные данные напрямую.

## <a name="service-bus-roles-and-permissions"></a>Роли и разрешения служебной шины

В исходной общедоступной предварительной версии можно только добавлять учетные записи Azure AD и субъекты-службы для ролей "Владелец" или "Участник" пространства имен служебной шины, предназначенного для обмена сообщениями. Эта операция предоставляет удостоверению полный доступ ко всем сущностям в пространстве имен. Операции управления, изменяющие топологию пространства имен, изначально поддерживаются только через управление ресурсами Azure, а не через интерфейс управления REST служебной шины. Это также означает, что объект клиента .NET Framework [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) нельзя использовать с учетной записью Azure AD.  

## <a name="use-service-bus-with-an-azure-ad-domain-user-account"></a>Использование служебной шины с помощью учетной записи пользователя домена в Azure AD

В следующем разделе описаны шаги, необходимые для создания и запуска примера приложения, которое запрашивает вход в систему интерактивного пользователя Azure AD. В этом разделе также содержатся сведения о том, как предоставить пользователю доступ к служебной шине с помощью учетной записи пользователя и как получить доступ к концентраторам событий с помощью удостоверения. 

В этом введении описывается простое консольное приложение, [код для которого находится на сайте Github](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/RoleBasedAccessControl).

### <a name="create-an-active-directory-user-account"></a>Создание учетной записи пользователя Active Directory

Первый шаг не является обязательным. Каждая подписка Azure автоматически связана с клиентом Azure Active Directory. Если у вас есть к ней доступ, то она уже зарегистрирована. Это означает, что вы можете использовать имеющуюся учетную запись. 

Если вам все же нужно создать учетную запись для этого сценария, [выполните следующие действия](../automation/automation-create-aduser-account.md). Необходимо наличие разрешения на создание учетных записей в клиенте Azure Active Directory. Этот вариант не подходит для крупных корпоративных сценариев.

### <a name="create-a-service-bus-namespace"></a>Создание пространства имен служебной шины

Далее [создайте пространство имен служебной шины, предназначенное для обмена сообщениями](service-bus-create-namespace-portal.md), в одном из регионов Azure с поддержкой предварительной версии для RBAC (**восточная часть США**, **восточная часть США 2** или **Западная Европа**). 

После создания пространства имен перейдите на страницу **Управление доступом (IAM)** на портале и щелкните **Add** (Добавить), чтобы добавить учетную запись пользователя Azure AD для роли владельца. Если вы используете собственную учетную запись пользователя и создали пространство имен, вам уже назначена роль владельца. Чтобы добавить другую учетную запись в роль, найдите название веб-приложения на панели **Добавление разрешений** в поле **Select** (Выбор), а затем щелкните запись. Нажмите кнопку **Сохранить**.

![](./media/service-bus-role-based-access-control/rbac1.PNG)

Теперь учетная запись имеет доступ к пространству имен служебной шины и к ранее созданной очереди.
 
### <a name="register-the-application"></a>Регистрация приложения

Перед запуском примера приложения зарегистрируйте его в Azure AD и утвердите запрос на продолжение, позволяющий приложению получить доступ к служебной шине Azure от его имени. 

Так как образец приложения является консольным, необходимо зарегистрировать собственное приложение и добавить разрешения API для **Microsoft.ServiceBus** в набор необходимых разрешений. Собственным приложениям также необходим **URI перенаправления** в Azure AD, выступающий в качестве идентификатора. URI не должен быть назначением сети. В этом примере используйте `http://servicebus.microsoft.com`, так как в образце кода уже используется этот URI.

Действия для регистрации подробно описаны в [этом руководстве](../active-directory/develop/active-directory-integrating-applications.md). Выполните следующие действия, чтобы зарегистрировать **собственное** приложение, а затем следуйте инструкциям по обновлению, чтобы добавить API **Microsoft.ServiceBus** для необходимых разрешений. Выполняя эти действия, запишите значения **TenantId** и **ApplicationId**, так как они необходимы для запуска приложения.

### <a name="run-the-app"></a>Запуск приложения

Перед запуском примера измените файл App.config и в зависимости от вашего сценария установите следующие значения.

- `tenantId`. Задайте значение **TenantId**.
- `clientId`. Задайте значение **ApplicationId**. 
- `clientSecret`. Если вы хотите войти в систему с помощью секрета клиента, создайте его в Azure AD. Кроме того, используйте веб-приложение или API вместо собственного приложения. Добавьте приложение на страницу **Управление доступом (IAM)** в созданном ранее пространстве имен.
- `serviceBusNamespaceFQDN`. Укажите полное DNS-имя созданного пространства имен служебной шины (например, `example.servicebus.windows.net`).
- `queueName`. Укажите имя созданной очереди.
- URI перенаправления, указанный в приложении на предыдущих шагах.
 
При запуске консольного приложения вам будет предложено выбрать сценарий. Щелкните **Interactive User Login** (Вход в систему с учетными данными текущего пользователя), указав его номер и нажав клавишу ВВОД. В приложении отображается окно входа в систему, оно запрашивает ваше согласие на доступ к служебной шине, а затем использует службу для запуска сценария отправки или получения с помощью удостоверения для входа.

## <a name="next-steps"></a>Дополнительная информация

Дополнительную информацию об обмене сообщениями через служебную шину см. в следующих разделах.

* [Базовая информация о Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Очереди, разделы и подписки служебной шины](service-bus-queues-topics-subscriptions.md)
* [Начало работы с очередями служебной шины](service-bus-dotnet-get-started-with-queues.md)
* [Как использовать разделы и подписки служебной шины](service-bus-dotnet-how-to-use-topics-subscriptions.md)