---
title: "Добавление пользователя службы совместной работы Azure Active Directory B2B в роль | Документация Майкрософт"
description: "Добавление роли гостевого пользователя в Azure Active Directory"
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
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b788b003299ff11bcbdcb11a2c7a21ac5081d634
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2018
---
# <a name="grant-permissions-to-users-from-partner-organizations-in-your-azure-active-directory-tenant"></a>Предоставление пользователям разрешений от партнерских организаций в клиенте Azure Active Directory

Пользователи службы совместной работы Azure Active Directory (Azure AD) B2B добавляются в каталог как гостевые пользователи, а разрешения гостей в каталоге по умолчанию ограничены. Ваша организация может быть заинтересована в назначении некоторым гостевым пользователям более привилегированных ролей. Чтобы это сделать, вы можете добавлять гостевых пользователей в любые роли, исходя из потребностей своей организации.

## <a name="default-role"></a>Роль по умолчанию

![роль по умолчанию](./media/active-directory-b2b-add-guest-to-role/default-role.png)

## <a name="global-administrator-role"></a>Роль "Глобальный администратор"

![роль "Глобальный администратор"](./media/active-directory-b2b-add-guest-to-role/global-admin-role.png)

## <a name="limited-administrator-role"></a>Роль "Администратор с ограниченными правами"

![роль "Администратор с ограниченными правами"](./media/active-directory-b2b-add-guest-to-role/limited-admin-role.png)

## <a name="next-steps"></a>Дополнительная информация

Другие статьи о службе совместной работы Azure AD B2B:

* [Что такое служба совместной работы Azure AD B2B?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Свойства пользователя службы совместной работы Azure Active Directory B2B](active-directory-b2b-user-properties.md)
* [Делегирование приглашений для службы совместной работы Azure Active Directory B2B](active-directory-b2b-delegate-invitations.md)
* [Динамические группы и служба совместной работы Azure Active Directory B2B](active-directory-b2b-dynamic-groups.md)
* [Примеры кода и команд PowerShell для службы совместной работы Azure Active Directory B2B](active-directory-b2b-code-samples.md)
* [Настройка приложений SaaS для службы совместной работы B2B](active-directory-b2b-configure-saas-apps.md)
* [Основные сведения о токенах пользователей в службе совместной работы Azure Active Directory B2B](active-directory-b2b-user-token.md)
* [Сопоставление утверждений пользователя службы совместной работы B2B в Azure Active Directory](active-directory-b2b-claims-mapping.md)
* [Доступ внешних пользователей к Office 365](active-directory-b2b-o365-external-user.md)
* [Текущие ограничения службы совместной работы Azure Active Directory B2B](active-directory-b2b-current-limitations.md)
