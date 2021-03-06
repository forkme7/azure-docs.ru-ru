---
title: Предоставление или отзыв приглашений пользователям B2B из отдельных организаций с помощью Azure Active Directory | Документация Майкрософт
description: Эта статья содержит сведения о том, как администратор может настроить список разрешенных и запрещенных для предоставления или отзыва приглашений пользователям B2B из определенных доменов с помощью портала Azure или PowerShell.
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
ms.date: 04/12/2018
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 21862bb110801a43f13e3e65811e10726c188614
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Предоставление или отзыв приглашений пользователям B2B из отдельных организаций

С помощью списка разрешенных или запрещенных можно предоставить или отозвать приглашения пользователям B2B из отдельных организаций. Например, если вы хотите заблокировать домены личного адреса электронной почты, можно настроить список запрещенных, содержащий домены, такие как Gmail.com и Outlook.com. Или, если ваша организация сотрудничает с другими предприятиями, например Contoso.com, Fabrikam.com и Litware.com, и вы хотите предоставлять приглашения только им, добавьте Contoso.com, Fabrikam.com и Litware.com в список разрешенных.
  
> [!NOTE]
> Сейчас можно использовать только списки запрещенных. Возможность использовать списки разрешенных будет добавлена в ближайшее время.

## <a name="important-considerations"></a>Важные сведения

- Вы можете создать только список разрешенных или запрещенных. Настроить оба типа списков нельзя. По умолчанию все домены, которые не входят в список разрешенных, помещаются в список запрещенных, и наоборот. 
- Для организации можно создать только одну политику. Ее можно обновить, чтобы включить дополнительные домены. Кроме того, вы можете удалить политику, чтобы создать другую. 
- Этот список и список разрешенных и запрещенных OneDrive для бизнеса и SharePoint Online независимы друг от друга. Если вы хотите ограничить общий доступ к отдельным файлам в SharePoint Online, необходимо настроить список разрешенных или запрещенных для OneDrive для бизнеса и SharePoint Online. Дополнительные сведения см. в статье [Ограничение общего доступа для доменов в SharePoint Online и OneDrive для бизнеса](https://support.office.com/article/restricted-domains-sharing-in-sharepoint-online-and-onedrive-for-business-5d7589cd-0997-4a00-a2ba-2320ec49c4e9).
- В этот список не входят внешние пользователи, которые уже активировали приглашение. Список будет действовать после его настройки. Если приглашения пользователей находятся в состоянии ожидания и вы настроили политику, блокирующую их домены, пользователи не смогут активировать приглашение.

## <a name="set-the-allow-or-deny-list-policy-in-the-portal"></a>Настройка политики списка разрешенных и запрещенных на портале

Параметр **Allow invitations to be sent to any domain (most inclusive)** (Разрешить отправлять приглашения любому домену (всеохватный)) активирован по умолчанию. В этом случае вы можете пригласить пользователей B2B из любой организации.

### <a name="add-a-deny-list"></a>Добавление списка запрещенных

Это самый распространенный сценарий, когда ваша организация хочет сотрудничать со всеми организациями, но не желает приглашать пользователей определенных доменов в качестве пользователей B2B.

Чтобы добавить список запрещенных, сделайте следующее:

1. Войдите на [портале Azure](https://portal.azure.com).
2. Выберите **Azure Active Directory** > **Пользователи** > **Параметры пользователей**.
3. В разделе **Внешние пользователи** выберите **Manage external collaboration settings** (Управление параметрами внешних партнеров).
4. В разделе **Collaboration restrictions** (Ограничения сотрудничества) выберите **Deny invitations to the specified domains** (Запретить отправлять приглашения указанным доменам).
5. В разделе **Целевые домены** введите имя одного из доменов, который следует заблокировать. Если нужно заблокировать несколько доменов, вводите каждый из них в новой строке.

   ![Отображение параметра запрета приглашений и добавленных доменов](./media/active-directory-b2b-allow-deny-list/DenyListSettings.png)
 
6. Закончив, нажмите кнопку **Сохранить**.

Если вы настроили политику и попытаетесь пригласить пользователя из заблокированного домена, отобразится сообщение о том, что пользователь сейчас заблокирован политикой приглашений.
 
### <a name="add-an-allow-list"></a>Добавление списка разрешенных

> [!NOTE]
> Сейчас параметр **Allow invitations only to the specified domains (most restrictive)** (Разрешить отправлять приглашения только указанным доменам (наиболее строгий)) недоступен. Возможность использовать списки разрешенных будет добавлена в ближайшее время.

Это более строгая конфигурация, где можно настроить определенные домены в списке разрешенных и запретить отправлять приглашения любой организации или доменам, которые не указаны. 

Прежде чем использовать список разрешенных, убедитесь, что вы полностью проанализировали потребности своего бизнеса. Если политика будет слишком строгой, ваши пользователи могут начать отправлять документы по электронной почте или найти другие способы сотрудничества (не санкционированные ИТ-службой).

### <a name="switch-from-allow-to-deny-list-and-vice-versa"></a>Переход со списка разрешенных на список запрещенных и наоборот 

При переходе на другую политику текущая конфигурация политики будет отменена. Прежде чем выполнить переход, создайте резервные копии сведений о конфигурации. 

## <a name="set-the-allow-or-deny-list-policy-using-powershell"></a>Настройка списка разрешенных или запрещенных с помощью PowerShell

### <a name="prerequisite"></a>Предварительные требования

Чтобы настроить список разрешенных или запрещенных с помощью PowerShell, необходимо установить предварительную версию модуля Azure Active Directory для Windows PowerShell. В частности, установите модуль AzureADPreview 2.0.0.98 или более поздней версии.

Чтобы проверить версию модуля (и проверить, установлен ли он), сделайте следующее:
 
1. Откройте Windows PowerShell с повышенными правами пользователя (запустите от имени администратора). 
2. Выполните следующую команду, чтобы проверить, установлены ли на компьютере версии модуля Azure Active Directory для Windows PowerShell:

   ````powershell  
   Get-Module -ListAvailable AzureAD*
   ````

Если модуль не установлен или у вас нет необходимой версии, выполните одно из следующих действий:

- Если результаты не отображаются, выполните следующую команду, чтобы установить последнюю версию модуля AzureADPreview:
  
   ````powershell  
   Install-Module AzureADPreview
   ````
- Если в результатах отображается только модуль AzureAD, выполните следующие команды, чтобы установить модуль AzureADPreview: 

   ````powershell 
   Uninstall-Module AzureAD 
   Install-Module AzureADPreview 
   ````
- Если в результатах отображается только модуль AzureADPreview, но используется версия ниже 2.0.0.98, выполните следующие команды, чтобы обновить ее: 

   ````powershell 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
   ````

- Если в результатах отображаются модули AzureAD и AzureADPreview, но версия модуля AzureADPreview ниже 2.0.0.98, выполните следующие команды, чтобы обновить ее: 

   ````powershell 
   Uninstall-Module AzureAD 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
    ````

### <a name="use-the-azureadpolicy-cmdlets-to-configure-the-policy"></a>Настройка политики с помощью командлетов AzureADPolicy

> [!NOTE]
> Сейчас можно настроить только списки запрещенных. Возможность использовать списки разрешенных будет добавлена в ближайшее время.

Чтобы создать список разрешенных или запрещенных, выполните командлет [New AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview). В следующем примере показано, как настроить список запрещенных, который блокирует домен live.com.

````powershell 
$policyValue = @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}")

New-AzureADPolicy -Definition $policyValue -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
````

Далее приведен идентичный пример, но со встроенным определением политики.

````powershell  
New-AzureADPolicy -Definition @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}") -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
````

Чтобы настроить политику разрешенных или запрещенных, выполните командлет [Set-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/set-azureadpolicy?view=azureadps-2.0-preview). Например: 

````powershell   
Set-AzureADPolicy -Definition $policyValue -Id $currentpolicy.Id 
````

Чтобы получить политику, выполните командлет [Get-AzureADPolicy](https://docs.microsoft.com/en-us/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview). Например: 

````powershell
$currentpolicy = Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy'} | select -First 1 
````

Чтобы удалить политику, выполните командлет [Remove-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/remove-azureadpolicy?view=azureadps-2.0-preview). Например: 

````powershell
Remove-AzureADPolicy -Id $currentpolicy.Id 
````

## <a name="next-steps"></a>Дополнительная информация

- Общие сведения об Azure AD B2B см. в статье [Что такое служба совместной работы Azure AD B2B](active-directory-b2b-what-is-azure-ad-b2b.md).
- Сведения об условном доступе и службе совместной работы B2B см. в статье [Условный доступ пользователей в службе совместной работы B2B](active-directory-b2b-mfa-instructions.md).



