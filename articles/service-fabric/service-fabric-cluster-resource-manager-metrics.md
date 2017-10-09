---
title: "aaaManage Azure mikroszolgáltatási terhelés mérőszámainkat |} Microsoft Docs"
description: "További információk a hogyan tooconfigure és -felhasználási metrikát a Service Fabric toomanage szolgáltatás hálózatierőforrás-fogyasztás."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 0d622ea6-a7c7-4bef-886b-06e6b85a97fb
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 592dc749ce30683a1e439a702b7d0dc0a638276f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-resource-consumption-and-load-in-service-fabric-with-metrics"></a>Kezelése erőforrás-felhasználás és a metrikák a Service Fabric terheléselosztási
*Metrikák* hello erőforrás, a szolgáltatások ítélt információkat, és amely hello hello fürt csomópontja által biztosított. Egy metrika bármilyen, hogy a rendelés tooimprove vagy a figyelő hello teljesítményét a szolgáltatások toomanage. Például memória-felhasználás tooknow előfordulhat, hogy figyelje, ha a szolgáltatás túlterhelt. Egy másik toofigure ki, hogy hello szolgáltatás tudta áthelyezni máshol esetén a kisebb korlátozottan működik rendelés tooget jobb teljesítmény memória használata.

Többek között a memória, a lemez és a CPU-használati metrikák példák. A metrikák fizikai metrika, erőforrásokat, amelyek megfelelnek a felügyelt toobe igénylő hello csomóponton toophysical erőforrások. Metrikák is lehet (és gyakran) logikai metrikákat. Logikai metrikák többek között a "MyWorkQueueDepth" vagy "MessagesToProcess" vagy "TotalRecords". Logikai metrikák alkalmazás által meghatározott, és toosome fizikai erőforrás-felhasználás közvetve tartozik. Logikai metrikák közös, mivel azok rögzített toomeasure és a szolgáltatás alapú fizikai erőforrások felhasználását. Miért Service Fabric biztosít bizonyos alapértelmezett metrikák méri, és jelentéskészítés a saját fizikai metrikák hello összetettsége is.

## <a name="default-metrics"></a>Alapértelmezett metrikák
Tegyük fel, amelyet az tooget írást, és a szolgáltatás telepítése megkezdődött. Ezen a ponton nem tudható, hogy milyen fizikai vagy logikai erőforrások. Ez ideális! Service Fabric fürt erőforrás-kezelő hello néhány alapértelmezett metrikákat használ, ha nincs más metrikákkal meg van adva. Ezek a következők:

  - PrimaryCount - hello csomóponton elsődleges replikák száma 
  - ReplicaCount - hello csomóponton teljes állapot-nyilvántartó replikák száma
  - Szám - szolgáltatás-objektumok (állapotmentes és állapotalapú) hello csomóponton száma

| Metrika | Állapot nélküli példány betöltése | Állapot-nyilvántartó másodlagos betöltése | Állapot-nyilvántartó elsődleges betöltése |
| --- | --- | --- | --- |
| PrimaryCount |0 |0 |1 |
| ReplicaCount |0 |1 |1 |
| Darabszám |1 |1 |1 |

Alapszintű munkaterhelések hello alapértelmezett metrikák hello fürt munka decent terjesztési adja meg. A következő példa hello nézzük meg, mi történik, ha azt hozzon létre két szolgáltatást, és a hello alapértelmezett metrikák a(z) terheléselosztást. hello első szolgáltatás egy állapotalapú szolgáltatás három partíciókat és a cél replika három méretének beállítása. második hello szolgáltatás egy olyan állapot nélküli szolgáltatás egy partíciót és három példányszám.

Íme tartalmával:

<center>
![Alapértelmezett metrikák fürt elrendezése][Image1]
</center>

Néhány dolgot toonote:
  - Elsődleges replikára változott hello állapotalapú szolgáltatás több csomópontot különböző pontjain.
  - A különböző csomópontokon vannak partícióra hello replikáit
  - elsődleges és másodlagos adatbázis teljes száma hello hello fürt terjesztése
  - minden egyes csomóponton egyenletesen kiosztott hello szolgáltatás objektumok teljes száma

jó!

hello alapértelmezett metrikák nagy egy indításakor működik. Azonban hello alapértelmezett metrikák csak végezze, amennyiben. Például: Mi hello valószínűsége, hogy Ön hello particionálási sémáját nek eredmények a tökéletesen még akkor is terhelése minden partíció? Mi az az hello esélye annak, hogy egy adott szolgáltatáshoz terhelés hello állandó adott idő alatt, vagy akár legkisebb hello azonos között több partíciót most?

Az imént hello alapértelmezett metrikák futtatható. Azonban ez általában azt jelenti, hogy a fürtkihasználtság kevesebb, illetve több egyenetlen, mint szeretné. Ennek az az oka hello alapértelmezett metrikák nem adaptív, és feltételezik, hogy minden megegyezik. Például az elfoglalt elsődleges, a másik nincs mindkét az "1" toohello PrimaryCount metrika függ. Hello legrosszabb esetben csak a hello alapértelmezett mérőszámainkat is eredményezhet teljesítményproblémák eredményezve overscheduled csomópontok. Ha érdekli hello legtöbb kívül a fürt és a teljesítménnyel kapcsolatos problémák elkerülése, akkor toouse egyéni metrikák és a dinamikus terheléselosztó jelentéskészítési.

## <a name="custom-metrics"></a>Egyéni metrikák
Metrikák konfigurálása egy nevű-szolgáltatás-példányonként hello szolgáltatás létrehozásakor.

Bármely metrika rendelkezik néhány tulajdonság, amely azt írják le: a neve, a súly és egy alapértelmezett betöltési.

* Metrika neve: hello hello metrika neve. hello metrika neve hello metrika hello Resource Manager szempontjából hello fürtön belül egyedi azonosítója.
* Súly: Metrika súlyának határozza meg, hogy mennyire fontos ez a metrika relatív toohello más metrikákkal ezt a szolgáltatást.
* Alapértelmezett betöltési: hello alapértelmezett betöltési képviselt másképp attól függően, hogy hello szolgáltatás állapot nélküli és állapotalapú.
  * Az állapotmentes szolgáltatások mindegyik metrikát DefaultLoad nevű egyetlen tulajdonsággal rendelkezik
  * Az állapotalapú szolgáltatások adhat meg:
    * PrimaryDefaultLoad: Ez a mérőszám a szolgáltatás fogyaszt elsődleges hello alapértelmezés szerinti
    * SecondaryDefaultLoad: Ez a mérőszám a szolgáltatás fogyaszt másodlagos hello alapértelmezés szerinti

> [!NOTE]
> Ha szeretné too_also_ használata hello alapértelmezett metrikák egyéni metrikáinak definiálása, szükség van-e too_explicitly_ hozzáadása hello alapértelmezett metrikák biztonsági másolatot, és az őket súlyok és értékek meghatározása. Ennek oka az, meg kell adnia a hello alapértelmezett metrikák és a egyéni metrikákat hello kapcsolatát. Például lehet, hogy az Ön számára legfontosabb ConnectionCount vagy WorkQueueDepth több mint elsődleges elosztása. Alapértelmezett hello súlya hello PrimaryCount metrika az magas, tehát tooreduce azt tooMedium a azok elsőbbséget más metrikák tooensure hozzáadásakor.
>

### <a name="defining-metrics-for-your-service---an-example"></a>A szolgáltatás - példa metrikák meghatározása
Tegyük fel, azt szeretné, hogy a következő konfigurációs hello:

  - A szolgáltatás "ConnectionCount" nevű metrika jelentések
  - Azt is szeretné toouse hello alapértelmezett metrikák 
  - Régebben már kötöttek néhány mérések és tudja, hogy általában egy elsődleges replika az adott szolgáltatás foglalja el "ConnectionCount" a 20 egység
  - Másodlagos adatbázis használja a "ConnectionCount" 5 egység
  - Hogy "ConnectionCount" hello legfontosabb mérőszám szempontjából az adott szolgáltatás hello a teljesítmény kezelése
  - Továbbra is szeretné az elosztott terhelésű elsődleges replikára változott. Terheléselosztás elsődleges replikára változott érdemes általában függetlenül attól, milyen. Ez segít a befolyásolása mellett az elsődleges replikák többsége néhány csomópont vagy a tartalék tartomány hello az adatvesztés elkerülése érdekében. 
  - Ellenkező esetben hello alapértelmezett metrikák rendben.

Íme hello, hogy volna írt kódot toocreate egy szolgáltatást, hogy a mérték konfigurálása:

Kód:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
StatefulServiceLoadMetricDescription connectionMetric = new StatefulServiceLoadMetricDescription();
connectionMetric.Name = "ConnectionCount";
connectionMetric.PrimaryDefaultLoad = 20;
connectionMetric.SecondaryDefaultLoad = 5;
connectionMetric.Weight = ServiceLoadMetricWeight.High;

StatefulServiceLoadMetricDescription primaryCountMetric = new StatefulServiceLoadMetricDescription();
primaryCountMetric.Name = "PrimaryCount";
primaryCountMetric.PrimaryDefaultLoad = 1;
primaryCountMetric.SecondaryDefaultLoad = 0;
primaryCountMetric.Weight = ServiceLoadMetricWeight.Medium;

StatefulServiceLoadMetricDescription replicaCountMetric = new StatefulServiceLoadMetricDescription();
replicaCountMetric.Name = "ReplicaCount";
replicaCountMetric.PrimaryDefaultLoad = 1;
replicaCountMetric.SecondaryDefaultLoad = 1;
replicaCountMetric.Weight = ServiceLoadMetricWeight.Low;

StatefulServiceLoadMetricDescription totalCountMetric = new StatefulServiceLoadMetricDescription();
totalCountMetric.Name = "Count";
totalCountMetric.PrimaryDefaultLoad = 1;
totalCountMetric.SecondaryDefaultLoad = 1;
totalCountMetric.Weight = ServiceLoadMetricWeight.Low;

serviceDescription.Metrics.Add(connectionMetric);
serviceDescription.Metrics.Add(primaryCountMetric);
serviceDescription.Metrics.Add(replicaCountMetric);
serviceDescription.Metrics.Add(totalCountMetric);

await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ConnectionCount,High,20,5”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

> [!NOTE]
> Példák fent hello és hello Ez a dokumentum többi részén leírást kezelése metrikák /-nevű-szolgáltatás alapon. Az is lehetséges toodefine metrikáját hello szolgáltatást a szolgáltatások _típus_ szintjét. Ez a szolgáltatás jegyzékfájlokban megadásával valósítható meg. Több okból nem ajánlott a típus szintű metrikák meghatározása. hello első oka az, hogy a mérték neve gyakran környezetre specifikus. Kivéve, ha egy fixen szerződés helyen, nem lehet arról, hogy egy környezetben hello metrika "Magok" nem "MiliCores" vagy "Magok" mások számára. Ha a metrikákat vannak meghatározva a jegyzékfájlban toocreate új jegyzékfájlokat egy környezetben kell. Ez általában csak a kisebb különbségek, ami toomanagement nehézségek tooa elterjedése a különböző jegyzékfájlokban érdeklődők.  
>
> Metrika terhelések általában a egy nevű-szolgáltatás-példányonként vannak hozzárendelve. Például tételezzük fel hello egy példányának létrehozása a toouse tervez CustomerA azt kiszolgálni csak némileg. Tételezzük is fel számára hoz létre egy másik CustomerB, aki rendelkezik egy nagyobb munkaterhelés. Ebben az esetben valószínűleg kívánt tootweak hello alapértelmezett ezekbe a szolgáltatásokba tölt be. Ha metrikák és keresztül jegyzékfájlokban, és meghatározott terhelések szeretné toosupport ebben a forgatókönyvben, azt másik alkalmazás és a szükséges szolgáltatástípusok minden ügyfél esetében. a szolgáltatás-létrehozás időpontjában hello értékek felülbírálják a hello jegyzékben meghatározott úgy, hogy tooset hello adott Alapértelmezések használata. Azonban történt, amely azt eredményezi, hello értékek hello jegyzékfájlokat toonot egyezést e hello szolgáltatás ténylegesen fut, deklarálva. Ennek eredményeképpen előfordulhat tooconfusion. 
>

Ne feledje: Ha csak toouse hello alapértelmezett metrikák, nem tootouch hello metrikák gyűjtemény minden kell vagy semmit különleges a szolgáltatás létrehozásakor. hello alapértelmezett metrikák beolvasása automatikusan használja, ha nincs más vannak definiálva. 

Most ugorjunk végig részletesebben ezeket a beállításokat, és szolgáltatással kapcsolatban, annak hatását hello viselkedését.

## <a name="load"></a>Betöltés
teljes pont metrikák meghatározásának hello toorepresent van néhány betöltése. *Betöltési* van mennyi egy metrika fel néhány szolgáltatáspéldány vagy egy adott csomópont a replikát. Betöltési szinte minden ponton konfigurálható. Példa:

  - Betöltési szolgáltatás létrehozásakor adható meg. Ez a lehetőség _alapértelmezett betöltési_.
  - hello kapcsolatos metrika-információkat, beleértve az alapértelmezett betölti, az a szolgáltatás frissítése is hello szolgáltatás létrehozása után. Ez a lehetőség _szolgáltatás frissítése_. 
  - egy adott partícióra hello terhelések alaphelyzetbe állítása toohello alapértelmezett értékeket, hogy a szolgáltatás lehet. Ez a lehetőség _partícióterhelés_.
  - Betöltési jelenthetők a egy szolgáltatás objektum alapján dinamikusan futásidőben száma. Ez a lehetőség _terhelés reporting_. 
  
Az összes alábbi stratégiák hello ugyanaz az élettartama szolgáltatás használhatja. 

## <a name="default-load"></a>Alapértelmezett betöltése
*Alapértelmezett betöltési* mennyi hello metrika minden szolgáltatás objektum (állapot nélküli példány vagy állapot-nyilvántartó replika) a szolgáltatás-t használ fel. hello fürt erőforrás-kezelő ennyi amíg nem kap további információkat, például a dinamikus terheléselosztó jelentés hello terhelés hello szolgáltatás objektum használja. Az egyszerűbb szolgáltatások hello alapértelmezett betöltési egy statikus definíciója. hello alapértelmezett betöltési soha nem frissül, és hello élettartama hello szolgáltatás használható. Alapértelmezett betölti működik nagy az egyszerű kapacitástervezési esetek, amikor bizonyos mennyiségű erőforrások dedikált toodifferent munkaterhelés, így nem módosítható.

> [!NOTE]
> A kapacitás felügyeleti és a kapacitások hello csomópontok meghatározása a fürt további információkért lásd: [Ez a cikk](service-fabric-cluster-resource-manager-cluster-description.md#capacity).
> 

hello fürt erőforrás-kezelő lehetővé teszi az állapotalapú szolgáltatások toospecify egy másik alapértelmezett betöltési az elsődleges és másodlagos. Állapotmentes szolgáltatásokhoz csak tooall példányok alkalmazó egy értéket lehet megadni. Állapotalapú szolgáltatások esetén az elsődleges és másodlagos replikák hello alapértelmezett betöltési eltérőek általában óta replikák hajtsa végre az egyes szerepkörökhöz munkahelyi különböző típusú. Például elsődleges általában mind az Olvasás, mind az írás szolgálnak, és többségének hello számítási terhet, miközben a másodlagos adatbázisok azonban nem. Általában egy elsődleges replika hello alapértelmezett betöltési értéke magasabb, mint a másodlagos replikák hello alapértelmezett betöltési. hello valós számok saját mérések függ.

## <a name="dynamic-load"></a>Dinamikus terheléselosztó
Tegyük fel, hogy Ön már futott a szolgáltatás egy ideig. Az egyes figyeléssel, amikor észrevette, hogy:

1. Mint mások egyes partíciók vagy egy megadott szolgáltatás példányának több erőforrást
2. Egyes szolgáltatások időnként eltérő terhelést rendelkeznek.

Nincs nagy mennyiségű dolog, ami ilyen típusú terheléselosztási ingadozását. Például különböző szolgáltatásokat vagy partíciók társított eltérő követelményekkel rendelkező, különböző ügyfelektől. Betöltési is volt módosítani, mert a munkahelyi hello szolgáltatás hello mennyisége hello nap hello folyamán változik. Függetlenül hello OK akkor általában egyetlen szám nem használhatja az alapértelmezett. Ez különösen igaz, ha azt szeretné, tooget hello hello fürtön kívüli legtöbb kihasználtságát. Az alapértelmezett betöltési probléma néhány hello idő válasszon értéket. Helytelen alapértelmezett betölti hello fürt erőforrás-kezelő erőforrások lefoglalása alatt vagy fölött eredményez. Ennek eredményeképpen a csomópontokra, amelyeket felett vagy alatt felhasználtuk annak ellenére, hogy a fürt erőforrás-kezelő hello úgy értelmezi hello fürt kiegyensúlyozott rendelkezik. Alapértelmezett terhelés továbbra is megfelelőek, mivel a kezdeti elhelyezésre néhány információt biztosítanak, de nem valódi munkaterhelések teljes szövegegység fontosságúak. erőforrás-követelmények és erőforrás-kezelő fürt hello módosítása tooaccurately rögzítési lehetővé teszi, hogy minden egyes szolgáltatás objektum tooupdate saját terhelést futásidőben. Ezt nevezik a dinamikus terheléselosztó jelentéskészítési.

A dinamikus terheléselosztási jelentések metrikákat a foglalási/jelentett betöltése lehetővé replikák és példányok tooadjust az élettartamuk során. A szolgáltatás replika vagy a cold és a nem végzett munka volt példány általában küldenek, hogy egy metrika kis mennyiségű használta. Egy foglalt replika- vagy küldenek, hogy használják több.

Betöltés / replika- vagy jelentéskészítő lehetővé teszi a fürt erőforrás-kezelő tooreorganize hello hello szolgáltatás objektumok hello fürtben. Hello szolgáltatások átrendezése segít biztosítjuk, hogy hello erőforrások van szükségük. Foglalt szolgáltatások hatékonyan beolvasása túl "VISSZAIGÉNYLÉSE" erőforrásokat más replikákat vagy a számlálókat, amelyek jelenleg a gyenge vagy kevesebb munka során.

Reliable Services belül hello kód tooreport terhelés dinamikusan néz ki:

Kód:

```csharp
this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("CurrentConnectionCount", 1234), new LoadMetric("metric1", 42) });
```

A szolgáltatás bármely hello metrikák létrehozáskor definiálva hozzá is tud jelentéseket. Ha olyan metrikajelentés, amely nem egy szolgáltatás jelentések terheléselosztási toouse konfigurálva, a Service Fabric, amelyek jelentést készítenek figyelmen kívül hagyja. Ha nincsenek más metrikák jelentett hello azonos idő, amelyek érvényesek a jelentések elfogadottak. Szolgáltatás kódot mérésére és jelentés összes hello metrikák tudja, hogyan és operátorok adhat meg hello konfigurációja toouse toochange hello szolgáltatást kód nélkül. 

### <a name="updating-a-services-metric-configuration"></a>A szolgáltatási metrika-konfigurációjának frissítése
hello szolgáltatással kapcsolatos metrikákat hello listáját, és ezek metrikák hello tulajdonságainak frissíthető dinamikusan élő hello szolgáltatás pedig. Ez lehetővé teszi, hogy kísérletezhet és rugalmasságot. Néhány példa, ha ez akkor hasznos, amelyek:

  - a metrika egy adott szolgáltatáshoz buggy jelentést letiltása
  - a kívánt viselkedés alapján metrikák hello súlyozását újrakonfigurálása
  - csak azután hello kódot már telepítve és érvényesítve, más mechanizmusok használatával, amely lehetővé teszi, hogy egy új metrika
  - hello alapértelmezett betöltési megfigyelt viselkedést és fogyasztás alapján szolgáltatás módosítása

hello fő API-k konfigurációja módosítására van `FabricClient.ServiceManagementClient.UpdateServiceAsync` C# és `Update-ServiceFabricService` a PowerShellben. Bármilyen ezen API-k a megadott információ azonnal lecseréli a meglévő metrika adatainak hello hello szolgáltatást. 

## <a name="mixing-default-load-values-and-dynamic-load-reports"></a>Alapértelmezett betöltése értékeket és a dinamikus terheléselosztó jelentések keverése
Alapértelmezett betöltési és dinamikus terhelések használható hello ugyanazt a szolgáltatást. Egy szolgáltatás használja az alapértelmezett betöltési és a dinamikus terheléselosztó jelentések, amikor alapértelmezett betöltési lesz becsült csak dinamikus jelentéseket jelenik meg. Alapértelmezett betöltési nem megfelelő, mert biztosít hello fürt erőforrás-kezelő valamit a toowork. hello alapértelmezett betöltési lehetővé teszi, hogy hello fürt erőforrás-kezelő tooplace jó helyeken hello szolgáltatás objektumok azok létrehozásakor. Ha nem alapértelmezett betöltési információ áll rendelkezésre, hatékonyan véletlenszerű szolgáltatások elhelyezését. Betöltési jelentések érkezésekor később hello kezdeti véletlenszerű elhelyezési gyakran nem megfelelő, és hello fürt erőforrás-kezelő rendelkezik toomove szolgáltatások.

Most eltarthat korábbi példában, és tekintse meg, mi történik, ha jelenleg az egyes egyéni metrikák és a dinamikus terheléselosztó jelentéskészítési felvenni. Ebben a példában a "MemoryInMb", egy példa a metrika használjuk.

> [!NOTE]
> Memória egyike, amelyek a Service Fabric hello rendszer metrikák [erőforrás szabályozására](service-fabric-resource-governance.md), és a saját kezűleg reporting általában nehéz. Nem ténylegesen várhatóan meg tooreport a memória-felhasználás; Felhasznált memória egy az erőforrás-kezelő fürt hello hello képességekre vonatkozó támogatási toolearning-ként.
>

Most feltételezik, eredetileg létrehozott hello állapotalapú szolgáltatás a hello a következő parancsot:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("MemoryInMb,High,21,11”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

Ne feledje a szintaxisa ("MetricName, MetricWeight, PrimaryDefaultLoad, SecondaryDefaultLoad").

Nézzük meg, milyen egyik lehetséges fürt elrendezés nézhet:

<center>
![Az alapértelmezett és egyéni metrikák fürt kiegyensúlyozott][Image2]
</center>

Bizonyos tényezőket érdemes megjegyezni vannak:

* Másodlagos replikák a partíción belüli egyes rendelkezhet saját betöltése
* A teljes hello metrikák hely kiegyensúlyozott. Memória, a hello maximális hello aránya, és minimális terhelés 1,75 összeget (hello csomópont hello a legtöbb terhelés N3, hello legalább N2 és 28/16 = 1,75 összeget).

Néhány dolgot, hogy továbbra is kell tooexplain:

* Mi határozza meg, hogy 1,75 összeget arányú logikus volt-e vagy sem? Honnan, a fürt erőforrás-kezelő hello a esetén is, hogy elég helyes, vagy ha nincs a további munkahelyi toodo?
* Ha nem terheléselosztás fordulhat elő?
* Mit jelent, hogy memória "Magas" súlyozott-e?

## <a name="metric-weights"></a>Metrika súlyozás
Nyomkövetési hello azonos fontos metrikák különböző szolgáltatásban. Globális nézet fogja hello fürt erőforrás-kezelő tootrack fogyasztás hello fürt, fogyasztás egyensúlyba csomópontok, és győződjön meg arról, hogy a csomópontok nem ismerteti a kapacitás lehetővé teszi. Azonban szolgáltatások lehet különböző nézeteket hello toohello fontosságát azonos metrikát. Is számos metrikák és szolgáltatásokat nagyszámú a fürtben, tökéletesen kiegyensúlyozott megoldások nem létezik az összes metrikát. Hogyan kezelje a fürt erőforrás-kezelő hello ezekben a helyzetekben?

Metrika súlyok hello hogyan toobalance hello fürtön, amikor nincs tökéletes válasz fürt erőforrás-kezelő toodecide engedélyezése. Metrika súlyok is hello fürt erőforrás-kezelő toobalance adott szolgáltatások engedélyezése másképp. Metrikák rendelkezhet négy különböző súly szint: nulla, alacsony, közepes és magas. Az egyik súlya nulla metrika hozzájárul semmi annak eldöntéséhez, hogy e dolgok vagy nem elosztott terhelésű. A betöltési azonban továbbra is közreműködhet toocapacity felügyeleti. Nulla súly metrikák továbbra is hasznosak, és gyakran használják a szolgáltatás-viselkedéshez és a teljesítmény figyelése részeként. [Ez a cikk](service-fabric-diagnostics-event-generation-infra.md) figyelés metrikák hello használata, és a szolgáltatások diagnosztikai nyújt részletesebb információt. 

hello fürt különböző metrika súlya hello valós hatása, a fürt erőforrás-kezelő hello különböző megoldások állít elő. Metrika súlyok mondja el hello fürt erőforrás-kezelő, hogy bizonyos metrikák fontosabbak, mint mások. Nincs tökéletes megoldás hello esetén a fürt erőforrás-kezelő is előnyben részesítik a megoldást, amely hello magasabb súlyozott metrikák jobb elosztása. Ha a szolgáltatás úgy értelmezi, egy adott metrika nem lényeges, csak, akkor imbalanced tapasztalhatja, hogy a metrika használatát. Ez lehetővé teszi egy másik szolgáltatás tooget az egyes mérőszám, amely fontos tooit még akkor is, terjesztési.

Példa néhány terhelés jelentések és a különböző metrika nézzük súlyozza hello fürt különböző azonosítóból eredményez. Ebben a példában látható, hogy hello relatív súly hello mérőszámokat váltás hatására hello fürt erőforrás-kezelő toocreate szolgáltatások különböző szabályok.

<center>
![Metrika súlyának példa és a hatása a terheléselosztási megoldások][Image3]
</center>

Ebben a példában nincsenek négy különböző szolgáltatások, a két különböző metrika, MetricA és MetricB összes jelentéskészítési eltérő értékeket. Egy esetben minden hello szolgáltatás MetricA határozza meg, hello legfontosabb egy (súly = kiváló) és MetricB, mint a lényeges, csak (súly = alacsony). Ennek eredményeképpen látható, hogy hello fürt erőforrás-kezelő hello szolgáltatások helyezi el, hogy MetricA nagyobb mint MetricB kiegyensúlyozott. "Jobban kiegyensúlyozott" azt jelenti, hogy rendelkezik-e alsó MetricA a alacsonyabb, mint MetricB szórás rendelkezik. Hello második esetben azt hello metrika súlyok fordított irányú. Ennek eredményeképpen hello fürt erőforrás-kezelő cseréje szolgáltatások A és B toocome fel, ahol jobban az elosztott terhelésű mint MetricA MetricB hozzárendelését.

> [!NOTE]
> Metrika súlyok határozza meg, hogyan kell terheléselosztást hello fürt erőforrás-kezelő, de nem amikor terheléselosztás történjen. A terheléselosztás további információkért tekintse meg [Ez a cikk](service-fabric-cluster-resource-manager-balancing.md)
>

### <a name="global-metric-weights"></a>Globális metrika súlyozás
Tegyük fel, ServiceA meghatározása szerint nagy súlyozási MetricA, és ServiceB beállítja hello súlya MetricA tooLow vagy nulla. Mi az a hello tényleges súly említi használnak?

Nincsenek minden mérőszám nyomon követett több súlyt. hello első súly hello több hello szolgáltatás létrehozásakor hello metrika definiálva. hello más súly a globális weight kiszámítása automatikusan történik. hello fürt erőforrás-kezelő mindkét e súlyok használja, ha a megoldások pontozási. Fontos, mindkét súlyok figyelembe véve. Ez lehetővé teszi hello fürt erőforrás-kezelő toobalance minden szolgáltatással, függően tooits prioritások saját, és gondoskodjon arról is hello fürthöz, a teljes megfelelően van lefoglalva.

Mi történne hello fürt erőforrás-kezelő nem érdeklik a globális és a helyi egyenleg? Jól könnyen tooconstruct megoldásokat, amely globálisan elosztott terhelésű, de az erőforrások egyensúlyára az egyes szolgáltatások eredményező. A következő példa hello most nézze meg a szolgáltatás csak a hello alapértelmezett metrikák konfigurálva, és mi történik, ha csak globális egyenleg tekinthető. lásd:

<center>
![hello globális csak megoldást hatása][Image4]
</center>

Hello felső példában csak a globális elosztás alapján hello fürt egészére valóban átgondolni. Összes számítógépen legyen ugyanaz az elsődleges számolja, és azonos hello hello teljes replikák száma. Azonban ha megnézi a foglalási hello tényleges hatása nincs így jó: hello adatvesztés minden csomópont befolyásolja egy adott munkaterhelés aránytalanul, mert az elsődleges kijelentkezik vesz igénybe. Például hello első csomópont meghibásodásakor hello három elsődleges hello három különböző partíciók hello kör szolgáltatás az összes lenne elveszett. Ezzel ellentétben az hello háromszög és hatszög szolgáltatások az a replika elveszítik partíciókkal rendelkezik. Ez azt eredményezi, hogy nincs megszűnésének eltérő toorecover hello replika le kellene.

Hello alsó példában hello fürt erőforrás-kezelő hello replikák mindkét hello globális és a szolgáltatás elosztás alapján van osztva. Hello megoldás hello pontszám kiszámítása során legtöbb hello súly toohello globális megoldást, és egy (konfigurálható) rész tooindividual szolgáltatásokat biztosít. Metrika globális elosztás alapján van kiszámítva hello átlagos hello metrika súlyok minden szolgáltatásból. Minden szolgáltatás kiegyensúlyozott függően tooits meghatározott saját metrika súlyt. Ez biztosítja, hogy hello szolgáltatások elosztását belül maguk tootheir saját igényeinek megfelelően. Ennek eredményeképpen ha hello azonos első csomópont meghibásodik hello hiba oszlik el az összes olyan szolgáltatás összes partíció. hello hatás tooeach van hello azonos.

## <a name="next-steps"></a>Következő lépések
- A szolgáltatások konfigurálásáról [szolgáltatások konfigurálásával kapcsolatos tudnivalók](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)
- Töredezettségmentesítés metrikák meghatározása a csomópont helyett ezzel az egyirányú tooconsolidate terhelése. toolearn hogyan tooconfigure töredezettségmentesítés, tekintse meg a túl[Ez a cikk](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
- toofind meg arról, hogyan hello fürt erőforrás-kezelő felügyeli, és elosztja a terhelést hello fürtben, tekintse meg hello a cikk a [terheléselosztás](service-fabric-cluster-resource-manager-balancing.md)
- Indítsa el a hello elejétől és [egy bevezető toohello Service Fabric fürt erőforrás-kezelő beolvasása](service-fabric-cluster-resource-manager-introduction.md)
- A mozgás költsége az egyik módja az, hogy egyes szolgáltatások-e többinél drágább toomove toohello fürt erőforrás-kezelő jelzés. További információ a mozgás költsége toolearn tekintse meg a túl[Ez a cikk](service-fabric-cluster-resource-manager-movement-cost.md)

[Image1]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-cluster-layout-with-default-metrics.png
[Image2]:./media/service-fabric-cluster-resource-manager-metrics/Service-Fabric-Resource-Manager-Dynamic-Load-Reports.png
[Image3]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-metric-weights-impact.png
[Image4]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-global-vs-local-balancing.png
