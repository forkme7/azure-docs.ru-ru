---
title: Этап развертывания в жизненном цикле процесса обработки и анализа данных группы в Azure | Документация Майкрософт
description: Цели, задачи и конечные результаты на этапе развертывания проектов обработки и анализа данных
services: machine-learning
documentationcenter: ''
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: 5cb6361ed674ffaaf776adafd6f3ff87272c73eb
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2018
---
# <a name="deployment"></a>Развертывание

В этой статье описаны цели, задачи и конечные результаты, связанные с этапом развертывания процесса обработки и анализа данных группы (TDSP). Этот процесс выполняется в рамках рекомендуемого жизненного цикла, позволяя структурировать проекты по обработке и анализу данных. Этот жизненный цикл представляет основные этапы, которые обычно выполняются проектами, часто итеративно:

   1. **Коммерческий аспект**.
   2. **Получение и анализ данных**.
   3. **Моделирование**
   4. **Развертывание**
   5. **Прием клиентом**.

Визуальное представление жизненного цикла процесса обработки и анализа данных группы: 

![Жизненный цикл процесса обработки и анализа данных группы](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Цель
Развернуть модели с конвейером данных в производственную или близкую к ней среду для окончательного утверждения клиентом. 

## <a name="how-to-do-it"></a>Как это сделать
Основная задача, решаемая на этом этапе.

**Ввод модели в эксплуатацию**. Разверните модель и конвейер в рабочую или близкую к ней среду, чтобы приложения могли к ней обращаться.

### <a name="operationalize-a-model"></a>Ввод модели в эксплуатацию
Создав набор эффективно работающих моделей, можно ввести их в эксплуатацию для взаимодействия с другими приложениями. В зависимости от бизнес-требований прогнозы выполняются в режиме реального времени или в пакетном режиме. Чтобы развернуть модели, нужно предоставить их с помощью открытого API-интерфейса. Интерфейс упрощает использование модели различными приложениями, например:

   * веб-сайты в Интернете;
   * электронные таблицы; 
   * Панели мониторинга
   * бизнес-приложения; 
   * серверные приложения. 

Примеры ввода в эксплуатацию веб-службы машинного обучения Azure см. в статье [Развертывание веб-службы машинного обучения Azure](../studio/publish-a-machine-learning-web-service.md). Мы рекомендуем встроить в производственную модель и развернутый конвейер средства телеметрии и мониторинга. Это позволит получать отчеты о состоянии системы и устранять неполадки.  

## <a name="artifacts"></a>Артефакты

* Панель мониторинга состояния, на которой представлены показатели работоспособности и ключевые метрики.
* Окончательный отчет по моделированию с информацией о развертывании.
* Документированная окончательная архитектура решения.


## <a name="next-steps"></a>Дополнительная информация

Ниже приведены ссылки на каждый этап жизненного цикла процесса обработки и анализа данных группы:

   1. [Коммерческий аспект](lifecycle-business-understanding.md).
   2. [Получение и анализ данных](lifecycle-data.md).
   3. [Моделирование](lifecycle-modeling.md)
   4. [Развертывание](lifecycle-deployment.md)
   5. [Прием клиентом](lifecycle-acceptance.md).

Кроме того, предоставляются полные пошаговые руководства, которые демонстрируют все этапы процесса для конкретных сценариев. Статья [Пошаговые руководства по процессу обработки и анализа данных группы](walkthroughs.md) содержит список сценариев, ссылки и описания эскизов. В пошаговых руководствах показано, как объединить облачные, локальные инструменты и службы в единый рабочий процесс или конвейер, чтобы создать интеллектуальное приложение. 

Примеры выполнения шагов в процессе обработки и анализа данных группы, который использует среду "Студия машинного обучения Azure", см. в статье [Командный процесс обработки и анализа данных с использованием службы "Машинное обучение Azure"](http://aka.ms/datascienceprocess).
