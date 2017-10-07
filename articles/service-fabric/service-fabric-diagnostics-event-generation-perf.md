---
title: "Service Fabric-figyelő aaaAzure |} Microsoft Docs"
description: "További információk a figyelési és az Azure Service Fabric-fürtök diagnostics teljesítményszámlálók."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 54d4c62b7250a1f70b0898ba07ae5a37716f4cf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performance-metrics"></a>Teljesítménymértékeket

Metrikák kell kell a fürt összegyűjtött toounderstand hello teljesítmény, valamint hello az alkalmazások azt. A Service Fabric-fürtök esetén ajánlott hello alábbi teljesítményszámlálók gyűjtése.

## <a name="nodes"></a>Csomópontok

Hello gépek a fürt fontolja meg a következő toobetter megérteni hello minden gép terhelését, és ellenőrizze a megfelelő döntések skálázás fürt teljesítményszámlálók hello gyűjtése.

| Teljesítményszámláló-kategóriája | Számláló neve |
| --- | --- |
| Fizikai lemez (lemezenként) | Átlagos Olvasási Lemezvárólista hossza |
| Fizikai lemez (lemezenként) | Átlagos Írási Lemezvárólista hossza |
| Fizikai lemez (lemezenként) | Átlagos Lemez mp/Olvasás |
| Fizikai lemez (lemezenként) | Átlagos Lemez mp/írás |
| Fizikai lemez (lemezenként) | Lemezolvasások/mp |
| Fizikai lemez (lemezenként) | Lemez sebessége olvasott bájt/mp |
| Fizikai lemez (lemezenként) | Lemezírás/mp |
| Fizikai lemez (lemezenként) | Lemez írási bájtok/s |
| Memory (Memória) | Rendelkezésre álló memória (MB) |
| PagingFile | Kihasználtsága |
| Process(total) | Processzor kihasználtsága |
| Folyamat száma (szolgáltatás) | Processzor kihasználtsága |
| Folyamat száma (szolgáltatás) | A folyamatot |
| Folyamat száma (szolgáltatás) | A saját memória |
| Folyamat száma (szolgáltatás) | Szálak száma |
| Folyamat száma (szolgáltatás) | Virtuális memória |
| Folyamat száma (szolgáltatás) | Munkakészlet |
| Folyamat száma (szolgáltatás) | Munkakészlet - saját |

## <a name="net-applications-and-services"></a>.NET-alkalmazások és szolgáltatások

.NET szolgáltatások tooyour fürt központi telepítése a következő számlálók hello gyűjtése. 

| Teljesítményszámláló-kategóriája | Számláló neve |
| --- | --- |
| .NET CLR memória (szolgáltatás) | Folyamatazonosító |
| .NET CLR memória (szolgáltatás) | # Összesen előjegyzett memória kihasználtsága |
| .NET CLR memória (szolgáltatás) | # Összesen fenntartott memória |
| .NET CLR memória (szolgáltatás) | (Bájt) összes halommemória |
| .NET CLR memória (szolgáltatás) | # 0. generációs gyűjtemények |
| .NET CLR memória (szolgáltatás) | # 1. generációból gyűjtemények |
| .NET CLR memória (szolgáltatás) | # Generációból 2 gyűjtemények |
| .NET CLR memória (szolgáltatás) | % Töltött idő |

### <a name="service-fabrics-custom-performance-counters"></a>A Service Fabric egyéni teljesítményszámlálói

A Service Fabric egyéni teljesítményszámlálóit jelentős mennyiségű állít elő. Ha hello SDK telepítve van, látható hello átfogó listáját a Windows-számítógépre az alkalmazás teljesítményének figyelése (Start > Teljesítményfigyelő). 

Hello alkalmazások elérhetővé tett tooyour fürt, ha Reliable Actors használ, adja hozzá a countes `Service Fabric Actor` és `Service Fabric Actor Method` kategóriák (lásd: [Service Fabric megbízható szereplője diagnosztika](service-fabric-reliable-actors-diagnostics.md)).

Ha Reliable Services használatához hasonló módon kell `Service Fabric Service` és `Service Fabric Service Method` kell gyűjtése teljesítményszámláló-kategóriák a teljesítményszámlálók. 

Ha megbízható gyűjteményeket használ, azt javasoljuk, hogy hello `Avg. Transaction ms/Commit` a hello `Service Fabric Transactional Replicator` toocollect hello átlagos véglegesítési késés / tranzakció metrikát.


## <a name="next-steps"></a>Következő lépések

* További információ [hello platform szintjén az eseménygenerálás](service-fabric-diagnostics-event-generation-infra.md) a Service Fabric
* Teljesítményadatok gyűjtéséhez keresztül [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md)
