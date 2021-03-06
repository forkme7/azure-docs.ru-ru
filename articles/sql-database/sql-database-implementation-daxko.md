---
title: 'Пример использования базы данных SQL Azure: Daxko/CSI | Документация Майкрософт'
description: Узнайте, как компания Daxko/CSI использует Azure для ускорения своего цикла разработки, расширения набора пользовательских услуг и повышения производительности.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: article
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 3779b32aa1397b2ca0e05e2627241c0bfb7a8622
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="daxkocsi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services-and-performance"></a>Компания Daxko/CSI использовала Azure для ускорения своего цикла разработки, расширения набора пользовательских услуг и повышения производительности
![Эмблема Daxko и CSI](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Перед Daxko/CSI Software стояла непростая задача: клиентская база их спортивно-развлекательных центров росла очень быстро благодаря успешному внедрению корпоративного программного решения, но возможности ИТ-инфраструктуры отставали от растущих потребностей, что стало настоящим испытанием для персонала в отделе ИТ компании. Компания все сильнее ощущала нехватку ресурсов из-за увеличивающегося объема операций, особенно по управлению растущими базами данных. Более того, нарастающее число операций отбирало ресурсы разработки, так как возникали новые потребности, такие как внедрение функций мобильности для программного обеспечения компании.

По словам Дэвида Молины (David Molina), директора по разработке продуктов Daxko/CSI, служба Azure предоставила CSI Software модель PaaS (платформа как услуга), которая позволила упростить управление базами данных, увеличить масштабируемость, высвободить ресурсы и сконцентрировать их на работе с ПО вместо обслуживания операций. "База данных SQL стала для нас отличным решением. Не нужно беспокоиться об обслуживании сервера SQL Server, отказоустойчивости кластеров и всех прочих инфраструктурных потребностях. Для нас это идеально".

С момента перехода на Azure компании CSI Software требуется всего два оператора, которые управляют более чем 600 базами данных клиентов. Компания использует пулы эластичных баз данных SQL Azure для переноса баз данных клиентов в зависимости от размера и потребности.

Г-н Молина продолжает: "Наши клиенты сразу почувствовали разницу. До использования пулов эластичных БД они периодически сталкивались со временем ожидания и другими проблемами в периоды пиковых нагрузок. Благодаря пулам эластичных БД Azure они могут увеличивать нагрузки на свое усмотрение и использовать программное обеспечение без каких-либо проблем".

Кроме повышения производительности для клиентов, пулы эластичных баз данных Azure освободили ресурсы CSI Software от необходимости тратить время на обслуживание операций и администрирование. Вместо этого они смогли сосредоточиться на разработке новых услуг и функций. Эти ресурсы ИТ помогли CSI Software улучшить свое корпоративное ПО, SpectrumNG, что помогло привлечь в тренажерные залы больше посетителей, повысило эффективность работы персонала и предоставило сотрудникам и клиентам мобильный доступ к интерактивным задачам и уведомлениям в реальном времени.

Служба Azure также помогла CSI Software ускорить и улучшить процесс разработки и цикл контроля качества (QA) через внедрение функций автоматизации. Благодаря реализации в компании службы Azure диспетчеры сборки могут запаковывать компоненты одним нажатием кнопки. Г-н Молина описывает это так: "В рамках цикла выпуска теперь можно выполнять развертывание QA в тестовой среде Azure, которая очень точно имитирует наш рабочий стек. Мы можем сразу же развертывать сборки в среду разработки для тестирования изменений. Это огромное преимущество для нас, так как раньше мы не имели подобной возможности для тестирования".

## <a name="offloading-to-the-cloud"></a>Разгрузка в облако
До переноса в облако CSI Software успешно создала собственную мультитенантную инфраструктуру в локальном центре обработки данных в Хьюстоне. Но мере разрастания компании она сталкивалась со все новыми проблемами: рост числа закупок и поставок, обслуживание всего оборудования и программного обеспечения, которое необходимо для поддержки клиентов. Нехватка персонала в отделе ИТ для обработки операций стала еще одной проблемой, которая замедлила подготовку новых ресурсов и развертывание новых услуг для клиентов.

Для решения этой проблемы CSI Software рассматривала возможности использования облака. Это бы позволило компании сосредоточиться на работе с кодом, а не на операциях. Они обнаружили, что большинство основных поставщиков облачных решений предлагают только решения IaaS (инфраструктура как услуга), а для управления стеком IaaS по-прежнему требуется много ИТ-персонала. В итоге CSI Software пришла к выводу: решение Azure PaaS лучше всего соответствует нуждам компании. Г-н Молина объясняет это так: "Azure позволяет забыть об оборудовании и системном программном обеспечении, так что мы можем сосредоточиться на наших предложениях ПО и сократить расходы на ИТ-инфраструктуру".

## <a name="making-the-transition-to-azure"></a>Осуществление перехода на Azure
Выбрав Azure в качестве решения PaaS, CSI Software начала переносить в облако свою серверную инфраструктуру и базы данных. До перехода на Azure пользователи SpectrumNG должны были устанавливать клиентское приложение, которое взаимодействовало со службой Windows Communication Foundation (WCF) в серверной части. По словам г-на Молины, "Несмотря на то, что некоторые клиенты размещали все в собственных центрах обработки данных, мы сделали продукт мультитенантным. Мы разместили все в центре обработки данных в Хьюстоне, используя в качестве хранилища данных сервер SQL Server.

Предлагаемый нами продукт также включал в себя веб-портал для участников (написанный в ASP.net), разработанный в рамках партнерской системы White Label для соответствия веб-присутствию клиента, а также интерфейс SOAP API для поддержки веб-страниц и интеграции с любыми продуктами сторонних поставщиков".

При переносе в облако построение архитектуры не заняло много времени. По словам г-на Молины, "Большая часть усилий была направлена на изменение способа считывания данных файла конфигурации, централизованное изменение строки подключения и автоматизацию процессов упаковки, отправки и развертывания наших выпусков".

Для разработки автоматизации сборки инженеры CSI Software использовали Azure PowerShell и REST API, чтобы создать пакеты и передать их в промежуточную среду для выпуска в конце каждого дня.
Весь переход на облачное развертывание Azure прошел для ИТ-команды CSI Software быстро и гладко. Г-н Молина объясняет: "В целом, у нас уже была бета-среда в облаке в пределах 3-4 недель с момента старта проекта. Для нас это было приятным сюрпризом".

После настройки и тестирования среды CSI Software начала осуществлять перенос клиентов. Для клиентов, которые уже использовали размещение в CSI Software, переход был почти незаметным. Для клиентов, переходящих с локального развертывания, процесс перехода в облако занял определенное время, но в целом был безболезненным как для клиентов, так и для CSI Software.

К новым клиентам ИТ-персонал CSI Software применяет следующую процедуру для регистрации их в Azure:

1. Сценарии Azure PowerShell используются для запуска новой базы данных для клиента; сначала всем клиентам предоставляется база данных уровня "Премиум", чтобы обеспечить достаточную пропускную способность для осуществления перехода.
2. Когда это возможно, CSI Software использует мастер миграции SQL Azure для переноса существующих данных в экземпляр Базы данных SQL Azure.
3. Наконец, службы Microsoft SQL Server Integration Services (SSIS) используются для согласования всех несоответствий в данных или для выполнения, при необходимости, очистки данных.

В настоящее время около 99 процентов клиентов CSI Software размещается в Azure — в четырех региональных центрах обработки данных (Северо-центральный регион, Южно-центральный регион, Восток и Запад). Благодаря наличию центров обработки данных во всех географических регионах клиентов задержка сводится к минимуму.

## <a name="azure-elastic-pools-free-up-it-resources"></a>Пулы эластичных баз данных Azure освобождают ресурсы ИТ
Некоторые возможности Azure помогли CSI Software переключиться с обслуживания инфраструктуры и операций на разработку функций и услуг. Вероятно, самым значительным преимуществом стало применение пулов эластичных баз данных.

В данный момент CSI Software предоставляет своим клиентам около 550 баз данных. Без пулов эластичных БД было сложно управлять множеством баз данных в многоуровневой структуре. Операторам приходилось назначать уровни производительности, исходя из максимальных потребностей клиентов, а для этого требовались значительные усилия ИТ-специалистов. С помощью пулов эластичных баз данных операторы могут назначить клиентам пул уровня "Премиум" или "Стандартный", а затем перемещать пользователей в зависимости от размера и потребностей. Клиенты немедленно ощутили эффективность пулов эластичных баз данных. До их применения в пиковые периоды возникали задержки, связанные с повышенным временем ожидания, а также другие проблемы. Благодаря пулам эластичных баз данных они могут спокойно переносить всплески активности и использовать SpectrumNG, не испытывая неудобств.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Активная георепликация Azure ускоряет работу с отчетами
Несколько клиентов CSI Software также используют активную георепликацию Azure. С помощью активной георепликации можно настроить до четырех доступных для чтения баз данных-получателей в одном или разных регионах центров обработки данных. CSI Software использует активную георепликацию двумя способами: во-первых, базы данных-получатели доступны в случае выхода из строя центра обработки данных или невозможности подключения к базе данных-источнику; а во-вторых, базы данных-получатели доступны для чтения и могут использоваться для разгрузки рабочих нагрузок с доступом только для чтения, таких как задания отчетов. Некоторые клиенты CSI Software используют это преимущество для ускорения рабочих процессов отчетности.

## <a name="csi-software-application-logic-and-architecture"></a>Логика и архитектура приложения CSI Software
SpectrumNG использует веб-роли. Так как приложение является мультитенантным, для обработки начальных запросов на подключение, поступающих от клиентов, используется служба WCF. Г-н Молина рассказывает: "Запрос идентифицирует каждого клиента, что позволяет нам создать строку подключения к соответствующим базам данных и сделать все, что нам нужно".

Для веб-уровня своей службы CSI Software использует автоматическое масштабирование Azure, основанное на дате и времени. Доступные ресурсы автоматически увеличиваются для обслуживания большего объема в рабочие часы в соответствии с часовым поясом каждого регионального центра обработки данных. По выходным, когда потребности клиентов ниже, масштаб ресурсов уменьшается.

![Архитектура Daxko и CSI](./media/sql-database-implementation-daxko/figure1.png)

Рисунок 1. Рабочая роль облачных служб использует структурированные данные из Базы данных SQL Azure и частично структурированные данные из хранилища таблиц. Пользователи SpectrumNG взаимодействуют с этими данными посредством веб-роли облачных служб.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Использование веб-приложений и уровня веб-плана для мобильных приложений
Благодаря внедрению Базы данных SQL Azure компании CSI Software удалось высвободить ресурсы и направить их на работу с новыми идеями, включая разработку полной мобильной платформы на основе настраиваемого интерфейса API с размещением в веб-приложениях Azure. Эта платформа позволяет посетителям тренажерных залов и персоналу использовать мобильные устройства для проверки расписаний, записи на занятия и получения сообщений.

Платформа использует сервисноориентированную архитектуру (SOA) для того, чтобы взять один компонент (такой как POS-система или система продаж), в режиме реального времени переместить его в другой веб-план, а затем запустить службу для поддержки этого компонента, оставив все остальное в исходном веб-плане. Эта возможность позволяет CSI Software обеспечить невероятную гибкость и сократить расходы.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Благодаря Azure разработчики CSI Software могут сконцентрироваться на приложениях и услугах
База данных SQL Azure — это не только удобное средство для пользователей SpectrumNG, которые получили быстрое и надежное обслуживание, но и большой успех для ИТ-персонала и разработчиков CSI Software. Разгрузив операции с помощью облака Azure, CSI Software сократила накладные расходы на ресурсы и инфраструктуру, значительно ускорила циклы разработки и больше не нуждается в управлении базами данных на низшем уровне, чтобы оптимизировать производительность для своих клиентов.

## <a name="more-information"></a>Дополнительные сведения
* Дополнительные сведения о пулах эластичных баз данных см. в статье [Что такое пул эластичных БД Azure?](sql-database-elastic-pool.md)
* Дополнительные сведения об инструментах баз данных и эластичном масштабировании см. в статье [Приступая к работе с инструментами эластичных баз данных](sql-database-elastic-scale-get-started.md).
* Дополнительные сведения о переносе базы данных SQL Server в Azure см. в [этой статье](sql-database-cloud-migrate.md).
* Дополнительные сведения об активной георепликации см. в статье [Обзор. Группы отработки отказа и активная георепликация](sql-database-geo-replication-overview.md).
* Дополнительные сведения о служебной шине Azure см. в статье [Служебная шина Azure](https://azure.microsoft.com/services/service-bus/).
* Дополнительные сведения об автомасштабировании см. в статье [Автомасштабирование облачной службы](../cloud-services/cloud-services-how-to-scale-portal.md).

