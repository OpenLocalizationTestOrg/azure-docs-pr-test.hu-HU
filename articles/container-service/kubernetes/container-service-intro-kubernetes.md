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
# <a name="introduction-tooazure-container-service-for-kubernetes"></a><span data-ttu-id="f7f29-104">Bevezetés tooAzure Kubernetes a Tárolószolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f7f29-104">Introduction tooAzure Container Service for Kubernetes</span></span>
<span data-ttu-id="f7f29-105">Az Azure Tárolószolgáltatás számára Kubernetes teszi egyszerű toocreate, konfigurálása és egy fürt virtuális gépek, amelyek indexelése előre konfigurált toorun alkalmazásokat kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="f7f29-105">Azure Container Service for Kubernetes makes it simple toocreate, configure, and manage a cluster of virtual machines that are preconfigured toorun containerized applications.</span></span> <span data-ttu-id="f7f29-106">Ez lehetővé teszi, hogy a hogy toouse a meglévő ismeretei, vagy egy nagy- és az egyre növekvő törzsének közösségi szakértelmét, toodeploy épülnek, és tároló-alapú alkalmazások a Microsoft Azure kezelése.</span><span class="sxs-lookup"><span data-stu-id="f7f29-106">This enables you toouse your existing skills, or draw upon a large and growing body of community expertise, toodeploy and manage container-based applications on Microsoft Azure.</span></span>

<span data-ttu-id="f7f29-107">Azure Tárolószolgáltatás használatával előnyeit hello Azure, a vállalati szintű funkciók emellett alkalmazás hordozhatóság Kubernetes keresztül van fenntartva, és a Docker képformátum hello.</span><span class="sxs-lookup"><span data-stu-id="f7f29-107">By using Azure Container Service, you can take advantage of hello enterprise-grade features of Azure, while still maintaining application portability through Kubernetes and hello Docker image format.</span></span>

## <a name="using-azure-container-service-for-kubernetes"></a><span data-ttu-id="f7f29-108">Az Azure Container Service for Kubernetes használata</span><span class="sxs-lookup"><span data-stu-id="f7f29-108">Using Azure Container Service for Kubernetes</span></span>
<span data-ttu-id="f7f29-109">Az Azure Tárolószolgáltatás célunk tooprovide tároló üzemeltetési környezet nyílt forrású eszközök és technológiák manapság népszerű, az ügyfelek között.</span><span class="sxs-lookup"><span data-stu-id="f7f29-109">Our goal with Azure Container Service is tooprovide a container hosting environment by using open-source tools and technologies that are popular among our customers today.</span></span> <span data-ttu-id="f7f29-110">toothis célból elérhetővé kell tenni hello szabványos Kubernetes API-végpontokon.</span><span class="sxs-lookup"><span data-stu-id="f7f29-110">toothis end, we expose hello standard Kubernetes API endpoints.</span></span> <span data-ttu-id="f7f29-111">Standard végpontokkal használatával teheti az olyan szoftvert, amely képes a tooa Kubernetes fürt van szó.</span><span class="sxs-lookup"><span data-stu-id="f7f29-111">By using these standard endpoints, you can leverage any software that is capable of talking tooa Kubernetes cluster.</span></span> <span data-ttu-id="f7f29-112">Például választhatja a következőket: [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) vagy [draft](https://github.com/Azure/draft).</span><span class="sxs-lookup"><span data-stu-id="f7f29-112">For example, you might choose [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/), or [draft](https://github.com/Azure/draft).</span></span>

## <a name="creating-a-kubernetes-cluster-using-azure-container-service"></a><span data-ttu-id="f7f29-113">Kubernetes-fürt létrehozása az Azure Container Service használatával</span><span class="sxs-lookup"><span data-stu-id="f7f29-113">Creating a Kubernetes cluster using Azure Container Service</span></span>
<span data-ttu-id="f7f29-114">az Azure Tárolószolgáltatás használatával toobegin Azure Tárolószolgáltatás-fürt üzembe helyezése a hello [Azure CLI 2.0](container-service-kubernetes-walkthrough.md) vagy hello portálon (keresési hello piactér a **Azure Tárolószolgáltatás**).</span><span class="sxs-lookup"><span data-stu-id="f7f29-114">toobegin using Azure Container Service, deploy an Azure Container Service cluster with hello [Azure CLI 2.0](container-service-kubernetes-walkthrough.md) or via hello portal (search hello Marketplace for **Azure Container Service**).</span></span> <span data-ttu-id="f7f29-115">Ha egy speciális felhasználókat, akiknek hello Azure Resource Manager-sablonok teljesebb körű vezérlése, használhatja a hello nyílt forráskódú [acs-motor](https://github.com/Azure/acs-engine) projekt toobuild saját egyéni Kubernetes fürtön, és telepítse azt keresztül hello `az` CLI-t.</span><span class="sxs-lookup"><span data-stu-id="f7f29-115">If you are an advanced user who needs more control over hello Azure Resource Manager templates, you can use hello open source [acs-engine](https://github.com/Azure/acs-engine) project toobuild your own custom Kubernetes cluster and deploy it via hello `az` CLI.</span></span>

### <a name="using-kubernetes"></a><span data-ttu-id="f7f29-116">A Kubernetes használata</span><span class="sxs-lookup"><span data-stu-id="f7f29-116">Using Kubernetes</span></span>
<span data-ttu-id="f7f29-117">A Kubernetes automatizálja a tárolóalapú alkalmazások üzembe helyezését, méretezését és felügyeletét.</span><span class="sxs-lookup"><span data-stu-id="f7f29-117">Kubernetes automates deployment, scaling, and management of containerized applications.</span></span> <span data-ttu-id="f7f29-118">Többek között a következő funkciókat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="f7f29-118">It has a rich set of features including:</span></span>
* <span data-ttu-id="f7f29-119">Automatikus bin-csomagolás</span><span class="sxs-lookup"><span data-stu-id="f7f29-119">Automatic binpacking</span></span>
* <span data-ttu-id="f7f29-120">Önjavítás</span><span class="sxs-lookup"><span data-stu-id="f7f29-120">Self-healing</span></span>
* <span data-ttu-id="f7f29-121">Vízszintes méretezés</span><span class="sxs-lookup"><span data-stu-id="f7f29-121">Horizontal scaling</span></span>
* <span data-ttu-id="f7f29-122">Szolgáltatásészlelés és terheléselosztás</span><span class="sxs-lookup"><span data-stu-id="f7f29-122">Service discovery and load balancing</span></span>
* <span data-ttu-id="f7f29-123">Automatizált kibocsátások és visszaállítások</span><span class="sxs-lookup"><span data-stu-id="f7f29-123">Automated rollouts and rollbacks</span></span>
* <span data-ttu-id="f7f29-124">Titkos kódok és konfigurációk kezelése</span><span class="sxs-lookup"><span data-stu-id="f7f29-124">Secret and configuration management</span></span>
* <span data-ttu-id="f7f29-125">Tárolóvezénylés</span><span class="sxs-lookup"><span data-stu-id="f7f29-125">Storage orchestration</span></span>
* <span data-ttu-id="f7f29-126">Kötegelt végrehajtás</span><span class="sxs-lookup"><span data-stu-id="f7f29-126">Batch execution</span></span>

<span data-ttu-id="f7f29-127">Az Azure Container Service szolgáltatással üzembe helyezett Kubernetes architekturális diagramja:</span><span class="sxs-lookup"><span data-stu-id="f7f29-127">Architectural diagram of Kubernetes deployed via Azure Container Service:</span></span>

![Az Azure Tárolószolgáltatás toouse Kubernetes konfigurálva.](media/acs-intro/kubernetes.png)

## <a name="videos"></a><span data-ttu-id="f7f29-129">Videók</span><span class="sxs-lookup"><span data-stu-id="f7f29-129">Videos</span></span>

<span data-ttu-id="f7f29-130">A Kubernetes támogatása az Azure Container Service szolgáltatásban (Azure Friday, 2017. január):</span><span class="sxs-lookup"><span data-stu-id="f7f29-130">Kubernetes Support in Azure Container Services (Azure Friday, January 2017):</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Kubernetes-Support-in-Azure-Container-Services/player]
>
>

<span data-ttu-id="f7f29-131">Eszközök az alkalmazások fejlesztéséhez és üzembe helyezéséhez a Kubernetes szolgáltatásban (Azure OpenDev, 2017. június):</span><span class="sxs-lookup"><span data-stu-id="f7f29-131">Tools for Developing and Deploying Applications on Kubernetes (Azure OpenDev, June 2017):</span></span>

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

## <a name="next-steps"></a><span data-ttu-id="f7f29-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f7f29-132">Next steps</span></span>

<span data-ttu-id="f7f29-133">Hello felfedezés [Kubernetes gyors üzembe helyezés](container-service-kubernetes-walkthrough.md) Azure Tárolószolgáltatás ma felfedezése toobegin.</span><span class="sxs-lookup"><span data-stu-id="f7f29-133">Explore hello [Kubernetes Quickstart](container-service-kubernetes-walkthrough.md) toobegin exploring Azure Container Service today.</span></span>
