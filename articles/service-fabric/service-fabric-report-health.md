---
title: "egyéni Service Fabric állapotjelentések aaaAdd |} Microsoft Docs"
description: "Ismerteti, hogyan toosend egyéni rendszerállapot-jelentéseket tooAzure Service Fabric állapotfigyelő entitásokat. Ad javaslatokat megtervezése és minőségi állapotjelentések végrehajtása."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 0a00a7d2-510e-47d0-8aa8-24c851ea847f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: oanapl
ms.openlocfilehash: 12c9f664e2a457b4e1e8f340873ca60ebcefb097
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-custom-service-fabric-health-reports"></a>Egyéni Service Fabric-állapotjelentések hozzáadása
Az Azure Service Fabric vezet be a [állapotmodell](service-fabric-health-introduction.md) tooflag sérült fürt és az alkalmazás feltételeket az adott entitásokra. hello állapotfigyelő modellje **állapotfigyelő jelentéskészítők** (rendszer összetevőit és watchdogs). könnyű és gyors diagnosztikai és javítási hello célja. Szolgáltatás írók kell kapcsolatos állapotfigyelő toothink előzetes megfizetése esetén. Minden olyan esetben, kedvezőtlen hatással lehet az állapotfigyelő kell kiválasztását, különösen akkor, ha segíthet jelző problémák toohello legfelső szintű bezárásához. hello állapottal kapcsolatos adatok is mentheti időt és erőfeszítést Hibakeresés és a vizsgálat. hello hasznosságát különösen törlése után hello szolgáltatás megfelelően működik, és hello felhőben léptékű (magán- vagy Azure).

a Service Fabric-jelentéskészítők nevű figyelő hello azonosított feltételek egyik fontos. Azok a helyi nézet alapján feltételek jelentést. Hello [a health Store adatbázisban](service-fabric-health-introduction.md#health-store) összesíti az összes jelentéskészítők toodetermine által küldött, hogy az entitások globálisan kifogástalan állapotának adatait. hello modell tervezett toobe sokoldalú, rugalmasan és egyszerűen toouse. hello állapotjelentések hello minőségének hello állapot nézet hello fürt hello pontosságát határozza meg. Téves tévesen jeleníti meg a nem megfelelő negatívan befolyásolhatja a frissítések vagy az egészségügyi adatokat használó más szolgáltatásokba. Ezek a szolgáltatások többek között a javítása és a riasztási mechanizmusokat. Néhány gondolat ezért hello érdeklődik feltételeinek rögzítéséhez szükséges tooprovide jelentések lehetséges módja az ajánlott.

Állapotfigyelő jelentések, a watchdogs és a rendszer összetevők toodesign és megvalósítása kell:

* Hello számítógép megfigyelés alatt áll, és a hello hatással lehet a hello fürt vagy az alkalmazás funkcióit és azok iránt érdeklődik hello feltétel meghatározása. Ezen információ alapján, döntse el, a hello jelentés tulajdonság és egészségügyi állapota.
* Határozza meg a hello [entitás](service-fabric-health-introduction.md#health-entities-and-hierarchy) , hogy hello jelentés vonatkozik.
* Határozza meg, ahol hello reporting történik, belül hello szolgáltatást vagy a belső vagy külső figyelő.
* Adja meg a használt forrás tooidentify hello jelentéskészítői.
* Válassza ki a jelentéskészítési stratégiát, vagy rendszeres időközönként, vagy a átmenetek. hello ajánlott módja a rendszeres időközönként, egyszerűbb kód szükséges, és kevesebb a hibalehetőség tooerrors azt.
* Mennyi ideig hello jelentés határozza meg, a nem megfelelő feltételek hello a health Store adatbázisban kell maradnak, és hogyan azt törölni kell. Ezen információk alapján döntse el, hello jelentés toolive és eltávolítása a lejárati viselkedés.

Ahogy azt korábban említettük, reporting teheti meg:

* hello figyeli a rendszer a Service Fabric szolgáltatás replika.
* Belső watchdogs telepített Service Fabric szolgáltatás (például a Service Fabric állapotmentes szolgáltatások, amely figyeli a feltételek és a jelentések problémák). hello watchdogs lehet telepíteni az összes csomópont, vagy a hozzárendelt toohello figyelt szolgáltatás lehet.
* Hello Service Fabric-csomópontokon futó, de belső watchdogs *nem* valósul meg a Service Fabric-szolgáltatás.
* Külső watchdogs a mintavételi hello erőforrás *kívül* hello Service Fabric-fürt (például figyelőszolgáltatás Gomez hasonlóan).

> [!NOTE]
> Hello kezdő verzióról a hello fürt hello rendszerösszetevők által küldött állapotjelentések telepítéskor. További információ: [hibaelhárítási rendszerállapot használatával jelentéseket](service-fabric-understand-and-troubleshoot-with-system-health-reports.md). hello felhasználói jelentések kell küldeni [állapotfigyelő entitások](service-fabric-health-introduction.md#health-entities-and-hierarchy) már létrehozott hello rendszer.
> 
> 

Egyszer hello jelentéskészítési állapotfigyelő Tervező nincs bejelölve, állapotjelentések könnyen küldhetők el. Használhat [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) tooreport állapotát, ha hello fürt nem [biztonságos](service-fabric-cluster-security.md) , vagy ha hello fabric-ügyfélnek rendszergazdai jogosultságokkal rendelkezik. Jelentéskészítési végezhető el hello API által használatával [FabricClient.HealthManager.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth), PowerShell vagy a többi útján. Konfigurációs forgatógombját kötegelt javítja a teljesítményt a jelentésekre.

> [!NOTE]
> A jelentés állapotának szinkron, és azt jelenti, hogy csak hello érvényesítési munkahelyi hello ügyféloldalon. hello tényt, hogy a jelentés hello hello állapotfigyelő ügyfél vagy elfogadják hello `Partition` vagy `CodePackageActivationContext` objektumok nem jelenti azt, hogy az hello áruházbeli alkalmazásának. Küldött aszinkron módon történik, és esetleg kötegelni más jelentésekre. hello kiszolgálón feldolgozása hello továbbra is sikertelen lehet: hello sorszám elavult, mely hello a jelentés alkalmazni kell a hello entitás lett törölve, stb.
> 
> 

## <a name="health-client"></a>Rendszerállapot-ügyfél
hello rendszerállapot-jelentések küldése toohello a health Store adatbázisban egy állapotfigyelő ügyfélen, amely él belül hello fabric-ügyfélnek. a következő beállítások hello hello állapotfigyelő ügyfél konfigurálható:

* **HealthReportSendInterval**: hello idő hello jelentés hello késleltetés az hozzáadott toohello ügyfél és a hello idő toohello a health Store adatbázisban továbbítja azokat. Egyetlen üzenet ahelyett, hogy az egyes jelentések küldő egy üzenetet a jelentések használt toobatch. hello kötegelés javítja a teljesítményt. Alapértelmezett: 30 másodperc.
* **HealthReportRetrySendInterval**: hello időköz, mely hello állapotfigyelő újraküldi halmozott health jelentéseket toohello a health Store adatbázisban. Alapértelmezett: 30 másodperc.
* **HealthOperationTimeout**: hello időkorlát jelentés üzenetet küldött toohello a health Store adatbázisban. Ha az üzenet túllépi az időkorlátot, hello állapotfigyelő ügyfél azt újrapróbálkozik, amíg hello a health Store adatbázisban megerősíti, hogy hello jelentés feldolgozása. Alapértelmezett: két perc.

> [!NOTE]
> Amikor hello jelentések vannak kötegelni, hello fabric-ügyfélnek kell életben tartania az legalább hello HealthReportSendInterval tooensure, amely elküldi őket. Ha üdvözlőüzenetére elvész, vagy hello a health Store adatbázisban nem vihetők át őket tootransient hibák miatt, hello fabric-ügyfélnek kell tartani életben hosszabb toogive azt egy alkalommal tooretry.
> 
> 

hello pufferelés hello ügyfélen hello egyediségi hello jelentések figyelembe vesz igénybe. Például ha egy adott rossz jelentéskészítői az jelentéskészítési 100 jelentések / másodperc hello azonos hello tulajdonságának ugyanaz az entitás, hello jelentések cserélése hello legfrissebb verziója. Legfeljebb egy ilyen jelentés hello ügyfél várólista létezik. Ha kötegelés van konfigurálva, hello számos olyan jelentést küldeni toohello a health Store adatbázisban csak egy küldési időszakonként. Ez a jelentés hello utolsó hozzáadott jelentést, amely tükrözi a legfrissebb állapotba hello hello entitás.
Adja meg a konfigurációs paraméterek amikor `FabricClient` jön létre úgy, hogy [FabricClientSettings](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclientsettings) hello a kívánt állapotát befolyásoló tételek értékeit.

hello alábbi példa létrehoz egy fabric-ügyfélnek, és meghatározza, hogy hello jelentéseket küldhetnek-e, amikor hozzáadja őket. Időtúllépések és követően újra megkísérelhető a hibákról újrapróbálkozások 40 másodpercenként kerül sor.

```csharp
var clientSettings = new FabricClientSettings()
{
    HealthOperationTimeout = TimeSpan.FromSeconds(120),
    HealthReportSendInterval = TimeSpan.FromSeconds(0),
    HealthReportRetrySendInterval = TimeSpan.FromSeconds(40),
};
var fabricClient = new FabricClient(clientSettings);
```

Azt javasoljuk, hogy hello alapértelmezett fabric ügyfélbeállítások, amelyek `HealthReportSendInterval` too30 másodperc. Ez a beállítás biztosítja az optimális teljesítmény esedékes toobatching. A kritikus jelentések, amelyek a lehető leghamarabb el kell küldeni, használjon `HealthReportSendOptions` az Immediate `true` a [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) API. Azonnali jelentések időköz kötegelés hello kihagyása. Ez a jelző körültekintően; használja azt szeretnénk, hogy a kötegelés, amikor csak lehetséges hello állapotfigyelő ügyfél tootake előnyeit. Azonnali küldés akkor hasznos, ha bezárja a hello háló ügyfél (például hello folyamat állapota érvénytelen megállapítása és tooshut tooprevent hatásai le kell). Biztosítja a legjobb küldési halmozott hello jelentések. Egy jelentés azonnali jelzővel felvételekor hello állapotfigyelő ügyfél kötegek hello összegzi a rendszer minden jelentést utolsó küldési óta.

Paramétereket a PowerShell segítségével egy kapcsolat tooa fürt létrehozásakor adható meg. a következő példa hello elindul egy kapcsolat tooa helyi fürt:

```powershell
PS C:\> Connect-ServiceFabricCluster -HealthOperationTimeoutInSec 120 -HealthReportSendIntervalInSec 0 -HealthReportRetrySendIntervalInSec 40
True

ConnectionEndpoint   :
FabricClientSettings : {
                       ClientFriendlyName                   : PowerShell-1944858a-4c6d-465f-89c7-9021c12ac0bb
                       PartitionLocationCacheLimit          : 100000
                       PartitionLocationCacheBucketCount    : 1024
                       ServiceChangePollInterval            : 00:02:00
                       ConnectionInitializationTimeout      : 00:00:02
                       KeepAliveInterval                    : 00:00:20
                       HealthOperationTimeout               : 00:02:00
                       HealthReportSendInterval             : 00:00:00
                       HealthReportRetrySendInterval        : 00:00:40
                       NotificationGatewayConnectionTimeout : 00:00:00
                       NotificationCacheUpdateTimeout       : 00:00:00
                       }
GatewayInformation   : {
                       NodeAddress                          : localhost:19000
                       NodeId                               : 1880ec88a3187766a6da323399721f53
                       NodeInstanceId                       : 130729063464981219
                       NodeName                             : Node.1
                       }
```

Hasonlóképpen tooAPI, jelentések is küld a `-Immediate` küldött azonnal, függetlenül attól, hello toobe kapcsoló `HealthReportSendInterval` érték.

A többi hello jelentések küldése toohello Service Fabric átjárón, amely rendelkezik egy belső fabric-ügyfélnek. Alapértelmezés szerint ez az ügyfél 30 másodpercenként kötegelni konfigurált toosend jelentések. Hello időköze módosítható hello fürt konfigurációs beállítással `HttpGatewayHealthReportSendInterval` a `HttpGateway`. Ahogy azt korábban említettük, a jobb megoldás toosend hello jelentéseket `Immediate` igaz. 

> [!NOTE]
> amely nem engedélyezett szolgáltatások tooensure nem tud jelenteni állapotfigyelő elleni hello entitások hello fürtben, konfigurálja a hello server tooaccept csak a biztonságos ügyfelektől érkező kéréseket. Hello `FabricClient` jelentéskészítési engedélyezve vannak az biztonsági toobe képes toocommunicate hello fürttel (például a Kerberos- vagy Tanúsítványalapú hitelesítés). Tudjon meg többet az [fürt biztonsági](service-fabric-cluster-security.md).
> 
> 

## <a name="report-from-within-low-privilege-services"></a>Jelentés az alacsony jogosultságú szolgáltatások belül
Service Fabric-szolgáltatás nem rendelkezik rendszergazdai hozzáférési toohello fürt, ha az aktuális környezetből hello keresztül entitásokra állapotfigyelő jelentést `Partition` vagy `CodePackageActivationContext`.

* Az állapotmentes szolgáltatásokhoz használja [IStatelessServicePartition.ReportInstanceHealth](https://docs.microsoft.com/dotnet/api/system.fabric.istatelessservicepartition.reportinstancehealth) tooreport hello aktuális szolgáltatáspéldányt a.
* Állapotalapú szolgáltatások esetén használjon [IStatefulServicePartition.ReportReplicaHealth](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition.reportreplicahealth) tooreport aktuális replikán.
* Használjon [IServicePartition.ReportPartitionHealth](https://docs.microsoft.com/dotnet/api/system.fabric.iservicepartition.reportpartitionhealth) tooreport hello aktuális partíció entitás.
* Használjon [CodePackageActivationContext.ReportApplicationHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportapplicationhealth) tooreport aktuális alkalmazásban.
* Használjon [CodePackageActivationContext.ReportDeployedApplicationHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedapplicationhealth) tooreport hello hello az aktuális csomóponton telepített aktuális alkalmazásban.
* Használjon [CodePackageActivationContext.ReportDeployedServicePackageHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedservicepackagehealth) tooreport egy szolgáltatás csomag hello alkalmazás telepített hello az aktuális csomóponton.

> [!NOTE]
> Belsőleg, hello `Partition` és hello `CodePackageActivationContext` állapotát az ügyfél alapértelmezett beállításokkal rendelkezik. Ahogy a hello [állapotfigyelő ügyfél](service-fabric-report-health.md#health-client), jelentések vannak és időzítő küldi. hello objektumok kell tartani életben toohave alkalommal toosend hello jelentést.
> 
> 

Megadhat `HealthReportSendOptions` keresztül jelentések küldéséhez `Partition` és `CodePackageActivationContext` rendszerállapot API-k. Ha kritikus jelentések, amelyek a lehető leghamarabb el kell küldeni, használja `HealthReportSendOptions` az Immediate `true`. Azonnali jelentések hello belső rendszerállapot ügyfél-ek intervallumának kötegelés hello kihagyása. Ahogy korábban említettük, körültekintően; használja ezt a jelzőt azt szeretnénk, hogy a kötegelés, amikor csak lehetséges hello állapotfigyelő ügyfél tootake előnyeit.

## <a name="design-health-reporting"></a>Állapotfigyelő jelentéskészítés tervezése
hello kiváló minőségű jelentések létrehozásának első lépéseként azonosítja hello feltételek, amelyek befolyásolják hello hello szolgáltatásának állapotát. Minden feltétel, amely segíthet a hello szolgáltatást vagy a fürt jelző problémák--és még jobban előtt történik, a probléma – is potenciálisan dolláros egy mentés. hello előnyöket nyújtja kisebb állásidő, kevesebb éjszakai töltött vizsgálja, és problémákat, és magasabb szintű ügyfelek elégedettségének javítása.

Miután hello feltételeket azonosítja, a figyelő írók kell toofigure hello őket a terhelés és hasznosságát közötti elosztása legjobb módja toomonitor ki. Tegyük fel, amelyet összetett számítások bizonyos megosztott fájlokat használó szolgáltatás. Egy figyelő figyelni hello megosztás tooensure elég hely áll rendelkezésre. Azt tudta figyelni az értesítéseket a fájl vagy könyvtár változások. Az sikerült jelentést egy figyelmeztetés, ha egy társaságuk küszöbérték elérésekor, és hibaüzenetet jelenít meg, ha hello megosztás teljes. A figyelmeztetést, a javítási rendszer elindítása volt, törölje a régebbi hello-megosztáson található fájlok. Hiba a javítási rendszer hello szolgáltatás replika tooanother csomópont sikerült áthelyezni. Vegye figyelembe a módját ismerteti a hello feltétel állapotok állapotfigyelő tekintetében: hello hello feltételt tekinthető Kifogástalan (ok), vagy nem megfelelő (figyelmeztetés vagy hibaüzenet) állapotát.

Hello figyelési részletek van beállítva, ha egy figyelő írójának toofigure ki hogyan tooimplement hello figyelő. Hello feltételek hello szolgáltatáson belül lehet határozni, ha a hello figyelő figyelt hello szolgáltatás része lehet. Például hello szolgáltatás kódot ellenőrizze hello megosztás használatát, és jelentést készít a minden alkalommal, amikor egy fájl toowrite megkísérli. hello ezt a módszert előnye, hogy a reporting felettébb egyszerű. Gondot kell fordítani a tooprevent figyelő hibák a hello szolgáltatást érintő.

Figyelt hello szolgáltatáson belül jelentések lehetőség nem mindig érhető el. Egy figyelő hello szolgáltatáson belül nem lehet képes toodetect hello feltételeket. Még nem rendelkezik hello logika és adatok toomake hello meghatározása. hello terhek hello feltételek figyelési magas lehet. hello feltételek is előfordulhat, hogy nem adott tooa szolgáltatást, de ehelyett a szolgáltatások közötti párbeszéd hatással. Másik lehetőség is toohave watchdogs hello fürt különálló folyamatként. hello watchdogs figyelése hello feltételek és a jelentés, az nem befolyásolja a hello fő szolgáltatások bármely olyan módon. Például ezek watchdogs sikerült implementálható hello állapotmentes szolgáltatások telepítve vannak minden csomópont vagy hello ugyanazon alkalmazás ugyanazon csomópontok hello szolgáltatást.

Bizonyos esetekben egy figyelő hello fürtben futó lehetőség nem érhető el vagy. Figyelt hello feltétele hello rendelkezésre állás vagy a hello szolgáltatás funkcióit, a felhasználók látni, esetén ajánlott toohave hello watchdogs hello hello felhasználói ügyfelek azonos helyen található. Nincs, akkor tesztelheti hello műveletek a hello azonos módon felhasználók keresheti őket. Lehet például egy figyelő hello fürtön kívüli él, problémák kérelmek toohello szolgáltatás, amely hello késést és hello eredményének helyességét ellenőrzi. (A Számológép szolgáltatáshoz, például nem 2 + 2 vissza 4 elfogadható időn belül?)

Miután hello figyelő részletek véglegesítése megtörtént, meg kell határoznia egy adatforrás azonosítója, amely egyedileg azonosítja a. Az azonos típusú élő hello több watchdogs hello fürt, ha azok kell különböző entitások jelentést, vagy, ha azok jelentést hello azonos entitás, használjon különböző adatforrás-azonosítója vagy tulajdonság. Ezzel a módszerrel a jelentések egyszerre is használható. hello tulajdonsága hello állapotjelentése figyelendő hello feltétel kell rögzíti. (Például hello fenti hello tulajdonság lehet **ShareSize**.) Ha több jelentések toohello alkalmazza ugyanazt a feltétel, hello tulajdonság néhány dinamikus információkkal rendelkezhetnek, amely lehetővé teszi a jelentések toocoexist kell tartalmaznia. Például, ha több megosztás toobe figyelni kell, hello tulajdonság neve lehet **ShareSize-megosztásnév**.

> [!NOTE]
> Tegye *nem* hello tároló tookeep állapotadatainak használja. Csak a rendszerállapot-kapcsolatos információkkal kell jelentett állapotát, az információ hatások hello állapotfigyelő értékelése entitás. hello a health Store adatbázisban nem egy általános célú tárolóként úgy lett kialakítva. Használja állapota kiértékelési logika tooaggregate minden adat hello állapotfigyelő állapotba kerülnek. Küldő adatokat (például a jelentéskészítési OK állapotot az állapot) független toohealth nem befolyásolja a hello összesített állapotát, de ronthatják hello a health Store adatbázisban hello teljesítményét.
> 
> 

hello következő döntési tényező mely entitás tooreport kapcsolva. Legtöbbször ennek hello hello feltétel egyértelműen idetifies hello entitás. Válassza ki a hello entitás legjobb lehetséges lépésköz. Ha egy feltétel hatással van egy partíció összes replika, hello partíció nem hello szolgáltatást a jelentést. Ha további gondolat van szükség, azonban esetekben van. Ha hello feltétel hatással van egy entitás, például egy replikát, de hello desire toohave hello feltétel meg van jelölve a replika élettartama hello időtartama meghaladja majd hello partíción kell megadni. Ellenkező esetben hello replika törlésekor hello a health Store adatbázisban a szükségtelenné vált a jelentéseket. Figyelő írók hello élettartama hello entitás és hello jelentés kell gondolni. Törölje a jelet kell, amikor egy jelentés tisztítani kell a tárolóból (például ha már nem érvényes egy entitás jelentett hiba).

Egy példa, amely az együtt I leírt hello pontok vizsgáljuk meg. Vegye figyelembe, hogy a Service Fabric-alkalmazás fő állapot-nyilvántartó állandó szolgáltatás és a másodlagos állapotmentes szolgáltatásokhoz (egy másodlagos szolgáltatás típusa az egyes feladatok) összes csomópontjára telepíti állnak. hello fő van parancsok toobe hajtja végre a másodlagos adatbázist tartalmazó feldolgozási várólista. hello másodlagos hello a bejövő kérelmeket, és hátsó nyugtázási jelek küldésére. Egy olyan feltételt, amely ellenőrizhető hello hello feldolgozási várólistájának hossza. Ha hello fő várólista hossza eléri a, a rendszer figyelmeztetést igen. hello figyelmeztetés azt jelzi, hogy hello másodlagos adatbázis nem terhelhető hello. Ha parancsok eldobott hello várólista eléri maximális hossza hello, rendszer hibát jelez, mint hello szolgáltatást nem lehet helyreállítani. hello jelentések lehet hello tulajdonság **QueueStatus**. hello figyelő él belül hello szolgáltatást, majd hello fő elsődleges replikán rendszeresen továbbítja. hello toolive ideje két percet, és rendszeres időközönként 30 másodpercenként továbbítja azokat. Ha elsődleges hello leáll, hello jelentés automatikusan törlődnek áruházból. Ha hello szolgáltatás replika működik-e, de van-e holtpontba vagy egyéb problémák történtek hello jelentés hello a health Store adatbázisban lejár. Ebben az esetben hello entitás: hiba ki lesz értékelve.

Egy másik figyelhető feltétele feladat végrehajtási ideje. hello fő osztja el a feladatok toohello másodlagos hello feladat típusától függően. Attól függően, hogy hello tervezési hello fő sikerült kérdezze le hello másodlagos adatbázist, a feladatok állapotát. Az sikerült is, amíg a másodlagos adatbázisok toosend hátsó nyugtázási jelek azok szabásakor. Hello második esetben gondot kell fordítani a toodetect olyan helyzetekben, ahol a másodlagos adatbázisok die vagy üzenetek elvesznek. Egy beállítás olyan hello fő toosend egy ping kérelem toohello azonos másodlagos, amely küld vissza az állapotát. Ha nincs állapot érkezik, hello master tartja hibát, és reschedules hello feladat. Ez a viselkedés feltételezi, hogy az idempotent hello feladatok.

figyelt hello feltétel fordítható Figyelmeztetés Ha hello feladatot nem végezheti el a bizonyos ideig (**t1**, például 10 perc). Ha hello feladat nem fejeződött be időben (**t2**, például 20 perc), figyelt hello feltétel fordítható – hiba. Ehhez a jelentéshez végezhető többféle módon:

* hello eredeti elsődleges replika jelentések maga rendszeres időközönként. Hello várólista akkor is az összes függő tevékenység egy tulajdonságot. Ha legalább egy feladat eljárás tovább tart, hello állapotjelentést hello tulajdonság **PendingTasks** figyelmeztető vagy hibaüzenet, szükség szerint van. Ha nincs folyamatban lévő tevékenységek, vagy minden feladat végrehajtási, hello jelentés állapota OK gombra. hello feladatokat is állandó. Ha elsődleges hello nem működik, újonnan előléptetett hello elsődleges továbbra is tooreport megfelelően.
* (A felhő hello vagy külső) egy másik figyelő folyamat ellenőrzi hello feladatok (a kívül, alapján szükséges hello feladat eredményét) toosee Ha teljesítése. Ha nem veszik figyelembe hello küszöbértékeket, hogy jelentést küld hello fő szolgáltatáson. A jelentés is lett van küldve, amely tartalmazza az hello feladatazonosítót a feladat, például minden tevékenység **PendingTask + taskId**. Jelentések csak nem kifogástalan állapotok kellene elküldeni. Állítsa idő toolive tooa néhány percet, majd hello jelentések toobe távolítja el, amikor a várósorban tooensure tisztítás megjelölni.
* hello másodlagos, a feladat éppen futó jelentést, ha hosszabb ideig tart, mint a várt toorun azt. Hello tulajdonság példányáról hello szolgáltatást jelenti **PendingTasks**. hello jelentés pinpoints problémákkal rendelkezik hello szolgáltatáspéldány, de nem rögzíti a ahol hello példány elhalálozik hello helyzet. hello jelentések majd megtisztítva. Az sikerült jelentés másodlagos hello szolgáltatást. Ha másodlagos hello hello feladat befejeződött, hello másodlagos példány törlése hello jelentés hello áruházból. hello jelentés nem rögzíti a hello helyzet, ahol nyugtázási üdvözlőüzenetére elvész, és a hello feladat nem fejeződött be a hello fő szempontjából.

A fent leírt hello esetekben hello reporting történik, de alkalmazás állapotának rögzített hello jelentések vannak, állapotának kiértékelésekor.

## <a name="report-periodically-vs-on-transition"></a>Jelentés rendszeres időközönként és átmenet
Hello állapotfigyelő reporting modell használatával watchdogs küldhet jelentések rendszeres időközönként vagy átmenetek. hello ajánlott, hogy a figyelő reporting módja a rendszeres időközönként, mivel hello kód sokkal egyszerűbb, és kevesebb a hibalehetőség tooerrors. hello watchdogs kell törekedni toobe más dolga, mint lehetséges tooavoid hibák helytelen jelentések kiváltani. Helytelen *sérült* jelentések hatással lehet a rendszerállapot-értékelések és forgatókönyvek alapján állapota, beleértve a frissítéseket. Helytelen *kifogástalan* jelentések elrejtése problémák hello fürt, amely nem kívánatos.

A rendszeres jelentési hello figyelő egy időzítővel valósítható meg. Egy időzítő visszahíváskor hello figyelő hello állapota, és küldjön jelentést a hello aktuális állapotán alapuló korlátozásához. Nincs szükség toosee mely jelentés korábban lett elküldve, vagy bármely optimalizálás szempontjából üzenetküldési ellenőrizze van. hello állapotfigyelő ügyfél rendelkezik kötegelés logika toohelp teljesítménnyel. Hello állapotfigyelő ügyfél aktív marad, amíg újbóli belső hello jelentés elfogadja hello a health Store adatbázisban vagy hello figyelő hello az újabb jelentést hoz létre ugyanazon entitás, property és forrás.

A átmenetek jelentéskészítéshez gondos kezelésére vonatkozó állapotát. hello figyelő bizonyos feltételek figyeli, és csak akkor, ha hello feltételek módosítása jelenti. Ez a megközelítés fejjel hello, hogy kevesebb jelentések van szükség. hello hátránya az, hogy hello figyelő hello logikája összetett. hello figyelő kezelnie kell hello feltételek vagy hello jelentéseket, hogy ellenőrzött toodetermine állapotváltozások el. Feladatátvétel esetén ügyelni kell a jelentések hozzá, de nem küldött toohello a health Store adatbázisban. hello sorszám egyre növekvő kell lennie. Ha nem, hello jelentések szerint elavult utasítja el. Hello bizonyos ritkán előforduló esetekben ahol adatvesztés van szükség a szinkronizálási hello jelentéskészítői hello állapotának és hello hello a health Store adatbázisban levő között lehet szükség.

Jelentés készítését átmenetek szabálykészletében keresztül, a reporting Services `Partition` vagy `CodePackageActivationContext`. Ha a helyi objektum hello (replika vagy a telepített szervizcsomag / alkalmazást telepített) van eltávolítva, a jelentések is törlődnek. Az automatikus tisztítás visszaállítja jelentéskészítői és a health Store adatbázisban közötti szinkronizálás hello szükségességét. Ha hello jelentés szülő alkalmazás vagy a szülőpartíció számára, ügyelni kell a feladatátvételi tooavoid elavult jelentésekben hello a health Store adatbázisban. Logikai hozzá kell adni toomaintain hello megfelelő állapotban, és törölje a jelet hello jelentés, ha már nincs szükség a.

## <a name="implement-health-reporting"></a>Állapotfigyelő reporting megvalósítása
Miután hello entitás és a jelentés adatainak törlése, rendszerállapot-jelentések küldése végezhető el hello API, a PowerShell vagy a többi.

### <a name="api"></a>API
tooreport hello API keresztül, toocreate állapotfigyelő jelentés adott toohello entitástípus azokat a kívánt tooreport kell. Hello jelentés tooa állapotfigyelő ügyfél biztosítják. Alternatív megoldásként hozzon létre egy állapottal kapcsolatos adatok, és adja át a jelentéskészítési módszerrel toocorrect `Partition` vagy `CodePackageActivationContext` tooreport az aktuális entitásokra.

hello következő példa bemutatja, rendszeres hello fürtön belül a figyelő jelentést. hello figyelő ellenőrzi, hogy egy külső forrásból elérhető csomóponton belül. a szolgáltatás jegyzék hello alkalmazáson belül hello erőforrás van szükség. Hello erőforrás nem érhető el, ha hello hello alkalmazáson belül más szolgáltatások is megfelelő működéséhez. Ezért hello jelentés küldött telepített hello szolgáltatás csomag entitás 30 másodpercenként.

```csharp
private static Uri ApplicationName = new Uri("fabric:/WordCount");
private static string ServiceManifestName = "WordCount.Service";
private static string NodeName = FabricRuntime.GetNodeContext().NodeName;
private static Timer ReportTimer = new Timer(new TimerCallback(SendReport), null, 30 * 1000, 30 * 1000);
private static FabricClient Client = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });

public static void SendReport(object obj)
{
    // Test whether hello resource can be accessed from hello node
    HealthState healthState = this.TestConnectivityToExternalResource();

    // Send report on deployed service package, as hello connectivity is needed by hello specific service manifest
    // and can be different on different nodes
    var deployedServicePackageHealthReport = new DeployedServicePackageHealthReport(
        ApplicationName,
        ServiceManifestName,
        NodeName,
        new HealthInformation("ExternalSourceWatcher", "Connectivity", healthState));

    // TODO: handle exception. Code omitted for snippet brevity.
    // Possible exceptions: FabricException with error codes
    // FabricHealthStaleReport (non-retryable, hello report is already queued on hello health client),
    // FabricHealthMaxReportsReached (retryable; user should retry with exponential delay until hello report is accepted).
    Client.HealthManager.ReportHealth(deployedServicePackageHealthReport);
}
```

### <a name="powershell"></a>PowerShell
A rendszerállapot-jelentések küldése  **küldési-ServiceFabric*EntityType*HealthReport **.

hello következő példa bemutatja, rendszeres jelentés készítését a csomópont CPU-értékek. hello jelentéseket küldjön 30 másodpercenként, és egyszerre pedig két perc toolive rendelkeznek. Járnak, ha az hello jelentéskészítői rendelkezik, így hello csomópont ki lesz értékelve, hiba: problémákkal kapcsolatban. Ha hello CPU küszöbértéket, a hello jelentés rendelkezik figyelmeztetési állapotot. Hello CPU a küszöbérték felett marad, több mint hello beállított idő, amikor azt az elvártnak megfelelően hiba. Ellenkező esetben a hello jelentéskészítői OK állapotot küld.

```powershell
PS C:\> Send-ServiceFabricNodeHealthReport -NodeName Node.1 -HealthState Warning -SourceId PowershellWatcher -HealthProperty CPU -Description "CPU is above 80% threshold" -TimeToLiveSec 120

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='CPU', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 5
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : CPU
                        HealthState           : Warning
                        SequenceNumber        : 130741236814913394
                        SentAt                : 4/21/2015 9:01:21 PM
                        ReceivedAt            : 4/21/2015 9:01:21 PM
                        TTL                   : 00:02:00
                        Description           : CPU is above 80% threshold
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:01:21 PM
```

hello alábbi példa hiányára átmeneti replikán. Először lekérdezi a hello Partícióazonosító, és ezután hello hello szolgáltatás iránt érdeklődik, másodpéldány-azonosító. Ezután elküldi a jelentést a **PowershellWatcher** hello tulajdonság **ResourceDependency**. hello jelentés egyik fontos csak két percig, és el van távolítva hello tároló automatikusan.

```powershell
PS C:\> $partitionId = (Get-ServiceFabricPartition -ServiceName fabric:/WordCount/WordCount.Service).PartitionId

PS C:\> $replicaId = (Get-ServiceFabricReplica -PartitionId $partitionId | where {$_.ReplicaRole -eq "Primary"}).ReplicaId

PS C:\> Send-ServiceFabricReplicaHealthReport -PartitionId $partitionId -ReplicaId $replicaId -HealthState Warning -SourceId PowershellWatcher -HealthProperty ResourceDependency -Description "hello external resource that hello primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes." -TimeToLiveSec 120 -RemoveWhenExpired

PS C:\> Get-ServiceFabricReplicaHealth  -PartitionId $partitionId -ReplicaOrInstanceId $replicaId


PartitionId           : 8f82daff-eb68-4fd9-b631-7a37629e08c0
ReplicaId             : 130740415594605869
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='ResourceDependency', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130740768777734943
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : ResourceDependency
                        HealthState           : Warning
                        SequenceNumber        : 130741243777723555
                        SentAt                : 4/21/2015 9:12:57 PM
                        ReceivedAt            : 4/21/2015 9:12:57 PM
                        TTL                   : 00:02:00
                        Description           : hello external resource that hello primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:12:32 PM
```

### <a name="rest"></a>REST
Nyissa meg a szükséges toohello entitást, és hello törzs hello állapotfigyelő jelentés leírása a POST kérelmek használatával REST állapotjelentések küldése. Lásd például: hogyan REST-toosend [állapotjelentések fürt](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-cluster) vagy [állapotjelentések szolgáltatás](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service). Minden entitások támogatottak.

## <a name="next-steps"></a>Következő lépések
Hello egészségügyi adatok alapján, szolgáltatás írók és a fürt/rendszergazdái tulajdonképpen módon tooconsume hello információkat. Például akkor állíthat be egészségi állapot toocatch súlyos problémák értesítőket ahhoz, azok kiváltó kimaradások esetén. Rendszergazdák is beállíthatnak javítási rendszerek toofix problémákat automatikusan.

[Bevezetés tooService állapotát figyelés](service-fabric-health-introduction.md)

[A Service Fabric rendszerállapot-jelentések megtekintése](service-fabric-view-entities-aggregated-health.md)

[Hogyan tooreport és ellenőrzés szolgáltatás állapotát](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Rendszerállapot-jelentések használata a hibaelhárításhoz](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Figyelése és diagnosztizálása helyileg szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[A Service Fabric-alkalmazás frissítése](service-fabric-application-upgrade.md)

