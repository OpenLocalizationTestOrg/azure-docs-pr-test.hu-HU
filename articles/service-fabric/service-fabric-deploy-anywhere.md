---
title: "a Windows Server és Linux aaaCreate Azure Service Fabric-fürtök |} Microsoft Docs"
description: "Futtassa a Windows Server és Linux, ami azt jelenti, hogy a Service Fabric-fürtök fogja tudni toodeploy és a gazdagép Service Fabric-alkalmazások bárhol is futtathatja a Windows Server vagy Linux rendszerű."
services: service-fabric
documentationcenter: .net
author: Chackdan
manager: timlt
editor: 
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/08/2017
ms.author: chackdan
ms.openlocfilehash: 46d5f3d019339c57a0024f5a9d47d9018cca01a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Service Fabric-fürtök létrehozása a Windows Server vagy Linux
Az Azure Service Fabric-fürt olyan hálózathoz csatlakozó virtuális vagy fizikai gépek, amelybe a mikroszolgáltatások telepíteni és felügyelni. Egy számítógép vagy virtuális Gépet, amely egy fürt része egy fürtcsomópont neve. Fürt a csomópontok toothousands méretezheti. Ha új csomópontok toohello fürt, a Service Fabric újra egyensúlyba hozza hello szolgáltatás partíció replikák és példányok csomópontok száma nagyobb hello között. Általános javítja az alkalmazások teljesítménye, és csökkenti a versengés a hozzáférési toomemory. Hello fürt csomópontja hello hatékonyan nincsenek használatban, ha hello fürtben található csomópontok számának hello csökkenthető. A Service Fabric újra újra egyensúlyba hozza hello partíció replikákat, és példányok között csomópontok toomake száma csökkent hello jobban használja hello hardver minden egyes csomóponton.

A Service Fabric lehetővé teszi, hogy a virtuális gépek vagy a Windows Server vagy Linux rendszerű számítógépeken a Service Fabric-fürtök hello létrehozásához. Ez azt jelenti, hogy képes toodeploy, és a Service Fabric-alkalmazások futtatása bármely környezetben, összekapcsolt Windows Server vagy Linux rendszerű számítógépek esetében, akkor a helyszíni Microsoft Azure vagy bármely felhőalapú szolgáltatóhoz.

## <a name="create-service-fabric-clusters-on-azure"></a>Az Azure Service Fabric-fürtök létrehozása
Fürt létrehozása az Azure-on vagy erőforrás-modellje sablon vagy hello Azure-portálon keresztül végezhető el. Olvasási [a Service Fabric-fürt létrehozása a Resource Manager-sablon használatával](service-fabric-cluster-creation-via-arm.md) vagy [hello Azure-portálon a Service Fabric-fürt létrehozása](service-fabric-cluster-creation-via-portal.md) további információt.

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Azure-fürtök támogatott operációs rendszerek
Az ilyen operációs rendszert futtató virtuális gépeken futó képes toocreate fürtök áll:

* Windows Server 2012 R2
* Windows Server 2016 
* Linux Ubuntu 16.04 (a nyilvános előzetes verzió) 

## <a name="create-service-fabric-standalone-clusters-on-premises-or-with-any-cloud-provider"></a>A Service Fabric önálló helyszíni fürtök létrehozása vagy bármely felhőalapú szolgáltatóhoz
A Service Fabric egy telepítési csomagot biztosít, akkor toocreate különálló Service Fabric fürt a helyszíni vagy bármely felhőalapú szolgáltató

A különálló service fabric beállításával kapcsolatos további információkat a Windows Server-fürtök, olvassa el a [Service Fabric-fürt létrehozása a Windows Server](service-fabric-cluster-creation-for-windows-server.md)

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>A felhő telepítések és a helyszíni központi telepítések összehasonlítása
hello a Service Fabric-fürt létrehozása a helyszínen történik, hasonló toohello folyamat a fürt létrehozása az Ön által választott bármely felhő virtuális gépek vannak beállítva. hello kezdeti lépéseket tooprovision hello virtuális gépek hello felhő szolgáltató vagy a helyszíni környezetben használt vonatkozik. Miután rendelkező virtuális gépek halmaza közöttük, engedélyezve van a hálózati kapcsolat hello lépéseket tooset hello Service Fabric-csomag mentése, hello fürt beállítások szerkesztése, majd futtatható hello fürt létrehozása és felügyeleti parancsfájlok megegyezik. Ez biztosítja, hogy a Tudásbázis és üzemeltetése és kezelése a Service Fabric-fürtök élménye átruházható tootarget új üzemeltetési környezetekben kiválasztásakor.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Különálló Service Fabric-fürtök létrehozása előnyei
* Szabad toochoose áll a felhőbeli szolgáltató toohost a fürthöz.
* Service Fabric alkalmazásai, egyszer, a minimális toono változások több üzemeltetési környezetekben is futtatható.
* A Service Fabric-alkalmazások ismerete végzi, egy üzemeltetési környezet tooanother a.
* Működési élménye futtatásáért és felügyeletéért felelős a Service Fabric-környezetek tooanother a fürtök keresztül végzi.
* Széles körű felhasználói reach unbounded üzemeltetési környezet megkötések.
* Megbízhatóság és a széles körű kimaradások elleni védelem egy további réteget létezik, mert hello szolgáltatások átvitele tooanother környezet, ha egy adott adatközpont vagy a felhőbeli szolgáltatónak van-e egy blackout.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Támogatott operációs rendszerek különálló fürtök
A virtuális gépek vagy ezek operációs rendszert futtató számítógépek képesek toocreate fürtök áll:

* Windows Server 2012 R2
* Windows Server 2016 
* Linux (hamarosan elérhető)

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Különálló Service Fabric-fürtök keresztül Azure Service Fabric-fürtök előnyei helyszíni létrehozása
Azure-on futó Service Fabric-fürtök előnyökkel felett hello helyszíni beállítást, így ha a fürtöket futtató az igény szerinti nem rendelkezik, majd javasoljuk, hogy az Azure-on futtatja. Az Azure a integráció más Azure-szolgáltatások és szolgáltatások, amely lehetővé teszi a műveletek és a felügyeleti fürt hello egyszerűbb és megbízhatóbb nyújtunk.

* **Azure-portálon:** Azure-portálon teszi, hogy könnyen toocreate, és kezelheti a fürtöket.
* **Az Azure Resource Manager:** használja az Azure Resource Manager lehetővé teszi, hogy egy egységként hello fürt által használt erőforrások kezelésének és egyszerűbbé teszi a költségek nyomon követését és számlázási.
* **Service Fabric-fürt Azure erőforrásként** A Service Fabric-fürtöt egy ARM-erőforrás, így a mintának használhatjuk, mint amikor más ARM erőforrások az Azure-ban.
* **Integráció az Azure infrastruktúra** Service Fabric koordinálja az alapul szolgáló Azure-infrastruktúra az operációs rendszer, hálózati, és más frissítések tooimprove rendelkezésre állási és az alkalmazások megbízhatóságát hello.  
* **Diagnosztika:** Azure, az Azure diagnostics és Naplóelemzési nyújtunk integráció.
* **Automatikus skálázással:** az Azure-fürtök esetén nyújtunk miatt tooVirtual gép méretezési-csoportok beépített automatikus skálázás funkciót. A helyszíni, vagy egyéb felhőalapú környezetben rendelkezik toobuild saját automatikus skálázás funkció vagy skálája hello Service Fabric elérhetővé tévő API-k segítségével manuálisan fürtöket a méretezéshez.

## <a name="next-steps"></a>Következő lépések

* Hozzon létre egy fürtöt virtuális gépek vagy a Windows Server rendszerű számítógépek: [Service Fabric-fürt létrehozása a Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Fürt létrehozása a virtuális gépek vagy Linux operációs rendszert futtató számítógépeken: [Service Fabric Linux rendszeren](service-fabric-linux-overview.md)
* A [Service Fabric támogatási lehetőségeinek](service-fabric-support.md) ismertetése

