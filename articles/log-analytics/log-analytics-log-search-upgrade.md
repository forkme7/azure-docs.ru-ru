---
title: "Обновление Azure Log Analytics до нового поиска по журналам | Документация Майкрософт"
description: "Новый язык запросов Log Analytics практически доступен, и вы сможете участвовать в общедоступной предварительной версии.  В этой статье описаны преимущества нового языка и процедура преобразования рабочей области."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 3bb54f7897876d656da6f1a4b349c9db202a142d
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/03/2018
---
# <a name="azure-log-analytics-upgrade-to-new-log-search"></a>Обновление Azure Log Analytics до нового поиска по журналам

Мы создали новый язык запросов для Log Analytics. Чтобы использовать его, вам необходимо обновить рабочую область.  Вы можете обновить рабочую область вручную или дождаться автоматического обновления, которое будет выполнено в течение периода развертывания с конца октября до конца текущего года.  В этой статье описаны преимущества нового языка и процедура преобразования рабочей области.  

## <a name="why-the-new-language"></a>Преимущества нового языка
Мы понимаем, что при переходе с одного языка на другой возникают проблемы. Мы не просто так меняем язык запросов.  Существует несколько причин, по которым это изменение предоставит значительную ценность клиентам Log Analytics.

- **Простой и эффективный.** Новый язык легче понять. Он аналогичен языку SQL с конструкциями, более похожими на естественный язык, чем устаревший.
- **Язык полной передачи.**  В отличие от устаревшего языка новый язык имеет более широкие возможности передачи.  Практически любые выходные данные можно передать в другую команду, чтобы создать более сложные запросы, чем раньше.
- **Извлечение полей во время поиска.**  По сравнению с устаревшим языком новый язык поддерживает более сложные вычисляемые поля среды выполнения.  Вы можете использовать сложные вычисления для расширенных полей, а затем — вычисляемые поля для дополнительных команд, включающих операции соединения и агрегирования.
- **Сложные соединения.**  Новый язык обеспечивает более сложные соединения, чем устаревший язык, включая возможность объединения таблиц по нескольким полям, использование внутренних и внешних соединений и объединение по расширенным полям.
- **Функции даты и времени.**  По сравнению с устаревшим языком этот язык содержит более расширенные функции даты и времени.
- **Смарт-аналитика.**  Новый язык содержит дополнительные алгоритмы для оценки шаблонов в наборах данных и сравнения различных наборов данных.
- **Портал расширенной аналитики.**  Портал расширенной аналитики предлагает функции анализа, не доступные на портале Log Analytics, включая редактирование нескольких строк запросов, дополнительные визуализации и расширенную диагностику.
- **Согласованность с другими приложениями.**  Новый язык и портал расширенной аналитики уже используются для аналитики в Application Insights.  Реализация этой функции в Log Analytics обеспечивает согласованность между службами Azure.
- **Улучшенная интеграция с Power BI.** Запросы на новом языке можно экспортировать в службу Power BI Desktop, а следовательно можно использовать ее расширенные возможности преобразования данных.
- **Многое другое.** Дополнительные сведения и руководства по новому языку см. на сайте [языка запросов Azure Log Analytics](https://docs.loganalytics.io).


## <a name="when-can-i-upgrade"></a>Когда можно выполнить обновление?
Обновление будет выполняться по всем регионам Azure, поэтому оно может быть доступно в некоторых регионах раньше других.  Когда для вашей рабочей области будет доступна возможность обновления, в верхней части появится баннер с сообщением об этом.

![Обновление 1](media/log-analytics-log-search-upgrade/upgrade-01a.png)

Если рабочая область обновится автоматически, вы увидите баннер с сообщением об этом и краткой сводкой возникших проблем.

 ![Автоматическое обновление](media/log-analytics-log-search-upgrade/auto-upgrade.png)


## <a name="what-happens-after-the-upgrade"></a>Что происходит после обновления?
При преобразовании в рабочую область вносятся следующие изменения.

- Все сохраненные поисковые запросы, правила генерации оповещений и представления, созданные с помощью конструктора представлений, автоматически преобразуются в новый язык.  Поисковые запросы, включенные в решения, не преобразуются автоматически, но они преобразуются "на лету", когда вы их открываете.  
- [Моя панель мониторинга](log-analytics-dashboards.md) заменяется [конструктором представлений](log-analytics-view-designer.md) и [панелями мониторинга Azure](../azure-portal/azure-portal-dashboards.md).  Плитки, которые вы ранее добавили в представление "Моя панель мониторинга", остаются доступными только для чтения.
- [Интеграции Power BI](log-analytics-powerbi.md) заменяется новым процессом.  Отключаются все ранее созданные расписания Power BI, и вам придется создать их заново с помощью нового процесса.
- Изменился формат ответов от [действий предупреждения](log-analytics-alerts-actions.md), использующих веб-перехватчики и модули Runbook и, возможно, вам нужно будет изменить правила генерации оповещений.
- Распространенные вопросы об этом обновлении рассматриваются в статье [Вопросы и ответы по новым возможностям поиска по журналам Log Analytics и известные проблемы](log-analytics-log-search-faq.md).

## <a name="how-do-i-know-if-there-were-any-issues-from-the-upgrade"></a>Как я узнаю, что возникли проблемы с обновлением?
Когда обновление завершится, в настройках рабочей области появится раздел **Сводка обновления**.  В этом разделе вы найдете сведения об обновлении и проблемах, возникших в ходе этого процесса.

 ![Сводка обновления](media/log-analytics-log-search-upgrade/upgrade-summary.png)

## <a name="how-do-i-manually-perform-the-upgrade"></a>Как выполнить обновление вручную?
Когда появится возможность обновить рабочую область, в верхней части портала отобразится соответствующий баннер.  

1.  Чтобы начать процесс обновления, щелкните баннер с сообщением **Дополнительные сведения и обновление**.

    ![Обновление 2](media/log-analytics-log-search-upgrade/upgrade-01a.png)<br>

2.  Дополнительные сведения об обновлении см. на странице сведений об обновлении.

    ![Обновление 2](media/log-analytics-log-search-upgrade/upgrade-03.png)<br>

3.  Щелкните **Обновить сейчас**, чтобы запустить обновление.

    ![Обновление 4](media/log-analytics-log-search-upgrade/upgrade-04.png)<br>В поле уведомления в правом верхнем углу отображается состояние.
    
    ![Обновление 5](media/log-analytics-log-search-upgrade/upgrade-05.png)

4.  Это все!  Перейдите на страницу поиска по журналам, чтобы просмотреть обновленную рабочую область.

    ![Обновление 6](media/log-analytics-log-search-upgrade/upgrade-06.png)

Если возникла проблема, которая приводит к сбою обновления, можно перейти на [форум для обсуждений](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) и опубликовать свой вопрос или [создать запрос в службу поддержки](../azure-supportability/how-to-create-azure-support-request.md) на портале Azure.

## <a name="how-do-i-learn-the-new-language"></a>Где найти дополнительные сведения о новом языке?
Так как он используется несколькими службами, мы создали [внешний сайт для размещения документации](https://docs.loganalytics.io/) по новому языку.  Сюда входят руководства, примеры и полный справочник. Вы можете изучить краткое руководство по новому языку [Getting Started with Queries](https://go.microsoft.com/fwlink/?linkid=856078) (Начало работы с запросами) или [справочник по языку запросов Log Analytics](https://go.microsoft.com/fwlink/?linkid=856079).  

Если вы хотите попробовать использовать новый язык в демонстрационной среде с большим набором тестовых данных, воспользуйтесь [игровой площадкой](https://portal.loganalytics.io/demo#/discover/home).

Если вы уже знакомы с прежней версией языка запросов Log Analytics, можно использовать преобразователь языка, который добавляется в рабочую область при обновлении.  Чтобы просмотреть преобразованную версию, просто введите запрос на старом языке и нажмите кнопку **Преобразовать**.  Затем вы можете нажать кнопку поиска, чтобы выполнить поиск, или скопировать и вставить преобразованный запрос в другое место, например в правило генерации оповещений.  Также мы предлагаем вам [памятку](log-analytics-log-search-transition.md) с прямым сравнением популярных запросов в старой версии языка.

![Преобразователь языка](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="next-steps"></a>Дополнительная информация
- Ознакомьтесь с [руководством по новому языку](https://go.microsoft.com/fwlink/?linkid=856078).
- Изучите [руководство по использованию портала поиска по журналам](log-analytics-log-search-log-search-portal.md) с новым языком запросов.
- Ознакомьтесь с новым [порталом расширенной аналитики](https://go.microsoft.com/fwlink/?linkid=856587).
