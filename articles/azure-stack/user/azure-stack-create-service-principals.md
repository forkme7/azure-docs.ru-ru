---
title: Создание субъекта-службы для Azure Stack | Документация Майкрософт
description: Описывается создание субъекта-службы, который можно использовать в Azure Resource Manager в сочетании с контролем доступа на основе ролей для управления доступом к ресурсам.
services: azure-resource-manager
documentationcenter: na
author: mattbriggs
manager: femila
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 0517c85c62aaffd1055206120281c7b7de31ad82
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2018
---
# <a name="provide-applications-access-to-azure-stack"></a>Предоставление приложениям доступа к Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Если приложению нужен доступ для развертывания или настройки ресурсов Azure Stack через Azure Resource Manager, вам следует создать субъект-службу, который будет использоваться как учетные данные для этого приложения.  Этому субъекту-службе вы сможете делегировать только минимально необходимые разрешения.  

Например, можно создать средство управления конфигурацией, которое использует Azure Resource Manager для создания списка ресурсов Azure.  Чтобы реализовать этот сценарий, вам нужно создать субъект-службу, назначить ему роль читателя и предоставить средству управления конфигурацией доступ только для чтения. 

Субъекты-службы предпочтительнее использовать для запуска приложения с вашими учетными данными по следующим причинам:

* Для субъекта-службы можно назначить разрешения, которые отличаются от ваших разрешений учетной записи. Как правило, приложение получает именно те разрешения, которые требуются для его работы.
* Не требуется изменять учетные данные приложения в случае изменения ваших обязанностей.
* Можно использовать сертификат, чтобы автоматизировать аутентификацию при выполнении автоматического сценария.  

## <a name="getting-started"></a>Приступая к работе

Прежде всего нужно создать субъект-службу. Процесс будет разным в зависимости от способа развертывания Azure Stack.  В этом документе описывается создание субъекта-службы для [Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad) и [службы федерации Active Directory (AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).  Создав субъект-службу, вы [делегируете разрешения](azure-stack-create-service-principals.md#assign-role-to-service-principal) для этой роли, используя единый процесс для AD FS и Azure AD.     

## <a name="create-service-principal-for-azure-ad"></a>Создание субъекта-службы для Azure AD

Если Azure Stack развернут с использованием Azure AD в качестве хранилища идентификаторов, создание субъекта-службы выполняется точно так же, как для Azure.  В этом разделе описан процесс с использованием портала.  Прежде чем начать, проверьте [необходимые разрешения Azure AD](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions).

### <a name="create-service-principal"></a>Создание субъекта-службы
В этом разделе вы создадите в Azure AD приложение (субъект-службу), которое будет представлять ваше приложение.

1. Войдите в учетную запись Azure на [портале Azure](https://portal.azure.com).
2. Выберите **Azure Active Directory** > **Регистрация приложений** > **Добавить**.   
3. Укажите имя и URL-адрес для приложения. Выберите тип создаваемого приложения: **веб-приложение или API** или **собственное приложение**. Выбрав нужные значения, нажмите кнопку **Создать**.

Субъект-служба для приложения создан.

### <a name="get-credentials"></a>Получение учетных данных
Если вход выполняется программными средствами, вам потребуются идентификатор приложения и ключ аутентификации. Получить эти значения можно следующим образом.

1. В Active Directory в разделе **регистрации приложений** выберите нужное приложение.

2. Скопируйте **идентификатор приложения** и сохраните его в коде приложения. Приложения в разделе [примеров приложений](#sample-applications) используют это значение как идентификатор клиента.

     ![Идентификатор клиента](./media/azure-stack-create-service-principal/image12.png)
3. Чтобы создать ключ проверки подлинности, щелкните **Ключи**.

4. Введите описание и срок действия ключа. Затем нажмите кнопку **Сохранить**.

После этого отобразится значение ключа. Это значение нельзя будет получить позже, поэтому скопируйте его сразу. Значение ключа необходимо предоставить вместе с идентификатором приложения для входа от имени приложения. Сохраните значение ключа, чтобы приложение могло получить к нему доступ.

![сохраненный ключ](./media/azure-stack-create-service-principal/image15.png)


После этого [назначьте приложению роль](azure-stack-create-service-principals.md#assign-role-to-service-principal).

## <a name="create-service-principal-for-ad-fs"></a>Создание субъекта-службы для AD FS
Когда вы развернете Azure Stack с использованием AD FS, для создания субъекта-службы, назначения роли для доступа и входа с этим идентификатором можно использовать PowerShell.

### <a name="before-you-begin"></a>Перед началом работы

[Скачайте на локальный компьютер средства, необходимые для работы с Azure Stack.](azure-stack-powershell-download.md)

### <a name="import-the-identity-powershell-module"></a>Импорт модуля Identity PowerShell
Когда скачивание завершится, перейдите в созданную папку средств и импортируйте модуль PowerShell Identity с помощью следующей команды:

```PowerShell
Import-Module .\Identity\AzureStack.Identity.psm1
```

При импорте модуля может появиться сообщение об ошибке, информирующее о том, что AzureStack.Connect.psm1 не имеет цифровой подписи и Скрипт не будет выполнен в системе". Чтобы устранить эту проблему, создайте политику выполнения, которая разрешает запуск скрипта. Для этого в сеансе PowerShell с повышенными привилегиями выполните следующую команду:

```PowerShell
Set-ExecutionPolicy Unrestricted
```

### <a name="create-the-service-principal"></a>Создание субъекта-службы
Чтобы создать субъект-службу, выполните следующую команду, указав требуемое значение для параметра *DisplayName*:
```powershell
$servicePrincipal = New-AzSADGraphServicePrincipal `
 -DisplayName "<YourServicePrincipalName>" `
 -AdminCredential $(Get-Credential) `
 -AdfsMachineName "AZS-ADFS01" `
 -Verbose
```
### <a name="assign-a-role"></a>Назначение роли
Созданному субъекту-службе необходимо [назначить роль](azure-stack-create-service-principals.md#assign-role-to-service-principal).

### <a name="sign-in-through-powershell"></a>Вход с помощью PowerShell
Назначив нужную роль, вы сможете войти в Azure Stack с помощью следующей команды, используя созданный субъект-службу:

```powershell
Add-AzureRmAccount -EnvironmentName "<AzureStackEnvironmentName>" `
 -ServicePrincipal `
 -CertificateThumbprint $servicePrincipal.Thumbprint `
 -ApplicationId $servicePrincipal.ApplicationId ` 
 -TenantId $directoryTenantId
```

## <a name="assign-role-to-service-principal"></a>Назначение роли субъекту-службе
Чтобы обеспечить доступ к ресурсам в подписке, необходимо назначить приложению роль. Укажите, какая роль предоставит приложению необходимые разрешения. Дополнительные сведения о доступных ролях см. в статье [RBAC: встроенные роли](../../role-based-access-control/built-in-roles.md).

Вы можете задать область действия на уровне подписки, группы ресурсов или ресурса. Разрешения наследуют более низкие уровни области действия. Например, добавление приложения в роль читателя для группы ресурсов означает, что оно может считывать группу ресурсов и все содержащиеся в ней ресурсы.

1. На портале Azure Stack перейдите на уровень области действия, в которой вы хотите разместить приложение. Например, чтобы назначить роль в области действия подписки выберите **Подписки**. Или же вы можете выбрать группу ресурсов либо отдельный ресурс.

2. Выберите определенную подписку, группу ресурсов или ресурс, которым будет назначено приложение.

     ![выбрать подписку для назначения](./media/azure-stack-create-service-principal/image16.png)

3. Выберите **Управление доступом (IAM)**.

     ![выбрать доступ](./media/azure-stack-create-service-principal/image17.png)

4. Выберите **Добавить**.

5. Выберите роль, которая будет назначена приложению.

6. Найдите приложение и выберите его.

7. Нажмите кнопку **ОК**, чтобы завершить назначение роли. Вы увидите свое приложение в списке пользователей, назначенных выбранной роли для выбранной области действия.

Итак, вы создали субъект-службу и назначили ему роль. Теперь вы можете использовать его в приложении для доступа к ресурсам Azure Stack.  

## <a name="next-steps"></a>Дополнительная информация

[Управление разрешениями пользователей](azure-stack-manage-permissions.md)
