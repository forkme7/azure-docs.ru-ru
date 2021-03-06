---
title: Вход через SSH в узлы кластера Службы контейнеров Azure (AKS)
description: Создание SSH-соединения с узлами кластера Службы контейнеров Azure (AKS)
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 04/06/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 085a2976443db8ece7a36dbfc133b173432ce4c8
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="ssh-into-azure-container-service-aks-cluster-nodes"></a>Вход через SSH в узлы кластера Службы контейнеров Azure (AKS)

В некоторых случаях может потребоваться доступ к узлу Службы контейнеров Azure (AKS) для обслуживания, сбора данных журналов или других операций по устранению неполадок. Узлы Службы контейнеров Azure (AKS) не доступны через Интернет. Чтобы создать SSH-соединение с узлом AKS, выполните действия, описанные в этом документе.

## <a name="get-aks-node-address"></a>Получение адреса узла AKS

Получите IP-адрес узла кластера AKS с помощью команды `az vm list-ip-addresses`. Замените имя группы ресурсов на имени своей группы ресурсов AKS.

```console
$ az vm list-ip-addresses --resource-group MC_myAKSCluster_myAKSCluster_eastus -o table

VirtualMachine            PrivateIPAddresses
------------------------  --------------------
aks-nodepool1-42032720-0  10.240.0.6
aks-nodepool1-42032720-1  10.240.0.5
aks-nodepool1-42032720-2  10.240.0.4
```

## <a name="create-ssh-connection"></a>Создание SSH-соединения

Запустите образ контейнера `debian` и свяжите с ним сеанс терминала. Контейнер можно использовать для создания сеанса SSH с любым узлом в кластере AKS.

```console
kubectl run -it --rm aks-ssh --image=debian
```

Установите клиент SSH в контейнере.

```console
apt-get update && apt-get install openssh-client -y
```

Откройте второй терминалов и отобразите список всех модулей, чтобы получить имя только что созданной группы pod.

```console
$ kubectl get pods

NAME                       READY     STATUS    RESTARTS   AGE
aks-ssh-554b746bcf-kbwvf   1/1       Running   0          1m
```

Скопируйте ключ SSH в группу pod и замените имя pod правильным значением.

```console
kubectl cp ~/.ssh/id_rsa aks-ssh-554b746bcf-kbwvf:/id_rsa
```

Настройте для файла `id_rsa` разрешение только для чтения.

```console
chmod 0600 id_rsa
```

Теперь создайте SSH-подключение к узлу AKS. Имя пользователя по умолчанию для кластера AKS — `azureuser`. Если эта учетная запись изменена во время создания кластера, укажите правильное имя пользователя-администратора.

```console
$ ssh -i id_rsa azureuser@10.240.0.6

Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.11.0-1016-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

48 packages can be updated.
0 updates are security updates.


*** System restart required ***
Last login: Tue Feb 27 20:10:38 2018 from 167.220.99.8
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@aks-nodepool1-42032720-0:~$
```

## <a name="remove-ssh-access"></a>Отключение доступа по протоколу SSH

Закончив, выйдите из сеанса SSH и интерактивного сеанса контейнера. Это действие удаляет группу pod, используемую для SSH-доступа из кластера AKS.