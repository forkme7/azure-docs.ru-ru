---
title: Синхронизация данных SQL Azure (предварительная версия) | Документация Майкрософт
description: В этом обзоре представлена синхронизация данных SQL Azure (предварительная версия)
services: sql-database
author: douglaslms
manager: craigg
ms.service: sql-database
ms.custom: data-sync
ms.topic: article
ms.date: 04/01/2018
ms.author: douglasl
ms.reviewer: douglasl
ms.openlocfilehash: e66adb8b0485e30fded487e18af6b2030f9c7f5b
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync-preview"></a>Синхронизация данных в нескольких облачных и локальных базах данных с помощью синхронизации данных SQL (предварительная версия)

Синхронизация данных SQL — это служба, созданная на основе базы данных SQL Azure, которая позволяет в двунаправленном режиме синхронизировать выбранные данные в нескольких базах данных SQL и экземплярах SQL Server.

В основе синхронизации данных лежит понятие группы синхронизации. Группа синхронизации — это группа баз данных, которые требуется синхронизировать.

Группа синхронизации имеет следующие свойства.

-   **Схема синхронизации**: описывает, какие данные синхронизируются.

-   **Направление синхронизации**: синхронизация может быть двунаправленной или выполняться в одном направлении. То есть синхронизация может выполняться *из центральной базы данных в член*, *из члена в центральную базу данных* или в обоих этих направлениях.

-   **Интервал синхронизации**: определяет, как часто выполняется синхронизация.

-   **Политика устранения конфликтов** — это политика уровня группы, которая может задавать режим *Выигрывает концентратор* или *Member wins* (Выигрывает член).

Служба синхронизации данных использует звездообразную топологию для синхронизации данных. Одну из баз данных в группе необходимо определить как центральную базу данных. Остальные базы данных являются рядовыми базами данных. Синхронизация происходит только между центральной базой данных и отдельными членами.
-   **Центральной базой данных** должна быть база данных SQL Azure.
-   **Рядовыми базами данных** могут быть базы данных SQL, локальные базы данных SQL Server или экземпляры SQL Server на виртуальных машинах Azure.
-   **База данных синхронизации** содержит метаданные и журнал синхронизации данных. Базой данных синхронизации должна быть база данных SQL Azure, которая находится в том же регионе, что и центральная база данных. База данных синхронизации создается клиентом и принадлежит ему.

> [!NOTE]
> Если вы используете локальную базу данных, необходимо [настроить локальный агент](sql-database-get-started-sql-data-sync.md#add-on-prem).

![Синхронизация данных между базами данных](media/sql-database-sync-data/sync-data-overview.png)

## <a name="when-to-use-data-sync"></a>Когда следует использовать синхронизацию данных

Использовать синхронизацию данных удобно в случаях, когда требуется обеспечить актуальное состояние данных в нескольких базах данных SQL Azure или базах данных SQL Server. Ниже приведены основных варианты использования синхронизации данных.

-   **Гибридная синхронизация данных.** Можно обеспечить синхронизацию данных между локальными базами данных и базами данных SQL Azure, чтобы включить гибридные приложения. Эта возможность может привлечь клиентов, которые рассматривают переход в облако и собираются поместить некоторые из своих приложений в Azure.

-   **Распределенные приложения**. Во многих случаях полезно разделить разные рабочие нагрузки на разные базы данных. Например, если у вас есть большая рабочая база данных, но необходимо также выполнять рабочую нагрузку для составления отчетов и анализа этих данных, удобно иметь вторую базу данных для этих целей. Такой подход сводит к минимуму влияние на производительность производственной рабочей нагрузки. Чтобы синхронизировать эти две базы данных, можно использовать службу синхронизации данных.

-   **Глобально распределенные приложения**. Многие организации охватывают несколько регионов и даже несколько стран. Чтобы свести к минимуму задержки в сети, рекомендуется хранить свои данные в регионе, ближайшем к вам. Используя синхронизацию данных, можно легко обеспечить синхронизацию баз данных в регионах по всему миру.

Синхронизация данных не предназначена для следующих сценариев:

-   Аварийное восстановление

-   масштаб чтения;

-   ETL (OLTP в OLAP);

-   перенос из локальной базы данных SQL Server в базу данных SQL Azure.

## <a name="how-does-data-sync-work"></a>Как работает синхронизация данных? 

-   **Отслеживание изменения данных**. Служба синхронизации данных отслеживают изменения при помощи триггеров вставки, обновления и удаления. Изменения записываются во вспомогательную таблицу в пользовательской базе данных.

-   **Синхронизация данных**. Служба синхронизации данных разработана на основе звездообразной модели. Центральная база данных синхронизируется с каждым членом по отдельности. Изменения в центральной базе данных передаются в член, после чего изменения из члена передаются в центральную базу данных.

-   **Устранение конфликтов**. Служба синхронизации данных предоставляет два параметра для устранения конфликтов, *Выигрывает концентратор* и *Member wins* (Выигрывает член).
    -   Если выбран параметр *Выигрывает концентратор*, то изменения в центральной базе данных всегда записываются поверх изменений в члене.
    -   Если выбран параметр *Member wins* (Выигрывает член), то изменения в члене всегда записываются поверх изменений в центральной базе данных. Если имеется несколько членов, то конечное значение зависит от того, какой член синхронизируется первым.

## <a name="sync-req-lim"></a> Требования и ограничения

### <a name="general-considerations"></a>Общие рекомендации

#### <a name="eventual-consistency"></a>Итоговая согласованность
Так как синхронизация данных основана на триггерах, транзакционная согласованность не гарантируется. Корпорация Майкрософт гарантирует, что все изменения будут внесены в конечном счете и что синхронизация данных не приведет к потере данных.

#### <a name="performance-impact"></a>Влияние на производительность
Служба синхронизация данных использует триггеры вставки, обновления и удаления для отслеживания изменений. Она создает вспомогательные таблицы в пользовательской базе данных для отслеживания изменений. Действия по отслеживанию изменений оказывают влияние на рабочую нагрузку базы данных. Оцените текущий уровень служб и повысьте его, если потребуется.

### <a name="general-requirements"></a>Общие требования

-   Каждая таблица должна иметь первичный ключ. Не изменяйте значение первичного ключа ни в какой строке. Если это необходимо сделать, удалите строку и создайте ее повторно с новым значением первичного ключа. 

-   Необходимо включить изоляцию моментального снимка. Дополнительные сведения см. в статье [Изоляция моментального снимка в SQL Server](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server).

### <a name="general-limitations"></a>Общие ограничения

-   В таблице не должно быть столбцов идентификаторов, которые не являются первичным ключом.

-   Первичный ключ не может иметь тип данных DateTime.

-   Имена объектов (баз данных, таблиц и столбцов) не могут содержать печатаемые символы: точку (.), левую квадратную скобку ([) или правую квадратную скобку (]).

-   Аутентификация Azure Active Directory не поддерживается.

#### <a name="unsupported-data-types"></a>Неподдерживаемые типы данных

-   FileStream;

-   UDT SQL или CLR;

-   XMLSchemaCollection (поддерживается XML);

-   Cursor, Timestamp, Hierarchyid.

#### <a name="limitations-on-service-and-database-dimensions"></a>Ограничения характеристик служб и баз данных

| **Характеристики**                                                      | **Ограничение**              | **Возможное решение**              |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| Максимальное количество групп синхронизации, которым может принадлежать любая база данных       | 5                      |                             |
| Максимальное количество конечных точек в отдельной группе синхронизации              | 30                     | Создайте несколько групп синхронизации. |
| Максимальное количество локальных конечных точек в отдельной группе синхронизации | 5                      | Создайте несколько групп синхронизации. |
| Имена баз данных, таблиц, схем и столбцов                       | Имя может содержать до 50 знаков. |                             |
| Таблицы в группе синхронизации                                          | 500                    | Создайте несколько групп синхронизации. |
| Столбцы в таблице в группе синхронизации                              | 1000                   |                             |
| Размер строки данных в таблице                                        | 24 МБ                  |                             |
| Минимальный интервал синхронизации                                           | 5 мин              |                             |
|||

## <a name="faq-about-sql-data-sync"></a>Вопросы и ответы о синхронизации данных SQL

### <a name="how-much-does-the-sql-data-sync-preview-service-cost"></a>Сколько стоит использование службы синхронизации данных SQL (предварительная версия)?

Во время действия предварительной версии сама по себе служба синхронизации данных SQL (предварительная версия) предоставляется бесплатно.  Однако вам все равно будет начисляться плата за передачу данных при перемещении данных в экземпляр базы данных SQL и из него. Дополнительные сведения см. на странице [цен на базу данных SQL](https://azure.microsoft.com/pricing/details/sql-database/).

### <a name="what-regions-support-data-sync"></a>Какие регионы поддерживают синхронизацию данных?

Синхронизация данных SQL (предварительная версия) доступна во всех регионах, поддерживающих общедоступные облачные службы.

### <a name="is-a-sql-database-account-required"></a>Требуется ли учетная запись базы данных SQL? 

Да. Для размещения центральной базы данных необходимо иметь учетную запись базы данных SQL.

### <a name="can-i-use-data-sync-to-sync-between-sql-server-on-premises-databases-only"></a>Можно ли использовать функцию синхронизации данных только для синхронизации между локальными базами данных SQL Server? 
Не напрямую. Вы можете опосредованно выполнять синхронизацию между локальными базами данных SQL Server. Это можно сделать, создав центральную базу данных в Azure, а затем добавив локальные базы данных в группу синхронизации.
   
### <a name="can-i-use-data-sync-to-seed-data-from-my-production-database-to-an-empty-database-and-then-keep-them-synchronized"></a>Можно ли использовать синхронизацию данных для заполнения данных из рабочей базы данных в пустую базу данных с последующей синхронизацией? 
Да. Вручную создайте схему в новой базе данных из оригинала с помощью скрипта. После создания схемы добавьте таблицы в группу синхронизации, чтобы скопировать данные и обеспечить их синхронизацию.

### <a name="should-i-use-sql-data-sync-to-back-up-and-restore-my-databases"></a>Следует ли использовать синхронизацию данных SQL для резервного копирования и восстановления баз данных?

Использовать синхронизацию данных SQL (предварительная версия) для создания резервной копии данных не рекомендуется. Резервное копирование и восстановление на определенный момент времени невозможно, так как при синхронизации, выполняемой с помощью синхронизации данных SQL (предварительная версия), не поддерживаются версии. Более того, синхронизация данных SQL (предварительная версия) не выполняет резервное копирование других объектов SQL, таких как хранимые процедуры, и не имеет эквивалента быстрой операции восстановления.

Один из рекомендуемых способов резервного копирования описан в статье [Копирование Базы данных SQL Azure](sql-database-copy.md).

### <a name="is-collation-supported-in-sql-data-sync"></a>Поддерживаются ли в синхронизации данных SQL параметры сортировки?

Да. Синхронизация данных SQL поддерживает параметры сортировки в следующих сценариях.

-   Если выбранные таблицы схемы синхронизации еще не содержатся в центральной базе данных или в рядовых базах данных, то при развертывании группы синхронизации служба автоматически создает соответствующие таблицы и столбцы с параметрами сортировки, выбранными в пустых целевых базах данных.

-   Если синхронизируемые таблицы уже существуют как в центральной базе данных, так и в рядовых базах данных, синхронизация данных SQL требует, чтобы столбцы первичного ключа имели одинаковые параметры сортировки в центральной базе данных и рядовых базах данных, для успешного развертывания группы синхронизации. Для столбцов, отличных от столбцов первичного ключа, на параметры сортировки нет ограничений.

### <a name="is-federation-supported-in-sql-data-sync"></a>Поддерживается ли федерация в синхронизации данных SQL?

Корневая база данных федерации может использоваться в службе синхронизации данных SQL (предварительная версия) без каких-либо ограничений. Конечную точку федеративной базы данных невозможно добавить в текущую версию синхронизации данных SQL (предварительная версия).

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о синхронизации данных SQL:

-   [Начало работы с синхронизацией данных SQL Azure (предварительная версия)](sql-database-get-started-sql-data-sync.md)
-   [Best practices for SQL Data Sync (Preview)](sql-database-best-practices-data-sync.md) (Рекомендации по синхронизации данных SQL (предварительная версия))
-   [Мониторинг синхронизации данных SQL Azure с помощью Log Analytics](sql-database-sync-monitor-oms.md)
-   [Устранение неполадок с синхронизацией данных SQL Azure (предварительная версия)](sql-database-troubleshoot-data-sync.md)

-   Полные примеры PowerShell, которые демонстрируют, как настроить синхронизацию данных SQL:
    -   [Использование PowerShell для синхронизации данных между несколькими базами данных SQL Azure](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Использование PowerShell для синхронизации данных между базой данных SQL Azure и локальной базой данных SQL Server](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Документация по REST API синхронизации данных SQL](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Дополнительные сведения о Базе данных SQL:

-   [Обзор Базы данных SQL](sql-database-technical-overview.md)
-   [Управление жизненным циклом базы данных](https://msdn.microsoft.com/library/jj907294.aspx)
