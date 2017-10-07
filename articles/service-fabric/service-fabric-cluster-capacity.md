---
title: "a Service Fabric-fürt kapacitás aaaPlanning hello |} Microsoft Docs"
description: "A Service Fabric fürt kapacitástervezésének szempontjai. A NodeType tulajdonságok értéke, a tartósság és a megbízhatóság rétegek"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: chackdan
ms.openlocfilehash: 83272ce7fefe698eef755cf66493c2874cc3b120
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>A Service Fabric fürt kapacitástervezésének szempontjai
Minden éles telepítésében kapacitásának megtervezése fontos lépés. Az alábbiakban néhány hello elemeket, hogy rendelkezik-e tooconsider folyamat részeként.

* csomópont hello számának meg kell adnia a fürt igények toostart, ki a
* az egyes csomóponttípus (méret, elsődleges, internetre irányuló, virtuális gépek száma stb.) hello tulajdonságait
* hello megbízhatóság és a tartósság jellemzők hello fürt

Ossza meg velünk rövid időre tekintse át az összes ezeket az elemeket.

## <a name="hello-number-of-node-types-your-cluster-needs-toostart-out-with"></a>csomópont hello számának meg kell adnia a fürt igények toostart, ki a
Először meg kell, hogy milyen hello fürt létrehozásakor használt toobe lesz, és milyen típusú alkalmazásokhoz tervezéséhez toofigure toodeploy a fürtbe. Ha olyan nem egyértelmű hello fürt hello célja, hogy valószínűleg még nem tooenter hello kapacitásának tervezési folyamat kész.

A fürt kell a kimenő toostart csomóponttípusok hello száma létrehozásához.  Az egyes csomóponttípusok csatlakoztatott tooa virtuálisgép-méretezési csoportban. Az egyes csomóponttípusok akár majd is méretezhető vagy rendelkezik egymástól függetlenül, a portok megnyitása más-más részhalmazához le, és különböző teljesítmény-mérőszámait lehet. Hello döntési csomóponttípusok hello számának úgy lényegében előre le toohello a következő szempontokat:

* Az alkalmazás nem rendelkezik több szolgáltatásra, és bármelyiket igényelnek toobe nyilvános vagy az internetre? Tipikus alkalmazások tartalmazzák egy előtér-átjáró szolgáltatás, amely a bemeneti kap egy ügyfél és egy vagy több háttérszolgáltatások kommunikáló hello előtér-szolgáltatások. Így ebben az esetben befejezi a legalább két csomópont típus rendelkezik.
* A szolgáltatások (az alkalmazást alkotó) rendelkeznek a különböző infrastruktúrához például nagyobb RAM vagy nagyobb CPU-ciklusok? Például tételezzük fel, hogy hello alkalmazás, amelyet toodeploy előtér-szolgáltatás és a háttér-szolgáltatás tartalmazza. hello előtér-szolgáltatás is futtathatók kisebb virtuális gépek (VM-méretek D2 beállításaihoz hasonlóan), amely a portokat nyissa meg a toohello internet.  hello háttér-szolgáltatás, azonban számítási intenzív és a vonatkozó igényeket toorun nagyobb virtuális gépek (VM-méretek D4, D6, D15 hasonlóan), amelyek nincsenek internet felé néző.
  
  Ebben a példában bár eldöntheti, hogy tooput összes hello szolgáltatások egy csomóponttípuson, azt javasoljuk, hogy Ön helyezze el őket egy fürtben két csomópont típussal.  Ez lehetővé teszi minden csomópont típus toohave különböző tulajdonságai például internetkapcsolat vagy a Virtuálisgép-méretet. virtuális gépek száma hello egymástól függetlenül, illetve is méretezhető.  
* Hello jövőbeli nem becsülhető, mivel tud tények-nal, és adja meg, amelyeket az alkalmazások toostart a csomóponttípusok hello száma. Mindig hozzáadása vagy eltávolítása a csomóponttípusok később. A Service Fabric-fürt legalább egy csomópont típusúnak kell lennie.

## <a name="hello-properties-of-each-node-type"></a>az egyes csomóponttípusok hello tulajdonságairól
Hello **csomóponttípus** elérhető Felhőszolgáltatások egyenértékű tooroles tekinthető. Csomóponttípusok hello Virtuálisgép-méretek, a virtuális gépek hello számát és azok tulajdonságait határozza meg. Minden csomópont-típus, a Service Fabric-fürt definiált egy külön virtuálisgép-méretezési csoport lett beállítva. Virtuálisgép-méretezési csoportban olyan Azure számítási erőforrás toodeploy használja, és a virtuális gépek készletként gyűjteményeinek kezelését. Folyamatban különálló virtuálisgép-méretezési csoport, az egyes csomóponttípusok akár majd is méretezhető vagy rendelkezik egymástól függetlenül, a portok megnyitása más-más részhalmazához le, és is rendelkezik különböző teljesítmény-mérőszámait.

Olvasási [Ez a dokumentum](service-fabric-cluster-nodetypes.md) hello kapcsolat a NodeType tulajdonságok értéke toovirtual gép méretezési olvashat, hogyan tooRDP oszthatók hello példányok, nyissa meg új portok stb.

A fürt rendelkezhet egynél több csomóponttípus, de hello elsődleges csomóponttípushoz (hello elsőt hello Portal meghatározó) rendelkeznie kell legalább öt virtuális gépeken használatos a termelési számítási feladatokhoz fürtök (vagy a tesztfürtökön legalább három virtuális gépek). A Resource Manager sablonnal hello fürt létrehozásakor, majd keresse meg **elsődleges** attribútum hello definíció csomópont alatt. hello elsődleges csomópont típus: hello csomóponttípus, ahol a Service Fabric rendszerszolgáltatások kerülnek.  

### <a name="primary-node-type"></a>Elsődleges csomóponttípusok
A több csomópont fürt esetén kell egyik toochoose toobe elsődleges. Az alábbiakban egy elsődleges csomóponttípusok hello jellemzői:

* Hello **virtuális gépek minimális mérete** hello elsődleges csomóponttípusok hello határozza meg a **tartóssági szint** választja. hello hello tartóssági szint alapértelmezés szerint bronz. Görgessen le, milyen hello tartóssági szint van, és hello értékeket is igénybe vehet.  
* Hello **virtuális gépek minimális száma** hello elsődleges csomóponttípusok hello határozza meg a **megbízhatósági szint** választja. hello hello megbízhatósági szint alapértelmezés szerint ezüst. Görgessen le, milyen hello megbízhatósági szint van, és hello értékeket is igénybe vehet. 


* hello Service Fabric rendszer szolgáltatások (például a hello kezelő szolgáltatás és a Image Store szolgáltatás) kerülnek, hello elsődleges csomóponttípusok és igen hello megbízhatóság és a tartós hello fürt hello megbízhatósági szint érték és a tartóssági szint határozza meg az elsődleges csomóponttípusok hello választott érték.

![Képernyőfelvétel egy fürt, amely két csomópont tartozik ][SystemServices]

### <a name="non-primary-node-type"></a>A csomóponttípus nem elsődleges
A több csomópont fürt esetén egy elsődleges csomóponttípusok és hello részeinek őket a rendszer nem elsődleges. Az alábbiakban egy nem elsődleges csomóponttípus hello jellemzői:

* hello minimális mérete virtuális gépek a csomóponttípus választja hello tartóssági szint határozza meg. hello hello tartóssági szint alapértelmezés szerint bronz. Görgessen le, milyen hello tartóssági szint van, és hello értékeket is igénybe vehet.  
* hello minimális virtuális gépek száma a csomóponttípus egyike lehet. Azonban ez a szám hello hello/alkalmazásszolgáltatások, hogy szeretné-e a csomópont típusban toorun replikáinak száma alapján kell kiválasztani. hello csomóponttípus a virtuális gépek száma növelhető hello fürt telepítése után.

## <a name="hello-durability-characteristics-of-hello-cluster"></a>hello tartósság jellemzők hello fürt
hello tartóssági szint használt tooindicate toohello rendszer hello jogosultsággal rendelkező hello tartozó Azure infrastruktúra, hogy a virtuális gépekre. Hello elsődleges csomóponttípushoz, a jogosultság lehetővé teszi a Service Fabric toopause minden virtuális gép szintű infrastruktúra kérések (például egy virtuális gép újraindítása, a VM-lemezkép-visszaállítási vagy a virtuális gép áttelepítése) hatással lehetnek a hello kvórum követelményei hello rendszerszolgáltatások és az állapotalapú szolgáltatások. Hello nem elsődleges csomóponttípusok, a jogosultság lehetővé teszi a Service Fabric toopause bármely virtuális gép szintű infrastruktúra kérelmek például a virtuális gép újraindítása, a VM-lemezkép alaphelyzetbe, a virtuális gép áttelepítése stb., hatással lehetnek a hello kvórum követelményei az állapotalapú szolgáltatások azt.

Ez a jogosultság van megadva a következő értékek hello:

* Arany - feladatok szüneteltethetők UD két órát határozatlan hello infrastruktúra. Arany tartóssági engedélyezhető csak a teljes csomópont VM termékváltozatok például D15_V2, G5 stb.
* Ezüst - hello infrastruktúra feladatok szüneteltethetők UD 10 perces időtartamra és elérhető összes szabványos virtuális egymagos és annál.
* Bronz - jogosultság nélküli. Ez az alapértelmezett hello. Csak a tartóssági szint csomópont típusú használ, amelynek futtatása _csak_ állapot nélküli munkaterheléseket. 

> [!WARNING]
> A NodeType tulajdonságok értéke bronz tartós futtató beszerzése _jogosultság nélküli_. Ez azt jelenti, hogy az állapot nélküli munkaterheléseket befolyásoló feladatok infrastruktúra nem fog leállt vagy késleltetett. Akkor lehet, hogy ilyen feladatok továbbra is hatással lehet a munkaterhelés, leállás vagy más olyan problémák. Az éles munkaterhelés rendezést, fut legalább ezüst ajánlott. 
> 

Az egyes a csomóponttípusok toochoose tartóssági szint kap. Dönthet egy típusú csomópont toohave a tartóssági szint arany vagy ezüst és egyéb hello vannak bronz hello ugyanabban a fürtben. **Kezelnie kell minden csomópont-típus, amely rendelkezik egy arany vagy ezüst tartósságát 5 csomópontok minimális száma**. 

**Ezüst vagy arany tartóssági szint használatának előnyei**
 
1. Csökkenti a skálázási művelet szükséges lépések számát hello (Ez azt jelenti, hogy csomópont inaktiválása és eltávolítása-ServiceFabricNodeState neve automatikusan)
2. Hello megváltoztat az adatvesztés tooa felhasználói által kezdeményezett helyben VM SKU módosítási művelet vagy Azure-infrastruktúra műveletek miatt.
     
**Hátrányait ezüst vagy arany tartóssági szint.**
 
1. Virtuálisgép-méretezési csoport központi telepítések tooyour és egyéb kapcsolódó Azure-erőforrások) késleltethető, is időtúllépéssel fejeződött be, vagy teljesen problémák, a fürtben vagy hello infrastruktúra szintjén blokkolhatja. 
2. Növekszik hello száma [replika életciklus-események](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle ) (például elsődleges swap) miatt tooautomated csomópont deactivations Azure-infrastruktúra műveletek során.

### <a name="recommendations-on-when-toouse-silver-or-gold-durability-levels"></a>Ha kapcsolatos javaslatok toouse ezüst vagy arany a tartóssági szint

Használjon ezüst vagy arany tartóssági az összes csomópontot meg kell adnia, hogy a gazdagép állapotfüggő szolgáltatások, várt tooscale (csökkentheti a Virtuálisgép-példányok száma) gyakran, és a telepítési műveletek elhalasztja a skálázási műveletek egyszerűsítése helyett inkább. hello kibővített forgatókönyvek (hozzáadása a virtuális gépek példányok) tölt be a választott hello tartóssági szint, csak méretezési a does.

### <a name="operational-recommendations-for-hello-node-type-that-you-have-set-toosilver-or-gold-durability-level"></a>Hello csomópont működési javaslatok adja meg, hogy beállította toosilver vagy arany tartóssági szint.

1. Tartsa a fürt és az alkalmazások megfelelő időben a tulajdonos, és győződjön meg arról, hogy az alkalmazások válaszol tooall [replika életciklus-események szolgáltatás](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle) (ilyen például a build replika Beragadt) időben.
2. Elfogadja a biztonságosabb módszer toomake a virtuális gép SKU módosítása (méretezési fel/le): virtuális gép egy virtuálisgép-méretezési csoport Termékváltozata elengedhetetlenül hello módosítása egy nem biztonságos művelet, ezért el kell kerülni Ha lehetséges. Itt az hello folyamat tooavoid gyakori problémákat is végrehajthatja.
    - **A nem elsődleges NodeType tulajdonságok értéke:** javasoljuk, hogy hozzon létre új virtuálisgép-méretezési készlet, módosítja hello szolgáltatás elhelyezési korlátozás tooinclude hello új virtuálisgép-méretezési készlet/csomóponttípus, és csökkentse hello régi virtuálisgép-méretezési csoport példányok száma too0, (Ez a toomake meg arról, hogy a hello csomópontok eltávolítása nincs hatással az hello megbízhatóság hello fürt) egyszerre csak egy csomópont.
    - **Az elsődleges nodetype hello:** azt javasoljuk, hogy hello elsődleges csomóponttípusok VM Termékváltozata nem módosítja. Hello okból hello új SKU van kapacitása, ha azt javasoljuk, hogy több példány, vagy ha lehetséges, hozzon létre egy új fürtöt. Ha még nem választott, végezze el a módosításokat toohello virtuális gép méretezési beállítása modell definition tooreflect hello új Termékváltozat. Ha a fürt csak egy nodetype, majd győződjön meg arról, hogy az állapotalapú alkalmazások válaszol-e tooall [replika életciklus-események szolgáltatás](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle) (ilyen például a build replika Beragadt) egy időben, valamint a szolgáltatás a replika újbóli létrehozása időtartam értéke kisebb, mint öt percet (ezüst tartóssági szint). 


> [!WARNING]
> Nincs ajánlott maszkolandó változó hello Virtuálisgép-méretezési készlet nem fut legalább ezüst tartóssági VM Termékváltozat-méretét. Virtuális gép SKU méretének módosítása egy olyan adatok felülíró helyszíni infrastruktúra művelet. Legalább egy lehetőséget toodelay vagy a figyelő nélkül ez a változás is lehetséges, hogy hello művelet állapotalapú szolgáltatások dataloss okozza-e, vagy okoz cselekvései működési problémákkal, még a állapot nélküli munkaterheléseket. 
> 
    
3. Minden virtuális gép méretezési csoportját, amely rendelkezik engedélyezett MR karbantartása öt csomópontok minimális száma
4. Nem véletlenszerű Virtuálisgép-példányok törléséhez mindig használja szolgáltatás vertikális virtuálisgép-méretezési készlet. egy véletlenszerű Virtuálisgép-példányok hello törlése egyensúlyhiány létrehozásának hello Virtuálisgép-példány UD és FD elosztva a hordozza. Ez egyensúlyhiány ronthatja a hello rendszerek képességét tooproperly terheléselosztás hello szolgáltatás példányok/szolgáltatás replikák között.
6. Ha használja az automatikus skálázás, majd hello szabályok beállítása úgy, hogy a skála (Virtuálisgép-példányok eltávolítása) egyszerre csak egy csomópont végzett. Skálázás le több példány egyszerre is nem biztonságos.
7. Ha skálázás le egy elsődleges csomóponttípushoz, meg kell soha nem csökkentheti azt több mint lehetővé teszi, hogy milyen hello megbízhatósági szint.


## <a name="hello-reliability-characteristics-of-hello-cluster"></a>hello fürt hello megbízhatóság jellemzői
hello megbízhatósági szint szolgál, hogy szeretné-e a fürt hello elsődleges csomóponttípuson toorun hello rendszerszolgáltatások replikák tooset hello száma. replikák több hello hello, hello megbízhatóbb hello rendszerszolgáltatások a fürtön.  

hello megbízhatósági szint is igénybe vehet a következő értékek hello:

* Platinum - Futtatás hello rendszerszolgáltatások a cél replika set számával, a 9-es
* Arany - Futtatás hello rendszerszolgáltatások és a cél replika beállítása 7 száma
* Ezüst - Futtatás hello rendszerszolgáltatások 5 cél replika set számaival együtt 
* Bronz - Futtatás hello rendszerszolgáltatások a cél replika set számával, a 3-ból

> [!NOTE]
> hello megbízhatósági szint választja hello rendelkeznie kell az elsődleges csomóponttípushoz csomópontok minimális száma határozza meg. 
> 
> 


### <a name="recommendations-for-hello-reliability-tier"></a>Hello megbízhatósági szint javaslatok.

 Ha növeli vagy csökkenti a fürt (Virtuálisgép-példány összes csomóponttípusok hello összege) hello méretét, frissítenie kell a egyrétegű tooanother fürtöt hello megbízhatóságát. Ezzel elindítja a hello frissítések szükséges toochange hello rendszer szolgáltatások replika set darabszám. Várjon, amíg folyamatban toocomplete hello frissítés bármely egyéb módosítások toohello fürt, például a csomópontok hozzáadása előtt.  Kísérheti hello hello frissítés Service Fabric Explorer vagy való futtatásával [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

Itt található hello ajánlást hello megbízhatósági szint kiválasztásával.

| **Fürt mérete** | **Megbízhatósági szint** |
| --- | --- |
| 1 |Ne adjon meg hello megbízhatósági szint paraméter, hello rendszer kiszámítja az |
| 3 |Bronz |
| 5 vagy 6|Ezüst |
| 7 vagy 8 |Arany |
| 9 vagy afölötti |Platinum |




## <a name="primary-node-type---capacity-guidance"></a>Típusú elsődleges csomópont - kapacitás útmutató

Íme hello útmutatást hello elsődleges csomópont típus kapacitásának tervezése

1. **Száma a Virtuálisgép-példányok toorun bármelyik üzemi feladat az Azure-ban:** meg kell adnia az elsődleges csomópont típus méretének legalább 5. 
2. **Száma a Virtuálisgép-példányok toorun tesztelési célú alkalmazásokat az Azure-ban** megadhat egy minimális elsődleges csomópont méretének 1-es vagy 3. hello több csomópontot tartalmazó fürtben, egy speciális konfigurációja, és így fut, méretezési kívül, hogy a fürt nem támogatott. hello több csomópontot tartalmazó fürtben, vannak nincs megbízhatóság és így a Resource Manager sablon, el kell tooremove/nem adja meg, hogy a konfigurálás (hello konfigurációs érték nem beállítása nincs elegendő). Ha beállította a portálon beállítása hello egy csomópontos fürtre, majd hello konfigurációs rendszer automatikusan hozott kezeli. 1 – 3 csomópontot tartalmazó fürt nem támogatottak a termelési számítási feladatokhoz futtatásához. 
3. **Virtuális gép Termékváltozat:** elsődleges csomóponttípusok az hello rendszerszolgáltatások hol fut, így a hello VM SKU választja, a teljes csúcsidőre fiók hello veszi figyelembe a töltődhet, tervezze meg tooplace hello fürtbe. Egy analógia tooillustrate mi I ide - jelenti azt gondolja, hogy hello elsődleges csomóponttípushoz, a "tüdő", mint azt oxigén tooyour agy tartalma ide, és így hello agy elég oxigén nem kérdezhető le, ha a szervezet romlik. 

Mivel hello kapacitási igényeihez a fürt munkaterhelés határozza meg azt tervezi, hogy a fürt hello toorun, jelenleg nem képes az adott munkaterhelés minőségi útmutatást, azonban itt hello széles körű útmutatót toohelp megkezdése

A termelési számítási feladatokhoz 


- hello ajánlott VM termékváltozata Standard D3, a Standard D3_V2 vagy a megfelelő, egy legalább 14 GB helyi SSD-Meghajtóra.
- hello minimális támogatott VM SKU használata Standard D1, a Standard D1_V2 vagy a megfelelő, egy legalább 14 GB helyi SSD-Meghajtóra. 
- Részleges core például normál A0 méretű SKU nem támogatottak a termelési számítási feladatokhoz.
- Standard A1 Termékváltozat nem támogatott a teljesítményre vonatkozó megfontolásból a termelési számítási feladatokhoz.


## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>A nem elsődleges csomóponttípus - kapacitás útmutató állapotalapú alkalmazások és szolgáltatások számára

Ez az útmutató az állapotalapú alkalmazások és szolgáltatások használatával a Service fabric rendszer [megbízható gyűjtemények vagy reliable Actors](service-fabric-choose-framework.md) hello nem elsődleges csomóponttípushoz futtatja.


**Virtuálisgép-példányok száma:** a termelési számítási feladatokhoz, amelyek állapotalapú, ajánlott a minimális és a cél replika számával, 5 futtassa. Ez azt jelenti, hogy stabil állapotban, végül replikát (a replikakészlethez) minden tartalék tartomány és a frissítési tartomány. hello teljes megbízhatósági szint koncepció hello elsődleges csomóponttípusok van egy módon toospecify rendszerszolgáltatások ezt a beállítást. Ezért hello azonos szempont tooyour állapotalapú szolgáltatások is vonatkozik.

Így a termelési számítási feladatokhoz, hello minimális ajánlott nem elsődleges csomópontot típus mérete 5, ha állapotalapú alkalmazások és szolgáltatások futtatja azt.


**Virtuális gép Termékváltozat:** Ez az hello csomóponttípus ahol az alkalmazás szolgáltatások futnak, így VM SKU úgy dönt, kell figyelembe fiók hello csúcsterhelés meg hello tooplace tervezze meg az egyes csomópontok. hello hello nodetype a lemezkapacitási igényekről, határozza meg munkaterhelés tervezi toorun hello fürt, ezért jelenleg nem képes az adott munkaterhelés, azonban ez hello széles körű útmutatót toohelp első lépések útmutató a minőségi

A termelési számítási feladatokhoz 

- hello ajánlott VM termékváltozata Standard D3, a Standard D3_V2 vagy a megfelelő, egy legalább 14 GB helyi SSD-Meghajtóra.
- hello minimális támogatott VM SKU használata Standard D1, a Standard D1_V2 vagy a megfelelő, egy legalább 14 GB helyi SSD-Meghajtóra. 
- Részleges core például normál A0 méretű SKU nem támogatottak a termelési számítási feladatokhoz.
- Standard A1 Termékváltozat termelési számítási feladatokhoz a teljesítményre vonatkozó megfontolásból kifejezetten nem támogatott.


## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>A nem elsődleges csomóponttípus - állapot nélküli munkaterheléseket kapacitás útmutatást

Ez az útmutató az állapot nélküli Munkaterheléseket, amelyek hello nem elsődleges nodetype futtatja.

**Virtuálisgép-példányok száma:** a termelési számítási feladatokhoz, amelyek állapotmentes, hello minimális támogatott nem elsődleges csomópontot típus mérete 2. Ez lehetővé teszi, hogy toorun, két állapotmentes példányát az alkalmazást, és így a szolgáltatás toosurvive hello egy Virtuálisgép-példány megszűnését. 

> [!NOTE]
> Ha a service fabric verziónál régebbi 5.6 fut a fürtön, hello tooa hiba miatt futásidejű (Ez a probléma fennáll 5.6), 5, mint egy nem elsődleges csomópontot típus tooless le skálázás bekapcsolása sérült állapotba, amíg meghívja a fürt állapota okoz [ Remove-ServiceFabricNodeState cmd](https://docs.microsoft.com/powershell/servicefabric/vlatest/Remove-ServiceFabricNodeState) hello megfelelő csomópont névvel. Olvasási [hajtsa végre a Service Fabric-fürt bejövő vagy kimenő](service-fabric-cluster-scale-up-down.md) további részletekért
> 
>

**Virtuális gép Termékváltozat:** Ez az hello csomóponttípus ahol az alkalmazás szolgáltatások futnak, így VM SKU úgy dönt, kell figyelembe fiók hello csúcsterhelés meg hello tooplace tervezze meg az egyes csomópontok. hello hello nodetype a lemezkapacitási igényekről, határozza meg munkaterhelés tervezi toorun hello fürt, ezért jelenleg nem képes az adott munkaterhelés, azonban ez hello széles körű útmutatót toohelp első lépések útmutató a minőségi

A termelési számítási feladatokhoz 


- hello ajánlott VM termékváltozata Standard D3 vagy Standard D3_V2 vagy ezzel egyenértékű csoport. 
- hello minimális támogatott VM SKU használata Standard D1 vagy Standard D1_V2 vagy ezzel egyenértékű csoport. 
- Részleges core például normál A0 méretű SKU nem támogatottak a termelési számítási feladatokhoz.
- Standard A1 Termékváltozat nem támogatott a teljesítményre vonatkozó megfontolásból a termelési számítási feladatokhoz.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="next-steps"></a>Következő lépések
Befejezés a kapacitástervezés és a fürt beállítása után olvassa el a következő hello:

* [A Service Fabric-fürt biztonsági](service-fabric-cluster-security.md)
* [A Service Fabric rendszerállapot-modell bemutatása](service-fabric-health-introduction.md)
* [A NodeType tulajdonságok értéke tooVirtual gép skála a kapcsolat beállítása](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
