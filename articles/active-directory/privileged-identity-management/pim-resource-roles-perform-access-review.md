---
title: Как выполнить проверку доступа в приложении "Управление привилегированными пользователями" для ресурсов Azure | Документация Майкрософт
description: Из этого документа вы узнаете, как выполнить проверку доступа в PIM для ресурсов Azure.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 4afb923058143dd1771043db8433aa3a65541bf7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-role---perform-access-review"></a>Управление привилегированными пользователями — роли ресурсов — выполнение проверки доступа
Приложение "Управление привилегированными пользователями" для ресурсов Azure упрощает управление привилегированным доступом к ресурсам в Azure для предприятий. 

Если вам назначена роль администратора, администратор привилегированных ролей вашей организации может попросить вас регулярно подтверждать, что эта роль по-прежнему требуется вам для работы. Вы можете получить сообщение электронной почты, содержащее ссылку для подтверждения, или перейти непосредственно на [портал Azure](https://portal.azure.com). Следуйте указаниям в этой статье, чтобы выполнить самостоятельную проверку назначенных вам ролей.

Если вы администратор привилегированных ролей и вас интересуют функции проверки доступа, дополнительные сведения см. в статье [Как запустить проверку доступа в управлении привилегированными пользователями Azure AD](pim-resource-roles-start-access-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Добавление приложения для управления привилегированными пользователями
Для выполнения проверки можно использовать приложение "Управление привилегированными пользователями" (PIM) Azure AD на [портале Azure](https://portal.azure.com/) .  Если приложение Azure AD PIM еще не установлено на портале, выполните следующие действия, чтобы приступить к работе.

1. Войдите на [портале Azure](https://portal.azure.com/).
2. Щелкните свое имя пользователя в правом верхнем углу портала Azure и выберите каталог, с которым будете работать.
3. Выберите **Все службы** и введите **Azure AD Privileged Identity Management** в текстовом поле "Фильтр".
4. Установите флажок **Закрепить на панели мониторинга** и нажмите кнопку **Создать**. Откроется приложение "Управление привилегированными пользователями".

## <a name="approve-or-deny-access"></a>Утверждение или отклонение доступа
При утверждении или отклонении доступа вы просто сообщаете проверяющему, используете ли вы еще эту роль. Чтобы оставаться в этой роли, выберите **Утвердить**, а когда доступ вам больше не требуется — **Запретить**. Состояние роли изменится лишь тогда, когда проверяющий применит результаты.
Выполните следующие действия, чтобы найти проверку доступа и завершить ее.
1. Перейдите к приложению "Управление привилегированными пользователями".
2. Выберите колонку "Проверка доступа".

![](media/azure-pim-resource-rbac/rbac-access-review-complete.png)

3. Выберите проверку, которую нужно выполнить. 
4. Выберите **Утвердить** или **Запретить**. Возможно, в текстовом поле **Указание причины** потребуется ввести причину своего решения.

![](media/azure-pim-resource-rbac/rbac-access-review-choice.png)
