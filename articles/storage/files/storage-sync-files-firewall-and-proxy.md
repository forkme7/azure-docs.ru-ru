---
title: Локальные параметры брандмауэра и прокси-сервера для службы "Синхронизация файлов Azure" | Документация Майкрософт
description: Конфигурация локальной сети для службы "Синхронизация файлов Azure".
services: storage
documentationcenter: ''
author: fauhse
manager: klaasl
editor: tamram
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: fauhse
ms.openlocfilehash: 81425c6ac4e463bd4242328206bd43ce78a1105a
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2018
---
# <a name="azure-file-sync-proxy-and-firewall-settings"></a>Параметры брандмауэра и прокси-сервера службы "Синхронизация файлов Azure"
Служба "Синхронизация файлов Azure" подключает локальные серверы к службе файлов Azure, обеспечивая синхронизацию нескольких сайтов и распределение данных по уровням облака. Таким образом локальный сервер должен быть подключен к Интернету. Администратор отдела ИТ должен выбрать наилучший путь подключения сервера к облачным службам Azure.

В этой статье описываются особые требования и возможности, позволяющие создать безопасное подключение сервера к службе "Синхронизация файлов Azure".

## <a name="overview"></a>Обзор
Служба "Синхронизация файлов Azure" действует как служба оркестрации между Windows Server, вашим файловым ресурсом Azure и несколькими другими службами Azure, которая синхронизирует данные, как описано в группе синхронизации. Чтобы служба "Синхронизация файлов Azure" работала правильно, необходимо настроить серверы для взаимодействия со следующими службами Azure.

- Хранилище Azure
- Служба синхронизации файлов Azure
- Диспетчер ресурсов Azure
- Службы аутентификации

> [!Note]  
> Агент службы "Синхронизация файлов Azure" на сервере Windows инициирует все запросы к облачным службам, поэтому в брандмауэре следует учитывать только исходящий трафик. <br /> Ни одна из служб Azure не инициирует подключение к агенту службы "Синхронизация файлов Azure".


## <a name="ports"></a>порты;
Служба "Синхронизация файлов Azure" перемещает файлы данных и метаданные только по протоколу HTTPS, и порт 443 должен быть открыт для исходящего трафика.
В результате шифруется весь трафик.

## <a name="networks-and-special-connections-to-azure"></a>Сети и специальные подключения к Azure
Агент службы "Синхронизация файлов Azure" не накладывает какие-либо требования в отношении специальных каналов, например [ExpressRoute](../../expressroute/expressroute-introduction.md), в Azure.

Служба "Синхронизация файлов Azure" будет использовать все доступные средства связи с Azure, автоматически адаптируясь к различным характеристикам сетей (пропускной способности, задержке и т. д.). Кроме того, она предоставляет административный контроль для точной настройки. Некоторые возможности сейчас недоступны. Если вы хотите настроить определенное поведение, свяжитесь с нами с помощью сайта [UserVoice для службы файлов Azure](https://feedback.azure.com/forums/217298-storage?category_id=180670).

## <a name="proxy"></a>Прокси-сервер
В настоящее время служба "Синхронизация файлов Azure" поддерживает параметры прокси-сервера, настроенные на компьютере. Эти параметры прокси-сервера являются прозрачными для агента службы "Синхронизация файлов Azure", так как весь трафик сервера направляется через этот прокси-сервер.

Параметры прокси-сервера для конкретных приложений в настоящее время находятся в разработке и будут поддерживаться в будущих версиях агента службы "Синхронизация файлов Azure". Это позволит настроить прокси-сервер специально для трафика службы "Синхронизация файлов Azure".

## <a name="firewall"></a>Брандмауэр
Как упоминалось в предыдущем разделе, порт 443 необходимо открыть для исходящего трафика. В зависимости от политик в центре обработки данных, филиале или регионе, возможно необязательное или обязательное наложение дополнительных ограничений трафика, проходящего через этот порт в определенные домены.

В следующей таблице описаны требуемые домены для обмена данными.

| Service | Домен | Использование |
|---------|----------------|------------------------------|
| **Azure Resource Manager** | https://management.azure.com | Любой вызов пользователя (например, PowerShell) проходит через этот URL-адрес, включая вызов для начальной регистрации сервера. |
| **Azure Active Directory** | https://login.windows.net | Вызовы Azure Resource Manager должны осуществляться пользователем, прошедшим аутентификацию. Чтобы вызовы могли быть выполнены, этот URL-адрес используется для аутентификации пользователей. |
| **Azure Active Directory** | https://graph.windows.net/ | В ходе развертывания службы "Синхронизация файлов Azure" в подписке Azure Active Directory будет создан субъект-служба. Для этого используется данный URL-адрес. Этот субъект используется для делегирования минимального набора прав службе "Синхронизация файлов Azure". Пользователь, выполняющий начальную настройку службы "Синхронизация файлов Azure", должен иметь привилегии владельца подписки. |
| **Хранилище Azure** | &ast;.core.windows.net | Если сервер выполняет скачивание файла, то он эффективнее перемещает данные, когда напрямую обращается к файловому ресурсу Azure в учетной записи хранения. Сервер имеет ключ SAS, предоставляющий доступ только к целевому файловому ресурсу. |
| **Служба "Синхронизация файлов Azure"** | &ast;.one.microsoft.com | После начальной регистрации сервер получает URL-адрес экземпляра службы "Синхронизация файлов Azure" в этом регионе. Сервер может использовать этот URL-адрес для прямого и эффективного взаимодействия с экземпляром, выполняющим его синхронизацию. |

> [!Important]
> Если разрешить передачу трафика в &ast;.one.microsoft.com, то с сервера будет передаваться не только трафик для службы синхронизации. В поддоменах могут размещаться многие другие службы Майкрософт.

Если &ast;.one.microsoft.com задает слишком много адресов, обмен данными с сервером можно ограничить, разрешив только региональные экземпляры службы "Синхронизация файлов Azure". Выбор экземпляров зависит от региона службы синхронизации хранилища, в котором развернут и зарегистрирован сервер. Именно этот регион необходимо разрешить для сервера. Скоро будут доступны дополнительные URL-адреса для новых возможностей обеспечения непрерывности бизнес-процессов. 

| Регион | URL-адрес региональной конечной точки службы "Синхронизация файлов Azure" |
|--------|---------------------------------------|
| Восточная часть Австралии | https://kailani-aue.one.microsoft.com |
| Центральная Канада | https://kailani-cac.one.microsoft.com |
| Восток США | https://kailani1.one.microsoft.com |
| Юго-Восточная Азия | https://kailani10.one.microsoft.com |
| Южная часть Великобритании | https://kailani-uks.one.microsoft.com |
| Западная Европа | https://kailani6.one.microsoft.com |
| Запад США | https://kailani.one.microsoft.com |

> [!Important]
> При определении подробных правил брандмауэра почаще сверяйтесь с этим документом и обновляйте правила брандмауэра, чтобы избежать перерывов в обслуживании из-за устаревших или неполных URL-адресов в параметрах брандмауэра.

## <a name="summary-and-risk-limitation"></a>Сводка и ограничение рисков
Списки, приведенные ранее в этом документе, содержат URL-адреса, с которыми в настоящее время взаимодействует служба "Синхронизация файлов Azure". Брандмауэры должны иметь возможность разрешить исходящий трафик для этих доменов, а также передачу ответов из них. Корпорация Майкрософт прилагает все усилия, чтобы этот список был актуальным.

Настройка домена с ограничивающими правилами брандмауэра может служить мерой повышения безопасности. Если используются такие конфигурации брандмауэров, необходимо помнить, что URL-адреса со временем добавляются и изменяются. Поэтому целесообразно проверять таблицы в этом документе в ходе процесса управления изменениями, когда выполняется переход с одной версии агента службы "Синхронизация файлов Azure" на другую для тестирования развертывания последней версии агента. Таким образом можно будет убедиться, что брандмауэр пропускает трафик к доменам, необходимым последней версии агента.

## <a name="next-steps"></a>Дополнительная информация
- [Планирование развертывания службы синхронизации файлов Azure (предварительная версия)](storage-sync-files-planning.md)
- [Развертывание службы синхронизации файлов Azure (предварительная версия)](storage-sync-files-deployment-guide.md)
