---
title: Элемент пользовательского интерфейса UserNameTextBox для портала Azure | Документация Майкрософт
description: Сведения об элементе пользовательского интерфейса Microsoft.Common.UserNameTextBox для портала Azure.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: 4c8f62784b563bd8d39ccc763598b73b9b5d7195
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2018
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a>Элемент пользовательского интерфейса Microsoft.Compute.UserNameTextBox
Элемент управления "Текстовое поле" со встроенной проверкой имен пользователей Windows и Linux.

## <a name="ui-sample"></a>Пример элемента пользовательского интерфейса
![Элемент пользовательского интерфейса Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a>Схема
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.UserNameTextBox",
  "label": "User name",
  "defaultValue": "",
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="remarks"></a>Примечания
- Если для параметра `constraints.required` задано значение **true**, то текстовое поле должно содержать значение, чтобы пройти проверку. Значение по умолчанию — **true**.
- Необходимо задать значение для параметра `osPlatform` (**Windows** или **Linux**).
- `constraints.regex` — это шаблон регулярного выражения JavaScript. Если параметр указан, значение текстового поля должно соответствовать шаблону, чтобы пройти проверку. Значение по умолчанию — **null**.
- `constraints.validationMessage` — это строка, которая отображается, когда значение текстового поля не проходит проверку, указанную в `constraints.regex`. Если параметр не указан, используются встроенные сообщения проверки текстового поля. Значение по умолчанию — **null**.
- Этот элемент содержит встроенную проверку, которая основана на значении, заданном для параметра `osPlatform`. Встроенная проверка может использоваться вместе с настраиваемым регулярным выражением.
Если для параметра `constraints.regex` значение указано, активируются встроенные и пользовательские проверки.

## <a name="sample-output"></a>Пример выходных данных
```json
"tabrezm"
```

## <a name="next-steps"></a>Дополнительная информация
* Общие сведения о создании определений пользовательского интерфейса см. в статье [Начало работы с CreateUiDefinition](create-uidefinition-overview.md).
* Дополнительные сведения об общих свойствах элементов пользовательского интерфейса см. в статье [Элементы CreateUiDefinition](create-uidefinition-elements.md).
