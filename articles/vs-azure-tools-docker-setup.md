---
title: "aaaConfigure VirtualBox Docker gazdagépet |} Microsoft Docs"
description: "Részletes útmutatás tooconfigure Docker alapértelmezett példány Docker gép és VirtualBox használatával"
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 0b1335a2-7720-42a8-8260-4e06fc00c9f6
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 1df2da4482444a803d05e413e019edcc57269062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-docker-host-with-virtualbox"></a>VirtualBox egy Docker-állomás konfigurálása
## <a name="overview"></a>Áttekintés
Ez a cikk végigvezeti Önt egy Docker-számítógép és VirtualBox alapértelmezett Docker-példány konfigurálása. Hello használata [Docker for Windows beta](http://beta.docker.com/), ez a konfiguráció nincs szükség.

## <a name="prerequisites"></a>Előfeltételek
hello következő eszközök kell toobe telepítve.

* [Docker eszközkészlet](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-hello-docker-client-with-windows-powershell"></a>Hello Docker-ügyfél konfigurálása a Windows PowerShell használatával
tooconfigure egy Docker-ügyfelet, egyszerűen nyissa meg a Windows Powershellt, és hajtsa végre az alábbi lépésekkel hello:

1. Hozzon létre alapértelmezett docker-állomás példányt.
   
    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
2. Győződjön meg arról hello alapértelmezett példány, konfigurálni és futtatni. (Látnia kell egy futtató a "default" nevű példány.
   
    ```PowerShell
    docker-machine ls 
    ```
   
    ![docker-gép ls kimeneti][0]
3. Alapértelmezett hello aktuális gazdagépként, és a rendszerhéj konfigurálása.
   
    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```
4. Hello active Docker-tárolók megjelenítéséhez. hello lista üresnek kell lennie.
   
    ```PowerShell
    docker ps
    ```
   
    ![docker ps kimeneti][1]

> [!NOTE]
> Minden alkalommal, amikor a fejlesztői számítógépén, indítsa újra kell toorestart az helyi docker-állomás.
> toodo, ez a probléma hello a következő parancsot a parancssorba: `docker-machine start default`.
> 
> 

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
