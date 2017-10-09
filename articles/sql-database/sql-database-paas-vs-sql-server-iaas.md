---
title: "aaaSQL (PaaS) Database és. SQL Server hello felhőben (IaaS) virtuális gépeken |} Microsoft Docs"
description: "Ismerje meg, melyik felhőalapú SQL Server lehetőség illik az alkalmazás: Azure SQL (PaaS) Database vagy az SQL Server Azure virtuális gépeken hello felhőben."
services: sql-database, virtual-machines
keywords: "SQL Server felhő, SQL Server hello felhőben PaaS adatbázis, a felhő típusú SQL Server"
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: cjgronlund
ms.assetid: 7467f422-b77d-4b60-9cb5-0f1ec17ec565
ms.service: sql-database
ms.custom: DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: vm-windows-sql-server
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: carlrab
ms.openlocfilehash: 1b462a9a822d04dc5deb8422ed505a5d09279253
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Felhőalapú SQL Server-verzió választása: Azure SQL Database (PaaS) adatbázis vagy az Azure virtuális gépeken futó SQL Server (IaaS)
Az Azure két lehetőséget biztosít SQL Server számítási feladatok a Microsoft Azure-ban történő üzemeltetésére:

* [Az Azure SQL Database](https://azure.microsoft.com/services/sql-database/): egy SQL-adatbázis natív toohello felhő, más néven a szolgáltatás (Platformszolgáltatásos) adatbázis vagy egy szolgáltatás, szoftverek,--szolgáltatás (SaaS) alkalmazások fejlesztésére optimalizált adatbázis platformok. Ez a megoldás az SQL Server legtöbb funkciójával kompatibilis. További információ a PaaS-ről: [Mi az a PaaS?](https://azure.microsoft.com/overview/what-is-paas/)
* [SQL Server Azure virtuális gépeken](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server telepített, és az Azure, más néven az infrastruktúra (IaaS) szolgáltatás fut a Windows Server virtuális gépek (VM) hello felhőben üzemeltetett.
  Az Azure virtuális gépeken futó SQL Server a létező SQL Server-alkalmazások áttelepítésére lett optimalizálva. Összes hello verzióit, és az SQL Server kiadások érhetők el. Kínál az SQL Server, így Ön toohost 100 %-os-kompatibilitási annyi adatbázisok szükséges és végrehajtás alatt lévő adatbázisok közötti tranzakcióként. Teljes körű vezérlést biztosít az SQL Serveren és Windows rendszeren.

Ismerje meg, hogyan minden egyes beállítás hello Microsoft adatplatform illeszkedik, és lekérése súgót egyező hello leginkább megfelelő lehetőségre tooyour üzleti követelmények. Költséghatékony vagy adminisztrációs terhek csökkentését helyezi előtérbe, hogy ez a cikk segítségével eldöntheti, melyik megközelítés képes teljesíteni hello üzleti követelmények legfontosabbnak ítélt információkat.

## <a name="microsofts-data-platform"></a>A Microsoft adatplatformja
Hello első dolog toounderstand: semmiről nem kell Azure és a helyszíni SQL Server-adatbázisok egyik, hogy az összes használhatja. A Microsoft adatplatformja különböző fizikai, helyszíni gépeken, magánfelhőt alkalmazó környezetekben, külső fejlesztőtől származó magánfelhő-környezetekben és nyilvános felhőkben egyaránt lehetővé teszi az SQL Server-technológia felhasználását. Azure virtuális gépeken futó SQL Server lehetővé teszi toomeet egyedi és sokszínű üzleti igényeit helyszíni és felhőben futó üzemelő példányok, amíg ugyanazokat a kiszolgálótermékeket, fejlesztői eszközöket és szakértői használatával hello ezek között környezetekben.

   ![Felhőalapú SQL Server-verziók: SQL server IaaS vagy SaaS SQL adatbázis-hello felhőben.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Hello ábrán látható, egymástól is lehet (a hello X tengely) hello infrastruktúra feletti felügyelet mértéke hello szintű és az adatbázisszintű konszolidáció és automatizálás (a hello Y-tengely) által elérhető költségmegtakarítás mértéke hello jellemzőek.

Az alkalmazások fejlesztése során négy alapvető lehetőség áll hello alkalmazás SQL Server részének hello üzemeltetéséhez érhető el:

* SQL Server nem virtuális, fizikai gépeken
* SQL Server helyszíni virtuális gépeken (magánfelhő)
* SQL Server Azure virtuális gépeken (Microsoft nyilvános felhő)
* Azure SQL Database (Microsoft nyilvános felhő)

A következő részekben hello, megismerkedhet a SQL Server a nyilvános felhő Microsoft hello: Azure SQL Database és SQL Server Azure virtuális gépeken. Ezenfelül áttekintünk néhány általános üzleti igényt, amelyek segítségével megállapíthatja, hogy melyik verzió a legjobb az Ön alkalmazása számára.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Az Azure SQL Database és az Azure virtuális gépeken futó SQL Server részletes bemutatása
**Az Azure SQL Database** egy relációs adatbázis-a-szolgáltatás (típusú) hello Azure felhőben található, a hello ipari kategóriákba tartozik üzemeltetett *szoftver,--szolgáltatás (SaaS)* és *Platform,--szolgáltatás (PaaS)* . Az [SQL Database](sql-database-technical-overview.md) a Microsoft tulajdonában álló, illetve általa futtatott és fenntartott szabványos hardvereken és szoftvereken fut. Az SQL Database fejleszthet közvetlenül a beépített szolgáltatásai és funkciói hello szolgáltatást. SQL Database használatalapú fizetési lehetőségek tooscale vagy horizontális nagyobb bekapcsolási megszakítás nélkül a használatakor.

**SQL Server Azure virtuális gépek (VM)** hello iparági kategóriába esik *infrastruktúra,--szolgáltatás (IaaS)* , és lehetővé teszi az SQL Server toorun hello felhőben egy virtuális gépen. Adatbázis tooSQL hasonló épül, amely tulajdonban lévő, futó, és a Microsoft által fenntartott szabványos hardvereken. Az SQL Server virtuális gépen történő futtatásakor választhat, hogy használat alapján fizet egy SQL Server-rendszerképben található SQL Server-licencért, vagy egy már meglévő licencet használ. Is könnyen felfelé vagy lefelé méretezéshez és szüneteltet vagy hello virtuális gép szükség szerint.

Általánosságban elmondható, hogy ezeket az SQL-verziókat különböző célok végrehajtására optimalizálták:

* **Az Azure SQL Database** optimalizált tooreduce az általános költségek toohello minimális kiépítése és kezelése több adatbázis számára. Csökkenti a adminisztrációra fordítandó összeget, mivel nem rendelkezik toomanage bármely virtuális gépek, operációs rendszerek vagy adatbázisszoftverek. Nem rendelkezik toomanage frissítések, magas rendelkezésre állású, vagy [biztonsági mentések](sql-database-automated-backups.md). Az Azure SQL Database általában jelentősen növelhető az hello számú adatbázis kezelésére egyetlen informatikai vagy fejlesztői erőforrás.
* **Azure virtuális gépeken futó SQL Server** áttelepítése meglévő alkalmazások tooAzure vagy bővíteni a meglévő helyszíni alkalmazások toohello felhő hibrid telepítések van optimalizálva. Ezenkívül az SQL Server használja a virtuális gép toodevelop, és a hagyományos SQL Server alkalmazást. SQL Server Azure virtuális gépeken hello teljes körű rendszergazdai jogosultságokkal lehetősége egy dedikált SQL Server-példány és egy felhőalapú virtuális Gépen. Amikor egy szervezet már rendelkezik informatikai erőforrások elérhető toomaintain hello virtuális gépek is tökéletes választás. Ezek a képességek egy nagy mértékben testre szabott rendszer tooaddress toobuild lehetővé teszi az alkalmazás teljesítmény- és rendelkezésre állási követelményeinek.

a következő táblázat hello SQL-adatbázis és az Azure virtuális gépeken futó SQL Server főbb jellemzőit hello foglalja össze:

| **A következőkre alkalmas:** | **Azure SQL Database** | **SQL Server Azure virtuális gépen** |
| --- | --- | --- |
|  |Új felhőalapú alkalmazásokhoz, amelyek fejlesztésére és népszerűsítésére szűkösebb időkeret áll rendelkezésre. |Gyors áttelepítés toohello felhő legfeljebb minimális változtatásokra igénylő meglévő alkalmazásokat. Gyors fejlesztési és tesztelési forgatókönyvek, ha nem szeretné, hogy toobuy helyszíni nem üzemi SQL Server-hardvert. |
|  | Magas rendelkezésre állást, a vész-helyreállítási és a frissítés hello adatbázis igénylő csoportok. |Olyan csoportok számára, amelyek konfigurálni és kezelni tudják az SQL Server magas rendelkezésre állását, a vészhelyreállítását és frissítéseit. Mindezeket egyes beépített automatizált funkciók jelentősen leegyszerűsítik. | |
|  | Csoportok, amelyeknél nem szeretné toomanage hello alapul szolgáló operációs rendszer és a konfigurációs beállításokat. |Teljes körű rendszergazdai jogosultságokkal rendelkező testreszabott környezetre van szükség. | |
|  | Adatbázisok too4 TB be, vagy nagyobb adatbázisok, amelyek lehetnek [vízszintes vagy függőleges irányú particionálva](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) kibővített minta használatával. |SQL Server-példányokat a másolatot too64 TB tárhelyet. hello példány annyi adatbázisok igény szerint is támogatja. | |
|  | [Szoftverszolgáltatás (SaaS) típusú alkalmazásokhoz](sql-database-design-patterns-multi-tenancy-saas-applications.md). |Vállalati és hibrid alkalmazások áthelyezéséhez és létrehozásához. | |
|  | | |
| **Erőforrások:** |Nem szeretné, hogy tooemploy informatikai erőforrások konfigurálását és felügyeletét a hello az alapul szolgáló infrastruktúra, de szeretné, hogy toofocus hello alkalmazás réteg. |Ha rendelkezik bizonyos informatikai erőforrásokkal a konfigurációhoz és felügyelethez. Mindezeket egyes beépített automatizált funkciók jelentősen leegyszerűsítik. |
| **Tulajdonosi költségek:** |Nincsenek hardverköltségek és alacsonyabbak a felügyeleti költségek. |Nincsenek hardverköltségek. |
| **Az üzletmenet folytonossága** |Továbbá toobuilt a tartalék tolerancia infrastruktúra lehetőségeit, az Azure SQL Database szolgáltatásokat biztosítja, például a [biztonsági mentések automatikus](sql-database-automated-backups.md), [pont időponthoz kötött visszaállítás](sql-database-recovery-using-backups.md#point-in-time-restore), [georedundánshelyreállítás](sql-database-recovery-using-backups.md#geo-restore), és [aktív georeplikáció](sql-database-geo-replication-overview.md) tooincrease az üzletmenet folytonossága. További információk: [SQL Database business continuity overview](sql-database-business-continuity.md) (Az SQL Database üzletmenet-folytonossági funkcióinak áttekintése). |Az Azure virtuális gépeken futó SQL Server lehetőséget kínál az adatbázis konkrét igényeinek megfelelő magas rendelkezésre állási és vészhelyreállítási megoldás kialakítására. Így az adott alkalmazásra optimalizálhatja a rendszert. Szükség esetén önállóan is tesztelheti a feladatátvételt. További információk: [High Availability and Disaster Recovery for SQL Server in Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md) (Magas rendelkezésre állás és vészhelyreállítás Azure virtuális gépeken futó SQL Serveren). |
| **Hibrid felhő:** |A helyszíni alkalmazások képesek az Azure SQL Database-ben tárolt adatok elérésére. |Az SQL Server Azure virtuális gépeken akkor lehet részben hello felhőben futó alkalmazások és részben helyszínen. Például bővítheti a helyszíni hálózat és az Active Directory-tartomány toohello felhőre [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Ezenfelül az [Azure SQL Server-adatfájlok](http://msdn.microsoft.com/library/dn385720.aspx) funkciójával elhelyezheti a helyszíni adatfájlokat az Azure Storage tárhelyen. További információkért lásd: [bemutatása tooSQL Server 2014 Hybrid Cloud](http://msdn.microsoft.com/library/dn606154.aspx). |
|  | Támogatja a [SQL Server tranzakciós replikáció](https://msdn.microsoft.com/library/mt589530.aspx) előfizető tooreplicate adatokat. |Teljes mértékben támogatja [SQL Server tranzakciós replikáció](https://msdn.microsoft.com/library/mt589530.aspx), [AlwaysOn rendelkezésre állási csoportok](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), az Integration Services és a Naplóküldés tooreplicate adatokat. Emellett a hagyományos SQL Server-biztonságimásolatok is teljes körűen támogatottak. | |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Az Azure SQL Database vagy az Azure virtuális gépeken futó SQL Server használatát támogató üzleti indokok
### <a name="cost"></a>Költségek
E most, hogy a rendszer egyformán cash indítási, vagy egy meglévő vállalati szoros költségvetési megkötések miatt korlátozott támogatás alatt működő csoport gyakran hello elsődleges illesztőprogram meghatározásakor hogyan toohost az adatbázisokat. Ebben a szakaszban megtudhatja hello számlázással kapcsolatos és licencelési alapokkal az Azure-ban tekintetében toothese két relációs adatbázis-beállítások: SQL Database és SQL Server Azure virtuális gépeken. Olvashat hello az alkalmazás összköltségének kiszámítása.

#### <a name="billing-and-licensing-basics"></a>A számlázás és a licencelés alapjai
**SQL-adatbázis** toocustomers megvásárolható licenccel nem rendelkező szolgáltatásként.  Az [Azure virtuális gépeken futó SQL Server](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) mellé jár egy percalapon fizetendő licenc is. Ha már rendelkezik egy licenccel, azt is használhatja.  

Jelenleg **SQL-adatbázis** többféle szolgáltatáscsomagban kapható, amelyek amelyekért óránkénti hello szolgáltatást és teljesítményszintet szintje alapján úgy dönt, rögzített alapján érhető el. Emellett a kimenő internetes forgalom számlázása a normál [adatátviteli díjszabások](https://azure.microsoft.com/pricing/details/data-transfers/) szerint történik. Basic, Standard, Premium hello, és a prémium RS szolgáltatási rétegek vannak tervezett több teljesítmény szintek toomatch toodeliver kiszámítható teljesítmény az alkalmazás maximális követelményeinek. Szolgáltatásszintek és az alkalmazás változatos átviteli kell teljesítmény szintek toomatch között módosíthatja. Ha az adatbázis magas tranzakciós kötet és a vonatkozó igényeket toosupport nagyszámú egyidejű felhasználót, ajánlott hello prémium szolgáltatásszintet. Hello legfrissebb információk az aktuális hello támogatott szolgáltatásszintek: [Azure SQL Database Service Tiers](sql-database-service-tiers.md). Is létrehozhat [rugalmas készletek](sql-database-elastic-pool.md) tooshare teljesítmény erőforrások közötti adatbázis-példány.

A **SQL-adatbázis**, hello adatbázisszoftver konfigurálásáért automatikusan konfigurált, lett, és frissítéséért a Microsoft, ami csökkenti az adminisztratív költségeket. Ezenfelül a [beépített biztonsági mentési](sql-database-automated-backups.md) funkciókkal is jelentős költségmegtakarítást érhet el, különösen, ha nagyszámú adatbázist használ.

A **SQL Server Azure virtuális gépeken**, használhatja bármelyik hello platform által biztosított SQL Server-lemezképekben (amelyhez licenc is tartozik), vagy az SQL Server-licenc. Minden hello támogatott SQL Server-verziók (2008R2, 2012, 2014, 2016) és (fejlesztői, Express, webalkalmazás, Standard, Enterprise) kiadások érhetők el. Emellett a hello lemezképek (használata BYOL) Bring Your-saját-licencet verzióit érhetők el. Hello Azure által biztosított rendszerképek használata esetén a működési költségek hello függ hello Virtuálisgép-méretet és hello használni kívánt SQL Server kiadása. Virtuálisgép-méretet és az SQL Server olyan kiadása, függetlenül percalapú licencelési költségeit-az SQL Server és a Windows Server hello Azure tárolási költségű hello méretű lemezek együtt kell fizetnie. hello percalapú számlázásnak köszönhetően toouse SQL Server mindaddig hozzáadása SQL-kiszolgálói licencek megvásárlása nélkül van szüksége. Ha később saját SQL Server-licenc tooAzure, van szó, a Windows Server és a csak a tárolási költségeket. A saját licenc használatával kapcsolatos további információkért lásd: [License Mobility through Software Assurance on Azure](https://azure.microsoft.com/pricing/license-mobility/) (Licenchordozás az Azure Frissítési garancia programja keretében).

#### <a name="calculating-hello-total-application-cost"></a>Számítási hello alkalmazás összköltsége
Amikor a felhőplatform használatának megkezdése az alkalmazás futtatásának hello költség tartalmaz hello fejlesztési és az adminisztratív költségeket, valamint a hello nyilvános felhő platform szolgáltatási költségei.

Ez az alkalmazás SQL Database és SQL Server Azure virtuális gépeken futó részletes költségszámítás hello:

**Az Azure SQL Database használata esetében:**

*Alkalmazás összköltsége = a lehető legalacsonyabbra csökkentett adminisztrációs költségek + szoftverfejlesztési költségek + SQL Database szolgáltatási költségei*

**Az Azure virtuális gépeken futó SQL Server esetében:**

*Alkalmazás összköltsége = a szoftver fejlesztésének lehető legalacsonyabbra csökkentett költségei + adminisztrációs költségek + az SQL Server és a Windows Server licencköltségei + az Azure Storage költségei*

Az árakkal kapcsolatos további információkért tekintse meg a következő erőforrások hello:

* [SQL Database – Díjszabás](https://azure.microsoft.com/pricing/details/sql-database/)
* [Virtuális gépek – Díjszabás ](https://azure.microsoft.com/pricing/details/virtual-machines/) ([SQL-hez](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) és [Windowshoz](https://azure.microsoft.com/pricing/details/virtual-machines/#windows))
* [Azure díjkalkulátor](https://azure.microsoft.com/pricing/calculator/)

> [!NOTE]
> Az SQL Server funkcióinak egy szűk köre nem használható az SQL Database-ben. További információért lásd az [SQL Database funkcióit](sql-database-features.md) és az [SQL Database Transact-SQL információit](sql-database-transact-sql-information.md) ismertető részeket. Ha egy meglévő SQL Server megoldást toohello felhőben, lásd: [egy SQL Server adatbázis tooAzure SQL-adatbázis áttelepítése](sql-database-cloud-migrate.md). Amikor telepít át egy meglévő helyszíni SQL Server alkalmazás tooSQL adatbázis, fontolja meg hello alkalmazás tootake felhőszolgáltatások által kínált hello képességek előnyeit. Például akkor lehet érdemes [Azure Web App Service](https://azure.microsoft.com/services/app-service/web/) vagy [Azure Felhőszolgáltatások](https://azure.microsoft.com/services/cloud-services/) toohost az alkalmazás réteg tooincrease költség előnyeit.
> 
> 

### <a name="administration"></a>Adminisztráció
Számos vállalat számára hello döntési tootransition tooa felhőszolgáltatás esetén, mert sokkal, mert az Adminisztráció összetettsége ösztönzi csupán a költségszempontok. A **SQL-adatbázis**, a Microsoft felügyeli az alapul szolgáló hardver hello. A Microsoft automatikusan replikálja az összes adat tooprovide magas rendelkezésre állású, konfigurálja és hello adatbázisszoftver konfigurálásáért frissíti, kezeli a terheléselosztás és does transzparens feladatátvétel, ha egy kiszolgáló sikertelen. Továbbra is tooadminister az adatbázis, de már nem szükséges toomanage hello adatbázis-kezelő, a kiszolgáló operációs rendszer vagy a hardver.  Például a következő elemek továbbra is tooadminister adatbázisok és bejelentkezések, index és lekérdezés hangolása, és a naplózás és a biztonsági tartalmazza.

A **SQL Server Azure virtuális gépeken**, hello operációs rendszer és az SQL Server-példány konfigurációja teljes hozzáféréssel rendelkeznek. Virtuális gépen, akkor működik tooyou toodecide tooupdate/frissítése hello operációs rendszer és az adatbázis szoftver, és ha tooinstall például a víruskereső további szoftvereket. Bizonyos automatikus szolgáltatások akkor megadott toodramatically egyszerűsítése javítási, a biztonsági mentési és a magas rendelkezésre állás. Ezenkívül azt is szabályozhatja hello hello virtuális gép méretétől, hello lemezek számát és a tárhely konfigurációját. Azure lehetővé teszi a virtuális gép, igény szerint toochange hello méretét. További információk: [Virtual Machine and Cloud Service Sizes for Azure](../virtual-machines/windows/sizes.md) (Virtuális gépek és felhőszolgáltatások mérete az Azure-ban). 

### <a name="service-level-agreement-sla"></a>Szolgáltatói szerződés (SLA)
Számos számítástechnikai osztály számára elsődleges prioritást jelent a szolgáltatói szerződésben (SLA) vállalt üzemidő biztosítása. Ebben a szakaszban úgy tekintünk, milyen SLA vonatkozik tooeach adatbázis-üzemeltetési megoldásokra.

Az **SQL Database** Alapszintű, Standard, Prémium és Prémium RS szolgáltatáscsomagja esetében a Microsoft 99,99%-os SLA-elérhetőséget garantál. Hello legfrissebb információkért lásd: [szolgáltatásiszint-megállapodás](https://azure.microsoft.com/support/legal/sla/sql-database/). Hello legújabb SQL Database szolgáltatási szinteket és hello támogatott üzleti folytonossági tervek kapcsolatos tudnivalókat lásd: [szolgáltatásszintek](sql-database-service-tiers.md).

A **Azure virtuális gépeken futó SQL Server**, hogy magában foglalja az imént hello virtuális gép biztosít a Microsoft 99,95 %-os SLA-elérhetőséget garantál. Az SLA nem fedi le hello folyamatok (például az SQL Server) hello virtuális gép futó, és megköveteli, hogy egy rendelkezésre állási csoportnak legalább két Virtuálisgép-példány működteti. Hello legfrissebb információkért lásd: hello [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Adatbázis magas rendelkezésre ÁLLÁS virtuális gépeken, úgy kell konfigurálnia egy hello támogatott magas rendelkezésre állású lehetőség az SQL Server, például a [AlwaysOn rendelkezésre állási csoportok](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx). A támogatott magas rendelkezésre állású beállítással nem biztosít további SLA-t, de lehetővé teszi a tooachieve > adatbázis rendelkezésre állás 99,99 %.

### <a name="market"></a>Idő toomarket
**SQL-adatbázis** akkor hello ideális megoldás a felhőalapú alkalmazásokhoz, ha a fejlesztés és a gyors idő piacra jutási kritikus. Programozott DBA hasonló funkciójú tökéletes felhőben dolgozó tervezők és fejlesztők számára, csökkenti a hello kell hello alapul szolgáló operációs rendszer és adatbázis felügyeletére. Például használhatja hello [REST API](http://msdn.microsoft.com/library/azure/dn505719.aspx) és [PowerShell-parancsmagok](http://msdn.microsoft.com/library/mt740629.aspx) tooautomate és akár több ezer adatbázis felügyeleti műveleteinek a felügyeletét. Szolgáltatások, például a [rugalmas készletek](sql-database-elastic-pool.md) lehetővé teszik a hello alkalmazásréteg toofocus, és a megoldás toohello piaci gyorsabban biztosításához.

**Azure virtuális gépeken futó SQL Server** tökéletes, ha a meglévő vagy új alkalmazásainak igényelnek nagy adatbázisok esetén egymáshoz adatbázisok vagy SQL Server vagy Windows tooall funkciók eléréséhez. Egyúttal remekül beválik, ha azt szeretné, hogy a toomigrate meglévő helyszíni alkalmazásokat és adatbázisokat tooAzure,-van. Mivel toochange hello bemutató, alkalmazási és adatréteg nem szükséges, időt és pénzt takarít költségvetési az, amit a meglévő megoldást. Ehelyett összpontosíthat a megoldások tooAzure áttelepítéssel, és ennek során néhány teljesítményoptimalizálás hello Azure platform által esetlegesen szükséges. További információk: [Performance Best Practices for SQL Server on Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md) (Teljesítményre vonatkozó ajánlott eljárások az Azure Virtual Machines szolgáltatásban futtatott SQL Server esetében).

## <a name="summary"></a>Összefoglalás
Ebben a cikkben bemutattuk az SQL Database-t és az Azure virtuális gépeken futó SQL Servert, és szót ejtettünk azokról az üzleti szempontokról, amelyek hatással lehetnek választására. hello az alábbiakban látható, tooconsider javaslatokat összefoglalása:

Válassza az **Azure SQL Database-t**, ha:

* Biztosan épület új felhőalapú alkalmazásokat hello költség megtakarítások és a teljesítmény optimalizálása, hogy a felhőalapú szolgáltatások kihasználása tootake adja meg. Ezt a módszert használja egy teljes körűen felügyelt felhőszolgáltatás hello előnyökkel, alacsonyabb kezdeti idő piacra jutási segít, és hosszú távú költség optimalizálási biztosít.
* Azt szeretné, hogy a toohave Microsoft az adatbázisok általános felügyeleti műveleteinek elvégzését és erősebb rendelkezésre állási SLA-k adatbázisokhoz.

Válassza az **Azure virtuális gépeken futó SQL Servert**, ha:

* Meglévő helyszíni alkalmazásokat szeretné, hogy toomigrate, vagy kiterjesztése toohello felhő, vagy ha azt szeretné, toobuild vállalati alkalmazások 4 TB-nál nagyobb. Ez a megközelítés hello előnye, hogy a 100 %-os SQL kompatibilitási, nagy adatbázis kapacitás, SQL Server és a Windows és a biztonságos bújtatás tooon helyszíni teljes hozzáférést biztosít. Ez a verzió a lehető legalacsonyabbra csökkenti a meglévő alkalmazások fejlesztési és átalakítási költségeit.
* Meglévő számítógépes erőforrások használatával Ön irányíthatja a frissítéseket, a biztonsági mentéseket és az adatbázisok magas rendelkezésre állását. Megjegyzendő, hogy egyes automatizált funkciók jelentősen leegyszerűsítik ezeket a műveleteket. 

## <a name="next-steps"></a>Következő lépések
* Lásd: [az első Azure SQL Database](sql-database-get-started-portal.md) tooget használatába SQL-adatbázis.
* [SQL Database – Díjszabás](https://azure.microsoft.com/pricing/details/sql-database/).
* Lásd: [egy SQL Server rendszerű virtuális gép az Azure-ban](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md) tooget lépések az SQL Server Azure virtuális gépeken.

