---
title: Настройка и администрирование политик репликации для репликации VMware с помощью Azure Site Recovery | Документация Майкрософт
description: В этой статье объясняется, как настроить параметры для репликации VMware в Azure, с помощью Azure Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: sutalasi
ms.openlocfilehash: 6a8e6e7c6fbdbcbf58557a0896e976a608164041
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2018
---
# <a name="configure-and-manage-replication-policies-for-vmware-replication"></a>Настройка и администрирование политик репликации для репликации VMware
Из этой статьи вы узнаете, как настроить политику репликации при репликации виртуальных машин VMware в Azure с помощью [Azure Site Recovery](site-recovery-overview.md).


## <a name="create-a-policy"></a>Создание политики

1. Выберите **Управление** > **Site Recovery Infrastructure** (Инфраструктура Site Recovery).
2. В разделе **For VMware and Physical machines** (Для виртуальных машин VMware и физических компьютеров) выберите **Политики репликации**. 
3. Щелкните **+ Политика репликации** и укажите имя политики.
5. В поле **Пороговое значение RPO**укажите предельное значение целевой точки восстановления (RPO). Если показатель непрерывной репликации превышает это значение, создаются оповещения.
6. В поле **Хранение точки восстановления** укажите период хранения каждой точки восстановления (в часах). Защищенные компьютеры будут восстанавливаться до любой точки в пределах этого периода. Для компьютеров, реплицируемых в хранилище уровня "Премиум", поддерживается хранение до 24 часов. Для стандартного хранилища поддерживается хранение до 72 часов.
7. В поле **Периодичность создания моментальных снимков с согласованием приложений** укажите, с какой периодичностью (в минутах) будут создаваться точки восстановления, содержащие моментальные снимки, согласованные на уровне приложений.
8. Последовательно выберите **ОК**. Политика будет создана в течение 30–60 секунд.

Когда вы создаете политику репликации, по умолчанию автоматически создается соответствующая политика для восстановления размещения с суффиксом failback. Чтобы изменить созданную политику, выберите ее и щелкните **Изменить параметры**.

## <a name="associate-a-configuration-server"></a>Связывание сервера конфигурации 

Свяжите политику репликации с локальным сервером конфигурации.

1. Щелкните **Связать** и выберите сервер конфигурации.

    ![Связывание сервера конфигурации](./media/vmware-azure-set-up-replication/associate1.png)

2. Последовательно выберите **ОК**. Связь сервера конфигурации с политикой будет задана в течение 1–2 минут.

    ![Связывание сервера конфигурации](./media/vmware-azure-set-up-replication/associate2.png)


## <a name="disassociate-or-delete-a-replication-policy"></a>Отмена связи или удаление политики репликации
1. Выберите политику репликации.
    a. Чтобы отменить связь между политикой и сервером конфигурации, убедитесь, что реплицируемые виртуальные машины не используют эту политику. Затем щелкните **Отменить связь**.
    Б. Чтобы удалить политику, убедитесь что она не связана с сервером конфигурации. Затем щелкните **Удалить**. Удаление занимает 30–60 секунд.
2. Последовательно выберите **ОК**.
