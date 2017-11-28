---
title: "Hibaelhárítás az esemény-nyomkövetés |} Microsoft Docs"
description: "A leggyakoribb problémák, a Microsoft Azure Service Fabric szolgáltatások üzembe helyezése során."
services: service-fabric
documentationcenter: .net
author: mattrowmsft
manager: timlt
editor: 
ms.assetid: 5eb8ef21-da04-4ac8-8b9a-5f7ff1e0a180
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/31/2016
ms.author: mattrow
redirect_url: /azure/service-fabric/service-fabric-diagnostics-overview
ms.openlocfilehash: e60bd4b9291cb2fc748921e42f11f54bb545984f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a><span data-ttu-id="13df5-103">Gyakori problémák elhárításához, az Azure Service Fabric szolgáltatások telepítésekor</span><span class="sxs-lookup"><span data-stu-id="13df5-103">Troubleshoot common issues when you deploy services on Azure Service Fabric</span></span>
<span data-ttu-id="13df5-104">Amikor a szolgáltatások a fejlesztői számítógépen futtatja, nem könnyen használható [Visual Studio eszközök a hibakeresés](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="13df5-104">When you're running services on your developer computer, it is easy to use [Visual Studio's debugging tools](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span> <span data-ttu-id="13df5-105">Távoli fürtök [állapotjelentések](service-fabric-view-entities-aggregated-health.md) a rendszer mindig remek kezdőpont.</span><span class="sxs-lookup"><span data-stu-id="13df5-105">For remote clusters, [health reports](service-fabric-view-entities-aggregated-health.md) are always a good place to start.</span></span> <span data-ttu-id="13df5-106">Ezek a jelentések eléréséhez legegyszerűbb módja a PowerShell segítségével vannak vagy [SFX](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="13df5-106">The easiest ways to access these reports are through PowerShell or [SFX](service-fabric-visualizing-your-cluster.md).</span></span> <span data-ttu-id="13df5-107">Ez a cikk feltételezi, hogy egy távoli fürt hibakeresést, és megismerheti, hogyan használhatja bármelyik eszközt.</span><span class="sxs-lookup"><span data-stu-id="13df5-107">This article assumes that you are debugging a remote cluster and have a basic understanding of how to use either of these tools.</span></span>

## <a name="application-crash"></a><span data-ttu-id="13df5-108">Alkalmazás-összeomlások</span><span class="sxs-lookup"><span data-stu-id="13df5-108">Application crash</span></span>
<span data-ttu-id="13df5-109">A "partíció nem éri el a cél replika- vagy példányszámot" jelentés jól jelzi, hogy a szolgáltatás összeomló rendszer.</span><span class="sxs-lookup"><span data-stu-id="13df5-109">The "Partition is below target replica or instance count" report is a good indication that your service is crashing.</span></span> <span data-ttu-id="13df5-110">Ha szeretné tudni, ahol a szolgáltatás összeomló veszi kissé további vizsgálat.</span><span class="sxs-lookup"><span data-stu-id="13df5-110">To find out where your service is crashing takes a little more investigation.</span></span> <span data-ttu-id="13df5-111">Ha a szolgáltatás léptékű fut, a legjobb barátjának lesz jól átgondolt nyomkövetések készlete.</span><span class="sxs-lookup"><span data-stu-id="13df5-111">When your service is running at scale, your best friend will be a set of well-thought-out traces.</span></span>  <span data-ttu-id="13df5-112">Javasoljuk, hogy meg [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) ezeket a nyomkövetéseket gyűjt, és olyan megoldást használni, mint [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) megtekintéséhez és a Keresés a nyomkövetési adatokat.</span><span class="sxs-lookup"><span data-stu-id="13df5-112">We suggest that you try [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) for collecting those traces and using a solution such as [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) for viewing and searching the traces.</span></span>

![SFX partíció állapota](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

### <a name="during-service-or-actor-initialization"></a><span data-ttu-id="13df5-114">Szolgáltatás vagy szereplő inicializálása közben</span><span class="sxs-lookup"><span data-stu-id="13df5-114">During service or actor initialization</span></span>
<span data-ttu-id="13df5-115">A kivételeket a szolgáltatás típusának inicializálása előtt, akkor a folyamat összeomolhat.</span><span class="sxs-lookup"><span data-stu-id="13df5-115">Any exceptions before the service type is initialized will cause the process to crash.</span></span> <span data-ttu-id="13df5-116">Az ilyen típusú összeomlik az alkalmazások eseménynaplójában a hiba a szolgáltatás jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="13df5-116">For these types of crashes, the application event log will show the error from your service.</span></span>
<span data-ttu-id="13df5-117">Ezek azok a leggyakrabban használt kivételeket tekintse meg a szolgáltatás inicializálása előtt.</span><span class="sxs-lookup"><span data-stu-id="13df5-117">These are the most common exceptions to see before the service is initialized.</span></span>

<span data-ttu-id="13df5-118">***System.IO.FileNotFoundException***</span><span class="sxs-lookup"><span data-stu-id="13df5-118">***System.IO.FileNotFoundException***</span></span>

<span data-ttu-id="13df5-119">Ez a hiba gyakran okozza hiányzik a szerelvény függőségeit.</span><span class="sxs-lookup"><span data-stu-id="13df5-119">This error is often due to missing assembly dependencies.</span></span> <span data-ttu-id="13df5-120">Ellenőrizze a Visual Studio vagy a globális szerelvény-gyorsítótárban, a csomópont CopyLocal tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="13df5-120">Check the CopyLocal property in Visual Studio or the global assembly cache for the node.</span></span>

<span data-ttu-id="13df5-121">***System.Runtime.InteropServices.COMException*** *System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory):*</span><span class="sxs-lookup"><span data-stu-id="13df5-121">***System.Runtime.InteropServices.COMException*** *at System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory(IntPtr, IFabricStatefulServiceFactory)*</span></span>

 <span data-ttu-id="13df5-122">Ez azt jelzi, hogy a regisztrált típus neve nem egyezik meg a szolgáltatás jegyzékfájlt.</span><span class="sxs-lookup"><span data-stu-id="13df5-122">This indicates that the registered service type name does not match the service manifest.</span></span>

<span data-ttu-id="13df5-123">[Az Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) beállítható úgy, hogy az alkalmazások eseménynaplójában keresse meg az összes csomópont automatikus feltöltése.</span><span class="sxs-lookup"><span data-stu-id="13df5-123">[Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) can be configured to upload the application event log for all your nodes automatically.</span></span>

### <a name="runasync-or-onactivateasync"></a><span data-ttu-id="13df5-124">RunAsync() vagy OnActivateAsync()</span><span class="sxs-lookup"><span data-stu-id="13df5-124">RunAsync() or OnActivateAsync()</span></span>
<span data-ttu-id="13df5-125">Ha a összeomlási történik, az inicializálás közben, vagy a regisztrált szolgáltatástípus vagy szereplő alkalmazást, a kivétel kezelve lesz Azure Service Fabric által.</span><span class="sxs-lookup"><span data-stu-id="13df5-125">If the crash happens during the initialization or running of your registered service type or actor, the exception will be caught by Azure Service Fabric.</span></span> <span data-ttu-id="13df5-126">Ezek a "További lépések" szakaszban részletes EventSource szolgáltatóktól tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="13df5-126">You can view these from the EventSource providers detailed in the "Next steps" section.</span></span>

## <a name="next-steps"></a><span data-ttu-id="13df5-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13df5-127">Next steps</span></span>
<span data-ttu-id="13df5-128">Ismerje meg a Service Fabric által biztosított meglévő diagnosztika:</span><span class="sxs-lookup"><span data-stu-id="13df5-128">Learn more about existing diagnostics provided by Service Fabric:</span></span>

* [<span data-ttu-id="13df5-129">Megbízható szereplője diagnosztika</span><span class="sxs-lookup"><span data-stu-id="13df5-129">Reliable Actors diagnostics</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="13df5-130">Megbízható Services diagnosztika</span><span class="sxs-lookup"><span data-stu-id="13df5-130">Reliable Services diagnostics</span></span>](service-fabric-reliable-services-diagnostics.md)

