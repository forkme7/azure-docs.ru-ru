---
title: Функции продуктов сети доставки содержимого Azure (CDN) | Документация Майкрософт
description: Сведения о возможностях, которые поддерживает каждый продукт Azure доставки содержимого сети (CDN).
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/23/2018
ms.author: rli
ms.custom: mvc
ms.openlocfilehash: 40638e2180b63c90fbcbe15cc2c1e944a97e166e
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2018
---
# <a name="azure-cdn-product-features"></a>Характеристики продукта Azure CDN

Существует три продукта Azure CDN: **Azure CDN уровня "Стандартный" от Akamai**, **Azure CDN уровня "Стандартный" от Verizon** и **Azure CDN уровня "Премиум" от Verizon**. В следующей таблице перечислены функции, доступные в каждом из продуктов.

| **Возможности для повышения производительности и оптимизации** | Akamai уровня "Стандартный" | Verizon уровня "Стандартный" | Verizon уровня "Premium" |
| --- | --- | --- | --- |
| [Динамическое ускорение сайтов](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration) | **&#x2713;**  | **&#x2713;** | **&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Динамическое ускорение сайтов — адаптивное сжатие изображений](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only) | **&#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Динамическое ускорение сайтов — предварительная выборка объектов](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only) | **&#x2713;**  |  |  |
| [Оптимизация потоковой передачи видео](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization) | **&#x2713;**  | \* |  \* |
| [Оптимизация больших файлов](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization) | **&#x2713;**  | \* |  \* |
| [Глобальная балансировка нагрузки сервера (GSLB)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Быстрая очистка.](cdn-purge-endpoint.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Предварительная загрузка ресурса.](cdn-preload-endpoint.md) | |**&#x2713;** |**&#x2713;** |
| Параметры кэша и заголовка (с использованием [правил кэширования](cdn-caching-rules.md)) |**&#x2713;** |**&#x2713;** | |
| Параметры кэша и заголовка (с использованием [обработчика правил](cdn-rules-engine.md)) | | |**&#x2713;** |
| [Кэширование строк запроса.](cdn-query-string.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Двойной стек IPv4/IPv6 |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Поддержка HTTP/2](cdn-http2.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
 **Безопасность** | **Akamai уровня "Стандартный"** | **Verizon уровня "Стандартный"** | **Verizon уровня "Премиум"** | 
| Поддержка HTTPS с конечной точкой CDN |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Пользовательский домен HTTPS](cdn-custom-ssl.md) | |**&#x2713;** |**&#x2713;** |
| [Поддержка пользовательских доменных имен.](cdn-map-content-to-custom-domain.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Геофильтрация](cdn-restrict-access-by-country.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Проверка подлинности по маркерам](cdn-token-auth.md)|  |  |**&#x2713;**| 
| [Защита от атак DDoS](https://www.us-cert.gov/ncas/tips/ST04-015) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
| **Аналитика и отчетность** | **Akamai уровня "Стандартный"** | **Verizon уровня "Стандартный"** | **Verizon уровня "Премиум"** | 
| [Журналы диагностики Azure](cdn-azure-diagnostic-logs.md) | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Основные отчеты из Verizon](cdn-analyze-usage-patterns.md) | |**&#x2713;** |**&#x2713;** |
| [Настраиваемые отчеты из Verizon](cdn-verizon-custom-reports.md) | |**&#x2713;** |**&#x2713;** |
| [Расширенные HTTP-отчеты.](cdn-advanced-http-reports.md) | | |**&#x2713;** |
| [Статистика в режиме реального времени.](cdn-real-time-stats.md) | | |**&#x2713;** |
| [Производительность граничного узла](cdn-edge-performance.md) | | |**&#x2713;** |
| [Оповещения в реальном времени](cdn-real-time-alerts.md) | | |**&#x2713;** |
||||
| **Простота использования** | **Akamai уровня "Стандартный"** | **Verizon уровня "Стандартный"** | **Verizon уровня "Премиум"** | 
| Простая интеграция со службами Azure, такими как [служба хранилища](cdn-create-a-storage-account-with-cdn.md), [облачные службы](cdn-cloud-service-with-cdn.md), [веб-приложения](../app-service/app-service-web-tutorial-content-delivery-network.md) и [службы мультимедиа](../media-services/media-services-portal-manage-streaming-endpoints.md). |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Управление с помощью [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) или [PowerShell](cdn-manage-powershell.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Настраиваемый модуль доставки контента на основе правил.](cdn-rules-engine.md) | | |**&#x2713;** |
| Перенаправление или перезапись URL-адреса (с использованием [обработчика правил](cdn-rules-engine.md)) | | |**&#x2713;** |
| Правила для мобильных устройств (с использованием [обработчик правил](cdn-rules-engine.md)) | | |**&#x2713;** |

\* Verizon поддерживает доставку больших файлов и мультимедиа непосредственно с использованием оптимизации общей веб-доставки.



