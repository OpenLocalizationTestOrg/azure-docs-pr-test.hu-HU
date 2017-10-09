---
title: "További információk az Azure Service Fabric aaaLearn |} Microsoft Docs"
description: "Hello alapvető fogalmait és főbb területei Azure Service Fabric megismerése. A Service Fabric kiterjesztett áttekintése és hogyan toocreate mikroszolgáltatások létrehozására."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/14/2017
ms.author: ryanwi
ms.openlocfilehash: 7fe8de777755be11635912613bb5b970e3fe3ea3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="so-you-want-toolearn-about-service-fabric"></a>Ezért azt szeretné, hogy a Service Fabric kapcsolatos toolearn?
Az Azure Service Fabric egy elosztottrendszer platform, így könnyen toopackage, telepítéséhez és felügyeletéhez a méretezhető és megbízható mikroszolgáltatások létrehozására.  A Service Fabric rendelkezik egy nagy felületet biztosít, és nincs sokkal toolearn.  Ez a cikk a Service Fabric kivonatát biztosít, és hello alapfogalmak programozási modellek, alkalmazás-életciklus, fürtök és a állapotfigyeléssel teszteléséhez ismerteti. Olvasási hello [áttekintése](service-fabric-overview.md) és [mikroszolgáltatások Mik?](service-fabric-overview-microservices.md) bevezetést és a Service Fabric hogyan lehet használt toocreate mikroszolgáltatások létrehozására. Ez a cikk átfogó tartalom listája nem tartalmaz, de a hivatkozások, toooverview és a Service Fabric minden területéhez elindított cikkek beolvasása. 

## <a name="core-concepts"></a>Alapfogalmak
[A Service Fabric-terminológia](service-fabric-technical-overview.md), [alkalmazásmodell](service-fabric-application-model.md), és [támogatott programozási modellek](service-fabric-choose-framework.md) adja meg a további fogalmakat és leírásokat, de itt hello alapjai.

<table><tr><th>Alapfogalmak</th><th>Tervezési idő</th><th>Futási idő</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/CoreConceptsVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965"><img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">
<img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td></tr>
</table>

### <a name="design-time-application-type-service-type-application-package-and-manifest-service-package-and-manifest"></a>Tervezési idő: alkalmazástípusok, szolgáltatás típusa, alkalmazáscsomag és jegyzék, szolgáltatáscsomag és jegyzék
Az alkalmazástípus hello neve/verziója hozzárendelt szolgáltatástípusok tooa gyűjteménye. Ez a van definiálva egy *ApplicationManifest.xml* fájlt, amely egy alkalmazás csomag könyvtárában van beágyazva. hello alkalmazáscsomag majd másolódik toohello Service Fabric fürt lemezképtárolóhoz. Létrehozhat egy nevű alkalmazás az alkalmazás típusa, majd futtatja a hello fürtön belül futó majd. 

A szolgáltatás típus hello neve/verziója hozzárendelt tooa szolgáltatás kód csomagok, adatok csomagokat és konfigurációs csomagokat. Ez egy ServiceManifest.xml fájlt, amely egy service-csomag könyvtárában van beágyazva van meghatározva. hello szolgáltatás csomag könyvtárában majd hivatkozik egy alkalmazáscsomagot *ApplicationManifest.xml* fájlt. Hello fürtön belül egy elnevezett alkalmazás létrehozása után is létrehozhat egy adott szolgáltatás egyik hello szolgáltatástípusok alkalmazás típusa. A szolgáltatás írja le annak *ServiceManifest.xml* fájlt. hello szolgáltatás típusának végrehajtható kód szolgáltatás konfigurációs beállításokat, amelyek futási időben betöltése, és a statikus adatok hello szolgáltatás által felhasznált állnak.

![A Service Fabric alkalmazás és szolgáltatás típusainak][cluster-imagestore-apptypes]

hello alkalmazáscsomag hello alkalmazás típust tartalmazó lemez könyvtár *ApplicationManifest.xml* fájlt, amely az egyes szolgáltatástípusokról hello alkalmazástípus alkotó hello szolgáltatás csomagok hivatkozik. Például egy alkalmazáscsomagot, egy e-mailek alkalmazás típusra hivatkozik tooa várólista szolgáltatáscsomagot, egy előtér-szolgáltatás csomag és egy adatbázis service-csomag tartalmazhatnak. hello hello alkalmazás csomag könyvtárában fájlok másolt toohello Service Fabric fürt lemezképtárolóhoz. 

A service-csomag hello szolgáltatás típust tartalmazó lemez könyvtár *ServiceManifest.xml* fájlt, amely hello kódot, a statikus adatok és a konfigurációs csomagok hello szolgáltatás típus hivatkozik. hello fájlok hello szolgáltatás csomag könyvtárban hello alkalmazás típusa által hivatkozott *ApplicationManifest.xml* fájlt. Például a szolgáltatáscsomagot is hivatkozhat toohello kódot, statikus adatok és egy adatbázis-szolgáltatás alkotó konfigurációs csomagokat.

### <a name="run-time-clusters-and-nodes-named-applications-named-services-partitions-and-replicas"></a>Futási idő: fürtök és a csomópontok, alkalmazások, szolgáltatások, a partíciók és a replikák nevű nevű
A [Service Fabric-fürt](service-fabric-deploy-anywhere.md) virtuális és fizikai gépek hálózaton keresztül csatlakozó készlete, amelyen mikroszolgáltatásokat helyezhet üzembe és felügyelhet. Fürtök toothousands gépek méretezhető.

Egy számítógép vagy virtuális Gépet, amely egy fürt része egy csomópont neve. Minden csomópont hozzá van rendelve egy csomópont neve (karakterlánc). Csomópontok például elhelyezési tulajdonságok jellemzőkkel bírnak. Minden számítógép vagy virtuális gép rendelkezik egy automatikusan induló Windows szolgáltatás `FabricHost.exe`, amely rendszerindítási után elindul, és elindítja a két végrehajtható fájlok: `Fabric.exe` és `FabricGateway.exe`. E két végrehajtható fájlok hello csomópont alkotják. Fejlesztési és tesztelési forgatókönyvekhez, tárolhatja, több csomópont egyetlen számítógép vagy virtuális gép több példányára futtatásával `Fabric.exe` és `FabricGateway.exe`.

Egy nevesített alkalmazásra az elnevezett szolgáltatások gyűjteménye, amely egy bizonyos funkciója vagy funkciói hajt végre. A szolgáltatás végzi a teljes és az önálló (akkor is elindításához és egyéb szolgáltatások függetlenül) és a kódot, a konfiguráció és az adatok tevődik össze. Miután egy alkalmazáscsomagot másolt toohello lemezképtárolóhoz, hello alkalmazás hello fürtön belül példányt hoz létre (használja a neve/verziója) hello alkalmazás csomag alkalmazástípus megadásával. Minden alkalmazás típuspéldány hozzá van rendelve egy URI-nevet, amely a következőképpen néz *fabric: / MyNamedApp*. A fürtön belül több nevű alkalmazást is létrehozhat egyetlen alkalmazás típusból. Különböző típusok elnevezett alkalmazások is létrehozhat. Minden elnevezett alkalmazása felügyelt és a rendszerverzióval ellátott egymástól függetlenül.

Egy nevesített alkalmazás létrehozása után is létrehozhat szolgáltatás típusa (egy adott szolgáltatás) egy példányának hello fürtön belül hello szolgáltatás típusa (használja a neve/verziója) megadásával. Minden szolgáltatáspéldány típus hozzá van rendelve egy elnevezett alkalmazásának URI alatt hatókörű URI nevét. Például, ha létrehoz egy "adatbázis" nevű szolgáltatás belül "MyNamedApp" nevű alkalmazás, hello URI néz ki: *fabric: / MyNamedApp/adatbázis*. Egy elnevezett alkalmazáson belül létrehozhat egy vagy több nevű szolgáltatás. Minden elnevezett szolgáltatás a saját partícióséma is rendelkezik, és a replika/példány adatokra. 

Szolgáltatások két típusa van: az állapotmentes és állapotalapú. Állapotmentes szolgáltatások állandó állapotát tárolhatja például az Azure Storage, az Azure SQL Database vagy az Azure Cosmos DB egy külső tároló szolgáltatást. Állapot nélküli szolgáltatást használ, amikor hello szolgáltatás egyáltalán nem állandó tároló rendelkeznek. Az állapotalapú szolgáltatás használ a Service Fabric toomanage a szolgáltatás állapotának a megbízható gyűjtemények vagy a Reliable Actors programozási modell használatával. 

Egy adott szolgáltatás létrehozásakor meg kell adni egy partícióséma. Szolgáltatások állapota nagy mennyiségű adatok hello osztani partíciók. Minden partíciója hello szolgáltatást, amely hello fürt csomópontjai között megoszlik teljes állapotának hello részét. A partíciókon belül elnevezett állapotmentes szolgáltatások példánya lehet, míg a állapot-nyilvántartó elnevezett szolgáltatások replikák. Általában elnevezett állapotmentes szolgáltatásokhoz csak kell egy partíciót óta belső állapot nélküli rendelkeznek. Állapotalapú alkalmazások és szolgáltatások nevű szolgáltatások karbantartása állapotukra belül replikákat, és mindegyik partíció rendelkezik saját replikakészlet. Olvasási és írási műveleteket, egy replika (más néven hello elsődleges). Módosítások toostate írási műveletek esetében a replikált toomultiple más replikák (más néven aktív másodlagos). 

hello alábbi ábrán látható alkalmazások és a szolgáltatáspéldány, a partíciók és a replikák hello kapcsolatát.

![Partíciók és replikáit a szolgáltatáson belül][cluster-application-instances]

### <a name="partitioning-scaling-and-availability"></a>Particionálás, a méretezés és a rendelkezésre állás
[Particionálás](service-fabric-concepts-partitioning.md) nincs egyedi tooService háló. Egy jól ismert particionálás formátuma adatparticionálás vagy horizontális. Állapotalapú szolgáltatások állapota nagy mennyiségű adatok hello osztani partíciók. Mindegyik partíció felelős hello hello szolgáltatás teljes állapota része. 

Mindegyik partíció hello replikáinak vannak elosztva hello fürt csomópontja, amely lehetővé teszi, hogy az adott szolgáltatás állapota túl[méretezési](service-fabric-concepts-scalability.md). Hello adatok igények nő, partíciók nő, és a Service Fabric partíciók csomópontok toomake hatékony kihasználását hardver-erőforrások között újra egyensúlyba hozza.. Ha új csomópontok toohello fürt, a Service Fabric egyensúlyba a hello partíció replikák csomópontok száma nagyobb hello között. Általános javítja az alkalmazások teljesítménye, és csökkenti a versengés a hozzáférési toomemory. Hello fürt csomópontja hello hatékonyan nincsenek használatban, ha hello fürtben található csomópontok számának hello csökkenthető. A Service Fabric újra újra egyensúlyba hozza hello partíció replikák csomópontok toomake jobb használata minden egyes csomóponton hello hardver csökkent hello száma különböző.

A partíciókon belül elnevezett állapotmentes szolgáltatások példánya lehet, míg a állapot-nyilvántartó elnevezett szolgáltatások replikák. Általában elnevezett állapotmentes szolgáltatásokhoz csak kell egy partíciót óta belső állapot nélküli rendelkeznek. hello partíció példányok nyújtása [rendelkezésre állási](service-fabric-availability-services.md). Ha nem sikerül egy példánya, más esetekben továbbra is toooperate általában pedig a Service Fabric létrehoz egy új példányt. Állapotalapú alkalmazások és szolgáltatások nevű szolgáltatások karbantartása állapotukra belül replikákat, és mindegyik partíció rendelkezik saját replikakészlet. Olvasási és írási műveleteket, egy replika (más néven hello elsődleges). Módosítások toostate írási műveletek esetében a replikált toomultiple más replikák (más néven aktív másodlagos). Amennyiben nem egy replikát, a Service Fabric hello meglévő replikák új replikát hoz létre.

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>Az állapotmentes és állapotalapú mikroszolgáltatások a Service Fabric
A Service Fabric lehetővé teszi toobuild alkalmazások, amelyek mikroszolgáltatások vagy a tárolókat állnak. Állapot nélküli mikroszolgáltatások (például a protokoll-átjáró és a webes proxykat) nem tartanak egy változtatható állapotot egy kérelem és a hello szolgáltatás válasza kívül. Azure Cloud Services feldolgozói szerepkörök olyan állapotmentes szolgáltatások példát. Állapot-nyilvántartó mikroszolgáltatások (például a felhasználói fiókok, adatbázisok, eszközök, bevásárlási kártyák és a várólisták) hello kérelem és a válaszában változtatható, mérvadó állapotának karbantartása. Napjaink Internet-méretű alkalmazások állapotmentes és állapotalapú mikroszolgáltatások kombinációjából áll. 

A Service Fabric egy kulcs differentation az állapotalapú szolgáltatások, vagy hello erős összpontosít [programozási modellek beépített ](service-fabric-choose-framework.md) vagy indexelése állapotalapú szolgáltatással. Hello [alkalmazás-forgatókönyvekre](service-fabric-application-scenarios.md) hello forgatókönyvek, ahol állapotalapú szolgáltatások használják mutatják be.

Miért kell állapotalapú mikroszolgáltatások állapotmentes ők együtt? hello két fő oka a következők:

* Nagy átviteli, alacsony késésű, hibatűrő online tranzakció-feldolgozási (OLTP-) szolgáltatások tartása kóddal, és zárja be az adatok hello ugyanaz a gép hozhat létre. Néhány többek között az interaktív kirakatokkal, keresést, az eszközök internetes hálózatát (IoT) rendszerek, kereskedési rendszerek-megnövelt, hitelkártya feldolgozási és csalás rendszerek és személyes rekord felügyeleti.
* Egyszerűbbé teheti az alkalmazás tervét. Állapot-nyilvántartó mikroszolgáltatások távolítsa el a további várólisták hello szükségességét, és gyorsítótárak, amelyek hagyományosan szükséges tooaddress hello rendelkezésre állás és a késés tisztán állapot nélküli alkalmazások követelményeinek. Állapotalapú szolgáltatások természetes, magas rendelkezésre állású és alacsony késésű, ami csökkenti az alkalmazás egészének áthelyezése részek toomanage hello száma.

## <a name="supported-programming-models"></a>Támogatott programozási modellek
A Service Fabric több módon toowrite kínál, és a szolgáltatások kezeléséhez. Szolgáltatások használhatnak hello Service Fabric API-k tootake teljes mértékben hello platform szolgáltatásai és alkalmazás-keretrendszerek számára. Szolgáltatások bármilyen lefordított végrehajtható bármilyen nyelven írt, és a Service Fabric fürt által futtatott is lehet. További információkért lásd: [támogatott programozási modellek](service-fabric-choose-framework.md).

### <a name="containers"></a>Tárolók
Alapértelmezés szerint a Service Fabric telepíti, és aktiválja a szolgáltatást is folyamatokat. A Service Fabric is telepítheti a szolgáltatások [tárolók](service-fabric-containers-overview.md). Fontos, kombinálhatja a folyamatokat a szolgáltatások és szolgáltatások hello a tárolókban lévő ugyanahhoz az alkalmazáshoz. A Service Fabric Windows Server 2016 Linux tárolók Windows tárolók központi telepítését támogatja. A meglévő alkalmazásokat, állapotmentes szolgáltatásokhoz vagy tárolókban lévő állapotalapú szolgáltatások telepítése. 

### <a name="reliable-services"></a>Reliable Services
[Megbízható szolgáltatások](service-fabric-reliable-services-introduction.md) egyszerűsített keretrendszere, amely a Service Fabric-platformról hello integrálása és platform szolgáltatások teljes készlete hello kihasználhassa szolgáltatások írása. Megbízható szolgáltatásokat lehet állapotmentes (hasonló toomost szolgáltatás platformokat, például a webkiszolgálók vagy feldolgozói szerepkörök az Azure Cloud Services), a külső megoldás például Azure DB vagy az Azure Table Storage állapotot tároló. Megbízható szolgáltatásokat is lehet állapotfüggő, állapotot tároló közvetlenül a megbízható gyűjtemények használatával hello szolgáltatást. Állapot legyen [magas rendelkezésre állású](service-fabric-availability-services.md) replikáció útján és elosztott keresztül [particionálás](service-fabric-concepts-partitioning.md), az összes kezelt Service Fabric által automatikusan.

### <a name="reliable-actors"></a>Reliable Actors
Hello Reliable Services platformra épül, [megbízható szereplő](service-fabric-reliable-actors-introduction.md) keretrendszer egy alkalmazás-keretrendszer, amely megvalósítja az hello virtuális szereplő mintát, hello szereplő kialakítási minta alapján. hello megbízható szereplő keretrendszer független egység a számítási műveletek és az állapot nevű szereplője egyszálas végrehajtási használ. a beépített hello keretrendszer által biztosított megbízható szereplő szereplője és előre beállított állapot megőrzését és kibővített konfigurációk közötti kommunikáció.

### <a name="aspnet-core"></a>ASP.NET-mag
Integrálható a Service Fabric [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) webes és API-alkalmazások készítéséhez első osztályú programozási modell

### <a name="guest-executables"></a>Vendég végrehajtható fájlok
A [Vendég végrehajtható](service-fabric-deploy-existing-app.md) meglévő, tetszőleges végrehajtható (bármilyen nyelven írt) más szolgáltatásokkal együtt a Service Fabric fürt által futtatott. Vendég végrehajtható fájlok integrálható közvetlenül Service Fabric API-k. Azonban továbbra is részesülnek funkciók platform ajánlatokat, például az egyéni állapotot hello és betölteni a jelentéskészítési és felderíthetőség szolgáltatás REST API-k hívásával. Teljes alkalmazás életciklusa támogatási is rendelkeznek. 

## <a name="application-lifecycle"></a>Alkalmazáséletciklus
És egyéb platformok, a Service Fabric alkalmazás általában végig kell vinnie a következő fázisok hello: tervezési, fejlesztési, tesztelési, központi telepítés, frissítés, karbantartási és eltávolítása. A Service Fabric első osztályú támogatást nyújt a felhőalapú alkalmazásokhoz, a központi telepítés, napi felügyeleti és karbantartási tooeventual leszerelése fejlesztési hello alkalmazás teljes életciklusát. hello modell lehetővé teszi, hogy több különböző szerepkörök tooparticipate hello alkalmazás életciklusa az egymástól függetlenül. [A Service Fabric-alkalmazás életciklusa](service-fabric-application-lifecycle.md) az API-k hello áttekintése és megismerkedhet használatukkal hello különböző szerepkörök hello fázisai hello Service Fabric-alkalmazás életciklusa során. 

hello teljes alkalmazás-életciklus felügyelhetők [PowerShell-parancsmagok](/powershell/module/ServiceFabric/), [C# API-k](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [Java API-k](/java/api/system.fabric._application_management_client), és [REST API-k](/rest/api/servicefabric/). Is beállíthat folyamatos integrációt/folyamatos telepítési folyamatok eszközökkel, például a [Visual Studio Team Services](service-fabric-set-up-continuous-integration.md) vagy [Jenkins](service-fabric-cicd-your-linux-java-application-with-jenkins.md).

hello alábbi Microsoft Virtual Academy videó ismerteti, hogyan toomanage az alkalmazás-életciklus:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=My3Ka56yC_6106218965">
<img src="./media/service-fabric-content-roadmap/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="test-applications-and-services"></a>Alkalmazások és szolgáltatások tesztelése
toocreate valóban felhőméretű szolgáltatások, fontos, hogy az alkalmazások és szolgáltatások is hardverkapacitás valós kritikus tooverify. hello hiba az Analysis Services szolgáltatásokról, amelyek a Service Fabric épülnek tesztelési tervezték. A hello [hiba az Analysis Services](service-fabric-testability-overview.md), idéz elő a jelentéssel bíró hibák, és a teljes teszt forgatókönyvek az alkalmazások futtatásához. Ezeket a hibákat és forgatókönyvek gyakorolja és érvényesítése hello számos állapotok és átmenetek, amelyeknek a szolgáltatás teljes élettartamuk, minden olyan módon ellenőrzött, biztonságos és konzisztens.

[Műveletek](service-fabric-testability-actions.md) céloz tesztelésre használt egyes hibák szolgáltatás. A szolgáltatás fejlesztők építőelemeket bonyolult toowrite forgatókönyvek ezek segítségével. Például szimulált hibák a következők:

* Indítsa újra a csomópont toosimulate tetszőleges számú helyzetekben, ahol egy számítógép vagy virtuális gép újraindítása után.
* Helyezze át az állapotalapú szolgáltatási toosimulate terheléselosztás, feladatátvétel vagy az alkalmazásfrissítés replikáját.
* Egy állapotalapú szolgáltatási toocreate olyan helyzet, ahol nem folytatható az írási műveletek, mert nincs elég "biztonsági másolat" vagy "másodlagos" replikák tooaccept új adatokat a kvórum elvesztése meghívni.
* Egy állapotalapú szolgáltatási toocreate olyan helyzet, ahol minden a memóriában teljesen tárolt adatok törléséig kimenő adatvesztéshez meghívni.

[Forgatókönyvek](service-fabric-testability-scenarios.md) összetett műveletek álló egy vagy több művelet. Hiba az Analysis Services hello két beépített teljes eseteit tartalmazza:

* [Chaos forgatókönyv](service-fabric-controlled-chaos.md)-hosszú időn keresztül a folyamatos, kihagyásos hibák (szabályos és ungraceful) hello fürt teljes szimulálja.
* [Feladatátvételi forgatókönyv](service-fabric-testability-scenarios.md#failover-test)-hello chaos tesztkörnyezet, amelynek célja egy adott szolgáltatás partíció úgy, hogy más szolgáltatások nem érinti a verzióját.

## <a name="clusters"></a>Fürtök
A [Service Fabric-fürt](service-fabric-deploy-anywhere.md) virtuális és fizikai gépek hálózaton keresztül csatlakozó készlete, amelyen mikroszolgáltatásokat helyezhet üzembe és felügyelhet. Fürtök toothousands gépek méretezhető. Egy számítógép vagy virtuális Gépet, amely egy fürt része egy fürtcsomópont neve. Minden csomópont hozzá van rendelve egy csomópont neve (karakterlánc). Csomópontok például elhelyezési tulajdonságok jellemzőkkel bírnak. Minden számítógép vagy a virtuális gép rendelkezik egy automatikusan induló szolgáltatás `FabricHost.exe`, amely rendszerindítási után elindul, és elindítja a két végrehajtható fájlok: Fabric.exe és FabricGateway.exe. E két végrehajtható fájlok hello csomópont alkotják. Tesztelési forgatókönyvek, tárolhatja, több csomópont egyetlen számítógép vagy virtuális gép több példányára futtatásával `Fabric.exe` és `FabricGateway.exe`.

Service Fabric-fürtök a Windows Server vagy Linux rendszerű virtuális vagy fizikai gépek hozhatók létre. Képes toodeploy, és a Service Fabric-alkalmazások futtatása bármely környezetben, összekapcsolt Windows Server vagy Linux rendszerű számítógépek esetében: a helyszíni Microsoft Azure, vagy bármely felhőalapú szolgáltató.

hello alábbi Microsoft Virtual Academy videó ismerteti a Service Fabric-fürt:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/ClusterOverview.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="clusters-on-azure"></a>Fürtök az Azure-on
Integráció más Azure-szolgáltatások és szolgáltatások, egyszerűbb és megbízhatóbb műveletek és a felügyeleti fürt hello így Service Fabric-fürtök futó Azure biztosít. A fürt egy Azure Resource Manager erőforrás, így a modellezhető fürtök, mint bármely más erőforrások, az Azure-ban. A Resource Manager is biztosít kezelésének egyetlen egységként hello fürt által használt összes erőforrást. Azure-fürtök integrált Azure diagnostics és az Naplóelemzési. Fürt csomópont típusok [virtuálisgép-méretezési csoportok](/azure/virtual-machine-scale-sets/index), így az automatikus skálázás funkciót részét.

Létrehozhat egy fürt az Azure-on keresztül hello [Azure-portálon](service-fabric-cluster-creation-via-portal.md), a egy [sablon](service-fabric-cluster-creation-via-arm.md), vagy a [Visual Studio](service-fabric-cluster-creation-via-visual-studio.md).

hello előzetes verzióját a Service Fabric Linux lehetővé teszi a toobuild, telepítéséhez és felügyeletéhez a magas rendelkezésre állású, nagymértékben méretezhető alkalmazások Linux, ugyanúgy, mint Windows rendszeren. hello Service Fabric keretrendszerek (Reliable Services és Reliable Actors) érhetők el a Linux, Java hozzáadása tooC # (.NET Core). Is létrehozható [végrehajtható vendégszolgáltatások](service-fabric-deploy-existing-app.md) rendelkező bármilyen nyelven vagy keretrendszerben. Emellett a hello előzetes verziója is támogatja a koordinálása Docker-tároló. Docker-tároló Vendég végrehajtható fájlok, illetve hello Service Fabric-keretrendszert használó natív Service Fabric szolgáltatások futtathatók. További információkért olvassa el a [Service Fabric Linux](service-fabric-linux-overview.md).

Mivel a Service Fabric Linux rendszeren előzetes verziójú, ezért néhány szolgáltatás csak Windows rendszeren támogatott. több, olvassa el az toolearn [Service Fabric Linux rendszeren és Windows közötti különbségek](service-fabric-linux-windows-differences.md).

### <a name="standalone-clusters"></a>Önálló fürtök
A Service Fabric egy csomag biztosít, akkor toocreate különálló Service Fabric fürt a helyszíni vagy bármely felhőalapú szolgáltató. Önálló fürtök szabadsága toohost fürt hello, ahol azt szeretné, adja meg. Ha az adatok tulajdonos toocompliance vagy jogszabályi korlátozásokat, vagy a adatok helyi tookeep kívánja, a saját fürt és az alkalmazások tárolhatja. Service Fabric-alkalmazások futtathat több üzemeltetési környezetekben változtatás nélkül, így a egy üzemeltetési környezet tooanother végzi az alkalmazások ismerete. 

[Az első Service Fabric önálló fürt létrehozása](service-fabric-get-started-standalone-cluster.md)

Linux önálló fürtök még nem támogatott.

### <a name="cluster-security"></a>Fürtbiztonság
Fürtök biztonságos tooprevent nem engedélyezett felhasználók tooyour fürthöz csatlakozzon, különösen akkor, ha a termelési számítási feladatokhoz fut rajta van kell lennie. Bár lehetséges toocreate egy nem biztonságos fürthöz, így lehetővé teszi a névtelen felhasználók tooconnect tooit felügyeleti végpontok esetén kitett toohello nyilvános internethez. Már nem lehetséges toolater security engedélyezése egy nem biztonságos fürt: fürt biztonsági engedélyezve van a fürt létrehozása során.

hello fürt biztonsági forgatókönyvek a következők:
* Csomópontok biztonsági
* Ügyfél-csomópont biztonsági
* Szerepköralapú hozzáférés-vezérlést (RBAC)

További információkért olvassa el a [fürt biztonságos](service-fabric-cluster-security.md).

### <a name="scaling"></a>Méretezés
Ha új csomópontok toohello fürt, a Service Fabric újra egyensúlyba hozza hello partíció replikák és példányok csomópontok száma nagyobb hello között. Általános javítja az alkalmazások teljesítménye, és csökkenti a versengés a hozzáférési toomemory. Hello fürt csomópontja hello hatékonyan nincsenek használatban, ha hello fürtben található csomópontok számának hello csökkenthető. A Service Fabric újra újra egyensúlyba hozza hello partíció replikákat, és példányok között csomópontok toomake száma csökkent hello jobban használja hello hardver minden egyes csomóponton. Azure-fürtök vagy méretezheti [manuálisan](service-fabric-cluster-scale-up-down.md) vagy [programozott módon](service-fabric-cluster-programmatic-scaling.md). Önálló fürtök méretezhetők [manuálisan](service-fabric-cluster-windows-server-add-remove-nodes.md).

### <a name="cluster-upgrades"></a>Fürt frissítése
Rendszeres időközönként hello Service Fabric-futtatókörnyezet új verzióinak kiadásakor. Hajtsa végre a futtatókörnyezet, vagy fordítva, frissítések a fürtön, hogy mindig futtatja egy [támogatott verziója](service-fabric-support.md). Ezenkívül toofabric frissíti, is frissítheti a fürt konfigurációját, például tanúsítványok vagy alkalmazás portok.

A Service Fabric-fürt, amely a saját, de részben Microsoft által felügyelt erőforrás. A Microsoft az alapul szolgáló operációs rendszer és a fürtön lévő fabric-frissítések végrehajtása javítási hello felelős. Állítsa be a fürt tooreceive automatikus háló frissítéseket, ha Microsoft kiadott új verzióra, vagy válassza a tooselect támogatott háló kívánt. Háló és a konfigurációs frissítések hello Azure-portálon keresztül vagy a Resource Manager használatával állítható be. További információkért olvassa el a [a Service Fabric-fürt frissítése](service-fabric-cluster-upgrade.md). 

Egy önálló fürthöz egy erőforrást, amikor teljesen saját. Az alapul szolgáló operációs rendszer, és kezdeményezi a fabric frissítéskezelésének javítási hello felelős áll. Ha a fürt túl csatlakozni tud-e[https://www.microsoft.com/download](https://www.microsoft.com/download), beállíthatja, hogy a fürt tooautomatically letöltés, és a kiépítés hello új Service Fabric futásidejű csomag. Akkor lesz majd indítsa el hello frissítés. Ha nem érhető el a fürt [https://www.microsoft.com/download](https://www.microsoft.com/download), manuálisan letöltheti internet csatlakoztatott gépből hello új futásidejű csomag és hello frissítés majd indítsa el. További információkért olvassa el a [különálló Service Fabric fürt frissítése](service-fabric-cluster-upgrade-windows-server.md).

## <a name="health-monitoring"></a>Állapotfigyelés
A Service Fabric vezet be a [állapotmodell](service-fabric-health-introduction.md) tooflag sérült fürt és az alkalmazás feltételeket az adott entitások (például a fürtcsomópontok és a szolgáltatás replikák). hello állapotmodell állapotfigyelő jelentéskészítők (rendszer összetevőit és watchdogs) használja. könnyű és gyors diagnosztikai és javítási hello célja. Szolgáltatás írók kell toothink előzetes megfizetése esetén kapcsolatos állapotát, és hogyan túl[állapotfigyelő reporting tervezése](service-fabric-report-health.md#design-health-reporting). Minden olyan esetben, kedvezőtlen hatással lehet az állapotfigyelő kell kiválasztását, különösen akkor, ha segíthet jelző problémák toohello legfelső szintű bezárásához. hello állapottal kapcsolatos adatok is mentheti időt és erőfeszítést Hibakeresés és vizsgálat után hello szolgáltatás működik-e és fut léptékű éles környezetben.

a Service Fabric-jelentéskészítők nevű figyelő hello azonosított feltételek egyik fontos. Azok a helyi nézet alapján feltételek jelentést. Hello [a health Store adatbázisban](service-fabric-health-introduction.md#health-store) összesíti az összes jelentéskészítők toodetermine által küldött, hogy az entitások globálisan kifogástalan állapotának adatait. hello modell tervezett toobe sokoldalú, rugalmasan és egyszerűen toouse. hello állapotjelentések hello minőségének hello állapot nézet hello fürt hello pontosságát határozza meg. Téves tévesen jeleníti meg a nem megfelelő negatívan befolyásolhatja a frissítések vagy az egészségügyi adatokat használó más szolgáltatásokba. Ezek a szolgáltatások többek között a javítása és a riasztási mechanizmusokat. Néhány gondolat ezért hello érdeklődik feltételeinek rögzítéséhez szükséges tooprovide jelentések lehetséges módja az ajánlott.

Jelentéskészítési teheti meg:
* hello figyeli a rendszer a Service Fabric szolgáltatás replika vagy a példány.
* Belső watchdogs telepített Service Fabric szolgáltatás (például a Service Fabric állapotmentes szolgáltatások, amely figyeli a feltételek és a jelentések problémák). hello watchdogs is telepíthető az összes olyan csomóponton, vagy hozzárendelt toohello figyelt szolgáltatás lehet.
* Belső watchdogs hello Service Fabric-csomópontokon futó, de nem valósult meg a Service Fabric szolgáltatásként.
* Külső watchdogs mintavételi hello erőforrás a külső hello Service Fabric-fürt (például figyelőszolgáltatás Gomez hasonlóan).

Hello kezdő verzióról a Service Fabric összetevői jelentés állapotfigyelő hello fürt valamennyi entitást. [Rendszerállapot-jelentések](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) adja meg a fürt- és funkcióit és jelző problémák keresztül állapotfigyelő láthatósága. Az alkalmazások és szolgáltatások rendszerállapot-jelentések ellenőrizze, hogy entitások megvalósított hello Service Fabric-futtatókörnyezet hello szempontjából megfelelően működik. hello jelentéseket bármely állapotának figyelésére hello logikájának hello szolgáltatást, vagy nem lefagyott folyamatok észlelésére. tooadd egészségügyi adatokat adott tooyour szolgáltatás logika [valósítja meg az egyéni állapotfigyelő reporting](service-fabric-report-health.md) a szolgáltatásban.

A Service Fabric túl több lehetőséget biztosít[rendszerállapot-jelentések megtekintéséhez](service-fabric-view-entities-aggregated-health.md) összesítve hello a health Store adatbázisban:
* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) vagy más képi megjelenítés eszközök.
* Állapotlekérdezések (keresztül [PowerShell](/powershell/module/ServiceFabric/), hello [C# FabricClient API-k](/api/system.fabric.fabricclient.healthclient) és [Java FabricClient API-k](/java/api/system.fabric._health_client), vagy [REST API-k](/rest/api/servicefabric)).
* Általános lekérdezések, amelyek entitások listáját adja vissza (a PowerShell, hello API vagy REST) hello tulajdonságok egyikének állapota lehet.

hello a következő Microsoft Virtual Academy videó hello Service Fabric állapotmodellt és használatának módját ismerteti:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tevZw56yC_1906218965">
<img src="./media/service-fabric-content-roadmap/HealthIntroVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="next-steps"></a>Következő lépések
* Megtudhatja, hogyan toocreate egy [az Azure-ban fürt](service-fabric-cluster-creation-via-portal.md) vagy egy [önálló fürt Windows](service-fabric-cluster-creation-for-windows-server.md).
* Próbáljon meg létrehozni egy hello szolgáltatást [Reliable Services](service-fabric-reliable-services-quick-start.md) vagy [Reliable Actors](service-fabric-reliable-actors-get-started.md) programozási modellek.
* Ismerje meg, hogyan túl[Felhőszolgáltatások át](service-fabric-cloud-services-migration-differences.md).
* Ismerje meg, túl[figyelése és diagnosztizálása szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). 
* Ismerje meg, túl[tesztelni az alkalmazások és szolgáltatások](service-fabric-testability-overview.md).
* Ismerje meg, túl[kezelése és levezényléséhez fürterőforrások](service-fabric-cluster-resource-manager-introduction.md).
* Nézze át a hello [Service Fabric minták](http://aka.ms/servicefabricsamples).
* További tudnivalók [Service Fabric támogatási lehetőségek](service-fabric-support.md).
* Olvasási hello [csapat blogján](https://blogs.msdn.microsoft.com/azureservicefabric/) cikkeket és közleményeket.


[cluster-application-instances]: media/service-fabric-content-roadmap/cluster-application-instances.png
[cluster-imagestore-apptypes]: ./media/service-fabric-content-roadmap/cluster-imagestore-apptypes.png
