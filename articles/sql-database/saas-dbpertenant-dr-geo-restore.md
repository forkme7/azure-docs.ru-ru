---
title: Аварийное восстановление приложений SaaS с применением геоизбыточных резервных копий базы данных SQL Azure | Документация Майкрософт
description: Узнайте, как использовать геоизбыточные резервные копии базы данных SQL Azure для восстановления мультитенантных приложений SaaS в случае сбоя
keywords: руководство по базе данных sql
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: saas apps
ms.topic: article
ms.date: 04/16/2018
ms.author: ayolubek
ms.openlocfilehash: a677e6eb583e293f83df824804aa4cd6f8f5d778
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="recover-a-multi-tenant-saas-application-using-geo-restore-from-database-backups"></a>Восстановление мультитенантного приложения SaaS с помощью геовосстановления из резервных копий базы данных

В этом руководстве вы изучите полный сценарий аварийного восстановления мультитенантных приложений SaaS, реализованный с использованием модели однотенантных баз данных. Вы используете [_геовосстановление_](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-recovery-using-backups) для восстановления баз данных каталогов и клиентов из автоматически сохраняемых геоизбыточных резервных копий в альтернативный регион восстановления. После устранения сбоя для репатриации измененных баз данных в их исходный регион используется [_георепликация_](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-overview).

![архитектура геовосстановления](media/saas-dbpertenant-dr-geo-restore/geo-restore-architecture.png)

Геовосстановление — это наименее затратное решение для аварийного восстановления баз данных SQL.  Тем не менее восстановление из геоизбыточных резервных копий может привести к потере данных за время до одного часа и может занять значительное время — в зависимости от размера каждой базы данных. **Чтобы восстанавливать приложения с наименьшими возможными RPO и RTO, используйте георепликацию вместо геовосстановления**.

В этом руководстве рассматриваются рабочие процессы восстановления и репатриации. Вы узнаете, как выполнять следующие задачи:
> [!div class="checklist"]

>* Синхронизировать сведения о конфигурации базы данных и эластичного пула с каталогом клиента.
>* Настраивать среды зеркалирования в регионе "восстановления", включая приложение, серверы и пулы.    
>* Восстанавливать базы данных каталога и клиента с использованием _геовосстановления_.
>* Репатриировать каталог и измененные базы данных клиента с помощью _георепликации_ после устранения сбоя.
>* Обновлять каталог после восстановления (или репатриации) каждой базы данных для отслеживания текущего расположения активной копии базы данных каждого клиента.
>* Обеспечивать постоянное размещение приложения и базы данных клиента в одном регионе Azure для уменьшения задержки.  
 

Прежде чем приступить к работе с этим руководством, выполните следующие предварительные требования:
* Разверните приложение SaaS Wingtip Tickets, использующее одну базу данных на клиент. См. инструкции из руководства по быстрому [развертыванию SaaS-приложения Wingtip Tickets c однотенантной БД](saas-dbpertenant-get-started-deploy.md).  
* Установите Azure PowerShell. Дополнительные сведения см. в статье [Начало работы с Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-geo-restore-recovery-pattern"></a>Введение в шаблон восстановления с использованием шаблона геовосстановления

Аварийное восстановление является важным аспектом для многих приложений с точки зрения соответствия требованиям или непрерывности бизнес-процессов. В случае длительного сбоя службы хорошо подготовленный план аварийного восстановления может свести к минимуму нарушение бизнес-процессов. План аварийного восстановления на основе геовосстановления должен предусматривать несколько целей:
 * Резервировать все необходимые ресурсы в выбранном регионе восстановления как можно скорее, чтобы гарантировать, что они будут доступны для восстановления баз данных клиента.
 * Создавать среду восстановления из зеркальных образов, которая будет соответствовать исходной конфигурации пула и базы данных. 
 * Если исходный регион возвращается в режим онлайн, необходима возможность отменить текущий процесс восстановления.
 * Быстро включать подготовку клиента, чтобы быстрее перезапустить адаптацию нового клиента.  
 * Обеспечивать восстановление клиентов в порядке приоритета.
 * Гарантировать восстановление работоспособности клиентов в кратчайшие сроки с выполнением шагов параллельно, когда это возможно.
 * Обеспечивать устойчивость к отказам, перезапускаемость и идемпотентность.
 * После устранения сбоя выполнять репатриацию баз данных в их исходный регион с минимальными последствиями для клиентов.  

> [!Note]
> Приложение восстанавливается в _парном регионе_ региона, в котором оно развернуто. Дополнительные сведения см. в статье [Непрерывность бизнес-процессов и аварийное восстановление в службах BizTalk: пары регионов Azure](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions).   



В этом руководстве эти проблемы решаются с помощью возможностей базы данных SQL Azure и платформы Azure:

* [Шаблоны Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template) для максимально быстрого резервирования необходимой емкости. Шаблоны Azure Resource Manager используются для подготовки зеркального образа исходных серверов и эластичных пулов в регионе восстановления. Для подготовки новых клиентов также создаются отдельные сервер и пул. 
* [Клиентская библиотека эластичной базы данных](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-elastic-database-client-library) (EDCL) — создание и обслуживание каталога клиентской базы данных.  Каталог расширен, чтобы включить в себя периодически обновляемые сведения о конфигурации пула и базы данных.
* [Функции восстановления управления сегментами](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-recovery-manager) EDCL — для изменения записей расположения базы данных в каталоге во время восстановления и репатриации.  
* [Геовосстановление](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery) — восстановление баз данных каталогов и клиентов из автоматически сохраняемых геоизбыточных резервных копий. 
* [Асинхронные операции восстановления](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) отправляются согласно порядку клиентов, которые формируются системой в очередь для каждого пула, а их обработка выполняется пакетами, чтобы не перегружать пул. Эти операции при необходимости можно отменить до или во время выполнения.    
* [Георепликация](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) — репатриация баз данных в исходную область после сбоя. Использование георепликации гарантирует отсутствие потери данных и минимальное влияние на клиента.
* [DNS-псевдонимы сервера SQL](https://docs.microsoft.com/azure/sql-database/dns-alias-overview) также используются, чтобы позволить процессу синхронизации каталога подключаться к активному каталогу, независимо от его расположения.  

## <a name="get-the-disaster-recovery--scripts"></a>Получение скриптов аварийного восстановления 

Скрипты восстановления, используемые в этом руководстве, доступны в [репозитории WingtipTicketsSaaS-DbPerTenant на сайте GitHub](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant). Ознакомьтесь с инструкциями по скачиванию и разблокированию скриптов управления Wingtip Tickets в статье [Общие рекомендации по работе с примерами приложений SaaS Wingtip Tickets](saas-tenancy-wingtip-app-guidance-tips.md).
> [!IMPORTANT]
> Как и все скрипты управления Wingtip Tickets, скрипты аварийного восстановления являются образцом качества и не должны использоваться в рабочей среде.   

## <a name="review-the-healthy-state-of-the-application"></a>Проверка состояния работоспособности приложения
Перед тем как начать процесс восстановления, проверьте состояние работоспособности приложения.
1. В веб-браузере откройте концентратор событий Wingtip Tickets: http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net. Замените &lt;user&gt; именем пользователя развертывания.
    * Прокрутите страницу вниз и обратите внимание на имя и расположение сервера каталогов в нижнем колонтитуле.  Расположение — это регион, в котором вы развернули приложение.    
        Совет: наведите указатель мыши на расположение, чтобы увеличить область отображения.

    ![Работоспособное состояние концентратора событий в исходной области](media/saas-dbpertenant-dr-geo-restore/events-hub-original-region.png)

1. Щелкните клиент Contoso Concert Hall и откройте его страницу событий.
    * В нижнем колонтитуле обратите внимание на имя сервера клиента. Расположение будет таким же, как и у сервера каталога.

    ![Исходный регион Contoso Concert Hall](media/saas-dbpertenant-dr-geo-restore/contoso-original-location.png)    
1. На [портале Azure](https://portal.azure.com) найдите и откройте группу ресурсов, в которой было развернуто приложение.
    * Обратите внимание на ресурсы и регион развертывания компонентов службы приложений и серверов баз данных SQL.

## <a name="sync-tenant-configuration-into-catalog"></a>Синхронизация сведений о конфигурации клиента с каталогом

В этой задаче вы запускаете процесс синхронизации конфигурации серверов, эластичных пулов и баз данных с каталогом клиентов.  Эта информация используется позднее для настройки среды зеркалирования в регионе восстановления.

> [!IMPORTANT]
> Для простоты процесс синхронизации и другие длительные процессы восстановления и репатриации реализуются в этих примерах в виде локальных заданий или сеансов PowerShell, которые выполняются под именем входа пользователя вашего клиента. Срок действия токенов проверки подлинности, выданных при входе в систему, истекает через несколько часов, а задания завершатся ошибкой. В рабочем сценарии длительные процессы должны быть реализованы как надежные службы Azure, работающие под управлением субъекта-службы. Дополнительные сведения см. в статье [Использование Azure PowerShell для создания субъекта-службы с сертификатом](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

1. В _интегрированной среде сценариев PowerShell_ откройте файл ...\Learning Modules\UserConfig.psm1. Замените `<resourcegroup>` и `<user>` в строках 10 и 11 значениями группы ресурсов и пользователя, используемыми при развертывании приложения.  Сохраните файл!

2. В *интегрированной среде сценариев PowerShell* откройте скрипт ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1.
    *  В этом руководстве будет выполняться каждый из сценариев в скрипте PowerShell, поэтому не закрывайте этот файл.

3. Задайте следующие параметры:
    * **$DemoScenario = 1**. Запуск фонового задания, которое синхронизирует данные о конфигурации сервера клиента и пула с каталогом.

3. Нажмите клавишу **F5** для запуска скрипта синхронизации. 
    *  Эта информация используется позднее, чтобы убедиться, что восстановления создает зеркальный образ баз данных, пулов и серверов в регионе восстановления.  
![Процесс синхронизации](media/saas-dbpertenant-dr-geo-restore/sync-process.png)

Оставьте окно PowerShell в фоновом режиме и продолжите работу с руководством.

> [!Note]
> Процесс синхронизации подключается к каталогу с помощью псевдонима DNS. Этот псевдоним изменяется во время восстановления и репатриации, чтобы указать на активный каталог. Этот процесс поддерживает актуальность каталога, синхронизируя изменения конфигурации базы данных или пула, внесенные в регионе восстановления.  Во время репатриации эти изменения применяются к эквивалентным ресурсам в исходном регионе.

## <a name="geo-restore-recovery-process-overview"></a>Общие сведения о процессе геовосстановления

В процессе геовосстановления приложение развертывается и базы данных восстанавливаются из резервных копий в регионе восстановления.

Процесс восстановления выполняет следующие задачи.

1. Отключает конечную точку диспетчера трафика для веб-приложения в исходном регионе. Отключение конечной точки предотвращает подключение пользователей к приложению в нерабочем состоянии, если исходный регион вернется к работе в сети во время восстановления.

1. Подготавливает сервер каталогов восстановления в регионе восстановления, выполняет геовосстановление баз данных каталогов и обновляет псевдоним _activecatalog_, чтобы он указывал на восстановленный сервер каталогов.  
    * Изменение псевдонима каталога гарантирует, что синхронизируется всегда именно активный каталог.

1. Помечает все существующие клиенты в каталоге восстановления как автономные, чтобы предотвратить доступ к базам данных клиентов, прежде чем они будут восстановлены.

1. Подготавливает экземпляр приложения в регионе восстановления и настраивает его на использование восстановленного каталога в таком регионе.
    * Чтобы свести задержки к минимуму, эталонное приложение разработано таким образом, чтобы оно всегда подключалось к базе данных клиента в том же регионе.

1. Подготавливает сервер и эластичный пул, в котором будут подготовлены новые клиенты. Создание этих ресурсов гарантирует, что подготовка новых клиентов не будет препятствовать восстановлению существующих клиентов.

1. Обновляет псевдоним нового клиента, чтобы он указывал на сервер для новых баз данных клиентов в регионе восстановления. Изменение этого псевдонима гарантирует, что базы данных для любых новых клиентов будут подготовлены в регионе восстановления.
        
1. Подготавливает серверы и эластичные пулы в регионе восстановления для восстановления баз данных клиентов. Эти серверы и кластеры являются зеркальными образами конфигурации в исходном районе.  Заблаговременная подготовка пулов резервирует ресурсы, необходимые для восстановления всех баз данных.
    * Сбой в регионе может создать значительную нагрузку на ресурсы, доступные в парном регионе.  Если для аварийного восстановления используется геовосстановление, рекомендуется прибегнуть к быстрому резервированию ресурсов. Рекомендуем использовать георепликацию, если крайне важно, чтобы приложения были восстановлены в определенном регионе. 

1. Включает конечную точку диспетчера трафика для веб-приложения в регионе восстановления. Это позволяет приложению подготавливать новые клиенты. На этом этапе существующие клиенты остаются в автономном режиме.

1. Отправляет пакеты запросов на восстановление баз данных в порядке приоритета. 
    * Пакеты упорядочены таким образом, чтобы восстановления баз данных не выполнялись параллельно во всех пулах.  
    * Запросы на восстановление отправляются асинхронно, поэтому отправка происходит быстро, а запросы формируют очередь на выполнение в каждом пуле.
    * Так как запросы на восстановление во всех пулах обрабатываются параллельно, лучше распределить важные клиенты по нескольким пулам. 

1. Отслеживает службу базы данных SQL, чтобы определить момент восстановления баз данных. После восстановления базы данных клиента она помечается в каталоге как подключенная и регистрируется сумма rowversion базы данных клиента. 
    * Приложение сможет получить доступ к базам данных клиентов, как только они будут помечены в каталоге с состоянием "В сети".
    * Сумма значений rowversion в базе данных клиента сохраняется в каталоге. Эта сумма выступает как отпечаток, который позволяет процессу репатриации определить, была ли база данных обновлена ​​в регионе восстановления.      

## <a name="run-the-recovery-script"></a>Запустите скрипт восстановления

> [!IMPORTANT]
> В этом руководстве рассматривается восстановление баз данных из геоизбыточных резервных копий. Несмотря на то что резервные копии обычно доступны в течение 10 минут, может пройти до одного часа, прежде чем они станут доступными. Скрипт будет выполняться, пока они не будут доступными.  Пора заняться кофе!

Теперь представьте, что в регионе, в котором развертывается приложение, произошел сбой, и запустите скрипт восстановления:

1. В *интегрированной среде сценариев PowerShell* откройте скрипт ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 и установите следующие значения.
    * **$DemoScenario = 2**. Восстановление приложения в регионе восстановления из геоизбыточных резервных копий.

1. Нажмите клавишу **F5** для запуска скрипта.  
    * Скрипт открывается в новом окне PowerShell, а затем запускает ряд заданий PowerShell, которые выполняются параллельно.  Эти задания восстанавливают серверы, пулы и базы данных в регионе восстановления. 
    * Регион восстановления представляет собой _парный регион_, связанный с регионом Azure, в котором вы развернули приложение. Дополнительные сведения см. в статье [Непрерывность бизнес-процессов и аварийное восстановление в службах BizTalk: пары регионов Azure](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions). 

1. Отслеживайте состояние процесса восстановления в окне PowerShell.

    ![процесс восстановления](media/saas-dbpertenant-dr-geo-restore/dr-in-progress.png)

>Чтобы изучить код заданий восстановления, просмотрите скрипты PowerShell в папке ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\RecoveryJobs.

## <a name="review-the-application-state-during-recovery"></a>Проверка состояния приложения во время восстановления
Когда конечная точка приложения отключена в диспетчере трафика, приложение недоступно. После восстановления каталога все клиенты помечаются как отключенные.  После этого включается конечная точка приложения в регионе восстановления и приложение слова будет подключено. Несмотря на то что приложение доступно, клиент отключен в концентраторе событий до восстановления своих баз данных. Важно, чтобы ваше приложение обрабатывало автономные базы данных клиентов.

1. После восстановления базы данных каталогов, но до переподключения клиентов к сети обновите концентратор событий Wingtip Tickets Events Hub в веб-браузере.
    * В нижнем колонтитуле обратите внимание, что имя сервера каталогов теперь имеет суффикс _-recovery_ и находится в регионе восстановления.
    * Обратите внимание, что клиенты, которые еще не восстановлены, отмечены как автономные и не могут быть выбраны.   
 
    ![процесс восстановления](media/saas-dbpertenant-dr-geo-restore/events-hub-tenants-offline-in-recovery-region.png)    

    * При открытии страницы событий клиента непосредственно, когда клиент отключен от сети, на странице отображается уведомление об автономном режиме для клиента. Например, если Contoso Concert Hall находится в автономном режиме, попробуйте открыть ![процесс восстановления](media/saas-dbpertenant-dr-geo-restore/dr-in-progress-offline-contosoconcerthall.png) http://events.wingtip-dpt.&lt;пользователь&gt;.trafficmanager.net/contosoconcerthall

## <a name="provision-a-new-tenant-in-the-recovery-region"></a>Подготовка нового клиента в регионе восстановления
Даже перед восстановлением баз данных клиентов вы можете подготовить новые клиенты в регионе восстановления. Новые базы данных клиентов, подготовленные в регионе восстановления, будут репатриированы с восстановленными базами данных позже.   

1. В *интегрированной среде сценариев PowerShell* откройте скрипт ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 и установите следующее свойство.
    * **$DemoScenario = 3**. Подготовка нового клиента в регионе восстановления.

1. Нажмите клавишу **F5** для запуска скрипта. 

1. По завершении подготовки в браузере откроется страница событий Hawthorn Hall. 
    * Обратите внимание, что база данных Hawthorn Hall расположена в регионе восстановления.
    ![Подготовка Hawthorn Hall в регионе восстановления](media/saas-dbpertenant-dr-geo-restore/hawthorn-hall-provisioned-in-recovery-region.png)

1. В браузере обновите страницу концентратора событий Widtip Tickets, чтобы увидеть добавленную страницу Hawthorn Hall. 
    * Если вы подготовили Hawthorn Hall, не дожидаясь восстановления других клиентов, эти клиенты могут оставаться в автономном режиме.

## <a name="review-the-recovered-state-of-the-application"></a>Проверка восстановленного состояния приложения

Когда процесс восстановления завершается, приложение и все клиенты будут полностью работоспособны в регионе восстановления. 

1. Как только в окне консоли PowerShell будет указано, что все клиенты восстановлены, обновите концентратор событий.  Клиенты вернутся в оперативный режим, включая новый клиент Hawthorn Hall.

    ![восстановленные и новые клиенты в концентраторе событий](media/saas-dbpertenant-dr-geo-restore/events-hub-with-hawthorn-hall.png)

1. Щелкните Contoso Concert Hall и откройте его страницу событий.
    * В нижнем колонтитуле обратите внимание, что база данных находится на сервере восстановления, расположенном в регионе восстановления.

    ![Contoso в регионе восстановления](media/saas-dbpertenant-dr-geo-restore/contoso-recovery-location.png)

1. На [портале Azure](https://portal.azure.com) перейдите к списку групп ресурсов.  
    * Обратите внимание на группу ресурсов, которую вы развернули, а также группу ресурсов восстановления с суффиксом _-recovery_.  Группа ресурсов восстановления содержит все ресурсы, созданные во время процесса восстановления, а также новые ресурсы, созданные во время сбоя.  

1. Откройте группу ресурсов восстановления и обратите внимание на следующие элементы:
    * Версии восстановления каталога и серверов tenants1 с суффиксом _-recovery_.  Восстановленные базы данных каталога и клиентов на этих серверах имеют имена, используемые в исходном регионе.

    * SQL Server _tenants2-dpt-&lt;пользователь&gt;-recovery_.  Этот сервер используется для подготовки новых клиентов во время сбоя.
    *   Служба приложений с именем _events-wingtip-dpt-&lt;recoveryregion&gt;-&lt;пользователь&gt_, которая является экземпляром восстановления приложения Events. 

    ![Contoso в регионе восстановления](media/saas-dbpertenant-dr-geo-restore/resources-in-recovery-region.png)   
    
1. Откройте SQL Server _tenants2-dpt-&lt;пользователь&gt;-recovery_.  Обратите внимание, что он содержит базу данных _hawthornhall_ и эластичный пул _Pool1_.  База данных _hawthornhall_ настроена как эластичная база данных в эластичном пуле _Pool1_.

## <a name="change-tenant-data"></a>Изменение данных клиента 
В этой задаче вы обновляете одну из восстановленных баз данных клиентов. Процесс репатриации скопирует восстановленные базы данных, которые были изменены в исходном регионе. 

1. В браузере найдите события для Contoso Concert Hall, просмотрите список событий до последнего события, _Seriously Strauss_.

1. В *интегрированной среде сценариев PowerShell* откройте скрипт ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 и установите следующее значение.
    * **$DemoScenario = 4**. Удаление события из клиента в регионе восстановления.
1. Нажмите клавишу **F5**, чтобы выполнить скрипт.
1. Обновите страницу событий Contoso Concert Hall (http://events.wingtip-dpt.&lt;пользователь&gt;.trafficmanager.net/contosoconcerthall) и обратите внимание, что событие Seriously Strauss отсутствует.

На этом этапе в руководстве уже восстановлено приложение, которое выполняется в регионе восстановления.  Новый клиент подготовлен в регионе восстановления, а также подготовлены измененные данные одного из восстановленных клиентов.  

> [!Note]
> Другие руководства в этом примере не предназначены для работы с приложением в состоянии восстановления. Если вы хотите изучить другие руководства, предварительно убедитесь, что приложение репатриировано.

## <a name="repatriation-process-overview"></a>Обзор процесса репатриации

После устранения сбоя процесс репатриации возвращает приложение и его базы данных в его исходный регион.

![Репатриация геовосстановления](media/saas-dbpertenant-dr-geo-restore/geo-restore-repatriation.png) 


Процесс:

1. Останавливает любые текущие процессы восстановления и отменяет все необработанные или активные запросы на восстановление баз данных.

1. Реактивирует клиентские базы данных в исходном регионе, которые не были изменены с момента сбоя.  Сюда входят базы данных, которые еще не восстановлены, и базы данных, которые были восстановлены, но не изменены позже. Реактивированные базы данных будут точно такими же, как в момент последнего доступа к ним их клиентов.

1. Подготавливает зеркальный образ сервера и эластичного пула новых клиентов в исходном регионе. После завершения этого действия псевдоним нового клиента обновляется и указывает уже на этот сервер. Обновление псевдонима вызывает адаптацию нового клиента в первоначальном регионе вместо региона восстановления.

1. При использовании георепликации каталог перемещается из региона восстановления в исходный регион.

1. Обновляет конфигурацию пула в исходном регионе, поэтому они будут согласованы с изменениями, внесенными в регионе восстановления за время сбоя.

1. Создает необходимые серверы и пулы для размещения новых баз данных, созданных во время сбоя.

1. При использовании георепликации выполняется репатриация восстановленных баз данных клиента, которые были обновлены после восстановления, а все новые базы данных клиентов подготавливаются во время сбоя. 

1. Очищает ресурсы, созданные в регионе восстановления во время восстановления.

Чтобы ограничить количество клиентских баз данных, которые должны быть репатриированы, шаги 1–3 выполняются по запросу.  

Шаг 4 выполняется только в том случае, если каталог в регионе восстановления был изменен во время сбоя. Каталог обновляется после создания новых клиентов или после какого-либо изменения конфигурации базы данных или пула в регионе восстановления.

Очень важно, что шаг 7 вызывает минимальное нарушение работы для клиентов, и данные не теряются. Для этого в процессе используется _георепликация_.

Перед георепликацией каждой базы данных соответствующая база данных в исходном регионе удаляется. База данных в регионе восстановления затем будет геореплицирована — когда создается вторичная реплика в исходном регионе. После завершения репликации клиент помечается как отключенный в каталоге, что приводит к отключению всех соединений с базой данных в регионе восстановления. База данных затем проходит отработку отказа, что вызывает обработку всех отложенных транзакций на сервере-получателе, а данные при этом не теряются. При отработке отказа роли базы данных меняются.  База данных-получатель в исходном регионе становится основной базой данных чтения и записи, а база данных в регионе восстановления становится доступной только для чтения вторичной репликой. Запись клиента в каталоге обновляется для ссылки на базу данных в исходном регионе, и клиент помечается как подключенный.  На этом этапе репатриация базы данных завершена. 

В приложениях должна быть предусмотрена логика повторных попыток, чтобы они автоматически переподключались после разрыва соединения.  При очередном подключении с помощью каталога для выбора соединения они подключаются к репатриированной базе данных в исходном регионе. Хотя недолгое отключение часто не существенно, вы можете запланировать репатриацию баз данных в нерабочее время. 

После репатриации базы данных базу данных-получатель в регионе восстановления удалить нельзя. Для обеспечения защиты базы данных с помощью аварийного восстановления в исходном регионе необходимо геовосстановление.

На шаге 8 ресурсы в регионе восстановления, включая серверы и пулы восстановления, будут удалены.

## <a name="run-the-repatriation-script"></a>Запуск скрипта репатриации
Теперь давайте представим, что сбой устранен, и запустите скрипт репатриации.

Если вы следовали руководству, скрипт немедленно повторно активирует Fabrikam Jazz Club и Dogwood Dojo в исходном регионе, так как они не изменялись. Затем выполняется репатриация нового клиента, Hawthorn Hall и Contoso Concert Hall, так как были внесены изменения. Скрипт также репатриирует каталог, который был обновлен во время подготовки Hawthorn Hall.
  
1. В *интегрированной среде сценариев PowerShell* откройте скрипт ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 и задайте следующее значение.

1. Убедитесь, что процесс синхронизации каталогов по-прежнему выполняется в экземпляре PowerShell.  При необходимости перезапустите его, задав:
    * **$DemoScenario = 1**. Запуск синхронизации сведений о конфигурации сервера клиента, пула и базы данных в каталоге.
    * Нажмите клавишу **F5** для запуска скрипта.
1.  Затем, чтобы начать процесс репатриации, установите:
    * **$DemoScenario = 5**. Репатриация приложения в исходный регион.
    * Нажмите клавишу **F5**, чтобы запустить скрипт восстановления в новом окне PowerShell.  Репатриация займет несколько минут и может контролироваться в окне PowerShell.
1. Пока выполняется скрипт, обновите страницу концентратора событий (http://events.wingtip-dpt.&lt;пользователь&gt;.trafficmanager.net).
    * Обратите внимание, что все клиенты находятся в оперативном режиме и доступны на протяжении всего процесса.
1. Щелкните Jazz Fabrikam Club, чтобы открыть его. Если этот клиент не был изменен, в нижнем колонтитуле обратите внимание, что этот сервер уже возвращен на исходный сервер.
1. Откройте или обновите страницу событий Contoso Concert Hall и обратите внимание на нижний колонтитул, где сказано, что база данных изначально находится на сервере _-восстановления_.  
1. Обновите страницу событий Contoso Concert Hall после завершения процесса репатриации и обратите внимание, что базы данных теперь находятся в вашем исходном регионе.
1. Снова обновите концентратор событий, откройте Hawthorn Hall и обратите внимание, что база данных также находится в исходном регионе. 

## <a name="clean-up-recovery-region-resources-after-repatriation"></a>Очистка ресурсов региона восстановления после репатриации
После завершения репатриации можно безопасно удалить ресурсы в регионе восстановления. 

> [!IMPORTANT]
> Удалите эти ресурсы незамедлительно, чтобы остановить все выставления счетов за них.

Процесс восстановления создает все ресурсы восстановления в группе ресурсов для восстановления. Процесс очистки удалит эту группу ресурсов и все ссылки на ресурсы из этого каталога. 

1. В *интегрированной среде сценариев PowerShell* откройте скрипт ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 и задайте следующее значение.
    * **$DemoScenario = 6**. Удаление устаревших ресурсов из региона восстановления.

1. Нажмите клавишу **F5** для запуска скрипта.

После очистки скриптов приложение вернется туда, откуда оно было запущено.  На этом этапе необходимо еще раз запустить скрипт или попробовать другие руководства.

## <a name="designing-the-application-to-ensure-app-and-database-are-colocated"></a>Разработка приложения для обеспечения совместного размещения приложения и базы данных 
Приложение разработано так, что оно всегда подключается к экземпляру в том же регионе, что и база данных клиента. Это уменьшает задержку между приложением и базой данных. Такая оптимизация предполагает, что взаимодействие приложения с базой данных происходит чаще, чем взаимодействие между пользователем и приложением.  

Базы данных клиентов могут быть распределены между регионами восстановления и исходными регионами в течение некоторого времени при выполнении репатриации.  Для каждой базы данных приложение ищет регион, в котором размещена база данных, выполняя поиск DNS по имени сервера клиента. В базе данных SQL имя сервера является псевдонимом. Псевдоним имени сервера содержит имя региона. Если приложение не находится в том же регионе, что и база данных, оно перенаправляется к экземпляру в том же регионе, где находится сервер базы данных.  Перенаправление в экземпляр в том же регионе, что и база данных, минимизирует задержку между приложением и базой данных.  

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как выполнять такие задачи:
> [!div class="checklist"]

>* Использование каталога клиента для хранения периодически обновляемой информации о конфигурации, позволяя создавать зеркалируемую среду восстановления в другом регионе.
>* Использование геовосстановления для восстановления баз данных Azure SQL в регионе восстановления.
>* Обновление каталога клиента с учетом расположений восстановленной базы данных клиента.  
>* Использование DNS-псевдонима, чтобы подключить приложение к каталогу клиента без изменения конфигурации.
>* Использование георепликации для репатриации восстановленных баз данных в их исходном регионе после устранения сбоя.

Чтобы узнать, как значительно сократить время, необходимое для восстановления крупномасштабных многопользовательских приложений с помощью георепликации, см. статью [Аварийное восстановление мультитенантных приложений SaaS с использованием георепликации базы данных](saas-dbpertenant-dr-geo-replication.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Дополнительные руководства по работе с приложением SaaS Wingtip](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-wtp-overview#sql-database-wingtip-saas-tutorials)
