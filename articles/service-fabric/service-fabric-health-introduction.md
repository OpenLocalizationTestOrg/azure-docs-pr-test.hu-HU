---
title: "a Service Fabric figyelési aaaHealth |} Microsoft Docs"
description: "Egy bevezető toohello Azure Service Fabric állapotfigyelési modell, amely az hello fürt és az alkalmazások és szolgáltatások felügyelete."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 1d979210-b1eb-4022-be24-799fd9d8e003
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: oanapl
ms.openlocfilehash: 904f36374ca6ca7e4caa1d43c92584e7e4c50087
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooservice-fabric-health-monitoring"></a>Bevezetés tooService Fabric állapotfigyelésének
Az Azure Service Fabric egy állapotmodell sokoldalú, rugalmasan és bővíthető állapotának kiértékelését és a jelentéskészítés biztosító vezet be. hello modell lehetővé teszi, hogy a közel valós idejű hello fürt és a benne lévő hello szolgáltatás hello állapotának figyelését. Egyszerűen állapottal kapcsolatos adatok beszerzéséhez, és kijavíthatja az esetleges problémák ahhoz, hogy kaszkádolt és okozhat nagy kimaradások esetén. Hello tipikus modellben szolgáltatások küldjön jelentést a helyi nézetek alapján, és hogy információkat összesíteni tooprovide egy általános fürt szintű nézetben.

A Service Fabric összetevői használja a gazdag állapotfigyelő modell tooreport a jelenlegi állapotában. Használhatja az alkalmazásokat az azonos mechanizmus tooreport állapotát hello. Fektet kiváló minőségű állapotfigyelő reporting, amely rögzíti az egyéni feltételek, ha észleli, és a futó alkalmazás sokkal könnyebben kapcsolatos problémák megoldása.

hello a következő Microsoft Virtual Academy videó is ismerteti hello Service Fabric állapotmodellt és használatának módját:<center><a target="_blank" href="https://mva.microsoft.com/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tevZw56yC_1906218965">
<img src="./media/service-fabric-health-introduction/HealthIntroVid.png" WIDTH="360" HEIGHT="244">
</a></center>

> [!NOTE]
> Hello állapotfigyelő alrendszer tooaddress figyelt frissítésre szükség lépések azt. A Service Fabric biztosít a figyelt alkalmazás és a fürt frissítések, amelyek biztosítják a teljes rendelkezésre állását, nincs állásidő és a minimális toono felhasználói beavatkozást. Ezen célok hello frissítés állapota alapján ellenőrzi tooachieve frissítési házirendet konfigurált. Egy frissítést folytatni lehessen, csak akkor, ha a rendszerállapot tiszteletben tartja a kívánt küszöbértékeket. Ellenkező esetben hello frissítés vagy automatikusan vissza lesz állítva, vagy egy alkalommal toofix hello problémák toogive rendszergazdák szünetel. További információ az alkalmazás frissítései toolearn lásd [Ez a cikk](service-fabric-application-upgrade.md).
> 
> 

## <a name="health-store"></a>A Health Store adatbázisban
hello a health Store adatbázisban tartja entitások állapotfigyelő kapcsolatos adatokat könnyen szabályzatokat beolvasó és kiértékelő hello fürtökben. Mivel a Service Fabric megőrzött állapot-nyilvántartó szolgáltatás tooensure magas rendelkezésre állás és méretezhetőség megvalósítása. hello a health Store adatbázisban része hello **fabric: / System** alkalmazás, és elérhető hello fürt működik-e és fut.

## <a name="health-entities-and-hierarchy"></a>Állapotfigyelő entitásokat és a hierarchia
hello állapotfigyelő entitások rögzíti a kapcsolati és a függőségek között különféle entitásokat logikai hierarchiában lévő vannak rendezve. hello a health Store adatbázisban a health entitásokat és a hierarchiában a Service Fabric összetevői érkezett jelentés alapján automatikusan létrehozza.

hello állapotfigyelő entitások hello Service Fabric entitások tükrözik. (Például **állapotfigyelő alkalmazás entitás** megegyezik egy alkalmazáspéldányt telepített hello fürt, miközben **állapotfigyelő csomópont entitás** megegyezik a Service Fabric fürt csomópontján fut.) hello állapotfigyelő hierarchia rögzíti hello kapcsolati hello rendszerentitások, és speciális állapotának kiértékelését hello alapját képezi. Megismerheti a Service Fabric alapfogalmak [Service Fabric a műszaki áttekintés](service-fabric-technical-overview.md). Az alkalmazás kapcsolatban bővebben lásd: [Service Fabric-alkalmazás modell](service-fabric-application-model.md).

hello állapotfigyelő entitásokat és a hierarchia engedélyezése hello fürt és az alkalmazások toobe hatékonyan jelentett indítja és figyelemmel kísérni. hello állapotmodell biztosít egy pontos *részletes* hello állapotának ábrázolását hello hello fürt számos áthelyezése adatot.

![Állapotfigyelő entitásokat.][1]
hello állapotfigyelő entitások, a hierarchia szülő-gyermek kapcsolatba szerint rendezve.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

hello állapotfigyelő entitások a következők:

* **Fürt**. A Service Fabric-fürt hello állapotát jelöli. Fürt állapotjelentések száma, amelyek hatással vannak a teljes fürt hello feltételek ismertetik. Ezek a feltételek hello fürt vagy hello fürt több entitás hatással. Hello feltétel alapján hello jelentéskészítői hello probléma tooone vagy több nem kifogástalan gyermek le nem szűkíthető. Például hello agy toonetwork particionálás vagy kommunikációs hiba miatt a felosztás hello fürt.
* **Csomópont**. A Service Fabric-csomópont hello állapotát jelöli. Csomópont állapotjelentések feltételek, amelyek hatással vannak a hello csomópont funkcióit ismerteti. Azok a általában hatással az összes telepített hello entitás fut rajta. Példák a csomópont nincs elég szabad lemezterület (vagy egyéb gépre kiterjedő tulajdonságai, például a memória, a kapcsolatok), és ha egy csomópont le. hello csomópont entitás azonosítására hello csomópont neve (karakterlánc).
* **Alkalmazás**. Egy hello fürtben futó alkalmazás példányon hello állapotát jelöli. Alkalmazás rendszerállapot-jelentéseket írják le a feltételeket, amelyek hatással vannak a hello hello alkalmazás általános állapotát. Ezeket nem kell leszűkítheti tooindividual gyermekek (szolgáltatások vagy telepített alkalmazások). Hello végpontok közötti interakció különböző szolgáltatások között például hello alkalmazásban. hello alkalmazás entitás azonosítására hello alkalmazásnév (URI).
* **Szolgáltatás**. Hello fürtben futó szolgáltatás hello állapotát jelöli. Szolgáltatás rendszerállapot-jelentéseket írják le a feltételeket, amelyek hatással vannak a hello hello szolgáltatás általános állapotát. hello jelentéskészítői hello probléma tooan sérült partíció vagy a replika nem szűkíthető. A szolgáltatáskonfiguráció (például a port vagy a külső fájlmegosztás) minden olyan partíciónak kapcsolatos problémát okozó például. hello szolgáltatás entitás azonosítására hello szolgáltatás neve (URI).
* **Partíció**. Hello Állapotfigyelő szolgáltatás partíció jelöli. Partíció állapotjelentések száma, amelyek hatással vannak a hello teljes replikakészlethez feltételek ismertetik. Például ha replikák hello száma kevesebb célként, és a partíció kvórumveszteségben van. hello partíció entitás azonosítására hello partíció Azonosítót (GUID).
* **A replika**. Az állapotalapú szolgáltatás replika vagy állapotmentes szolgáltatáspéldány hello állapotát jelöli. hello replikája hello legkisebb egység watchdogs és rendszerösszetevők jelentést tudjon készíteni az alkalmazáshoz. Állapotalapú szolgáltatások például egy elsődleges másodpéldány, amelyek műveletek toosecondaries és lassú replikációs nem replikálódnak. Állapot nélküli példány is, ha nincs elegendő szabad erőforrások vagy kapcsolódási problémák léptek fel is tud jelentéseket. hello replika entitás által azonosított hello Azonosítót (GUID) és hello replika- vagy Partícióazonosító (hosszú).
* **DeployedApplication**. Jelöli hello állapotát egy *csomóponton futó alkalmazás*. Telepített alkalmazás állapotjelentések feltételek adott toohello alkalmazás leírása nem lehet tooservice csomagok hello telepített leszűkítheti hello csomóponton ugyanahhoz a csomóponthoz. Ilyen például a hibák, amikor alkalmazáscsomag nem lehet letölteni a csomóponton és alkalmazás rendszerbiztonsági hello csomóponton beállításával kapcsolatos problémák. hello telepített alkalmazás meghatározva alkalmazásnév (URI) és a csomópont neve (karakterlánc).
* **DeployedServicePackage**. A szolgáltatáscsomagot a hello fürt egy csomópontján fut hello állapotát jelöli. Ismerteti feltételek adott tooa szolgáltatáscsomagot, amelyek nem befolyásolják a hello más szolgáltatás-csomagokat a hello ugyanahhoz a csomóponthoz tartozó hello ugyanahhoz az alkalmazáshoz. A kódcsomag például hello service-csomag, amely nem indítható el, és a konfigurációs csomag nem olvasható. telepített hello szolgáltatáscsomag azonosíthatók alkalmazásnév (URI), a csomópont neve (karakterlánc), a jegyzékfájl neve (karakterlánc) és a szolgáltatás csomag aktiválási azonosító (karakterlánc).

hello állapotmodell hello granularitása segítségével könnyen toodetect és a megfelelő problémákat. Például, ha a szolgáltatás nem válaszol, a rendszer valósítható meg, amely alkalmazáspéldány hello tooreport állapota nem megfelelő. Jelentéskészítés, hogy szintje nem ideális, azonban mert hello probléma előfordulhat, hogy nem kell érintő összes hello szolgáltatást, hogy az alkalmazáson belül. Ha további információt toothat partíció hello jelentés alkalmazott toohello sérült szolgáltatás vagy tooa adott gyermekpartíciót, kell lennie. hello adatok automatikusan felületeken keresztül hello hierarchia, és nem kifogástalan partíción láthatóvá válnak szolgáltatások és alkalmazások szinten. Ez az összesítés toopinpoint segítségével, és oldja meg gyorsabban hello hello probléma okozza.

hello állapotfigyelő hierarchia szülő-gyermek kapcsolatba áll. Egy fürt csomópontja és az alkalmazások tevődik össze. Alkalmazások vannak olyan szolgáltatások és alkalmazások telepítését. A telepített alkalmazások telepített szervizcsomagok. Szolgáltatások partíciókkal rendelkezik, és mindegyik partíció rendelkezik egy vagy több replikákat. Különleges csomópontok és a telepített entitások közötti kapcsolat áll fenn. Egy nem megfelelő állapotú csomóponti alapján a szolgáltató rendszerösszetevő hello Feladatátvevőfürt-kezelő szolgáltatás, hatással van hello központilag telepített alkalmazások, szervizcsomagok és replikák telepítve rajta.

hello állapotfigyelő hierarchia alapuló hello legújabb állapotjelentések száma, vagyis csaknem valós idejű információkat hello rendszer hello legfrissebb állapotát jeleníti meg.
Belső és külső watchdogs jelentést tudjon készíteni hello azonos entitások alkalmazás-specifikus logika és egyéni figyelt feltételek alapján. Felhasználói jelentések együttes létezése hello rendszer jelentéseket.

Tervezze meg, hogyan tooreport és válaszoljon toohealth hello során nagy felhőalapú szolgáltatások Tervező tooinvest. A kezdeti befektetési teszi hello szolgáltatás könnyebb toodebug, figyeléséhez, és működnek.

## <a name="health-states"></a>Állapotok
A Service Fabric három állapotfigyelő állapotok toodescribe használja, hogy egy entitás kifogástalan-e: OK, figyelmeztetés és hiba. Bármely jelentés küldött toohello a health Store adatbázisban kell megadnia, ezek egyikének. hello állapotfigyelő kiértékelésének eredménye az egyik ezeket az állapotokat.

lehetséges hello [állapotokat](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate) vannak:

* **OK**. hello entitás állapota kifogástalan. Nincsenek küldött, vagy saját gyermekéhez. (ha alkalmazható) ismert problémák.
* **Figyelmeztetés**. hello entitásnak van néhány problémát, de továbbra is működhet megfelelően. Például késések fordulnak elő, de azok nem indítják el működési probléma merül fel még. Bizonyos esetekben hello figyelmeztetési feltételének megoldhatja maga külső beavatkozás nélkül. Ezekben az esetekben állapotjelentések felhívják a figyelmet, és adja meg, mi láthatósága van folyamatban. Más esetekben hello figyelmeztetési feltételének ronthatja egy súlyos hiba, felhasználói beavatkozás nélkül.
* **Hiba**. hello entitás állapota nem megfelelő. A művelet hello entitás állapotának toofix hello kell venni, ugyanis nem tud megfelelően működni.
* **Ismeretlen**. hello entitás hello a health Store adatbázisban nem létezik. Ez az eredmény hello elosztott lekérdezések egyesítése több összetevőből származó eredmények lehet lekérni. Például hello get csomópont listájának lekérdezéséhez túl kerül**FailoverManager**, **ClusterManager**, és **HealthManager**; alkalmazás listájának lekérdezéséhez túl kerül **ClusterManager** és **HealthManager**. A lekérdezések eredményeit a rendszer több összetevő egyesíteni. Ha egy másik rendszerösszetevő adja vissza, amely nem található a health Store adatbázisban entitás, hello egyesített eredmény tartalmaz ismeretlen állapot. Egy entitás oka nem a tárolóban állapotjelentések még nem dolgozott, vagy hello entitás törölve lett törlése után.

## <a name="health-policies"></a>Házirendek
entitás állapota kifogástalan alapján a kapcsolódó jelentések és a gyermekek hello a health Store adatbázisban érvényes állapotfigyelő házirendek toodetermine.

> [!NOTE]
> A fürtjegyzékben hello (a fürt és a csomópont állapotának kiértékelését) vagy (az alkalmazás kiértékelése és bármely gyermeke) hello alkalmazásjegyzékben állapotházirendeket adható meg. Rendszerállapot-értékelés kérelmek az egyéni értékelési állapotházirendeket, amelyek csak a értékelés is átadhatja.
> 
> 

Alapértelmezés szerint a Service Fabric hello szülő-gyermek hierarchikus kapcsolat (mindent lehet kifogástalan) szigorú szabályok vonatkoznak. Ha valamely hello gyermekek egy sérült állapotot jelző esemény, hello szülő nem megfelelő állapotúnak számít.

### <a name="cluster-health-policy"></a>Fürt állapotházirend
Hello [állapotházirend fürt](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy) van használt tooevaluate hello fürt állapotfigyelő állapotot és a csomópont állapotokat. hello házirend hello fürtjegyzékben definiálhatók. Ha nincs jelen, hello alapértelmezett házirend (nulla megengedett hibák) használja.
hello fürt állapotházirend tartalmazza:

* [A ConsiderWarningAsError](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.considerwarningaserror). Meghatározza, hogy tootreat figyelmeztetés állapotjelentések hibaként állapot kiértékelésekor. Alapértelmezett: false.
* [MaxPercentUnhealthyApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications). Hello maximális megengedett százalékos értékét határozza meg, amely a nem megfelelő lehet, mielőtt hello fürt tekinthető hiba történt az alkalmazások.
* [MaxPercentUnhealthyNodes](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes). Hello maximális megengedett százalékos értékét határozza meg, amely a nem megfelelő lehet, mielőtt hello fürt tekinthető hiba a csomópontok. Nagy fürtökhöz, néhány csomópontok mindig lefelé vagy ki a javításához, ezért ezen százalékos arány kell konfigurált tootolerate, amely.
* [Applicationtypehealthpolicymap paraméter hiányzó értékei](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap). hello alkalmazás típusát állapotfigyelő házirend-hozzárendelés során fürt állapotának kiértékelési toodescribe különleges típusok használhatók. Alapértelmezés szerint minden alkalmazás kerüljenek a készlet és a MaxPercentUnhealthyApplications értékeli ki. Néhány alkalmazástípus eltérően kell kezelni, ha ezek le lehessen állítani kívül hello globális készlet. Ehelyett kiértékelésük hello százalékos aránya az alkalmazástípus neve hello leképezés társított ellen. Például a fürt számos különböző típusú alkalmazások ezer, és néhány speciális alkalmazás típusú vezérlő az alkalmazáspéldányok. hello vezérlő alkalmazások soha nem lehet a hibás. Globális MaxPercentUnhealthyApplications too20 % tootolerate néhány hiba is megadhat, de a hello alkalmazástípus "ControlApplicationType" be hello MaxPercentUnhealthyApplications too0. Így, ha néhány hello számos alkalmazás sérült állapotban, de hello globális sérült százalékos arány alatt, hello fürt lenne tooWarning értékeli ki. Figyelmeztetési állapot nem befolyásolja a fürt frissítése, és más figyelési aktivált hiba állapota. De még egy vezérlőt alkalmazás hibás tenné a fürt nem kifogástalan, amely visszaállítása elindítja vagy leállítja a hello Fürtfrissítés hello frissítési konfigurációjától függően.
  Hello alkalmazás hello térkép definiált típusok minden alkalmazáspéldányok kiveszik alkalmazások hello globális készlet. Kiértékelésük hello hello alkalmazás típusú kérelmek teljes száma alapján, a hello adott MaxPercentUnhealthyApplications leképezése hello segítségével. Minden hello rest hello alkalmazások hello globális készlet maradnak, és a MaxPercentUnhealthyApplications kiértékelése.

a következő példa hello egy fürtjegyzékben fájlból. hello alkalmazás toodefine bejegyzést írja be a térkép, előtag hello paraméter neve az "ApplicationTypeMaxPercentUnhealthyApplications-", hello alkalmazástípus neve követ.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Az alkalmazás állapotházirendje
Hello [az alkalmazás állapotházirendje](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy) ismerteti, hogyan az alkalmazások és azok alárendelt események és a gyermek-állapotok összesítési hello kiértékelése történik. Hello alkalmazásjegyzékben, definiálható **ApplicationManifest.xml**, hello alkalmazáscsomagban. Ha nincsenek házirendek meg van adva, a Service Fabric feltételezi, hogy a hello entitás sérült állapotba, ha egy jelentés vagy egy alárendelt rendelkezik: hello figyelmeztetés vagy hibaüzenet állapota.
hello konfigurálható házirendek a következők:

* [A ConsiderWarningAsError](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.considerwarningaserror.aspx). Meghatározza, hogy tootreat figyelmeztetés állapotjelentések hibaként állapot kiértékelésekor. Alapértelmezett: false.
* [MaxPercentUnhealthyDeployedApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications). Hello maximális megengedhető százalékos értékét határozza meg, hogy a nem megfelelő lehet, mielőtt hello alkalmazás tekinthető hiba a központilag telepített alkalmazások. Ez a százalékérték keresztül hello hello alkalmazások jelenleg telepítve vannak a hello fürtben lévő csomópontok száma nem megfelelő a telepített alkalmazások hello számát elosztjuk. hello számítási tootolerate felfelé kerekít egy kis mennyiségű csomópontok meghibásodása. Alapértelmezett százalékos aránya: nulla.
* [DefaultServiceTypeHealthPolicy](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy). Megadja a hello alapértelmezett service type állapotházirend, amely hello alapértelmezett állapotházirend hello alkalmazásban minden szolgáltatás esetében.
* [Servicetypehealthpolicymap paraméterek hiányzó értékei](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap). Szolgáltatás állapotházirendeket szolgáltatás típusonkénti térképére biztosít. Ezek a házirendek hello alapértelmezett szolgáltatás típus állapotházirendeket az egyes megadott helyett. Például ha egy alkalmazás egy állapot nélküli átjáró szolgáltatás típusa, egy állapot-nyilvántartó motor szolgáltatás, beállíthatja hello állapotát a tesztelési másképp. Szolgáltatástípus száma szabályzat megadása esetén is révén a több tanúsítványhasználat pontos szabályzása hello hello szolgáltatásának állapotát.

### <a name="service-type-health-policy"></a>Service type állapotházirend
Hello [service type állapotházirend](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy) határozza meg, hogyan tooevaluate és összevont hello szolgáltatások és szolgáltatások gyermekek hello. hello házirend tartalmazza:

* [MaxPercentUnhealthyPartitionsPerService](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice). Hello maximális megengedhető százalékos értékét határozza meg a nem megfelelő partíciók előtt a szolgáltatás nem megfelelő állapotúnak számít. Alapértelmezett százalékos aránya: nulla.
* [MaxPercentUnhealthyReplicasPerPartition](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition). Hello maximális megengedhető százalékos értékét határozza meg a nem megfelelő replikák előtt a partíció nem megfelelő állapotúnak számít. Alapértelmezett százalékos aránya: nulla.
* [MaxPercentUnhealthyServices](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices). Hello maximális megengedhető százalékos értékét határozza meg a nem megfelelő szolgáltatások előtt hello alkalmazás nem megfelelő állapotúnak számít. Alapértelmezett százalékos aránya: nulla.

a következő példa hello fájlból alkalmazásjegyzéket:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Állapot kiértékelésekor
Felhasználók és az automatikus szolgáltatások meg tudja határozni, egyetlen entitás állapotát bármikor. tooevaluate egy entitás állapotát, a hello health store összesítések állapotát hello entitás jelentéseket, és kiértékeli minden gyermekét (ha alkalmazható). hello állapotát összesítő algoritmus által használt állapotházirendeket, adja meg, hogyan tooevaluate rendszerállapot-jelentéseket, és hogyan tooaggregate gyermek állapotadatok (ha alkalmazható).

### <a name="health-report-aggregation"></a>Állapotfigyelő jelentés aggregáció
Egy entitás különböző jelentéskészítők (rendszerösszetevők vagy watchdogs) a tulajdonságai eltérők által küldött több állapotjelentések rendelkezhet. hello összesítést használ hello társított állapotházirendeket, különösen hello ConsiderWarningAsError tag alkalmazás vagy a fürt állapotházirend. A ConsiderWarningAsError Megadja, hogyan tooevaluate figyelmeztetéseket.

hello összesített állapotát váltja ki hello *legrosszabb* hello entitás rendszerállapot-jelentéseket. Ha legalább egy hiba állapotjelentése, hello összesített állapota nem megfelelő.

![Állapotfigyelő jelentés összesítési az esetleges hibajelentésben való megjelenítéshez.][2]

A health entitás, amely rendelkezik egy vagy több hiba állapotjelentések hiba történik. hello esetében is igaz lejárt állapotjelentése, függetlenül annak állapotát.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Ha nem a hibajelentések és egy vagy több figyelmeztetés, hello összesített állapotát figyelmeztetés vagy a hiba, attól függően, hogy hello ConsiderWarningAsError házirend jelzőt.

![Állapotfigyelő jelentés összesítési figyelmeztetés jelentés és a ConsiderWarningAsError hamis.][3]

Állapotfigyelő jelentés összesítési figyelmeztetés jelentés és a ConsiderWarningAsError beállítása toofalse (alapértelmezett).

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Gyermek állapotának összesítése
hello entitás összesített állapotát tükrözi hello gyermek állapotokat (ha alkalmazható). hello algoritmus gyermek állapotokat összesítéséhez szükséges tartományt használ hello állapotházirendeket alkalmazható hello entitás típusa alapján.

![Gyermek entitások állapotának összesítése.][4]

Gyermek-összesítési házirendek alapján.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Miután hello a health Store adatbázisban kiértékelése minden hello gyermeknek, összesíti az a sérült gyermek hello konfigurált maximális százalékát alapján állapotokat. A százalékos hello entitás és alárendelt típusától függően hello házirend származik.

* Minden gyermeknek OK állapotok van, hello gyermek összesített állapotát OK gombra.
* Ha gyermeke OK és a figyelmeztetési állapotok hello gyermek összesített állapota figyelmet igényel.
* Ha a hiba állapotnak, amely nem veszik figyelembe hello megengedett sérült gyermekek százalékos gyermek, hello összesített állapota nem megfelelő.
* Ha hiba állapotok tekintetben hello maximális hello gyermekek engedélyezett sérült gyermekek százalékban hello összesített állapota figyelmet igényel.

## <a name="health-reporting"></a>Rendszerállapot-jelentés
A Service Fabric entitások rendszer összetevőit, a rendszer Fabric-alkalmazások és a belső/külső watchdogs jelenthetnek. hello jelentéskészítők ellenőrizze *helyi* meghatározás hello rendszerállapot figyelt hello entitások várják hello feltételek alapján. Bármely globális állapota vagy összesített adatok toolook, nem kell. a keresett hello viselkedését az toohave egyszerű jelentéskészítők, és nem összetett szervezetek toolook számos dolgot tooinfer, milyen információt toosend kell.

toosend egészségügyi adatok toohello a health Store adatbázisban, egy jelentéskészítő tooidentify hatással hello entitás kell, és hozzon létre egy jelentés. toosend hello jelentés használata hello [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) API-jelentés rendszerállapot API-k kikerültek a hello `Partition` vagy `CodePackageActivationContext` objektumok, a PowerShell-parancsmagok vagy a többi.

### <a name="health-reports"></a>Állapotjelentések száma
Hello [állapotjelentések](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthreport) hello entitások hello fürt minden egyes tartalmaz a következő információ hello:

* **SourceId**. Egy karakterlánc, amely egyedileg azonosítja a hello jelentéskészítői hello állapotfigyelő esemény.
* **Entitás azonosítója**. Amikor hello jelentés van érvényben hello entitás azonosítja. Hello alapján során is tapasztalhatunk eltéréseket [entitástípus](service-fabric-health-introduction.md#health-entities-and-hierarchy):
  
  * A fürt. nincs.
  * Csomópont. Csomópont neve (karakterlánc).
  * az alkalmazás. Alkalmazás neve (URI). Hello fürtben telepített hello alkalmazáspéldány hello nevét jelenti.
  * A szolgáltatás. Szolgáltatás neve (URI). Hello fürtben telepített hello szolgáltatáspéldány hello nevét jelenti.
  * A partíció. Partíció azonosítója (GUID). Hello partíció egyedi azonosítóját jelöli.
  * A replika. hello állapotalapú szolgáltatási másodpéldány-azonosító vagy hello állapotmentes szolgáltatások Példányazonosító (INT64).
  * DeployedApplication. Az alkalmazásnév (URI) és a csomópont neve (karakterlánc).
  * DeployedServicePackage. Alkalmazás neve (URI), a csomópont neve (karakterlánc) és a service manifest neve (karakterlánc).
* **Tulajdonság**. A *karakterlánc* (nem fix enumerálási), amely lehetővé teszi hello jelentéskészítői toocategorize hello állapotfigyelő hello entitás egy adott tulajdonságra eseménye. Például A jelentési hello állapotának hello Node01 "Tároló" tulajdonság is tud jelentéseket, és jelentéskészítői B is tud jelentéseket hello állapotának hello Node01 "Kapcsolat" tulajdonság. Hello a health Store adatbázisban ezek a jelentések külön állapotával kapcsolatos események a hello Node01 entitás tekintendők.
* **Leírás**. Karakterlánc, amely lehetővé teszi, hogy a jelentési tooprovide hello állapotesemény részletes. **SourceId**, **tulajdonság**, és **HealthState** teljesen le kell írnia a hello jelentés. hello leírás hello jelentés emberek számára olvasható információt ad. hello szöveg egyszerűbbé teszi a rendszergazdák és felhasználók toounderstand hello állapotjelentést.
* **HealthState**. Egy [számbavételi](service-fabric-health-introduction.md#health-states) , amely leírja, hogy hello jelentés hello állapotát. hello elfogadható érték OK, figyelmeztetés és hiba.
* **A TimeToLive tulajdonság**. Timespan érték, amely azt jelzi, mennyi ideig hello állapotjelentése esetén érvényes. Alapján kialakulhat **RemoveWhenExpired**, ez lehetővé teszi, hogy tudja, hogyan tooevaluate lejárt események hello a health Store adatbázisban. Alapértelmezés szerint hello értéke végtelen, és hello jelentés érvényes tartja.
* **RemoveWhenExpired**. Olyan logikai érték. Ha tootrue beállításához lejárt állapotfigyelő hello jelentés hello a health Store adatbázisban automatikusan törlődik, és hello jelentés nem befolyásolja az entitás állapotának kiértékelését. Használható, ha egy megadott ideig csak egyszer, és hello jelentéskészítői hello jelentés érvényes nem kell tooexplicitly törölnie kell azt. Hogy toodelete jelentések is használják a hello health store (például egy figyelő módosul, és leállítja az előző és tulajdonság jelentéseinek). Egy jelentés ezzel a rövid TimeToLive RemoveWhenExpired tooclear együtt is küldheti a hello health store bármely korábbi állapotát. Ha hello értéke toofalse, hello lejárt jelentés minősül hello állapot kiértékelésekor a hiba. hello hamis értéket jelzi, hogy hello adatforrás jelentést kell rendszeresen ezt a tulajdonságot toohello a health Store adatbázisban. Ha nem, majd kell van hello figyelő kódjával. annak eldöntéséhez, hogy a hibát hello esemény rögzített hello figyelő állapotát.
* **SequenceNumber**. Egy pozitív egész szám, amelyet a toobe egyre növekvő, hello jelentések hello sorrendjét jelöli. Hello tároló toodetect elavult állapotjelentések hálózati késések vagy más olyan problémák miatt késői kapott használják. Egy jelentés elutasítják, ha hello sorszáma nagyobb vagy egyenlő, mint az utoljára alkalmazott toohello szám hello azonos entitás, a forrás és a tulajdonság. Ha nincs megadva, a hello sorszám automatikusan létrejön. Hello sorszáma a szükséges tooput csak állapotváltozáskor jelentésekor. Ebben a helyzetben hello forrás tooremember jelentéseket az elküldött és hello információk helyreállítási feladatátvétel van szüksége.

Négy darabot információk – SourceId, entitás azonosítója, Property és HealthState--szükségesek a minden jelentés. hello SourceId karakterlánc nem engedélyezett hello előtaggal rendelkező toostart "**rendszer.**", amely rendszer jelentések számára van fenntartva. A hello ugyanaz az entitás, csak egy jelentés, az hello azonos forrás- és a tulajdonság. A jelentések hello azonos forrásból, és tulajdonság fedjék át egymást, hello állapotfigyelő ügyféloldalon (ha ezek vannak kötegelni) vagy a hello health store oldalon. hello helyettesítő alapul sorszámok; újabb jelentéskészítés (a magasabb sorszámok) cserélje le a régebbi jelentéseket.

### <a name="health-events"></a>Állapotával kapcsolatos események
Belső, hello a health Store adatbázisban tartja [állapotával kapcsolatos események](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthevent), amely tartalmazza az összes hello információt hello jelentéseket, illetve további metaadatokat a. hello metaadatokban hello hello jelentés toohello állapotfigyelő ügyfél adott és hello idő hello kiszolgálóoldalon módosítva lett. hello állapotával kapcsolatos események által visszaadott [állapotlekérdezések](service-fabric-view-entities-aggregated-health.md#health-queries).

hello hozzáadott metaadatokat tartalmazza:

* **SourceUtcTimestamp**. hello idő hello jelentés toohello állapotfigyelő ügyfél (egyezményes világidő) lett megadva.
* **LastModifiedUtcTimestamp**. hello idő hello jelentés utolsó módosításának hello kiszolgáló oldalán (egyezményes világidő).
* **IsExpired**. A tooindicate jelzőt, hogy hello jelentés lejárt amikor hello lekérdezés által hello a health Store adatbázisban történt meg. Egy esemény is lehet lejárt, csak akkor, ha RemoveWhenExpired értéke "false". Ellenkező esetben hello esemény lekérdezés nem ad vissza, és a rendszer eltávolítja a hello tároló.
* **LastOkTransitionAt**, **LastWarningTransitionAt**, **LastErrorTransitionAt**. hello OK/figyelmeztetés/hiba átmenetek utolsó ideje. Ezeket a mezőket ad hello állapotfigyelő Állapotváltások hello esemény hello előzményeit.

hello állapot átmenet mezők a intelligensebb riasztásokat vagy a "korábbi" állapottal kapcsolatos adatok esemény használhatók. Forgatókönyvek például ezek engedélyezése:

* Riasztás, ha egy tulajdonság már figyelmeztetés/hiba a több, mint X perc. Egy adott időn belül hello feltételét ellenőrzése Ezzel elkerülheti a riasztásokat az ideiglenes feltételek alapján. Például egy riasztás, ha a hello állapota több mint öt perc figyelmeztetést rendelkezik lett lefordítható (HealthState figyelmeztetés, és most - LastWarningTransitionTime == > 5 perc).
* Riasztás, csak a feltételeket, amelyek megváltoztak a hello utolsó X perc. Ha jelentést már volt, hiba, mielőtt hello megadott idő, mert volt korábban már jelezték mellőzhető.
* Egy tulajdonság váltása van figyelmeztetés és hiba, ha határozza meg, mennyi ideig lett sérült (Ez azt jelenti, hogy nem OK). Például egy riasztás, ha a hello tulajdonság még nem működik megfelelően öt percnél hosszabb lefordítható (HealthState! = az OK gombra, és most - LastOkTransitionTime > 5 perc).

## <a name="example-report-and-evaluate-application-health"></a>Példa: Jelentés és a alkalmazás állapotának kiértékelése
hello alábbi példa állapotfigyelő jelentést küld a Powershellen keresztül hello alkalmazástól **fabric: / WordCount** hello forrásból **MyWatchdog**. hello állapotjelentése hello állapotfigyelő tulajdonság hibaállapot, végtelen TimeToLive az "availability" információkat tartalmaz. Ezt követően lekérdezi hello alkalmazás állapotának, mely értéket ad vissza összesített állapotát állapot hibák és hello jelentett állapotával kapcsolatos események hello állapotával kapcsolatos események közül.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Állapotfigyelő model felhasználás
hello állapotmodell lehetővé teszi, hogy felhőalapú szolgáltatásokhoz és hello a Service Fabric platform tooscale alapul szolgáló, mert a figyelés és a rendszerállapot-meghatározás hello különböző figyelő hello fürtön belül vannak osztva.
Más rendszerek rendelkezik egyetlen központi szolgáltatás szintű hello fürt, amely minden hello elemez *potenciálisan* szolgáltatások által kibocsátott hasznos információkat. Ez a megközelítés hátráltatja a méretezhetőséget. Még nem teszi lehetővé a őket toocollect toohelp problémák és a potenciális problémák azonosításához Bezárás toohello okozza, lehetséges, konkrét információk.

hello állapotmodell fokozottan szolgál a figyelése és diagnosztizálása, a fürt és az alkalmazás állapotának kiértékelésekor és a figyelt frissítésekre. Egyéb szolgáltatások használható egészségügyi adatok tooperform automatikus javítása, hozza létre a fürt előzmények, és adja ki a riasztások bizonyos feltételek esetén a.

## <a name="next-steps"></a>Következő lépések
[A Service Fabric rendszerállapot-jelentések megtekintése](service-fabric-view-entities-aggregated-health.md)

[Rendszerállapot-jelentések használata a hibaelhárításhoz](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Hogyan tooreport és ellenőrzés szolgáltatás állapotát](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Adja hozzá az egyéni Service Fabric állapotjelentések száma](service-fabric-report-health.md)

[Figyelése és diagnosztizálása helyileg szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[A Service Fabric-alkalmazás frissítése](service-fabric-application-upgrade.md)

