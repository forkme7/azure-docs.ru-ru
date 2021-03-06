---
title: Цены и выставление счетов на службы "Машинное обучение Azure"
description: В этой статье приведены ответы на часто задаваемые вопросы о ценах и выставлении счетов на предварительные версии функций в службе "Машинное обучение Azure".
services: machine-learning
author: j-martens
ms.author: jmartens
manager: cgronlund
ms.reviewer: mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 03/30/2018
ms.openlocfilehash: 5e057f3fe4a4ff06e0acac29b3dcd11fa901fc40
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/19/2018
---
# <a name="pricing-and-billing-for-azure-machine-learning-services"></a>Цены и выставление счетов на службы "Машинное обучение Azure"

Дополнительные сведения или пример счета см. на [странице с ценами Azure](https://azure.microsoft.com/pricing/details/machine-learning-services/).  

Ниже приведены ответы на некоторые часто задаваемые вопросы о ценах и выставлении счетов.

## <a name="can-i-use-azure-machine-learning-for-free-during-preview"></a>Можно ли использовать службу "Машинное обучение Azure" бесплатно во время действия предварительной версии?    

Для подписчиков Azure приложение Azure Machine Learning Workbench доступно бесплатно.

В службах "Экспериментирование" и "Управление моделями" есть уровень "Бесплатный", помимо платных уровней, которые предлагаются со скидкой на период действия общедоступной предварительной версии.

## <a name="am-i-charged-based-on-how-many-experiments-i-run"></a>Зависит ли тариф от числа выполненных экспериментов?

Нет, служба "Экспериментирование" позволяет проводить любое число экспериментов. Счета выставляются только на основе количества пользователей. Плата за вычислительные ресурсы для экспериментов начисляется отдельно.  Мы рекомендуем выполнять несколько экспериментов, чтобы вы могли выбрать подходящую модель для своего решения. 

## <a name="am-i-charged-based-on-how-many-times-my-web-services-is-called"></a>Зависит ли тариф от числа вызовов веб-служб?

Нет. Веб-службы можно вызывать по мере необходимости без дополнительной платы за использование службы "Управление моделями". Вы полностью управляете масштабом развертываний в соответствии с требованиями приложения.

## <a name="how-can-i-scale-the-units-i-purchase-in-the-azure-machine-learning-model-management"></a>Как увеличить число единиц, приобретаемых в службе "Управление моделями Машинного обучения"?

Вы можете изменить число единиц (увеличить или уменьшить), воспользовавшись порталом Azure или CLI. 

## <a name="what-does-a-bill-look-like"></a>Как выглядит счет?

Плата начисляется ежедневно. При расчете стоимости предполагается, что день начинается в полночь (время UTC). Счета выставляются ежемесячно. За все службы Azure, используемые со службой "Машинное обучение Azure", взимается отдельная плата. Помимо прочего, плата может взиматься: 
- за использование вычислительных ресурсов;
- HDInsight
- Служба контейнеров Azure
- реестр контейнеров Azure; 
- Хранилище больших двоичных объектов Azure
- Application Insights
- Хранилище ключей Azure
- Visual Studio Team Services
- концентратору событий Azure
- Azure Stream Analytics. Дополнительные сведения или пример счета см. на [странице с ценами Azure](https://azure.microsoft.com/pricing/details/machine-learning-services/). 
