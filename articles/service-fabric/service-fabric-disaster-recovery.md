---
title: "a Service Fabric vész-helyreállítási aaaAzure |} Microsoft Docs"
description: "Az Azure Service Fabric hello képességek szükséges toodeal katasztrófák minden típusú kínál. Ez a cikk ismerteti a hello típusú katasztrófák, amik akkor léphetnek fel, és hogyan toodeal velük."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 04b8348fb63e8a1c76a8f722c4c8255b339908e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="disaster-recovery-in-azure-service-fabric"></a>Az Azure Service Fabric katasztrófa utáni helyreállítás
A magas rendelkezésre állású kézbesítéséhez kritikus része annak ellenőrzése, hogy a szolgáltatások hibatűrését összes különböző típusú hibák. Ez különösen fontos a nem tervezett hibák és a vezérlőn kívül. Ez a cikk ismerteti az egyes közös függően katasztrófák lehet, ha nem modellezve, és a megfelelő felügyelt. Is tárgyalja azok mérséklési és műveletek tootake, ha egy olyan vészhelyzet esetén mégis történt. hello cél toolimit vagy leállás vagy adatvesztés kockázatát hello kiküszöbölheti a hibák, a tervezett fordulhat elő, vagy ellenkező esetben fordulhat elő, amikor.

## <a name="avoiding-disaster"></a>Vészhelyreállítás elkerülése
A Service Fabric elsődleges célja, mind a környezettől, és a szolgáltatások úgy, hogy általános hiba típusok nincsenek katasztrófák modell toohelp. 

Általános vész és sikertelen forgatókönyvek két típusa van:

1. Hardver- vagy hibák
2. Működési hibák

### <a name="hardware-and-software-faults"></a>Hardver- és hibák
Hardver- és hibák előre nem látható. hello legegyszerűbb módja toosurvive hibák hardver- vagy tartalék határain túlnyúló ölel hello szolgáltatás több példánya fut. Például ha a szolgáltatás csak egy adott számítógép fut, majd hello, hogy egy géphez hibája, hogy a szolgáltatás egy olyan vészhelyzet esetén. hello egyszerűen tooavoid a katasztrófa tooensure, amely hello szolgáltatás ténylegesen fut a több számítógép. Tesztelés az is szükséges tooensure hello sikertelen volt-e egy számítógép nem zavarja hello szolgáltatást futtató. Egy helyettesítő példányt is létrehozható máshol és, hogy kapacitásának csökkentése nem túlterhelés fennmaradó szolgáltatások hello kapacitástervezés biztosítja. hello ugyanilyen mintájú dolog, amit tooavoid hello meghibásodása függetlenül működik. Például. Ha aggódik hello sikertelen volt-e a TÁROLÓHÁLÓZAT, több San-okkal keresztül futtatja. Ha aggódik kiszolgálók állvány hello elvesztését, akkor több állványra futtatása. Ha aggódik adatközpontok hello adatvesztés, a szolgáltatás a több Azure-régiók vagy adatközpontok futhat. 

Ha ilyen típusú átnyúló módban fut, még mindig tulajdonos toosome típusú egyidejű hibák, de egyetlen és még több hibák számát egy adott típusának (például: egyetlen virtuális gép vagy a hálózati kapcsolat sikertelen) kezeli automatikusan (és már nem a "vész"). A Service Fabric a bővített hello fürt és kezeli a hogy vissza a meghibásodott csomópontok és a szolgáltatások számos mechanizmust biztosít. A Service Fabric is lehetővé teszi, hogy a szolgáltatások több példányát rendelés tooavoid a nem tervezett leállások ilyen típusú forduljon rendszert futtató valódi katasztrófa.

Miért egy központi telepítési elég nagy toospan hibák az állomásokon futó esetén nem valósítható meg oka lehet. Például további hardver-erőforrások, mint a most nem kívánok eltarthat toopay a hiba relatív toohello esélyét. Elosztott alkalmazások a meghatározásakor, hogy további kommunikációs ugrások vagy állapot replikációs költségek földrajzi távolság okok elfogadhatatlan késleltetés között lehet. Ha ezt a sort megrajzolása eltér az egyes alkalmazásokhoz. Szoftver hibák pontosabban hello hiba lehet hello szolgáltatásban tooscale próbált. Ebben az esetben több példányt nem akadályozzák meg a hello katasztrófa, mivel hello hibafeltétel minden hello példányára visszamenőleges korrelációban állnak.

### <a name="operational-faults"></a>Működési hibák
Akkor is, ha a szolgáltatás a sok létszámcsökkentések hello földgömb van átnyúlhatnak, továbbra is fedezheti katasztrofális esemény. Ha például valaki véletlenül újrakonfigurálja hello szolgáltatás hello DNS-nevét, vagy törlése végleges. Tegyük fel tegyük fel, az állapotalapú Service Fabric-szolgáltatás volt, és valaki az adott szolgáltatás véletlenül törölt. Kivéve, ha egy másik megoldás, hogy a szolgáltatás és az összes hello állapotban volt van most már meg. Működési katasztrófák ilyen típusú ("sajnos") különböző megoldást és lépéseket helyreállításához szükséges rendszeres nem tervezett leállások-nál. 

Ezek a típusok működési hibáinak éppen hello legjobb módjai tooavoid
1. működési access toohello környezet korlátozása
2. szigorúan naplózási veszélyes műveletek
3. automatizálási ugyanazok, megakadályozza a manuális és a módosítások sávon kívüli és hello tényleges környezet ellen adott módosítások érvényesítése előtt életbe őket
4. Gondoskodjon arról, hogy ilyen beállítások mellett pusztító műveletek "Soft szót". Szoftveres műveletek nem azonnal lépnek érvénybe, vagy visszavonható néhány időkerete belül

A Service Fabric biztosít bizonyos mechanizmusok tooprevent működési hibák, így például [szerepköralapú](service-fabric-cluster-security-roles.md) hozzáférés-vezérlés a fürt működését. A működési hibák többségét kérhetnek szervezeti erőfeszítéseket és más rendszerek. A Service Fabric egyes fennmaradó működési hibák, ilyen például a biztonsági mentési mechanizmus biztosítása, és állítsa vissza az állapotalapú szolgáltatások.

## <a name="managing-failures"></a>Hibák kezelése
hello Service Fabric célja szinte mindig automatikus hiba felügyeletét. Azonban a rendezés toohandle hibák bizonyos típusú, a szolgáltatások további kódot kell rendelkeznie. Más típusú hibák kell _nem_ automatikusan címezhető biztonsági és üzleti folytonossági okok miatt. 

### <a name="handling-single-failures"></a>Egyetlen hibák kezelése
Egyetlen gépek mindenféle okból sikertelen lehet. Ezek némelyike hardver oka van, például a áramforrások és a hálózati hardver meghibásodása. Szoftver más hibák vannak. Ezek közé tartozik a hibák hello tényleges operációs rendszer és hello szolgáltatást. A Service Fabric automatikusan észleli a hibákat, beleértve azokat az eseteket, ahol hello gép válik különítve a többi gép toonetwork problémák miatt az ilyen típusú.

Hello szolgáltatás típusa, függetlenül fut egy példányban eredmények állásidőt, hogy a szolgáltatás Ha hello kódot, hogy egyetlen példányát bármilyen okból nem sikerül. 

Rendelés toohandle minden egyetlen hiba hello legegyszerűbb művelet, amelyet, amely alapértelmezés szerint több csomópontján futtatni a szolgáltatások tooensure. Az állapotmentes szolgáltatások ehhez azzal, hogy egy `InstanceCount` 1-nél nagyobb. Állapotalapú szolgáltatások hello minimális ajánljuk, mindig egy `TargetReplicaSetSize` és `MinReplicaSetSize` legalább 3. A szolgáltatáskód hibáit további példányait futtató biztosítja, hogy a szolgáltatás képes automatikusan kezelni minden egyetlen hiba. 

### <a name="handling-coordinated-failures"></a>Kezelési koordinált hibák
Koordinált hiba akkor fordulhat elő, a fürt megfelelő tooeither tervezett vagy nem tervezett infrastruktúra hibák és módosításokat, vagy tervezett szoftver változásainak. A Service Fabric modellek infrastruktúra zónák, hogy a tartalék tartományok, koordinált hibák. Frissítési tartományok, amelyeknek a koordinált szoftvermódosítások területek van modellezve. További információ a tartalék és a frissítési tartományok van [Ez a dokumentum](service-fabric-cluster-resource-manager-cluster-description.md) , amely leírja a fürt topológia és definíciója.

Alapértelmezés szerint a Service Fabric hiba és a frissítési tartományok tekinti, ahol kell futtatni a szolgáltatások tervezése során. Alapértelmezés szerint a Service Fabric megpróbál tooensure, amely a szolgáltatások több hiba és a frissítési tartományok közötti futtatni, hogy a tervezett vagy nem tervezett módosítások fordulhat elő, ha a szolgáltatások elérhetők maradnak. 

Tételezzük fel például, hogy sikertelen volt-e az áramforrás egyidejűleg okozza-e a gépek toofail rögzítve. Több gép hello megszűnését futó és tartalék tartomány hibát hello szolgáltatás több példányával kapcsolja be egy másik példa egy adott szolgáltatáshoz egyetlen hiba. Éppen ezért a tartalék tartományok kezelése többé már kritikus tooensuring magas rendelkezésre állású a szolgáltatásokat. Az Azure Service Fabric futtatásakor tartalék tartományok automatikusan kezeli. Más környezetekben nem lehetnek. Ha a saját telephelyükön található fürtök most létre, meg arról, hogy toomap és a tartalék tartomány elrendezését tervezik megfelelően.

Frissítési tartományok akkor hasznosak, ahol szoftver érintetlen toobe frissítheti hello azonos területek modellezési idő. Ebből kifolyólag frissítési tartományok is gyakran hello határokat, ahol szoftvereket van leállítaná tervezett frissítéskor határozza meg. A Service Fabric- és a szolgáltatások frissítéseket kövesse hello ugyanannak a modellnek. A működés közbeni frissítés frissítési tartományok és hello Service Fabric állapotmodell, amelyek segítségével a nemkívánatos módosítások megakadályozása hello fürt és a szolgáltatást érintő kapcsolatban bővebben lásd: a dokumentumok:

 - [Az alkalmazásfrissítés](service-fabric-application-upgrade.md)
 - [Frissítési vonatkozó oktatóanyag](service-fabric-application-upgrade-tutorial.md)
 - [Service Fabric Állapotmodell](service-fabric-health-introduction.md)

A megadott hello betűcsoport-leképezés használatával fürt hello elrendezés jelenítheti meg [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md):

<center>
![A Service Fabric Explorerben tartalék tartományok elosztva csomópontok][sfx-cluster-map]
</center>

> [!NOTE]
> Hiba: a működés közbeni frissítés, a szolgáltatáskód hibáit és a állapot több példányát futtató területéhez modellezési elhelyezési szabályokat tooensure a szolgáltatások futnak hiba és a frissítési tartományok között, és beépített állapotfigyelés csak **néhány** a hello Service Fabric rendelés tookeep normál működési problémák és hibák forduljon az katasztrófák biztosító funkciókat. 
>

### <a name="handling-simultaneous-hardware-or-software-failures"></a>Egyidejű hardver- vagy hibák kezelése
Újabb megtartásról egyetlen hibákról. Látható, a rendszer mind az állapotmentes és állapotalapú szolgáltatások könnyen toohandle csak tartalék és verziófrissítési futtatásáért hello kódot (és állapot) további példányait tartja. Több egyidejű véletlen hibákat is megtörténhet. Ezek a tényleges katasztrófa nagy valószínűséggel toolead tooan.


### <a name="random-failures-leading-tooservice-failures"></a>Véletlenszerűen hibák vezető tooservice hibák
Tegyük fel, hogy hello szolgáltatást kellett egy `InstanceCount` 5, és több csomópontok azokat a példányokat futtató összes sikertelen a következő hello azonos idő. A Service Fabric válaszol a többi csomóponton helyettesítő példányok automatikusan létrehozásával. Csere létrehozását, amíg hello szolgáltatás vissza tooits szükségeskonfiguráció-példányok száma továbbra is. Másik példaként, tegyük fel, az állapot nélküli szolgáltatás történt egy `InstanceCount`-1, ami azt jelenti, az érvényes hello fürt csomópontjaihoz fut. Tegyük fel, hogy a logikailemez némelyike volt toofail. Ebben az esetben a Service Fabric megjegyzések hello szolgáltatás nem a megfelelő állapotban van, és megpróbál toocreate hello példányok hello csomópontján, ha hiányoznak. 

Az állapotalapú szolgáltatások hello helyzet attól függ, hogy hello szolgáltatást rendelkezik megőrzött állapot vagy nem. Is függ, hogy hány replikák hello szolgáltatást kellett, és hány sikertelen. Hogy egy olyan vészhelyzet esetén egy állapotalapú szolgáltatás történt, ezért a kezelése, a következő három szakaszban meghatározása:

1. Ha történt kvórum elvesztése vagy nem meghatározása
 - A kvórum elvesztése egy állapotalapú szolgáltatás hello replikák többsége nem működnek a hello bármikor egy időben, ideértve a elsődleges hello.
2. Ha hello kvórum elvesztése állandó van-e vagy sem
 - Legtöbbször ennek hello hibák átmeneti jellegűek. Folyamatok újraindul, csomópont újraindul, virtuális gépek vannak s, hálózati partíciók javítandó. Egyes esetekben azonban hibák sem lesznek állandó. 
    - A szolgáltatások nélkül megőrzött állapot legalább egy kvórum-replikák eredmények hibát _azonnal_ állandó kvórumveszteségben. Service Fabric kvórum elvesztése állapot-nyilvántartó nem állandó szolgáltatás észleli, ha 3 toostep is deklarálni kell (esetleges) dataloss azonnal folytatódik. A Folytatás toodataloss szabálykészletében, mert a Service Fabric ismeri az, hogy nincs-e a Várakozás hello replikák toocome vissza, a pont, mert még akkor is, ha helyre lett állítva azok lenne üres.
    - Állapot-nyilvántartó állandó szolgáltatásokhoz legalább egy kvórum-replikák hiba hatására a Service Fabric toostart hello replikák toocome vissza és visszaállítási kvórum várakozás. Bármely ennek eredményeként a szolgáltatáskimaradás _ír_ hatással toohello hello szolgáltatás partíció (vagy "replikakészlethez"). Előfordulhat azonban, olvasási továbbra is lehetséges a csökkentett konzisztencia biztosítja. hello alapértelmezett, hogy mennyi ideig Service Fabric megvárja-e a kvórum toobe vissza azért végtelen, mert a Folytatás az (esetleges) dataloss esemény, és más kockázatokat hordoz magában. Hello alapértelmezett felülbírálása `QuorumLossWaitDuration` érték lehet, de nem ajánlott. Ehelyett jelenleg összes kell erőfeszítéseket toorestore hello replikák le. Ehhez a gépidőt hello csomópontot, amely le a biztonsági mentést, és győződjön meg arról, hogy azok is újracsatlakoztatni hello meghajtók hello helyi állandó állapot tárolásához. Hello kvórum elvesztése folyamat hibája okozza, ha a Service Fabric automatikusan megpróbál toorecreate hello folyamatokat, és indítsa újra a bennük hello replikák. Ha ez nem sikerül, a Service Fabric állapot hibát jelez. Ha ezek orvosolni tudja majd hello replikák általában térjen vissza. Egyes esetekben azonban hello replikák nem térhetnek vissza. Például hello meghajtók összes sikertelen volt, vagy hello gépek fizikailag megsemmisül valamilyen módon. Ebben az esetben egy állandó kvórum adatvesztés most van. hello le ismét, replikák toocome a fürt rendszergazdája Várakozás tootell Service Fabric toostop meg kell határoznia, amelyek szolgáltatásokat érinti, és hívja meg hello partíciók `Repair-ServiceFabricPartition -PartitionId` vagy ` System.Fabric.FabricClient.ClusterManagementClient.RecoverPartitionAsync(Guid partitionId)` API.  Ez az API lehetővé teszi, hogy hello partíció toomove QuorumLoss kívül, és a potenciális dataloss hello Azonosítójának megadása.

> [!NOTE]
> Az _soha nem_ biztonságos toouse az API nem adott partíciókra elleni célzott módon. 
>

3. Meghatározhatja, hogy a tényleges adatvesztés lett, és a biztonsági mentésekből visszaállítása
  - Ha a Service Fabric meghívja a hello `OnDataLossAsync` miatt a rendszer mindig metódus _gyanús_ dataloss. A Service Fabric biztosítja, hogy a hívás érkezik toohello _legjobb_ fennmaradó replika. Ez a tetszőleges kiegészítendő replika tett hello legtöbb folyamatban van. mindig fel ok hello _gyanús_ dataloss, lehetséges, hogy hello fennmaradó replika rendelkezik-e minden olyan állapotban, ahogy hello elsődleges amikor csökkent. Azonban, hogy állapot toocompare nélkül azt, hogy nincs megfelelő mód a Service Fabric vagy operátorok tooknow biztosan. Ezen a ponton a Service Fabric is tudja hello más replikák nem érkeznek vissza. Hello döntés, ha azt várakozik hello kvórum elvesztése tooresolve magát, amely volt. hello helyes intézkedéseket hello szolgáltatás az általában toofreeze és a megadott rendszergazdai beavatkozás várakozás. Ezért funkciója hello tipikus végrehajtásának `OnDataLossAsync` metódus tegye?
  - Első lépésként jelentkezzen, amely `OnDataLossAsync` lett elindítva, és minden szükséges felügyeleti riasztások megkezdéséhez.
   - Általában ezen a ponton toopause és további döntések és manuális műveletek toobe végrehajtott várakozás. Ennek oka az, akkor is, ha a biztonsági másolatok szükségük toobe előkészítve. Például két különböző szolgáltatások koordinálja az adatokat, ha azokat a biztonsági mentések esetleg toobe, amely után a hello visszaállítás történik, hogy hello információt e két szolgáltatás érdeklik megfelelő sorrendben tooensure módosítva. 
  - Gyakran van is néhány egyéb telemetriai vagy származó hello szolgáltatást. A metaadatok tartalmazhatja az egyéb szolgáltatások vagy a naplókban. Ezeket az információkat lehet használt szükséges toodetermine, ha bármely hívások kapott és elsődleges hello feldolgozni, amely nincs jelen volt a biztonsági mentési vagy replikált toothis adott replika hello történt. Ezek esetleg toobe megismételt vagy hozzáadott toohello biztonsági másolat, előtt visszaállítás esetén valósítható meg.  
   - A replika tartozó állapot toothat minden biztonsági másolatának elérhető szereplő fennmaradó hello összehasonlítást. Hello Service Fabric megbízható gyűjtemények használatával, akkor vannak eszközökhöz, és feldolgozza a úgy érhető el, ha ismertetett [Ez a cikk](service-fabric-reliable-services-backup-restore.md). hello célja toosee Ha hello belül hello a replika állapota megfelelő, vagy milyen hello biztonsági mentés hiányozhat is.
  - Egyszer hello összehasonlítás történik, és szükséges hello visszaállítás befejeződött, ha a hello szolgáltatást kód értéke true, ha az állapot módosítások kell visszaadnia. Ha hello replika határozza meg, hogy hello legjobb elérhető példányát hello állapotban volt, és nem módosítja, majd térjen vissza hamis. Igaz érték azt jelenti, hogy minden _más_ replikák fennmaradó most lehet a erre. Azok lesz dobva, újraépíti a replikából. Hamis azt jelzi, hogy nincs állapot változások, így hello más replikák biztosítható, azok rendelkeznek. 

Különösen fontos, hogy szolgáltatás szerzők gyakorlatban lehetséges dataloss és a hibát jelentő forgatókönyvek előtt szolgáltatás telepítve van a termelési. tooprotect elleni dataloss hello lehetőségét, fontos tooperiodically [készítsen biztonsági másolatot hello állapot](service-fabric-reliable-services-backup-restore.md) tooa georedundáns tárolt állapotalapú szolgáltatások egyikét sem. Azt is ellenőrizze, hogy rendelkezik-e hello képességét toorestore azt. Mivel számos különböző szolgáltatások biztonsági mentést készít a különböző időpontokban, kell, hogy a visszaállítást követően a szolgáltatások rendelkeznek egymástól egységes megjelenítése tooensure. Vegye figyelembe például olyan helyzet, ahol egy szolgáltatás állít elő, egy szám és tárolja, majd elküldi tooanother szolgáltatás, amely is tárolja. A visszaállítást követően felfedezheti, hogy rendelkezik hello második hello szolgáltatást, de hello először viszont nem, mert annak biztonsági mentési művelet nem tartozik.

Megállapítja, hogy hello fennmaradó replikák dataloss esetén a megfelelő toocontinue, és telemetriai vagy kipufogógáz szolgáltatás állapota nem helyreállítására, ha a biztonsági mentések gyakoriságát hello meghatározza, hogy a legjobb lehetséges helyreállítási időkorlát (RPO) . A Service Fabric számos eszközt kínál a tesztelési különböző sikertelen forgatókönyvek, köztük a végleges kvórum és dataloss van szükség egy biztonsági másolatból. Ezek a forgatókönyvek szerepelnek a Service Fabric tesztelhetőségi eszközök hello hiba az Analysis Services által kezelt részeként. További információ ezen eszközök és a minták érhető [Itt](service-fabric-testability-overview.md). 

> [!NOTE]
> Rendszerszolgáltatások is hello terheli adott toohello szolgáltatás az adott a kvórum elvesztése esetén csökkenhet. Például hello a naming service a kvórum elvesztése hatással van a névfeloldás, mivel a kvórum elvesztése a Feladatátvevőfürt-kezelő szolgáltatás hello blokkolja az új szolgáltatás létrehozása és a feladatátvétel. Hello Service Fabric rendszerszolgáltatások hello ugyanaz az állapotkezelést a szolgáltatásként mintát követi, amíg nem ajánlott, hogy meg kell próbálni toomove kívül a kvórum elvesztése, valamint a potenciális dataloss őket. hello javasoljuk túl, hanem[támogatási pozícionálni](service-fabric-support.md) olyan megoldás, amely toodetermine céloz tooyour adott helyzet.  Általában akkor előnyösebb toosimply várakozási amíg hello visszatérési replikák le.
>

## <a name="availability-of-hello-service-fabric-cluster"></a>Service Fabric-fürt hello rendelkezésre állása
Hello Service Fabric-fürt maga általánosságban véve nem meghibásodási ponttal rendelkező magas elosztott környezetben. Bármely egy csomópont hiba nem okoz rendelkezésre állás vagy a megbízhatósági hibákat elhárító hello fürt elsősorban, mert hello Service Fabric rendszerszolgáltatások hajtsa végre a korábban megadott azonos irányelvek hello: mindig hibaüzenettel három vagy több replikák alapértelmezés szerint, és azokat állapot nélküli rendszer-szolgáltatásokat futtatni az összes olyan csomóponton. hello alapul szolgáló Service Fabric hálózatkezelési és hiba észlelése rétegek teljesen terjesztése. A legtöbb rendszerszolgáltatások úgy is, a metaadatok hello fürt, vagy tudja, hogyan tooresynchronize állapotukra más helyeiről. hello fürt hello rendelkezésre állását képes lesz biztonsága sérül, ha rendszerszolgáltatások feltölti a kvórum elvesztése helyzetek például szerepel a fenti. Ezekben az esetekben nem lehet bizonyos hello fürtön műveletek, például egy frissítés elindítása vagy új szolgáltatások telepítése, de hello fürt továbbra is fel tud tooperform. Már fut a szolgáltatás fut, ezek a feltételek a marad, kivéve, ha szükségük van az írási műveletek toohello rendszer szolgáltatások toocontinue működik. Például ha a Feladatátvevőfürt-kezelő hello kvórumveszteségben van minden szolgáltatás toorun folytatódik, de bármely eleget nem tevő szolgáltatás nem lesz képes tooautomatically újraindítást, mivel ehhez a Feladatátvevőfürt-kezelő hello hello bevonása. 

### <a name="failures-of-a-datacenter-or-azure-region"></a>A datacenter vagy az Azure-régió hibák
Bizonyos ritkán előforduló esetekben, átmenetileg nem érhető el, miatt válhat a fizikai adatközpont tooloss a teljesítmény vagy a hálózati kapcsolat. Ezekben az esetekben a Service Fabric-fürtök és a szolgáltatások, az adott datacenter vagy az Azure-régió nem lesz elérhető. Azonban _az adatok megmaradjanak_. Az Azure-ban futó fürtök esetén a hello kimaradások megtekintheti a frissítések [Azure állapotlapon][azure-status-dashboard]. A hello nagyon valószínű esemény, amely a fizikai adatközpont részlegesen vagy teljesen megsemmisül, a Service Fabric-fürtök üzemeltetett van, vagy bennük hello szolgáltatások elveszhet. Ez magában foglalja a bármely kívül adatközpontját, illetve a régió nem biztonsági állapotát.

Nincs a két különböző stratégiák a még működő hello egyetlen datacenter vagy terület végleges vagy tartós hiba. 

1. Különálló Service Fabric-fürtök futtassa több régióba, és a feladatátvétel és visszavenni a feladatokat a különböző környezetek között valamilyen mód használatára. Az ilyen több fürt aktív-aktív vagy aktív-passzív modell felügyelete és műveletei kód szükséges. Ehhez is készített biztonsági másolat egy adatközpontban, illetve a régió hello szolgáltatások összehangolását, hogy elérhetők más adatközpontok vagy régiókban, ha egy sikertelen lesz. 
2. Futtassa egy egyetlen Service Fabric-fürt több adatközpontban vagy régiókból is. hello támogatott konfigurációkra vonatkozó minimális ez három adatközpontban vagy régióban. hello javasolt régiók száma vagy adatközpontok öt. Ehhez egy összetettebb fürtjének topológiája. Azonban hello Ez a modell előnye, hogy egy datacenter vagy régió sikertelen alakul át egy olyan vészhelyzet esetén a normál hiba. Ezek a hibák kezelhetik hello mechanizmusokat, amelyek működnek a fürtök egyetlen régión belül. Tartalék tartományok, a frissítési tartományok és a Service Fabric elhelyezési szabályokat győződjön meg arról, munkaterhelések terjesztése, hogy azok működését normál hibák. Amely segít az ilyen típusú fürt szolgáltatások működtetéséhez házirendekkel kapcsolatos további információkért olvassa a [elhelyezési házirendeket](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)

### <a name="random-failures-leading-toocluster-failures"></a>Véletlenszerűen hibák vezető toocluster hibák
A Service Fabric rendelkezik kezdőérték csomópontok hello fogalmát. Ezek a csomópontokra, amelyeket az alapul szolgáló fürt hello hello rendelkezésre állását fenntartásához. Ezek a csomópontok segíthet az tooensure hello fürt továbbra is másolatot létrehozó további csomópontokkal bérleteket és bizonyos típusú hálózati hibák során tiebreakers szolgál. Ha a véletlenszerűen hibák hello kezdőérték csomópontok többsége eltávolítása hello fürt, és azok nem teszik vissza, hello fürt automatikusan leáll. Az Azure kezdőérték csomópontok automatikusan kezelt: hello elérhető tartalék és verziófrissítési keresztül továbbítja, és egyetlen kezdőérték csomópont eltávolításakor hello fürtből létrejön-e egy másik helyén. 

A különálló Service Fabric-fürtök és a Azure "Elsődleges csomópont Type" hello egy hello magok futtató hello. Egy elsődleges csomóponttípusok meghatározásakor a Service Fabric lesz automatikusan előnyeit hello too9 kezdőérték csomópontok és az egyes hello rendszerszolgáltatások 9 replikák létrehozott csomópontok száma. Ha egy véletlenszerűen hibák készletét ki ezeket a rendszer szolgáltatás replikák többsége fogad adatokat egyidejűleg, hello rendszerszolgáltatások lép kvórum elvesztése, azt a fent leírt módon. Hello kezdőérték csomópontok többsége elvesznek, ha hello fürt le fog állni hamarosan után.

## <a name="next-steps"></a>Következő lépések
- Megtudhatja, hogyan toosimulate hello segítségével különféle hibák [tesztelhetőségi keretrendszer](service-fabric-testability-overview.md)
- Olvassa el a vész-helyreállítási és magas rendelkezésre állású erőforrását. A Microsoft tett közzé útmutatást nagy mennyiségű ezekben a kérdésekben. Közben ezeket a dokumentumokat némelyike toospecific technikák használható egyéb termékek, sok általános gyakorlati tanácsok hello Service Fabric környezetben is alkalmazhat tartalmazzák:
  - [Rendelkezésre állási ellenőrzőlista](../best-practices-availability-checklist.md)
  - [A vész-helyreállítási részletezési végrehajtása](../sql-database/sql-database-disaster-recovery-drills.md)
  - [Vész-helyreállítási és magas rendelkezésre állás a Azure-alkalmazások][dr-ha-guide]
- A [Service Fabric támogatási lehetőségeinek](service-fabric-support.md) ismertetése

<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
