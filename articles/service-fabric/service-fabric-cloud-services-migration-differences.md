---
title: "Cloud Services és a Service Fabric közötti aaaDifferences |} Microsoft Docs"
description: "Fogalmi áttekintése áttelepítéséhez Felhőszolgáltatások tooService háló alkalmazások."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 0b87b1d3-88ad-4658-a465-9f05a3376dee
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: bbc5ef4fe0fe1b0da55454cb6b766925030198fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-hello-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Tudnivalók a felhőalapú szolgáltatások és a Service Fabric hello különbségei áttelepítése előtt alkalmazások.
A Microsoft Azure Service Fabric a hello következő generációs felhőalapú alkalmazás platformja jól skálázható, nagymértékben megbízható elosztott alkalmazásokhoz. Számos új szolgáltatást csomagolása, telepítése, frissítése és elosztott felhőalapú alkalmazások kezelése okozna. 

Ez a Felhőszolgáltatások tooService háló egy bevezető útmutatóban toomigrating alkalmazások. Architekturális elsősorban a összpontosít, és Felhőszolgáltatások és a Service Fabric közötti különbségek tervezése.

## <a name="applications-and-infrastructure"></a>Alkalmazások és infrastruktúra
A felhőalapú szolgáltatások és a Service Fabric alapvető különbség, virtuális gépek, munkaterhelések és alkalmazások hello kapcsolatát. Itt egy munkaterhelés hello kód írása tooperform egy adott feladatot vagy szolgáltatás típusúként van definiálva.

* **Cloud Services csomag van, mint a virtuális gépek alkalmazások központi telepítésével kapcsolatos.** hello írt kódot szorosan összekapcsolt tooa Virtuálisgép-példány, például egy webes vagy feldolgozói szerepkör. toodeploy a munkaterhelés, a Cloud Services toodeploy egy vagy több Virtuálisgép-példányok futtatási hello munkaterhelés. Az alkalmazások és a virtuális gépek nem elválasztási van, és ezért nem formális definíciója van egy alkalmazás. Az alkalmazás-re webes vagy feldolgozói szerepkör példányok belül a Felhőszolgáltatások központi telepítés akár egy teljes felhőalapú szolgáltatások telepítése. Ebben a példában az alkalmazás szerepkör példányok jelenik meg.

![A felhőalapú szolgáltatások alkalmazásokhoz és topológia][1]

* **A Service Fabric telepítését alkalmazások tooexisting virtuális gépek vagy a Service Fabric futó Windows vagy Linux rendszerű gépek tárgya.** írt hello szolgáltatásainak használata az alapul szolgáló infrastruktúra, amelyeket számítógépnél hello Service Fabric-alkalmazás platform, így az alkalmazás telepített toomultiple környezetek lehet hello teljesen leválasztott. A munkaterhelés, a Service Fabric "szolgáltatás" nevezik, és egy vagy több szolgáltatás hello Service Fabric-alkalmazás platformon futó hivatalosan által definiált alkalmazásban vannak csoportosítva. Az alkalmazások több telepített tooa egyetlen Service Fabric-fürt lehetnek.

![Service Fabric-alkalmazások és a topológia][2]

Maga a Service Fabric egy platform alkalmazásréteg futó Windows vagy Linux,, mivel a Cloud Services rendszer csatolt munkaterhelések az Azure által kezelt virtuális gépek telepítéséhez.
hello Service Fabric-alkalmazás modell számos előnnyel rendelkezik:

* Gyors üzembe helyezési idők. Virtuálisgép-példányok létrehozása időigényes lehet. A Service Fabric virtuális gépek csak telepítése után tooform a fürt, amelyen a Service Fabric-alkalmazás platform hello. Ettől kezdve alkalmazáscsomagok tárolófürt is lehet központilag telepített toohello nagyon gyorsan.
* Nagy sűrűségű üzemeltetéséhez. A felhőalapú szolgáltatások a feldolgozói szerepkör virtuális gépek egy munkaterhelés üzemelteti. A Service Fabric alkalmazások függetlenek hello futtató virtuális gépeken őket, tehát a központilag telepíthető alkalmazások tooa kevés virtuális gépeket, amely csökkentheti a nagyobb üzemelő példányok esetében a teljes költség szempontjából sok.
* hello platform futtatható bárhol, amely a Service Fabric Windows Server vagy Linux rendszerű gépek, rendelkezik, hogy Azure vagy a helyszíni. hello platform absztrakciós réteget biztosít a hello alapul szolgáló infrastruktúrán keresztül, az alkalmazás futtatható a különböző környezetekben. 
* Elosztott alkalmazások kezelése. A Service Fabric, nem csak állomások elosztott alkalmazásokat, de is segít Virtuálisgép üzemeltetési hello függetlenül az életciklus kezeléséhez, illetve a számítógép-életciklus platformot.

## <a name="application-architecture"></a>Alkalmazásarchitektúra
hello architektúra Felhőszolgáltatások alkalmazás általában tartalmaz számos külső függőségei, például a Service Bus Azure Table és a Blob-tároló, SQL, Redis vagy mások toomanage hello állapotát és adatait egy alkalmazás és a webkiszolgáló közötti kommunikáció és feldolgozói szerepkörök Felhőszolgáltatások telepítésben. A teljes Felhőszolgáltatások alkalmazás például nézhet ki:  

![A felhőalapú szolgáltatások architektúrája][9]

Service Fabric-alkalmazások is beállíthatja toouse hello azonos külső szolgáltatások teljes alkalmazásban. Ez a példa Felhőszolgáltatások architektúra, hello legegyszerűbb áttelepítési út a Felhőszolgáltatások tooService háló használata tooreplace csak hello Felhőszolgáltatások telepítés hello tartása Service Fabric-alkalmazás az általános architektúrája hello azonos. hello webes és feldolgozói szerepkörök lehet áthelyezni tooService háló állapotmentes szolgáltatások révén minimális kódmódosítással.

![Egyszerű áttelepítés után a Service Fabric-architektúra][10]

Ebben a szakaszban továbbra is hello rendszer toowork hello ugyanaz, mint korábban. A Service Fabric állapotalapú szolgáltatások, külső állapot tárolók kihasználni is internalized, állapotalapú alkalmazások és szolgáltatások részlegeinek. Ez az bonyolultabb mint egy egyszerű áttelepítési webes és feldolgozói szerepkörök tooService háló állapotmentes szolgáltatások, mivel az egyéni szolgáltatások, mint hello külső szolgáltatások előtt adja meg a megfelelő funkciók tooyour alkalmazás írásakor. így hello előnyei a következők: 

* Külső függőségek eltávolítása 
* Egységes hello telepítési, kezelési és frissítési modell. 

Egy példa eredményül kapott architektúrájának ezeket a szolgáltatásokat internalizing nézhet ki:

![A teljes áttelepítés után a Service Fabric-architektúra][11]

## <a name="communication-and-workflow"></a>Kommunikációs és a munkafolyamat
A legtöbb Cloud Service-alkalmazások egynél több réteg áll. Hasonlóképpen a Service Fabric-alkalmazás több szolgáltatás (általában számos szolgáltatás) áll. Két közös kommunikációs modellt közvetlen kommunikációra és egy külső tartós tárolási keresztül.

### <a name="direct-communication"></a>Közvetlen kommunikációt
Az közvetlen kommunikációt rétegek közvetlenül a végpont által az egyes rétegek kommunikációt. Állapot nélküli környezetekben például Cloud Services, ez azt jelenti kiválasztása egy Virtuálisgép-szerepkör példánya vagy véletlenszerűen vagy ciklikus multiplexelés toobalance terhelés, és közvetlenül csatlakozó tooits végpont.

![A közvetlen kommunikációt cloud Services csomag][5]

 Közvetlen kommunikációra a Service Fabric egy általános kommunikációs modellt. hello kulcs Service Fabric és a Felhőszolgáltatások közötti különbség, hogy a Cloud Services csatlakozás tooa VM, mivel a Service Fabric connect tooa szolgáltatás. Ez fontos különbség néhány lehetnek az okai:

* A Service Fabric szolgáltatások nincsenek kötött toohello virtuális gépeket üzemeltető szolgáltatások hello fürt Navigálás is, és valójában körül várt toomove különböző okokból: erőforrás terheléselosztás, feladatátvevő, alkalmazások és az infrastruktúra frissítések és elhelyezési vagy terheléselosztási vannak. Ez azt jelenti, hogy egy szolgáltatáspéldány cím bármikor módosíthatja. 
* Egy virtuális Gépet, a Service Fabric több szolgáltatásra, az egyedi végpontok száma tárolhatja.

A Service Fabric szolgáltatás felderítési mechanizmust, hello elnevezési szolgáltatást, amelyet a végpont-címek használt tooresolve szolgáltatások nevű biztosít. 

![A Service Fabric közvetlen kommunikációt][6]

### <a name="queues"></a>Üzenetsorok
Állapot nélküli környezetekben, például Felhőszolgáltatások rétegek közötti közös kommunikációs mechanizmusra egy külső tároló várólista toodurably toouse tárolja az egyrétegű tooanother munkahelyi feladatokhoz. Egy gyakori forgatókönyv, de egy webes réteg által küldött feladatok tooan Azure Queue Service Bus ahol feldolgozói szerepkör példányok created és hello feladatok feldolgozásához.

![Cloud Services várólista kommunikáció][7]

hello azonos kommunikációs modellt a Service Fabric használható. Ez akkor lehet hasznos, amikor telepít egy meglévő Felhőszolgáltatások alkalmazás tooService háló. 

![A Service Fabric közvetlen kommunikációt][8]

## <a name="next-steps"></a>Következő lépések
hello háló csak hello felhőalapú szolgáltatások telepítése a Service Fabric-alkalmazás, hogy hello az alkalmazás általános architektúrája nagyjából tooreplace Felhőszolgáltatások tooService a legegyszerűbb áttelepítési út hello azonos. a következő cikk hello biztosít egy útmutató toohelp convert egy webes vagy feldolgozói szerepkör tooa Service Fabric állapotmentes szolgáltatások.

* [Egyszerű áttelepítési: egy webes vagy feldolgozói szerepkör tooa Service Fabric állapotmentes szolgáltatások átalakítása](service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
