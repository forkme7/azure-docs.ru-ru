---
title: "Руководство по устранению неполадок потоковой трансляции | Документация Майкрософт"
description: "В этом разделе приводятся предложения по решению проблем потоковой передачи в реальном времени."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 3a7f6c1d-ce57-4fa4-a7a6-edb526b3ffbf
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: fa91baf7c494941fccf0e6ca38b930f3c2a521ce
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-guide-for-live-streaming"></a>Руководство по устранению неполадок потоковой передачи в реальном времени
В этом разделе приводятся предложения по решению некоторых проблем потоковой передачи в реальном времени.

## <a name="issues-related-to-on-premises-encoders"></a>Проблемы, связанные с локальными кодировщиками
Данный раздел содержит рекомендации по устранению неполадок, связанных с локальными кодировщиками, настроенными для отправки односкоростного потока в каналы AMS, которые выполняют кодирование в реальном времени.

### <a name="problem-would-like-to-see-logs"></a>Проблема: необходимо просмотреть журналы
* **Потенциальная проблема**. Не удается найти журналы кодировщика, которые могут помочь в отладке.
  
  * **Telestream Wirecast**. Обычно журналы можно найти в папке C:\Users\{имя_пользователя}\AppData\Roaming\Wirecast\. 
  * **Elemental Live**. Ссылки на журналы можно найти на портале управления. Щелкните **Статистика**, а затем **Журналы**. На странице **Файлы журналов** вы увидите список журналов для всех элементов LiveEvent. Выберите журнал текущего сеанса. 
  * **Flash Media Live Encoder**. Каталог **Log Directory** можно найти, перейдя на вкладку **Encoding Log** (Журнал Службы кодирования).

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Проблема. Невозможно вывести поток с прогрессивной разверткой
* **Потенциальная причина**. Используемый кодировщик не выполняет автоматическое устранение чересстрочной развертки. 
  
    **Шаги по устранению неполадок**. Проверьте параметр устранения чересстрочной развертки в интерфейсе кодировщика. После включения устранения чересстрочной развертки еще раз проверьте настройки вывода потока с прогрессивной разверткой. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Проблема. Я попробовал несколько вариантов выходных кодировщика и по-прежнему не могу подключить кодировщик.
* **Потенциальная причина**. Канал службы кодирования Azure не был сброшен должным образом. 
  
    **Шаги по устранению неполадок**. Убедитесь, что кодировщик больше не отправляет поток в AMS, остановите и сбросьте канал. После повторного запуска попробуйте подключить кодировщик с новыми параметрами. Если это еще не решило проблему, попробуйте создать полностью новый канал. Иногда работа каналов нарушается после нескольких неудачных попыток подключения.  
* **Потенциальная причина**. Размер GOP или ключевого кадра не оптимален. 
  
    **Шаги по устранению неполадок**. Рекомендуемое значение размера GOP или интервала опорного кадра — 2 секунды. В некоторых кодировщиках этот параметр задается в количестве кадров, в других — в количестве секунд. Например: при выводе с частотой кадров 30 кадров/с размер GOP составит 60 кадров, что соответствует 2 секундам.  
* **Потенциальная причина**. Поток блокируется из-за закрытых портов. 
  
    **Шаги по устранению неполадок**. При потоковой передаче через RTMP проверьте параметры брандмауэра или прокси-сервера, чтобы убедиться, что исходящие порты 1935 и 1936 открыты. При потоковой передаче через RTP убедитесь, что открыт исходящий порт 2010. 

### <a name="problem-when-configuring-the-encoder-to-stream-with-the-rtp-protocol-there-is-no-place-to-enter-a-host-name"></a>Проблема. При настройке кодировщика для отправки потока по протоколу RTP негде ввести имя узла.
* **Потенциальная причина**. Многие кодировщики RTP не разрешают ввода имен узлов, и для них необходимо получить IP-адрес.  
  
    **Шаги по устранению неполадок**. Чтобы получить IP-адрес, откройте командную строку на любом компьютере. В Windows откройте окно «Выполнить» (WIN + R) и введите cmd, чтобы открыть командную строку.  
  
    После открытия командной строки введите «Ping [имя_узла_AMS]». 
  
    Имя узла можно получить, опустив номер порта в URL-адресе приема Azure, как показано в следующем примере: 
  
    rtp://test2-amstest009.rtp.channel.mediaservices.windows.net:2010/ 
  
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

> [!NOTE]
> Если после выполнения этих действий воспроизвести поток по-прежнему не получается, отправьте запрос в службу поддержки с помощью портала Azure.
> 
> 

## <a name="media-services-learning-paths"></a>Схемы обучения работе со службами мультимедиа
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Отзывы
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

