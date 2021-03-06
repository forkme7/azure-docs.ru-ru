---
title: Импорт данных Log Analytics Azure в Power BI | Документация Майкрософт
description: Power BI — это облачная служба бизнес-аналитики от Майкрософт, предоставляющая расширенную визуализацию данных и отчеты по анализу различных наборов данных.  В этой статье описываются способы настройки и импорта данных Log Analytics в Power BI, а также настройка этой службы для автоматического обновления.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/19/2018
ms.author: bwren
ms.openlocfilehash: 725828c2acc5ac4bb53c5e6af14d20578a3d3652
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2018
---
# <a name="import-azure-log-analytics-data-into-power-bi"></a>Импорт данных Log Analytics Azure в Power BI


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) — это облачная служба бизнес-аналитики от Майкрософт, предоставляющая расширенную визуализацию данных и отчеты по анализу различных наборов данных.  Результаты поиска по журналам Log Analytics можно импортировать в набор данных Power BI, чтобы можно было воспользоваться такими функциями, как объединение данных из различных источников и совместное использование отчетов в веб-устройствах и мобильных устройствах.

## <a name="overview"></a>Обзор
Для импорта данных из рабочей области Log Analytics в Power BI создается набор данных в Power BI на основе запроса поиска по журналам в службе Log Analytics.  Запрос выполняется каждый раз при обновлении набора данных.  Затем можно создавать отчеты Power BI, которые используют данные из набора данных.  Чтобы создать набор данных в Power BI, нужно экспортировать запрос из Log Analytics в [язык Power Query (M)](https://msdn.microsoft.com/library/mt807488.aspx).  Потом этот импортированный запрос используется для создания запроса в Power BI Desktop, а затем публикуется в Power BI в качестве набора данных.  Ниже описаны подробности этого процесса.

![Из Log Analytics в Power BI](media/log-analytics-powerbi/overview.png)

## <a name="export-query"></a>Экспорт запроса
Нужно начать с создания [поиска по журналам](log-analytics-log-search-new.md), который возвращает данные из Log Analytics, которые в свою очередь необходимы для заполнения набора данных Power BI.  Затем экспортируйте этот запрос в [язык Power Query (M)](https://msdn.microsoft.com/library/mt807488.aspx), который может использоваться в Power BI Desktop.

1. Создайте поиск по журналам в Log Analytics для извлечения данных для своего набора данных.
2. Если вы используете портал поиска по журналам, щелкните **Power BI**.  Если вы используете портал Analytics, выберите **Экспорт** > **Power BI Query (M)**.  Оба этих параметра экспортируют запрос в текстовый файл с именем **PowerBIQuery.txt**. 

    ![Экспорт поиска по журналам](media/log-analytics-powerbi/export-logsearch.png) ![Экспорт поиска по журналам](media/log-analytics-powerbi/export-analytics.png)

3. Откройте текстовый файл и скопируйте его содержимое.

## <a name="import-query-into-power-bi-desktop"></a>Импорт запроса в Power BI Desktop
Power BI Desktop — это классическое приложение, позволяющее создавать наборы данных и отчеты, которые можно опубликовать в Power BI.  Его также можно использовать для создания запросов на языке Power Query, экспортированном из Log Analytics. 

1. Установите приложение [Power BI Desktop](https://powerbi.microsoft.com/desktop/), если у вас его еще нет, а затем откройте его.
2. Чтобы открыть новый запрос, выберите **Получить данные** > **Пустой запрос**.  Затем выберите **Расширенный редактор** и вставьте содержимое экспортированного файла в запрос. Нажмите кнопку **Done**(Готово).

    ![Запрос Power BI Desktop](media/log-analytics-powerbi/desktop-new-query.png)

5. Запрос выполняется и его результаты отображаются.  Возможно потребуется ввести учетные данные для подключения к Azure.  
6. Введите описательное имя для запроса.  По умолчанию используется значение **Query1**. Чтобы добавить набор данных в отчет, нажмите кнопку **Close and Apply** (Закрыть и применить).

    ![Имя Power BI Desktop](media/log-analytics-powerbi/desktop-results.png)



## <a name="publish-to-power-bi"></a>Публикация в Power BI
При публикации в Power BI создается набор данных и отчет.  При создании отчета в Power BI Desktop он будет опубликован с вашими данными.  Если нет, то будет создан пустой отчет.  В Power BI можно изменить отчет или создать новый на основе набора данных.

8. Создайте отчет на основе данных.  Прочитайте [документацию по Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-report-view), если вы не знакомы с ней.  Когда вы будете готовы отправить отчет в Power BI, выберите **Опубликовать**.  При появлении запроса выберите место назначения в своей учетной записи Power BI.  При отсутствии определенного места назначения можно использовать область **Моя рабочая область**.

    ![Публикация в Power BI Desktop](media/log-analytics-powerbi/desktop-publish.png)

3. Чтобы открыть Power BI с новым набором данных, после завершения публикации щелкните **Открыть в Power BI**.


### <a name="configure-scheduled-refresh"></a>Настройка запланированного обновления
В наборе данных, созданном в Power BI, будут содержаться те же данные, что и ранее в Power BI Desktop.  Чтобы снова запустить запрос и заполнить его последними данными из Log Analytics, нужно периодически обновлять набор данных.  

1. Щелкните в рабочей области, где находится переданный отчет, и выберите меню **Наборы данных**. Откройте контекстное меню нового набора данных и выберите **Параметры**. В разделе **Учетные данные источника данных** должно быть указано, что учетные данные недействительны.  Это связано с тем, что учетные данные, используемые при обновлении данных в наборе данных, еще не указаны.  Щелкните **Изменить учетные данные** и укажите учетные данные, позволяющие получить доступ к Log Analytics.

    ![Расписание Power BI](media/log-analytics-powerbi/powerbi-schedule.png)

5. В разделе **Запланированное обновление** включите переключатель в области **Поддерживать актуальность данных**.  При необходимости можно изменить **периодичность обновления**, а также задать одну или несколько точек времени для обновления.

    ![Обновление Power BI](media/log-analytics-powerbi/powerbi-schedule-refresh.png)



## <a name="next-steps"></a>Дополнительная информация
* Узнайте больше о [запросах поиска по журналам](log-analytics-log-searches.md) , чтобы создавать запросы, которые можно экспортировать в Power BI.
* Узнайте больше о [Power BI](http://powerbi.microsoft.com), чтобы создавать визуализации на основе экспорта Log Analytics.
