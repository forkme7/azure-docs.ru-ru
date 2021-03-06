---
title: Условия расположения в условном доступе Azure Active Directory | Документация Майкрософт
description: Узнайте, как использовать условие расположения для управления доступом к облачным приложениям на основе расположения пользователя в сети.
services: active-directory
keywords: условный доступ к приложениям, условный доступ посредством Azure Active Directory, безопасный доступ к ресурсам организации, политики условного доступа
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/17/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 687f3c4a5f70154b6335563d65443c12463b0b74
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/19/2018
---
# <a name="location-conditions-in-azure-active-directory-conditional-access"></a>Условия расположения в условном доступе Azure Active Directory 

С помощью [условного доступа Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) можно контролировать доступ авторизованных пользователей к облачным приложениям. Условие расположения политики условного доступа позволяет привязать параметры управления доступом к расположениям пользователей в сети.

В этой статье приведены сведения, необходимые для настройки условия расположения. 

## <a name="locations"></a>Расположения

Azure AD обеспечивает единый вход для устройств, приложений и служб из любой общедоступной точки в Интернете. Условие расположения позволяет управлять доступом к облачным приложениям на основе расположения пользователя в сети. Ниже приведены наиболее распространенные варианты использования условия расположения.

- Требование многофакторной проверки подлинности для пользователей, которые пытаются получить доступ к службе, находясь вне корпоративной сети.  

- Блокировка доступа для пользователей, которые пытаются получить доступ к службе из определенных стран или регионов. 

Расположение — это метка для расположения в сети, которая представляет собой именованное расположение или надежные IP-адреса для многофакторной проверки подлинности.


## <a name="named-locations"></a>Именованные расположения 

С помощью именованных расположений можно создать логические группирования диапазонов IP-адресов, стран и регионов. 

Доступ к именованным расположениям можно получить в разделе **Управление** страницы "Условный доступ".

![Расположения](./media/active-directory-conditional-access-locations/02.png)

 


Именованное расположение состоит из следующих компонентов.

![Расположения](./media/active-directory-conditional-access-locations/42.png)

- **Имя** — отображаемое имя именованного расположения.

- **Диапазоны IP-адресов** — один или не сколько диапазонов IP-адресов в формате CIDR.

- **Отметить как надежное расположение** — флажок, который можно установить для именованного расположения, чтобы указать надежное расположение. Как правило, надежные расположения являются областями сети, которые контролируются ИТ-отделом. Помимо условного доступа, надежные именованные расположения также используются в отчетах по защите идентификации Azure и безопасности Azure AD для уменьшения количества [ложных срабатываний](active-directory-reporting-risk-events.md#impossible-travel-to-atypical-locations-1).

- **Country / Regions** (Страны или регионы). Этот параметр позволяет выбрать одну или несколько стран или регионов для определения именованного расположения. 

- **Включить неизвестные области**. Некоторые IP-адреса не сопоставляются с определенной страной. Этот параметр позволяет указать, нужно ли включать эти IP-адреса в именованное расположение. Его можно установить, когда политика, использующая именованное расположение, должна применяться к неизвестным расположениям.

Количество именованных расположений, которые можно настроить, ограничено размером соответствующего объекта в Azure AD. Вы можете настроить:

- Именованное расположение с диапазонами IP-адресов до 1200.

- Максимум 90 именованных расположений, каждому из которых назначен один диапазон IP-адресов.




## <a name="trusted-ips"></a>Надежные IP-адреса

Вы также можете настроить диапазоны IP-адресов, представляющие локальную интрасеть в [параметрах службы многофакторной проверки подлинности](https://account.activedirectory.windowsazure.com/usermanagement/mfasettings.aspx). Эта функция позволяет настроить до 50 диапазонов IP-адресов. Диапазоны IP-адресов имеют формат CIDR. Дополнительные сведения см. в разделе [Надежные IP-адреса](authentication/howto-mfa-mfasettings.md#trusted-ips).  

Если настроены надежные IP-адреса, они отображаются в списке расположений для условия расположения как **надежные IP-адреса MFA**.   

### <a name="skipping-multi-factor-authentication"></a>Пропуск многофакторной проверки подлинности

На странице параметров службы многофакторной проверки подлинности можно определить пользователей корпоративной интрасети, установив флажок **Пропустить многофакторную проверку подлинности для запросов от федеративных пользователей из моей интрасети**. Этот параметр указывает, что внутреннее утверждение корпоративной сети, выдаваемое сервером служб федерации Active Directory (AD FS), должно быть надежным и использоваться для идентификации пользователя в корпоративной сети. Дополнительные сведения см. в разделе [Включение функции надежных IP-адресов с помощью условного доступа](authentication/howto-mfa-mfasettings.md#enable-the-trusted-ips-feature-by-using-conditional-access).

Если установить этот параметр, он, включая именованное расположение **надежных IP-адресов MFA**, будет применяться ко всем политикам, для которых установлен этот флажок.

Для мобильных и классических приложений с долгим временем существования сеансов периодически проводится повторная проверка условного доступа. По умолчанию — один раз в час. Если внутренне утверждение корпоративной сети выпускается только во время первоначальной проверки подлинности, Azure AD может не иметь списка диапазонов надежных IP-адресов. В этом случае трудно определить, находится ли по-прежнему пользователь в корпоративной сети.

1. Проверьте, входит ли IP-адрес пользователя в один из диапазонов надежных IP-адресов.

2. Проверьте, соответствуют ли первые три октета IP-адреса пользователя первым 3 октетам IP-адреса начальной проверки подлинности. IP-адрес сравнивается с адресом при начальной проверке подлинности, так как тогда было создано утверждение внутри корпоративной сети, а также произведена проверка расположения пользователя.

Если оба этапа завершатся ошибкой, считается, что пользователь больше не использует надежный IP-адрес.



## <a name="location-condition-configuration"></a>Настройка условия расположения

При настройке условия расположения вы можете выбрать:

- Любое расположение 
- Все надежные расположения
- Выбранные расположения

![Расположения](./media/active-directory-conditional-access-locations/01.png)

### <a name="any-location"></a>Любое расположение

По умолчанию выбор варианта **Любое местонахождение** приводит к тому, что политика применяется ко всем IP-адресам (то есть к любому адресу в Интернете). Этот параметр не ограничивается IP-адресами, настроенными как именованное расположение. Выбрав вариант **Любое местонахождение**, вы все равно можете исключить определенные расположения из политики. Например, можно применить политику ко всем расположениям (за исключением надежных расположений), чтобы задать для области все расположения (за исключением корпоративной сети).

### <a name="all-trusted-locations"></a>Все надежные расположения

Этот параметр применяется:

- ко всем расположениям, помеченным как надежное расположение;
- **надежным IP-адресам MFA** (если настроены).


### <a name="selected-locations"></a>Выбранные расположения

С помощью этого параметра можно выбрать одно или несколько именованных расположений. Для применения политики с этим параметром пользователю необходимо подключиться из любого из выбранных расположений. Если щелкнуть **Выбрать**, откроется элемент управления со списком именованных сетей для выбора. В списке также показано, является ли надежным расположение в сети. Именованное расположение **Надежные IP-адреса MFA** используется для включения параметров IP-адресов, которые можно настроить на странице параметров службы многофакторной проверки подлинности.

## <a name="what-you-should-know"></a>Необходимая информация

### <a name="when-is-a-location-evaluated"></a>Когда выполняется проверка расположения

Политики условного доступа оцениваются: 

- Изначально пользователь входит в веб-приложение, мобильное или классическое приложение. 

- Мобильное или классическое приложение, в котором применяется современная аутентификация, использует маркер обновления для получения нового маркера доступа. По умолчанию это происходит один раз в час. 

Это значит, что для мобильных и классических приложений, использующих современную аутентификацию, изменение сетевого расположения будет обнаружено в течение часа. Для мобильных и классических приложений, которые не используют современную аутентификацию, к каждому запросу на маркер применяется политика. Частота выполнения запроса зависит от приложения. Для веб-приложений политика также применяется во время начального входа и действует в течение времени существования сеанса веб-приложения. Так как в разных приложениях время сеанса будет разным, время между оценкам политики также может изменяться. Каждый раз, когда приложение запрашивает новый маркер входа, применяется политика.


По умолчанию Azure AD выдает токен на почасовой основе. После перемещения из корпоративной сети в течение часа политика применяется для приложений, использующих современную проверку подлинности.


### <a name="user-ip-address"></a>IP-адрес пользователя

При оценке политики используется общедоступный IP-адрес пользователя. Для устройств в частной сети используется не клиентский IP-адрес устройства пользователя в интрасети, а адрес, применяемый в сети для подключения к Интернету. 

### <a name="bulk-uploading-and-downloading-of-named-locations"></a>Массовое обновление и загрузка именованных расположений

При создании или обновлении именованных расположений для массовых обновлений можно передать или скачать CSV-файл с диапазонами IP-адресов. При передаче диапазоны IP-адресов заменяются адресами, указанными в файле. Каждая строка файла содержит один диапазон IP-адресов в формате CIDR. 


### <a name="cloud-proxies-and-vpns"></a>Облачные прокси-серверы и виртуальные частные сети (VPN) 

Если используется прокси-сервер, размещенный в облаке, или решение VPN, при оценке политики Azure AD использует IP-адрес прокси-сервера. Заголовок X-Forwarded-For (XFF), содержащий общедоступный IP-адрес пользователей, не используется, так как нет подтверждения того, что он поступает из надежного источника, а значит может быть поддельным. 

При наличии облачного прокси-сервера можно использовать политику, требующую присоединенного к домену устройства, или внутреннее утверждение корпоративной сети из AD FS.



### <a name="api-support-and-powershell"></a>Поддержка API и PowerShell 

API и PowerShell пока не поддерживаются для именованных расположений и политик условного доступа.





## <a name="next-steps"></a>Дополнительная информация

- Если вы хотите узнать, как настроить политику условного доступа, см. статью о том, как [начать работу с условным доступом в Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Если вы готовы к настройке политик условного доступа для своей среды, см. статью [Рекомендации по работе с условным доступом в Azure Active Directory](active-directory-conditional-access-best-practices.md). 
