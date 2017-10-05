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
# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Gyakori problémák elhárításához, az Azure Service Fabric szolgáltatások telepítésekor
Amikor a szolgáltatások a fejlesztői számítógépen futtatja, nem könnyen használható [Visual Studio eszközök a hibakeresés](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Távoli fürtök [állapotjelentések](service-fabric-view-entities-aggregated-health.md) a rendszer mindig remek kezdőpont. Ezek a jelentések eléréséhez legegyszerűbb módja a PowerShell segítségével vannak vagy [SFX](service-fabric-visualizing-your-cluster.md). Ez a cikk feltételezi, hogy egy távoli fürt hibakeresést, és megismerheti, hogyan használhatja bármelyik eszközt.

## <a name="application-crash"></a>Alkalmazás-összeomlások
A "partíció nem éri el a cél replika- vagy példányszámot" jelentés jól jelzi, hogy a szolgáltatás összeomló rendszer. Ha szeretné tudni, ahol a szolgáltatás összeomló veszi kissé további vizsgálat. Ha a szolgáltatás léptékű fut, a legjobb barátjának lesz jól átgondolt nyomkövetések készlete.  Javasoljuk, hogy meg [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) ezeket a nyomkövetéseket gyűjt, és olyan megoldást használni, mint [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) megtekintéséhez és a Keresés a nyomkövetési adatokat.

![SFX partíció állapota](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

### <a name="during-service-or-actor-initialization"></a>Szolgáltatás vagy szereplő inicializálása közben
A kivételeket a szolgáltatás típusának inicializálása előtt, akkor a folyamat összeomolhat. Az ilyen típusú összeomlik az alkalmazások eseménynaplójában a hiba a szolgáltatás jelennek meg.
Ezek azok a leggyakrabban használt kivételeket tekintse meg a szolgáltatás inicializálása előtt.

***System.IO.FileNotFoundException***

Ez a hiba gyakran okozza hiányzik a szerelvény függőségeit. Ellenőrizze a Visual Studio vagy a globális szerelvény-gyorsítótárban, a csomópont CopyLocal tulajdonság.

***System.Runtime.InteropServices.COMException*** *System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory):*

 Ez azt jelzi, hogy a regisztrált típus neve nem egyezik meg a szolgáltatás jegyzékfájlt.

[Az Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) beállítható úgy, hogy az alkalmazások eseménynaplójában keresse meg az összes csomópont automatikus feltöltése.

### <a name="runasync-or-onactivateasync"></a>RunAsync() vagy OnActivateAsync()
Ha a összeomlási történik, az inicializálás közben, vagy a regisztrált szolgáltatástípus vagy szereplő alkalmazást, a kivétel kezelve lesz Azure Service Fabric által. Ezek a "További lépések" szakaszban részletes EventSource szolgáltatóktól tekintheti meg.

## <a name="next-steps"></a>Következő lépések
Ismerje meg a Service Fabric által biztosított meglévő diagnosztika:

* [Megbízható szereplője diagnosztika](service-fabric-reliable-actors-diagnostics.md)
* [Megbízható Services diagnosztika](service-fabric-reliable-services-diagnostics.md)

