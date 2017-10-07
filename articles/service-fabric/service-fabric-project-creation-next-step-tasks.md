---
title: "aaaService háló projekt létrehozásának további lépések |} Microsoft Docs"
description: "Ez a cikk tartalmaz hivatkozásokat tooa core fejlesztési feladatokhoz a Service Fabric"
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: rwike77
ms.openlocfilehash: 45598bfabedf280fba8af449ef920f40b409a609
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="your-service-fabric-application-and-next-steps"></a>A Service Fabric-alkalmazás és a következő lépések
Az Azure Service Fabric-alkalmazás létrehozása. Ez a cikk ismerteti a projekt és a potenciális lépéseket hello makeup.

## <a name="your-application"></a>Az alkalmazás
Minden egyes új alkalmazás egy alkalmazás projektet tartalmaz. Előfordulhat, hogy egy vagy két további projektek, attól függően, hogy a kiválasztott szolgáltatás hello típusát.

### <a name="hello-application-project"></a>hello projekt
hello projektet tartalmaz:

* Az alkalmazás alkotó hivatkozások toohello szolgáltatások készlete.
* Profilok (1-csomópont helyi 5-csomópont helyi és felhőalapú) használható toomaintain beállításainak különböző környezetekhez – például a beállítások kapcsolódó tooa a fürt végpontja, és hogy tooperform végezze el központi telepítések alapértelmezés szerint három közzététele.
* Három alkalmazásparaméter-fájlokat (megegyezik a fenti) használható toomaintain környezetfüggő alkalmazás beállításai, például egy szolgáltatás partíciók toocreate hello száma.
* A telepítési parancsfájl használható toodeploy az alkalmazás hello parancssorban vagy egy automatizált folyamatos integrációt és telepítést folyamat részeként.
* hello alkalmazásjegyzék, amely hello alkalmazás ismerteti. Hello jegyzékfájl hello ApplicationPackageRoot mappában található.

### <a name="stateless-service"></a>Állapotmentes szolgáltatások
Ha hozzáad egy új állapotmentes szolgáltatások, a Visual Studio hozzáadja-e a szolgáltatás projekt tooyour megoldás, amely tartalmazza a felvett típus `StatelessService`. hello szolgáltatást egy helyi változót a számlálót növeli.

### <a name="stateful-service"></a>Az állapotalapú szolgáltatás
Ha hozzáad egy új állapotalapú szolgáltatásból, Visual Studio hozzáadja-e a szolgáltatási projekt tooyour megoldás, amely tartalmazza a felvett típus `StatefulService`. hello szolgáltatás növeli a számlálót a `RunAsync` metódus és a tárolók hello eredményez a `ReliableDictionary`.

### <a name="actor-service"></a>Aktor szolgáltatás
Ha hozzáad egy új megbízható szereplő, a Visual Studio hozzáadja két projektek tooyour megoldás: az aktor projektek és egy illesztőfelület-projekthez.

hello szereplő projekt beállítás metódusokat biztosít, és a számláló értéke hello beolvasása, amely megbízhatóan megőrzött hello szereplő állapot belül. hello felület projekt olyan felületet biztosít a többi szolgáltatások tooinvoke hello szereplő használhatja.

### <a name="stateless-web-api"></a>Állapot nélküli webes API-k
hello állapotmentes Web API-projektet tartalmaz egy alapszintű webszolgáltatás használható tooopen az alkalmazás tooexternal ügyfelek. Hogyan hello projekt strukturált kapcsolatos további információkért lásd: [Service Fabric Web API szolgáltatások OWIN önálló üzemeltető](service-fabric-reliable-services-communication-webapi.md).


### <a name="aspnet-core"></a>Az ASP.NET core
Service Fabric SDK hello biztosít azonos beállítása az ASP.NET Core sablonokat, amelyeket az ASP.NET Core projektek önálló hello: üres, [Web API][aspnet-webapi], és [webalkalmazás][aspnet-webapp].

### <a name="guest-executables-and-guest-containers"></a>Vendég végrehajtható fájlok és a Vendég-tárolókon

A Service Fabric "Vendég" egy olyan szolgáltatás, nem épített hello platform programozási modellek. Csomagot hello bináris fájlok esetében a Vendég vagy [közvetlenül a hello alkalmazáscsomag](service-fabric-deploy-existing-app.md) vagy [keresztül a tároló lemezkép](service-fabric-deploy-container.md). Mindkét esetben a Visual Studio létrehozza hello hello a szükséges összetevők **ApplicationPackageRoot** hello projekt mappájából. A Visual Studio nem hoz létre egy új service-projekt, mert hello kódot már létezik a rendszerben található. Ha szeretné, hogy a Vendég projektek mellett hello Service Fabric-alkalmazás projekt toomanage, később is hozzáadhatja toohello azonos Visual Studio-megoldáshoz.

## <a name="next-steps"></a>Következő lépések
### <a name="create-an-azure-cluster"></a>Egy Azure-fürt létrehozása
Service Fabric SDK hello biztosít a helyi fürt a fejlesztéshez és teszteléshez. az Azure, a fürt toocreate lásd: [a Service Fabric-fürt beállítása az Azure-portálon hello][create-cluster-in-portal].

### <a name="publish-your-application-tooazure"></a>Az alkalmazás tooAzure közzététele
A közvetlenül a Visual Studio tooan Azure-fürttel alkalmazást is közzétehet. toolearn című témakörben talál [közzététele az alkalmazás tooAzure][publish-app-to-azure].

### <a name="use-service-fabric-explorer-toovisualize-your-cluster"></a>Service Fabric Explorer toovisualize a fürt használatára
Service Fabric Explorer nyújt egy egyszerű módot toovisualize a fürtöt, beleértve a központilag telepített alkalmazások és a fizikai elrendezését. több, lásd: toolearn [a fürt megjelenítése a Service Fabric Explorer használatával][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Verziója és a szolgáltatások frissítésére
A Service Fabric lehetővé teszi, hogy függetlenül verziószámozásának és független az alkalmazás szolgáltatások frissítésével. több, lásd: toolearn [Versioning és a szolgáltatások frissítése][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>A Visual Studio Team Services folyamatos integráció konfigurálása
toolearn hogyan állíthatja be a folyamatos integrációs folyamatban a Service Fabric-alkalmazás, lásd: [folyamatos integráció konfigurálása a Visual Studio Team Services][ci-with-vso].

<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
