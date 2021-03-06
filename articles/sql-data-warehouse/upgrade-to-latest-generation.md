---
title: Обновление до последнего поколения хранилища данных SQL Azure | Документация Майкрософт
description: Обновление хранилища данных SQL Azure до последнего поколения аппаратного обеспечения и архитектуры службы хранилища Azure.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 673386ad236f596aa4c64fe2e8c885fb86afe170
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/19/2018
---
# <a name="optimize-performance-by-upgrading-sql-data-warehouse"></a>Оптимизация производительности путем обновления хранилища данных SQL
Обновление хранилища данных SQL Azure до последнего поколения аппаратного обеспечения и архитектуры службы хранилища Azure.

## <a name="why-upgrade"></a>Причины, по которым стоит установить обновление
Сейчас вы можете легко перейти на уровень производительности "Оптимизировано для вычислений" на портале Azure. При наличии оптимизированного для эластичности хранилища данных рекомендуется обновление. После обновления можно будет использовать последнее поколение оборудования Azure и улучшенную архитектуру службы хранилища Azure. Это дает возможность получить повышенную производительность, простую масштабируемость и неограниченное хранение данных в столбцах. 

## <a name="applies-to"></a>Применяется к
Это обновление применяется к хранилищам данных для уровня производительности "Оптимизировано для эластичности".

## <a name="sign-in-to-the-azure-portal"></a>Выполните вход на портал Azure.

Войдите на [портале Azure](https://portal.azure.com/).

## <a name="before-you-begin"></a>Перед началом работы
> [!NOTE]
> Если имеющееся у вас хранилище данных уровня "Оптимизация для эластичности" находится не в том регионе, где доступен этот уровень, с помощью PowerShell вы можете [выполнить георепликацию в регион с поддержкой уровня "Оптимизация для эластичности"](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-restore-database-powershell#restore-from-an-azure-geographical-region).
> 
>

1. Если работа хранилища данных при обновлении с уровня "Оптимизировано для эластичности" приостановлена, [возобновите ее](pause-and-resume-compute-portal.md).
2. Будьте готовы к непродолжительному простою. 



## <a name="start-the-upgrade"></a>Начало обновления

1. Зайдите в хранилище данных уровня "Оптимизация для эластичности" на портале Azure и выберите **Перейти на уровень, оптимизированный для вычислений**: ![Upgrade_1](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_1.png)

2. В разделе "Оптимизировано для эластичности" **выберите предлагаемый по умолчанию уровень производительности** для хранилища данных на основе текущего уровня производительности, используя представленное ниже сопоставление.
    
| Оптимизация для эластичности | Оптимизация для вычислений |
| :----------------------: | :-------------------: |
|      DW100–DW1000      |        DW1000c        |
|          DW1200          |        DW1500c        |
|          DW1500          |        DW1500c        |
|          DW2000          |        DW2000c        |
|          DW3000          |        DW3000c        |
|          DW6000          |        DW6000c        |


3. Перед обновлением убедитесь, что рабочая нагрузка завершена и приостановлена. Несколько минут ресурсы будут недоступны, прежде чем хранилище данных опять подключится в качестве хранилища уровня "Оптимизация для эластичности". Выберите **Обновить**. В настоящее время при использовании предварительной версии уровень производительности "Оптимизировано для вычислений" стоит в половину меньше.
    
    ![Upgrade_2](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_2.png)

4. **Наблюдайте за процессом обновления**, проверяя состояние на портале Azure.

   ![Upgrade3](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_3.png)
   
   Первым шагом процесса обновления является масштабирование ("Обновление — автономный режим"), при котором все сеансы будут прерваны, а соединение разорвано. 
   
   Второй шаг процесса обновления — перенос данных ("Обновление — в сети"). Перенос данных является малозаметным фоновым сетевым процессом, который медленно перемещает данные столбцов из старой архитектуры службы хранилища в новую архитектуру службы хранилища, использующую локальный кэш SSD. В течение этого периода хранилище данных будет подключено к сети и доступно для запросов и загрузки данных. При этом все данные будут доступны для запросов, вне зависимости от того, были ли они перенесены. Перенос данных происходит с разной скоростью в зависимости от их размера, уровня производительности и количества сегментов columnstore. 

5. **Дополнительная рекомендация.** Для ускорения фонового процесса миграции данных мы рекомендуем сразу инициировать передачу данных, выполнив инструкцию [Alter Index rebuild](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-tables-index) для всех таблиц columnstore при наличии широких возможностей единого выхода и оптимального класса ресурсов. Эта операция проводится в автономном режиме в отличие от малозаметного фонового процесса, однако перенос данных будет происходить намного быстрее, после чего вы сможете использовать все преимущества новой улучшенной архитектуры службы хранилища после завершения работы с группами строк высокого качества. 

Следующий запрос формирует необходимые команды Alter Index Rebuild для ускорения процесса переноса данных.

```sql
SELECT 'ALTER INDEX [' + idx.NAME + '] ON [' 
       + Schema_name(tbl.schema_id) + '].[' 
       + Object_name(idx.object_id) + '] REBUILD ' + ( CASE 
                                                         WHEN ( 
                                                     (SELECT Count(*) 
                                                      FROM   sys.partitions 
                                                             part2 
                                                      WHERE  part2.index_id 
                                                             = idx.index_id 
                                                             AND 
                                                     idx.object_id = 
                                                     part2.object_id) 
                                                     > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              ELSE '' 
                                                       END ) + '; SELECT ''[' + 
              idx.NAME + '] ON [' + Schema_name(tbl.schema_id) + '].[' + 
              Object_name(idx.object_id) + '] ' + ( 
              CASE 
                WHEN ( (SELECT Count(*) 
                        FROM   sys.partitions 
                               part2 
                        WHERE 
                     part2.index_id = 
                     idx.index_id 
                     AND idx.object_id 
                         = part2.object_id) > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              + ' completed'';' 
              ELSE ' completed'';' 
                                                    END ) 
FROM   sys.indexes idx 
       INNER JOIN sys.tables tbl 
               ON idx.object_id = tbl.object_id 
       LEFT OUTER JOIN sys.partitions part 
                    ON idx.index_id = part.index_id 
                       AND idx.object_id = part.object_id 
WHERE  idx.type_desc = 'CLUSTERED COLUMNSTORE'; 
```



## <a name="next-steps"></a>Дополнительная информация
Ваше хранилище данных подключено к сети. Чтобы воспользоваться преимуществами новой, усовершенствованной архитектуры, см. [Классы ресурсов для управления рабочей нагрузкой](resource-classes-for-workload-management.md).
 
