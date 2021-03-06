---
title: Номера SKU реестра контейнеров Azure
description: Сравнение разных уровней служб, доступных в Реестре контейнеров Azure.
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 03/15/2018
ms.author: marsma
ms.openlocfilehash: c9b8e072b5ccd89c27d9c46407e472d6bf1e1e84
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2018
---
# <a name="azure-container-registry-skus"></a>Номера SKU реестра контейнеров Azure

Реестр контейнеров Azure (ACR) доступен на нескольких уровнях обслуживания, известных как номера SKU. Эти номера SKU предоставляют прогнозируемые цены и несколько вариантов, относящихся к согласованию с емкостью и шаблонами потребления частного реестра Docker в Azure.

| SKU | Управляемые | ОПИСАНИЕ |
| --- | :-------: | ----------- |
| **базовая;** | Yes | Недорогой начальный уровень для разработчиков, изучающих реестр контейнеров Azure. Реестры уровня "Базовый" располагают теми же программными возможностями, что и реестры уровней "Стандартный" и "Премиум" (интеграция проверки подлинности Azure Active Directory, удаление образов и веб-перехватчики), но на этом уровне действуют ограничения, касающиеся размера и использования. |
| **Стандартный** | Yes | Реестры уровня "Стандартный" предоставляют те же возможности, что и реестры уровня "Базовый", но больший размер хранилища и более высокую пропускную способность образов. Реестры уровня "Стандартный" подходят для большинства рабочих сценариев. |
| **Премиальный** | Yes | В реестрах уровня "Премиум" ограничения расширены. Это касается объема хранилища и количества параллельных операций, что делает возможными сценарии с большим объемом данных. Помимо повышенной пропускной способности образов, на уровне "Премиум" доступны дополнительные функции, такие как [георепликация][container-registry-geo-replication] для управления одним реестром в нескольких регионах. Это позволяет при каждом развертывании располагать реестр в регионе, который находится ближе всего к целевой сети. |
| Классический | Нет  | Номер SKU реестра уровня "Классический" включил первоначальный выпуск службы реестра контейнеров Azure в Azure. Реестры уровня "Классический" поддерживаются учетной записью хранения, которую создает Azure в подписке, из-за чего ACR предоставляет меньше возможностей более высокого уровня, таких как повышенная пропускная способность и георепликация. Из-за ограниченных возможностей номера SKU уровня "Классический" мы планируем в будущем отказаться от него. |

При выборе SKU более высокого уровня обеспечивается дополнительная производительность и масштабируемость, однако все управляемые SKU обеспечивают одинаковые программные возможности. При наличии нескольких уровней обслуживания можно начать работу с "Базового" уровня обслуживания, а затем перейти к уровню "Стандартный" и "Премиум" по мере увеличения потребления реестра.

> [!NOTE]
> В связи с запланированным отказом от номера SKU реестра уровня "Классический" мы рекомендуем использовать уровни "Базовый", "Стандартный" и "Премиум" для всех новых реестров. Дополнительные сведения о преобразовании имеющегося реестра уровня "Классический" см. в статье [Обновление реестра контейнеров уровня "Классический"][container-registry-upgrade].
>

## <a name="managed-vs-unmanaged"></a>Сравнение управляемого и неуправляемого реестра

SKU уровня "Базовый", "Стандартный" и "Премиум" называются *управляемыми* реестрами, а "Классический" — *неуправляемым*. Основное различие между ними — это способ хранения образов контейнера.

### <a name="managed-basic-standard-premium"></a>Управляемый реестр ("Базовый", "Стандартный", "Премиум")

Управляемые реестры используют хранилище образов, полностью управляемое Azure. То есть учетная запись хранения, которая хранит образы, не отображается в подписке Azure. Использование одного из управляемых номеров SKU реестра обеспечивает несколько преимуществ, подробно описанных в статье о [хранилище образов контейнеров в Реестре контейнеров Azure][container-registry-storage]. В этой статье рассматриваются управляемые номера SKU реестра и их возможности.

### <a name="unmanaged-classic"></a>Неуправляемый реестр ("Классический")

Классические реестры являются неуправляемыми в том смысле, что учетная запись хранения, поддерживающая его, находится в пределах *вашей* подписки Azure. Таким образом вы отвечаете за управление учетной записью хранения, в котором хранятся образы контейнера. В неуправляемых реестрах нельзя переключаться между номерами SKU по мере изменения потребностей (отличных от [обновления][container-registry-upgrade] до управляемого реестра), несколько возможностей управляемых реестров недоступны (например, удаление образа контейнера, [георепликация][container-registry-geo-replication] и [веб-перехватчики][container-registry-webhook]).

Дополнительные сведения об обновлении классического реестра до одного из управляемых номеров SKU см. в статье [Обновление реестра контейнеров уровня "Классический"][container-registry-upgrade].

## <a name="sku-feature-matrix"></a>Матрица возможностей номеров SKU

В следующей таблице приведены сведения о возможностях и ограничениях уровней обслуживания "Базовый", "Стандартный" и "Премиум".

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>Изменение номеров SKU

Номер SKU реестра можно изменить с помощью Azure CLI или на портале Azure. Вы можете свободно перемещаться между управляемыми номерами SKU, пока в них имеется необходимая максимальная емкость. При переходе на один из управляемых номеров SKU с реестра уровня "Классический", невозможно вернуться обратно на этот реестр — это одностороннее преобразование.

### <a name="azure-cli"></a>Инфраструктура CLI Azure

Для перемещения между номерами SKU в Azure CLI используйте команду [az acr update][az-acr-update]. Например, чтобы перейти на уровень обслуживания "Премиум":

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Портал Azure

В колонке **Обзор** реестра контейнеров на портале Azure выберите **Обновить**, а затем выберите новый **номер SKU** из раскрывающегося списка номеров SKU.

![Обновление номера SKU реестра контейнеров на портале Azure][update-registry-sku]

При наличии классического реестра на портале Azure нельзя выбрать управляемый номер SKU. Вместо этого сначала необходимо выполнить [обновление][container-registry-upgrade] до управляемого реестра (см. раздел [Переход с уровня "Классический"](#changing-from-classic)).

## <a name="changing-from-classic"></a>Переход с уровня "Классический"

При переносе неуправляемого классического реестра в один из управляемых номеров SKU уровня "Базовый", "Стандартный" или "Премиум" необходимо учитывать дополнительные рекомендации. Если в классическом реестре есть много образов и он большой, процесс миграции может занять некоторое время. Кроме того, операции `docker push` отключены до завершения миграции.

Дополнительные сведения об обновлении классического реестра до одного из управляемых номеров SKU см. в статье [Обновление реестра контейнеров уровня "Классический"][container-registry-upgrade].

## <a name="pricing"></a>Цены

Сведения о ценах на каждый номер SKU реестра контейнеров Azure см. на странице [цен на реестр контейнеров][container-registry-pricing].

## <a name="next-steps"></a>Дополнительная информация

**Схема развития реестра контейнеров Azure**

Ознакомьтесь со [схемой развития ACR][acr-roadmap] на GitHub, чтобы найти сведения о новых возможностях в службе.

**UserVoice реестра контейнеров Azure**

Отправляйте свои предложения и голосуйте за предложения функций на [форуме UserVoice для ACR][container-registry-uservoice].

<!-- IMAGES -->
[update-registry-sku]: ./media/container-registry-skus/update-registry-sku.png

<!-- LINKS - External -->
[acr-roadmap]: https://aka.ms/acr/roadmap
[container-registry-pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[container-registry-uservoice]: https://feedback.azure.com/forums/903958-azure-container-registry

<!-- LINKS - Internal -->
[az-acr-update]: /cli/azure/acr#az_acr_update
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-upgrade]: container-registry-upgrade.md
[container-registry-storage]: container-registry-storage.md
[container-registry-webhook]: container-registry-webhook.md
