---
title: "Добавление пользователей службы совместной работы B2B в Azure Active Directory без приглашения | Документация Майкрософт"
description: "Вы можете разрешить гостевому пользователю добавлять других гостевых пользователей в Azure AD без активации приглашения в службу совместной работы Azure Active Directory B2B."
services: active-directory
documentationcenter: 
author: twooley
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 03/15/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 68d828b8fb23f7074be5fe691a4250d0fa653ab7
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2018
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a>Добавление гостевых пользователей службы совместной работы B2B без приглашения

Можно разрешить пользователю, например представителю партнера, добавлять пользователей из партнерской организации в вашу организацию без необходимости активировать приглашения. Все, что необходимо сделать, — предоставить этому пользователю привилегии на перечисление в каталоге, используемом для партнерской организации. 

Предоставляйте эти привилегии в следующих случаях.

1. Пользователь в главной организации (например, WoodGrove) приглашает одного пользователя из партнерской организации (например, Sam@litware.com) в качестве гостя.
2. Администратор главной организации настраивает политики, позволяющие Сэму (Sam) идентифицировать и добавлять других пользователей из партнерской организации (Litware).
3. Теперь Сэм может добавлять других пользователей из Litware в каталог, группы или приложения WoodGrove без отправки и активации приглашений. Если у Сэма есть соответствующие привилегии на перечисление в Litware, то это происходит автоматически.

### <a name="next-steps"></a>Дополнительная информация

Другие статьи о службе совместной работы Azure AD B2B:

* [Что такое служба совместной работы Azure AD B2B?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Как администраторы Azure Active Directory могут добавить пользователей службы совместной работы B2B?](active-directory-b2b-admin-add-users.md)
* [Как информационные работники могут добавить пользователей службы совместной работы B2B в Azure Active Directory?](active-directory-b2b-iw-add-users.md)
* [Элементы сообщения с приглашением в службу совместной работы B2B](active-directory-b2b-invitation-email.md)
* [Активация приглашения службы совместной работы B2B](active-directory-b2b-redemption-experience.md)
* [Руководство по лицензированию службы совместной работы Azure Active Directory B2B](active-directory-b2b-licensing.md)
* [Устранение неполадок службы совместной работы Azure Active Directory B2B](active-directory-b2b-troubleshooting.md)
* [Часто задаваемые вопросы о службе совместной работы Azure Active Directory B2B](active-directory-b2b-faq.md)
* [API службы совместной работы Azure Active Directory B2B и настройка](active-directory-b2b-api.md)
* [Многофакторная идентификация для пользователей службы совместной работы B2B](active-directory-b2b-mfa-instructions.md)
* [Указатель статьей по управлению приложениями в Azure Active Directory](active-directory-apps-index.md)