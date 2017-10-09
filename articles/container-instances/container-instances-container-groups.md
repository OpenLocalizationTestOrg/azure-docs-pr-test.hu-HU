---
title: "aaaAzure tároló példányok tárolócsoportok"
description: "Tárolócsoportok működése az Azure-tároló példányok ismertetése"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2b0e784609979045c8f77d7b6d0987ec3fee714c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="container-groups-in-azure-container-instances"></a><span data-ttu-id="d7e0f-103">Azure-tárolót példányát tárolócsoportok</span><span class="sxs-lookup"><span data-stu-id="d7e0f-103">Container Groups in Azure Container Instances</span></span>

<span data-ttu-id="d7e0f-104">legfelső szintű erőforrás hello Azure tároló példányát tároló olyan.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-104">hello top-level resource in Azure Container Instances is a container group.</span></span> <span data-ttu-id="d7e0f-105">Ez a cikk ismerteti a tárolócsoportok és forgatókönyvek milyen típusú engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-105">This article describes what container groups are and what types of scenarios they enable.</span></span>

## <a name="how-a-container-group-works"></a><span data-ttu-id="d7e0f-106">Egy tároló csoport működése</span><span class="sxs-lookup"><span data-stu-id="d7e0f-106">How a container group works</span></span>

<span data-ttu-id="d7e0f-107">Egy tároló csoport gyűjteménye, amelyek a hello ütemezhetők tároló azonos gép működtetéséhez, és egy életciklussal, helyi hálózati és tárolási köteteket.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-107">A container group is a collection of containers that get scheduled on hello same host machine and share a lifecycle, local network, and storage volumes.</span></span> <span data-ttu-id="d7e0f-108">Hasonló toohello fogalma egy *pod* a [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) és [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).</span><span class="sxs-lookup"><span data-stu-id="d7e0f-108">It is similar toohello concept of a *pod* in [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) and [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).</span></span>

<span data-ttu-id="d7e0f-109">hello alábbi ábrán egy példa látható egy tároló csoport, amely több tároló tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-109">hello following diagram shows an example of a container group that includes multiple containers.</span></span>

![Tároló csoportok – példa][container-groups-example]

<span data-ttu-id="d7e0f-111">Vegye figyelembe:</span><span class="sxs-lookup"><span data-stu-id="d7e0f-111">Note that:</span></span>

- <span data-ttu-id="d7e0f-112">hello csoport, egy önálló gazdagépen van ütemezve.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-112">hello group is scheduled on a single host machine.</span></span>
- <span data-ttu-id="d7e0f-113">hello csoport közzétesz egy egyetlen nyilvános IP-cím, egy kitett port.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-113">hello group exposes a single public IP address, with one exposed port.</span></span>
- <span data-ttu-id="d7e0f-114">hello csoport két tárolók tevődik össze.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-114">hello group is made up of two containers.</span></span> <span data-ttu-id="d7e0f-115">Egy tároló figyeli 80,-as porton, miközben hello más porton figyel 5000.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-115">One container listens on port 80, while hello other listens on port 5000.</span></span>
- <span data-ttu-id="d7e0f-116">hello csoport tartalmazza a kötet csatlakoztatások, két Azure fájlmegosztásokat, és minden tárolót csatlakoztatja egy helyileg hello megosztások.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-116">hello group includes two Azure file shares as volume mounts, and each container mounts one of hello shares locally.</span></span>

### <a name="networking"></a><span data-ttu-id="d7e0f-117">Hálózat</span><span class="sxs-lookup"><span data-stu-id="d7e0f-117">Networking</span></span>

<span data-ttu-id="d7e0f-118">Tárolócsoportok ossza meg IP-cím és port névtér az IP-címet.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-118">Container groups share an IP address and a port namespace on that IP address.</span></span> <span data-ttu-id="d7e0f-119">tooenable külső ügyfelek tooreach hello csoporton belüli tárolója, fel kell fednie a hello IP-címe és hello tárolóból hello port.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-119">tooenable external clients tooreach a container within hello group, you must expose hello port on hello IP address and from hello container.</span></span> <span data-ttu-id="d7e0f-120">Mert hello csoport tárolókra port névtér port hozzárendelése nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-120">Because containers within hello group share a port namespace, port mapping is not supported.</span></span> <span data-ttu-id="d7e0f-121">Tárolók egy csoporton belüli képes elérni egymást keresztül hello a localhost portok, amely ki van téve, akkor is, ha azokat a portokat nem érhetők el külsőleg hello csoport IP-címe.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-121">Containers within a group can reach each other via localhost on hello ports that they have exposed, even if those ports are not exposed externally on hello group's IP address.</span></span>

### <a name="storage"></a><span data-ttu-id="d7e0f-122">Storage</span><span class="sxs-lookup"><span data-stu-id="d7e0f-122">Storage</span></span>

<span data-ttu-id="d7e0f-123">Megadhatja, hogy külső kötetek toomount tároló csoporton belül.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-123">You can specify external volumes toomount within a container group.</span></span> <span data-ttu-id="d7e0f-124">A megadott elérési utak belül hello egyes tárolók egy csoport a köteteket is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-124">You can map those volumes into specific paths within hello individual containers in a group.</span></span>

## <a name="common-scenarios"></a><span data-ttu-id="d7e0f-125">Gyakori forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="d7e0f-125">Common scenarios</span></span>

<span data-ttu-id="d7e0f-126">Több tároló csoportok olyan hasznos olyan esetekben, ahol a tároló lemezképekre különböző csapatok által kézbesítése, és külön erőforrás követelményekkel rendelkezik néhány másolatot egyetlen működési feladat toodivide kívánt.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-126">Multi-container groups are useful in cases where you want toodivide up a single functional task into a small number of container images, which can be delivered by different teams and have separate resource requirements.</span></span> <span data-ttu-id="d7e0f-127">Példa használati lehetnek:</span><span class="sxs-lookup"><span data-stu-id="d7e0f-127">Example usage could include:</span></span>

- <span data-ttu-id="d7e0f-128">Egy alkalmazás-tárolót és a naplózási tároló.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-128">An application container and a logging container.</span></span> <span data-ttu-id="d7e0f-129">hello naplózási tároló hello fő alkalmazás hello naplók és a metrikák kimeneti gyűjt, és írja őket toolong távú tárolási.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-129">hello logging container collects hello logs and metrics output by hello main application and writes them toolong-term storage.</span></span>
- <span data-ttu-id="d7e0f-130">Az alkalmazások és a figyelési tároló.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-130">An application and a monitoring container.</span></span> <span data-ttu-id="d7e0f-131">rendszeres időközönként figyelési tároló hello lehetővé teszi egy kérelem toohello alkalmazás tooensure, hogy fut, és helytelenül válaszol, és riasztást küld, ha nem.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-131">hello monitoring container periodically makes a request toohello application tooensure that it's running and responding correctly and raises an alert if it's not.</span></span>
- <span data-ttu-id="d7e0f-132">Egy tároló kiszolgáló webalkalmazás és húzza a legújabb tartalom hello verziókövetésből tárolója.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-132">A container serving a web application and a container pulling hello latest content from source control.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7e0f-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d7e0f-133">Next steps</span></span>

<span data-ttu-id="d7e0f-134">Ismerje meg, hogyan túl[a több tároló csoport telepítése](container-instances-multi-container-group.md) az Azure Resource Manager sablonnal.</span><span class="sxs-lookup"><span data-stu-id="d7e0f-134">Learn how too[deploy a multi-container group](container-instances-multi-container-group.md) with an Azure Resource Manager template.</span></span>

<!-- IMAGES -->

[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png