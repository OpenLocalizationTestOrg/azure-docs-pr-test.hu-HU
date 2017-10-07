---
title: az Azure Service Fabric aaaArchitecture |} Microsoft Docs
description: "A Service Fabric egy elosztottrendszer-platform használt toobuild méretezhető, megbízható, és könnyen által felügyelt alkalmazások hello felhő. Ez a cikk a Service Fabric a hello architektúráját mutatja be."
services: service-fabric
documentationcenter: .net
author: rishirsinha
manager: timlt
editor: rishirsinha
ms.assetid: 6b554243-70cb-4c22-9b28-1a8b4703f45e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/19/2017
ms.author: rsinha
ms.openlocfilehash: 0268578094ad1a0010ef44ed940f828b985f6c40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-architecture"></a>A Service Fabric-architektúra
A Service Fabric-t a réteges alrendszereket. Alrendszereket toowrite alkalmazások engedélyezése:

* Magas rendelkezésre állású
* Méretezhető
* Kezelhető
* Testable

a következő diagram hello hello fő alrendszereket a Service Fabric jeleníti meg.

![A Service Fabric-architektúra ábrája](media/service-fabric-architecture/service-fabric-architecture.png)

Elosztott rendszer esetén hello képességét toosecurely csomópontjai közötti kommunikációhoz a fürt elengedhetetlen. Hello: hello verem alapja hello átviteli alrendszer, amely biztosít a biztonságos kommunikáció a csomópontok között. Hello átviteli fent alrendszer értékű hello összevonási alrendszer, mely fürtök hello csomópontjai az egyetlen entitás (a fürt neve), hogy a Service Fabric észlelésére, hajtsa végre a vezető választás és egységes útválasztást. hello megbízhatóság alrendszer hello összevonási alrendszer, rétegre felelős a Service Fabric szolgáltatások, például a replikáció, az erőforrás-kezelés és a feladatátvételi mechanizmusokon keresztül hello megbízhatóságát. hello összevonási alrendszer is alapjául szolgáló hello üzemeltető és aktiválási alrendszer, amely egycsomópontos alkalmazás hello életciklusát felügyeli. hello hálózatfelügyeleti alrendszer alkalmazások és szolgáltatások hello életciklusát felügyeli. hello tesztelhetőségi alrendszer a fejlesztőket alkalmazás tesztelése szimulált hibák a szolgáltatásaikat előtt, és az alkalmazások és szolgáltatások tooproduction környezetekben való telepítése után. A Service Fabric tooresolve szolgáltatási helyek keresztül a kommunikációs alrendszer hello lehetőséget nyújt. hello programozási modellek kitett toodevelopers vannak rétegre alrendszereket hello alkalmazás modell tooenable tooling együtt.

## <a name="transport-subsystem"></a>Átviteli alrendszer
hello átviteli alrendszer valósítja meg a pont-pont kommunikáció datagramcsatornából. Ez a csatorna belül service fabric-fürtök és hello service fabric-fürt és az ügyfelek közötti kommunikációt szolgál. Az egyirányú támogatja-e, és hello alapot biztosít szórás és a csoportos küldést végrehajtási kérelem-válasz kommunikációs minták hello összevonási réteg. hello átviteli alrendszer X509 használatával titkosítja a kommunikációs tanúsítványok vagy a Windows biztonsági. Ezt az alrendszer belsőleg Service Fabric, és nem érhető el közvetlenül toodevelopers alkalmazás programozáshoz.

## <a name="federation-subsystem"></a>Összevonási alrendszer
Rendelés tooreason kapcsolatos meg csomópontok elosztott rendszer kell toohave hello rendszer egységes megjelenítése. hello összevonási alrendszer hello kommunikációs primitívek hello átviteli alrendszer által biztosított használja, és stitches hello különböző csomópontok egyetlen egyesített fürtbe, amely azt az okból kapcsolatos is. Hello elosztott rendszerek primitívek szükség szerint hello más alrendszerek - tárhelyhiba-észlelés vezető választás és egységes útválasztási biztosít. hello összevonási alrendszer elosztott kivonattáblákkal 128 bites token szóközzel épül. hello alrendszer létrehoz egy körgyűrűs topológia hello csomópontokat, a lefoglalt hello token terület részhalmazának tulajdonosát hello ring minden csomópontja alatt. A tárhelyhiba-észlelés hello réteg zúzása szív és egyeztetési alapján bérlési mechanizmus használja. hello összevonási alrendszer is biztosítja, hogy bonyolult csatlakozási és, hogy csak egy tulajdonoshoz jogkivonat létezik bármikor indító protokollok keresztül. Így lehetővé teszi a vezető választási és egységes útválasztási garanciát.

## <a name="reliability-subsystem"></a>Megbízhatóság alrendszer
hello megbízhatóság alrendszer biztosít hello mechanizmus toomake hello Service Fabric szolgáltatás állapotának hello hello használata magas rendelkezésre állású *replikátor*, *Feladatátvevőfürt-kezelő*, és  *Erőforrás-elosztó*.

* hello replikátor biztosítja, hogy állapotváltozások hello elsődleges replika automatikusan lesz replikák replikált toosecondary, egy szolgáltatás replikakészlethez hello elsődleges és másodlagos replikák közötti konzisztencia fenntartása. hello replikátor felelős kvórumkezelés hello replikák hello replika között. Hatással van az hello feladatátvételi egységek tooget hello listája műveletek tooreplicate, és a hello újrakonfigurálás ügynök hello replikakészlethez hello konfigurációját. Ez a konfiguráció azt jelzi, hogy mely replikák hello műveletek toobe replikálni kell. A Service Fabric biztosít egy alapértelmezett replikátor nevű háló replikátor, amely programozási modell API toomake hello szolgáltatás állapotának magas rendelkezésre állású és megbízható hello által használható.
* Feladatátvevőfürt-kezelő hello biztosítja, hogy csomópontok hozzáadásakor tooor eltávolított hello fürtről, hello terhelés eloszlik hello elérhető csomópontjai között. Ha hello fürtben egy csomópont meghibásodik, a hello fürt automatikusan konfigurálja újra hello szolgáltatás replikák toomaintain rendelkezésre állását.
* hello erőforrás-kezelő szolgáltatás replikák helyezi hello fürt hiba tartományon keresztül, és biztosítja, hogy az összes feladatátvételi egységek működési. hello erőforrás-kezelő szolgáltatás erőforrásainak is kiegyensúlyozza az alapul szolgáló fürt csomópontjai tooachieve optimális egységes terheléselosztási megosztott készletéhez hello között.

## <a name="management-subsystem"></a>Hálózatfelügyeleti alrendszer
hello hálózatfelügyeleti alrendszer végpont szolgáltatás és alkalmazás-életciklus kezelésének biztosít. PowerShell-parancsmagok és a felügyeleti API-k lehetővé teszik a tooprovision, telepíti, a javítás, frissítés, és leépíti a rendelkezésre állás elvesztése nélkül alkalmazások. hello hálózatfelügyeleti alrendszer hajtja végre ezt a következő szolgáltatások hello keresztül.

* **Fürtkezelő**: Ez az hello elsődleges szolgáltatás, amely együttműködik a Feladatátvevőfürt-kezelő hello alkalmazásaiból megbízhatóság tooplace hello hello csomópontjai hello szolgáltatás placement Constraints korlátozásokat. Erőforrás-kezelő hello feladatátvételi alrendszerben biztosítja, hogy hello megkötések soha nem sérült. hello kezelő a kiépítés toode-provision hello alkalmazások hello életciklusát felügyeli. Hello health manager tooensure, hogy rendelkezésre állásának legyen adatvesztés szemantikai állapotfigyelő szempontjából frissítéskor integrálható.
* **Állapotkezelő**: Ez a szolgáltatás lehetővé teszi, hogy az alkalmazások, szolgáltatások és a fürt entitások állapotfigyelés. Fürt entitások (például a csomópontok szolgáltatáspartíciók és replikák) jelenthetik-e majd összesítve hello központosított a health Store adatbázisban állapotinformációkat. Az állapotinformációkat biztosít egy átfogó-időpontban állapotfigyelő pillanatkép hello szolgáltatások és a csomópontok hello fürt, amely lehetővé teszi, tootake több csomópontja elosztva el a szükséges javítási műveleteket. API-k lehetővé teszik a tooquery hello állapotával kapcsolatos események állapotának lekérdezés toohello állapotfigyelő alrendszer jelentett. hello állapotfigyelő lekérdezés API-k hello állapotfigyelő tárolt hello nyers egészségügyi adatok tárolására, vagy hello összesítve, az egészségügyi adatokat egy adott fürt entitás értelmezni vissza.
* **Image Store**: Ez a szolgáltatás biztosít tárolási és elosztását illetően hello bináris alkalmazásfájlokat. Ez a szolgáltatás biztosítja egy egyszerű elosztott fájltároló letöltésének feltöltött tooand hello alkalmazások esetén.

## <a name="hosting-subsystem"></a>Üzemeltetési alrendszer
hello kezelőt arról értesíti az üzemeltető (minden egyes csomóponton futó) alrendszer, általa nyújtott hello toomanage szüksége van egy adott csomópont. majd üzemeltető alrendszer hello hello alkalmazás ezen a csomóponton hello életciklusát felügyeli. Hello megbízhatóságát és működését összetevők tooensure, hogy hello replikák megfelelően kerülnek, és a kifogástalan állapotú kommunikál.

## <a name="communication-subsystem"></a>Kommunikációs alrendszer
Ezt az alrendszer belül hello fürt és a szolgáltatás felderítése hello Naming Service megbízható üzenetküldést biztosít. hello Naming service oldja fel a neveket tooa szolgáltatáskeresés hello fürtben, és lehetővé teszi, hogy a felhasználók toomanage szolgáltatás neveit és tulajdonságait. Hello elnevezési szolgáltatás használata ügyfelek biztonságosan hello fürt tooresolve bármely csomópontjának kommunikálni a szolgáltatás nevét és szolgáltatás metaadatok lekérése. Egy egyszerű elnevezési ügyfél API-ja használ, a Service Fabric felhasználók fejleszthet szolgáltatások és az aktuális hálózati helyüket hello annak ellenére, hogy a csomópont dinamizmusát vagy hello újra méretezése hello fürt feloldása alkalmas ügyfelek.

## <a name="testability-subsystem"></a>Tesztelhetőségi alrendszer
Tesztelhetőségi olyan eszközkészlet a kifejezetten a Service Fabric épülő szolgáltatások tesztelése. hello eszközök segítségével könnyen értelmezhető hibák idéz elő és tesztelési forgatókönyvek tooexercise futtatni, és ellenőrizze a fejlesztő hello számos állapotok és átmenetek, amelyeknek a szolgáltatás teljes élettartamuk, a felügyelt és biztonságos módon az összes értéket. Tesztelhetőségi is biztosít a mechanizmus toorun hosszabb tesztet, amely a rendelkezésre állás elvesztése nélkül is iterációt különféle lehetséges hibákat. Ez lehetővé teszi a teszt az éles környezetben.

