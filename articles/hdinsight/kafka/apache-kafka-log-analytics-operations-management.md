---
title: Log Analytics для Apache Kafka в Azure HDInsight | Документация Майкрософт
description: Сведения о том, как использовать Log Analytics для анализа журналов из кластера Apache Kafka в Azure HDInsight.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/30/2018
ms.author: larryfr
ms.openlocfilehash: a373ef5cc71d5ae69c83555dc71525aa2188233e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2018
---
# <a name="analyze-logs-for-apache-kafka-on-hdinsight"></a>Анализ журналов для Apache Kafka в HDInsight

Сведения о том, как использовать Log Analytics для анализа журналов, созданных в Apache Kafka в Azure HDInsight.

## <a name="enable-log-analytics-for-kafka"></a>Включение Log Analytics для Kafka

Инструкции по включению Log Analytics для HDInsight идентичны для всех кластеров HDInsight. Чтобы узнать, как создать и настроить необходимые службы, воспользуйтесь ссылками ниже.

1. Создание рабочей области Log Analytics. Дополнительные сведения см. в документе [Начало работы с рабочей областью Log Analytics](https://docs.microsoft.com/azure/log-analytics).

2. Создание Kafka в кластере HDInsight. Дополнительные сведения см. в документе [Приступая к работе с Apache Kafka (предварительная версия) в HDInsight](apache-kafka-get-started.md).

3. Настройка кластера Kafka для использования Log Analytics. Дополнительные сведения см. в статье об [использовании Log Analytics для мониторинга HDInsight](../hdinsight-hadoop-oms-log-analytics-tutorial.md).

    > [!NOTE]
    > Кроме того, можно настроить кластер для использования Log Analytics с помощью командлета `Enable-AzureRmHDInsightOperationsManagementSuite`. Для использования этого командлета требуется указанная ниже информация.
    >
    > * Имя кластера HDInsight.
    > * Идентификатор рабочей области для Log Analytics. Идентификатор рабочей области можно найти в рабочей области Log Analytics.
    > * Первичный ключ для подключения к Log Analytics. Чтобы определить первичный ключ, выберите свой экземпляр Log Analytics, а затем перейдите на __портал OMS__. На портале OMS выберите __Параметры__, __Подключенные источники__, а затем — __Серверы Linux__.


> [!IMPORTANT]
> Подготовка данных для Log Analytics может занять около 20 минут.

## <a name="query-logs"></a>Журналы запросов

1. На [портале Azure](https://portal.azure.com) выберите рабочую область Log Analytics.

2. Выберите __Поиск по журналам__. Здесь можно выполнять поиск данных, собранных из Kafka. Ниже приведены некоторые примеры поисковых запросов:

    * Использование дискового пространства: `Type=Perf ObjectName="Logical Disk" (CounterName="Free Megabytes")  InstanceName="_Total" Computer='hn*-*' or Computer='wn*-*' | measure avg(CounterValue) by   Computer interval 1HOUR`
    * Загрузка ЦП: `Type:Perf CounterName="% Processor Time" InstanceName="_Total" Computer='hn*-*' or Computer='wn*-*' | measure avg(CounterValue) by Computer interval 1HOUR`
    * Входящих сообщений в секунду: `Type=metrics_kafka_CL ClusterName_s="your_kafka_cluster_name" InstanceName_s="kafka-BrokerTopicMetrics-MessagesInPerSec-Count" | measure avg(kafka_BrokerTopicMetrics_MessagesInPerSec_Count_value_d) by HostName_s interval 1HOUR`
    * Входящих байт в секунду: `Type=metrics_kafka_CL HostName_s="wn0-kafka" InstanceName_s="kafka-BrokerTopicMetrics-BytesInPerSec-Count" | measure avg(kafka_BrokerTopicMetrics_BytesInPerSec_Count_value_d) interval 1HOUR`
    * Исходящих байт в секунду: `Type=metrics_kafka_CL ClusterName_s="your_kafka_cluster_name" InstanceName_s="kafka-BrokerTopicMetrics-BytesOutPerSec-Count" |  measure avg(kafka_BrokerTopicMetrics_BytesOutPerSec_Count_value_d) interval 1HOUR`

    > [!IMPORTANT]
    > Замените значения запроса своими сведениями об определенном кластере. Например, для параметра `ClusterName_s` укажите имя кластера. `HostName_s` должно быть присвоено доменное имя рабочего узла в кластере.

    Кроме того, вы можете ввести `*` для поиска всех типов данных журнала. В настоящее время для запросов доступны следующие журналы:

    | Тип журнала | ОПИСАНИЕ |
    | ---- | ---- |
    | log\_kafkaserver\_CL | Брокер Kafka, server.log |
    | log\_kafkacontroller\_CL | Брокер Kafka, controller.log |
    | metrics\_kafka\_CL | Метрики Kafka JMX |

    ![Окно поиска сведений об использовании ЦП](./media/apache-kafka-log-analytics-operations-management/kafka-cpu-usage.png)
 
 ## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о Log Analytics см. в документе [Начало работы с рабочей областью Log Analytics](../../log-analytics/log-analytics-get-started.md).

Дополнительные сведения о работе с Kafka см. в следующих документах:

 * [Зеркальное отображение Kafka в кластерах HDInsight](apache-kafka-mirroring.md).
 * [Повышение уровня масштабируемости Kafka в HDInsight](apache-kafka-scalability.md).
 * [Использование потоковой передачи Spark с Kafka](../hdinsight-apache-spark-with-kafka.md).
 * [Использование структурированной потоковой передачи Spark с Kafka](../hdinsight-apache-kafka-spark-structured-streaming.md).
