---
title: Общие сведения о самостоятельном сбросе пароля в Azure AD | Документация Майкрософт
description: Чем может быть полезен самостоятельный сброс пароля Azure AD для вашей организации?
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: e084db41cd199a9609e3edaf8b427a85ab2366b4
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2018
---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>Самостоятельный сброс пароля в Azure AD для ИТ-специалистов

Используя самостоятельный сброс пароля (SSPR) Azure Active Directory (Azure AD), пользователи смогут сами сбросить свои пароли, если в этом возникнет необходимость. В то же время администраторы могут управлять сбросом пароля пользователями. Пользователям больше не требуется обращаться в службу поддержки, чтобы сбросить пароль. SSPR Azure AD включает в себя следующие возможности:

* **Самостоятельная смена пароля**. Пользователь знает пароль, но хочет изменить его на новый.
* **Самостоятельный сброс пароля**. Пользователю не удается выполнить вход, и он хочет сбросить пароль, используя один или несколько из приведенных ниже проверенных методов аутентификации:
   * отправка текстового сообщения на проверенный мобильный телефон;
   * звонок на проверенный мобильный или рабочий телефон;
   * отправка сообщения в проверенную учетную запись электронной почты;
   * ответы на свои контрольные вопросы.
* **Самостоятельное разблокирование учетной записи**. Пользователю не удается выполнить вход с паролем, и его учетная запись блокируется. Пользователю необходимо разблокировать свою учетную запись без вмешательства администратора, воспользовавшись настроенными методами аутентификации.

## <a name="why-choose-azure-ad-sspr"></a>Преимущества SSPR Azure AD

SSPR Azure AD помогает достичь следующего:

* **Снижение затрат**. На сброс пароля обычно приходится 20 % обращений в службу технической поддержки ИТ-организации. 
* **Улучшение взаимодействия с пользователем** и **сокращение нагрузки на службу технической поддержки** за счет предоставления пользователям возможности устранять проблемы с их паролем. Им нет необходимости обращаться в службу технической поддержки или отправлять запрос в службу поддержки.
* **Поддержка мобильности.** Пользователи могут сбрасывать пароли независимо от своего местонахождения.
* **Контроль** на политикой безопасности. Администраторы могут по-прежнему применять уже имеющиеся политики.

Если вы готовы, то можете приступить к работе с SSPR Azure AD с помощью нашего [краткого руководства](quickstart-sspr.md). Вы можете быстро предоставить пользователям возможность сбрасывать свои пароли.

## <a name="azure-ad-sspr-availability"></a>Доступность SSPR Azure AD

В зависимости от подписки функция SSPR Azure AD доступна на трех уровнях:

* **Azure AD Free**. Администраторы облака могут сбрасывать свои пароли.
* **Azure AD Basic** или любая **платная подписка Office 365**. Пользователи облака могут сбрасывать свои пароли.
* **Azure AD Premium**. Любой пользователь или администратор, включая пользователей облака, федеративных пользователей и пользователей с синхронизацией паролей, может сбрасывать свои пароли. Для локальных паролей требуется включить компонент обратной записи паролей.

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Цены на Azure AD, соглашение об уровне обслуживания, обновления и планы

Более подробные сведения о лицензировании, ценах и планах на будущее можно найти на приведенных ниже сайтах:

* [Цены на Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Цены на Office 365](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [Соглашения об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/)
* [Соглашение об уровне обслуживания для Microsoft Online Services](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [Обновления Azure](https://azure.microsoft.com/updates/)
* [Стратегия развития Azure](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Дополнительная информация

* Вы готовы приступить к работе с SSPR? [Настройка самостоятельного сброса пароля Azure AD](quickstart-sspr.md).
* Спланируйте успешное развертывание SSPR для пользователей с помощью рекомендаций из нашего [руководства по развертыванию](howto-sspr-deployment.md).
* [Сброс или изменение пароля](../active-directory-passwords-update-your-own-password.md)
* [Регистрация для самостоятельного сброса пароля](../active-directory-passwords-reset-register.md).
