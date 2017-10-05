---
title: "Az Azure Service Fabric mintákat és forgatókönyvek |} Microsoft Docs"
description: "Gyakorlati tanácsokat és bizonyítása, újrahasznosítható minták megtervezésére, fejleszthet, és a Service Fabric a mikroszolgáltatások működnek."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: d5aa75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2017
ms.author: ryanwi
ms.openlocfilehash: fb2fa495758433e357722427b1c162420935955d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="service-fabric-patterns-and-scenarios"></a><span data-ttu-id="939a8-103">A Service Fabric mintákat és forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="939a8-103">Service Fabric patterns and scenarios</span></span>
<span data-ttu-id="939a8-104">Ha felépítése az Azure Service Fabric használatával a nagyméretű mikroszolgáltatások tartózkodik, ismerje meg, a szakértők, akik erre a platformra beépített szolgáltatásként (PaaS), és a.</span><span class="sxs-lookup"><span data-stu-id="939a8-104">If you’re looking at building large-scale microservices using Azure Service Fabric, learn from the experts who designed and built this platform as a service (PaaS).</span></span> <span data-ttu-id="939a8-105">Ismerkedés a megfelelő architektúra, és majd megtudhatja, hogyan optimalizálható az erőforrások az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="939a8-105">Get started with proper architecture, and then learn how to optimize resources for your application.</span></span> <span data-ttu-id="939a8-106">A [Service Fabric minták és gyakorlatok](https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344) működés során a leggyakrabban ismételt valós ügyfelek Service Fabric forgatókönyvek és alkalmazási területek kérdésekre.</span><span class="sxs-lookup"><span data-stu-id="939a8-106">The [Service Fabric Patterns and Practices](https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344) course answers the questions most often asked by real-world customers about Service Fabric scenarios and application areas.</span></span>
 
<span data-ttu-id="939a8-107">Megtudhatja, hogyan lehet a Service Fabricben mikroszolgáltatásokat megtervezni, fejleszteni és működtetni, és ehhez ajánlott eljárásokat és kipróbált, újrahasznosítható mintákat is kap.</span><span class="sxs-lookup"><span data-stu-id="939a8-107">Find out how to design, develop, and operate your microservices on Service Fabric using best practices and proven, reusable patterns.</span></span> <span data-ttu-id="939a8-108">A Service Fabric áttekintést kaphat, és ezután alaposabban tanulmányozhatja a mély témakörök, amelyek közé tartozik a fürt optimalizálása és a biztonsági, örökölt alkalmazások áttelepítése léptékű játék motorok és egyéb üzemeltetési IoT.</span><span class="sxs-lookup"><span data-stu-id="939a8-108">Get an overview of Service Fabric and then dive deep into topics that cover cluster optimization and security, migrating legacy apps, IoT at scale, hosting game engines, and more.</span></span> <span data-ttu-id="939a8-109">Nézze meg a különböző munkaterhelések folyamatos kézbesítési, és a Linux-támogatás és a tárolók a részleteket még beolvasása.</span><span class="sxs-lookup"><span data-stu-id="939a8-109">Look at continuous delivery for various workloads, and even get the details on Linux support and containers.</span></span> 

## <a name="introduction"></a><span data-ttu-id="939a8-110">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="939a8-110">Introduction</span></span>
<span data-ttu-id="939a8-111">Gyakorlati tanácsok vizsgálatát, és Sajátítsa el kiválasztására vonatkozó platform (PaaS) szolgáltatás keresztül infrastruktúra (IaaS) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="939a8-111">Explore best practices, and learn about choosing platform as a service (PaaS) over infrastructure as a service (IaaS).</span></span> <span data-ttu-id="939a8-112">Ismerje meg a részleteket a következő bevált alkalmazás tervezési alapelvek.</span><span class="sxs-lookup"><span data-stu-id="939a8-112">Get the details on following proven application design principles.</span></span>

<table><tr><th><span data-ttu-id="939a8-113">Videó</span><span class="sxs-lookup"><span data-stu-id="939a8-113">Video</span></span></th><th><span data-ttu-id="939a8-114">PowerPoint fedélzet</span><span class="sxs-lookup"><span data-stu-id="939a8-114">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=N2KwbbSGD_6405167344">
<img src="./media/service-fabric-patterns-and-scenarios/intro.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="939a8-115"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344">A Service Fabric bemutatása</a></span><span class="sxs-lookup"><span data-stu-id="939a8-115"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344">Introduction to Service Fabric</a></span></span></td></tr>
</table>

## <a name="cluster-planning-and-management"></a><span data-ttu-id="939a8-116">Tervezési és a felügyeleti fürtön</span><span class="sxs-lookup"><span data-stu-id="939a8-116">Cluster planning and management</span></span>
<span data-ttu-id="939a8-117">További tudnivalók a kapacitástervezés, a fürt optimalizálása és a fürt biztonsági, az Azure Service Fabric nézze meg a.</span><span class="sxs-lookup"><span data-stu-id="939a8-117">Learn about capacity planning, cluster optimization, and cluster security, in this look at Azure Service Fabric.</span></span>

<table><tr><th><span data-ttu-id="939a8-118">Videó</span><span class="sxs-lookup"><span data-stu-id="939a8-118">Video</span></span></th><th><span data-ttu-id="939a8-119">PowerPoint fedélzet</span><span class="sxs-lookup"><span data-stu-id="939a8-119">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=cyDYZcSGD_2805167344">
<img src="./media/service-fabric-patterns-and-scenarios/cluster.png" WIDTH="360" HEIGHT="244">
</a></td><td> <span data-ttu-id="939a8-120"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=E5B3nJSGD_805167344">Fürt tervezési és kezelése</a></span><span class="sxs-lookup"><span data-stu-id="939a8-120"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=E5B3nJSGD_805167344">Cluster Planning and Management</a></span></span></td></tr>
</table>

## <a name="hyper-scale-web"></a><span data-ttu-id="939a8-121">Hyper-skálázási webes</span><span class="sxs-lookup"><span data-stu-id="939a8-121">Hyper-scale web</span></span>
<span data-ttu-id="939a8-122">Körül kapacitású webalkalmazás rendelkezésre állásának és megbízhatóságának, kapacitású és állapotkezelés fogalmak áttekintése.</span><span class="sxs-lookup"><span data-stu-id="939a8-122">Review concepts around hyper-scale web, including availability and reliability, hyper-scale, and state management.</span></span>

<table><tr><th><span data-ttu-id="939a8-123">Videó</span><span class="sxs-lookup"><span data-stu-id="939a8-123">Video</span></span></th><th><span data-ttu-id="939a8-124">PowerPoint fedélzet</span><span class="sxs-lookup"><span data-stu-id="939a8-124">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=NgldAdSGD_405167344">
<img src="./media/service-fabric-patterns-and-scenarios/hyperscaleweb.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="939a8-125"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=CPMLBLSGD_7705167344">Hyper-skálázási webes</a></span><span class="sxs-lookup"><span data-stu-id="939a8-125"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=CPMLBLSGD_7705167344">Hyper-scale web</a></span></span></td></tr>
</table>

## <a name="iot"></a><span data-ttu-id="939a8-126">IoT</span><span class="sxs-lookup"><span data-stu-id="939a8-126">IoT</span></span>
<span data-ttu-id="939a8-127">Megismerkedhet a az eszközök internetes hálózatát (IoT) Azure Service Fabric, beleértve az Azure IoT folyamat, a több-bérlős és a IoT léptékű környezetében.</span><span class="sxs-lookup"><span data-stu-id="939a8-127">Explore the Internet of Things (IoT) in the context of Azure Service Fabric, including the Azure IoT pipeline, multi-tenancy, and IoT at scale.</span></span>

<table><tr><th><span data-ttu-id="939a8-128">Videó</span><span class="sxs-lookup"><span data-stu-id="939a8-128">Video</span></span></th><th><span data-ttu-id="939a8-129">PowerPoint fedélzet</span><span class="sxs-lookup"><span data-stu-id="939a8-129">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=naFUVeSGD_1505167344">
<img src="./media/service-fabric-patterns-and-scenarios/iot.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="939a8-130"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">IoT</a></span><span class="sxs-lookup"><span data-stu-id="939a8-130"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">IoT</a></span></span></td></tr>
</table>

## <a name="gaming"></a><span data-ttu-id="939a8-131">Játékok</span><span class="sxs-lookup"><span data-stu-id="939a8-131">Gaming</span></span>
<span data-ttu-id="939a8-132">Nézze meg a kapcsolja játékokat, interaktív játékok és meglévő játék motorok üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="939a8-132">Look at turn-based games, interactive games, and hosting existing game engines.</span></span>

<table><tr><th><span data-ttu-id="939a8-133">Videó</span><span class="sxs-lookup"><span data-stu-id="939a8-133">Video</span></span></th><th><span data-ttu-id="939a8-134">PowerPoint fedélzet</span><span class="sxs-lookup"><span data-stu-id="939a8-134">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=6wECzeSGD_3805167344">
<img src="./media/service-fabric-patterns-and-scenarios/gaming.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="939a8-135"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">Játék</a></span><span class="sxs-lookup"><span data-stu-id="939a8-135"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">Gaming</a></span></span></td></tr>
</table>

## <a name="continuous-delivery"></a><span data-ttu-id="939a8-136">Folyamatos készregyártás</span><span class="sxs-lookup"><span data-stu-id="939a8-136">Continuous delivery</span></span>
<span data-ttu-id="939a8-137">Megismerkedhet a fogalmakat, beleértve a Visual Studio Team Services, a munkafolyamat build/csomag/közzététele, a több környezet beállítása és a szolgáltatás csomagmegosztás folyamatos integráció/folyamatos kézbesítési.</span><span class="sxs-lookup"><span data-stu-id="939a8-137">Explore concepts, including continuous integration/continuous delivery with Visual Studio Team Services, build/package/publish workflow, multi-environment setup, and service package/share.</span></span>

<table><tr><th><span data-ttu-id="939a8-138">Videó</span><span class="sxs-lookup"><span data-stu-id="939a8-138">Video</span></span></th><th><span data-ttu-id="939a8-139">PowerPoint fedélzet</span><span class="sxs-lookup"><span data-stu-id="939a8-139">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=78h5ofSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/cd.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="939a8-140"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=VlENvOSGD_105167344">Folyamatos kézbesítés</a></span><span class="sxs-lookup"><span data-stu-id="939a8-140"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=VlENvOSGD_105167344">Continuous delivery</a></span></span></td></tr>
</table>

## <a name="migration"></a><span data-ttu-id="939a8-141">Migrálás</span><span class="sxs-lookup"><span data-stu-id="939a8-141">Migration</span></span>
<span data-ttu-id="939a8-142">Ismerje meg az áttelepítés egy felhőalapú szolgáltatás, az örökölt alkalmazások áttelepítése mellett.</span><span class="sxs-lookup"><span data-stu-id="939a8-142">Learn about migrating from a cloud service, in addition to migration of legacy apps.</span></span>

<table><tr><th><span data-ttu-id="939a8-143">Videó</span><span class="sxs-lookup"><span data-stu-id="939a8-143">Video</span></span></th><th><span data-ttu-id="939a8-144">PowerPoint fedélzet</span><span class="sxs-lookup"><span data-stu-id="939a8-144">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=hd1cMgSGD_5605167344">
<img src="./media/service-fabric-patterns-and-scenarios/migration.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="939a8-145"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=GQAq4QSGD_8305167344">Áttelepítés</a></span><span class="sxs-lookup"><span data-stu-id="939a8-145"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=GQAq4QSGD_8305167344">Migration</a></span></span></td></tr>
</table>

## <a name="containers-and-linux-support"></a><span data-ttu-id="939a8-146">Tárolók és a Linux-támogatás</span><span class="sxs-lookup"><span data-stu-id="939a8-146">Containers and Linux support</span></span>
<span data-ttu-id="939a8-147">A válasz a kérdésre "Miért tárolók?"</span><span class="sxs-lookup"><span data-stu-id="939a8-147">Get the answer to the question, "Why containers?"</span></span> <span data-ttu-id="939a8-148">További tudnivalók a Windows-tárolók, támogatja a Linux és Linux tárolók vezénylési előzetes verzióját.</span><span class="sxs-lookup"><span data-stu-id="939a8-148">Learn about the preview for Windows containers, Linux supports, and Linux containers orchestration.</span></span> <span data-ttu-id="939a8-149">Valamint megtudhatja, hogyan telepíthetők át a .NET Core alkalmazások Linux.</span><span class="sxs-lookup"><span data-stu-id="939a8-149">Plus, find out how to migrate .NET Core apps to Linux.</span></span>

<table><tr><th><span data-ttu-id="939a8-150">Videó</span><span class="sxs-lookup"><span data-stu-id="939a8-150">Video</span></span></th><th><span data-ttu-id="939a8-151">PowerPoint fedélzet</span><span class="sxs-lookup"><span data-stu-id="939a8-151">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=V1ERJhSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/containers.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="939a8-152"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mlYsZRSGD_2105167344">Tárolók és a Linux-támogatás</a></span><span class="sxs-lookup"><span data-stu-id="939a8-152"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mlYsZRSGD_2105167344">Containers and Linux support</a></span></span></td></tr>
</table>

## <a name="next-steps"></a><span data-ttu-id="939a8-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="939a8-153">Next steps</span></span>
<span data-ttu-id="939a8-154">Most, hogy megismerte a Service Fabric mintákat és forgatókönyvek, további tudnivalókat arról, hogy hogyan [létrehozása és kezelése a fürtök](service-fabric-deploy-anywhere.md), [Felhőszolgáltatások alkalmazásokat telepíthet át a Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md), [folyamatos kézbesítési beállítása](service-fabric-set-up-continuous-integration.md), és [tároló üzembe helyezése](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="939a8-154">Now that you've learned about Service Fabric patterns and scenarios, read more about how to [create and manage clusters](service-fabric-deploy-anywhere.md), [migrate Cloud Services apps to Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md), [set up continuous delivery](service-fabric-set-up-continuous-integration.md), and [deploy containers](service-fabric-containers-overview.md).</span></span>
