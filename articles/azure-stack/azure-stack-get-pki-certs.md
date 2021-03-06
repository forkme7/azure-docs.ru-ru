---
title: Создание сертификатов инфраструктуры открытых ключей Azure Stack для развертывания интегрированных систем Azure Stack | Документация Майкрософт
description: В этой статье описано, как развернуть сертификаты PKI Azure Stack для интегрированных систем Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: cbc1efaee7404c3ffc82acea0846136c43eba2a9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2018
---
# <a name="azure-stack-certificates-signing-request-generation"></a>Создание запроса на подпись сертификата Azure Stack

Инструмент проверки готовности Azure Stack, описанный в этой статье, доступен в [коллекции PowerShell](https://aka.ms/AzsReadinessChecker). С его помощью можно создавать запросы на подпись сертификатов, которые подходят для развертывания Azure Stack. Сертификаты необходимо запросить, создать и проверить, а также выделить достаточно времени на их тестирование перед развертыванием. 

Инструмент проверки готовности Azure Stack (AzsReadinessChecker) выполняет следующие запросы сертификатов:

 - **Стандартные запросы сертификатов**  
    В [этой статье](azure-stack-get-pki-certs.md) описано, как выполнять эти запросы. 
 - **Тип запроса**  
    Запросите несколько шаблонов SAN-сертификата, сертификаты доменов и единый групповой сертификат.
 - **Платформа как услуга (PaaS)**  
    При необходимости запросите сертификаты PaaS, как описано в разделе [Необязательные сертификаты PaaS](azure-stack-pki-certs.md#optional-paas-certificates).

## <a name="prerequisites"></a>предварительным требованиям

Прежде чем создать запросы на подпись сертификатов PKI для развертывания Azure Stack, необходимо убедиться, что в системе присутствуют приведенные ниже компоненты и установлена нужная ОС.

 - Инструмент проверки готовности Microsoft Azure Stack
 - Атрибуты сертификата:
    - Имя региона
    - Внешнее полное доменное имя (FQDN)
    - Субъект
 - Windows 10 или Windows Server 2016;

## <a name="generate-certificate-signing-requests"></a>Создание запроса на подпись сертификата

Для подготовки и проверки сертификатов PKI Azure Stack выполните следующие действия: 

1.  Установите AzsReadinessChecker из командной строки PowerShell (версии 5.1 или более поздней), выполнив следующий командлет:

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker
    ````

2.  Объявите **subject** в качестве упорядоченного словаря. Например:  

    ````PowerShell  
    $subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"} 
    ````
    > [!note]  
    > Если указано общее имя (CN), оно будет использоваться в качестве первого DNS-имени запроса сертификата.

3.  Объявите имеющийся выходной каталог:

    ````PowerShell  
    $outputDirectory = "$ENV:USERNAME\Documents\AzureStackCSR" 
    ````

4. Объявите **имя региона** и **внешнее полное доменное имя**, которые предназначены для развертывания Azure Stack.

    ```PowerShell  
    $regionName = 'east'
    $externalFQDN = 'azurestack.contoso.com'
    ````

    > [!note]  
    > На основе `<regionName>.<externalFQDN>` создаются все внешние DNS-имена в Azure Stack. В этом примере используется портал `portal.east.azurestack.contoso.com`.

5. Чтобы создать единый запрос на сертификат с несколькими альтернативными именами субъекта, в том числе те, которые необходимы для службы PaaS, выполните следующую команду:

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType SingleCSR -OutputRequestPath $OutputDirectory -IncludePaaS
    ````

6. Чтобы создать отдельные запросы на подпись сертификатов для каждого DNS-имени (без необходимых службам PaaS), выполните следующую команду:

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType MultipleCSR -OutputRequestPath $OutputDirectory
    ````

7. Просмотрите выходные данные:

    ````PowerShell  
    AzsReadinessChecker v1.1803.405.3 started
    Starting Certificate Request Generation

    CSR generating for following SAN(s): dns=*.east.azurestack.contoso.com&dns=*.blob.east.azurestack.contoso.com&dns=*.queue.east.azurestack.contoso.com&dns=*.table.east.azurestack.cont
    oso.com&dns=*.vault.east.azurestack.contoso.com&dns=*.adminvault.east.azurestack.contoso.com&dns=portal.east.azurestack.contoso.com&dns=adminportal.east.azurestack.contoso.com&dns=ma
    nagement.east.azurestack.contoso.com&dns=adminmanagement.east.azurestack.contoso.com
    Present this CSR to your Certificate Authority for Certificate Generation: C:\Users\username\Documents\AzureStackCSR\wildcard_east_azurestack_contoso_com_CertRequest_20180405233530.req
    Certreq.exe output: CertReq: Request Created

    Finished Certificate Request Generation

    AzsReadinessChecker Log location: C:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.ReadinessChecker\1.1803.405.3\AzsReadinessChecker.log
    AzsReadinessChecker Completed
    ````

8.  Отправьте созданный **REQ-файл** в центр сертификации (внутренний или общедоступный).  В выходном каталоге **Start-AzsReadinessChecker** содержится запрос на подпись сертификатов, который необходимо отправить в центр сертификации.  Он также содержит дочерний каталог, содержащий INF-файлы, которые используются во время создания запроса сертификата. Убедитесь, что сертификаты в центре сертификации создаются с помощью запроса, который соответствует требованиям из статьи [Требования к сертификатам инфраструктуры открытых ключей Azure Stack](azure-stack-pki-certs.md).

## <a name="next-steps"></a>Дополнительная информация

[Подготовка сертификатов PKI Azure Stack](azure-stack-prepare-pki-certs.md)

