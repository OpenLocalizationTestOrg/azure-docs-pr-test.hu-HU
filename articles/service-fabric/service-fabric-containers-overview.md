---
title: "a Service Fabric és a tárolók aaaOverview |} Microsoft Docs"
description: "A Service Fabric és hello áttekintést tárolók toodeploy mikroszolgáltatási alkalmazások használatát. A cikkben megtudhatja, hogyan tárolók is használhatók, és elérhető képességek a Service Fabric hello áttekintését."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: c98b3fcb-c992-4dd9-b67d-2598a9bf8aab
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/16/2017
ms.author: msfussell
ms.openlocfilehash: fce94c4b476351c90f23f706aab8bc17319cce22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-and-containers"></a>A Service Fabric és a tárolók
> [!NOTE]
> A funkció jelenleg előzetes Linux.  A Windows 10-es tooa Service Fabric-fürt nem támogatott tárolók üzembe helyezése, miközben a (hamarosan elérhető). 
>   

## <a name="introduction"></a>Bevezetés
Az Azure Service fabric egy [orchestrator](service-fabric-cluster-resource-manager-introduction.md) szolgáltatások gépet fürtön belül, használatát és az optimalizálást a jelentős mértékű évnyi szolgáltatásokat, amely Microsoft. Szolgáltatások fejleszthetők hello használata az sokféleképpen [programozási modellek Service Fabric](service-fabric-choose-framework.md) toodeploying [Vendég végrehajtható fájlok](service-fabric-deploy-existing-app.md). Alapértelmezés szerint a Service Fabric telepíti, és ezek a szolgáltatások, folyamatok aktiválja. Folyamatok hello leggyorsabb aktiválási és hello erőforrások a fürt lehető legmagasabb sűrűség használati biztosítanak. A Service Fabric szolgáltatások tároló képek is telepítheti. Fontos, kombinálhatja a folyamatokat a szolgáltatások és szolgáltatások hello a tárolókban lévő ugyanahhoz az alkalmazáshoz. 

## <a name="containers-and-service-fabric-roadmap"></a>Tárolók és a Service Fabric terv
A következő kiadásokban számos fejlesztést tervbe van véve a Service Fabric tároló vezénylési. Fejlesztései közé tartozik a továbbfejlesztett hálózati támogatja a virtuális hálózatokon belül egy alkalmazást, a biztonsági funkciók, a jobb diagnosztika, és eszköztámogatás funkciói. A Service Fabric lehetővé teszi a meglévő kódot (például az IIS MVC alkalmazások) tartalmazó tárolók keverése hello Service Fabric programozási modell segítségével létrehozott szolgáltatásokkal toocreate alkalmazások.  Több ilyen alkalmazást futtathatja ugyanazon a fürtön. 

## <a name="what-are-containers"></a>Mik azok a tárolók?
Tárolók a következőkkel van beágyazva, külön-külön telepíthető összetevők, amelyek elszigetelt példányai futtassa hello azonos kernel tootake előnyeit, amely az operációs rendszer biztosít a virtualizálási. Így minden alkalmazás és a futtatókörnyezet, a függőségeket és a rendszer tárak futhat egy tároló teljes, privát hozzáférést toohello tároló saját elkülönített láthassák operációs rendszer szerkezeteket. Hordozhatósága, valamint biztonsági és erőforrás-elkülönítést az ilyen mértékű hello fő előnye a Service Fabric, amelyek egyébként szolgáltatást futtat folyamatokat a tárolók használata.

Tárolók egy virtualizációs technológia, amely Virtualizálja a hello alapul szolgáló operációs rendszer alkalmazások. Tárolók alkalmazások toorun elkülönítési különböző mértékben megváltoztathatatlan környezetet biztosíthat. Tárolók közvetlenül hello kernel fut, és egy elkülönített nézet hello fájlrendszer és más erőforrásokat. Az összehasonlított toovirtual gépek tárolók rendelkeznek a következő előnyöket hello:

* **Kis**: tárolók használata egy egyetlen tárolási hely és a réteg verziója és a frissítések tooincrease hatékonyságát.
* **Gyors**: tárolók nem rendelkezik tooboot teljes operációs rendszer, így indításához sokkal gyorsabb, általában másodpercben.
* **Hordozhatósága**: egy indexelése alkalmazás-lemezképet áthelyezni toorun hello felhőben, a helyszíni virtuális gépeken belül, vagy közvetlenül a fizikai gépek lehet.
* **Erőforrás-irányítás**: A tárolók korlátozhatja hello fizikai erőforrásokat vehet igénybe, a gazdagépen.

## <a name="container-types"></a>Tároló típusok
Service Fabric tárolók támogatja a Linux és a Windows, és az utóbbi hello támogatja a Hyper-V elkülönítési módot. 

### <a name="docker-containers-on-linux"></a>Linux docker-tároló
Docker magas szintű API-k toocreate biztosít, és a tárolók fölött Linux kernel tárolók kezelése. Docker Hub egy központi tárházban toostore és tároló képek beolvasása.
Az oktatóanyagok esetén lásd: [központi telepítése egy Docker-tároló tooService háló](service-fabric-get-started-containers-linux.md).

### <a name="windows-server-containers"></a>Windows Server-tárolók
Windows Server 2016 biztosít, amely nem egyezik a megadott elkülönítési szintjét hello tárolók két különböző típusú. Windows Server-tárolók és a Docker-tárolók hasonlítanak, mivel a névtér és a fájl rendszer elkülönítési, de a megosztás hello kernel futnak a hello gazdával is. Linux, elkülönítés hagyományosan által szolgáltatott `cgroups` és `namespaces`, és a Windows Server-tárolók hasonlóan viselkednek.

A Windows Hyper-V-tárolók további elkülönítési és biztonsági adja meg, mert minden egyes tároló nem hello operációs rendszer kernel más tárolók vagy hello állomás. A magasabb szintű biztonsági elkülönítés a Hyper-V tárolók rosszindulatú, több-bérlős forgatókönyvek célozzák.
Az oktatóanyagok esetén lásd: [központi telepítése a Windows-tároló tooService háló](service-fabric-get-started-containers.md).

hello alábbi ábrán látható hello különböző típusú virtualizálás és az elkülönítési szint hello operációs rendszeren elérhető.
![Service Fabric-platformról][Image1]

## <a name="scenarios-for-using-containers"></a>Tárolók használatára vonatkozó forgatókönyvek
Íme jellemző példa arra, ahol a tároló, akkor a:

* **IIS növekedési, és az eltolás mértékét megadó**: Ha rendelkezik meglévő [ASP.NET MVC](https://www.asp.net/mvc) toocontinue toouse, kívánt alkalmazások helyezze őket ahelyett, hogy a tároló áttelepítése őket tooASP.NET Core. ASP.NET MVC alkalmazások az Internet Information Services (IIS) határozza meg. Az alkalmazások a tároló képek kép: az IIS precreated hello csomagot, és azok a Service Fabric. Lásd: [tároló lemezképek a Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/quick_start/quick_start_images) információ toocreate IIS képek.
* **Tárolók és a Service Fabric mikroszolgáltatások**: meglévő tároló lemezképet használja az alkalmazás a része. Például használhatja a hello [NGINX tároló](https://hub.docker.com/_/nginx/) a hello előtér az alkalmazás és állapotalapú szolgáltatások hello intenzívebb háttér-számítási.
* **"Zajos szomszédok" szolgáltatások hatásának csökkentéséhez**: hello erőforrás irányítási lehetősége tárolók toorestrict hello erőforrások a szolgáltatás által használt egy gazdagépen is használhatja. Ha szolgáltatásokat lehet, hogy sok erőforrást és hello teljesítményét mások (például egy hosszan futó, a lekérdezés-szerű művelet), fontolja meg, ezek a szolgáltatások üzembe erőforrás irányítás tárolókat.

## <a name="service-fabric-support-for-containers"></a>A Service Fabric tárolók támogatása
A Service Fabric jelenleg támogatja a Docker-tároló központi telepítését, a Linux és Windows Server-tárolókra vonatkozóan a Windows Server 2016 működési feltételeit, támogatja a Hyper-V elkülönítési üzemmódját. 

A Service Fabric hello [alkalmazásmodell](service-fabric-application-model.md), a tároló jelöli egy alkalmazásgazda, mely több szolgáltatásban replikák kerülnek. A Service Fabric tárolókkal futtathatja, és hello forgatókönyv hasonló toohello [Vendég végrehajtható forgatókönyv](service-fabric-deploy-existing-app.md), ahol a becsomagolt belül egy tárolót egy meglévő alkalmazást. Ebben a forgatókönyvben hello gyakori használati eset az tárolókat, és példák bármilyen nyelvet vagy keretrendszert használja, de nem használ hello beépített Service Fabric programozási modellek írása egy alkalmazást futtat.

Ezenkívül futtatása a Service Fabric szolgáltatások belül tárolókat is beleértve. Tárolók belül futó Service Fabric-szolgáltatások támogatása jelenleg, de a következő kiadásokban továbbfejlesztett toobe korlátozódik.

* **Megbízható állapotmentes szolgáltatások tárolókba**: megbízható állapotmentes szolgáltatások hello programozási modellek csak támogatott Linux Reliable Services használatával. Windows-tárolókban lévő megbízható állapotmentes szolgáltatások támogatása tervezünk-e egy későbbi kiadásban.
* **Állapotalapú szolgáltatások tárolókba**: ezek a szolgáltatások hello Reliable Actors vagy Reliable Services programozási modellt használja. Támogatás az állapot-nyilvántartó szolgáltatásokat futtató tárolókban lévő megkapja a jövőbeli kiadás.

A Service Fabric van több tárolóhoz képességeket, amelyek segítenek a mikroszolgáltatások létrehozására, amelyek indexelése épülnek alkalmazásokat hozhatnak létre. A Service Fabric ajánlatok hello indexelése services lehetőségeit a következő:

* Tároló lemezkép-telepítés és az aktiválás.
* Erőforrás-irányítás.
* Tárház-hitelesítés.
* Tárolóportot toohost porthozzárendelést.
* Tároló-tároló felderítése és a kommunikáció.
* Képes tooconfigure és környezeti változók értékét.

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, tárolók, kapcsolatban, hogy a Service Fabric egy tárolót az orchestrator, és a Service Fabric tárolókat támogató szolgáltatások. A következő lépésben azt ismerteti az egyes szolgáltatások tooshow hello példák, hogyan toouse őket.

[Egy Windows-tároló tooService Windows Server 2016 háló telepítése](service-fabric-get-started-containers.md)

[Egy Docker-tároló tooService Linux háló telepítése](service-fabric-get-started-containers-linux.md)

[Image1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png
