---
title: 'Знакомство с Azure Cosmos DB: API MongoDB | Документация Майкрософт'
description: Узнайте, как можно использовать Azure Cosmos DB для хранения больших объемов документов JSON и выполнения запросов к ним с минимальной задержкой с помощью популярных API-интерфейсов MongoDB OSS.
keywords: что такое MongoDB
services: cosmos-db
author: AndrewHoh
manager: kfile
editor: ''
documentationcenter: ''
ms.assetid: 4afaf40d-c560-42e0-83b4-a64d94671f0a
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: anhoh
ms.openlocfilehash: 11cc9510449c2db93c9fe5115d9d2cc7b4872b84
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/20/2018
---
# <a name="introduction-to-azure-cosmos-db-mongodb-api"></a>Знакомство с Azure Cosmos DB: API MongoDB

[Azure Cosmos DB](../cosmos-db/introduction.md) — это глобально распределенная, многомодельная служба базы данных Майкрософт, необходимая для работы с критически важными приложениями. Эта служба базы данных обеспечивает [готовое к использованию глобальное распределение](distribute-data-globally.md), [гибкое масштабирование пропускной способности и хранилища](partition-data.md) по всему миру, задержки менее 10 миллисекунд на уровне 99-го процентиля, а также гарантированную высокую доступность — все это согласно [ведущим в отрасли соглашениям об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB [автоматически индексирует данные](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) без необходимости управлять схемой и индексом. Так как эта база данных является многомодельной, она поддерживает модели данных документа, "ключ — значение", графа и столбчатые модели данных. 

![Azure Cosmos DB: API MongoDB](./media/mongodb-introduction/cosmosdb-mongodb.png) 

Базы данных Azure Cosmos DB можно использовать как хранилище данных для приложений, написанных для [MongoDB](https://docs.mongodb.com/manual/introduction/). Эта означает, что с помощью существующих [драйверов](https://docs.mongodb.org/ecosystem/drivers/) приложение, написанное для MongoDB, теперь может обмениваться данными с Azure Cosmos DB и использовать базы данных Azure Cosmos DB вместо баз данных MongoDB. Чаще всего, чтобы переключиться с использования MongoDB на Azure Cosmos DB, нужно просто изменить строку подключения. Это позволяет легко создавать и запускать глобально распределенные приложения базы данных MongoDB в облаке Azure с Azure Cosmos DB и [ведущие в отрасли комплексные соглашения об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/cosmos-db), продолжая при этом работать с привычными инструментами для MongoDB.

**Совместимость с MongoDB**: можно применять имеющиеся знания о MongoDB, код приложений и инструменты, так как Azure Cosmos DB реализует протокол коммутации MongoDB 3.4 (версии 5) и поддерживает [конвейер статистической обработки MongoDB](mongodb-feature-support.md#aggregation-pipeline). Кроме того, вы можете разрабатывать приложения с использованием MongoDB и развертывать их в рабочей среде с помощью полностью управляемой глобально распределенной службы Azure Cosmos DB.

## <a name="what-is-the-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>Преимущества использования базы данных Azure Cosmos для приложений MongoDB

**Гибко масштабируемые хранилище и пропускная способность**. База данных MongoDB легко масштабируется в соответствии с потребностями вашего приложения. Ваши данные хранятся на твердотельных накопителях (SSD), что обеспечивает прогнозируемо низкие задержки. Azure Cosmos DB поддерживает коллекции MongoDB, которые можно масштабировать, достигая практически неограниченных размеров хранилища и подготовленной пропускной способности. Вы можете гибко масштабировать Azure Cosmos DB с предсказуемым уровнем производительности по мере увеличения объема данных приложения. 

**Репликация между несколькими регионами.** Azure Cosmos DB прозрачно реплицирует данные во все регионы, связанные с вашей учетной записью MongoDB. Это позволяет разрабатывать приложения, которым требуется глобальный доступ к данным, обеспечивая при этом баланс между согласованностью, доступностью и производительностью с соответствующими гарантиями. Эта служба обеспечивает прозрачную отработку отказа между регионами с помощью API-интерфейсов с поддержкой множественной адресации, а также возможность гибко масштабировать пропускную способность и размер хранилища по всему миру. Дополнительные сведения см. в статье [DocumentDB — глобально распределенная служба базы данных в Azure](distribute-data-globally.md).

**Отсутствие сервера управления.** Управление и масштабирование базы данных MongoDB не требуется. Azure Cosmos DB — это полностью управляемая служба. Это означает, что вам не нужно самостоятельно управлять инфраструктурой или виртуальными машинами. Azure Cosmos DB доступна более чем в 30 [регионах Azure](https://azure.microsoft.com/regions/services/).

**Настраиваемые уровни согласованности.** В Azure Cosmos DB сейчас реализована версия MongoDB 3.4 с двумя параметрами согласованности: строгой и итоговой. Так как Azure Cosmos DB поддерживает несколько API, параметры согласованности применяются на уровне учетной записи. Этот процесс контролируется каждым API. В версиях ниже MongoDB 3.6 отсутствовало понятие согласованности сеансов. Поэтому, если вы настроили учетную запись API MongoDB для использования согласованности сеансов, уровень согласованности при использовании API MongoDB понижается до итогового. Если для учетной записи API MongoDB требуется гарантия чтения собственных записей, для согласованности по умолчанию следует установить строгий уровень или уровень ограниченного устаревания. Дополнительные сведения см. в статье [Настраиваемые уровни согласованности данных в DocumentDB](consistency-levels.md).

| Уровни согласованности данных по умолчанию в Azure Cosmos DB |   Mongo API (3.4) |
|---|---|
|Уровень согласованности Eventual (в конечном счете)| Уровень согласованности Eventual (в конечном счете) |
|Согласованность префиксов| Итоговый уровень с согласованным порядком |
|Сеанс| Итоговый уровень с согласованным порядком |
|Ограниченное устаревание| Уровень согласованности Strong (сильная) |
| Уровень согласованности Strong (сильная) | Уровень согласованности Strong (сильная) |

**Автоматическая индексация.** По умолчанию Azure Cosmos DB автоматически индексирует все свойства в документах базы данных MongoDB, не ожидая и не требуя наличия схемы и создания дополнительных индексов. Кроме того, использование уникальных индексов позволяет применить ограничение уникальности к любым полям документа, которые уже автоматически проиндексированы в Azure Cosmos DB.

**Корпоративный класс.** Azure Cosmos DB поддерживает несколько локальных реплик, гарантируя высокий уровень доступности (99,99 %) и защиту данных в случае локальных и региональных сбоев. Azure Cosmos DB имеет [сертификаты соответствия требованиям](https://www.microsoft.com/trustcenter) и функции обеспечения безопасности корпоративного класса. 

Дополнительные сведения см. в этом видео со старшим программистом Azure Cosmos DB Алексеем Саватеевым.

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T136/player]
> 

## <a name="how-to-get-started"></a>Как приступить к работе

Пройдите краткие руководства по MongoDB, чтобы создать учетную запись Azure Cosmos DB и перенести существующее приложение MongoDB в Azure Cosmos DB (либо создать новое):

* [Перемещение имеющегося веб-приложения MongoDB Node.js](create-mongodb-nodejs.md)
* [Создание веб-приложения API MongoDB с использованием языка .NET и портала Azure](create-mongodb-dotnet.md)
* [Создание консольного приложения API MongoDB с использованием языка Java и портала Azure](create-mongodb-java.md)

## <a name="next-steps"></a>Дополнительная информация

Материалы по API для MongoDB в Azure Cosmos DB интегрированы в общую документацию по Azure Cosmos DB, но ниже приводится несколько ссылок, которые помогут вам приступить к работе.

* Инструкции по получению сведений о строке подключения для учетной записи MongoDB см. в [этом руководстве](connect-mongodb-account.md).
* Сведения о создании подключения между базой данных Azure Cosmos DB и приложением MongoDB в Studio 3T см. в статье [Использование Studio 3T (MongoChef) с Azure Cosmos DB](mongodb-mongochef.md).
* При импорте данных в API для базы данных MongoDB следуйте инструкциям в статье [Перенос данных в DocumentDB с помощью mongoimport и mongorestore](mongodb-migrate.md).
* Подключитесь к учетной записи API для MongoDB с помощью [Robomongo](mongodb-robomongo.md).
* С помощью [команды GetLastRequestStatistics и метрик портала Azure](set-throughput.md#GetLastRequestStatistics) узнайте, сколько единиц запроса (ЕЗ) используют ваши операции.
* Узнайте, как [настроить параметры чтения для глобально распределенных приложений](../cosmos-db/tutorial-global-distribution-mongodb.md).
