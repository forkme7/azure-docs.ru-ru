---
title: Обзор функции управления доступом на основе ролей для ресурса в Azure PIM | Документация Майкрософт
description: Общие сведения о функции RBAC в PIM, включая терминологию и уведомления
services: active-directory
documentationcenter: ''
author: barclayn
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: billmath
ms.openlocfilehash: edf22ea0cfe60cb734b4339363d50af050466000
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2018
---
# <a name="pim-for-azure-resources"></a>PIM для ресурсов Azure

С помощью службы Azure Active Directory Privileged Identity Management (PIM) можно администрировать, контролировать и отслеживать доступ к ресурсам Azure в пределах организации. Это включает в себя подписки, группы ресурсов и даже виртуальные машины. Любой ресурс на портале Azure, который использует функции RBAC Azure, может воспользоваться всеми преимуществами управления безопасностью и жизненным циклом, которые может предложить Azure AD PIM, а также некоторыми новыми функциями, которые мы планируем скоро реализовать в ролях Azure AD. 

## <a name="pim-for-azure-resources-helps-resource-administrators"></a>PIM для ресурсов Azure помогает администраторам ресурсов

- Узнать, каким пользователям и группам назначены роли администрируемых ресурсов Azure.
- Включать доступ по требованию, JIT-доступ для управления ресурсами, такими как подписки, группы ресурсов и т. д.
- Управлять окончанием срока доступа к ресурсам пользователей или групп автоматически с новыми параметрами назначения времени.
- Назначать временный доступ к ресурсам для быстрых или срочных задач.
- Принудительно применять многофакторную проверку подлинности для доступа к ресурсам для любой стандартной или пользовательской роли. 
- Получать отчеты о действиях с ресурсами, связанными с получением доступа к ним, в ходе активного сеанса пользователя.
- Получать оповещения, когда новым пользователям или группам назначается доступ к ресурсам и когда они активируют подходящие назначения.

Azure AD PIM может управлять встроенными ролями ресурсов Azure, а также настраиваемыми (RBAC) ролями, включая (но не ограничиваясь ими):

- Владелец.
- Администратор доступа пользователей
- участник;
- администратор безопасности;
- Диспетчер безопасности и другие.

>[!NOTE]
Пользователи или члены группы, которым назначены роли владельца или администратора доступа пользователей, и глобальные администраторы, которые поддерживают управление подпиской в ​​Azure AD, являются администраторами ресурсов. Эти администраторы могут назначать роли, настраивать параметры роли и проверять доступ с помощью PIM для ресурсов Azure. Просмотрите список [встроенных ролей ресурсов Azure](../../role-based-access-control/built-in-roles.md).

## <a name="tasks"></a>Задачи

Служба PIM обеспечивает удобный доступ к активации ролей, просмотру ожидающих активаций и запросов, ожидающих утверждений (для [ролей каталога Azure AD](azure-ad-pim-approval-workflow.md)) и проверок, ожидающих ответа, в разделе "Задачи" в левом навигационном меню.

При доступе к любому из пунктов меню "Задачи" из точки входа "Обзор" результирующее представление содержит результаты как для ролей каталогов Azure AD, так и ролей ресурсов Azure. 

![](media/azure-pim-resource-rbac/role-settings-details.png)

Раздел "Мои роли" содержит список активных и допустимых назначений ролей для каталога Azure AD и ресурсов Azure.

## <a name="activate-roles"></a>Активация ролей

Активация ролей для ресурсов Azure предоставляет новое взаимодействие с пользователем, которое позволит участникам, имеющим право на участие, назначать активацию на будущую дату или время и выбирать конкретную продолжительность активации в пределах максимума (заданного администраторами). Дополнительные сведения см. в статье [Как активировать и деактивировать роли в компоненте управления привилегированными пользователями Azure AD](../active-directory-privileged-identity-management-how-to-activate-role.md).

![](media/azure-pim-resource-rbac/contributor.png)

В меню активаций введите нужные дату и время начала для активации роли. При необходимости уменьшите продолжительность активации (время, в течение которого роль активна) и введите обоснование, если это необходимо, а затем щелкните "Активировать".

Если дата и время начала не изменены, роль будет активирована в течение нескольких секунд. На странице "Мои роли" вы увидите сообщение о роли, помещенной в очередь для активации. Нажмите кнопку "Обновить", чтобы это сообщение исчезло.

![](media/azure-pim-resource-rbac/my-roles.png)

Если активация запланирована на будущую дату, ожидающий запрос появится на вкладке "Ожидающие запросы" в левом навигационном меню. Если активация роли больше не требуется, пользователь может отменить запрос, нажав кнопку "Отмена" в правой части страницы.

![](media/azure-pim-resource-rbac/pending-requests.png)

## <a name="discover-and-manage-azure-resources"></a>Поиск ресурсов Azure и управление ими

Чтобы найти и администрировать роли ресурса Azure, выберите ресурс Azure на вкладке "Управление" в левом меню навигации. Используйте фильтры или панель поиска в верхней части страницы для поиска ресурса.

![](media/azure-pim-resource-rbac/azure-resources.png)

## <a name="resource-dashboards"></a>Информационные панели ресурсов

Информационная панель представления администратора содержит четыре основных компонента. Графическое представление активаций ролей ресурсов за последние семь дней. Эти данные привязаны к выбранному ресурсу и отображают активацию для наиболее распространенных ролей (владелец, участник, администратор доступа пользователей) и всех объединенных ролей.

Справа от графика активации представлены две диаграммы, отображающие распределение назначений ролей по типу назначения для пользователей и групп. При выборе фрагмента диаграммы изменяется значение в процентах (или наоборот).

![](media/azure-pim-resource-rbac/admin-view.png)

Под диаграммами отображается количество пользователей и групп с новыми назначениями ролей за последние 30 дней (слева) и список ролей, отсортированных по общему числу назначений (по убыванию).

![](media/azure-pim-resource-rbac/role-settings.png)

## <a name="manage-role-assignments"></a>Управление назначением ролей

Администраторы могут управлять назначениями ролей, выбирая либо вкладку "Роли", либо "Участники" на левой панели навигации. Выбор ролей позволяет администраторам ограничить свои задачи управления определенной ролью, тогда как на вкладке "Участники" отображаются все назначения ролей пользователя и группы для ресурса.

![](media/azure-pim-resource-rbac/roles.png)

![](media/azure-pim-resource-rbac/members.png)

>[!NOTE]
Если у вас есть роль, ожидающая активации, баннер с уведомлением отображается в верхней части страницы при просмотре членства.

## <a name="assign-roles"></a>Назначение ролей

Чтобы назначить пользователю или группе роль, выберите роль (в представлении "Роли") или нажмите "Добавить" на панели действий (в представлении "Участники").

![](media/azure-pim-resource-rbac/members2.png)

>[!NOTE]
Если вы добавляете пользователя или группу на вкладке "Участники", вам нужно будет выбрать роль из меню "Добавить", прежде чем вы сможете выбрать пользователя или группу.

![](media/azure-pim-resource-rbac/select-role.png)

Выберите пользователя или группу из каталога.

![](media/azure-pim-resource-rbac/choose.png)

В раскрывающемся меню выберите соответствующий тип назначения. 

**JIT-назначение.** Предоставляет пользователю или участникам группы допустимый, но не постоянный доступ к роли в течение определенного периода времени или не ограничено (если настроено в параметрах ролей). 

**Прямое назначение.** Пользователю или членам группы не требуется активировать назначение роли (известное как постоянный доступ). Корпорация Майкрософт рекомендует использовать прямое назначение для краткосрочного использования, например смены по требованию или срочных действий, когда доступ не потребуется после завершении задачи.

![](media/azure-pim-resource-rbac/membership-settings.png)

Флажок под раскрывающимся списком типа назначения позволяет указать, должно ли назначение быть постоянным (на постоянной основе можно активировать JIT-назначение или постоянно действующее для прямого назначения). Чтобы указать конкретную продолжительность назначения, снимите флажок и измените поля даты и времени начала и окончания.

>[!NOTE]
Флажок может быть неактивным, если другой администратор указал максимальную продолжительность назначения для каждого типа назначения в параметрах роли.

![](media/azure-pim-resource-rbac/calendar.png)

## <a name="view-activation-and-azure-resource-activity"></a>Просмотр активации и активность ресурсов Azure

В случае, если вам нужно выяснить, какие действия выполняет конкретный пользователь с разными ресурсами, вы можете просмотреть активность ресурсов Azure, связанную с данным периодом активации (для допустимых пользователей). Начните с выбора пользователя из представления "Участники" или списка участников в определенной роли. Отобразится графическое представление действий пользователя в ресурсах Azure по датам и недавние активации роли за тот же период времени.

![](media/azure-pim-resource-rbac/user-details.png)

При выборе активации определенной роли будут показаны детали активации роли и соответствующая активность с ресурсами Azure, которая произошла, когда этот пользователь был активен.

![](media/azure-pim-resource-rbac/audits.png)

## <a name="modify-existing-assignments"></a>Изменение существующих назначений

Чтобы изменить существующие назначения из подробного представления пользователя или группы, выберите параметр Change Settings (Изменить параметры) на панели действий в верхней части страницы. Измените тип назначения на JIT-назначение или прямое назначение.

![](media/azure-pim-resource-rbac/change-settings.png)

## <a name="review-who-has-access-in-a-subscription"></a>Просмотр пользователей с доступом в подписке

Чтобы просмотреть назначения ролей в своей подписке, выберите вкладку "Участники" в левой навигационной панели или выберите роли, а затем выберите определенную роль для просмотра участников. 

Выберите "Проверить" на панели действий, чтобы просмотреть существующие проверки доступа, и выберите "Добавить", чтобы создать проверку.

![](media/azure-pim-resource-rbac/owner.png)

[Дополнительные сведения о проверке доступа](../active-directory-privileged-identity-management-how-to-perform-security-review.md).

>[!NOTE]
Сейчас проверки поддерживаются только для типов ресурсов подписки.

## <a name="configure-role-settings"></a>Изменение параметров роли

Настройка параметров ролей определяет значения по умолчанию, применяемые к назначениям в среде PIM. Чтобы определить их для своего ресурса, выберите вкладку "Параметры роли" на левой панели навигации или нажмите кнопку "Параметры ролей" на панели действий в любой роли, чтобы просмотреть текущие параметры.

Щелкнув "Редактировать" на панели действий в верхней части страницы, можно изменить каждый параметр.

![](media/azure-pim-resource-rbac/owner.png)

![](media/azure-pim-resource-rbac/owner02.png)

Изменения в параметрах регистрируются на странице параметров ролей, включая последнее время обновления и имя администратора, который внес изменения.

![](media/azure-pim-resource-rbac/role-settings-02.png)

## <a name="resource-audit"></a>Аудит ресурсов

Аудит ресурсов дает вам представление о всей активности роли для этого ресурса. Вы можете отфильтровать информацию с помощью предопределенной даты или настраиваемого диапазона.
![](media/azure-pim-resource-rbac/last-day.png) Аудит ресурсов также обеспечивает быстрый доступ для просмотра подробностей действий пользователя. В представлении все действия Activate role (Активировать роль) являются ссылками на действия с ресурсом для конкретного запрашивающего.
![](media/azure-pim-resource-rbac/resource-audit.png)

## <a name="just-enough-administration"></a>Just Enough Administration (JEA)

Использовать рекомендации JEA с назначениями ресурсов легко с помощью PIM для ресурсов Azure. Пользователи и участники групп с назначениями в подписках Azure или группах ресурсов могут активировать существующее назначение ролей в уменьшенном объеме. 

На странице поиска найдите подчиненный ресурс, которым вам нужно управлять.

![](media/azure-pim-resource-rbac/azure-resources-02.png)

Выберите "Мои роли" в меню навигации слева и выберите соответствующую роль для активации. Обратите внимание, что типом назначения является "Унаследовано", так как роль была назначена в подписке, а не в группе ресурсов, как показано ниже.

![](media/azure-pim-resource-rbac/my-roles-02.png)

## <a name="next-steps"></a>Дополнительная информация

- [Встроенные роли управления доступом на основе ролей в Azure](../../role-based-access-control/built-in-roles.md)
- Дополнительные сведения см. в статье [Как активировать и деактивировать роли в компоненте управления привилегированными пользователями Azure AD](../active-directory-privileged-identity-management-how-to-activate-role.md)
- [Утверждения (предварительная версия)](azure-ad-pim-approval-workflow.md)
