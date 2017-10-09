---
title: "Kiszolgálói alkalmazás mintákat a virtuális gépek aaaSQL |} Microsoft Docs"
description: "Ez a cikk az Azure virtuális gépeken futó SQL Server alkalmazás mintákat ismerteti. Az megoldás mérnökök és a fejlesztők alapokat nyújt jó alkalmazás felépítéséről és kialakításáról."
services: virtual-machines-windows
documentationcenter: na
author: ninarn
manager: jhubbard
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 41863c8d-f3a3-4584-ad86-b95094365e05
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: ninarn
ms.openlocfilehash: e18597598fdf9fe534ed07331d6f531d4dca0601
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Azure-beli virtuális gépeken futó SQL Server – alkalmazásminták és fejlesztési stratégiák
[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="summary"></a>Összefoglalás:
Annak meghatározása, mely alkalmazásminta és minták toouse az SQL Server-alapú alkalmazások az Azure-ban környezet egy fontos tervezési döntés és hogy az SQL Server és Azure infrastruktúra-összetevőkhöz együttműködni alapos ismerete szükséges . Az SQL Server az Azure infrastruktúra-szolgáltatásokat ezt könnyen áttelepítése, karbantartása, és figyelheti a meglévő SQL Server alkalmazások beépített a Windows Server toovirtual gépek az Azure-ban.

hello Ez a cikk célja tooprovide megoldás mérnökök és a megfelelő alkalmazás felépítéséről és kialakításáról, amely azt az követve, mint a meglévő alkalmazások tooAzure áttelepítésekor alapját a fejlesztők is jól fejlesztését, új alkalmazásokat az Azure-ban.

Minden alkalmazás minta egy helyszíni forgatókönyvet, a saját felhő által engedélyezett megoldásnak talál, és a hello kapcsolódó technikai javaslatokat tartalmaz. Ezenkívül hello cikkből Azure-specifikus fejlesztési stratégiák, hogy az alkalmazások helyesen kialakítani. Esedékes toohello sok lehetséges alkalmazás kombinációját, ajánlott mérnökök és a fejlesztők az alkalmazásaikat és a felhasználók hello legmegfelelőbb mintát kell kiválasztani.

**Műszaki közreműködők:** Luis Carlos Vargas hering Madhan Arumugam Ramakrishnan

**Műszaki véleményezők:** Corey Sanders, Drew McDaniel, Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani, lengyel Schackow Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>Bevezetés
Számos különböző típusú n szintű alkalmazásokhoz fejleszthet hello összetevők hello másik alkalmazási rétegek hasonlóan külön összetevők különböző gépeken is megadhat. Például hello ügyfélalkalmazás elhelyezheti, és üzleti szabályokat egy számítógépen, az előtérbeli webes réteg és az adatelérési réteg összetevők egy másik gépen, és egy másik gépen háttér-adatbázis-rétegből összetevők. Az ilyen szerkezetének kialakítása segít elkülöníteni az egyes rétegek egymástól. Honnan származnak adatok módosításakor nem kell toochange hello ügyfél vagy a webes alkalmazás, de csak hello az adat-hozzáférési réteg összetevőket.

Egy tipikus *n szintű* alkalmazás tartalmazza a hello bemutatási szint, hello üzleti szint és hello adatréteg:

| Szint | Leírás |
| --- | --- |
| **Bemutató** |Hello *bemutatási szint* (webes réteg, előtér-réteg), amelyben a felhasználók interakcióba az alkalmazás hello réteg. |
| **Üzleti** |Hello *üzleti szint* (középső réteg) hello réteg adott hello bemutató réteg és hello adatok réteg használata toocommunicate egymással, és alapfunkciókat hello hello rendszer tartalmazza. |
| **Adatok** |Hello *adatszinten* alapjában hello kiszolgáló, amely az alkalmazás adatait (például egy SQL Servert futtató kiszolgáló) tárolja. |

Alkalmazás rétegek leírják hello logikai csoportjai hello funkcióit és az alkalmazásban; összetevők mivel a rétegek hello fizikai terjesztési összetevőiről és hello funkcióit mutatják külön fizikai kiszolgálók, számítógépek, hálózatok és a távoli helyeken. hello alkalmazás rétegek előfordulhat, hogy legyen elhelyezve hello azonos fizikai számítógép (hello azonos tartozó) ugyanarra a gépre (n szintű) kell elosztani, illetve hello minden egyes rétegben összetevői összetevőket más rétegeiből jól meghatározott felületeken keresztül kommunikálnak. Az eltolásokat tekintheti hello kifejezés réteg toophysical terjesztési minták például n szintű, kétrétegű és háromrétegű hivatkozásként. A **2 szintű alkalmazásminta** két alkalmazásrétegek tartalmazza: application server és adatbázis-kiszolgáló. hello közvetlen kommunikációt hello alkalmazáskiszolgáló és hello adatbázis-kiszolgáló között történik. hello alkalmazáskiszolgáló webes-réteg és a vállalati szintű összetevőket tartalmaz. A **3 szintű alkalmazás mintát**, nincsenek az alkalmazás három réteg: webkiszolgáló, az alkalmazás a kiszolgálón, amely hello üzleti logikai rétegből és/vagy az üzleti szint adat-hozzáférési összetevőket tartalmaz, és hello adatbázis-kiszolgáló. hello webkiszolgáló és az adatbázis-kiszolgáló hello hello kommunikációját hello alkalmazáskiszolgáló keresztül történik. Alkalmazás rétegeken és rétegek részletes információkért lásd: [Microsoft alkalmazás Referenciaarchitektúra útmutatója](https://msdn.microsoft.com/library/ff650706.aspx).

A cikk elolvasása előtt rendelkeznie kell az SQL Server és Azure hello alapfogalmait ismeretek. További információ: [SQL Server Books Online](https://msdn.microsoft.com/library/bb545450.aspx), [SQL Server Azure virtuális gépek](virtual-machines-windows-sql-server-iaas-overview.md) és [Azure.com webhelyre](https://azure.microsoft.com/).

Ez a cikk ismerteti, amely alkalmas a egyszerű alkalmazások, valamint a hello nagyon összetett vállalati alkalmazások több alkalmazás-minták. Minden egyes minta részletező, mielőtt azt javasoljuk, hogy tanulmányozza át hello rendelkezésre álló adatok tárolási szolgáltatásokkal, az Azure-ban, mint [Azure Storage](../../../storage/common/storage-introduction.md), [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md), és [ SQL Server Azure virtuális gép a](virtual-machines-windows-sql-server-iaas-overview.md). toomake hello legjobb kialakításakor a gazdafürttagok számára az alkalmazások megismerése mikor melyik adattárolási szolgáltatás egyértelműen toouse.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>Válassza ki az SQL Server Azure virtuális gépre, ha:
* Vezérlő SQL Server és a Windows van szüksége. Ez lehet például a hello SQL Server verziót, különleges gyorsjavítások, teljesítmény konfigurációs, stb.
* Egy teljes körű kompatibilitásra van a helyszíni SQL Server szükséges, és szeretné, hogy toomove meglévő alkalmazások tooAzure,-van.
* Azt szeretné, hogy tooleverage hello képességeit hello Azure-környezetéhez, de az Azure SQL Database nem támogatja az alkalmazás által igényelt összes hello szolgáltatás. Ez magában foglalhatja a következő területeken hello:
  
  * **Adatbázis mérete**: a cikk frissült hello időpontban SQL-adatbázis too1 TB adatot fel az adatbázis támogatja. Ha az alkalmazás által igényelt több mint 1 TB adatot, és nem szeretné, hogy tooimplement egyéni horizontális megoldások, ajánlott használt SQL Server egy Azure virtuális gépen. Hello legfrissebb információkért lásd: [Scaling Out Azure SQL-adatbázisok](https://msdn.microsoft.com/library/azure/dn495641.aspx) és [Azure SQL adatbázis szolgáltatásszintjei és Teljesítményszintjei](../../../sql-database/sql-database-service-tiers.md).
  * **HIPAA megfelelőségi**: egészségügyi ügyfelek, valamint a független szoftverszállítók (ISV) érdemes választania [SQL Server Azure virtuális gépek](virtual-machines-windows-sql-server-iaas-overview.md) helyett [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md) mert SQL Egy Azure virtuális gép a kiszolgáló által HIPAA üzleti társítsa a szerződés (BAA) vonatkozik. A megfelelőségi információk: [Microsoft Azure Trust Center: megfelelőségi](https://azure.microsoft.com/support/trust-center/compliance/).
  * **Példányszintű szolgáltatások**: jelenleg SQL-adatbázis nem támogatja a funkciókat, amelyek kívül hello adatbázis élő (például a csatolt kiszolgálók, ügynök feladatok, FileStream, Service Broker, stb.). További információkért lásd: [Azure SQL adatbázis tudnivalók és korlátozások](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## <a name="1-tier-simple-single-virtual-machine"></a>1-réteg (egyszerű): egyetlen virtuális gép
Az alkalmazás a mintában telepít az SQL Server alkalmazás- és adatbázis tooa különálló virtuális gépet az Azure-ban. hello ugyanaz a virtuális gép tartalmazza az ügyfél-/ webalkalmazás, üzleti összetevők, az adatelérési réteg és hello adatbázis-kiszolgáló. hello bemutatási, üzleti és adat-hozzáférési kód logikailag el egymástól, de fizikailag egy egyetlen kiszolgálós számítógép. A legtöbb ügyfél a alkalmazásminta kezdődhet és ezt követően azok terjessze ki további webes szerepkörök vagy a virtuális gépek tootheir rendszer hozzáadásával.

A alkalmazásminta akkor hasznos, ha:

* Egy egyszerű áttelepítési tooAzure platform tooevaluate tooperform azt szeretné, hogy hello platform vagy nem ad választ az alkalmazás követelményeinek.
* Azt szeretné, hogy az összes üzemeltetett alkalmazásrétegek hello tookeep hello azonos virtuális gép a hello azonos Azure adatközpont tooreduce hello késés rétegek között.
* Szeretné, hogy tooquickly rendelkezés fejlesztési és tesztelési környezetben rövid időszakokra.
* Azt szeretné, hogy tooperform magas terhelés munkaterhelési szintek változó, de nem szeretné, hogy tooown és sok fizikai karbantartása egy időben számítógépek mindig hello hello: tesztelése.

hello következő ábra azt mutatja be egy egyszerű helyszíni forgatókönyv, és hogyan telepítheti az engedélyezett felhőalapú megoldás egyetlen virtuális gépre az Azure-ban.

![1 szintű alkalmazásminta](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Hello üzleti réteg (üzleti logikát és adatokat érik el a összetevőket) telepítése a hello fizikai rétegének azonosnak hello bemutató réteg maximalizálhatja alkalmazás teljesítményét, kivéve, ha egy külön réteget tooscalability vagy biztonsági okok miatt kell használnia.

Mivel ez egy nagyon gyakori mintát toostart rendelkező, bizonyára hasznosnak találja a cikk az áttelepítési hasznos, ha az adatok tooyour SQL Server virtuális gép áthelyezése a következő hello: [egy adatbázis tooSQL kiszolgálót egy Azure virtuális gép áttelepítése](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3 rétegű (egyszerű): több virtuális gép
Az alkalmazás a mintában alkalmazást telepít központilag 3 szintű az Azure-ban úgy, hogy minden egyes alkalmazásszinten jelentkezett a másik virtuális gépet. Ez egy egyszerű méretezett és kibővített forgatókönyvek rugalmas környezetet biztosít. Ha egy virtuális gépet tartalmaz az ügyfél vagy webes alkalmazás, egy másikra hello a üzleti összetevőit üzemelteti, és más egy állomások hello adatbázis-kiszolgáló hello.

A alkalmazásminta akkor hasznos, ha:

* Azt szeretné, hogy tooperform összetett adatbázis alkalmazások tooAzure virtuális gépek áttelepítését.
* Azt szeretné, hogy másik alkalmazás rétegek toobe különböző régiókban található. Például előfordulhat, hogy megosztott adatbázisok, amelyek a telepített toomultiple régiók jelentéskészítési célokból.
* Azt szeretné, hogy a helyszíni toomove vállalati alkalmazások virtualizált platformok tooAzure virtuális gépek. A vállalati alkalmazások részletes tárgyalását lásd: [Mi az vállalati alkalmazás](https://msdn.microsoft.com/library/aa267045.aspx).
* Szeretné, hogy tooquickly rendelkezés fejlesztési és tesztelési környezetben rövid időszakokra.
* Azt szeretné, hogy tooperform magas terhelés munkaterhelési szintek változó, de nem szeretné, hogy tooown és sok fizikai karbantartása egy időben számítógépek mindig hello hello: tesztelése.

hello következő ábra azt mutatja be, hogy hogyan helyezhető egy egyszerű 3 szintű alkalmazás az Azure-ban úgy, hogy minden egyes alkalmazásszinten jelentkezett a másik virtuális gépet.

![3 rétegű alkalmazásminta](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

Az alkalmazás ebben a mintában van csak egy virtuális gép (VM) minden egyes rétegben. Ha több virtuális gép az Azure-ban, azt javasoljuk, hogy hozza létre a virtuális hálózatot. [Azure-beli virtuális hálózat](../../../virtual-network/virtual-networks-overview.md) hoz létre a megbízható biztonsági határ, és lehetővé teszi virtuális gépek toocommunicate egymás között keresztül hello magánhálózati IP-címet. Ezenkívül mindig győződjön meg arról, hogy csak az összes hálózati kapcsolatok használata esetén nyissa meg toohello bemutatási szint. Ha az alkalmazás mintát a következő hello hálózati biztonsági szabályok toocontrol csoporthozzáférés kezelése. További információkért lásd: [engedélyezése külső hozzáférés tooyour VM használatával hello Azure-portálon](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Hello diagramon internetes protokollokban TCP, UDP, HTTP vagy HTTPS lehet.

> [!NOTE]
> Az Azure virtuális hálózat létrehozása az ingyenesen elérhető. Azonban a hello VPN-átjáró, amely a helyi tooon van szó. Ez a díj hello, hogy mennyi ideig kapcsolat egy telepített és elérhető alapul.
> 
> 

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>2 rétegbeli és 3 szintű a bemutatási szint kibővített
Az alkalmazás a mintában a adatbázis rétegének 2 vagy 3 szintű alkalmazás tooAzure virtuális gépek úgy, hogy minden egyes alkalmazásszinten jelentkezett a másik virtuális gépet telepít. Emellett a horizontális hello bemutatási szint miatt tooincreased mennyiségű bejövő ügyfélkérelmeket.

A alkalmazásminta akkor hasznos, ha:

* Azt szeretné, hogy a helyszíni toomove vállalati alkalmazások virtualizált platformok tooAzure virtuális gépek.
* Azt szeretné, hogy a kimenő hello bemutatási szint miatt tooincreased mennyiségű bejövő ügyfélkérelmeket tooscale.
* Szeretné, hogy tooquickly rendelkezés fejlesztési és tesztelési környezetben rövid időszakokra.
* Azt szeretné, hogy tooperform magas terhelés munkaterhelési szintek változó, de nem szeretné, hogy tooown és sok fizikai karbantartása egy időben számítógépek mindig hello hello: tesztelése.
* Azt szeretné, hogy tooown is méretezhető felfelé és lefelé az igény szerinti hálózatiinfrastruktúra-környezeten.

hello következő ábra bemutatja, hogyan helyezheti hello alkalmazásrétegek több virtuális gép az Azure által hello bemutatási szint miatt a bejövő ügyfélkérelmeket tooincreased kötet kiterjesztése. Hello ábrán látható, Azure Load Balancer felelős forgalom terjesztésére el több virtuális gépre, és melyik web server tooconnect való is meghatározása. Egy terheléselosztó mögött hello webes kiszolgálók több példányára biztosítja hello hello bemutatási szint magas rendelkezésre állású.

![Alkalmazásminta - bemutatási szint kibővítési](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Ajánlott eljárások az alacsonyabb szintű 2, 3 szintű vagy n szintű mintákat, amelyek több virtuális gép egy rétegben
Javasoljuk, hogy hová kívánja helyezni, amelyek azonos hello ugyanaz a felhőalapú szolgáltatás, és a hello ugyanazon a szinten toohello hello virtuális gépek rendelkezésre állási csoport hello. Például a webkiszolgálók olyan készlete helyezze **CloudService1** és **AvailabilitySet1** és az adatbázis-kiszolgálók egy csoportja **CloudService2** és  **AvailabilitySet2**. Rendelkezésre állási készlet Azure lehetővé teszi tooplace hello magas rendelkezésre állású csomópontok külön tartalék tartományok és a frissítési tartományok.

tooleverage több Virtuálisgép-példányok a réteg tooconfigure Azure Load Balancer alkalmazásrétegek közötti szüksége. minden egyes réteg terheléselosztó tooconfigure hozzon létre egy elosztott terhelésű végpont az egyes rétegek virtuális gépeken futó külön-külön. Egy konkrét csomagot kiválasztani, előbb hozzon létre virtuális gépeket a hello ugyanaz a felhőalapú szolgáltatás. Ez biztosítja, hogy rendelkezik-e hello azonos nyilvános virtuális IP-cím. Ezután hozzon létre egy olyan végpont az egyik hello virtuális gépeket az adott réteg sablonbeállításait. Ezután rendelje hozzá hello azonos végpont toohello más virtuális gépek az adott réteg sablonbeállításait terheléselosztás. Hozzon létre egy elosztott terhelésű készlet, forgalom szét több virtuális gép, és is engedélyezése melyik csomópont tooconnect hello terheléselosztó toodetermine, ha a háttérkiszolgáló VM csomópont meghibásodik. Például egy terheléselosztó mögött hello webes kiszolgálók több példányára biztosítja hello hello bemutatási szint magas rendelkezésre állású.

Ajánlott eljárásként mindig győződjön meg arról, hogy minden hálózati kapcsolatok használata esetén nyissa meg a toohello bemutatási szint. hello bemutató réteg hello üzleti szint fér hozzá, és hogy hello üzleti szint majd fér hozzá a hello adatrétegbeli. Hogyan érik el tooallow toohello bemutató réteg további információkért lásd: [engedélyezése külső hozzáférés tooyour VM használatával hello Azure-portálon](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Vegye figyelembe, hogy az Azure Load Balancer hello hasonló tooload egy terheléselosztó működik a helyszíni környezetben. További információkért lásd: [Azure infrastruktúra-szolgáltatásokat a terheléselosztás](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Ezenkívül azt javasoljuk, állítsa be a magánhálózati a virtuális gépek számára az Azure Virtual Network használatával. Ez lehetővé teszi őket toocommunicate egymás között keresztül hello magánhálózati IP-címet. További információkért lásd: [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md).

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>2 rétegbeli és 3 szintű az üzleti szint kibővített
Az alkalmazás a mintában a adatbázis rétegének 2 vagy 3 szintű alkalmazás tooAzure virtuális gépek úgy, hogy minden egyes alkalmazásszinten jelentkezett a másik virtuális gépet telepít. Emellett érdemes toodistribute hello application server összetevők toomultiple virtuális gépek az alkalmazás toohello összetettsége miatt.

A alkalmazásminta akkor hasznos, ha:

* Azt szeretné, hogy a helyszíni toomove vállalati alkalmazások virtualizált platformok tooAzure virtuális gépek.
* Azt szeretné, hogy toodistribute hello application server összetevők toomultiple virtuális gépek az alkalmazás toohello összetettsége miatt.
* Azt szeretné, hogy toomove üzleti logika nehéz helyszíni LOB (üzleti-) alkalmazások tooAzure virtuális gépek. LOB-alkalmazások olyan kritikus fontosságú számítógépes alkalmazások, amelyek fontos toorunning vállalati, például a számviteli, emberi erőforrások (HR), bérlista, ellátásilánc és erőforrás-tervező alkalmazások.
* Szeretné, hogy tooquickly rendelkezés fejlesztési és tesztelési környezetben rövid időszakokra.
* Azt szeretné, hogy tooperform magas terhelés munkaterhelési szintek változó, de nem szeretné, hogy tooown és sok fizikai karbantartása egy időben számítógépek mindig hello hello: tesztelése.
* Azt szeretné, hogy tooown is méretezhető felfelé és lefelé az igény szerinti hálózatiinfrastruktúra-környezeten.

hello a következő ábra azt mutatja be, egy helyszíni forgatókönyv és annak engedélyezve felhőalapú megoldás. Ebben a forgatókönyvben a több virtuális gép az Azure-ban hello alkalmazásrétegek hello üzleti szint, amely tartalmaz hello üzleti logikai rétegből és az adat-hozzáférési összetevőket kiterjesztése által elhelyezni. Hello ábrán látható, Azure Load Balancer felelős forgalom terjesztésére el több virtuális gépre, és melyik web server tooconnect való is meghatározása. Egy terheléselosztó mögött hello alkalmazás kiszolgálók több példányára biztosítja az üzleti szint hello hello magas rendelkezésre állású. További információkért lásd: [ajánlott eljárások, amelyek több virtuális gép egy rétegben rétegének 2, 3 szintű vagy n szintű alkalmazás mintára](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![Az üzleti szint kibővítési alkalmazásminta](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>2 rétegbeli és 3 szintű megjelenítési és üzleti rétegek kibővített és HADR
Az alkalmazás a mintában adatbázis rétegének 2 vagy 3 szintű alkalmazás tooAzure virtuális gépek telepítése hello bemutatási szint (webkiszolgáló) elosztásával, és üzleti szint (kiszolgáló) összetevők toomultiple virtuális gépek hello. Magas rendelkezésre állású és vész-helyreállítási megoldások emellett az adatbázisokat az Azure virtuális gépeken a megvalósítása.

A alkalmazásminta akkor hasznos, ha:

* SQL Server magas rendelkezésre állási és vészhelyreállítási helyreállítási funkciók implementálásával kívánt virtualizált platformok helyszíni tooAzure toomove vállalati alkalmazások.
* Azt szeretné, hogy tooscale hello bemutatási szint és hello üzleti szint tooincreased mennyiségű bejövő ügyfélkérelmek és az alkalmazás hello összetettsége miatt.
* Szeretné, hogy tooquickly rendelkezés fejlesztési és tesztelési környezetben rövid időszakokra.
* Azt szeretné, hogy tooperform magas terhelés munkaterhelési szintek változó, de nem szeretné, hogy tooown és sok fizikai karbantartása egy időben számítógépek mindig hello hello: tesztelése.
* Azt szeretné, hogy tooown is méretezhető felfelé és lefelé az igény szerinti hálózatiinfrastruktúra-környezeten.

hello a következő ábra azt mutatja be, egy helyszíni forgatókönyv és annak engedélyezve felhőalapú megoldás. Ebben a forgatókönyvben a horizontális hello bemutatási szint és hello üzleti szint összetevői több virtuális gép az Azure-ban. Ezenkívül megvalósításához a magas rendelkezésre állási és vészhelyreállítási (HADR) helyreállítással az SQL Server-adatbázisok Azure-ban.

Egy alkalmazás több példánya különböző futó virtuális gépek ellenőrizze, hogy meg terheléselosztásának kérelmek őket. Ha több virtuális gép, van szüksége arról, hogy a virtuális gépek elérhető és egy pontján futó idő toomake. Konfigurálnia kell a terheléselosztást, ha az Azure terheléselosztó nyomon követi hello virtuális gépek állapotát, és megfelelően irányítja a bejövő hívások toohello kifogástalan működő virtuális gép csomópont. Információ tooset mentése terheléselosztás hello virtuális gépek, lásd: [Azure infrastruktúra-szolgáltatásokat a terheléselosztás](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Egy terheléselosztó mögött a web- és alkalmazáskiszolgálók kiszolgálók több példányára biztosítja hello hello magas rendelkezésre állású bemutató és business szintek.

![A kibővített és magas rendelkezésre állás](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>Ajánlott eljárások az SQL HADR igénylő alkalmazás-minták
SQL Server magas rendelkezésre állású és vész-helyreállítási megoldások Azure virtuális gépek beállításakor használata a virtuális gépek virtuális hálózat beállítása [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) megadása kötelező.  A virtuális hálózaton belüli virtuális gépek egy stabil magánhálózati IP-cím lesz a szolgáltatás leállás után is, így elkerülheti a DNS-név feloldása szükséges hello frissítés időpontja. Ezenkívül hello virtuális hálózat lehetővé teszi tooextend a helyszíni hálózati tooAzure, és a megbízható biztonsági határ létrehozása. Ha az alkalmazás rendelkezik vállalati tartomány korlátozása (mint például a Windows-hitelesítést, az Active Directory), például beállítása [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) szükséges.

A legtöbb, rendszert futtató ügyfelek éles kódban az Azure-on, elsődleges és másodlagos replikák megakadályozzák az Azure-ban.

Átfogó információkat és a magas rendelkezésre állású és vész-helyreállítással kapcsolatos bemutatók, [magas rendelkezésre álláshoz és Vészhelyreállításhoz az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>2 rétegbeli és 3 szintű Azure virtuális gépek és Felhőszolgáltatások
Alkalmazás ebben a mintában használatával kíván üzembe helyezni az alacsonyabb szintű 2 vagy 3 szintű alkalmazás tooAzure mindkét [Azure Felhőszolgáltatások](../../../cloud-services/cloud-services-choose-me.md#tellmecs) (webes és feldolgozói szerepkörök - Platform (PaaS) szolgáltatás) és [Azure virtuális gépek](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) () Infrastruktúrák (IaaS)). Használatával [Azure Felhőszolgáltatások](https://azure.microsoft.com/documentation/services/cloud-services/) hello bemutatási szint/üzleti szint és az SQL Server [Azure virtuális gépek](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hello adatok érvényben lévő korlát miatt a legtöbb alkalmazás Azure-on futó előnyös. hello oka, hogy egy, a Cloud Services számítási példány összevonása egy egyszerűen kezelhető, telepítési, figyelés és kibővített.

A Cloud Serviceshez Azure hello infrastruktúra az Ön kezeli, rendszeres karbantartást végez, javításokkal hello operációs rendszerek és a szolgáltatás és a hardveres hibák toorecover kísérletek. Ha az alkalmazás kibővített, automatikus és manuális kibővített lehetőség áll rendelkezésre a felhőszolgáltatás-projekt növelésével vagy csökkentésével hello példányok vagy az alkalmazás által használt virtuális gépek száma. Ezenkívül használhatja a helyszíni Visual Studio toodeploy az alkalmazás tooa felhőszolgáltatás-projekt az Azure-ban.

Összefoglalva a hello bemutató/üzleti szint nem kívánt tooown kiterjedt felügyeleti feladatokat, és az alkalmazás nem igényel olyan összetett beállítás a szoftver vagy hello operációs rendszer, ha Azure Felhőszolgáltatások használata. Ha az Azure SQL Database nem támogatja a keresett összes hello szolgáltatás, használni az SQL Server egy Azure virtuális gép hello adatrétegbeli. Azure Cloud Services egy alkalmazást futtat, és az Azure virtuális gépek adatainak tárolásához egyesíti hello mindkét szolgáltatás előnyeit. Részletes, lásd: hello szakasz ebben a témakörben a [összehasonlítása az Azure-ban fejlesztési stratégiák](#comparing-web-development-strategies-in-azure).

Az alkalmazás a mintában hello bemutatási szint a webes szerepkör, amely Felhőszolgáltatások összetevő hello Azure végrehajtási környezetet futtatja, és azt testreszabott webes alkalmazásprogramozási támogatja az IIS és ASP.NET tartalmaz. hello üzleti vagy háttéradatbázis réteg a feldolgozói szerepkör, amely a Cloud Services összetevő hello Azure végrehajtási környezetet futtatja, és akkor hasznos, ha általánosított fejlesztési és hajthat végre a háttérben történő feldolgozás a webes szerepkör magában foglalja. adatbázis-rétegből hello egy SQL Server virtuális gép az Azure-ban található. hello bemutatási szint és az adatbázis-rétegből hello hello kommunikációját közvetlenül vagy hello üzleti szint – feldolgozói szerepkör összetevők keresztül történik.

A alkalmazásminta akkor hasznos, ha:

* SQL Server magas rendelkezésre állási és vészhelyreállítási helyreállítási funkciók implementálásával kívánt virtualizált platformok helyszíni tooAzure toomove vállalati alkalmazások.
* Azt szeretné, hogy tooown is méretezhető felfelé és lefelé az igény szerinti hálózatiinfrastruktúra-környezeten.
* Az Azure SQL-adatbázis nem támogatja az összes hello szolgáltatás, amelyet az alkalmazás vagy az adatbázis.
* Azt szeretné, hogy tooperform magas terhelés munkaterhelési szintek változó, de nem szeretné, hogy tooown és sok fizikai karbantartása egy időben számítógépek mindig hello hello: tesztelése.

hello a következő ábra azt mutatja be, egy helyszíni forgatókönyv és annak engedélyezve felhőalapú megoldás. Ebben a forgatókönyvben helyezze a hello bemutatási szint a webes szerepkörök, a feldolgozói szerepkörök hello üzleti szint, de hello adatrétegbeli az Azure virtuális gépeken. Ezek között hello bemutatási szint több példányát futtató különböző webes szerepkörök biztosítja a tooload érkező kérések elosztása. Azure Cloud Services Azure virtuális gépekkel kombinálásával, azt javasoljuk, hogy [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) is. A [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md), akkor is stabil és állandó magánhálózati IP-címek belül ugyanaz a felhőalapú szolgáltatás a hello hello felhő. Adja meg a virtuális hálózat a virtuális gépek számára, és a felhőalapú szolgáltatások, miután indításához egymással kommunikáló hello magánhálózati IP-címet. Ezenkívül rendelkező virtuális gépek és az Azure webes vagy feldolgozói szerepkörök az azonos hello [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) alacsony késéssel és további biztonságos kapcsolatot nyújt. További információkért lásd: [Mi az a cloud service](../../../cloud-services/cloud-services-choose-me.md).

Hello ábrán látható, az Azure Load Balancer osztja el a forgalmat több virtuális gép között, és is meghatározza, mely webkiszolgáló vagy a kiszolgáló tooconnect számára. Egy terheléselosztó mögött web- és alkalmazáskiszolgálók kiszolgálók hello több példányára biztosítja hello és magas rendelkezésre állásának hello bemutatási szint hello üzleti szint. További információkért lásd: [ajánlott eljárásokat, az alkalmazás mintákra igénylő SQL HADR](#best-practices-for-application-patterns-requiring-sql-hadr).

![Alkalmazás-minták a Felhőszolgáltatásokkal.](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Egy másik módszer tooimplement alkalmazás zárolás egy összevont webes szerepkör, amely a bemutatási szint és az üzleti szint összetevők is tartalmaz, ahogy az alábbi hello toouse diagramja. A alkalmazásminta akkor hasznos, állapot-nyilvántartó tervezési igénylő alkalmazások. Mivel Azure állapot nélküli számítási csomópontok Ez a webes és feldolgozói szerepkörök, azt javasoljuk, hogy egy logikai toostore munkamenet-állapot a következő technológiák hello egyikével megvalósítása: [Azure gyorsítótárazás](https://azure.microsoft.com/documentation/services/redis-cache/), [Azure Table Storage](../../../cosmos-db/table-storage-how-to-use-dotnet.md) vagy [Azure SQL adatbázis](../../../sql-database/sql-database-technical-overview.md).

![Alkalmazás-minták a Felhőszolgáltatásokkal.](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Minta Azure virtuális gépek, az Azure SQL Database és Azure App Service (webalkalmazások)
hello elsődleges célja az, a alkalmazásminta tooshow, hogyan toocombine Azure-infrastruktúra egy szolgáltatási (IaaS) összetevőként a megoldás az Azure platform,--szolgáltatás összetevőket (PaaS). Ebben a mintában arra irányul, hogy az Azure SQL Database relációs adatok tárolására. Nem tartalmazza az SQL Server Azure virtuális gépre, amely hello szolgáltatásajánlat Azure infrastruktúra része.

Alkalmazás ebben a mintában telepít egy adatbázis-alkalmazás tooAzure hello bemutató jelezzük, és az üzleti rétegeihez hello azonos virtuális gép és az Azure SQL Database (SQL-adatbázis) kiszolgálók adatbázis eléréséhez. Hello bemutatási szint a hagyományos IIS-alapú webes megoldások segítségével valósíthatja meg. Vagy segítségével Megvalósíthat egy kombinált megjelenítési és üzleti szint [Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/).

A alkalmazásminta akkor hasznos, ha:

* Már van egy meglévő SQL adatbázis-kiszolgáló konfigurálva az Azure-ban, és szeretné tootest az alkalmazás gyorsan.
* Azt szeretné, hogy tootest hello képességek az Azure környezetben.
* Szeretné, hogy tooquickly rendelkezés fejlesztési és tesztelési környezetben rövid időszakokra.
* Lehet, hogy az üzleti logikát és adatokat hozzáférés összetevői önálló webalkalmazásból.

hello a következő ábra azt mutatja be, egy helyszíni forgatókönyv és annak engedélyezve felhőalapú megoldás. Ebben a forgatókönyvben az Azure SQL adatbázis Azure és a hozzáférési adatok egyetlen virtuális gép hello alkalmazásrétegek helyezze el.

![Vegyes alkalmazásminta](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Ha úgy dönt, hogy tooimplement egy kombinált web- és alkalmazáskiszolgálók réteg Azure Web Apps használatával, azt javasoljuk, hogy maradjon hello középső rétegbeli vagy alkalmazás szint, dinamikus kötésű kódtárakon (dll) webalkalmazás hello környezetében.

Emellett tekintse át a hello hello ajánlottak [összehasonlítása az Azure-ban webes fejlesztési stratégiák](#comparing-web-development-strategies-in-azure) szakasz Ez a cikk toolearn technikák programozásával kapcsolatos további hello végén.

## <a name="n-tier-hybrid-application-pattern"></a>N szintű hibrid alkalmazásminta
N szintű hibrid alkalmazás mintában valósítja meg az alkalmazást a helyszíni és az Azure között megoszlanak a többrétegű konfigurációk. Ezért, hozzon létre egy rugalmas és újrafelhasználható hibrid rendszer, amely módosítása, vagy adja hozzá egy konkrét csomagot kiválasztani módosítása nélkül hello más szintekre. a vállalati hálózat toohello tooextend a felhő, használhatja a [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) szolgáltatás.

A hibrid alkalmazásminta akkor hasznos, ha:

* Azt szeretné, hogy toobuild alkalmazásokat, amelyek részben hello felhőben futtatható és részben helyszínen.
* Érdemes toomigrate egy meglévő helyszíni toohello felhő néhány vagy az összes elemet.
* Azt szeretné, hogy a helyszíni virtualizált platformok tooAzure toomove vállalati alkalmazások.
* Azt szeretné, hogy tooown is méretezhető felfelé és lefelé az igény szerinti hálózatiinfrastruktúra-környezeten.
* Szeretné, hogy tooquickly rendelkezés fejlesztési és tesztelési környezetben rövid időszakokra.
* Adatbázis-alkalmazások vállalati egy költséghatékony módot tootake biztonsági mentések használni szeretne.

hello a következő ábra azt mutatja be, az n szintű hibrid alkalmazásminta, amely a helyszíni és az Azure módon történjen. Ahogy hello ábrán is látható, a helyszíni infrastruktúrát a [Active Directory tartományi szolgáltatások](https://technet.microsoft.com/library/hh831484.aspx) tartományvezérlői toosupport felhasználói hitelesítés és engedélyezés. Vegye figyelembe, hogy hello ábra azt mutatja be, egy forgatókönyv hello adatrétegbeli egyes részei lakhelyétől egy helyszíni adatközpontban mivel egyes részei hello adatrétegbeli élő Azure-ban. Attól függően, hogy az alkalmazás igényeinek több más hibrid környezetekben is létrehozható. Például előfordulhat, hogy megakadályozhatja hello bemutatási szint és hello üzleti szint rétegében egy a helyszíni környezetben, de hello adatokat az Azure-ban.

![N szintű alkalmazásminta](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

Az Azure-ban is használhatja az Active Directory önálló címtárak a szervezet számára, vagy is integrálhatja a meglévő helyszíni Active Directoryról szinkronizálva [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Hello ábrán látható, hello üzleti szint összetevők hozzáférhet toomultiple adatforrások, például túl[SQL Server az Azure-ban](virtual-machines-windows-sql-server-iaas-overview.md) keresztül saját belső IP-címnek, tooon helyszíni SQL Server kiszolgálóhoz [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md), vagy túl[SQL-adatbázis](../../../sql-database/sql-database-technical-overview.md) hello .NET-keretrendszer data provider technológiák használata. Ezen a diagramon az Azure SQL Database egy nem kötelező adatokat tároló szolgáltatás.

N szintű hibrid alkalmazásminta, a munkafolyamat megadott hello sorrendben következő hello valósíthatja meg:

1. Vállalati adatbázis alkalmazások magasabbra állítani toocloud hello segítségével toobe igénylő [Microsoft Assessment és tervezési (Leképezés) eszközkészlet](http://microsoft.com/map). hello MAP eszközkészlet leltár-és teljesítményadatokat gyűjt a számítógépről, virtualizálási tervezi, és javaslatokat nyújt kapacitás és a assessment tervezési.
2. Tervezze meg hello erőforrások és az Azure platformon, például a storage-fiókok és a virtuális gépek hello szükséges konfiguráció.
3. Hello vállalati hálózat helyszíni közötti hálózati kapcsolat beállítása és [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md). hello kapcsolat hello vállalati hálózathoz a helyszíni és az Azure virtuális gép közötti tooset hello a következő két módszer egyikével:
   
   1. Nyilvános végpontok egy virtuális gépen keresztül a helyszíni és az Azure közötti kapcsolatot létesíteni az Azure-ban. Ez a módszer egy egyszerű telepítő, és lehetővé teszi a virtuális gépen toouse SQL Server-hitelesítést. Ezenkívül a hálózati biztonsági csoport szabályok toocontrol nyilvános forgalom toohello VM beállítása. További információkért lásd: [engedélyezése külső hozzáférés tooyour VM használatával hello Azure-portálon](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   2. A kapcsolatot a helyszíni és az Azure közötti Azure virtuális magánhálózatot (VPN-t) alagúton keresztül. Ez a módszer lehetővé teszi tooextend tartományi házirendek tooa virtuális gépet az Azure-ban. Ezenkívül állítson be tűzfalszabályokat, és a virtuális gép Windows-hitelesítés használata. Azure jelenleg, biztonságos telephelyek közötti VPN és a pont-pont VPN-kapcsolatokat támogatja:
      
      * Biztonságos webhelyek kapcsolattal, a hálózati kapcsolat létrehozhat a helyszíni hálózat és a virtuális hálózat között az Azure-ban. Kapcsolódás a helyi adatok center környezet tooAzure ajánlott.
      * Biztonságos pont – hely kapcsolatot, a hálózati kapcsolatot létesíthet a virtuális hálózat az Azure-ban és a bárhol futtató önálló számítógép között. Leginkább ajánlott fejlesztési és tesztelési célokra.
      
      Információ tooconnect tooSQL Server az Azure, lásd: [csatlakozzon az SQL Server virtuális gépet az Azure tooa](virtual-machines-windows-sql-connect.md).
4. Ütemezett feladatok és a riasztásokat, amelyek a virtuális gép lemezének az Azure-ban a helyszíni adatok biztonsági mentését beállítása. További információkért lásd: [SQL Server biztonsági mentése és visszaállítása az Azure Blob Storage Service](https://msdn.microsoft.com/library/jj919148.aspx) és [biztonsági mentése és visszaállítása az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-backup-recovery.md).
5. Attól függően, hogy az alkalmazás igényeinek valósíthatja meg a következő három gyakori helyzetek hello egyikét:
   
   1. Tárolhatja a webkiszolgáló, a kiszolgáló és a kis-és nagybetűket adatok egy adatbázis-kiszolgálót az Azure-ban mivel hello bizalmas adatokat a helyszínen tárolja.
   2. Hálózati adaptere esetében megtarthatja a webkiszolgáló és a kiszolgáló helyszíni mivel hello adatbázis-kiszolgáló az Azure virtuális gépen.
   3. Megtarthatja az adatbázis-kiszolgáló, webkiszolgáló, és az alkalmazáskiszolgáló a helyszíni mivel hello adatbázis-replikák virtuális gépek Azure-ban tárolja. Ez a beállítás lehetővé teszi, hogy hello helyszíni webkiszolgálók vagy jelentéskészítési alkalmazások tooaccess hello adatbázis-replikákat az Azure-ban. Ezért a helyi adatbázisban toolower hello munkaterhelés érhet el. Azt javasoljuk, hogy ez a forgatókönyv gyakori az megvalósítása olvassa el a munkaterhelések és fejlesztési célokra. Információ az adatbázis-replikák létrehozása az Azure-ban: AlwaysOn rendelkezésre állási csoportok: [magas rendelkezésre álláshoz és Vészhelyreállításhoz az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="comparing-web-development-strategies-in-azure"></a>Az Azure-ban webes fejlesztési stratégiák összehasonlítása
tooimplement és központi telepítése egy többszintes Azure SQL Server-alapú alkalmazás, a következő két programozási módszerek hello egyikét használhatja:

* Az SQL Server Azure virtuális gépek Azure- és hozzáférés adatbázisok hagyományos webkiszolgáló (IIS - Internet Information Services) beállítása.
* Alkalmazza, és egy felhőalapú szolgáltatás tooAzure telepítése. Ezt követően győződjön meg arról, hogy a felhőalapú szolgáltatás hozzáférhet-e az SQL Server adatbázisok Azure virtuális gépeken. Egy felhőalapú szolgáltatás több webes és feldolgozói szerepkörök tartalmazhatnak.

a következő táblázat hello hagyományos webalkalmazás fejlesztése az Azure felhőalapú szolgáltatásairól és az Azure Web Apps a tekintetben tooSQL Server Azure virtuális gépek összehasonlítása biztosít. hello táblázat tartalmazza az Azure Web Apps, mert az SQL Server Azure virtuális gép lehetséges toouse adatforrásként Azure Web Apps használatával a nyilvános virtuális IP-cím vagy a DNS-nevét.

|  | Hagyományos webes fejlesztése az Azure virtuális gépek | Azure cloud Services | Az Azure Web Apps webszolgáltatási |
| --- | --- | --- | --- |
| **A helyszíni alkalmazások áttelepítése** |Meglévő alkalmazások, például-van. |Alkalmazások webes és feldolgozói szerepkörök kell. |Meglévő alkalmazások, például-önálló webes alkalmazások és webszolgáltatások gyors méretezhetőség igénylő de illeszkedik. |
| **Fejlesztési és üzembe helyezés** |A Visual Studio, WebMatrix, Visual Web Developer, WebDeploy, FTP, TFS, IIS-kezelő PowerShell. |Visual Studio, az Azure SDK-val TFS, PowerShell. Minden felhőalapú szolgáltatás telepítése a service-csomag és a konfigurációs két környezetek toowhich rendelkezik: átmeneti és üzemi. Egy felhőalapú szolgáltatás toohello átmeneti környezet tootest telepítése előtt kiosztását tooproduction. |Visual Studio, WebMatrix, Visual Web Developer, FTP, GIT, BitBucket, Codeplex webhelyen, dropbox-ba, a GitHub, Mercurial, TFS, webes telepíti, PowerShell. |
| **Felügyelet és a telepítő** |Ön felelősséggel tartozik a hello alkalmazások, adatok, tűzfalszabályok, virtuális hálózati és operációs rendszer felügyeleti feladatokat. |Ön felelősséggel tartozik hello alkalmazás, az adatok, a tűzfalszabályok és a virtuális hálózati rendszergazdai feladatok. |Ön felelősséggel tartozik hello alkalmazás és az adatok csak a felügyeleti feladatokat. |
| **Magas rendelkezésre állású és vész-helyreállítási (HADR)** |Javasoljuk, hogy virtuális gépeket helyez hello ugyanabban a rendelkezésre állási beállítása és a hello ugyanaz a felhőalapú szolgáltatás. A virtuális gépek megőrzi található hello azonos rendelkezésre állási készlet lehetővé teszi, hogy az Azure tooplace hello magas rendelkezésre állású csomópontok külön tartalék tartományok és a frissítési tartományok. Hasonlóképpen a virtuális gépek hello a ugyanazt a felhőalapú szolgáltatás lehetővé teszi a terheléselosztás és az virtuális gépek közvetlenül kommunikálhatnak egymással belül egy Azure-adatközpont hello a helyi hálózaton keresztül.<br/><br/>Ön felelősséggel tartozik valósít meg egy magas rendelkezésre állási és vészhelyreállítási helyreállítási megoldás az SQL Server Azure virtuális gépek tooavoid leállási. A támogatott HADR technológiákhoz, lásd: [magas rendelkezésre álláshoz és Vészhelyreállításhoz az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>Ön felelősséggel tartozik a saját adatok és az alkalmazás biztonsági mentése.<br/><br/>Ha hello gazdaszámítógépen hello adatközpont toohardware problémák miatt nem sikerül Azure át a virtuális gépeket. Ezenkívül lehetnek a virtuális gép tervezett leállások hello gazdaszámítógépen biztonsági vagy szoftverek frissítésekor frissítések. Ezért azt javasoljuk, hogy minden alkalmazás réteg tooensure hello folyamatos rendelkezésre állási karbantartása legalább két virtuális gépeket. Azure által nyújtott nem SLA-t egyetlen virtuális gépre. További információkért lásd: [Azure rugalmassági műszaki útmutatót](../../../resiliency/resiliency-technical-guidance.md). |Azure kezeli az alapul szolgáló hardver vagy operációs rendszer szoftver hello eredő hello hibák. Azt javasoljuk, hogy a webes vagy feldolgozói szerepkör tooensure hello magas rendelkezésre állású az alkalmazás több példánya megvalósítása. További információ: [Felhőszolgáltatások, virtuális gépek és virtuális hálózati szolgáltatásiszint-megállapodás](http://www.microsoft.com/download/details.aspx?id=38427) és [vész-helyreállítási és magas rendelkezésre állás a Azure-alkalmazások](../../../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>Ön felelősséggel tartozik a saját adatok és az alkalmazás biztonsági mentése.<br/><br/>Az adatbázisok SQL Server-adatbázisban egy Azure virtuális gép található a magas rendelkezésre állás és a vész helyreállítási megoldást tooavoid leállási végrehajtásáért felelős. Támogatott HDAR technológiákkal tekintse meg a magas rendelkezésre álláshoz és Vészhelyreállításhoz az SQL Server Azure virtuális gépeken.<br/><br/>**SQL Server adatbázis-tükrözés**: az Azure Cloud Services (webes/munkavégző szerepkörök) használja. SQL Server virtuális gépek és a felhőszolgáltatás-projekt is lehet a hello azonos Azure virtuális hálózatban. Ha SQL Server virtuális gép nincs a hello azonos virtuális hálózatban, egy SQL Server-aliast tooroute kommunikációs toohello példány az SQL Server toocreate van szüksége. Ezenkívül hello alias nevének egyeznie kell hello SQL Server nevével. |Magas rendelkezésre állású Azure feldolgozói szerepköröket, az Azure blob storage és az Azure SQL Database öröklődik. Például az Azure Storage összes blob, table és várólista adatok három replikáit tárolja. Egy időben, az Azure SQL Database tartja a három futtató adatok replikáinak – egy elsődleges másodpéldány és két másodlagos replika. További információkért lásd: [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) és [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md).<br/><br/>Ha SQL Server Azure virtuális gép adatforrásként a Azure Web Apps, vegye figyelembe, hogy az Azure Web Apps nem támogatja az Azure Virtual Network. Ez azt jelenti az összes kapcsolat a kiszolgáló virtuális gépek Azure-ban Azure Web Apps tooSQL haladjon végig a virtuális gépek nyilvános végpontja. Ez a magas rendelkezésre állású és vész-helyreállítási eljárással korlátozások okozhatja. Például hello ügyfélalkalmazást Azure-webalkalmazásokban tooSQL kiszolgálói virtuális gép csatlakoztatása az adatbázis-tükrözés nem lenne képes tooconnect toohello új elsődleges kiszolgáló adatbázis-tükrözésre van szüksége, állítsa be az Azure virtuális hálózat között az SQL Server állomás virtuális gépek Azure. Ezért használatával **SQL Server adatbázis-tükrözés** az Azure Web Apps nem támogatott jelenleg.<br/><br/>**SQL Server AlwaysOn rendelkezésre állási csoportok**: állíthat be AlwaysOn rendelkezésre állási csoportok használata az Azure Web Apps SQL Server virtuális gépek Azure-ban. De tooconfigure AlwaysOn rendelkezésre állási csoport figyelőjének tooroute hello kommunikációs toohello elsődleges replika nyilvános elosztott terhelésű végpont keresztül van szüksége. |
| **létesítmények közötti kapcsolat** |Az Azure Virtual Network tooconnect tooon-hez is használhatja. |Az Azure Virtual Network tooconnect tooon-hez is használhatja. |Azure-beli virtuális hálózat esetén támogatott. További információkért lásd: [Web Apps virtuális hálózati integráció](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/). |
| **Méretezhetőség** |Méretezett növelésével hello virtuális gépek méretét, vagy további lemezek hozzáadása a érhető el. További információ a virtuálisgép-méretek: [Azure virtuálisgép-méretek](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).<br/><br/>**Adatbázis-kiszolgáló**: kibővített adatbázis particionálási technikák és az SQL Server AlwaysOn rendelkezésre állási csoportok keresztül érhető el.<br/><br/>Az olvasási munkaterhelést használhatja [AlwaysOn rendelkezésre állási csoportok](https://msdn.microsoft.com/library/hh510230.aspx) több másodlagos csomópontot, valamint az SQL Server-replikációt.<br/><br/>A nagy írási munkaterhelések között több fizikai kiszolgálók tooprovide alkalmazás kibővített vízszintes particionálási adatok valósíthat meg.<br/><br/>Továbbá használatával Megvalósíthat egy kibővített [SQL Server-adatok függő útválasztási](https://technet.microsoft.com/library/cc966448.aspx). Az adatok függő útválasztás (DDR), particionálás hello ügyfélalkalmazást, általában a hello üzleti szint réteg mechanizmus tooimplement hello van szüksége, tooroute hello adatbázis-kérelmek toomultiple SQL Server-csomópontok. hello üzleti szint leképezéseit tartalmazza toohow hello adatok particionálása, és melyik csomópontján hello adatokat tartalmaz.<br/><br/>Méretezhető virtuális gépeken futó alkalmazások. További információkért lásd: [hogyan tooScale alkalmazás](../../../cloud-services/cloud-services-how-to-scale.md).<br/><br/>**Fontos megjegyzés**: hello **automatikus skálázás** az Azure-szolgáltatás lehetővé teszi a tooautomatically növelését, vagy csökkentse a hello az alkalmazás által használt virtuális gépek. Ez a szolgáltatás garantálja, hogy a hello végfelhasználói élmény nem érinti hátrányosan csúcsidőben, és a virtuális gépek állítsa le, ha kevés a hello igény szerint. Javasoljuk, hogy nincs megadva hello automatikus skálázási beállítás a felhőalapú szolgáltatás, ha az SQL Server virtuális gépeket tartalmaz. hello oka az, hogy hello automatikus skálázási funkció lehetővé teszi a virtuális gépen Azure tooturn, amikor hello ezt a virtuális Gépet a magasabb, mint néhány küszöbértéket, és ki egy virtuális gépet tooturn hello CPU-használat visszatér alacsonyabb, mint az. hello automatikus skálázás rendkívül hasznos állapot nélküli alkalmazások, például a webkiszolgálók, ahol a virtuális gép kezelhetik hello munkaterhelés bármely hivatkozások tooany előző állapot nélkül. Hello automatikus skálázási funkciót azonban nem akkor hasznos, állapotalapú alkalmazásokhoz, például az SQL Server, ahol csak egy példány lehetővé teszi, hogy toohello adatbázis írásakor. |Méretezett több webes és feldolgozói szerepkörök használatával érhető el. A webes és feldolgozói szerepkörök virtuálisgép-méretek kapcsolatos további információkért lásd: [Felhőszolgáltatások mérete konfigurálása](../../../cloud-services/cloud-services-sizes-specs.md).<br/><br/>Használata esetén **Felhőszolgáltatások**, több szerepkörök toodistribute feldolgozási határozza meg, és is a rugalmas méretezést az alkalmazás eléréséhez. Minden felhőalapú szolgáltatás tartalmaz egy vagy több webes szerepkörök és/vagy feldolgozói szerepköröket, mindegyiket a saját fájljainak és a konfiguráció. Is méretezett egy felhőalapú szolgáltatás növelésével hello száma (virtuális gépek). szerepkörpéldányokra a szerepkör telepítésére és lefelé méretezéshez egy felhőalapú szolgáltatást a szerepkörpéldányok számát hello csökkentésével. Részletes információkért lásd: [Azure végrehajtási modell](../../../cloud-services/cloud-services-choose-me.md).<br/><br/>Kibővített Azure magas rendelkezésre állást támogatását keresztül érhető el [Felhőszolgáltatások, virtuális gépek és virtuális hálózati szolgáltatásiszint-megállapodás](http://www.microsoft.com/download/details.aspx?id=38427) és a terheléselosztó hasonló adataival.<br/><br/>Egy többrétegű alkalmazást javasoljuk, hogy a webes vagy feldolgozói szerepkörök toodatabase kiszolgáló virtuális gépek Azure virtuális hálózaton keresztül csatlakozni. Emellett Azure terheléselosztást biztosít a virtuális gépek hello ugyanaz a felhőalapú szolgáltatás felhasználói kérelmek terjednek ezek között. Ezzel a módszerrel csatlakoztatott virtuális gépek közvetlenül kommunikálhatnak egymással belül egy Azure-adatközpont hello a helyi hálózaton keresztül.<br/><br/>Beállíthat **automatikus skálázás** hello Azure-portálon, valamint a hello ütemezés. További információkért lásd: [hogyan tooconfigure automatikus skálázás egy felhőalapú szolgáltatás hello portálon](../../../cloud-services/cloud-services-how-to-scale-portal.md). |**Felfelé és lefelé méretezési**: növelése vagy csökkentése hello példány (VM), a webhely fenntartott hello méretétől is.<br/><br/>A kibővítéshez: a webhely (VM) több fenntartott példányok is hozzáadhat.<br/><br/>Beállíthat **automatikus skálázás** hello portal, valamint a hello ütemezés. További információkért lásd: [hogyan tooScale webalkalmazások](../../../app-service-web/web-sites-scale.md). |

Ezen programozási módszerek közötti kiválasztásáról további információkért lásd: [Azure Web Apps, a Cloud Services és a virtuális gépek: amikor toouse amely](../../../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Következő lépések
További információ az Azure virtuális gépeken futó SQL Server: [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).

