---
title: "aaaApplication forgatókönyvek és kialakítása |} Microsoft Docs"
description: "A Service Fabric felhőalkalmazásokhoz kategóriáinak áttekintése. Állapot nélküli és állapotalapú alkalmazások és szolgáltatások használó alkalmazás tervét, és ismerteti."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 3a8ca6ea-b8e9-4bc3-9e20-262437d2528e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/02/2017
ms.author: mfussell
ms.openlocfilehash: e36d5b2d21a6a1e3e85c9b21190072616e4921e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-scenarios"></a>A Service Fabric-alkalmazás-forgatókönyvek
Azure Service Fabric toowrite, amely lehetővé teszi a megbízható és rugalmas platformot biztosít, és számos üzleti alkalmazások és szolgáltatások futtatásához. Ezek az alkalmazások és mikroszolgáltatások létrehozására lehet állapot nélküli és állapotalapú, és erőforrás rendelkező virtuális gépek toomaximize hatékonyságát között. a Service Fabric hello egyedi architektúrája lehetővé teszi tooperform közel valós idejű adatelemzés, a memórián belüli számítási, a párhuzamos tranzakciókat és a esemény feldolgozását az alkalmazásokban. Könnyedén méretezhető az alkalmazások felfelé vagy lefelé (valóban bejövő vagy kimenő), a változó erőforrás követelményeitől függően.

az Azure Service Fabric-platformról hello ideális megoldás a következő alkalmazások kategóriáinak hello:

* **Magas rendelkezésre állású szolgáltatások**: a Service Fabric szolgáltatásokat biztosítanak az gyors több másodlagos szolgáltatás replikák létrehozásával. Ha egy csomópont, a folyamat vagy a szolgáltatás toohardware vagy egyéb hiba miatt nem működik, hello másodlagos replikák egyik szolgáltatás minimális szintű kiesése előléptetett tooa elsődleges replikán.
* **Méretezhető szolgáltatások**: egyéni szolgáltatások lehet particionálni, lehetővé teszi a állapot toobe horizontálisan felskálázott hello fürtön. Emellett az egyes szolgáltatások hozható létre és hello parancsprogramok eltávolítása. Szolgáltatások a példányok sok csomópontján néhány csomópontok toothousands néhány példányokból horizontálisan gyorsan és egyszerűen, és majd méretezve, az ebben az esetben erőforrás igényeitől függően. A Service Fabric toobuild használja ezeket a szolgáltatásokat, és a teljes életciklusának kezelésére.
* **A következő nem statikus adatok számítási**: a Service Fabric lehetővé teszi a toobuild adatok bemeneti/kimeneti és számítási állapotalapú alkalmazások. A Service Fabric lehetővé teszi, hogy hello közös elhelyezés feldolgozásának (számítás) és az adatok az alkalmazások. Általában az alkalmazás által igényelt hozzáférés toodata, ha van egy külső adatforráshoz gyorsítótár vagy tárolási réteget társított hálózati késés. Állapotalapú Service Fabric-szolgáltatásokkal a késés nem szűnik, beolvassa és további performant engedélyezése. Tegyük fel például például, hogy rendelkezik-e olyan alkalmazás, amely közel valós idejű ajánlott beállításokat, kevesebb mint 100 milliomod másodperc körbejárási időt követelményt rendelkező ügyfelek esetén hajt végre. hello késést és teljesítménybeli jellemzőit a Service Fabric szolgáltatás (ahol a javaslat kijelölés hello számítási közös elhelyezésű hello adatok és a szabályok) biztosít egy rugalmas toohello felhasználói hello szabványos végrehajtása hasonlítani a modell, hogy toofetch hello szükséges adatokat.  
* **Munkamenet-alapú interaktív alkalmazások**: a Service Fabric akkor hasznos, ha az alkalmazások, például az online játékok vagy csevegőüzenetben, kis késleltetésű olvasása és írása szükséges. A Service Fabric lehetővé teszi, hogy Ön toobuild ezeket interaktív, az állapotalapú alkalmazások toocreate külön áruházból vagy az állapot nélküli alkalmazások szükség szerint a gyorsítótárban. (Ez növeli a késés és potenciálisan vezet be konzisztenciabeli problémákat.).
* **Adatelemzés és munkafolyamatok**: hello gyors olvasási és írási műveletek a Service Fabric lehetővé teszik az alkalmazások számára megbízhatóan feldolgozni eseményeket vagy adatstreamek. A Service Fabric is lehetővé teszi az alkalmazások feldolgozási folyamatok, ahol eredményeket kell a megbízható és átadott leíró toohello a következő feldolgozás szakasz adatvesztés nélkül. Ezek közé tartozik a tranzakciós és pénzügyi rendszerek, ahol adatok konzisztencia- és számítási garanciák nélkülözhetetlenek.
* **Az adatgyűjtés, feldolgozása és az IoT**: mivel a Service Fabric nagy méretű kezeli, és rendelkezik az állapot-nyilvántartó szolgáltatásokon keresztül az alacsony késleltetés, ideális adatok feldolgozása a eszközök millióira hello eszköz és a számítást, hello hello adatok esetén közös elhelyezésű.
Úgy találtuk, hogy létrehozta a buildet használ, beleértve a Service Fabric IoT rendszerek több felhasználókat [BMW](https://blogs.msdn.microsoft.com/azureservicefabric/2016/08/24/service-fabric-customer-profile-bmw-technology-corporation/), [Schneider elektromos](https://blogs.msdn.microsoft.com/azureservicefabric/2016/08/05/service-fabric-customer-profile-schneider-electric/) és [háló rendszerek](https://blogs.msdn.microsoft.com/azureservicefabric/2016/06/20/service-fabric-customer-profile-mesh-systems/).

## <a name="application-design-case-studies"></a>Alkalmazás tervezési esettanulmányok
Esettanulmányok, amely mutatja, hogyan Service Fabric használt toodesign alkalmazások számos közzé hello [Service Fabric csapat blogja](https://blogs.msdn.microsoft.com/azureservicefabric/tag/customer-profile/) és hello [mikroszolgáltatások megoldások hely](https://azure.microsoft.com/solutions/microservice-applications/).

## <a name="design-applications-composed-of-stateless-and-stateful-microservices"></a>Az állapotmentes és állapotalapú mikroszolgáltatások álló alkalmazások
Alkalmazások az Azure Cloud Service feldolgozói szerepkörök az állapotmentes szolgáltatások egy példája. Ezzel szemben állapot-nyilvántartó mikroszolgáltatások állapotban a mérvadó hello kérelem és a válaszában. Ez biztosítja a magas rendelkezésre állás és a tranzakciós replikáció által támogatott garanciákat nyújt egyszerű API-k segítségével hello állapot konzisztencia. A Service Fabric állapotalapú szolgáltatások democratize magas rendelkezésre állás érdekében tooall típusú alkalmazásokat, nem csak adatbázisok és egyéb adattárakhoz kerülne. Ez a természetes előmenetel. Alkalmazások az tisztán relációs adatbázisok magas rendelkezésre állású tooNoSQL adatbázisok már került át. Maguk hello alkalmazások most a "Forró" állapot és a rajtuk kezelt további teljesítménynövekedés érhető a megbízhatóságot, konzisztencia vagy rendelkezésre állási feláldozása nélkül is rendelkezhetnek.

Ha a mikroszolgáltatások álló alkalmazások létrehozásához, általában akkor az állapot nélküli web apps (ASP.NET, Node.js, stb.) kombinációjából hívja meg az állapotmentes és állapotalapú üzleti középső rétegbeli szolgáltatások telepítését, az összes helyezett hello azonos Service Fabric-fürt hello Service Fabric telepítési parancsok használatával. Ezek a szolgáltatások egy független a legutóbb tooscale, megbízhatóságát és erőforrás-használat, fejlesztési és életciklus-kezelés agilitásának nagy mértékben javítva.

Állapot-nyilvántartó mikroszolgáltatások egyszerűbbé teszik alkalmazás tervek, mert eltávolítják az hello további várólisták hello szükségességét, és, hogy hagyományosan szükséges tooaddress gyorsítótárak hello tisztán állapot nélküli alkalmazások rendelkezésre állás és a késés követelményeinek. Mivel az állapotalapú szolgáltatások természetes magas rendelkezésre állású és alacsony késést, ez azt jelenti, hogy nincsenek-e kevesebb áthelyezése részek toomanage az alkalmazás egészének. az alábbi ábrákon hello állapot nélküli alkalmazások, a másik állapotalapú tervezése hello különbségei mutatják be. Ha kihasználja a hello [Reliable Services](service-fabric-reliable-services-introduction.md) és [Reliable Actors](service-fabric-reliable-actors-introduction.md) programozási modellek, állapotalapú szolgáltatások alkalmazás bonyolultságának csökkentése nagyobb teljesítményt és alacsony késést elérése közben.

## <a name="an-application-built-using-stateless-services"></a>Egy alkalmazás állapotmentes szolgáltatások segítségével
![Állapot nélküli szolgáltatást használó alkalmazások][Image1]

## <a name="an-application-built-using-stateful-services"></a>Egy alkalmazás állapotalapú szolgáltatások segítségével
![Állapot nélküli szolgáltatást használó alkalmazások][Image2]

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések

* Figyeljen túl[ügyfél esettanulmányok](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=qDJnf86yC_5206218965
)
* További információ a [ügyfél esettanulmányok](https://blogs.msdn.microsoft.com/azureservicefabric/tag/customer-profile/)
* További információ [mintákat és forgatókönyvek](service-fabric-patterns-and-scenarios.md)

* Bevezetés a Service Fabric hello állapotmentes és állapotalapú szolgáltatással építése [megbízható szolgáltatások](service-fabric-reliable-services-quick-start.md) és [megbízható szereplője](service-fabric-reliable-actors-get-started.md) programozási modellek.
* A következő témakörök hello is látható:
  * [További részletek a mikroszolgáltatások](service-fabric-overview-microservices.md)
  * [Meghatározhatja és kezelheti a szolgáltatás állapota](service-fabric-concepts-state.md)
  * [A Service Fabric-szolgáltatások rendelkezésre állása](service-fabric-availability-services.md)
  * [Skála Service Fabric-szolgáltatások](service-fabric-concepts-scalability.md)
  * [Partíció Service Fabric-szolgáltatások](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.jpg
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.jpg
