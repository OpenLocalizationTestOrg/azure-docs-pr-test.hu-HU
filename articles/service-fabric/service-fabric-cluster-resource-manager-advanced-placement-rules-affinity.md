---
title: "aaaService Fabric fürt Resource Manager - kapcsolat |} Microsoft Docs"
description: "A Service Fabric szolgáltatások kapcsolat konfigurálása – áttekintés"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 678073e1-d08d-46c4-a811-826e70aba6c4
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 7dc9b6d9c18d9d615d39cff7de9d7cba1c040474
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Konfigurálásával és a Service Fabric szolgáltatás affinitás használatával
Kapcsolat egy vezérlő főként toohelp könnyű hello átmeneteit felhő- és mikroszolgáltatások world hello nagyobb egységes alkalmazások biztosított. Azt is szolgál az optimalizálás hello teljesítmény szolgáltatások, bár ez hatásai lehetnek.

Tegyük fel, egy nagyobb alkalmazást, vagy olyat, amely nem csupán kialakítása során szem előtt, a háló tooService (vagy bármely elosztott környezetben) mikroszolgáltatások most helyeznek. Ez a átviteli típus általános. Hello teljes app feloldása hello környezetbe, csomagolás és meggyőződött arról, hogy fut-e zökkenőmentesen megkezdéséhez. Majd megkezdése ossza fel, hogy az összes beszélgetés tooeach más különböző kisebb szolgáltatásokat.

Végül azt tapasztalhatja, hogy hello alkalmazás bizonyos hibákat észlelt. hello problémák általában egyikébe ezen kategóriák:

1. Néhány összetevőt X hello egységes alkalmazás összetevő Y olyan nem dokumentált függőségi volt, és az összetevőket külön szolgáltatás csak kapcsolva. Mivel ezek a szolgáltatások hello fürt különböző csomópontokon futnak, fontosságúak megszakadt.
2. Ezek az összetevők kommunikációra keresztül (nevesített csövek helyi |} megosztott memória |} lemezen tárolt fájlok), és valóban toobe képes toowrite megosztott tooa helyi erőforrás a teljesítményre vonatkozó megfontolásból most. Rögzített függőséget is törlődik, talán.
3. Minden megfelelően működik, de azt elemről kiderül, hogy, hogy a két összetevő-e a ténylegesen chatty/teljesítmény-és nagybetűket. Ha azok áthelyezi őket szolgáltatások külön teljes tanked az alkalmazások teljesítményének és késést növelni. Ennek eredményeképpen hello alkalmazás általános nem teljesíti elvárásainak.

Ezekben az esetekben azt nem szeretné toolose a újrabontási munka, és szeretné toogo hátsó toohello monolit. hello utolsó feltétel lehet egy egyszerű optimalizálás kívánatos. Azonban amíg azt is Újratervezés hello összetevők toowork természetes szolgáltatásként (vagy igazolnia megoldható hello teljesítményre vonatkozó elvárásokat bármilyen más módon) programot fogjuk tooneed bizonyos értelemben az helység szerint.

Milyen toodo? Jól próbálkozzon affinitás bekapcsolásával.

## <a name="how-tooconfigure-affinity"></a>Hogyan tooconfigure kapcsolat
tooset fel a kapcsolatot, megadhatja a egy két különböző szolgáltatások közötti kapcsolat. Az eltolásokat tekintheti affinitás "mutató" egy szolgáltatás más, valamint arról, hogy "csak fut, ahol, hogy fut-e a szolgáltatás." Néha igazolnia tekintse meg a tooaffinity (Ha Ön pont hello gyermek hello szülő) szülő-gyermek kapcsolatként. Affinitás biztosítja, hogy hello replikák és a szolgáltatás egy példánya kerülnek, hello ugyanaz, mint egy másik szolgáltatás csomópontok.

```csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

> [!NOTE]
> Egy alárendelt szolgáltatás csak egyetlen kapcsolat vehet részt. Ha hello gyermek affinitás toobe tootwo szülő szolgáltatások egyszerre néhány lehetőség közül választhat:
> - Hello kapcsolatok fordított (parentService1 és hello aktuális gyermek szolgáltatási pont parentService2 rendelkeznek), vagy
> - Megjelölést egy hello szülők hubot konvenció, és a pont, hogy a szolgáltatás összes szolgáltatásokat. 
>
> Elhelyezési viselkedés hello fürt eredő hello azonos kell hello.
>

## <a name="different-affinity-options"></a>Másik kapcsolat beállításai
Kapcsolat több korrelációs rendszer keresztül képviseli, és két különböző módja van. hello affinitás leggyakoribb módja, úgynevezett NonAlignedAffinity. NonAlignedAffinity, a replikák hello vagy hello különböző services példányát kerülnek, hello azonos csomópontok. hello más módja AlignedAffinity. Igazított affinitás akkor hasznos, csak a állapotalapú szolgáltatás. Szolgáltatások igazítva toohave affinitás biztosítja, hogy a szolgáltatások hello elsődleges kerülnek-e a két stateful beállítása hello azonos csomópontok, minden más. Az ezen szolgáltatások toobe helyezve hello azonos is okoz minden párt több másodlagos adatbázist csomópontok. Emellett (noha ritkább) tooconfigure NonAlignedAffinity állapotalapú szolgáltatások. NonAlignedAffinity két állapotalapú szolgáltatások lefut a hello hello különböző replikáinak hello azonos csomópontok, de az elsődleges különböző csomópontokon sikerült befejezéséhez.

<center>
![Kapcsolat módok és azok által okozott hatások][Image1]
</center>

### <a name="best-effort-desired-state"></a>Szükséges elérhető legjobb állapota
Egy kapcsolat a lehető legkedvezőbb módon. Nem biztosít hello közös elhelyezés vagy megbízhatóságát, hogy a futó hello azonos végrehajtható folyamat nem azonos garanciát. egy kapcsolat hello szolgáltatások sikertelen lehet, és egymástól függetlenül helyezhető alapvetően különböző entitások. Egy kapcsolat is érvénytelenítheti, bár ezeket ideiglenes. Például kapacitás korlátozások jelentheti, hogy csak néhány hello szolgáltatásobjektumokat hello kapcsolat egy adott csomópont kiférjen. Ezekben az esetekben annak ellenére, hogy egy kapcsolat van érvényben, akkor nem kényszeríthető esedékes toohello egyéb megkötések. Ha így lehetséges toodo, hello megsértése automatikusan kijavításáig később.

### <a name="chains-vs-stars"></a>Csillag és láncok
Ma hello fürt erőforrás-kezelő nem tud toomodel láncok affinitás kapcsolatok. Ez azt jelenti, hogy egy szolgáltatás, amely egy kapcsolat a gyermek nem lehet szülője egy másik kapcsolat. Ha azt szeretné, toomodel a kapcsolat típusát, hatékonyan van toomodel csillag, nem pedig a lánc azt. a lánc tooa csillag, hello legalsó gyermek toomove helyette toohello szülőjének első gyermekét szülő lenne. Attól függően, hogy a szolgáltatások hello elrendezésének előfordulhat, hogy toodo ezt többször. Ha nincs természetes szülő szolgáltatás, előfordulhat, hogy toocreate tartalmazó helyőrzőjeként szolgál. A követelményeitől függően akkor is érdemes lehet a toolook [alkalmazáscsoportok](service-fabric-cluster-resource-manager-application-groups.md).

<center>
![Láncok vs. Hello affinitás kapcsolatokat a környezetben található csillagokra][Image2]
</center>

Egy másik művelet toonote affinitás kapcsolatok ma, hogy azok irányt. Ez azt jelenti, hogy hello affinitás szabály csak érvénybe lépteti a hello szülő elhelyezett hello gyermek. Ezért nem biztosítja, hogy hello szülő elhelyezve hello gyermek. Szintén fontos, hogy a kapcsolat hello toonote perfect vagy azonnal kényszerítése óta különböző szolgáltatásokhoz különböző életciklusának rendelkező és is sikertelen mozognak. Például tételezzük fel hello szülő hirtelen meghibásodik tooanother csomópont mert lefagyott. hello fürt erőforrás-kezelő és a Feladatátvevőfürt-kezelő leíró hello feladatátvételi először, mivel a szeretné megtartani a hello szolgáltatások, egységes, és elérhető hello prioritás. Hello feladatátvétel után hello kapcsolat megszakad, de hello fürt erőforrás-kezelő úgy értelmezi, minden rendben mindaddig, amíg azt észleli, hogy hello alárendelt helye nem hello szülő. Ezek rendezi az ellenőrzések rendszeres időközönként kerül sor. További információkat a hogyan értékeli ki a fürt erőforrás-kezelő hello megkötések [Ez a cikk](service-fabric-cluster-resource-manager-management-integration.md#constraint-types), és [a](service-fabric-cluster-resource-manager-balancing.md) többet hogyan tooconfigure hello ütemben történik, amelyen ezek a megkötések teljesíthetők a kiszolgálóhoz értékeli ki.   


### <a name="partitioning-support"></a>Particionálás támogatása
hello utolsó lépésként toonotice kapcsolatra vonatkozó, hogy affinitás kapcsolatok nem támogatottak, ahol hello szülő particionálva van. Lehet, hogy végül támogatja a particionált szülő szolgáltatások, de jelenleg nem engedélyezett.

## <a name="next-steps"></a>Következő lépések
- A szolgáltatások konfigurálásáról [további információ a szolgáltatások konfigurálása](service-fabric-cluster-resource-manager-configure-services.md)
- toolimit szolgáltatások tooa kis készletét a gépek vagy összesítése hello betöltése a szolgáltatások, használjon [alkalmazáscsoportok](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png