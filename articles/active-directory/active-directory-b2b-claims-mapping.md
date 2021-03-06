---
title: Сопоставление утверждений пользователя службы совместной работы B2B в Azure Active Directory | Документация Майкрософт
description: Настройка утверждений пользователей, передаваемых в токене SAML для пользователей Azure Active Directory (Azure AD) B2B.
services: active-directory
documentationcenter: ''
author: twooley
manager: mtillman
editor: ''
tags: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/06/2018
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 8f5e471d4e7102300cd5581976b45c9fa8cc57bc
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>Сопоставление утверждений пользователя службы совместной работы B2B в Azure Active Directory

Azure Active Directory (Azure AD) позволяет настраивать утверждения, которые передаются в токене SAML для пользователей службы совместной работы B2B. Когда пользователь проходит аутентификацию в приложении, Azure AD выдает токен SAML для приложения. В токене собрана информация (утверждения), которая однозначно идентифицирует пользователя. По умолчанию здесь указаны имя пользователя, адрес электронной почты, имя и фамилия пользователя.

На [портале Azure](https://portal.azure.com) можно просмотреть или изменить утверждения, которые передаются приложению в токене SAML. Чтобы открыть параметры, последовательно выберите **Azure Active Directory** > **Корпоративные приложения** > приложения, для которых настроен единый вход > **Единый вход**. Параметры токена SAML отобразятся в разделе **Атрибуты пользователя**.

![Список атрибутов токена SAML в пользовательском интерфейсе](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

У вас могут быть две причины изменять утверждения, выдаваемые в токене SAML:

1. Возможно, приложение ожидает утверждения с другим набором URI и значениями.

2. Приложение ожидает, что утверждение NameIdentifier будет содержать данные, отличные от имени участника-пользователя, которое хранится в Azure Active Directory.

Дополнительные сведения о добавлении и изменении утверждений см. в статье [Настройка утверждений, выпущенных в токене SAML для корпоративных приложений в Azure Active Directory](develop/active-directory-saml-claims-customization.md).

По соображениям безопасности сопоставление NameID и имени участника-пользователя между клиентами для пользователей службы совместной работы B2B запрещено.

## <a name="next-steps"></a>Дополнительная информация

- Сведения о свойствах пользователя службы совместной работы B2B см. в статье [Свойства пользователя службы совместной работы Azure Active Directory B2B](active-directory-b2b-user-properties.md).
- Сведения о токенах пользователей службы совместной работы B2B см. в статье [Основные сведения о токенах пользователей в службе совместной работы Azure AD B2B](active-directory-b2b-user-token.md).

