---
title: "Вычисление оценок в службе \"Миграция Azure\" | Документация Майкрософт"
description: "Эта статья содержит обзор вычисления оценок в службе \"Миграция Azure\"."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/02/2017
ms.author: raynew
ms.openlocfilehash: b264e2ceac4e76faa37d21972b94cfe323aa3ce5
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/23/2018
---
# <a name="assessment-calculations"></a>Вычисление оценок

Служба [Миграция Azure](migrate-overview.md) выполняет оценку локальных рабочих нагрузок для переноса в Azure. В этой статье содержатся сведения о том, как эти оценки вычисляются.


## <a name="overview"></a>Обзор

Оценка службы "Миграция Azure" состоит из трех этапов. Оценка начинается с анализа пригодности, затем следует оценка размера и, наконец, оценка ежемесячной стоимости. Компьютер переходит на следующий этап, только если проходит предыдущий. Например, если компьютер не прошел проверку пригодности для Azure, то он помечается как непригодный для Azure, и размер и стоимость для него вычисляться не будут. 


## <a name="azure-suitability-analysis"></a>Анализ пригодности для Azure

Не все виртуальные машины подходят для работы в облаке, так как облако имеет свои собственные ограничения и требования. Служба "Миграция Azure" оценивает целесообразность миграции каждого локального компьютера в Azure и классифицирует в соответствии с одной из следующих категорий:
- **Готово к работе в Azure**. Компьютер может быть перенесен в Azure без каких-либо изменений. Он загрузится в Azure и будет иметь полную поддержку Azure.
- **Условно готово к работе в Azure**. Компьютер может загружаться в Azure, но не будет иметь полную поддержку Azure. Например компьютеры с более старой версией ОС Windows Server не поддерживаются в Azure. Прежде чем переносить эти машины в Azure, необходимо внимательно следовать рекомендациям по исправлению, предложенным в процессе оценки, и устранить проблемы с готовностью.
- **Не готово к работе в Azure**. Компьютер не загрузится в Azure. Например, если на локальном компьютере установлен диск размером более 4 ТБ, он не может быть размещен в Azure. Необходимо следовать рекомендациям по исправлению, предложенным в процессе оценки, и устранить проблемы с готовностью переноса в Azure. Не выполнена правильная оценка размера и стоимости для компьютера, отмеченных как не готовых к работе в Azure.
- **Готовность неизвестна**. Служба не смогла определить готовность компьютера из-за недостатка данных, доступных в vCenter Server. 

Служба "Миграция Azure" проверяет свойства компьютера и гостевую операционную систему, чтобы определить готовность локального компьютера к работе в Azure.

### <a name="machine-properties"></a>Свойства виртуальной машины
Служба проверяет следующие свойства локальной виртуальной машины, чтобы определить, может ли она работать в Azure.
 
**Свойство** | **Дополнительные сведения** | **Состояние готовности к работе в Azure**
--- | --- | ---
**Тип загрузки** | Azure поддерживает виртуальные машины с типом загрузки BIOS, а не UEFI. | Если тип загрузки — UEFI, устройство считается условно готовым к работе в Azure. 
**Ядра** | Число ядер в компьютерах должно быть равно (или меньше) максимальному количеству ядер (32), поддерживаемых для виртуальной машины Azure.<br/><br/> Если доступен журнал производительности, служба "Миграция Azure" рассматривает используемые ядра для сравнения. Если в параметрах оценки указан фактор комфорта, то количество используемых ядер умножается на него.<br/><br/> Если журнал производительности отсутствует, служба "Миграция Azure" использует выделенные ядра, не применяя фактор комфорта. | Если количество ядер больше 32, устройство считается условно готовым к работе в Azure. 
**Память** | Объем памяти компьютера должен быть равен (или меньше) максимальному объему памяти (448 ГБ), допустимому для виртуальной машины Azure. <br/><br/> Если доступен журнал производительности, служба "Миграция Azure" рассматривает используемую память для сравнения. Если задан фактор комфорта, объем используемой памяти умножается на него.<br/><br/> Если журнал отсутствует, используется выделенная память без применения фактора комфорта.<br/><br/> | Если объем памяти больше чем 448 ГБ, устройство считается условно готовым к работе в Azure.
**Диск хранилища** | Выделенный размер диска не должен превышать 4 ТБ (4096 ГБ).<br/><br/> Количество дисков, подключенных к компьютеру, не должно превышать 65, включая диск операционной системы. | Если размер какого-либо диска превышает 4 ТБ или на машине установлено более 65 дисков, устройство считается условно готовым к работе в Azure. 
**Сеть** | К компьютеру должно быть подключено не более 32 сетевых адаптеров. | Если на компьютере установлено более 32 сетевых адаптеров, устройство считается условно готовым к работе в Azure.

### <a name="guest-operating-system"></a>Операционная система на виртуальной машине 
Помимо свойств виртуальных машин служба "Миграция Azure" также просматривает гостевую ОС локальной виртуальной машины, чтобы определить, может ли виртуальная машина работать в Azure.

> [!NOTE]
> Служба "Миграция Azure" рассматривает ОС, указанную в vCenter Server, и выполняет следующий анализ. Так как обнаружение, выполняемое службой "Миграция Azure", основано на использовании устройства, не предусмотрен способ проверить, является ли ОС виртуальной машины той же, что и в vCenter Server.

Службой "Миграция Azure" используется следующая логика для идентификации готовности виртуальной машины для работы в Azure на основе операционной системы.

**Операционная система** | **Дополнительные сведения** | **Состояние готовности к работе в Azure**
--- | --- | ---
Windows Server 2016 и все пакеты обновления | Azure обеспечивает полную поддержку. | Готово к работе в Azure
Windows Server 2012 R2 и все пакеты обновления | Azure обеспечивает полную поддержку. | Готово к работе в Azure
Windows Server 2012 и все пакеты обновления | Azure обеспечивает полную поддержку. | Готово к работе в Azure
Windows Server 2008 R2 со всеми пакетами обновления | Azure обеспечивает полную поддержку.| Готово к работе в Azure
Windows Server 2003-2008 R2 | Эти операционные системы больше не поддерживаются. Для их поддержки в Azure требуется [соглашение Custom Support Agreement (CSA)](https://aka.ms/WSosstatement). | Условно готово к работе в Azure. Следует обновить ОС перед миграцией в Azure.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Эти операционные системы больше не поддерживаются. Компьютер может быть загружен в Azure, но без поддержки ОС. | Условно готово к работе в Azure. Следует обновить ОС перед миграцией в Azure.
Windows Client 7, 8 и 10 | Azure обеспечивает поддержку только для подписки Visual Studio. | Условно готово к работе в Azure.
Windows Vista, XP Professional | Эти операционные системы больше не поддерживаются. Компьютер может быть загружен в Azure, но без поддержки ОС. | Условно готово к работе в Azure. Следует обновить ОС перед миграцией в Azure.
Linux | Azure рекомендует эти [операционные систем Linux](../virtual-machines/linux/endorsed-distros.md). Другие операционные системы Linux могут быть загружены в Azure, но перед переносом рекомендуется обновить ОС до рекомендуемой версии. | Устройство готово к работе в Azure, если использует рекомендуемую версию.<br/><br/>Условно готово, если версия не рекомендуется.
Другие операционные системы<br/><br/> Например, Oracle Solaris, Apple Mac OS, FreeBSD и т. д. | Azure не рекомендует эти операционные системы. Компьютер может быть загружен в Azure, но Azure не будет поддерживать ОС. | Условно готово к работе в Azure. Следует установить поддерживаемую ОС перед миграцией в Azure.  
ОС, указанные как *Другое* в vCenter Server | Служба "Миграция Azure" не сможет идентифицировать ОС. | Готовность неизвестна. Убедитесь, что ОС виртуальной машины поддерживается в Azure. 
32-разрядные операционные системы | Компьютер может быть загружен в Azure, но Azure не предоставит полную поддержку. | Условно готово к работе в Azure. Измените операционную систему компьютера с 32-разрядной на 64-разрядную перед миграцией в Azure

## <a name="sizing"></a>Определение размера

После того, как машина отмечена как готовая к работе в Azure, служба "Миграция Azure"задает размеры виртуальной машины и ее дисков для Azure. Если определение размера, указанное в свойствах оценки, состоит в том, чтобы выполнить измерение на основе производительности, служба "Миграция Azure" анализирует историю производительности компьютера для определения размера виртуальной машины в Azure. Этот метод полезен в сценариях, в которых у вас есть мало используемая избыточная локальная виртуальная машина, и вы хотите изменить размеры виртуальных машин в Azure для экономии затрат.

> [!NOTE]
> Служба "Миграция Azure" получает историю производительности локальных виртуальных машин с сервера vCenter. Чтобы обеспечить точную настройку, для статистики в vCenter Server задайте уровень 3 и подождите по крайней мере один день до того, как начать анализ локальных виртуальных машин. Если для статистики в vCenter Server задан уровень меньше чем 3, данные о производительности диска и сети не собираются. 

Если анализ истории производительности не требуется и вы хотите перенести виртуальную машину в Azure "как есть", можно указать критерий выбора размера *как для локальной виртуальной машины*. Затем служба "Миграция Azure" изменит размеры виртуальных машин на основе конфигурации в локальной среде без учета данных об использовании.

### <a name="performance-based-sizing"></a>Выбор размера на основе производительности

Выбор размера на основе производительности в службе "Миграция Azure" начинается с дисков, подключенных к виртуальной машине, сетевых адаптеров. Затем сопоставляется виртуальная машина Azure в соответствии с потребностями в вычислительных ресурсах локальной виртуальной машины. 
 
- **Диски**. Служба "Миграция Azure" пытается сопоставить каждый диск, подключенный к компьютеру, с диском в Azure. 
    
    > [!NOTE]
    > Служба "Миграция Azure" поддерживает только оценку управляемых дисков.
    
    - Чтобы получить данные об эффективности дисковых операций ввода-вывода и использования пропускной способности (МБ/с), служба "Миграция Azure" умножает операции ввода-вывода диска и пропускную способность на фактор комфорта. На основе этих данных служба определяет, должен ли диск сопоставляться с диском уровня "Стандартный" или "Премиум" в Azure.
    - Если службе не удается найти диск, обеспечивающий необходимое число операций ввода-вывода и пропускную способность, она помечает компьютер как неподходящий для Azure. [Узнайте больше](../azure-subscription-service-limits.md#storage-limits) об ограничениях Azure для диска и виртуальной машины.
    - При обнаружении набора подходящих дисков служба "Миграция Azure" выбирает те, которые поддерживают метод обеспечения избыточности хранилища и расположение, указанное в параметрах оценки.
    - Если доступно несколько соответствующих дисков, она выбирает тот, который обеспечивает наименьшие затраты.
    - Если данные о производительности жестких дисков недоступны, все диски сопоставляются со стандартными дисками в Azure.

- **Сетевые адаптеры**. Служба "Миграция Azure" пытается подобрать виртуальную машину Azure, которая сможет поддерживать количество сетевых адаптеров, подключенных к локальной машине, и требуемую производительность.
    - Чтобы получить эффективную производительность локальной сети, служба объединяет данные, передаваемые в секунду (Мб/с) с компьютера (сеть), через все сетевые адаптеры и применяет фактор комфорта. Это число используется для поиска виртуальной машины Azure, обеспечивающей нужную производительность сети.
    - Наряду с производительностью сети также учитывается, поддерживает ли виртуальная машина Azure необходимое количество сетевых адаптеров.
    - Если данные о производительности сети недоступны, то для выбора размера виртуальной машины учитывается только число сетевых адаптеров.

- **Размер виртуальной машины**. После вычисления требования к хранилищу и сети служба "Миграция Azure" рассматривает требования к ЦП и памяти для поиска подходящего размера виртуальной машины в Azure.
    - Служба рассматривает используемые ядра и память и применяет фактор комфорта для получения эффективных ядер и памяти. Исходя из этого числа служба пытается найти подходящий размер виртуальной машины в Azure.
    - Если найти подходящий размер не удалось, компьютер помечается как неподходящий для Azure.
    - Если найден подходящий размер, служба "Миграция Azure" применяет результаты вычисления параметров хранилища и сети. Затем она применяет параметры расположения и ценовой категории, чтобы получить окончательный рекомендуемый размер виртуальной машины.
    - Если доступно несколько соответствующих размеров виртуальных машин, служба выбирает рекомендуемый, который обеспечивает наименьшие затраты.

### <a name="as-on-premises-sizing"></a>Определение размера как для локальной виртуальной машины
При определении размера *как для локальной виртуальной машины* служба "Миграция Azure" не учитывает историю производительности виртуальных машин и выделяет виртуальные машины и диски на основе их размера в локальной среде.
- **Хранилище**. Для каждого диска рекомендуется стандартный диск в Azure того же размера, что и локальный.
- **Сеть**. Для каждого сетевого адаптера рекомендуется использовать сетевой адаптер в Azure.
- **Среда выполнения приложений**. Служба"Миграция Azure" анализирует количество ядер и объем памяти виртуальной машины в локальной среде и рекомендует виртуальную машину Azure с такой же конфигурацией. Если доступно несколько соответствующих размеров виртуальных машин, служба выбирает рекомендуемый, который обеспечивает наименьшие затраты.
 
### <a name="confidence-rating"></a>Оценка достоверности

Каждая оценка в службе "Миграция Azure" связана с оценкой достоверности, которая колеблется в диапазоне от 1 до 5 звездочек (1 звезда — низкая достоверность, а 5 — высокая). Оценка достоверности назначается на основе доступности точек данных, необходимых для вычисления оценки. Оценка достоверности помогает оценить надежность рекомендаций по выбору размера, предоставленных службой "Миграция Azure". 

Оценка достоверности необходима, если вы *определяете размер на основе производительности*, так как служба "Миграция Azure" может не иметь достаточного количества точек данных для определения размера на основе использования. При *определении размера как для локальной виртуальной машины* значение оценки достоверности всегда равно 5 звездам, так как в службе "Миграция Azure" есть все данные для определения размера виртуальной машины. 

Для определения размера виртуальной машины на основе производительности службе "Миграция Azure" требуются данные об использовании ЦП и памяти. Кроме того, для каждого диска, присоединенного к виртуальной машине, необходимы сведения об операциях записи и чтения в секунду и пропускной способности. Аналогичным образом, для каждого сетевого адаптера, подключенного к виртуальной машине, службе "Миграция Azure" необходимы сведения о сетевом вводе-выводе. Если какой-либо из приведенных выше показателей использования недоступен в vCenter Server, рекомендации по выбору размера, предоставленные службой "Миграция Azure", могут быть ненадежными. В зависимости от процента доступных точек данных оценка достоверности может быть такой:

   **Уровень доступности точек данных** | **Оценка достоверности**
   --- | ---
   0–20 % | 1 звезда
   21–40 % | 2 звезды
   41–60 % | 3 звезды
   61–80 % | 4 звезды
   81–100 % | 5 звезд

В процессе оценки могут быть доступны не все точки данных по одной из следующих причин.
- Для статистики в vCenter Server не задан уровень 3 и для оценки выбрано условие определения размера на основе производительности. Если для статистики в vCenter Server задан уровень меньше чем 3, данные о производительности диска и сети не собираются из vCenter Server. В этом случае рекомендации для диска и сети, предоставленные службой "Миграция Azure", не основаны на показателях их использования. Для хранения в службе "Миграция Azure" рекомендуется использовать стандартные диски. Это связано с тем, что без показателя операций ввода-вывода в секунду или пропускной способности диска служба не может определить, требуется ли в Azure диск категории "Премиум".
- Для статистики в vCenter Server был установлен уровень 3 в течение меньшего периода времени перед выполнением обнаружения. Например, рассмотрим сценарий, в котором вы зададите для статистики уровень 3 сегодня, а начнете выполнение обнаружения с помощью модуля сборщика завтра (через 24 часа). При создании оценки за один день вам будут доступны все точки данных, а значение оценки достоверности будет равно 5 звездам. Но если изменить продолжительность производительности в свойствах оценки до одного месяца, оценка достоверности снижается, так как данные о производительности диска и сети за последний месяц будут недоступны. Если необходимо рассмотреть данные о производительности за последний месяц, рекомендуется задать для статистики vCenter Server уровень 3 на один месяц, прежде чем начать выполнение обнаружения. 
- В течение периода, для которого рассчитывается оценка, несколько виртуальных машин были отключены. Если какие-либо виртуальные машины были отключены в течение некоторого времени, в vCenter Server не будет данных о производительности за этот период. 
- За период, для которого рассчитывается оценка, было создано несколько виртуальных машин. Например, вы создаете оценку для журнала производительности за последний месяц, при этом несколько виртуальных машин были созданы всего неделю назад. В таких случаях журнал производительности новых виртуальных машин не будет доступен за весь период.

> [!NOTE]
> Если оценка достоверности ниже 4 звезд, рекомендуем задать для статистики vCenter Server уровень 3, подождать в течение периода, для которого нужно сформировать оценку (1 день, 1 неделя или 1 месяц), а затем выполнить обнаружение и формирование оценки. Если не сделать этого, определение размера на основе производительности может оказаться ненадежным. Рекомендуется переключиться на режим *определения размера как для локальной ВМ*, изменив свойства оценки.

## <a name="monthly-cost-estimation"></a>Примерная ежемесячная стоимость

После определения рекомендуемого размера служба "Миграция Azure" рассчитывает затраты на вычисления и хранилище после миграции.

- **Стоимость вычислений**. Служба "Миграция Azure" использует API выставления счетов, чтобы на основании рекомендуемого размера виртуальной машины Azure вычислить ежемесячные затраты на виртуальную машину. При этом учитывается операционная система, программа Software Assurance, расположение и параметры валюты. Служба суммирует затраты на все компьютеры, чтобы рассчитать общие ежемесячные затраты на вычисления.
- **Стоимость хранилища**. Ежемесячные затраты на хранилище для компьютера вычисляются путем суммирования ежемесячных затрат на все диски, подключенные к компьютеру. Служба "Миграция Azure" вычисляет общие ежемесячные затраты на хранилище, суммируя затраты на хранилище для всех компьютеров. В настоящее время при вычислении не учитываются предложения, указанные в параметрах оценки.

Цены отображаются в валюте, заданной в настройках оценки. 


## <a name="next-steps"></a>Дополнительная информация

[Обнаружение и оценка локальных виртуальных машин VMware для миграции в Azure](tutorial-assessment-vmware.md)
