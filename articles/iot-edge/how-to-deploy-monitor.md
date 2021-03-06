---
title: "Развертывание и мониторинг модулей для Azure IoT Edge | Документация Майкрософт"
description: "Управление модулями, выполняемыми на пограничных устройствах."
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/07/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: cc7d1e290465d9254cbd7fe9e8ba71cc740b0368
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/15/2017
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale---preview"></a>Развертывание и мониторинг модулей IoT Edge в нужном масштабе (предварительная версия)

Azure IoT Edge позволяет перемещать аналитику в пограничную зону и предоставляет облачный интерфейс, чтобы вы могли выполнять мониторинг устройств IoT Edge и управлять ими без физического доступа. Увеличилась потребность в удаленном управлении устройствами, так как решения Интернета вещей становятся более крупными и сложными. Azure IoT Edge предназначен для поддержки бизнес-задач, независимо от того, сколько устройств вы добавляете.

Вы можете управлять отдельными устройствами и последовательно развертывать в них модули. Тем не менее, если вы хотите внести изменения в устройства в больших масштабах, можно создать **развертывание IoT Edge**. Развертывания — это динамические процессы, которые позволяют развертывать несколько модулей в нескольких устройствах одновременно, отслеживать состояние и работоспособность модулей и вносить изменения при необходимости. 

## <a name="identify-devices-using-tags"></a>Определение устройств с помощью тегов

Перед созданием развертывания необходимо указать устройства, на которые вы хотите повлиять. Azure IoT Edge определяет устройства с помощью **тегов** в двойнике устройства. Каждое устройство может иметь несколько тегов. Их можно определить любым способом, который лучше всего подходит для вашего решения. Например, при управлении кампусом интеллектуальных зданий можно добавлять к устройству следующие теги:

```json
"tags":{
    "location":{
        "building": "20",
        "floor": "2"
    },
    "roomtype": "conference",
    "environment": "prod"
}
```

Дополнительные сведения о двойниках устройств и тегах см. в статье [Общие сведения о двойниках устройств и их использование в Центре Интернета вещей][lnk-device-twin].

## <a name="create-a-deployment"></a>Создание развертывания

1. Найдите нужный Центр Интернета вещей на [портале Azure][lnk-portal]. 
1. Выберите **IoT Edge (preview)** (IoT Edge (предварительная версия)).
1. Выберите **Add IoT Edge Deployment** (Добавить развертывание IoT Edge).

Процедура создания развертывания состоит из пяти шагов. В следующих разделах описан каждый из этих шагов. 

### <a name="step-1-name-and-label"></a>Шаг 1. Имя и метка

1. Укажите для развертывания уникальное имя. Не используйте пробелы и следующие недопустимые символы: `& ^ [ ] { } \ | " < > /`.
1. Добавьте метки для отслеживания развертываний. Метки представляют собой пары **имени** и **значения**, которые описывают развертывание. Например, `HostPlatform, Linux` или `Version, 3.0.1`.
1. Нажмите кнопку **Далее**, чтобы перейти ко второму шагу. 

### <a name="step-2-add-modules-optional"></a>Шаг 2. Добавление модулей (необязательно)

Существует два типа модулей, которые можно добавить в развертывание. Первый — это модуль, основанный на службе Azure, например учетной записи хранения или Stream Analytics. Второй — это модуль, основанный на вашем собственном коде. Вы можете добавить несколько модулей любого типа в развертывание. 

Если вы создаете развертывание без модулей, с устройств удаляются все существующие модули. 

>[!NOTE]
>Службы "Машинное обучение Azure" и "Функции Azure" еще не поддерживают автоматическое развертывание службы Azure. Вы можете вручную добавить эти службы к своему развертыванию, применив пользовательское развертывание модуля. 

Чтобы добавить модуль из Azure Stream Analytics, сделайте следующее:
1. Выберите **Import Azure Stream Analytics IoT Edge Module** (Импортировать модуль IoT Edge для Azure Stream Analytics).
1. В раскрывающемся меню можно выбрать экземпляры служб Azure для развертывания.
1. Выберите **Сохранить**, чтобы добавить модуль в развертывание. 

Чтобы добавить пользовательский код в качестве модуля или вручную добавить модуль службы Azure, сделайте следующее:
1. Выберите **Add IoT Edge modulе** (Добавить модуль IoT Edge).
1. Укажите **имя** модуля.
1. В поле **URI образа** введите образ контейнера Docker для вашего модуля. 
1. Укажите любые **параметры создания контейнера**, которые следует передать в контейнер. Дополнительные сведения см. в статье [о создании Docker][lnk-docker-create].
1. В раскрывающемся меню выберите **политику перезагрузки**. Выберите один из следующих параметров: 
   * **Всегда.** Модуль всегда перезапускается, если он завершил работу по любой причине.
   * **Никогда.** Модуль никогда не перезапускается, если он завершил работу по любой причине.
   * **On-failed** (В случае сбоя). Модуль перезапускается, если произошел сбой, но не при правильном завершении работы. 
   * **On-unhealthy** (В случае неработоспособности). Модуль перезапускается, если произошел сбой или когда он возвращает состояние неработоспособности. Реализация функции состояния работоспособности зависит от модуля. 
1. В раскрывающемся меню выберите **требуемое состояние** модуля. Выберите один из следующих параметров:
   * **Выполняется.** Это вариант по умолчанию. Модуль начнет выполняться сразу после развертывания.
   * **Остановлено.** После развертывания модуль остается неактивным, пока не будет вызван для работы вами или другим модулем.
1. Выберите **Включить**, чтобы добавить любые теги или необходимые свойства в двойник модуля. 
1. Выберите **Сохранить**, чтобы добавить модуль в развертывание. 

Настроив все модули для развертывания, нажмите кнопку **Далее**, чтобы перейти к шагу 3.

### <a name="step-3-specify-routes-optional"></a>Шаг 3. Указание маршрутов (необязательно)

Маршруты определяют, как модули взаимодействуют между собой в рамках развертывания. Укажите любые маршруты для развертывания, а затем нажмите кнопку **Далее**, чтобы перейти к шагу 4. 

### <a name="step-4-target-devices"></a>Шаг 4. Назначение устройств

С помощью свойств тегов ваших устройств можно назначить конкретные устройства, к которым будет применяться это развертывание. 

Так как несколько развертываний могут предназначаться для одного устройства, каждому развертыванию необходимо присвоить номер приоритета. Если возникает конфликт, побеждает развертывание с наивысшим приоритетом. Когда оба развертывания имеют одинаковый номер приоритета, побеждает то, которое было создано недавно. 

1. Введите положительное целое число для **приоритета** развертывания.
1. Введите **условие назначения**, чтобы определить, для каких устройств будет предназначено это развертывание. Условие основано на тегах двойника устройства и должно соответствовать формату выражения. Например, `tags.environment='test'`. 
1. Нажмите кнопку **Далее**, чтобы перейти к последнему шагу.

### <a name="step-5-review-template"></a>Шаг 5. Просмотр шаблона

Просмотрите сведения о развертывании, а затем выберите **Отправить**.

## <a name="monitor-a-deployment"></a>Мониторинг развертывания

Чтобы просмотреть сведения о развертывании и узнать, какое устройство его выполняет, сделайте следующее:

1. Войдите на [портал Azure][lnk-portal] и перейдите к своему Центру Интернета вещей. 
1. Выберите **IoT Edge (preview)** (IoT Edge (предварительная версия)).
1. Выберите **IoT Edge deployments** (Развертывания IoT Edge). 

   ![Просмотр развертываний IoT Edge][1]

1. Изучите список развертывания. Для каждого развертывания можно просмотреть следующие сведения:
   * **Идентификатор**. Имя развертывания.
   * **Target condition** (Условие назначения). Тег, используемый для определения целевых устройств.
   * **Приоритет.** Номер приоритета, назначенный для развертывания.
   * **IoT Edge agent status** (Состояние агента IoT Edge). Количество устройств, к которым применено развертывание, и их состояние работоспособности. 
   * **Unhealthy modules** (Неработоспособные модули). Количество модулей в уведомлениях об ошибках развертывания. 
   * **Время создания.** Метка времени, когда было создано развертывание. Метка времени используется, чтобы разорвать связи, если два развертывания имеют одинаковый приоритет. 
1. Выберите развертывание, которое вы хотите отслеживать.  
1. Изучите сведения о развертывании. С помощью вкладок можно просмотреть конкретные сведения об устройствах, к которым применено развертывание: 
   * **Targeted** (Нацелено). Пограничные устройства, которые соответствуют условию назначения. 
   * **Применено.** Целевые пограничные устройства, которые не используются другими развертываниями с высшим приоритетом. Это устройства, которые получают развертывание. 
   * **Reporting success** (Отчет об успешном завершении). Применимые пограничные устройства, которые передают в службу сведения об успешном развертывании модулей. 
   * **Reporting success** (Отчет о сбое). Применимые пограничные устройства, которые передают в службу информацию о том, что при развертывании одного или нескольких моделей произошел сбой. Для дальнейшего изучения ошибки вам необходимо удаленно подключиться к этим устройствам и просмотреть файлы журнала. 
   * **Reporting success** (Отчет о неработоспособных модулях). Применимые пограничные устройства, которые передают в службу информацию о том, что один или несколько модулей успешно развернуты, но сейчас сообщают об ошибках. 

## <a name="modify-a-deployment"></a>Изменение развертывания

При изменении развертывания изменения немедленно реплицируются во все целевые устройства. 

Если обновить целевое условие, произойдут следующие обновления:
* Если устройство не соответствует предыдущему условию назначения, однако соответствует новому и это развертывание имеет самый высокий приоритет для устройства, то оно будет применено к устройству. 
* Если устройство, на котором сейчас выполняется развертывание, больше не соответствует условию назначения, оно удаляет это развертывание и выполняет следующее развертывание с самым высоким приоритетом. 
* Если устройство, на котором сейчас выполняется развертывание, больше не соответствует условию назначения, а также условию назначения любого другого развертывания, в устройстве не происходит никаких изменений. Устройство продолжает выполнять текущие модули в их актуальном состоянии, однако больше не управляется как часть этого развертывания. Если устройство соответствует условию назначения любого другого развертывания, оно удаляет это развертывание и выполняет новое. 

Чтобы изменить развертывание, сделайте следующее: 

1. Войдите на [портал Azure][lnk-portal] и перейдите к своему Центру Интернета вещей. 
1. Выберите **IoT Edge (preview)** (IoT Edge (предварительная версия)).
1. Выберите **IoT Edge deployments** (Развертывания IoT Edge). 

   ![Просмотр развертываний IoT Edge][1]

1. Выберите развертывание, которое вы хотите изменить. 
1. Внесите изменения в следующие поля: 
   * Условие назначения 
   * Метки 
   * Приоритет 
1. Щелкните **Сохранить**.
1. Следуйте указаниям раздела [Мониторинг развертывания][anchor-monitor] для отслеживания выполнения изменений. 

## <a name="delete-a-deployment"></a>Удаление развертывания

При удалении развертывания все устройства выполняют следующее развертывание, которое имеет наивысший приоритет. Если устройства не соответствуют условиям назначения любого другого развертывания, то модули не будут удалены при удалении развертывания. 

1. Войдите на [портал Azure][lnk-portal] и перейдите к своему Центру Интернета вещей. 
1. Выберите **IoT Edge (preview)** (IoT Edge (предварительная версия)).
1. Выберите **IoT Edge deployments** (Развертывания IoT Edge). 

   ![Просмотр развертываний IoT Edge][1]

1. Установите флажок, чтобы выбрать развертывание, которое необходимо удалить. 
1. Нажмите кнопку **Удалить**.
1. Появится запрос с сообщением о том, что это действие удалит развертывание и будут возвращены предыдущие состояния всех устройств.  Это значит, что будет применено развертывание с низким приоритетом.  Если не указаны другие развертывания, модули не будут удалены. Если клиенты хотят выполнить это действие, необходимо создать развернутую службу без модулей и развернуть ее в том же устройстве. Выберите **Да**, если вы хотите продолжить. 

## <a name="next-steps"></a>Дополнительная информация

Узнайте подробнее о [развертывании модулей на пограничных устройствах][lnk-deployments].

<!-- Images -->
[1]: ./media/how-to-deploy-monitor/iot-edge-deployments.png

<!-- Links -->
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-portal]: https://portal.azure.com
[lnk-docker-create]: https://docs.docker.com/engine/reference/commandline/create/
[lnk-deployments]: module-deployment-monitoring.md

<!-- Anchor links -->
[anchor-monitor]: #monitor-a-deployment
