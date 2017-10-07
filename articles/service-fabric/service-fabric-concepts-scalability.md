---
title: "a Service Fabric szolgáltatások aaaScalability |} Microsoft Docs"
description: "Ismerteti, hogyan tooscale Service Fabric-szolgáltatások"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: ed324f23-242f-47b7-af1a-e55c839e7d5d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 5af06f8f71ad5dee32ba115b922842684867e654
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-in-service-fabric"></a>A Service Fabric skálázás
Az Azure Service Fabric esetén könnyen toobuild méretezhető alkalmazások hello szolgáltatások, partíciók és hello fürtcsomópontokat a replikák kezelése. Számos különféle munkaterheléshez futó hello azonos hardverek lehetővé teszi, hogy maximális erőforrás-használat, de emellett az, hogyan kezeli a tooscale a munkaterhelések rugalmas. 

A Service Fabric skálázás több módon valósítható meg:

1. Létrehozásában és eltávolításában állapotmentes szolgáltatáspéldány skálázása
2. Létrehozásában és eltávolításában új méretezhetővé nevű szolgáltatások
3. Az alkalmazáspéldányok létrehozásában és eltávolításában új méretezhetővé nevű
4. A particionált szolgáltatások méretezése
5. Hozzáadása és eltávolítása a csomópont fürtből hello skálázása 
6. A fürt erőforrás-kezelő metrikák skálázás

## <a name="scaling-by-creating-or-removing-stateless-service-instances"></a>Létrehozásában és eltávolításában állapotmentes szolgáltatáspéldány skálázása
Állapotmentes szolgáltatások egyik legegyszerűbb módja tooscale hello belül a Service Fabric működik. Egy állapotmentes szolgáltatások létrehozásakor kap egy alkalommal toodefine egy `InstanceCount`. `InstanceCount`határozza meg, hány futó példánya a service kód jön létre, amikor hello szolgáltatás elindul. Tegyük fel, például, hogy nincsenek-e 100 csomópontok hello fürt. Tételezzük is fel, hogy a szolgáltatás hozza létre az `InstanceCount` 10. Futásidőben hello kód 10 futó példányokat összes válhat túl elfoglalt (vagy nem sikerült elég foglalt). Egyirányú tooscale munkaterhelés-példányok száma toochange hello. Például az egyes kódrészletek, figyelés, vagy a felügyeleti példányok too50 vagy too5 meglévő számának hello módosításához, attól függően, hogy hello munkaterhelést kell tooscale bejövő vagy kimenő alapján hello betölteni. 

C#:

```csharp
StatelessServiceUpdateDescription updateDescription = new StatelessServiceUpdateDescription(); 
updateDescription.InstanceCount = 50;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

PowerShell:

```posh
Update-ServiceFabricService -Stateless -ServiceName $serviceName -InstanceCount 50
```
### <a name="using-dynamic-instance-count"></a>Dinamikus példányszám használatával
Kifejezetten állapotmentes szolgáltatásokhoz, a Service Fabric automatikus módon toochange hello példányszám kínál. Ez lehetővé teszi a hello szolgáltatás tooscale dinamikusan számával hello elérhető csomópontra. hello módon tooopt be ezt a viselkedést tooset hello példányok száma -1 =. InstanceCount = -1 egy utasítás tooService háló, amely szerint "Az állapotmentes szolgáltatások Futtatás minden csomóponton." Csomópontok száma hello változik, ha a Service Fabric automatikusan megváltozik hello példányok száma toomatch, győződjön meg arról, hogy minden érvényes csomópontján fut-e a hello szolgáltatás. 

C#:

```csharp
StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
//Set other service properties necessary for creation....
serviceDescription.InstanceCount = -1;
await fc.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName -Stateless -PartitionSchemeSingleton -InstanceCount "-1"
```

## <a name="scaling-by-creating-or-removing-new-named-services"></a>Létrehozásában és eltávolításában új méretezhetővé nevű szolgáltatások
A megnevezett példánya egy adott típusú példány szolgáltatás (lásd: [Service Fabric-alkalmazás életciklusa](service-fabric-application-lifecycle.md)) néhány elnevezett alkalmazáspéldány hello fürt belül. 

Új elnevezett szolgáltatáspéldány létrehozott (vagy eltávolítása) válik több vagy kevesebb foglalt szolgáltatásként. Ez lehetővé teszi a kérelmek toobe további szolgáltatáspéldány általában lehetővé téve a terhelést a meglévő szolgáltatások toodecrease elosztva. Szolgáltatások létrehozásakor hello Service Fabric fürt erőforrás-kezelő hello szolgáltatások hello fürt elosztott módon helyezi. hello pontos döntések irányadó hello [metrikák](service-fabric-cluster-resource-manager-metrics.md) hello fürt és más elhelyezési szabály. Szolgáltatások számos különböző módon hozható létre, de hello leggyakoribb vagy felügyeleti műveletek, például valaki hívása [ `New-ServiceFabricService` ](https://docs.microsoft.com/en-us/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps), vagy a kód hívása [ `CreateServiceAsync` ](https://docs.microsoft.com/en-us/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync?view=azure-dotnet). `CreateServiceAsync`akkor is hívható a belül más hello fürtben futó szolgáltatásokat.

Szolgáltatások létrehozása dinamikusan kívüli bármilyen típusú forgatókönyvben is használható, és a megszokott mintát követi. Vegye figyelembe például egy állapotalapú szolgáltatás, amely egy adott munkafolyamat jelöli. Jelölő munka hívásokat fog tooshow toothis szolgáltatás, és ez a szolgáltatás folyamatos tooexecute hello lépéseket toothat munkafolyamat-rekord folyamatban. 

Hogyan lenne az adott szolgáltatáshoz méretezési? hello szolgáltatást is valamilyen, több-bérlős és a hívásokat fogadni és hello számos különböző példányai lépései indítsa azonos munkafolyamat egyszerre. Azonban, hogy tehet hello kód összetettebb, mert már tooworry hello számos különböző példányai ugyanabban a munkafolyamatban, és különböző ügyfelektől származó összes különböző szakaszaiban. Emellett: hello nem oldja meg egyszerre több munkafolyamatok kezelése hello méretezési probléma. Ennek az az oka egy bizonyos ponton a szolgáltatás túl sok erőforrást toofit egy adott számítógépen fog használni. Ebben a mintában hello első helyen nem épített számos szolgáltatás is nehézséget okoz megfelelő toosome rejlő szűk vagy lassulást kódjukat. Ilyen jellegű problémák esetén hello szolgáltatás nem toowork is, amikor azt nyomon követéséhez egyidejű munkafolyamat hello száma nagyobb lekérdezi.  

A megoldás toocreate példánya a szolgáltatás minden különböző hello munkafolyamat-példány tootrack szeretné. Ez egy nagy mintát, és működik-e hello szolgáltatás állapot nélküli és állapotalapú. A minta toowork nincs általában egy másik szolgáltatás, amely a "Munkaterhelés-kezelő szolgáltatás". a szolgáltatás hello feladat tooreceive kérelmek és tooroute van azon kérelmek tooother szolgáltatásokat. hello manager hozható létre dinamikusan hello munkaterhelés szolgáltatás egy példánya hello üzenet fogadásakor, és akkor továbbítja a kérelmek toothose szolgáltatásokra. hello kezelő szolgáltatás is kapnak visszahívások, amikor egy adott munkafolyamat szolgáltatás befejezi a feladatot. Hello manager fogadásakor a visszahívások azt nem adott hello munkafolyamat szolgáltatás példányának törlése, vagy hagyhatja, ha a várt több hívást. 

Az ilyen típusú manager speciális verziói is létrehozhat az általa felügyelt hello services készletek. hello készlet biztosíthatja, hogy amikor új kérelem érkezik nem rendelkezik a hello szolgáltatás toospin toowait. Ehelyett hello manager is csak egy munkafolyamat-szolgáltatás, amely nincs jelenleg foglalt hello készletből válasszon, vagy véletlenszerűen irányításához. Megőrzi az elérhető szolgáltatások készlete teszi kezelési új kérelem gyorsabb, mert valószínűleg kevesebb hello a kérésre egy új szolgáltatás toobe hoz létre a toowait rendelkezik. Új szolgáltatások létrehozásának kész, de nem szabad és azonnali. hello készlet csökkentheti hello időn hello kérelem rendelkezik toowait előtt kiszolgálását. Amikor válaszidők fontos hello gyakran jelennek meg a kezelő és a készlet minta. Üzenetsor-kezelés hello kérelem és hello háttérben létrehozása hello szolgáltatást és _majd_ átadja azt egyben egy népszerű manager mintát, létrehozása és -szolgáltatások néhány nyomon követése hello munka mennyiségét, hogy a szolgáltatás jelenleg alapján törlése van függőben. 

## <a name="scaling-by-creating-or-removing-new-named-application-instances"></a>Az alkalmazáspéldányok létrehozásában és eltávolításában új méretezhetővé nevű
Létrehozása és törlése teljes alkalmazáspéldányok a hasonló toohello mintát létrehozásának és-szolgáltatások törlése. Ebben a mintában az egyes kezelő szolgáltatás, amely lehetővé teszi, hogy lát hello kérésnek hello döntési és hello adatokat fogad az hello hello fürtben található más szolgáltatásokkal. 

Ha egy új példányának létrehozásakor nevű alkalmazás használja az egy már meglévő alkalmazásban egy új elnevezett szolgáltatáspéldány létrehozása helyett? Néhány esetben van:

  * hello új alkalmazás-példány egy ügyfél, amelynek kódot kell toorun néhány egyedi azonosítóval, vagy a biztonsági beállítások alapján.
    * A Service Fabric megadhatja, hogy más kódot csomagok toorun az adott identitást. A rendelés toolaunch hello különböző identitásokat hello aktiválások azonos kódcsomag kell toooccur másik alkalmazási esetekben. Fontolja meg egy meglévő ügyfél üzembe helyezett munkaterhelések esetében eset. Ezek is futhat egy adott identitást, figyelheti és szabályozhatja a hozzáférést tooother erőforrások, például a távoli adatbázisokhoz vagy más rendszerekkel. Ebben az esetben, ha egy új ügyfél jelentkezik be, valószínűleg nem szeretné, hogy tooactivate a hello kódjukat ugyanabban a környezetben (folyamat terület). Sikertelen, amíg ez megnehezíti a szolgáltatást kód tooact belül egy adott identitás hello környezetében. Általában rendelkeznie kell további biztonsági, izolációs és identity management kódot. Különböző nevű szolgáltatás használata helyett példányok belül azonos alkalmazáspéldány hello, és ezért hello azonos folyamat terület, az elnevezett Service Fabric-alkalmazás példány is használható. Így könnyebben toodefine identitások környezeteket.
  * hello új alkalmazáspéldány is szolgál pedig a konfiguráció
    * Alapértelmezés szerint minden szolgáltatáspéldány egy alkalmazáspéldányt belül egy adott szolgáltatáshoz típusú nevű hello fog futni hello adott csomóponton ugyanezt a folyamatot. Ez azt jelenti, hogy közben minden szolgáltatáspéldány másképp is konfigurálhat, ezzel ezért nem bonyolult. Szolgáltatások néhány használják fel a config belül a konfigurációs csomag toolook jogkivonatot kell rendelkeznie. Általában ez a csak hello szolgáltatás nevét. A szolgáltatás megfelelően működik, de azt párok hello konfigurációs toohello nevek hello egyedi névvel ellátott szolgáltatás példányok adott alkalmazáspéldány belül. Ez összezavarhatja és konfigurációs óta rögzített toomanage általában egy tervezési idő összetevő alkalmazás példányt adott értékek. További szolgáltatások mindig létrehozásának azt jelenti, hogy további alkalmazásfrissítések toochange hello információi hello konfigurációs csomagokat vagy toodeploy újakat, hogy az új szolgáltatások hello is kereshet adott adataikat. Általában célszerű könnyebb toocreate egy teljesen új elnevezett alkalmazáspéldányt. Ezután használhatja hello alkalmazás paraméterek tooset bármilyen konfiguráció szükség az hello szolgáltatások. Összes hello szolgáltatást, amely belül létrehozott nevű alkalmazáspéldány így is örökölje a konfigurációs beállításokat. Például ahelyett, hogy egyetlen konfigurációs fájl hello beállításainak és testreszabásainak minden ügyfél, például a titkokat vagy felhasználói erőforrás korlátokat, inkább akkor egy másik alkalmazáspéldány ezekkel a beállításokkal minden ügyfél esetében felülbírálható. 
  * hello új alkalmazás verziófrissítési határként szolgál
    * A Service Fabric belül különböző elnevezett alkalmazáspéldányok frissítés határok szolgál. Egy nevesített alkalmazáspéldány frissítése nem befolyásolja, hogy egy másik nevesített alkalmazáspéldány hello a kód. hello különböző alkalmazások kat, azonos code hello különböző verzióit futtató hello a csomópontok azonos. Ez akkor lehet egy tényező, ha toomake méretezési döntést kell, mert dönthet úgy, hogy hello új kódot kell követnie hello azonos frissíti egy másik szolgáltatás, vagy nem. Tegyük fel, hogy a hívás érkezik hello kezelő szolgáltatás, amely felelős az adott ügyfél munkaterheléseinek skálázás hoz létre, és dinamikusan-szolgáltatások törlése. Ebben az esetben azonban hello tekintendő, amely a társított munkaterhelés egy _új_ ügyfél. A legtöbb ügyfél kedveli különítve egymástól nem csak hello biztonsági okokból és konfigurációs, előzőleg felsorolt, de mert tekintetében hello szoftver- és kiválasztása adott verzióit futtató nagyobb rugalmasságot nyújt, amikor azok lesz frissítve. Is előfordulhat, hogy hozzon létre egy új alkalmazás-példányt, és hozzon létre hello szolgáltatást nem egyszerűen toofurther partíció hello a szolgáltatásokat érintő bármilyen egy frissítést fog mennyisége. Külön alkalmazáspéldányok adja meg a nagyobb részletességgel alkalmazás frissítése során, és is engedélyezheti A / B tesztelés és a kék/zöld központi telepítéseket. 
  * hello meglévő alkalmazáspéldány megtelt.
    * A Service Fabric [alkalmazás kapacitás](service-fabric-cluster-resource-manager-application-groups.md) használható toocontrol hello rendelkezésre álló erőforrások mennyiségét az adott alkalmazás példány fogalom. Dönthet például úgy, hogy egy adott szolgáltatáshoz kell toohave rendelés tooscale létrehozott egy másik példánya. Azonban ez alkalmazáspéldány bizonyos metrika kapacitása kívül esik. Ha az adott ügyfél vagy a munkaterhelés továbbra is adható több erőforrást, majd sikerült kapacitásbővítés hello meglévő az adott alkalmazáshoz vagy hozzon létre egy új alkalmazást. 

## <a name="scaling-at-hello-partition-level"></a>Hello partíció szintű skálázás
A Service Fabric támogatja a particionálást. Particionálás felosztja a szolgáltatás több logikai és fizikai szakaszokra, amelyek egymástól függetlenül működnek. Ez akkor hasznos, állapotalapú szolgáltatással, mert nincs beállítva replikák rendelkezik toohandle összes hello hívások és kezelheti az összes hello állapot egyszerre. Hello [áttekintése particionálás](service-fabric-concepts-partitioning.md) információt nyújt a hello típusú particionálási rendszerek által támogatott. Mindegyik partíció hello replikáinak vannak elosztva a fürt, hogy a szolgáltatás betöltése terjesztésére, és győződjön meg arról, hogy az egész sem hello szolgáltatást, vagy minden olyan partíció rendelkezik-e a hibaérzékeny pontok kialakulását hello csomópontján. 

Fontolja meg a 0 alsó kulcsa, 99 magas kulcs és a partíciók száma a 4 a ranged particionálási sémát használó szolgáltatásokat. Egy három csomópontos fürtben hello szolgáltatás előfordulhat, hogy leírva négy replikával, amely minden csomóponton, ahogy az itt látható hello erőforrások megosztása:

<center>
![A három csomópont partícióelrendezés](./media/service-fabric-concepts-scalability/layout-three-nodes.png)
</center>

Növeli a csomópontok hello számát, a Service Fabric áthelyezi néhány hello meglévő replikák van. Például tételezzük fel hello csomópontok száma növekszik toofour és hello replikák terjeszthető beolvasása. Most hello szolgáltatás most már három replikák minden egyes csomóponton futó, minden egyes tartozó toodifferent partíciókat. Ez lehetővé teszi az erőforrás-kihasználást, mivel hello új csomópont nem cold. Általában akkor is javítja a teljesítményt, ha minden egyes szolgáltatás további erőforrások elérhető tooit.

<center>
![A négy csomópont partícióelrendezés](./media/service-fabric-concepts-scalability/layout-four-nodes.png)
</center>

## <a name="scaling-by-using-hello-service-fabric-cluster-resource-manager-and-metrics"></a>Service Fabric fürt erőforrás-kezelő hello és metrikák használatával skálázás
[Metrikák](service-fabric-cluster-resource-manager-metrics.md) hogyan szolgáltatások express az erőforrás-felhasználás tooService háló vannak. Mérőszámainkat hello fürt erőforrás-kezelő egy lehetőség tooreorganize biztosít, és optimalizálja a hello fürt hello elrendezését. Például lehet bőven hello fürterőforrások, de azok előfordulhat, hogy nem osztható ki vannak jelenleg végez műveletet toohello szolgáltatásokat. A metrikák lehetővé teszi hello fürt erőforrás-kezelő tooreorganize hello fürt tooensure szolgáltatások férhetnek toohello elérhető erőforrások. 


## <a name="scaling-by-adding-and-removing-nodes-from-hello-cluster"></a>Hozzáadása és eltávolítása a csomópont fürtből hello skálázása 
A Service Fabric méretezéshez másik lehetőség is hello fürt toochange hello méretét. Hello fürt hello méretének módosítása azt jelenti, hogy csomópontokat hozzáadni vagy eltávolítani egy vagy több hello csomóponttípusok hello fürtben. Vegyük példaként egy olyan esetben, ahol hello fürtben található csomópontok hello mindegyike gyakran használt adatok. Ez azt jelenti, hogy hello fürt erőforrásait szinte minden használni. Ebben az esetben további csomópontokat toohello hozzáadása akkor a fürt hello legjobb módja tooscale. Hello új csomópontok hello fürt hello csatlakozás után a Service Fabric fürt erőforrás-kezelő helyezi szolgáltatások toothem hello meglévő csomópontok kevesebb teljes terhelése eredményez. Az állapotmentes szolgáltatások példányok száma = -1, további példányok automatikusan jönnek létre a szolgáltatás. Ez lehetővé teszi, hogy néhány hívások toomove hello meglévő csomópontok toohello új csomópontjából. 

Hozzáadása és eltávolítása, csomópontok toohello fürt hello Service Fabric Azure Resource Manager PowerShell modulon keresztül is elvégezhető.

```posh
Add-AzureRmServiceFabricNode -ResourceGroupName $resourceGroupName -Name $clusterResourceName -NodeType $nodeTypeName  -NumberOfNodesToAdd 5 
Remove-AzureRmServiceFabricNode -ResourceGroupName $resourceGroupName -Name $clusterResourceName -NodeType $nodeTypeName -NumberOfNodesToRemove 5
```

## <a name="putting-it-all-together"></a>A teljes kép
Megtudhatja, hogy minden hello ötleteket, azt itt már tárgyalt, és egy példán keresztül kommunikál. Vegye figyelembe a következő szolgáltatás hello: próbált egy szolgáltatás, amely különbséglemezként funkcionál címjegyzék toobuild üzem toonames és kapcsolatfelvételi adatokat is. 

Jobb előre álló, lemezcsoport típusú kérdéseket kapcsolódó tooscale rendelkezik: hány felhasználó van folyamatban toohave? Hány névjegyek fogja tárolni a minden felhasználó? Megpróbálná toofigure ki minden Ha meg vannak állandó hello a szolgáltatás első alkalommal is nehézkes. Tegyük fel, egy adott partícióra Count egy statikus szolgáltatásnak toogo volt lesz. nem megfelelő a partíciók száma, toohave méretezési hibát okozhat később hello kiadási hello következményeit. Hasonlóképpen még akkor is, ha válasszon hello esetleg nincs megfelelő száma hello kapcsolatos összes információ van szüksége. Például akkor is toodecide hello mérete hello fürt szükséges előre hello csomópontok száma és a mérete. Az általában rögzített toopredict erőforrásoknak a számát a szolgáltatása lesz tooconsume az élettartama során. Rögzített tooknow idő hello forgalom mintát, amely hello szolgáltatás ténylegesen látja előre is lehet. Például lehet, hogy a felhasználók hozzáadásához és eltávolításához a thing csak először a hello reggel, vagy lehet, hogy azt van elosztva hello hello nap folyamán. Szükség lehet tooscale és dinamikusan ennek alapján. Lehet, hogy toopredict megtudhatja, és tooneed tooscale fog, de akár valószínűleg fog tooneed tooreact toochanging erőforrás-felhasználás a szolgáltatás. Ez magába foglaló rendelés tooprovide hello fürt hello méretének módosítása több erőforrást, amikor a meglévő erőforrások átrendezése nem elegendőek. 

De miért akkor próbálja meg toopick egyetlen partícióséma ki az összes felhasználó számára? Miért korlátozza a saját kezűleg tooone szolgáltatás és egy statikus fürt? hello valós helyzet általában több dinamikus. 

A skála kiépítéséhez, vegye figyelembe a következő dinamikus mintát hello. Szükség lehet a tooadapt azt tooyour helyzet:

1. Nem próbálja toopick particionálási sémát előre mindenki számára, a "manager"szolgáltatás létrehozása.
2. hello feladat hello kezelő szolgáltatás, ügyféladatok toolook esetén azok Regisztrálás a service. Attól függően, hogy ezt az információt hello kezelő szolgáltatás példányt létrehozni, majd a _tényleges_ ügyfél-tároló szolgáltatás _csak az adott ügyfélhez tartozó_. Ha speciális konfigurációja, elkülönítés vagy frissítések van szükségük, az ügyfél is eldöntheti, toospin alkalmazás példány. 

A dinamikus létrehozása számos előnyt mintát:

  - Nem kívánt tooguess hello megfelelő partíciók száma az összes felhasználó számára szükséges előre, vagy egyetlen szolgáltatás, amely minden a saját végtelenül skálázható felépítéséhez. 
  - Különböző felhasználóknak nem kell azonos partícióazonosító száma, replikaszám, egy elhelyezési korlátozás, metrikákat, alapértelmezett terhelések, szolgáltatásnevek, DNS-beállítások és az egyéb hello más hello szolgáltatás megadott tulajdonságok toohave hello vagy az alkalmazás szintjén. 
  - Ettől kezdve további adatok Szegmentálás. Minden egyes ügyfélnek van saját példányát hello szolgáltatás
    - Minden egyes ügyfélszolgálat másképp konfigurálhatók és több vagy kevesebb erőforrást, partíciók vagy a replikákat a várható mérete alapján szükség szerint több vagy kevesebb kap.
      - Például hogy hello "Gold" réteg kifizette hello ügyfél - azok sikerült további replikák vagy nagyobb partíciók száma és potenciálisan erőforrások dedikált metrikák és az alkalmazás kapacitásának tootheir a szolgáltatásokat.
      - Vagy tegyük fel például, hogy megadott Megadja, hogy azok szükséges kapcsolatokat hello száma volt "Kicsi",-csak néhány partíciók visszajelzést kap, vagy más ügyfelekkel megosztott szolgáltatás készletbe még ütemezésbe helyezheti.
  - Nem futtatja egy csoportját egy szolgáltatáspéldány vagy replikákat az ügyfelek tooshow mentése várakozás közben
  - Ha az ügyfél kikerül, majd eltávolítja a szolgáltatásból adataikat más dolga, mint hogy hello manager törli az adott szolgáltatás vagy alkalmazás, amely az jön létre a.

## <a name="next-steps"></a>Következő lépések
A Service Fabric fogalmakat további információkért tekintse meg a következő cikkek hello:

* [A Service Fabric-szolgáltatások rendelkezésre állása](service-fabric-availability-services.md)
* [A Service Fabric szolgáltatások particionálás](service-fabric-concepts-partitioning.md)
