---
title: Создание пакетов ELT вместо ETL для хранилища данных SQL в Azure | Документация Майкрософт
description: Для загрузки данных в хранилище данных SQL Azure вместо пакетов ETL создавайте процессы извлечения, загрузки и преобразования данных (ELT).
services: sql-data-warehouse
author: ckarst
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 5ceb8cfd8efea66dbf17b8c522316b9a010e437d
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2018
---
# <a name="designing-extract-load-and-transform-elt-for-azure-sql-data-warehouse"></a>Разработка процесса извлечения, загрузки и преобразования (ELT) для хранилища данных SQL Azure

Для загрузки данных в хранилище данных SQL Azure вместо извлечения, преобразования и загрузки (ETL) используйте механизм извлечения, загрузки и преобразования данных (ELT). В этой статье описаны способы проектирования ELT-процесса для перемещения данных в хранилище данных Azure.

## <a name="what-is-elt"></a>Что такое ELT?

Извлечение, загрузка и преобразование (ELT) — это процесс, с помощью которого данные перемещаются из исходной системы в хранилище данных назначения. Этот процесс выполняется на регулярной основе, например ежечасно или ежедневно, чтобы поместить только что созданные данные в хранилище данных. Лучший способ переместить данные из источника в хранилище данных — разработать процесс ELT, в котором используется PolyBase для загрузки данных в хранилище данных SQL.

ELT сначала загружает, а затем преобразует данные, в то время как ETL сначала извлекает и преобразовывает данные, а затем загружает их. Выполнение ELT вместо ETL экономически выгоднее в отношении предоставления собственных ресурсов для преобразования данных перед их непосредственной загрузкой. При использовании хранилища данных SQL ELT использует систему MPP для выполнения преобразований.

Хотя есть много разновидностей реализации ELT для хранилища данных SQL, ниже приведены основные шаги.  

1. Извлеките исходные данные в текстовые файлы.
2. Поместите данные в хранилище BLOB-объектов Azure или Azure Data Lake Store.
3. Подготовьте данные для загрузки.
2. Загрузите данные в промежуточные таблицы хранилища данных SQL с помощью PolyBase.
3. Преобразуйте данные.
4. Вставьте данные в рабочие таблицы.


Инструкции по загрузке см. в руководстве [Загрузка данных из хранилища BLOB-объектов Azure в хранилище данных SQL Azure с помощью PolyBase](load-data-from-azure-blob-storage-using-polybase.md).

Дополнительные сведения см. в записи блога о [стратегиях и шаблонах загрузки в хранилище данных SQL Azure](http://blogs.msdn.microsoft.com/sqlcat/2017/05/17/azure-sql-data-warehouse-loading-patterns-and-strategies/). 

## <a name="options-for-loading-with-polybase"></a>Варианты загрузки данных с помощью PolyBase

PolyBase — это технология, позволяющая получить доступ к данным за пределами базы данных с помощью языка T-SQL. Эта технология является лучшим способом загрузить данные в хранилище данных SQL. С PolyBase данные загружаются в параллельном режиме из источника данных непосредственно на вычислительные узлы. 

Чтобы загрузить данные с помощью PolyBase, можно использовать любые из вариантов загрузки ниже.

- [PolyBase с использованием T-SQL](load-data-from-azure-blob-storage-using-polybase.md) хорошо работает, когда данные хранятся в хранилище BLOB-объектов Azure или Azure Data Lake Store. Этот вариант предоставляет наибольший контроль над процессом загрузки, но также требует определения объектов внешних данных. Другие методы определяют эти объекты в фоновом режиме, когда вы сопоставляете исходные таблицы с целевыми.  Для оркестрации загрузок T-SQL можно использовать фабрику данных Azure, службы SSIS или функции Azure. 
- [PolyBase со службами SSIS](/sql/integration-services/load-data-to-sql-data-warehouse) является оптимальным выбором, когда исходные данные хранятся в SQL Server — локально или в облаке. Службы SSIS определяют сопоставления исходной и целевой таблиц, а также управляют загрузкой. При наличии пакетов служб SSIS можно изменить пакеты для работы с новым назначением хранилища данных. 
- [PolyBase с фабрикой данных Azure (ADF)](sql-data-warehouse-load-with-data-factory.md) представляет собой другое средство оркестрации.  Оно определяет конвейер и планирует расписания заданий. 
- [PolyBase с Azure DataBricks](../azure-databricks/databricks-extract-load-sql-data-warehouse.md) передает данные из таблицы хранилища данных SQL в кадр данных Databricks и (или) записывает данные из кадра данных Databricks в таблицу хранилища данных SQL.

### <a name="polybase-external-file-formats"></a>Форматы внешних файлов PolyBase

PolyBase загружает данные из текстовых файлов с разделителями в кодировке UTF-8 и UTF-16. Кроме текстовых файлов с разделителями данные также загружаются из форматов файлов Hadoop: RC, ORC и PARQUET. PolyBase может загрузить данные из сжатых файлов Gzip и Snappy. PolyBase в настоящее время не поддерживает расширенную кодировку ASCII, форматы с фиксированной шириной и вложенные форматы, такие как WinZip, JSON и XML.

### <a name="non-polybase-loading-options"></a>Варианты загрузки, отличные от PolyBase
Если данные несовместимы с PolyBase, можно использовать программу [bcp](/sql/tools/bcp-utility) или [API-интерфейс SQLBulkCopy](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy.aspx). bcp загружает данные напрямую в хранилище данных SQL, минуя хранилище BLOB-объектов Azure и предназначается только для небольших загрузок. Обратите внимание, что производительность загрузки этих вариантов значительно ниже, чем у PolyBase. 


## <a name="extract-source-data"></a>Извлечение исходных данных

Получение данных за пределами исходной системы зависит от источника.  Основной целью является перемещение данных в текстовые файлы с разделителями. Если вы используете SQL Server, можно воспользоваться [программой командной строки bcp](/sql/tools/bcp-utility) для экспорта данных.  

## <a name="land-data-to-azure-storage"></a>Помещение данных в службу хранилища Azure

Чтобы поместить данные в службу хранилища Azure, их можно переместить в [хранилище BLOB-объектов Azure](../storage/blobs/storage-blobs-introduction.md) или [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md). В любом расположении данные должны храниться в текстовых файлах. Polybase может загрузить их из любого расположения.

Ниже приведены средства и службы, которые можно использовать для перемещения данных в службу хранилища Azure.

- Служба [Azure ExpressRoute](../expressroute/expressroute-introduction.md) повышает пропускную способность сети, производительность, а также предсказуемое поведение. ExpressRoute — это служба, которая направляет данные с помощью выделенного частного подключения в Azure. Подключения ExpressRoute не направляют данные через общедоступный Интернет. Они отличаются повышенной надежностью, более высокой скоростью, меньшей задержкой и дополнительной безопасностью по сравнению с обычными подключениями через общедоступный Интернет.
- [Служебная программа AZCopy](../storage/common/storage-moving-data.md) перемещает данные в службу хранилища Azure через общедоступный Интернет. Этот способ оптимален, если размер данных не превышает 10 ТБ. Для выполнения загрузок на регулярной основе с помощью AZCopy проверьте скорость сети, чтобы просмотреть, подходит ли она. 
- [Фабрика данных Azure (ADF)](../data-factory/introduction.md) включает шлюз, который можно установить на локальном сервере. Затем можно создать конвейер для перемещения данных из локального сервера в службу хранилища Azure. Использование фабрики данных с хранилищем данных SQL описывается в разделе [Загрузка данных в службу "Хранилище данных SQL Azure" с помощью службы "Фабрика данных Azure"](/azure/data-factory/load-azure-sql-data-warehouse).

## <a name="prepare-data"></a>Подготовка данных

Может потребоваться подготовка и очистка данных в учетной записи хранения перед их загрузкой в хранилище данных SQL. Подготовить данные можно, пока они хранятся в источнике, при экспорте данных в текстовые файлы или после того, как данные окажутся в службе хранилища Azure.  Лучше всего как можно раньше начать работу с данными.  

### <a name="define-external-tables"></a>Определение внешних таблиц
Перед загрузкой данных необходимо определить внешние таблицы в хранилище данных. PolyBase использует внешние таблицы для определения данных и доступа к ним в службе хранилища Azure. Внешняя таблица аналогична обычной. Основное отличие в том, что внешняя таблица указывает на данные, хранящиеся за пределами хранилища данных. 

Определение внешних таблиц включает указание источника данных, формата текстовых файлов и определений таблицы. Ниже приведены разделы синтаксиса T-SQL, которые вам понадобятся:
- [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql)
- [CREATE EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql)

Пример создания внешних объектов см. в разделе [Создание внешних таблиц для примеров данных](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) в руководстве по загрузке.

### <a name="format-text-files"></a>Форматирование текстовых файлов

После определения внешних объектов необходимо выровнять строки текстовых файлов с внешней таблицей и определением формата файла. Данные в каждой строке текстового файла должны совпадать с определением таблицы.

Для форматирования текстовых файлов сделайте следующее:

- Если данные поступают из нереляционного источника, необходимо преобразовать их в строки и столбцы. Независимо от того, поступают ли данные из реляционного или нереляционного источника, их необходимо преобразовать для соответствия определениям столбцов таблицы, в которую вы планируете загрузить данные. 
- Форматируйте данные в текстовом файле для соответствия определениям столбцов и типам данных в целевой таблице хранилища данных SQL. Неполное соответствие между типами данных во внешних текстовых файлах и таблице хранилища данных вызовет отклонение строк во время загрузки.
- Отделите поля в текстовом файле символом завершения.  Обязательно используйте уникальный символ или последовательность символов. Используйте указанный символ завершения для [создания формата внешнего файла](/sql/t-sql/statements/create-external-file-format-transact-sql).

## <a name="load-to-a-staging-table"></a>Загрузка в промежуточную таблицу
Чтобы поместить данные в хранилище данных, сначала загрузите их в промежуточную таблицу. С помощью промежуточной таблицы вы можете обрабатывать ошибки без влияния на рабочие таблицы, а также избежать запуска операций отката в рабочей таблице. Промежуточная таблица также предоставляет возможность использовать хранилище данных SQL для запуска преобразований перед вставкой данных в рабочие таблицы.

Чтобы выполнить загрузку с помощью T-SQL, запустите инструкцию T-SQL [CREATE TABLE AS SELECT (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md). Эта команда вставит результаты инструкции select в новую таблицу. После выбора из внешней таблицы с помощью этой инструкции будет выполнен импорт внешних данных. 

В примере ниже ext.Date является внешней таблицей. Все строки импортируются в новую таблицу с именем dbo.Date.

```sql
CREATE TABLE [dbo].[Date]
WITH
( 
    CLUSTERED COLUMNSTORE INDEX
)
AS SELECT * FROM [ext].[Date]
;
```

## <a name="transform-the-data"></a>Преобразование данных
Пока данные находятся в промежуточной таблице, выполните преобразования, необходимые для рабочей нагрузки. Затем переместите данные в рабочую таблицу.

## <a name="insert-data-into-production-table"></a>Вставка данных в рабочую таблицу

С помощью инструкции INSERT INTO... SELECT данные перемещаются из промежуточной таблицы в постоянную. 

При разработке процесса ETL попробуйте запустить его для небольшого тестового примера. Попробуйте извлечь 1000 строк из таблицы в файл, переместить его в Azure, а затем загрузить в промежуточную таблицу. 

## <a name="partner-loading-solutions"></a>Партнерские решения для загрузки
Многие из наших партнеров предлагают решения для загрузки. Дополнительные сведения см. в статье [Партнеры по бизнес-аналитике хранилища данных SQL](sql-data-warehouse-partner-business-intelligence.md). 

## <a name="next-steps"></a>Дополнительная информация
Инструкции по загрузке см. [здесь](guidance-for-loading-data.md).


