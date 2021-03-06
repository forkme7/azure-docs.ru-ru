---
title: включение файла
description: включение файла
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: b4b06119b9d46781b967fc8d98808c60d2b41ccb
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2018
---
Вам нужно, чтобы у пользователей вашей организации был необходимый уровень доступа к этим ресурсам. Вы не хотите предоставлять пользователям неограниченный доступ, но при этом требуется обеспечить им возможность работать. Управление доступом на основе ролей (RBAC) позволяет предоставлять пользователям разрешения на выполнение определенных действий в той или иной области. Роль определяет набор разрешенных действий. Вы назначаете ее для области и определяете, какие пользователи принадлежат к этой роли для области.

Когда вы разрабатываете стратегию управления доступом, предоставьте пользователям минимальные разрешения, чтобы они могли выполнять свои задачи. На следующем изображении представлен рекомендуемый шаблон для назначения ролей:

![Область](./media/resource-manager-governance-rbac/role-examples.png)

Есть три основные роли, которые применяются ко всем типам ресурсов: владелец, участник и читатель. Все учетные записи, которым назначена роль владельца, нужно строго контролировать и редко использовать. Пользователю, который должен только наблюдать за состоянием решений, нужно предоставить роль читателя.

Большинству пользователей предоставляются [роли, относящиеся к конкретному ресурсу,](../articles/role-based-access-control/built-in-roles.md) или [пользовательские роли](../articles/role-based-access-control/custom-roles.md) на уровне подписки или группы ресурсов. Эти роли строго определяют разрешенные действия. Назначая эти роли пользователям, вы предоставляете им необходимый уровень доступа без лишних прав на управление. Вы можете назначить учетной записи несколько ролей, и пользователь получит их объединенные разрешения. Как правило, при предоставлении доступа на уровне ресурсов разрешения пользователей слишком ограничены. Но такой способ подходит для автоматизированного процесса, разработанного для конкретной задачи.

### <a name="who-can-assign-roles"></a>Кто может назначать роли

Для создания и удаления назначений ролей пользователи должны иметь доступ `Microsoft.Authorization/roleAssignments/*`. Такой доступ предоставляется с помощью ролей владельца или администратора доступа пользователей.