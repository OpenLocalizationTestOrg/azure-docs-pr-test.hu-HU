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
# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Gyakori problémák elhárításához, az Azure Service Fabric szolgáltatások telepítésekor
Szolgáltatások a fejlesztői számítógépen futtatja, esetén könnyen toouse [Visual Studio eszközök a hibakeresés](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Távoli fürtök [állapotjelentések](service-fabric-view-entities-aggregated-health.md) a rendszer mindig a megfelelő hely toostart. Ezek a jelentések megtalálhatók a Powershellen keresztül legegyszerűbb módja tooaccess hello vagy [SFX](service-fabric-visualizing-your-cluster.md). Ez a cikk feltételezi, hogy egy távoli fürt hibakeresést, és megismerheti, hogyan toouse mindkét eszközei.

## <a name="application-crash"></a>Alkalmazás-összeomlások
hello "partíció nem éri el a cél replika- vagy példányszámot" jelentés jól jelzi, hogy a szolgáltatás összeomló rendszer. toofind, ahol a szolgáltatás összeomló egy kis további vizsgálat szükséges. Ha a szolgáltatás léptékű fut, a legjobb barátjának lesz jól átgondolt nyomkövetések készlete.  Javasoljuk, hogy meg [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) ezeket a nyomkövetéseket gyűjt, és olyan megoldást használni, mint [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) megtekintése és keresése hello nyomkövetési adatokat.

![SFX partíció állapota](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

### <a name="during-service-or-actor-initialization"></a>Szolgáltatás vagy szereplő inicializálása közben
Hello szolgáltatás típusának inicializálása előtt kivételek hello folyamat toocrash miatt. Az ilyen típusú összeomlik hello alkalmazások eseménynaplójában keresse meg a szolgáltatásból hello hiba jelennek meg.
Ezek a hello leggyakoribb kivételek toosee hello szolgáltatás inicializálása előtt.

***System.IO.FileNotFoundException***

Ez a hiba gyakran toomissing szerelvény függőségek miatt van. Ellenőrizze a Visual Studio vagy a globális szerelvény-gyorsítótárban hello hello csomópont hello CopyLocal tulajdonság.

***System.Runtime.InteropServices.COMException*** *System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory):*

 Ez azt jelzi, hogy hello regisztrált típus neve nem egyezik meg a hello szolgáltatás jegyzék.

[Az Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) automatikusan lehet konfigurált tooupload hello alkalmazások eseménynaplójában keresse meg az összes csomópont.

### <a name="runasync-or-onactivateasync"></a>RunAsync() vagy OnActivateAsync()
Ha hello összeomlási hello inicializálása közben történik, vagy a regisztrált szolgáltatástípus vagy szereplő alkalmazást, hello kivétel kezelve lesz Azure Service Fabric által. Ezeket a hello EventSource szolgáltatók hello "További lépések" szakaszban ismertetett tekintheti meg.

## <a name="next-steps"></a>Következő lépések
Ismerje meg a Service Fabric által biztosított meglévő diagnosztika:

* [Megbízható szereplője diagnosztika](service-fabric-reliable-actors-diagnostics.md)
* [Megbízható Services diagnosztika](service-fabric-reliable-services-diagnostics.md)

