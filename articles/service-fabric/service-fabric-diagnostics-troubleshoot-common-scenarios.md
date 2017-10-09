---
title: "az esemény-nyomkövetés aaaTroubleshooting |} Microsoft Docs"
description: "hello leggyakoribb problémákat észlelt a Microsoft Azure Service Fabric szolgáltatások telepítése közben."
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
ms.openlocfilehash: f5adb7e15fa1e2c964bbbc5726246630c95e13f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a><span data-ttu-id="b74b2-103">Gyakori problémák elhárításához, az Azure Service Fabric szolgáltatások telepítésekor</span><span class="sxs-lookup"><span data-stu-id="b74b2-103">Troubleshoot common issues when you deploy services on Azure Service Fabric</span></span>
<span data-ttu-id="b74b2-104">Szolgáltatások a fejlesztői számítógépen futtatja, esetén könnyen toouse [Visual Studio eszközök a hibakeresés](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="b74b2-104">When you're running services on your developer computer, it is easy toouse [Visual Studio's debugging tools](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span> <span data-ttu-id="b74b2-105">Távoli fürtök [állapotjelentések](service-fabric-view-entities-aggregated-health.md) a rendszer mindig a megfelelő hely toostart.</span><span class="sxs-lookup"><span data-stu-id="b74b2-105">For remote clusters, [health reports](service-fabric-view-entities-aggregated-health.md) are always a good place toostart.</span></span> <span data-ttu-id="b74b2-106">Ezek a jelentések megtalálhatók a Powershellen keresztül legegyszerűbb módja tooaccess hello vagy [SFX](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="b74b2-106">hello easiest ways tooaccess these reports are through PowerShell or [SFX](service-fabric-visualizing-your-cluster.md).</span></span> <span data-ttu-id="b74b2-107">Ez a cikk feltételezi, hogy egy távoli fürt hibakeresést, és megismerheti, hogyan toouse mindkét eszközei.</span><span class="sxs-lookup"><span data-stu-id="b74b2-107">This article assumes that you are debugging a remote cluster and have a basic understanding of how toouse either of these tools.</span></span>

## <a name="application-crash"></a><span data-ttu-id="b74b2-108">Alkalmazás-összeomlások</span><span class="sxs-lookup"><span data-stu-id="b74b2-108">Application crash</span></span>
<span data-ttu-id="b74b2-109">hello "partíció nem éri el a cél replika- vagy példányszámot" jelentés jól jelzi, hogy a szolgáltatás összeomló rendszer.</span><span class="sxs-lookup"><span data-stu-id="b74b2-109">hello "Partition is below target replica or instance count" report is a good indication that your service is crashing.</span></span> <span data-ttu-id="b74b2-110">toofind, ahol a szolgáltatás összeomló egy kis további vizsgálat szükséges.</span><span class="sxs-lookup"><span data-stu-id="b74b2-110">toofind out where your service is crashing takes a little more investigation.</span></span> <span data-ttu-id="b74b2-111">Ha a szolgáltatás léptékű fut, a legjobb barátjának lesz jól átgondolt nyomkövetések készlete.</span><span class="sxs-lookup"><span data-stu-id="b74b2-111">When your service is running at scale, your best friend will be a set of well-thought-out traces.</span></span>  <span data-ttu-id="b74b2-112">Javasoljuk, hogy meg [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) ezeket a nyomkövetéseket gyűjt, és olyan megoldást használni, mint [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) megtekintése és keresése hello nyomkövetési adatokat.</span><span class="sxs-lookup"><span data-stu-id="b74b2-112">We suggest that you try [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) for collecting those traces and using a solution such as [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) for viewing and searching hello traces.</span></span>

![SFX partíció állapota](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

### <a name="during-service-or-actor-initialization"></a><span data-ttu-id="b74b2-114">Szolgáltatás vagy szereplő inicializálása közben</span><span class="sxs-lookup"><span data-stu-id="b74b2-114">During service or actor initialization</span></span>
<span data-ttu-id="b74b2-115">Hello szolgáltatás típusának inicializálása előtt kivételek hello folyamat toocrash miatt.</span><span class="sxs-lookup"><span data-stu-id="b74b2-115">Any exceptions before hello service type is initialized will cause hello process toocrash.</span></span> <span data-ttu-id="b74b2-116">Az ilyen típusú összeomlik hello alkalmazások eseménynaplójában keresse meg a szolgáltatásból hello hiba jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b74b2-116">For these types of crashes, hello application event log will show hello error from your service.</span></span>
<span data-ttu-id="b74b2-117">Ezek a hello leggyakoribb kivételek toosee hello szolgáltatás inicializálása előtt.</span><span class="sxs-lookup"><span data-stu-id="b74b2-117">These are hello most common exceptions toosee before hello service is initialized.</span></span>

<span data-ttu-id="b74b2-118">***System.IO.FileNotFoundException***</span><span class="sxs-lookup"><span data-stu-id="b74b2-118">***System.IO.FileNotFoundException***</span></span>

<span data-ttu-id="b74b2-119">Ez a hiba gyakran toomissing szerelvény függőségek miatt van.</span><span class="sxs-lookup"><span data-stu-id="b74b2-119">This error is often due toomissing assembly dependencies.</span></span> <span data-ttu-id="b74b2-120">Ellenőrizze a Visual Studio vagy a globális szerelvény-gyorsítótárban hello hello csomópont hello CopyLocal tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b74b2-120">Check hello CopyLocal property in Visual Studio or hello global assembly cache for hello node.</span></span>

<span data-ttu-id="b74b2-121">***System.Runtime.InteropServices.COMException*** *System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory):*</span><span class="sxs-lookup"><span data-stu-id="b74b2-121">***System.Runtime.InteropServices.COMException*** *at System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory(IntPtr, IFabricStatefulServiceFactory)*</span></span>

 <span data-ttu-id="b74b2-122">Ez azt jelzi, hogy hello regisztrált típus neve nem egyezik meg a hello szolgáltatás jegyzék.</span><span class="sxs-lookup"><span data-stu-id="b74b2-122">This indicates that hello registered service type name does not match hello service manifest.</span></span>

<span data-ttu-id="b74b2-123">[Az Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) automatikusan lehet konfigurált tooupload hello alkalmazások eseménynaplójában keresse meg az összes csomópont.</span><span class="sxs-lookup"><span data-stu-id="b74b2-123">[Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) can be configured tooupload hello application event log for all your nodes automatically.</span></span>

### <a name="runasync-or-onactivateasync"></a><span data-ttu-id="b74b2-124">RunAsync() vagy OnActivateAsync()</span><span class="sxs-lookup"><span data-stu-id="b74b2-124">RunAsync() or OnActivateAsync()</span></span>
<span data-ttu-id="b74b2-125">Ha hello összeomlási hello inicializálása közben történik, vagy a regisztrált szolgáltatástípus vagy szereplő alkalmazást, hello kivétel kezelve lesz Azure Service Fabric által.</span><span class="sxs-lookup"><span data-stu-id="b74b2-125">If hello crash happens during hello initialization or running of your registered service type or actor, hello exception will be caught by Azure Service Fabric.</span></span> <span data-ttu-id="b74b2-126">Ezeket a hello EventSource szolgáltatók hello "További lépések" szakaszban ismertetett tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="b74b2-126">You can view these from hello EventSource providers detailed in hello "Next steps" section.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b74b2-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b74b2-127">Next steps</span></span>
<span data-ttu-id="b74b2-128">Ismerje meg a Service Fabric által biztosított meglévő diagnosztika:</span><span class="sxs-lookup"><span data-stu-id="b74b2-128">Learn more about existing diagnostics provided by Service Fabric:</span></span>

* [<span data-ttu-id="b74b2-129">Megbízható szereplője diagnosztika</span><span class="sxs-lookup"><span data-stu-id="b74b2-129">Reliable Actors diagnostics</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="b74b2-130">Megbízható Services diagnosztika</span><span class="sxs-lookup"><span data-stu-id="b74b2-130">Reliable Services diagnostics</span></span>](service-fabric-reliable-services-diagnostics.md)

