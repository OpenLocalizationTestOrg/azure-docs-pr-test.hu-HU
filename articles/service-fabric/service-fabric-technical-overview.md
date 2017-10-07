---
title: "Azure Service Fabric-terminológia aaaLearn |} Microsoft Docs"
description: "A Service Fabric terminológia áttekintése. Kulcs terminológia fogalmakat és szakkifejezéseket tartalmazza hello dokumentáció hello többi használt ismerteti."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: chackdan;subramar
ms.assetid: 3a970679-e19e-43b3-9be8-71773f307c57
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/02/2017
ms.author: ryanwi
ms.openlocfilehash: 4781fc0527b8a58e534183249bc2759aded2730b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-terminology-overview"></a>A Service Fabric-terminológia áttekintése
A Service Fabric egy elosztottrendszer platform, így könnyen toopackage, telepítéséhez és felügyeletéhez a méretezhető és megbízható mikroszolgáltatások létrehozására. Ez a témakör részletek hello terminológia használják a Service Fabric toounderstand hello szereplő kifejezések jegyzékét hello dokumentációját.

hello a jelen szakaszban felsorolt fogalmakat is ismerteti a Microsoft Virtual Academy videók következő hello: <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">alapvető fogalmait</a>, <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965">tervezési idejű fogalmak</a>, és <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">futásidejűfogalmak</a>.

## <a name="infrastructure-concepts"></a>Infrastruktúrával kapcsolatos fogalmak
**Fürt**: virtuális vagy fizikai gépek, amelybe a mikroszolgáltatások telepíteni és felügyelni hálózathoz csatlakozó készlete.  Fürtök toothousands gépek méretezhető.

**Csomópont**: egy számítógép vagy virtuális Gépet, amely egy fürt része nevezzük. Minden csomópont hozzá van rendelve egy csomópont neve (karakterlánc). Csomópontok például elhelyezési tulajdonságok jellemzőkkel bírnak. Minden számítógép vagy virtuális gép rendelkezik egy automatikusan induló Windows szolgáltatás `FabricHost.exe`, amely rendszerindítási után elindul, és elindítja a két végrehajtható fájlok: `Fabric.exe` és `FabricGateway.exe`. E két végrehajtható fájlok hello csomópont alkotják. Tesztelési forgatókönyvek, tárolhatja, több csomópont egyetlen számítógép vagy virtuális gép több példányára futtatásával `Fabric.exe` és `FabricGateway.exe`.

## <a name="application-concepts"></a>Alkalmazás fogalmak
**Az alkalmazástípus**: hello szolgáltatástípusok neve/verziója hozzárendelt tooa gyűjteménye. Az egy `ApplicationManifest.xml` fájlt, a beágyazott egy alkalmazás csomag könyvtárában, amely majd másolódik toohello Service Fabric fürt lemezképtárolóhoz. Ez az alkalmazástípus hello fürtön belül majd létrehozhat egy elnevezett alkalmazást.

Olvasási hello [alkalmazásmodell](service-fabric-application-model.md) cikkében találja.

**Alkalmazáscsomag**: hello alkalmazás típust tartalmazó lemez könyvtár `ApplicationManifest.xml` fájlt. Hivatkozásokat az egyes szolgáltatástípusokról hello alkalmazástípus alkotó szolgáltatáscsomagok hello. hello hello alkalmazás csomag könyvtárában fájlok másolt tooService Fabric fürt lemezképtárolóhoz. Például egy alkalmazáscsomagot, egy e-mailek alkalmazás típusra hivatkozik tooa várólista szolgáltatáscsomagot, egy előtér-szolgáltatás csomag és egy adatbázis service-csomag tartalmazhatnak.

**Alkalmazás nevű**: Miután egy alkalmazáscsomagot másolt toohello lemezképtárolóhoz, hello alkalmazás csomag alkalmazás típusa (használja a neve/verziója) megadásával hello alkalmazás hello fürtön belül példányt létrehozni. Minden alkalmazás értéktípus példánya hozzá van rendelve egy URI-nevet, következőhöz hasonló: `"fabric:/MyNamedApp"`. A fürtön belül több nevű alkalmazást is létrehozhat egyetlen alkalmazás típusból. Különböző típusok elnevezett alkalmazások is létrehozhat. Minden elnevezett alkalmazása felügyelt és a rendszerverzióval ellátott egymástól függetlenül.      

**Service Type**: hello neve/verziója hozzárendelt tooa szolgáltatás kód csomagok, adatok csomagokat és konfigurációs csomagokat. Az egy `ServiceManifest.xml` fájl, a szolgáltatás csomag és hello szolgáltatás csomag könyvtárat ágyazott hivatkozik rá egy alkalmazáscsomagot `ApplicationManifest.xml` fájlt. Hello fürtön belül egy elnevezett alkalmazás létrehozása után is létrehozhat egy adott szolgáltatás egyik hello szolgáltatástípusok alkalmazás típusa. hello szolgáltatás típusának `ServiceManifest.xml` fájl hello szolgáltatást ismerteti.

Olvasási hello [alkalmazásmodell](service-fabric-application-model.md) cikkében találja.

A szolgáltatások két típusa van:

* **Stateless:** állapotmentes szolgáltatások használja, ha egy külső tároló szolgáltatás, például az Azure Storage, az Azure SQL Database vagy az Azure Cosmos DB hello szolgáltatás állandó-állapot tárolása. Állapot nélküli szolgáltatást használ, amikor hello szolgáltatás egyáltalán nem állandó tároló rendelkeznek. Például egy Számológép szolgáltatással, ahol érték lett átadva toohello szolgáltatás, a számítási segítségével végezhető el ezeket az értékeket, és eredményt adja vissza.
* **Állapotalapú alkalmazások és szolgáltatások:** állapot-nyilvántartó szolgáltatást használ, ha azt szeretné, a Service Fabric toomanage a szolgáltatás állapotának a megbízható gyűjtemények vagy a Reliable Actors programozási modell használatával. Adja meg, hogy hány partíciók kívánt toospread a állapota (méretezhetőségre) Ha a nevesített szolgáltatás létrehozása. Is megadhatja, hogy hányszor tooreplicate a állapota (a megbízhatóság) csomópontjai között. Minden elnevezett szolgáltatásához tartozik egy elsődleges replikát, és több másodlagos replika. A nevesített szolgáltatás állapotának módosítása írásával toohello elsődleges másodpéldány. A Service Fabric replikálja a állapot tooall hello másodlagos replikák a állapot szinkronban tartja. A Service Fabric automatikusan észleli, ha egy elsődleges másodpéldány nem sikerül, és elősegíthető a egy létező másodlagos replika tooa elsődleges másodpéldány. A Service Fabric majd létrehoz egy új másodlagos replikára.  

**Szolgáltatáscsomag**: hello szolgáltatás típust tartalmazó lemez könyvtár `ServiceManifest.xml` fájlt. Ez a fájl hello kódot, a statikus adatok és a konfigurációs csomagok hello szolgáltatás típus hivatkozik. hello fájlok hello szolgáltatás csomag könyvtárban hello alkalmazás típusa által hivatkozott `ApplicationManifest.xml` fájlt. Például a szolgáltatáscsomagot is hivatkozhat toohello kódot, statikus adatok és egy adatbázis-szolgáltatás alkotó konfigurációs csomagokat.

**Nevű szolgáltatás**: egy elnevezett alkalmazás létrehozása után létrehozhat egy példányát, a szolgáltatás típusú hello fürtön belül hello szolgáltatás típusa (használja a neve/verziója) megadásával. Minden szolgáltatáspéldány típus hozzá van rendelve egy elnevezett alkalmazásának URI alatt hatókörű URI nevét. Például, ha létrehoz egy "adatbázis" nevű szolgáltatás belül "MyNamedApp" nevű alkalmazás, hello URI néz ki: `"fabric:/MyNamedApp/MyDatabase"`. Egy elnevezett alkalmazáson belül több elnevezett szolgáltatások is létrehozhat. Minden elnevezett szolgáltatás a saját partícióséma is rendelkezik, és a replika/példány adatokra.

**Csomag Code**: végrehajtható fájlok hello szolgáltatás típusa (általában a következő EXE/DLL-fájlok) tartalmazó lemez könyvtár. hello fájlok hello kódjának csomag könyvtárában hello szolgáltatás típusának által hivatkozott `ServiceManifest.xml` fájlt. Egy adott szolgáltatás létrehozásakor hello kódcsomag másolt toohello csomópontot vagy csomópontokat kijelölt toorun hello nevű szolgáltatás. Majd hello kód elindul. A kód csomag végrehajtható két típusa van:

* **Vendég végrehajtható fájlok**: végrehajtható fájlok, amelyek futtató-hello állomás operációs rendszeren (Windows vagy Linux). Ez azt jelenti, hogy a végrehajtható fájlok nem tooor hivatkozás a Service Fabric futásidejű fájlok ne, ezért nem használhatja a Service Fabric programozási modellek. A végrehajtható fájlok néhány a Service Fabric szolgáltatás, például hello szolgáltatást a végpont felderítése elnevezési nem toouse. Vendég végrehajtható fájlok betöltési metrikák adott tooeach szolgáltatáspéldány nem tud jelenteni.
* **Szolgáltatás állomás végrehajtható fájlok**: végrehajtható fájlok, amelyek a Service Fabric programozási modellek tooService háló futtatási fájlokat, létrehozhatja a Service Fabric-szolgáltatások engedélyezését. Például a megnevezett példánya végpontok regisztrálhatja a Service Fabric-szolgáltatás, és terhelési metrika is jelentheti.      

**Adatcsomag**: hello szolgáltatás típusának statikus, csak olvasható adatok fájlok (általában fénykép, hang-, és videó) tartalmazó lemez könyvtár. az hello csomag adatkönyvtárában hello fájlok hello szolgáltatás típusának által hivatkozott `ServiceManifest.xml` fájlt. Egy adott szolgáltatás létrehozásakor hello adatcsomag másolt toohello csomópontot vagy csomópontokat kijelölt toorun hello nevű szolgáltatás.  hello kód elindul, és most már elérheti az adatfájlok hello.

**Konfigurációs csomag**: hello szolgáltatás típusának statikus tartalmazó lemez könyvtár, csak olvasható konfigurációs fájlok (általában szöveg fájlok). hello fájlok hello konfigurációs csomag könyvtárban hello szolgáltatás típusának által hivatkozott `ServiceManifest.xml` fájlt. Egy adott szolgáltatás létrehozásakor hello fájlok hello konfigurációs csomag másolt toohello egy vagy több csomópontja kijelölt toorun hello nevű szolgáltatás. Hello kód majd elindul, és most már elérheti az hello konfigurációs fájlok.

**Tárolók**: alapértelmezés szerint a Service Fabric telepíti, és aktiválja a szolgáltatást is folyamatok. A Service Fabric szolgáltatások tároló képek is telepítheti. Tárolók egy virtualizációs technológia, amely Virtualizálja a hello alapul szolgáló operációs rendszer alkalmazások. Az alkalmazások és a futtatókörnyezet, a függőségeket és a rendszer szalagtárak futhat egy tároló teljes, privát hozzáférést toohello tároló saját elkülönített láthassák operációs rendszer szerkezeteket. A Service Fabric Docker-tároló támogatja a Linux és Windows Server-tárolókra vonatkozóan.  További információkért olvassa el a [Service Fabric és a tárolók](service-fabric-containers-overview.md).

**Partícióazonosító séma**: egy adott szolgáltatás létrehozásakor megadott partícióséma. Szolgáltatások állapota nagy mennyiségű adatok hello osztani partíciók, amelyek között osztja el a hello állapot hello fürt csomópontjai között. Ez lehetővé teszi, hogy az adott szolgáltatás állapota tooscale. A partíciókon belül elnevezett állapotmentes szolgáltatások példánya lehet, míg a állapot-nyilvántartó elnevezett szolgáltatások replikák. Általában elnevezett állapotmentes szolgáltatásokhoz csak kell egy partíciót óta belső állapot nélküli rendelkeznek. hello partíció példányok adja meg a rendelkezésre állás érdekében; egy példány nem sikerül, ha más esetekben továbbra is toooperate általában, és a Service Fabric majd létrehoz egy új példányát. Állapotalapú alkalmazások és szolgáltatások nevű szolgáltatások karbantartása állapotukra belül replikákat, és mindegyik partíció rendelkezik saját replikakészlet szinkronban tartani minden hello állapotú. Amennyiben nem egy replikát, a Service Fabric hello meglévő replikák új replikát hoz létre.

Olvasási hello [partíció Service Fabric megbízható szolgáltatások](service-fabric-concepts-partitioning.md) cikkében találja.

## <a name="system-services"></a>Rendszerszolgáltatások
Nincsenek rendszerszolgáltatások minden fürtben létrehozott, adja meg a Service Fabric hello platform képességeit.

**Naming Service**: minden Service Fabric-fürt rendelkezik elnevezési szolgáltatással, amely nevek tooa szolgáltatáskeresés hello fürt mutat. Kezelheti a hello szolgáltatás nevét és tulajdonságait, hasonló tooan internetes tartománynév-szolgáltatás (DNS) hello fürthöz. Az ügyfelek biztonságosan tudjon kommunikálni hello szolgáltatás tooresolve használatával hello fürt bármely csomópontjára a szolgáltatás nevét és a helyére.  Alkalmazások áthelyezése, például toofailures, erőforrás terheléselosztás vagy hello hello fürt átméretezése miatt hello fürtön belül. Szolgáltatások és az ügyfelek, amelyek hello aktuális hálózati hely feloldása fejleszthet. Ügyfelek hello tényleges gép IP-cím és port, amelyen fut beszerzése.

Olvasási [szolgáltatásokkal kommunikáljon](service-fabric-connect-and-communicate-with-services.md) hello ügyfél és a szolgáltatás kommunikációs API-t a Naming service hello olvashat.

**Image Store szolgáltatás**: minden Service Fabric-fürt rendelkezik egy Image Store szolgáltatás ahol telepített, verzióval ellátott alkalmazáscsomagok tartanak. Egy alkalmazás csomag toohello Image Store másolja, és regisztrálja az adott alkalmazás csomagban lévő hello alkalmazástípus. Hello alkalmazástípus üzembe helyezése után hoz létre egy elnevezett alkalmazás azt. Hello Image Store szolgáltatás egy alkalmazás típusának tudja törölni, miután az elnevezett alkalmazásokhoz, hogy törölték.

Olvasási [hello előtaggal beállítás megértéséhez](service-fabric-image-store-connection-string.md) hello Image Store szolgáltatás további információt.

Olvasási hello [alkalmazás üzembe helyezése](service-fabric-deploy-remove-applications.md) cikk alkalmazások toohello kép store szolgáltatás telepítésével kapcsolatos további információt.

## <a name="built-in-programming-models"></a>Beépített programozási modellek
Nincsenek elérhető az Ön toobuild Service Fabric szolgáltatások .NET-keretrendszer programozási modellek:

**Megbízható szolgáltatások**: az API toobuild állapotmentes és állapotalapú szolgáltatással. Az állapotalapú szolgáltatás megbízható gyűjtemények állapotukra (például egy könyvtár, illetve a várólista) tárolja. Például a Web API és a Windows Communication Foundation (WCF) különböző kommunikációs verem tooplug juthat.

**Reliable Actors**: az API toobuild állapotmentes és állapotalapú hello virtuális szereplő programozási modell használatával objektumok. Ez a modell akkor lehet hasznos, ha nagyszámú független egység számítási/állapot. Mivel ez a modell kapcsolja alapú szálmodell használ, akkor ajánlott tooavoid kódot, amely behívja tooother szereplője vagy szolgáltatások egy egyedi szereplő más bejövő kérelmek nem dolgozható fel addig, amíg az összes kimenő kérelem végrehajtása óta.

Olvasási hello [válasszon programozási modellt a szolgáltatáshoz](service-fabric-choose-framework.md) cikkében találja.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések
További információk a Service Fabric toolearn:

* [A Service Fabric áttekintése](service-fabric-overview.md)
* [Miért egy mikroszolgáltatások közelítse toobuilding alkalmazások?](service-fabric-overview-microservices.md)
* [Alkalmazáshasználati helyzetek](service-fabric-application-scenarios.md)

