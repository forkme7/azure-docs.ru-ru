---
title: Основные понятия Служб мобильного взамодействия | Документация Майкрософт
description: Основные понятия Служб мобильного взаимодействия Azure
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 8d19abd1-0a6c-4772-9fa5-5e99980ac5da
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 1bc09de37c1b2aca35ef1ea74227df770f15baf5
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2018
---
# <a name="azure-mobile-engagement-concepts"></a>Основные понятия Служб мобильного взаимодействия Azure
> [!IMPORTANT]
> Срок действия Служб мобильного взаимодействия истекает 31.03.2018. Вскоре после этого страница будет удалена.
> 

Службы мобильного взаимодействия определяют несколько основных понятий, общих для всех поддерживаемых платформ. В этой статье кратко описаны эти понятия.

Если вы не знакомы со Службами мобильного взаимодействия, то эта статья — хорошее начало. Также обязательно прочтите документацию, которая относится к используемой платформе, чтобы подробнее узнать об основных понятиях, примерах и возможных ограничениях.

## <a name="devices-and-users"></a>Устройства и пользователи
Службы мобильного взаимодействия идентифицируют пользователей с помощью создания уникального идентификатора для каждого устройства. Этот идентификатор называется идентификатором устройства (или `deviceid`). Он создается таким образом, чтобы все приложения, запущенные на одном устройстве, использовали один идентификатор устройства.

Косвенно это означает, что Службы мобильного взаимодействия считают, что одно устройство принадлежит одному пользователю. Таким образом, пользователи и устройства являются эквивалентными понятиями.

## <a name="sessions-and-activities"></a>Сеансы и действия
Сеанс — это однократное использование приложения пользователем с момента начала использования до момента прекращения использования.

Действие — это однократное использование определенной подчасти приложения одним пользователем (обычно это экран, но может быть и что-то другое, что подходит для приложения).

За раз пользователь может выполнить только одно действие.

Действие идентифицируется по имени (ограничено 64 символами) и может при необходимости внедрить дополнительные данные (с ограничением в 1024 байт).

Сеансы вычисляются автоматически из последовательности действий, выполняемых пользователями. Сеанс начинается, когда пользователь выполняет свое первое действие, и завершается, когда пользователь заканчивает свое последнее действие. Это означает, что сеансы не требуется запускать или завершать явным образом. Вместо этого явным образом запускаются или завершаются действия. При отсутствии действий сеансы также отсутствуют.

## <a name="events"></a>События
События используются для отправки отчетов о мгновенных действиях (например, о нажатии кнопки или прочитанных статьях).

Событие может быть связано с текущим сеансом, с выполняющимся заданием или может быть изолированным.

Событие идентифицируется по имени (ограничено 64 символами) и может при необходимости внедрять дополнительные данные (с ограничением в 1024 байт).

## <a name="error"></a>Ошибка
Ошибки используются для отправки отчетов о проблемах, правильно обнаруженных приложением (например, неправильное действие пользователя или сбои вызовов API).

Ошибка может быть связана с текущим сеансом, с выполняющимся заданием или может быть изолированной.

Ошибка идентифицируется по имени (ограничено 64 символами) и может при необходимости внедрять дополнительные данные (с ограничением в 1024 байт).

## <a name="job"></a>Задание
Задания используются для отправки отчетов о действиях, имеющих длительность (например, длительность вызовов API, время отображения рекламы, длительность фоновых задач или длительность действий пользователя).

Задание не связано с сеансом, так как задача может выполняться в фоновом режиме без вмешательства пользователя.

Задание идентифицируется по имени (ограничено 64 символами) и может при необходимости внедрять дополнительные данные (с ограничением в 1024 байт).

## <a name="crash"></a>Сбой
Пакет SDK для Служб мобильного взаимодействия автоматически отправляет отчеты о сбоях приложений, когда проблемы, не обнаруженные приложением, вызывают его сбой.

## <a name="application-information"></a>Сведения о приложении
Сведения о приложении (или информация о приложении) используются, чтобы отмечать пользователей, т. е. связывать данные с пользователями приложения (аналогично файлам cookie за исключением того, что сведения о приложении хранятся на стороне сервера на платформе Служб мобильного взаимодействия).

Сведения о приложении можно зарегистрировать с помощью API пакета SDK для Служб мобильного взаимодействия или API устройств платформы Служб мобильного взаимодействия.

Сведения о приложении представляют собой пару "ключ — значение", связанную с устройством. Ключ — это имя сведений о приложении (ограничено 64 буквами ASCII [a-z, A-Z], цифрами [0-9] и символами подчеркивания [_]). Значение (ограничено 1024 символами) может быть любой строкой, целым числом, датой (гггг-ММ-дд) или логическим значением (true или false).

Любое количество сведений о приложении может быть связано с устройством в рамках, определенных заданными ценами на Службы мобильного взаимодействия. Для данного ключа Службы мобильного взаимодействия отслеживают только набор последних значений (без журнала). Если значение сведений о приложении будет задано или изменено, Службы мобильного взаимодействия выполнят принудительное повторное вычисление набора критериев аудитории, указанных в этих сведениях о приложении (если они есть). Это означает, что сведения о приложении можно использовать для активации push-уведомлений в режиме реального времени.

## <a name="extra-data"></a>Дополнительные данные
Дополнительные данные — это некоторые произвольные данные, которые можно присоединить к событиям, ошибкам, действиям и заданиям.

Дополнительные данные структурируются так же, как объекты JSON: они созданы из дерева пар "ключ-значение". Ключи ограничены 64 буквами ASCII [a-z, A-Z], цифрами [0-9] и символами подчеркивания [_]), а общий размер дополнительных данных ограничен 1024 знаками (после кодировки в JSON пакетом SDK для Служб мобильного взаимодействия).

Дерево пар "ключ-значение" полностью сохранено как объект JSON. Тем не менее только первый уровень пар "ключ — значение" можно разделить для прямого доступа некоторыми сложными функциями. К таким функциям, например, относятся "Сегменты" (т. е. можно легко определить сегмент "Любители научной фантастики", в который входят все пользователи, который за последний месяц не менее десяти раз отправили событие с именем content_viewed с дополнительным ключом content_type, имеющим значение scifi). Таким образом, рекомендуется отправлять только дополнительные данные, созданные из простых списков пар "ключ — значение", использующих скалярные значения (например, строки, даты, целые числа или логические значения).

## <a name="next-steps"></a>Дополнительная информация
* [Общие сведения о пакете SDK для универсальных приложений Windows для Служб мобильного взаимодействия Azure](mobile-engagement-windows-store-sdk-overview.md)
* [Общие сведения о пакете SDK для Windows Phone Silverlight для Служб мобильного взаимодействия Azure](mobile-engagement-windows-phone-sdk-overview.md)
* [Пакет SDK для Служб мобильного взаимодействия Azure (iOS)](mobile-engagement-ios-sdk-overview.md)
* [Пакет Android SDK для Служб мобильного взаимодействия Azure](mobile-engagement-android-sdk-overview.md)

