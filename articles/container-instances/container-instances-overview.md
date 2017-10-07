---
title: "aaaAzure tároló példányok áttekintése |} Az Azure Docs"
description: "Az Azure Container Instances ismertetése"
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
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c0662ede1260b15d9841bfc2c3c4cec4c30338d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-instances"></a><span data-ttu-id="a8ca3-103">Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="a8ca3-103">Azure Container Instances</span></span>

<span data-ttu-id="a8ca3-104">Tárolók gyorsan egyre hello előnyben részesített módja toopackage, telepítéséhez és felügyeletéhez a felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-104">Containers are quickly becoming hello preferred way toopackage, deploy, and manage cloud applications.</span></span> <span data-ttu-id="a8ca3-105">Azure tároló példányok hello leggyorsabb és legegyszerűbb módja toorun Azure, a tároló, anélkül, hogy tooprovision bármely virtuális gépek és anélkül, hogy tooadopt egy magasabb szintű szolgáltatást kínál.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-105">Azure Container Instances offers hello fastest and simplest way toorun a container in Azure, without having tooprovision any virtual machines and without having tooadopt a higher-level service.</span></span> 

<span data-ttu-id="a8ca3-106">Az Azure Container Instances ideális megoldás minden olyan forgatókönyv esetében, amely elkülönített tárolókban valósulhat meg, beleértve az egyszerű alkalmazásokat, a tevékenységek automatizálását és a buildfeladatokat.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-106">Azure Container Instances is a great solution for any scenario that can operate in isolated containers, including simple applications, task automation, and build jobs.</span></span> <span data-ttu-id="a8ca3-107">Forgatókönyvek, ahol teljes van szükség a tároló vezénylési, beleértve a szolgáltatásészlelés több tárolók, automatikus skálázás és koordinált alkalmazásfrissítések, azt javasoljuk, hello [Azure Tárolószolgáltatás](https://docs.microsoft.com/azure/container-service/).</span><span class="sxs-lookup"><span data-stu-id="a8ca3-107">For scenarios where you need full container orchestration, including service discovery across multiple containers, automatic scaling, and coordinated application upgrades, we recommend hello [Azure Container Service](https://docs.microsoft.com/azure/container-service/).</span></span>

## <a name="fast-startup-times"></a><span data-ttu-id="a8ca3-108">Rövid indítási idők</span><span class="sxs-lookup"><span data-stu-id="a8ca3-108">Fast startup times</span></span>

<span data-ttu-id="a8ca3-109">A tárolók jelentős előnyöket nyújtanak a virtuális gépekkel szemben az indítás terén.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-109">Containers offer significant startup benefits over virtual machines.</span></span> <span data-ttu-id="a8ca3-110">Azure-tároló osztályt indítsa el a tároló nélkül hello kell tooprovision másodpercben az Azure-ban, és a virtuális gépek kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-110">With Azure Container Instances, you can start a container in Azure in seconds without hello need tooprovision and manage VMs.</span></span>

## <a name="hypervisor-level-security"></a><span data-ttu-id="a8ca3-111">Hipervizorszintű biztonság</span><span class="sxs-lookup"><span data-stu-id="a8ca3-111">Hypervisor-level security</span></span>

<span data-ttu-id="a8ca3-112">Korábban a tárolók biztosítottak ugyan alkalmazásfüggőség-elkülönítési és erőforrás-szabályozási funkciót, azonban nem voltak eléggé megerősítve a rosszindulatú több-bérlős alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-112">Historically, containers have offered application dependency isolation and resource governance but have not been considered sufficiently hardened for hostile multi-tenant usage.</span></span> <span data-ttu-id="a8ca3-113">Az Azure Container Instances használatakor az alkalmazások ugyanannyira el vannak különítve a tárolóban, mintha egy virtuális gépen futnának.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-113">With Azure Container Instances, your application is as isolated in a container as it would be in a VM.</span></span>

## <a name="custom-sizes"></a><span data-ttu-id="a8ca3-114">Egyéni méretek</span><span class="sxs-lookup"><span data-stu-id="a8ca3-114">Custom sizes</span></span>

<span data-ttu-id="a8ca3-115">Tárolók csak egyetlen alkalmazás, de ezeket az alkalmazásokat hello pontos igényeinek nagy mértékben eltérhetnek általában optimalizált toorun.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-115">Containers are typically optimized toorun just a single application, but hello exact needs of those applications can differ greatly.</span></span> <span data-ttu-id="a8ca3-116">Az Azure Container Instanceszel pontosan annyi processzormagot és memóriát igényelhet, amennyire szüksége van.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-116">With Azure Container Instances, you can request exactly what you need in terms of CPU cores and memory.</span></span> <span data-ttu-id="a8ca3-117">Után kell fizetnie függően mi kér, hello által kiszámlázott második, így finom optimalizálhatja kiadásait igényei szerint.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-117">You pay based on what you request, billed by hello second, so you can finely optimize your spending based on your needs.</span></span>

## <a name="public-ip-connectivity"></a><span data-ttu-id="a8ca3-118">Nyilvános IP-kapcsolat</span><span class="sxs-lookup"><span data-stu-id="a8ca3-118">Public IP connectivity</span></span>

<span data-ttu-id="a8ca3-119">Azure tároló példányaival is elérhetővé teheti a tárolók közvetlenül toohello internet nyilvános IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-119">With Azure Container Instances, you can expose your containers directly toohello internet with a public IP address.</span></span> <span data-ttu-id="a8ca3-120">Jövőbeli hello azt fogja bontsa ki a hálózati képességekkel tooinclude integráció a virtuális hálózatok, terheléselosztók, és más alapvető hello Azure hálózati infrastruktúra betölteni.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-120">In hello future, we will expand our networking capabilities tooinclude integration with virtual networks, load balancers, and other core parts of hello Azure networking infrastructure.</span></span>

## <a name="persistent-storage"></a><span data-ttu-id="a8ca3-121">Állandó tárolók</span><span class="sxs-lookup"><span data-stu-id="a8ca3-121">Persistent storage</span></span>

<span data-ttu-id="a8ca3-122">tooretrieve és Azure tároló osztályt állapotban maradnak, kínálunk közvetlen csatlakoztatása az Azure-megosztások fájlokat.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-122">tooretrieve and persist state with Azure Container Instances, we offer direct mounting of Azure files shares.</span></span>

## <a name="linux-and-windows-containers"></a><span data-ttu-id="a8ca3-123">Linux- és Windows-tárolók</span><span class="sxs-lookup"><span data-stu-id="a8ca3-123">Linux and Windows containers</span></span>

<span data-ttu-id="a8ca3-124">Azure tároló példányaival ütemezéssel megadhatja, hogy mindkét Windows és Linux tárolók hello azonos API.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-124">With Azure Container Instances, you can schedule both Windows and Linux containers with hello same API.</span></span> <span data-ttu-id="a8ca3-125">Egyszerűen jelezheti hello alap operációs rendszer típusa, és minden más azonos.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-125">Simply indicate hello base OS type and everything else is identical.</span></span>

## <a name="co-scheduled-groups"></a><span data-ttu-id="a8ca3-126">Együttesen ütemezett csoportok</span><span class="sxs-lookup"><span data-stu-id="a8ca3-126">Co-scheduled groups</span></span>

<span data-ttu-id="a8ca3-127">Az Azure Container Instances támogatja az olyan több tárolóból álló csoportok ütemezését, amelyek osztoznak a gazdagépen, a helyi hálózaton és a tárolón, valamint azonos az életciklusuk.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-127">Azure Container Instances supports scheduling of multi-container groups that share a host machine, local network, storage, and lifecycle.</span></span> <span data-ttu-id="a8ca3-128">Ez lehetővé teszi, hogy Ön toocombine másokkal a fő alkalmazás támogató szerepkörben, például naplózásra működött.</span><span class="sxs-lookup"><span data-stu-id="a8ca3-128">This enables you toocombine your main application with others acting in a supporting role, such as logging.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8ca3-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a8ca3-129">Next steps</span></span>

<span data-ttu-id="a8ca3-130">Próbálja üzembe helyezni a tároló tooAzure egy egyetlen parancs használatával a [gyors üzembe helyezési útmutató](container-instances-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="a8ca3-130">Try deploying a container tooAzure with a single command using our [quickstart guide](container-instances-quickstart.md).</span></span>
