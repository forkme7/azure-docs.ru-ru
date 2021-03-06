---
title: Общие сведения об удостоверениях Azure Stack | Документация Майкрософт
description: Сведения о системах идентификации, которые можно использовать с помощью Azure Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/22/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 607c7938a789b3504a425057645b291bd4c8235b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2018
---
# <a name="overview-of-identity-for-azure-stack"></a>Общие сведения об удостоверениях Azure Stack

Для работы Azure Stack требуется Azure Active Directory (Azure AD) или службы федерации Active Directory (AD FS) на базе Active Directory в качестве поставщика удостоверений. Решение о выборе поставщика принимается один раз во время развертывания Azure Stack. Основные понятия и сведения об авторизации из этой статьи могут помочь вам выбрать поставщика удостоверений. 

Выбор между Azure AD или AD FS может определяться режимом, в котором вы развернули Azure Stack: 
- Если вы выполнили развертывание в режиме с подключением, можно использовать Azure AD или AD FS. 
- Если вы выполнили развертывание в отключенном режиме без подключения к Интернету, поддерживается только AD FS.

В зависимости от среды Azure Stack дополнительные сведения о параметрах смотрите в следующих статьях:
- Если вы используете комплект развертывания Azure Stack, ознакомьтесь с разделом [Вопросы относительно удостоверений](azure-stack-datacenter-integration.md#identity-considerations).
- Если вы используете интегрированные системы Azure Stack, ознакомьтесь со статьей [Модели подключения интегрированных систем Azure Stack](azure-stack-deployment-decisions.md).

 
## <a name="common-concepts-for-identity"></a>Основные понятия об удостоверении
В следующих разделах рассматриваются основные понятия, связанные с поставщиками удостоверений, и сведения об их использовании в Azure Stack.

![Терминология для поставщиков удостоверений](media/azure-stack-identity-overview/terminology.png)

### <a name="directory-tenants-and-organizations"></a>Организации и клиенты каталога
Каталог — это контейнер, который содержит информацию о *пользователях*, *приложениях*, *группах* и *субъектах-служб*.
 
Клиент каталога — это *организация*, например, корпорация Майкрософт или ваша компания. 
- Azure AD поддерживает нескольких клиентов и может поддерживать несколько организаций (каждую в собственном каталоге). Если вы используете Azure AD и имеете несколько клиентов, можно предоставить приложениям и пользователям из одного клиента доступ к другим клиентам того же каталога.
- AD FS поддерживает только один клиент, и, следовательно, только одну организацию. 

### <a name="users-and-groups"></a>Пользователи и группы
Учетные записи пользователей (удостоверения) — это стандартные учетные записи, выполняющие проверку подлинности отдельных пользователей с помощью идентификатора и пароля пользователя. Группы могут содержать пользователей или другие группы. 

Способ создания пользователей и групп и управление ими зависит от используемого вами решения для идентификации. 

В Azure Stack учетные записи пользователей имеют следующие свойства: 
- Создаются в формате *username@domain*. Хотя AD FS сопоставляет учетные записи пользователей с экземпляром Active Directory, она не поддерживает использование формата *\<домен>\<псевдоним>*. 
- Настраиваются для использования многофакторной проверки подлинности. 
- Ограничены каталогом, в котором они были впервые зарегистрированы. Это каталог организации.
- Их можно импортировать из локальных каталогов. Дополнительные сведения см. в статье [Интеграция локальных каталогов с Azure Active Directory](/azure/active-directory/connect/active-directory-aadconnect). 

При входе на клиентский портал организации используется URL-адрес *https://portal.local.azurestack.external*. 

### <a name="guest-users"></a>Гостевые пользователи
Гостевые пользователи — это учетные записи пользователей из других клиентов каталога, которым предоставили доступ к ресурсам в вашем каталоге. Для поддержки гостевых пользователей используйте Azure AD и включите поддержку мультитенантного режима. Если поддержка включена, вы можете предоставить гостевому пользователю доступ к ресурсам в вашем клиенте каталога. Это в свою очередь позволяет активировать совместную работу за пределами организации. 

Чтобы пригласить гостевых пользователей, операторы и пользователи облака могут использовать [службу совместной работы Azure AD B2B](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Приглашенные пользователи получают доступ к документам, ресурсам и приложениям из вашего каталога, сохраняя при этом контроль над собственными ресурсами и данными. 

В качестве гостевого пользователя вы можете войти в другой клиент каталога организации. Для этого добавьте имя каталога организации в URL-адрес портала. Например, если вы принадлежите к организации Contoso и хотите войти в каталог компании Fabrikam, используйте https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.

### <a name="applications"></a>ПРИЛОЖЕНИЯ
Вы можете зарегистрировать приложения в Azure AD или AD FS, а затем предложить приложения пользователям в организации. 

Приложения включают:
- **Веб-приложение.** В качестве примера можно привести портал Azure и Azure Resource Manager. Они поддерживают вызовы веб-API. 
- **Собственный клиент.** В качестве примера можно привести Azure PowerShell, Visual Studio и Azure CLI.

Приложения могут поддерживать два типа клиентской модели: 
- **Один клиент.** Поддерживает пользователей и службы только из того же каталога, в котором зарегистрировано приложение. 

  > [!NOTE]    
  > Так как AD FS поддерживает только один каталог, приложения, созданные в топологии AD FS являются по своей природе приложениями с одним клиентом.

- **Несколько клиентов.** Поддерживает использование приложения пользователями и службами из обоих каталогов (каталога, в котором зарегистрировано приложение, а также из дополнительных каталогов клиента). С помощью мультитенантных приложений пользователи из другого каталога клиента (другого клиента Azure AD) могут войти в ваше приложение. 

  Дополнительные сведения о мультитенантности см. в статье [Поддержка мультитенантности в Azure Stack](azure-stack-enable-multitenancy.md). 

  Дополнительные сведения о разработке мультитенантного приложения см. в статье [Как реализовать вход любого пользователя Azure Active Directory (AD) с помощью шаблона мультитенантного приложения](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview).

При регистрации приложения создаются два объекта:

- **Объект приложения** — это глобальное представление приложения во всех клиентах. С программным приложением устанавливается отношение "один к одному", которое существует только в каталоге, в котором оно было впервые зарегистрировано.

- **Объект субъекта-службы** — это объект учетных данных, созданный для приложения в каталоге, в котором впервые зарегистрировано приложение. Субъект-служба также создается в каталоге каждого дополнительного клиента, в которых используется это приложение. С программным приложением может устанавливаться отношение "один ко многим". 

Дополнительные сведения об объектах приложения и субъектах-службы см. в статье [Объекты приложения и субъекта-службы в Azure Active Directory (Azure AD)](/azure/active-directory/develop/active-directory-application-objects). 

### <a name="service-principals"></a>Субъекты-службы 
Субъект-служба — это набор *учетных данных* для приложения или службы, которые предоставляют доступ к ресурсам в Azure Stack. Использование субъекта-службы разделяет разрешения приложения от разрешений пользователя приложения.

Субъект-служба создается в каждом клиенте, в котором используется приложение. Она создает удостоверения для входа и доступа к ресурсам (например, пользователям), защищенных этим клиентом. 

- Приложение с одним клиентом имеет только один субъект-службу в каталоге, в котором оно впервые создано. Субъект-служба создается и дает согласие быть использованной во время регистрации приложения. 
- Для мультитенантного веб-приложения или API есть субъект-служба, которая будет создана в каждом клиенте, пользователь которого даст согласие на его использование. 

В качестве учетных данных для субъектов-служб могут использоваться ключ, который был создан на портале Azure, или сертификат. Использование сертификата хорошо подходит для автоматизации, так как сертификаты считаются более безопасными, чем ключи. 


> [!NOTE]    
> При использовании AD FS с Azure Stack только администраторы могут создавать субъекты-службы. При использовании AD FS для субъектов-служб требуются сертификаты. В таком случае субъекты-службы создаются с помощью привилегированной конечной точки. Дополнительные сведения см. в статье [Предоставление приложениям доступа к Azure Stack](azure-stack-create-service-principals.md).

Дополнительные сведения о субъектах-службах для Azure Stack см. в статье [Предоставление приложениям доступа к Azure Stack](azure-stack-create-service-principals.md).


### <a name="services"></a>Службы
Службы в Azure Stack, взаимодействующие с поставщиками удостоверений, регистрируются как приложения с поставщиком удостоверений. Как и в случае с приложениями, регистрация позволяет службе выполнить проверку подлинности с помощью системы идентификации. 

Все службы Azure используют протоколы [OpenID Connect](/azure/active-directory/develop/active-directory-protocols-openid-connect-code) и [JSON Web Tokens](/azure/active-directory/develop/active-directory-token-and-claims) для идентификации. Благодаря согласованному использованию протоколов службами Azure AD и AD FS вы можете использовать [библиотеку проверки подлинности Azure Active Directory](/azure/active-directory/develop/active-directory-authentication-libraries) ADAL, чтобы выполнить проверку подлинности в локальной среде или в Azure (с подключением к Интернету). С помощью ADAL также можно использовать такие средства, как Azure PowerShell и Azure CLI, для управления ресурсами в локальной среде и в нескольких облаках. 


### <a name="identities-and-your-identity-system"></a>Удостоверения и система идентификации 
К удостоверениям для Azure Stack относятся учетные записи пользователей, группы и субъекты-службы. 

При установке Azure Stack несколько встроенных приложений и служб автоматически регистрируются с помощью поставщика удостоверений в клиенте каталога. Некоторые регистрируемые службы используются для администрирования. Другие службы доступны для пользователей. При стандартной регистрации основные службы получают удостоверения, которые могут взаимодействовать между собой и с удостоверениями, которые будут добавлены позже.

Если вы настроили Azure AD с поддержкой мультитенантности, некоторые приложения распространяются в новые каталоги. 

## <a name="authentication-and-authorization"></a>Аутентификация и авторизация
 

### <a name="authentication-by-applications-and-users"></a>Проверка подлинности, выполняемая приложениями и пользователями
  
![Удостоверения между слоями Azure Stack](media/azure-stack-identity-overview/identity-layers.png)

Для приложений и пользователей архитектура Azure Stack описывается четырьмя слоями. При взаимодействии между каждым из этих слоев могут использоваться разные типы проверки подлинности.


|Слой    |Проверка подлинности между слоями  |
|---------|---------|
|Средства и клиенты, например портал администрирования     | Чтобы получить доступ к ресурсам в Azure Stack или изменить их, средства и клиенты используют [JSON Web Token](/azure/active-directory/develop/active-directory-token-and-claims) для вызова Azure Resource Manager. <br>Azure Resource Manager проверяет JSON Web Token и просматривает *утверждения* в выданном маркере, чтобы оценить уровень авторизации пользователя или субъекта-службы в Azure Stack. |
|Платформа Azure Resource Manager и ее основные службы     |Azure Resource Manager взаимодействует с поставщиками ресурсов для передачи данных от пользователей. <br> Для передачи данных используются *прямые императивные* вызовы или *декларативные* вызовы через [шаблоны Azure Resource Manager](/azure/azure-stack/user/azure-stack-arm-templates.md).|
|Поставщики ресурсов     |Вызовы, которые передаются поставщикам ресурсов, защищены проверкой подлинности на основе сертификатов. <br>Azure Resource Manager и поставщик удостоверений затем взаимодействуют с помощью API. Каждый вызов, полученный из Azure Resource Manager, поставщик удостоверений проверяет с помощью сертификата.|
|Инфраструктура и бизнес-логика     |Поставщики ресурсов взаимодействуют с бизнес-логикой и инфраструктурой с помощью режима проверки подлинности на свой выбор. Стандартные поставщики удостоверений, которые поставляются с Azure Stack, используют проверку подлинности Windows для защиты этой связи.|

![Сведения, необходимые для проверки подлинности](media/azure-stack-identity-overview/authentication.png)


### <a name="authenticate-to-azure-resource-manager"></a>Проверка подлинности в Azure Resource Manager
Чтобы выполнить проверку подлинности с помощью поставщика удостоверений и получить JSON Web Token, необходимо иметь следующие сведения: 
1.  **URL-адрес для системы идентификации (центр).** URL-адрес, по которому можно связаться с поставщиком удостоверений. (например, *https://login.windows.net*). 
2.  **URI идентификатора приложения для Azure Resource Manager.** Уникальный идентификатор для Azure Resource Manager, зарегистрированный с помощью поставщика удостоверений. Он уникальный для каждой установки Azure Stack.
3.  **Учетные данные.** Учетные данные, которые вы используете для выполнения проверки подлинности с помощью поставщика удостоверений. 
4.  **URL-адрес для Azure Resource Manager.** URL-адрес — это расположение службы Azure Resource Manager. Например, *https://management.azure.com* или *https://management.local.azurestack.external*.

Когда субъект (клиент, приложение или пользователь) выполняет запрос на проверку подлинности для доступа к ресурсу, запрос должен содержать следующие данные:
- Учетные данные субъекта.
- URI идентификатора приложения для ресурса, к которому субъект хочет получить доступ. 

Учетные данные проверяются поставщиком удостоверений. Поставщик удостоверений также проверяет, чтобы URI идентификатора приложения использовался для зарегистрированного приложения и чтобы субъект имел правильные разрешения для получения маркера для ресурса. Если запрос допустимый, предоставляется JSON Web Token. 

Затем маркер передается в заголовке запроса к Azure Resource Manager. Azure Resource Manager в любом порядке выполняет следующее:
- Проверяет утверждение *издателя*, чтобы убедиться, что маркер получен из правильного поставщика удостоверений. 
- Проверяет утверждение *аудитории*, чтобы убедиться, что маркер был выдан Azure Resource Manager. 
- Проверяет, что JSON Web Token подписан с помощью сертификата, настроенного через OpenID, известного для Azure Resource Manager. 
- Просматривает утверждения *времени выдачи* и *окончания срока действия*, чтобы убедиться, что маркер активный и может быть принят. 

После выполнения всех проверок Azure Resource Manager использует утверждения *объектов* и *групп*, чтобы создать список ресурсов, к которым может получить доступ субъект. 

![Схема протокола обмена маркерами](media/azure-stack-identity-overview/token-exchange.png)


### <a name="use-role-based-access-control"></a>Использование контроля доступа на основе ролей  
Управление доступом на основе ролей (RBAC) в Azure Stack согласуется с реализацией в Microsoft Azure. Вы можете управлять доступом к ресурсам путем назначения соответствующей роли RBAC пользователям, группам и приложениям. Для получения дополнительных сведений об использовании RBAC с помощью Azure Stack, ознакомьтесь со следующими статьями:
- [Начало работы с управлением доступом на основе ролей на портале Azure](/azure/role-based-access-control/overview).
- [Использование управления доступом на основе ролей для контроля доступа к ресурсам в подписке Azure](/azure/role-based-access-control/role-assignments-portal).
- [Создание пользовательских ролей для управления доступом на основе ролей в Azure](/azure/role-based-access-control/custom-roles).
- [Управление доступом на основе ролей](azure-stack-manage-permissions.md) в Azure Stack.


### <a name="authenticate-with-azure-powershell"></a>Проверка подлинности с помощью Azure PowerShell  
Сведения об использовании Azure PowerShell для выполнения проверки подлинности с помощью Azure Stack см. в статье [Настройка пользовательской среды PowerShell в Azure Stack](azure-stack-powershell-configure-user.md).

### <a name="authenticate-with-azure-cli"></a>Проверка подлинности с помощью Azure CLI
Сведения об использовании Azure PowerShell для выполнения проверки подлинности с помощью Azure Stack см. в статье [Установка и настройка интерфейса командной строки для работы с Azure Stack](/azure/azure-stack/user/azure-stack-connect-cli.md).

## <a name="next-steps"></a>Дополнительная информация
- [Архитектура удостоверения Azure Stack](azure-stack-identity-architecture.md)   
- [Интеграция центра обработки данных Azure Stack: идентификация](azure-stack-integrate-identity.md)




