---
title: "a Microsoft Azure Service Fabric aaaCommon kérdésekre |} Microsoft Docs"
description: "A Service Fabric és a válaszok kapcsolatos gyakori kérdések"
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: 
ms.assetid: 5a179703-ff0c-4b8e-98cd-377253295d12
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: chackdan
ms.openlocfilehash: 4cbe92d2a03f7a1ea5d077807fdc982288220a7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="commonly-asked-service-fabric-questions"></a>A Service Fabric gyakori kérdések

Nincsenek számos gyakran feltett kérdésekre a Service Fabric Teendők, és hogyan kell használni. Ez a dokumentum ismerteti a fenti gyakori kérdések és a válaszok.

## <a name="cluster-setup-and-management"></a>Fürt beállítása és kezelése

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions-or-my-own-datacenters"></a>Egy fürt, amely több Azure-régiók és a saját adatközpontját hozhat létre?

Igen. 

Service Fabric fürtözési technológia hello core lehet működő bárhol hello world használt toocombine gépek, feltéve, hogy rendelkeznek-e hálózati kapcsolat tooeach más. Azonban kialakításához és futtatásához az ilyen fürt bonyolult lehet.

Ha érdekli, ebben a forgatókönyvben, javasoljuk az forduljon tooget keresztül hello [Service Fabric Github problémák listájába](https://github.com/azure/service-fabric-issues) vagy a támogató szolgálat képviselője rendelés tooobtain további útmutatást a keresztül. a Service Fabric-csapat hello működik-e tooprovide további egyértelműség, útmutatást és javaslatokat ehhez a forgatókönyvhöz. 

Néhány dolgot tooconsider: 

1. hello Azure Service Fabric fürt erőforrás regionális ma, mert hello virtulálisgép-skálázási készletekben hello fürthöz épül. Ez azt jelenti, hogy hello eseményben regionális hiba előfordulhat, hogy elveszti hello képességét toomanage hello fürt hello Azure Resource Manageren keresztül vagy hello Azure portálon. Ez akkor fordulhat elő, annak ellenére, hogy hello fürt továbbra is futni fog, és hozzá tud toointeract közvetlenül lenne. Emellett Azure ma nem biztosít hello képességét toohave egyetlen virtuális hálózaton használható régiók között. Ez azt jelenti, hogy az Azure-ban több területi fürt szükséges [nyilvános IP-címeket az egyes virtuális gépek, a Virtuálisgép-méretezési készlet hello](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#public-ipv4-per-virtual-machine) vagy [Azure VPN Gatewayek](../vpn-gateway/vpn-gateway-about-vpngateways.md). A fenti hálózatkezelési lehetőségek hatással különböző van a költségek, a teljesítmény és a toosome mértékben alkalmazás tervét, így gondos elemzés és tervezése előtt meg kell adni egy ilyen környezet állandó.
2. hello karbantartási, felügyeleti és figyelési ezeknek a gépeknek válhat bonyolult, különösen akkor, ha átnyúlhatnak _típusok_ a környezetek, például különböző szolgáltatók közötti vagy a helyszíni erőforrások és az Azure közötti . Gondot kell fordítani, tooensure, amely frissíti, a figyelés, a felügyeleti és diagnosztikai értendők hello fürt-vagy hello alkalmazások, az ilyen környezetekben termelési számítási feladatokhoz futtatása előtt. Ha már rendelkezik Azure-ban vagy a saját adatközpontját belül problémák megoldásához élmény rengeteg, akkor valószínű, hogy ezek azonos megoldások is alkalmazható, ha épület kimenő, vagy a Service Fabric-fürt fut. 

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Hajtsa végre a Service Fabric-csomópontok automatikusan frissítését az operációs rendszer?

Nem ma de ez történik akkor is, hogy Azure százalékát toodeliver közös kérelmet.

Az ideiglenes hello tudunk [alkalmazás megadott](service-fabric-patch-orchestration-application.md) , hogy hello operációs rendszerek a Service Fabric-csomópont alatt maradnak javított és toodate fel.

hello kihívás az operációs rendszer frissítése érdekében, hogy ezek általában a számítógép újraindítása szükséges hello gép, amely ideiglenes rendelkezésre állást eredményez. Önmagában ez nem probléma, mivel a Service Fabric automatikusan átirányítja a forgalmat ezen szolgáltatások tooother csomópontok. Azonban ha operációs rendszer frissítése érdekében van megfelelő koordináció hiányában hello fürtön, nincs hello lehetséges, hogy sok csomópont egyszerre leáll. Ilyen egyidejű újraindítások okozhat a teljes rendelkezésre állást egy szolgáltatás, vagy legalább egy adott partícióra (az állapotalapú szolgáltatás).

A jövőbeli hello hogy terv toosupport egy operációs rendszer frissítési házirend teljesen automatizált és frissítési tartományokon, koordinált győződjön meg arról, hogy rendelkezésre állás annak ellenére, hogy az újraindítások és más váratlan meghibásodások esetén.

### <a name="can-i-use-large-virtual-machine-scale-sets-in-my-sf-cluster"></a>Használható a ú fürt nagy virtuálisgép-méretezési csoportok? 

**Válasz rövid** – nem 

**Hosszú válasz** – bár hello nagy virtuálisgép-méretezési csoportok lehetővé teszik tooscale egy virtuálisgép-méretezési csoport legfeljebb 1000 Virtuálisgép-példányok, elhelyezési csoportok (PGs) használatával hello ilyeneket. Tartalék tartományok (FDs) és a frissítési tartományok (UDs) belül egy elhelyezési csoport Service fabric használ FDs és UDs toomake elhelyezési döntéseit a szolgáltatáspéldány replikák/szolgáltatás csak megegyeznek. Mivel a hello FDs és UDs összehasonlítható elhelyezési csoporton belül csak ú nem tudja azt használni. Például, ha a PG1 VM1 a FD-topológia = 0, és a PG2 VM9 a FD-topológia = 4, ez nem jelenti azt, hogy VM1 és vm2 virtuális gépnek a két különböző hardver Rackszekrények, ezért ú nem használható hello FD értékek az az eset toomake elhelyezési kapcsolatos döntések meghozásakor.

Jelenleg más problémákat nagy virtuálisgép-méretezési csoportok, például a 4. szint hello hiánya betölteni a terheléselosztási támogatását. Tekintse meg a toofor [nagy részleteinek méretezése beállítása](../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md)



### <a name="what-is-hello-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller"></a>Mi az a Service Fabric-fürt hello minimális mérete? Miért nem legyen kisebb?

hello minimális támogatott termelési számítási feladatokhoz futó Service Fabric-fürt mérete az öt csomóponttal. Fejlesztési/Tesztelési forgatókönyvek esetén három csomópontos fürt támogatott.

Ezek minimumértékének létezik, mert a Service Fabric-fürt hello lefuttat egy állapotalapú rendszerszolgáltatások, beleértve a naming service hello és hello Feladatátvevőfürt-kezelő. Ezeket a szolgáltatásokat, amelyek nyomon követheti, mi le lett telepítve toohello fürt, és ha azok még jelenleg üzemel, az erős konzisztencia függenek. Hello képességét tooacquire viszont függ, hogy az erős konzisztencia egy *kvórum* bármely adott frissítés toohello állapotának azokat szolgáltatási, ahol a kvórum jelenti szigorú többsége hello replikák (N/2 + 1) egy adott szolgáltatáshoz.

A háttérben történő vizsgáljuk meg néhány lehetséges fürtkonfigurációk:

**Egy csomópont**: ezt a beállítást nem biztosít magas rendelkezésre állású, mivel hello megszűnését hello egyetlen csomópont bármilyen okból azt jelenti, hogy a hello fürtözési hello megszűnését.

**Két csomópont**: két csomópont között telepített szolgáltatáshoz a kvórum (N = 2) 2 (2/2 + 1 = 2). Egy replikát nem vesztek el, esetén nem lehetséges toocreate kvórumához. Az szolgáltatás frissítés végrehajtása szükséges egy replika le ideiglenesen véve, ez a beállítás nem hasznos.

**Három csomópont**: a három csomópont (N = 3) révén hello követelmény toocreate egy kvórum még két csomópont (3/2 + 1 = 2). Ez azt jelenti, hogy az egyes csomópontok elvesznek, és továbbra is a kvórum fenntartása.

három csomópont fürtkonfiguráció hello támogatott fejlesztési és tesztelési célú biztonságosan frissítések végrehajtása és után is megmaradnak az egyes csomópontok hibáit, mert mindaddig, amíg azok nem fordulhat elő, egyidejűleg. A termelési számítási feladatokhoz rugalmas toosuch egyidejű hibája esetén, kell lennie, ezért a öt csomópontra szükség.

### <a name="can-i-turn-off-my-cluster-at-nightweekends-toosave-costs"></a>Lehet kikapcsolni a fürt éjszakai/hétvégéket toosave költségek?

Általában nem. A Service Fabric tárolja állapot helyi, rövid élettartamú lemezeken, ami azt jelenti, hogy ha hello virtuális gép áthelyezett tooa másik gazdagépet, hello adatok nem helyezi át a vele. A normál működés, amely nincs probléma, hello új csomópont jelenik toodate más csomópontok. Azonban ha állítson le minden csomópontot, és azokat később újraindíthatja a rendszert, nincs jelentős esély arra, hogy hello csomópontok többsége indítsa el az új gazdagépeknek, és ellenőrizze a rendszer nem toorecover hello.

Ha szeretné, hogy az alkalmazás teszteléséhez, mielőtt telepítené toocreate fürtök, azt javasoljuk, hogy dinamikusan hozzon létre ezeket a fürt részeként a [folyamatos integráció/folyamatos központi telepítési folyamat](service-fabric-set-up-continuous-integration.md).


### <a name="how-do-i-upgrade-my-operating-system-for-example-from-windows-server-2012-toowindows-server-2016"></a>Hogyan lehet frissíteni az operációs rendszer (például a Windows Server 2012 tooWindows Server 2016-os)?

Napjainkban fejlesztjük fejlett élményt, amíg való telepítésért felelős hello frissítését. Hello hello az operációs rendszer lemezképét frissítenie kell a fürt egy virtuális gép virtuális gépeinek hello egyszerre. 

## <a name="container-support"></a>Tároló-támogatás

### <a name="why-are-my-containers-that-are-deployed-toosf-unable-tooresolve-dns-addresses"></a>Miért tárolói a telepített tooSF nem tooresolve DNS képező címeinek?

A probléma 5.6.204.9494 lévő fürtökön érkezett jelentés verziója 

**Megoldás** : kövesse [Ez a dokumentum](service-fabric-dnsservice.md) tooenable hello DNS service fabric-szolgáltatás a fürtön.

**Javítsa ki** : támogatott frissítési tooa fürt verziója, amely nagyobb, mint 5.6.204.9494, ha azok elérhetők. Ha a fürt tooautomatic frissítéseket, majd hello fürt automatikusan frissíti a rögzített probléma toohello verziót.

  
## <a name="application-design"></a>Alkalmazás tervezése

### <a name="whats-hello-best-way-tooquery-data-across-partitions-of-a-reliable-collection"></a>Mi az a hello legjobb módja tooquery adatok megbízható gyűjtemény partíciók között?

Megbízható gyűjtemények jellemzően [particionált](service-fabric-concepts-partitioning.md) a nagyobb teljesítmény és átviteli kibővítési tooenable. Ez azt jelenti, hogy egy adott szolgáltatáshoz hello állapot terjedhetnek 10 egység vagy 100-as egység gépek között. tooperform műveletek során a teljes adatkészlet, közül néhány:

- Hozzon létre egy másik szolgáltatás toopull hello szükséges adatok minden partíciója lekérdező szolgáltatást.
- Hozzon létre egy szolgáltatás, amely egy másik szolgáltatás minden partíciója fogadhat adatokat.
- Minden szolgáltatás tooan külső store-ból adatokat rendszeres időközönként. Ezt a módszert használja, csak ha hello lekérdezések végrehajtásához nem szerepelnek a fő üzleti logika.


### <a name="whats-hello-best-way-tooquery-data-across-my-actors"></a>Mi az a hello legjobb módja tooquery adatok a szereplője között?

Szereplője tervezett toobe független egység állapot és számítási, így nem ajánlott tooperform aktorállapot futásidőben széleskörű lekérdezését. Ha egy szükséges tooquery keresztül hello aktorállapot teljes készletét, érdemes vagy:

- A aktorszolgáltatások cseréje az állapot-nyilvántartó megbízható szolgáltatásokat, úgy, hogy a hálózati hello száma toogather összes adatokat kér hello szereplője toohello száma a szolgáltatás a partíciók száma.
- A szereplője tooperiodically leküldéses tervezése a tooan külső állapottárolóhoz könnyebb lekérdezése. Újabb verzióiban ez a megközelítés lesz csak kivitelezhető, ha a helyreállítást hajt végre hello lekérdezések esetén nincs szükség a működését.

### <a name="how-much-data-can-i-store-in-a-reliable-collection"></a>Mennyi adatot tud tárolni egy megbízható gyűjtemény?

Megbízható szolgáltatások általában particionáltak, így hello tárolhatja csak korlátozza, és a hello ezeken a számítógépeken elérhető memória mennyiségének hello fürt rendelkezik gépek hello száma.

Tegyük fel tegyük fel, hogy egy megbízható gyűjtemény 100 partíciók 3 replikák egy szolgáltatást a 1kb méretű átlagos objektumok tárolására. Most tegyük fel, hogy rendelkezik-e a 16gb memóriája gépenként 10 gép fürtben. Az egyszerűség és nagyon óvatos toobe azt feltételezik, hogy hello operációs rendszerek és rendszerszolgáltatások hello Service Fabric-futtatókörnyezet és a szolgáltatások felhasználásához 6gb, így a 10 GB-os gépenként érhető el, vagy 100gb hello fürt.

Figyelembe vételével kell lennie a minden objektumon tárolt három időpontokban (egy elsődleges és két replika), akkor egy körülbelül 35 millió objektumok elegendő memória áll rendelkezésre a gyűjteményben teljes kapacitás üzemi. Azt javasoljuk azonban egy hiba tartományban, és egy frissítési tartomány, amely körülbelül 1/3 kapacitás, és a csökkenne hello számú tooroughly 23 millió egyidejű megszűnését rugalmas toohello alatt.

Vegye figyelembe, hogy a számítási is feltételezi, hogy:

- Hogy az adatok így vannak elrendezve hello partíciók hello terjesztési többé-kevésbé egységes, és, hogy van-e reporting terhelési metrika toohello fürt erőforrás-kezelő. Alapértelmezés szerint a Service Fabric rendszer terheléselosztásához replikáinak száma alapján. A fenti példában, célszerű helyezni 10 elsődleges replika és 20 másodlagos replikák hello fürt minden csomópontjának. Betöltési, amely egyenlően elosztva hello partíciók esetén, amelyek működik. Betöltési nincs még akkor is, ha jelenteniük kell terhelést, hogy hello erőforrás-kezelő is kisebb replikák együtt csomag és annak engedélyezése, hogy nagyobb replikák tooconsume több memóriát az egyes csomópontok.

- A szóban forgó hello megbízható szolgáltatás hello fürt tárolni állapot csak egy hello. Mivel több szolgáltatás tooa fürtök telepítése, kell toobe szem előtt tartva hello erőforrásokat, hogy minden egyes fog toorun kell, és az állapot kezelése.

- Adott hello fürtnek nincs növekvő vagy zsugorítását. További gépek hozzáadásakor a Service Fabric egyensúlyba a replikák tooleverage hello további kapacitást, amíg gépek hello száma meghaladja a hello száma a szolgáltatás a partíciók, mivel az egyes replika gépek nem terjedhet ki. Ezzel szemben ha gépek eltávolításával csökkentenie hello fürt hello méretét, a replikák szigorúbban csomagolt tartalmaz, és teljes kapacitásának rendelkezik.

### <a name="how-much-data-can-i-store-in-an-actor"></a>Mennyi adatot is tárolnak a egy szereplő?

Csakúgy, mint a megbízható szolgáltatások hello adatmennyiséget szereplő szolgáltatásnak tárolhat csak korlátozza hello teljes lemezterület és a rendelkezésre álló memória hello a fürtben található csomópontok között. Azonban egyedi szereplője esetén leghatékonyabb használt tooencapsulate állapot és a kapcsolódó üzleti logika kis mennyiségű. Általános szabályként elmondható egy egyéni szereplő, amelyet a rendszer kilobájtban kell rendelkeznie.

## <a name="other-questions"></a>Egyéb kérdések

### <a name="how-does-service-fabric-relate-toocontainers"></a>Milyen kapcsolatban áll a Service Fabric toocontainers?

Tárolók ajánlatot egy egyszerű módon toopackage szolgáltatások és függőségi viszonyaikat, úgy, hogy következetesen minden környezetben futtatni, és egy gépen csak elkülönített módon kapnak. A Service Fabric egy módon toodeploy nyújt, és szolgáltatásaink, így kezelése [, amely rendelkezik egy tárolóba van csomagolva szolgáltatások](service-fabric-containers-overview.md).

### <a name="are-you-planning-tooopen-source-service-fabric"></a>A Service Fabric tooopen forrás tervezési?

Azt tervezi, tooopen forrás hello reliable services és a megbízható szereplője keretrendszerek a Githubon, és elfogadja a közösségi hozzájárulásokat toothose projektek. Kövesse a hello [Service Fabric blog](https://blogs.msdn.microsoft.com/azureservicefabric/) azok még jelent további részletekért.

hello jelenleg nincs tervek tooopen forrás hello Service Fabric-futtatókörnyezet.

## <a name="next-steps"></a>Következő lépések

- [További tudnivalók a Service Fabric Alapfogalmak és ajánlott eljárások](https://mva.microsoft.com/en-us/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965)
