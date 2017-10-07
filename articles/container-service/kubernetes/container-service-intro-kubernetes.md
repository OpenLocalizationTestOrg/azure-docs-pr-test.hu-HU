---
title: "aaaIntroduction tooAzure Kubernetes a Tárolószolgáltatás |} Microsoft Docs"
description: "Az Azure Tárolószolgáltatás számára Kubernetes teszi egyszerű toodeploy, és tároló-alapú alkalmazások felügyeletét az Azure."
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Kubernetes, Docker, tárolók, mikroszolgáltatások, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: bfc85a180bdf4a405c9047eb882d3eed01640dd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-container-service-for-kubernetes"></a>Bevezetés tooAzure Kubernetes a Tárolószolgáltatás
Az Azure Tárolószolgáltatás számára Kubernetes teszi egyszerű toocreate, konfigurálása és egy fürt virtuális gépek, amelyek indexelése előre konfigurált toorun alkalmazásokat kezeléséhez. Ez lehetővé teszi, hogy a hogy toouse a meglévő ismeretei, vagy egy nagy- és az egyre növekvő törzsének közösségi szakértelmét, toodeploy épülnek, és tároló-alapú alkalmazások a Microsoft Azure kezelése.

Azure Tárolószolgáltatás használatával előnyeit hello Azure, a vállalati szintű funkciók emellett alkalmazás hordozhatóság Kubernetes keresztül van fenntartva, és a Docker képformátum hello.

## <a name="using-azure-container-service-for-kubernetes"></a>Az Azure Container Service for Kubernetes használata
Az Azure Tárolószolgáltatás célunk tooprovide tároló üzemeltetési környezet nyílt forrású eszközök és technológiák manapság népszerű, az ügyfelek között. toothis célból elérhetővé kell tenni hello szabványos Kubernetes API-végpontokon. Standard végpontokkal használatával teheti az olyan szoftvert, amely képes a tooa Kubernetes fürt van szó. Például választhatja a következőket: [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) vagy [draft](https://github.com/Azure/draft).

## <a name="creating-a-kubernetes-cluster-using-azure-container-service"></a>Kubernetes-fürt létrehozása az Azure Container Service használatával
az Azure Tárolószolgáltatás használatával toobegin Azure Tárolószolgáltatás-fürt üzembe helyezése a hello [Azure CLI 2.0](container-service-kubernetes-walkthrough.md) vagy hello portálon (keresési hello piactér a **Azure Tárolószolgáltatás**). Ha egy speciális felhasználókat, akiknek hello Azure Resource Manager-sablonok teljesebb körű vezérlése, használhatja a hello nyílt forráskódú [acs-motor](https://github.com/Azure/acs-engine) projekt toobuild saját egyéni Kubernetes fürtön, és telepítse azt keresztül hello `az` CLI-t.

### <a name="using-kubernetes"></a>A Kubernetes használata
A Kubernetes automatizálja a tárolóalapú alkalmazások üzembe helyezését, méretezését és felügyeletét. Többek között a következő funkciókat tartalmazza:
* Automatikus bin-csomagolás
* Önjavítás
* Vízszintes méretezés
* Szolgáltatásészlelés és terheléselosztás
* Automatizált kibocsátások és visszaállítások
* Titkos kódok és konfigurációk kezelése
* Tárolóvezénylés
* Kötegelt végrehajtás

Az Azure Container Service szolgáltatással üzembe helyezett Kubernetes architekturális diagramja:

![Az Azure Tárolószolgáltatás toouse Kubernetes konfigurálva.](media/acs-intro/kubernetes.png)

## <a name="videos"></a>Videók

A Kubernetes támogatása az Azure Container Service szolgáltatásban (Azure Friday, 2017. január):

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Kubernetes-Support-in-Azure-Container-Services/player]
>
>

Eszközök az alkalmazások fejlesztéséhez és üzembe helyezéséhez a Kubernetes szolgáltatásban (Azure OpenDev, 2017. június):

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

## <a name="next-steps"></a>Következő lépések

Hello felfedezés [Kubernetes gyors üzembe helyezés](container-service-kubernetes-walkthrough.md) Azure Tárolószolgáltatás ma felfedezése toobegin.
