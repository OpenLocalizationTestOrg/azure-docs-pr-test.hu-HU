---
title: aaaStateful Reliable Services diagnosztika |} Microsoft Docs
description: "A Stateful Reliable Services diagnosztikai funkciói"
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ae0e8f99-69ab-4d45-896d-1fa80ed45659
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 6200800b858957c06039d9af062633b12a446318
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>A Stateful Reliable Services diagnosztikai funkciói
állapotalapú alkalmazások és szolgáltatások megbízható szolgáltatások StatefulServiceBase osztály hello bocsát ki [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) eseményeket, amelyek lehetnek használt toodebug hello szolgáltatást, hogyan hello futásidejű működik, és segítenek a hibaelhárításban betekintést.

## <a name="eventsource-events"></a>EventSource események
hello EventSource neve hello állapotalapú alkalmazások és szolgáltatások megbízható szolgáltatások StatefulServiceBase osztály: "Microsoft-ServiceFabric-szolgáltatások". Ez az esemény forrásból származó események megjelennek a [diagnosztika események](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) ablakot, ha hello szolgáltatás folyamatban van [indítja a Visual Studio](service-fabric-debugging-your-application.md).

Például az eszközök és technológiák segíti a összegyűjtése és/vagy EventSource események megtekintése [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Microsoft Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md), és a [Microsoft TraceEvent Library](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Események
| esemény neve | Eseményazonosító | Szint | Esemény leírása |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Tájékoztató |Amikor a szolgáltatás RunAsync feladat elindítva |
| StatefulRunAsyncCancellation |2 |Tájékoztató |Amikor a szolgáltatás RunAsync feladat meg lett szakítva |
| StatefulRunAsyncCompletion |3 |Tájékoztató |Amikor a szolgáltatás RunAsync feladat befejezése |
| StatefulRunAsyncSlowCancellation |4 |Figyelmeztetés |Amikor a szolgáltatás RunAsync feladat túl hosszú toocomplete cancellation vesz igénybe. |
| StatefulRunAsyncFailure |5 |Hiba |Amikor a szolgáltatás RunAsync tevékenység kivételt jelez. |

## <a name="interpret-events"></a>Események értelmezése
StatefulRunAsyncInvocation StatefulRunAsyncCompletion és StatefulRunAsyncCancellation eseményeket is hasznos toohello szolgáltatás író toounderstand hello életciklus--szolgáltatás, valamint ha a szolgáltatás elindult, megszakítva, vagy befejeződött hello ütemezése . Ez akkor lehet hasznos, ha a kérdések vagy ismertetése hello szolgáltatás életciklus-hibakeresés.

Szolgáltatás írók kell fizetnie a elolvassa tooStatefulRunAsyncSlowCancellation és StatefulRunAsyncFailure események, mert hello szolgáltatás problémákat jeleznek.

Amikor hello szolgáltatás RunAsync() tevékenység kivételt StatefulRunAsyncFailure kibocsátott. Egy kivétel lépett fel általában azt jelzi, hibás utasítást vagy hiba hello szolgáltatásban. Emellett hello kivétel hatására hello szolgáltatás toofail, az, hogy áthelyezett tooa másik csomópont. Ez lehet egy drága művelet, és késleltetheti a bejövő kérelmeket, ha hello szolgáltatás áthelyezte. Szolgáltatás írók határozza meg, hello kivétel hello okát és, ha lehetséges, mérsékelni.

StatefulRunAsyncSlowCancellation kibocsátott amikor hello RunAsync feladat megszakításának kérelem 4 másodpercnél hosszabb időbe telik. A szolgáltatás túl hosszú toocomplete visszavonásra kerül, ha gyorsan újraindítását egy másik csomópontján hello szolgáltatás toobe hello mostantól hatással van. Ez hatással lehet a hello hello szolgáltatás rendelkezésre állását.

## <a name="next-steps"></a>Következő lépések
* [A PerfView EventSource szolgáltatók](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
