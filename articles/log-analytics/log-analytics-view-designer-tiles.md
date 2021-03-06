---
title: Справочное руководство по плиткам конструктора представлений в Azure Log Analytics | Документация Майкрософт
description: С помощью конструктора представлений в Log Analytics можно создавать пользовательские представления на портале Azure, которые отображают различные визуализации данных из рабочей области Log Analytics. В этой статье содержится справочное руководство по параметрам плиток, доступных в пользовательских представлениях.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: ''
ms.assetid: 41787c8f-6c13-4520-b0d3-5d3d84fcf142
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: bwren
ms.openlocfilehash: f341cb9430c7750909c1fc1f50c15f0620e74366
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2018
---
# <a name="reference-guide-to-view-designer-tiles-in-log-analytics"></a>Справочное руководство по плиткам конструктора представлений в Log Analytics
С помощью конструктора представлений в Azure Log Analytics можно создавать пользовательские представления на портале Azure, содержащие различные визуализации данных в рабочей области Log Analytics. В этой статье содержится справочное руководство по параметрам плиток, доступных в пользовательских представлениях.

Дополнительные сведения о конструкторе представлений см. в следующих ресурсах:

* [Использование конструктора представлений для создания пользовательских представлений Log Analytics](log-analytics-view-designer.md) — обзор конструктора представлений и процедур создания и редактирования пользовательских представлений.
* [Справка по элементам визуализации конструктора представлений Log Analytics](log-analytics-view-designer-parts.md) — справочник по параметрам элементов визуализации, доступных в пользовательских представлениях.


В приведенной ниже таблице описаны плитки, доступные в конструкторе представлений.  

| Плитка | ОПИСАНИЕ |
|:--- |:--- |
| [Число](#number-tile) |Количество записей из запроса. |
| [Два числа](#two-numbers-tile) |Числа записей из двух разных запросов. |
| [Кольцо](#donut-tile) | Диаграмма, составленная на основе запроса, с суммарным значением в центре. |
| [График с выноской](#line-chart-amp-callout-tile) | Составленный на основе запроса график и выноска, в которой указывается суммарное значение. |
| [График](#line-chart-tile) |График, составленный на основе запроса. |
| [Две временные шкалы](#two-timelines-tile) | Гистограмма с двумя рядами данных, каждый из которых составляется на основе отдельного запроса. |

В следующих разделах подробно описаны плитки различных типов и их свойства.

## <a name="number-tile"></a>Плитка "Число"
На плитке **Число** отображается число, обозначающее количество записей из запроса журнала, и подпись.

![Плитка "Число"](media/log-analytics-view-designer/tile-number.png)

| Параметр | ОПИСАНИЕ |
|:--- |:--- |
| ИМЯ |Текст, отображаемый в верхней части плитки. |
| ОПИСАНИЕ |Текст, отображаемый под именем плитки. |
| **Плитка** | |
| Условные обозначения |Текст, отображаемый под значением. |
| Запрос |Выполняемый запрос. Отобразится счетчик количества записей, возвращенных запросом. |
| **Дополнительно** |**> Проверка потока данных** |
| Включено |Выберите эту ссылку, если требуется включить проверку потока данных для плитки. Такой подход предоставляет альтернативное сообщение при отсутствии данных. Как правило, этот подход используется для предоставления сообщения в течение некоторого времени, пока не установится представление или пока не появятся данные. |
| Запрос |Запрос, позволяющий определить, доступны ли данные для представления. Если запрос не возвращает результат, то вместо значения из основного запроса отображается соответствующее сообщение. |
| Сообщение |Сообщение, которое отображается, если запрос на проверку потока данных не возвращает данные. Если сообщение не указано, отображается сообщение о состоянии *Идет оценка*. |


## <a name="two-numbers-tile"></a>Плитка "Два числа"
На этой плитке отображаются числа, означающие количество записей из двух разных запросов журнала, с подписью под каждым из них.

![Плитка "Два числа"](media/log-analytics-view-designer/tile-two-numbers.png)

| Параметр | ОПИСАНИЕ |
|:--- |:--- |
| ИМЯ |Текст, отображаемый в верхней части плитки. |
| ОПИСАНИЕ |Текст, отображаемый под именем плитки. |
| **Первая плитка** | |
| Условные обозначения |Текст, отображаемый под значением. |
| Запрос |Выполняемый запрос. Отобразится счетчик количества записей, возвращенных запросом. |
| **Вторая плитка** | |
| Условные обозначения |Текст, отображаемый под значением. |
| Запрос |Выполняемый запрос. Отобразится счетчик количества записей, возвращенных запросом. |
| **Дополнительно** |**> Проверка потока данных** |
| Включено |Выберите эту ссылку, если требуется включить проверку потока данных для плитки. Такой подход предоставляет альтернативное сообщение при отсутствии данных. Как правило, этот подход используется для предоставления сообщения в течение некоторого времени, пока не установится представление или пока не появятся данные. |
| Запрос |Запрос, позволяющий определить, доступны ли данные для представления. Если запрос не возвращает результат, то вместо значения из основного запроса отображается соответствующее сообщение. |
| Сообщение |Сообщение, которое отображается, если запрос на проверку потока данных не возвращает данные. Если сообщение не указано, отображается сообщение о состоянии *Идет оценка*. |


## <a name="donut-tile"></a>Плитка "Кольцо"
На плитке **Кольцо** отображается одно число, полученное путем сложения чисел из столбца значений в запросе журнала. На кольцевой диаграмме отображаются результаты трех первых записей.

![Плитка "Кольцо"](media/log-analytics-view-designer/tile-donut.png)

| Параметр | ОПИСАНИЕ |
|:--- |:--- |
| ИМЯ |Текст, отображаемый в верхней части плитки. |
| ОПИСАНИЕ |Текст, отображаемый под именем плитки. |
| **Кольцо** | |
| Запрос |Запрос на выполнение для кольцевой диаграммы. Первое свойство — это текстовое значение, а второе — числовое. Обычно этот запрос использует ключевое слово *measure* для суммирования результатов. |
| **Кольцо** |**> Центр** |
| текст |Текст, отображаемый под значением на кольцевой диаграмме. |
| Операция |Операция, которую необходимо выполнить со свойством значения, чтобы получить одно итоговое значение.<ul><li>Sum: добавляет значения всех записей со значением свойства.</li><li>Percentage: процентное отношение суммы значений из записей со значением свойства к суммарному значению всех записей.</li></ul> |
| Итоговые значения, используемые при вычислении центра |При необходимости щелкните знак "плюс" (+), чтобы добавить одно или несколько значений. Результаты запроса ограничены записями со значениями свойств, которые вы указали. Если значения не добавлены, в запрос включаются все записи. |
| **Кольцо** |**> Дополнительные параметры** |
| Цвета |Цвет, отображаемый для каждого из трех основных свойств. Чтобы задать другие цвета для значений определенных свойств, используйте функцию *Расширенное сопоставление цветов*. |
| Расширенное сопоставление цветов |Отображает цвет, предоставляющий значения определенных свойств. Если указанное значение является значением одного из трех основных свойств, вместо стандартного цвета отображается другой цвет. Если свойство не является одним из трех основных свойств, цвет не отображается. |
| **Дополнительно** |**> Проверка потока данных** |
| Включено |Выберите эту ссылку, если требуется включить проверку потока данных для плитки. Такой подход предоставляет альтернативное сообщение при отсутствии данных. Как правило, этот подход используется для предоставления сообщения в течение некоторого времени, пока не установится представление или пока не появятся данные. |
| Запрос |Запрос, позволяющий определить, доступны ли данные для представления. Если запрос не возвращает результат, то вместо значения из основного запроса отображается соответствующее сообщение. |
| Сообщение |Сообщение, которое отображается, если запрос на проверку потока данных не возвращает данные. Если сообщение не указано, отображается сообщение о состоянии *Идет оценка*. |


## <a name="line-chart-tile"></a>Плитка "График"
Эта плитка представляет собой график с несколькими рядами данных из запроса к журналу за некоторый промежуток времени. 

![Плитка "График с выноской"](media/log-analytics-view-designer/tile-line-chart.png)

| Параметр | ОПИСАНИЕ |
|:--- |:--- |
| ИМЯ |Текст, отображаемый в верхней части плитки. |
| ОПИСАНИЕ |Текст, отображаемый под именем плитки. |
| **График** | |
| Запрос |Запрос на выполнение для графика. Первое свойство — это текстовое значение, а второе — числовое. Обычно этот запрос использует ключевое слово *measure* для суммирования результатов. Если запрос использует ключевое слово *interval*, для оси Х используется этот временной интервал. Если запрос не содержит ключевое слово *interval*, для оси Х используются ежечасные интервалы. |
| **График** |**> Ось Y** |
| Использовать логарифмическую шкалу |Щелкните эту ссылку для отображения логарифмической шкалы для оси Y. |
| Units |Укажите единицы измерения для значений, возвращенных запросом. Эти сведения используются для отображения на графике подписей, в которых указаны типы значений, и при необходимости для преобразования значений. **Тип единицы** определяет категорию единицы измерения и доступные значения для **единиц текущего типа**. При выборе значения параметра **Преобразовать в** числовые значения преобразуются из значений **единиц текущего типа** в значения того типа, который указан для параметра **Преобразовать в**. |
| Настраиваемая подпись |Текст, отображаемый для оси Y рядом с подписью, в которой указан тип *единицы*. Если подпись не указана, отображается только тип *единицы*. |
| **Дополнительно** |**> Проверка потока данных** |
| Включено |Выберите эту ссылку, если требуется включить проверку потока данных для плитки. Такой подход предоставляет альтернативное сообщение при отсутствии данных. Как правило, этот подход используется для предоставления сообщения в течение некоторого времени, пока не установится представление или пока не появятся данные. |
| Запрос |Запрос, позволяющий определить, доступны ли данные для представления. Если запрос не возвращает результат, то вместо значения из основного запроса отображается соответствующее сообщение. |
| Сообщение |Сообщение, которое отображается, если запрос на проверку потока данных не возвращает данные. Если сообщение не указано, отображается сообщение о состоянии *Идет оценка*. |


## <a name="line-chart-and-callout-tile"></a>Плитка "График с выноской"
В этой плитке имеется график с несколькими рядами данных из запросов к журналу за некоторый промежуток времени и выноска с итоговым значением. 

![Плитка "График с выноской"](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Параметр | ОПИСАНИЕ |
|:--- |:--- |
| ИМЯ |Текст, отображаемый в верхней части плитки. |
| ОПИСАНИЕ |Текст, отображаемый под именем плитки. |
| **График** | |
| Запрос |Запрос на выполнение для графика. Первое свойство — это текстовое значение, а второе — числовое. Обычно этот запрос использует ключевое слово *measure* для суммирования результатов. Если запрос использует ключевое слово *interval*, для оси Х используется этот временной интервал. Если запрос не содержит ключевое слово *interval*, для оси Х используются ежечасные интервалы. |
| **График** |**> Выноска** |
| Имя выноски | Текст, отображаемый над значением выноски. |
| Имя ряда |Значение свойства ряда, используемое в качестве значения выноски. Если ни один ряд не указан, используются все записи из запроса. |
| Операция |Операция, которую необходимо выполнить со свойством значения, чтобы получить одно итоговое значение для выноски.<ul><li>Average: среднее значение для всех записей.</li><li>Count: количество всех записей, возвращенных запросом.</li><li>Last Sample: значение последнего интервала, представленного на графике.</li><li>Max: максимальное значение всех интервалов, представленных на графике.</li><li>Min: минимальное значение всех интервалов, представленных на графике.</li><li>Sum: сумма значений всех записей.</li></ul> |
| **График** |**> Ось Y** |
| Использовать логарифмическую шкалу |Щелкните эту ссылку для отображения логарифмической шкалы для оси Y. |
| Units |Укажите единицы измерения для значений, возвращенных запросом. Эти сведения используются для отображения на графике подписей, в которых указаны типы значений, и при необходимости для преобразования значений. *Тип единицы* определяет категорию единицы измерения и доступные значения для *единиц текущего типа*. При выборе значения параметра *Convert to* (Преобразовать в) числовые значения преобразуются из значений *единиц текущего типа* в значения того типа, который указан для параметра *Convert to* (Преобразовать в). |
| Настраиваемая подпись |Текст, отображаемый для оси Y рядом с подписью, в которой указан тип *единицы*. Если подпись не указана, отображается только тип *единицы*. |
| **Дополнительно** |**> Проверка потока данных** |
| Включено |Выберите эту ссылку, если требуется включить проверку потока данных для плитки. Такой подход предоставляет альтернативное сообщение при отсутствии данных. Как правило, этот подход используется для предоставления сообщения в течение некоторого времени, пока не установится представление или пока не появятся данные. |
| Запрос |Запрос, позволяющий определить, доступны ли данные для представления. Если запрос не возвращает результат, то вместо значения из основного запроса отображается соответствующее сообщение. |
| Сообщение |Сообщение, которое отображается, если запрос на проверку потока данных не возвращает данные. Если сообщение не указано, отображается сообщение о состоянии *Идет оценка*. |


## <a name="two-timelines-tile"></a>Плитка "Две временные шкалы"
На плитке **Две временные шкалы** отображаются результаты двух запросов журнала за определенный отрезок времени, представленные в виде гистограмм. Для каждого ряда отображается отдельная выноска. 

![Плитка "Две временные шкалы"](media/log-analytics-view-designer/tile-two-timelines.png)

| Параметр | ОПИСАНИЕ |
|:--- |:--- |
| ИМЯ |Текст, отображаемый в верхней части плитки. |
| ОПИСАНИЕ |Текст, отображаемый под именем плитки. |
| Первая диаграмма | |
| Условные обозначения |Текст, отображаемый под выноской для первого ряда. |
| Цвет |Цвет, используемый для столбцов в первом ряду. |
| Запрос диаграммы |Запрос на выполнение первого ряда. Счетчик записей за каждый временной интервал представлен в виде столбцов диаграммы. |
| Операция |Операция, которую необходимо выполнить со свойством значения, чтобы получить одно итоговое значение для выноски.<ul><li>Average: среднее значение для всех записей.</li><li>Count: количество всех записей, возвращенных запросом.</li><li>Last Sample: значение последнего интервала, представленного на графике.</li><li>Max: максимальное значение всех интервалов, представленных на графике.</li></ul> |
| **Вторая диаграмма** | |
| Условные обозначения |Текст, отображаемый под выноской для второго ряда. |
| Цвет |Цвет, используемый для столбцов во втором ряду. |
| Запрос диаграммы |Запрос на выполнение второго ряда. Счетчик записей за каждый временной интервал представлен в виде столбцов диаграммы. |
| Операция |Операция, которую необходимо выполнить со свойством значения, чтобы получить одно итоговое значение для выноски.<ul><li>Average: среднее значение для всех записей.</li><li>Count: количество всех записей, возвращенных запросом.</li><li>Last Sample: значение последнего интервала, представленного на графике.</li><li>Max: максимальное значение всех интервалов, представленных на графике. |
| **Дополнительно** |**> Проверка потока данных** |
| Включено |Выберите эту ссылку, если требуется включить проверку потока данных для плитки. Такой подход предоставляет альтернативное сообщение при отсутствии данных. Как правило, этот подход используется для предоставления сообщения в течение некоторого времени, пока не установится представление или пока не появятся данные. |
| Запрос |Запрос, позволяющий определить, доступны ли данные для представления. Если запрос не возвращает результат, то вместо значения из основного запроса отображается соответствующее сообщение. |
| Сообщение |Сообщение, которое отображается, если запрос на проверку потока данных не возвращает данные. Если сообщение не указано, отображается сообщение о состоянии *Идет оценка*. |


## <a name="next-steps"></a>Дополнительная информация
* Узнайте больше о [поиске по журналу](log-analytics-log-searches.md) для поддержки запросов на плитках.
* Добавьте [элементы визуализации](log-analytics-view-designer-parts.md) к пользовательскому представлению.
