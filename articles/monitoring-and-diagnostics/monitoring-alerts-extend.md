---
title: 'Расширение (копирование) оповещений с портала OMS в Azure: общие сведения | Документация Майкрософт'
description: Общие сведения о процессе копирования оповещений с портала OMS в службу "Оповещения Azure", дополнительные сведения о распространенных проблемах клиентов.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2018
ms.author: vinagara
ms.openlocfilehash: 54ec12f24ddbad6227a306aeae86658807f85b4e
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2018
---
# <a name="extend-copy-alerts-from-oms-portal-into-azure"></a>Расширение (копирование) оповещений с портала OMS в Azure
На портале Operations Management Suite (OMS) отображаются только оповещения Log Analytics.  Новые возможности оповещений теперь интегрированы в различные службы и элементы Microsoft Azure. Новая функция **Оповещения** в Azure Monitor на портале Azure содержит оповещения журнала действий, оповещения метрик и оповещения журнала в Log Analytics и Application Insights. 


Некоторые пользователи работали с Log Analytics и смежными функциями, такими как оповещения, через [портал Microsoft Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). И следовательно, чтобы они могли легко управлять другими ресурсами Azure, используя Log Analytics, корпорация Майкрософт систематически обеспечивала доступность возможностей портала OMS на портале Azure. В связи с этим оповещения Azure уже позволяют пользователям управлять оповещениями на основе запросов Log Analytics. Дополнительные сведения см. в статье [Оповещения журнала в Azure Monitor. Оповещения Azure (предварительная версия)](monitor-alerts-unified-log.md). В службе "Оповещения" в Azure Monitor оповещения, создаваемые на портале OMS, уже отображаются в соответствующей рабочей области журналов аналитики. Однако любое изменение оповещений, созданных на портале OMS, требует от пользователя выхода из Azure и использования портала OMS, а затем возвращения в Azure, если нужно управлять любой другой службой. Чтобы упростить работу, корпорация Майкрософт предоставила пользователям возможность расширения оповещений с портала OMS в Azure.

## <a name="benefits-of-extending-your-alerts"></a>Преимущества расширения оповещений
Помимо отсутствия необходимости перехода с портала Azure есть и другие существенные преимущества в расширении оповещений с портала OMS в Azure.

- В отличие от портала OMS, где можно было создавать и просматривать только 250 оповещений, в оповещениях в Azure это ограничение отсутствует.
- Из оповещений Azure можно перечислить и просматривать все типы оповещений (а также управлять ими), а не только оповещения Log Analytics, как в случае с порталом OMS.
- Вы можете управлять доступом пользователей, ограничив его только функциями мониторинга и оповещений, с использованием [роли Azure Monitor](monitoring-roles-permissions-security.md).
- Оповещения в Azure используют [группы действия](monitoring-action-groups.md), что позволяет воспользоваться несколькими действиями для каждого оповещения, включая SMS, голосовой вызов, Runbook автоматизации, веб-перехватчик, соединитель ITSM и т. д. 

## <a name="process-of-extending-your-alerts"></a>Процесс расширения оповещений
Процесс расширения оповещений с портала OMS в Azure **не** затрагивает изменения определения, запроса или конфигурации оповещения каким-либо образом. Единственное изменение — чтобы в Azure все действия, такие как оповещение по электронной почте, вызов веб-перехватчика, выполнение Runbook автоматизации или подключение средства ITSM, выполнялось через группу действия. Следовательно, если соответствующая группа действий связана с оповещением, они будут расширены в Azure.

Так как процесс расширения является обратимым и непрерывным, корпорация Майкрософт расширит оповещения, созданные на портале OMS, до оповещений Azure автоматически начиная с **14 мая 2018 года**. С этой даты корпорация Майкрософт планирует расширять оповещения в Azure и постепенно реализует возможность управлять всеми оповещениями на портале OMS с портала Azure. 

Когда запланированное расширение оповещений из рабочей области Log Analytics в Azure вступит в силу, они продолжат работу и **не будут** каким-либо образом компрометировать мониторинг. Во время планирования оповещения могут быть временно недоступны для изменения или редактирования, однако в течение этого короткого времени могут быть созданы новые оповещения Azure. Если в течение этого короткого периода будут изменены или созданы оповещения с портала OMS, пользователи смогут продолжить работу в Azure Log Analytics или в службе "Оповещения Azure".

 ![В течение запланированного периода действия пользователя с оповещениями перенаправляются в Azure](./media/monitor-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> Плата за расширение уведомлений с портала OMS в Azure и использование оповещений Azure для запросов на основе журнала Log Analytics не взимается, если это осуществляется в рамках и по условиям, указанным в статье [Цены на Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).  

Пользователи могут воспользоваться преимуществами расширения оповещений до этой даты, добровольно решив сделать свои оповещения управляемыми в Azure.

### <a name="how-to-voluntarily-extending-your-alerts"></a>Как добровольно расширить оповещения
Чтобы обеспечить пользователям OMS простой переход к оповещениям Azure, корпорация Майкрософт создала инструменты, обеспечивающие расширения оповещений. Портал Microsoft OMS позволяет расширять свои оповещения в Azure из мастера на портале OMS (или) программно, используя новый API. Дополнительные сведения см. в статье [Расширение (копирование) оповещений из OMS в Azure](monitoring-alerts-extend-tool.md).


## <a name="usage-after-extending-your-alerts"></a>Использование после расширения оповещений
Как уже говорилось, оповещения, созданные в Microsoft Operation Management Suite, будут расширены в службу "Оповещения Azure", после чего ими можно управлять из Azure. Оповещения будут по-прежнему отображаться на портале OMS в разделе параметров оповещения, как показано ниже:

 ![Список оповещений портала OMS после расширения в Azure](./media/monitor-alerts-extend/PostExtendList.png)

Для любых операций с оповещениями, таких как изменение или создание, выполняемых на портале Azure, пользователь будет прозрачно перенаправляться в службу "Оповещения Azure". 

> [!NOTE]
> Так как пользователи прозрачно перенаправляется в Azure, при любом действии добавления или изменения оповещения в OMS удостоверьтесь, что данные пользователей правильно сопоставлены с соответствующими [разрешениями на использование Azure Monitor и интерфейса оповещений](monitoring-roles-permissions-security.md).

Создание оповещений будет продолжаться с использованием имеющегося [API Log Analytics](../log-analytics/log-analytics-api-alerts.md), как и раньше, с незначительными изменениями, заключающимися в том, что после того, как оповещения будут расширены в Azure, группы действий должны быть связаны в расписании.

## <a name="next-steps"></a>Дополнительная информация

* Узнайте о средствах [инициирования расширения оповещений из OMS в Azure](monitoring-alerts-extend-tool.md)
* [Изучение возможностей новой службы "Оповещения (предварительная версия)" в Azure Monitor](monitoring-overview-unified-alerts.md).
* Дополнительные сведения см. в статье [Оповещения журнала в Azure Monitor. Оповещения Azure (предварительная версия)](monitor-alerts-unified-log.md).
