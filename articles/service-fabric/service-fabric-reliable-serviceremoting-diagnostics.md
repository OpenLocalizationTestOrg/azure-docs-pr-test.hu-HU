---
title: "aaaAzure ServiceFabric diagnosztika és figyelése |} Microsoft Docs"
description: "Ez a cikk ismerteti a hello alkalmazásteljesítmény-figyelési szolgáltatásokkal hello Service Fabric megbízható ServiceRemoting futásidejű, például a teljesítményszámlálók azt által kibocsátott."
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: suchiagicha
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: suchiagicha
ms.openlocfilehash: 64db9a890bd59a1326e587d14b89c007b71a9059
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-service-remoting"></a>Diagnosztika és teljesítményfigyelés a megbízható szolgáltatás távelérése
hello megbízható ServiceRemoting futásidejű bocsát ki [teljesítményszámlálók](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Ezek hello ServiceRemoting működéséről betekintést, és a hibaelhárítás és a Teljesítményfigyelő segítségével.


## <a name="performance-counters"></a>Teljesítményszámlálók
hello megbízható ServiceRemoting futásidejű meghatározza, hogy a következő teljesítményszámláló-kategóriák hello:

| Kategória | Leírás |
| --- | --- |
| Service Fabric-szolgáltatás |Adott tooAzure Service Fabric szolgáltatás távoli eljáráshívási teljesítményszámlálók, például szükséges átlagos idő tooprocess kérése |
| Service Fabric szolgáltatás módszer |Számlálók adott toomethods által megvalósított Service Fabric Remoting Service, például egy metódus meghívja gyakoriságát. |

Egyes megelőző kategóriák hello egy vagy több számlálóval rendelkezik.

Hello [Windows Teljesítményfigyelő](https://technet.microsoft.com/library/cc749249.aspx) hello Windows operációs rendszerben alapértelmezés szerint elérhető alkalmazás lehet használt toocollect és nézet teljesítményszámláló-adatokat. [Az Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) van lehetősége, hogy a teljesítményszámláló-adatok összegyűjtésére, majd ismét feltölteni a tooAzure táblákat.

### <a name="performance-counter-instance-names"></a>Teljesítményszámláló-példányok nevei
Egy fürt, amely nagy számú ServiceRemoting szolgáltatások vagy a partíciók teljesítmény számlálópéldány nagy számú rendelkezik. hello teljesítményszámláló-példány nevét segíthet beazonosítani hello adott partícióra és metódus (ha van ilyen), hogy hello teljesítményszámláló-példány társítva van.

#### <a name="service-fabric-service-category"></a>Service Fabric-szolgáltatás kategória
Hello kategória `Service Fabric Service`, hello teljesítményszámláló-példányok nevei szerepelnek hello a következő formátumban:

`ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*ServiceFabricPartitionID* hello karakterláncos ábrázolása hello hello teljesítményszámláló-példány Service Fabric Partícióazonosító társítva. hello Partícióazonosító GUID-nak, és a karakterlánc-ábrázolása hello segítségével jön létre [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) formátummegadó "D" metódust.

*ServiceReplicaOrInstanceId* hello karakterláncos ábrázolása hello szolgáltatás háló replika/példány azonosítója, amely hello teljesítményszámláló-példány társítva van.

*ServiceRuntimeInternalID* hello karakterláncos ábrázolása egy 64 bites egész hello Fabric Service-futtatókörnyezet belső használatra készült. Ez megtalálható hello teljesítményszámláló-példány neve tooensure annak egyediségét és más teljesítményszámláló-példányok nevei való ütközés elkerülése érdekében. Felhasználók nem próbálkozhat toointerpret hello teljesítményszámlálójának példánynevét Ez a része.

hello az alábbiakban látható egy példa egy teljesítményszámlálójának példánynevét toohello tartozó számláló `Service Fabric Service` kategória:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046_5008379932`

Az előző példában hello `2740af29-78aa-44bc-a20b-7e60fb783264` hello Service Fabric partícióazonosító: hello karakterláncos ábrázolása `635650083799324046` replika/InstanceId karakterláncos ábrázolása és `5008379932` a hello futásidejű tartozó belső generált hello 64-bit-es azonosító használjon.

#### <a name="service-fabric-service-method-category"></a>Service Fabric szolgáltatás metódus kategória
Hello kategória `Service Fabric Service Method`, hello teljesítményszámláló-példányok nevei szerepelnek hello a következő formátumban:

`MethodName_ServiceRuntimeMethodId_ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*MethodName* a teljesítményszámláló-példány hello hello metódus neve hello társítva van. hello metódus nevének formátuma hello néhány logika, hogy korlátozza a hello teljesítményszámláló-példányok nevei legfeljebb hello Windows hello nevű hello olvashatóságát hello Hálószolgáltatás futásidejű alapján határozza meg.

*ServiceRuntimeMethodId* hello karakterláncos ábrázolása egy 32 bites egész hello Fabric Service-futtatókörnyezet belső használatra készült. Ez megtalálható hello teljesítményszámláló-példány neve tooensure annak egyediségét és más teljesítményszámláló-példányok nevei való ütközés elkerülése érdekében. Felhasználók nem próbálkozhat toointerpret hello teljesítményszámlálójának példánynevét Ez a része.

*ServiceFabricPartitionID* hello karakterláncos ábrázolása hello hello teljesítményszámláló-példány Service Fabric Partícióazonosító társítva. hello Partícióazonosító GUID-nak, és a karakterlánc-ábrázolása hello segítségével jön létre [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) formátummegadó "D" metódust.

*ServiceReplicaOrInstanceId* hello karakterláncos ábrázolása hello szolgáltatás háló replika/példány azonosítója, amely hello teljesítményszámláló-példány társítva van.

*ServiceRuntimeInternalID* hello karakterláncos ábrázolása egy 64 bites egész hello Fabric Service-futtatókörnyezet belső használatra készült. Ez megtalálható hello teljesítményszámláló-példány neve tooensure annak egyediségét és más teljesítményszámláló-példányok nevei való ütközés elkerülése érdekében. Felhasználók nem próbálkozhat toointerpret hello teljesítményszámlálójának példánynevét Ez a része.

hello az alábbiakban látható egy példa egy teljesítményszámlálójának példánynevét toohello tartozó számláló `Service Fabric Service Method` kategória:

`ivoicemailboxservice.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486_5008380`

Az előző példában hello `ivoicemailboxservice.leavemessageasync` hello metódus neve, `2` van 32-bit-es Azonosítójú létrehozni a hello futásidejű tartozó belső használatához hello `89383d32-e57e-4a9b-a6ad-57c6792aa521` hello Service Fabric partícióazonosító: hello karakterláncos ábrázolása`635650083804480486` hello karakterlánc Service Fabric replika/példány azonosítója hello ábrázolását és `5008380` létrehozni a hello futásidejű tartozó belső hello 64-bit-es azonosító használata.

## <a name="list-of-performance-counters"></a>Teljesítményszámlálók listája
### <a name="service-method-performance-counters"></a>Szolgáltatások metódus teljesítményszámlálói

hello megbízható szolgáltatás futásideje teljesítmény számlálók kapcsolódó toohello metódusok végrehajtása a következő hello közzéteszi.

| Kategória neve | Számláló neve | Leírás |
| --- | --- | --- |
| Service Fabric szolgáltatás módszer |Indítások/mp |Amelyet hello metódus másodpercenkénti száma |
| Service Fabric szolgáltatás módszer |Hívásonkénti átlagos idő ezredmásodpercben |Idő tooexecute hello szolgáltatás metódus ezredmásodpercben |
| Service Fabric szolgáltatás módszer |Kiváltott kivételek száma |Ennyiszer hello metódusa kivételt okozott / másodperc |

### <a name="service-request-processing-performance-counters"></a>Szolgáltatási kérelem feldolgozása teljesítményszámlálói
Amikor egy ügyfél keresztül a szolgáltatási proxy objektumhoz egy metódust hívja, a hello hálózati toohello távoli eljáráshívási szolgáltatás keresztül küldött kérelemüzenet eredményez. hello szolgáltatás hello kérelemüzenet feldolgozza, és elküldi a válasz hátsó toohello ügyfélnek. hello megbízható ServiceRemoting futásidejű teljesítmény számlálók kapcsolódó tooservice kérelem feldolgozása a következő hello közzéteszi.

| Kategória neve | Számláló neve | Leírás |
| --- | --- | --- |
| Service Fabric-szolgáltatás |Függőben lévő kérések száma |Hello szolgáltatásban feldolgozás alatt álló kérelmek száma |
| Service Fabric-szolgáltatás |Kérelmenkénti átlagos idő ezredmásodpercben |Szükséges időt (milliszekundumban) hello szolgáltatás tooprocess kérelem |
| Service Fabric-szolgáltatás |Kérelem deszerializálásának átlagos ideje ezredmásodpercben |Idő (ezredmásodpercben) tett toodeserialize szolgáltatás felderítéskérelmi üzenetet fogadja hello szolgáltatás |
| Service Fabric-szolgáltatás |Válasz szerializálásának átlagos ideje ezredmásodpercben |Idő (ezredmásodpercben) tett tooserialize hello szolgáltatás válaszüzenetet hello szolgáltatás hello válasz toohello ügyfél elküldése előtt: |

## <a name="next-steps"></a>Következő lépések
* [Mintakód](https://github.com/Azure/servicefabric-samples)
* [A PerfView EventSource szolgáltatók](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
