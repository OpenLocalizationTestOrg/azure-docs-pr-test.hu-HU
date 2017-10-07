---
title: "a Windows Azure mikroszolgáltatások aaaDebug |} Microsoft Docs"
description: "Megtudhatja, hogyan toomonitor és diagnosztizálhatja a szolgáltatások egy helyi fejlesztési gépen a Microsoft Azure Service Fabric használatával készítettek."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: edcc0631-ed2d-45a3-851d-2c4fa0f4a326
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2017
ms.author: dekapur
ms.openlocfilehash: 24868aa194b8a28fa3e6de95c1de5506d912a544
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Figyelése és diagnosztizálása szolgáltatásai a helyi számítógép fejlesztési beállítása
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
> 
> 

Figyelés, észlelésére, diagnosztizálása és hibaelhárítási lehetővé teszi a minimális megszakítás toohello felhasználói élmény szolgáltatások toocontinue. Amíg figyelése és a diagnosztika nem kritikus tényleges telepített éles környezetben, hello hatékonyságát függ hasonló modell bevezetése tooa valós telepítés áthelyezésekor működnek szolgáltatások tooensure fejlesztése során. A Service Fabric megkönnyíti a szolgáltatás a fejlesztők tooimplement diagnosztikai, amely zökkenőmentesen használható legyen a helyi fejlesztési egyszámítógépes beállításokat és a valós üzemi fürt beállításokat.

## <a name="event-tracing-for-windows"></a>A Windows esemény-nyomkövetés
[Esemény nyomkövetése Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) technológia nyomkövetési üzenet, a Service Fabric ajánlott hello. Néhány ETW használatának előnyei a következők:

* **ETW gyors.** A nyomkövetés technológia, amely rendelkezik egy kód végrehajtásának lassúságát gyakorolt minimális hatás mellett azokat készítették.
* **ETW-nyomkövetés zökkenőmentesen helyi fejlesztési környezetekben, és valós fürt beállítások között.** Ez azt jelenti, hogy nem rendelkezik a nyomkövetési kód, amikor készen áll a toodeploy toorewrite a kód tooa valós fürt.
* **A Service Fabric rendszer kód ETW is használja a belső nyomkövetés.** Ez lehetővé teszi az alkalmazás nyomkövetési adatait a Service Fabric rendszer nyomkövetések időosztásos tooview. Emellett segít toomore, könnyen megértéséhez hello feladatütemezések és a kapcsolatokat, az alkalmazás kódjában és az események között hello alapul szolgáló rendszer.
* **A rendszer beépített támogatja a Service Fabric Visual Studio eszközök tooview ETW-események.** Ha a Visual Studio megfelelően van beállítva a Service Fabric hello diagnosztikai események megtekintése a Visual Studio ETW-események jelennek meg. 

## <a name="view-service-fabric-system-events-in-visual-studio"></a>A Visual Studio Service Fabric rendszer eseményeinek megtekintése
A Service Fabric toohelp alkalmazásfejlesztők megérteni, hogy mi történik a hello platform ETW-események bocsát ki. Ha még nem tette meg, lépjen tovább, és kövesse hello [az első alkalmazás létrehozásának a Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md). Ez az információ segít elérhetővé, az alkalmazás működik, és amelyen hello hello nyomkövetési üzeneteket bemutató diagnosztikai eseménynapló.

1. Ha hello diagnosztika események ablak nem automatikusan megjelenítéséhez nyissa meg toohello **nézet** a Visual Studio lapra, majd **más Windows** , majd **diagnosztikai eseménynaplóhoz**.
2. Mindegyik esemény rendelkezik szabványos metaadat-információi, amely jelzi, hogy hello csomópont, alkalmazás és szolgáltatás hello esemény érkezik. Események listája hello hello segítségével is végezhet **események szűréséhez** hello felső hello események ablak bezárásához. Végezhet, például a **csomópontnév** vagy **szolgáltatás neve.** Amikor tartózkodik esemény részleteit, fel is függesztheti hello segítségével és **szüneteltetése** hello események ablak hello tetején gombra, majd később események adatvesztés nélkül folytathatja.
   
   ![A Visual Studio diagnosztikai eseménynapló](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/DiagEventsExamples2.png)

## <a name="add-your-own-custom-traces-toohello-application-code"></a>Adja hozzá a saját egyéni nyomkövetési toohello alkalmazás kódot
Service Fabric Visual Studio projektsablonjai hello példakódot tartalmaz. hello kód bemutatja, hogyan tooadd egyéni alkalmazáskódok ETW követi, hogy másolatot hello Visual Studio ETW-megjelenítőben mellett a Service Fabric rendszer nyomkövetéseit. hello ezt a módszert előnye, hogy a metaadatokat a rendszer automatikusan tootraces, és a Visual Studio diagnosztikai eseménynaplóhoz már hello konfigurált toodisplay őket.

Hello alapján létrehozott projektek **szolgáltatássablonok** (állapot nélküli és állapotalapú) csak keressen hello `RunAsync` végrehajtására:

1. hívás túl hello`ServiceEventSource.Current.ServiceMessage` a hello `RunAsync` metódus hello alkalmazás kódból egyéni ETW-nyomkövetés példáját mutatja be.
2. A hello **ServiceEventSource.cs** fájl, talál egy túlterhelési a hello `ServiceEventSource.ServiceMessage` nagyon gyakori események tooperformance okok miatt használandó módszert.

Hello alapján létrehozott projektek **szereplő sablonok** (állapot nélküli és állapotalapú):

1. Nyissa meg hello **"Projektnév".cs** where fájl *projektnév* hello neve, a Visual Studio-projekt számára is választott.  
2. Hello kód található `ActorEventSource.Current.ActorMessage(this, "Doing Work");` a hello *DoWorkAsync* metódust.  Ez az egy egyéni ETW-nyomkövetés alkalmazáskód írásbeli példát.  
3. A fájl **ActorEventSource.cs**, túlterhelés megtalálja a hello `ActorEventSource.ActorMessage` nagyon gyakori események tooperformance okok miatt használandó módszert.

A felvett egyéni ETW-nyomkövetés tooyour szolgáltatáskód hibáit, létre, telepíthetnek és hello alkalmazás futtatásához újra toosee az esemény a hello diagnosztikai eseménynaplóhoz. Ha hello alkalmazás hibakeresése **F5**, hello diagnosztikai eseménynaplóhoz automatikusan megnyílik.

## <a name="next-steps"></a>Következő lépések
hello ugyanazt a hozzáadott tooyour alkalmazás fent helyi diagnosztikai nyomkövetési kódot együttműködve tooview használt eszközök ezeket az eseményeket az Azure-fürttel az alkalmazás futtatásakor. Tekintse meg ezek a cikkek ismertetik hello hello eszközök különböző lehetőségek közül, és ismertetik, hogyan állíthat be őket.

* [Hogyan toocollect naplózza az Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md)
* [Esemény összevonásának és a gyűjtemény EventFlow használatával](service-fabric-diagnostics-event-aggregation-eventflow.md)

