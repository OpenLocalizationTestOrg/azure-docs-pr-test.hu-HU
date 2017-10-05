---
title: "Az Azure infrastruktúra-szolgáltatási lemezek a biztonsági mentés és katasztrófa-helyreállítás |} Microsoft Docs"
description: "Ebben a cikkben ismertetjük a biztonsági mentés és katasztrófa utáni helyreállítás (DR) az infrastruktúra-szolgáltatási virtuális gépeket (VM) tervezéséről és a lemezek az Azure-ban. Ez a dokumentum ismerteti a felügyelt és a nem felügyelt lemezek"
services: storage
cloud: Azure
documentationcenter: na
author: luywang
manager: kavithag
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: luywang
ms.openlocfilehash: 0de675e46dbd527148ac5fedcd35d81962adfb7e
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="backup-and-disaster-recovery-for-azure-iaas-disks"></a>Az Azure infrastruktúra-szolgáltatási lemezek a biztonsági mentés és katasztrófa-helyreállítás

Ebben a cikkben ismertetjük a biztonsági mentés és katasztrófa utáni helyreállítás (DR) az infrastruktúra-szolgáltatási virtuális gépeket (VM) tervezéséről és a lemezek az Azure-ban. Ez a dokumentum ismerteti a felügyelt és a nem felügyelt lemezek.

Lesz először döntésről bővebben a beépített tartalék tolerancia képességek az Azure platformon, amely segítségével védelmet biztosítanak a helyi hibák. Mutatjuk a vész-eljárással nem teljes mértékben a beépített képességei, majd, ami témája a fő ebben a dokumentumban tárgyalt. Néhány példa munkaterhelés forgatókönyvek, ahol különböző biztonsági mentési és vész-Helyreállítási feltételek vonatkoznak, előfordulhat, hogy is mutat. Azt majd áttekinti a lehetséges megoldásokról az infrastruktúra-szolgáltatási lemezek vész-Helyreállítási. 

## <a name="introduction"></a>Bevezetés

Az Azure platformon segítséget nyújtanak a honosított hardver hibák merülnek fel a redundancia és a hibatűrés különböző módszereket használ. Helyi hibák tartalmazhatják egy tároló virtuális lemez számára az adatok egy részét vagy az SSD és HDD sikertelen ezen a kiszolgálón az Azure storage server gépével kapcsolatos probléma. Ilyen elkülönített hardver összetevőinek meghibásodása esetén fordulhat elő, a normál működés során, és a platform az célja, hogy ezek a hibák rugalmasak lehetnek. Jelentős katasztrófa hibák vagy a tároló kiszolgálók vagy a teljes adatközpont nagy számú inaccessibility eredményezhetnek. Miközben a virtuális gépek és a lemezek megfelelően védettek a helyi hibák, további lépések szükségesek a számítási feladatok a meghibásodásával szembeni védelemhez régió kiterjedő katasztrofális (például egy jelentős katasztrófa), amelyek hatással lehetnek a virtuális gép és a lemezek.

Platform hibák lehetőségét, valamint az ügyfél alkalmazáshoz vagy adatok hibák léphetnek fel. Például az alkalmazás új verziójának véletlenül tehet egy friss, módosítsa az adatokat. Ebben az esetben érdemes lehet visszaállítani az alkalmazás és az adatokat a legutóbbi ismert helyes állapotát tartalmazó előzetes verzióra. Ehhez a rendszeres biztonsági mentések karbantartása.

Regionális katasztrófa utáni helyreállítás biztonsági másolatot az infrastruktúra-szolgáltatási virtuális gépek lemezei egy másik régióban. 

Mielőtt úgy tekintünk, biztonsági mentési és vész-Helyreállítási beállítások, most recap néhány módszer áll rendelkezésre helyi hibák kezelése.

### <a name="azure-iaas-resiliency"></a>Az Azure IaaS rugalmasság

*Rugalmasság* normál hardverösszetevők az előforduló hibák tolerancia hivatkozik. Rugalmasságra helyreállni hibák után, és továbbra is működik. Nincs kapcsolatos hibák elkerülése, de válaszol a hibák úgy, hogy elkerülhető az állásidő és adatvesztés. A rugalmasság célja térjen vissza az alkalmazást a hiba teljes mértékben működő állapotú. Az Azure virtuális gépek és a lemezek úgy tervezték, hogy közös hardver hibákra rugalmasak lehetnek. Ossza meg velünk keresse meg, hogy az Azure infrastruktúra-szolgáltatási platform ez rugalmasságot biztosít.

A virtuális gépek általában két részből áll: (1) A kiszolgálón, és (2) az állandó lemezek számítási. A hibatűrés egy virtuális gép is érinti.

Ha az Azure compute gazdagép-kiszolgálón, amely a virtuális gép hardverhiba (amely ritka), Azure célja, hogy automatikusan állítsa helyre a virtuális Gépet, egy másik kiszolgálón. Ez akkor fordul elő, ha újraindítás fog tapasztalni, és a virtuális gép biztonsági mentése némi várakozás után lesz. Azure automatikusan észleli az ilyen hardverhibák, és végrehajtja a helyreállítások érdekében a lehető legrövidebb időn belül az ügyfél virtuális gép lesz elérhető.

Infrastruktúra-szolgáltatási lemezekre vonatkozó adatok tartósságát fontos állandó tároló platform. Azure-ügyfél rendelkezik infrastruktúra-szolgáltatáson futó üzleti alkalmazások és a megőrzése függenek. Azure tervek védelmét a három redundáns másolatok IaaS lemezeket helyileg tárolt adatok biztosít magas tartósság helyi hibák ellen. Ha egy a hardverösszetevők, amely tárolja a lemez meghibásodik, a virtuális gép nem változik, mert a lemez kérelmek támogatásához két további példányban. Jól működik akkor is, ha egy lemez támogató két különböző hardverösszetevők (amely általában nagyon ritkán) egyszerre sikertelen. Győződjön meg arról, hogy mindig karbantartása három replikák érdekében az Azure Storage szolgáltatás automatikusan indít az adatokat egy új példányát, ha az egyik három példány nem érhető el. Ezért azt nem kell RAID használható Azure-lemezeket a hibatűrés szükséges. Egy egyszerű RAID 0 konfigurációs elegendő-e a lemezek szétosztott, ha nagyobb kötetek létrehozásához szükséges kell lennie.

Ebben az architektúrában miatt **Azure következetesen rendelkezik kézbesíteni a vállalati szintű tartósságot, az infrastruktúra-szolgáltatási lemezek, egy iparágvezető a nulla % [Annualized hibaaránya](https://en.wikipedia.org/wiki/Annualized_failure_rate).**

Honosított hardver hibáit a számítási a gazdagép, vagy a tárolási platform is néha elérhetetlenség eredményez a virtuális gép hatálya alá a [Azure garantált szolgáltatási szintje](https://azure.microsoft.com/support/legal/sla/virtual-machines/) a virtuális gép rendelkezésre állás érdekében. Azure iparágvezető szolgáltatásiszint-szerződésben garantált is biztosít, a prémium szintű Storage lemezeket használó egyetlen Virtuálisgép-példányok.

Állásidő egy lemezt vagy a virtuális gép ideiglenes elérhetetlensége miatt az alkalmazások és szolgáltatások biztosításához, kihasználhatják a felhasználók [rendelkezésre állási készletek](../virtual-machines/windows/manage-availability.md). Két vagy több virtuális gépek rendelkezésre állási csoport számára biztosít redundanciát az alkalmazáshoz. Azure majd hoz létre a virtuális gépek és a lemezek külön tartalék tartományok különböző tápellátáshoz, a hálózati és a kiszolgáló-összetevők. Ebből kifolyólag honosított hardver meghibásodása általában nem befolyásolják a készlet több virtuális gép egyszerre, magas rendelkezésre állás biztosítása az alkalmazáshoz. Megállapították, célszerű rendelkezésre állási készletek használata, ha magas rendelkezésre állású szükség. További információkért lásd: a vész-helyreállítási szempontok részletes alatt.

### <a name="backup-and-disaster-recovery"></a>Biztonsági mentés és katasztrófa utáni helyreállítás

Vészhelyreállítás (DR) a ritka, de jelentős események esetén lehetősége: nem átmeneti, szinten hibák, például egy teljes terület érintő üzemkimaradás időtartamát. Vész-helyreállítási magában foglalja az adatok biztonsági mentése és az archiválásra, és előfordulhat, hogy tartalmazza a manuális beavatkozásra, például egy adatbázis biztonsági másolatból történő visszaállítását.

Az Azure platformon beépített védelmet biztosít a helyi hibák lehetséges, hogy nem teljes védelme érdekében a virtuális gépek/lemezek esetén jelentős katasztrófa, amelyek nagy méretű kimaradások esetén. Ez magában foglalja a katasztrofális esemény, például egy Hurrikán, földrengés, tűz vagy nagy méretű méretezési egység hardverhibák alatt kattintson az adatközpontban. Ezenkívül alkalmazás vagy adatokkal kapcsolatos problémák miatt hibák fordulhatnak.

A kimaradások az IaaS munkaterhelések védelme érdekében meg kell terveznie a redundancia és a biztonsági mentések történő helyreállítás engedélyezéséhez. A vészhelyreállítás esetében meg kell terveznie a redundancia és a különböző földrajzi helyen elhagyja az elsődleges hely biztonsági mentése. Ez biztosítja a biztonsági másolat nem érinti, amely a virtuális gép vagy lemezek eredetileg érintett ugyanarra az eseményre. További információkért lásd: [alkalmazások vész-helyreállítási épülő Azure](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications).

A vész-Helyreállítási szempontokat is figyelembe venni a következő szempontokat:

1. Magas rendelkezésre állású (HA) azt a képességet az alkalmazás állapota kifogástalan, jelentős állásidőt nélkül folytatódik. "Kifogástalan állapot," által azt jelenti, az alkalmazás működik-e, és a felhasználók az alkalmazás csatlakozhat, és használhatja őket. Egyes kritikus fontosságú alkalmazások és adatbázis elérhető legyen mindig, még akkor is, ha a hibák vannak a platform szükség lehet. A munkaterhelések esetén szükség lehet tervezze meg az alkalmazás, valamint az adatok redundancia.

2. Adatok tartóssága: Bizonyos esetekben a fő szempont van biztosítva az adatok megmaradjanak katasztrófa esetén. Ezért szükség lehet egy másik helyen az adatok biztonsági másolata. Az ilyen munkaterhelések esetén nem szükség lehet az alkalmazás, de a lemezek rendszeres biztonsági másolatot a teljes redundanciát.

## <a name="backup-and-dr-scenarios"></a>Biztonsági mentési és vész-Helyreállítási forgatókönyvek

Vizsgáljuk meg néhány gyakori példák alkalmazás munkaterhelés forgatókönyveket és vész-helyreállítási tervezésével kapcsolatos szempontokat.

### <a name="scenario-1-major-database-solutions"></a>1. forgatókönyv: Fő adatbázis megoldások

Vegye figyelembe például az SQL Server vagy Oracle, amely támogatja a magas rendelkezésre állású éles adatbázis-kiszolgálót. Kritikus termelési alkalmazások és a felhasználók ezt az adatbázist függ. A vész-helyreállítási terv, a rendszer a következő követelmények támogatására lehet szükség:

1. Az adatok védett és helyreállítható kell lennie.
2.  Lehet, hogy a kiszolgáló használható.

Elképzelhető, hogy szükség az adatbázis biztonsági mentéséhez egy másik régióban karbantartása. Attól függően, hogy a kiszolgáló rendelkezésre állása és az adatok helyreállítása a követelményei a megoldás az aktív-aktív vagy aktív-passzív replika hely terjedhet rendszeres offline biztonsági mentést az adatok számára. Például az SQL Server és Oracle relációs adatbázisok replikációs különböző lehetőségeket biztosítanak. Az SQL Server [SQL Server Always On rendelkezésre állási csoportok](https://msdn.microsoft.com/library/hh510230.aspx) magas rendelkezésre állás is használható.

MongoDB hasonló NoSQL-adatbázisok is támogatja a [replikák](https://docs.mongodb.com/manual/replication/) a redundancia érdekében. A magas rendelkezésre állás a replikák is használható.

### <a name="scenario-2-a-cluster-of-redundant-vms"></a>2. forgatókönyv: Egy fürt redundáns virtuális gépek

Érdemes lehet a munkaterhelés, a fürt virtuális gépeket, a redundanciát és terheléselosztást kezeli. Egy példa erre egy Cassandra fürt üzembe helyezett egy régiót. Az ilyen típusú architektúra már biztosít magas szintű redundancia adott régión belül. Azonban a munkaterhelések védelme a regionális szintű meghibásodása, vegye figyelembe a fürt két régióban is terjednek vagy rendszeres biztonsági mentést tételére egy másik régióban.

### <a name="scenario-3-iaas-application-workload"></a>3. forgatókönyv: Infrastruktúra-szolgáltatási alkalmazás munkaterhelés

Ennek oka lehet egy tipikus környezet munkaterhelés egy Azure virtuális gépen. Elképzelhető például, egy webkiszolgáló vagy a fájlkiszolgáló a tartalom és a hely más erőforrások. Egy egyedi üzleti alkalmazás, amely az adatok, erőforrások és alkalmazásállapot lemezen tárolja a a virtuális gép egy virtuális gépen is lehet. Ebben az esetben fontos ellenőrizze, hogy rendszeresen a biztonsági mentések igénybe vehet. Biztonsági mentési gyakoriság alapján a virtuális gép munkaterhelés jellegét. Például ha az alkalmazás naponta fut, és módosítja az adatokat, majd a biztonsági mentést kell végezni minden órában.

Egy másik példa a jelentéskészítő kiszolgáló más forrásokból származó adatok lekéréses és összesített jelentéseket hoz létre. A virtuális gép vagy a lemezek a jelentések elvesztését eredményezi. Azonban lehet futtassa újra a jelentéskészítési folyamat és a kimenet újragenerálása. Ebben az esetben valóban nincs adatvesztést akkor is, ha a jelentéskészítő kiszolgáló egy olyan vészhelyzet esetén a találati, így magasabb szintű tűréshatár elvesztése a jelentéskészítő kiszolgálón az adatok egy részét sikerült kell. Ebben az esetben ritkábban biztonsági mentések lehetősége arra a költségek csökkentése.

### <a name="scenario-4-iaas-application-data-issues"></a>4. forgatókönyv: Az infrastruktúra-szolgáltatási alkalmazás adatok kapcsolatos problémákat

Van olyan alkalmazás, amely kiszámítja, tart fenn és kritikus kereskedelmi forgalomban beszerezhető adatok, például a díjszabásról szolgál. Az alkalmazás új verziójának szoftver programhiba helytelenül az árképzés számított, és a meglévő kereskedelmi adatok, a platform által kiszolgált sérült volt. Állítsa vissza a korábbi verzióra, az alkalmazás és az adatok itt lenne, a helyes intézkedéseket. Ehhez igénybe vehet a rendszer rendszeres biztonsági mentést.

## <a name="disaster-recovery-solution-azure-backup-service"></a>Vész-helyreállítási megoldást: Azure biztonsági mentési szolgáltatás

[Az Azure Backup szolgáltatás](https://azure.microsoft.com/services/backup/) biztonsági mentési és vész-Helyreállítási is használható, és működik együtt a [kezelt lemezek](storage-managed-disks-overview.md) , valamint [nem felügyelt lemezek](storage-about-disks-and-vhds-windows.md#unmanaged-disks). Az idő-alapú biztonsági mentések, könnyű VM-helyreállítás és biztonsági másolatok megőrzésének házirendek segítségével hozhat létre egy biztonsági mentési feladat. 

Ha [prémium szintű Storage lemezek](storage-premium-storage.md), [kezelt lemezek](storage-managed-disks-overview.md), vagy más lemeztípusok esetén a a [helyileg redundáns tárolás (LRS)](storage-redundancy.md#locally-redundant-storage) beállítást, különösen fontos ki rendszeres vész-Helyreállítási biztonsági mentést. Az Azure Backup a Recovery Services-tároló, a hosszú távú megőrzési tárolja az adatokat. Válassza ki a [georedundáns tárolás (GRS)](storage-redundancy.md#geo-redundant-storage) beállítás megadása a biztonsági mentés Recovery Services-tároló. Amely biztosítja a biztonsági mentések replikálva vannak a regionális katasztrófák megóvása egy másik Azure-régiót.

A [nem felügyelt lemezek](storage-about-disks-and-vhds-windows.md#unmanaged-disks), az infrastruktúra-szolgáltatási lemezek az LRS tárolási típust használja, de győződjön meg arról, hogy az Azure Backup szolgáltatás engedélyezve van-e a Georedundáns beállítással a Recovery Services-tároló.

**Ha használja a [Georedundáns](storage-redundancy.md#geo-redundant-storage)/[RA-GRS](storage-redundancy.md#read-access-geo-redundant-storage) beállítás megadása után még a nem felügyelt lemezek kell alkalmazáskonzisztens pillanatképeket vonatkozó biztonsági mentési és vész-Helyreállítási.** Egyikét kell használnia [Azure Backup szolgáltatás](https://azure.microsoft.com/services/backup/) vagy [alkalmazáskonzisztens pillanatképek](#alternative-solution-consistent-snapshots).

A következő egy vész-Helyreállítási megoldások összegzését.

| Forgatókönyv | Automatikus replikáció | Vész-Helyreállítási megoldás |
| --- | --- | --- |
| *Prémium szintű Storage-lemezekhez* | Helyi ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Managed Disks* | Helyi ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Nem felügyelt LRS-lemezek* | Helyi ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Nem felügyelt Georedundáns lemezek* | Több régióban ([Georedundáns](storage-redundancy.md#geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Alkalmazáskonzisztens pillanatképek](#alternative-solution-consistent-snapshots) |
| *Nem felügyelt RA-GRS-lemezek* | Több régióban ([RA-GRS](storage-redundancy.md#read-access-geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Alkalmazáskonzisztens pillanatképek](#alternative-solution-consistent-snapshots) |

Magas rendelkezésre állású, legjobban érheti kezelt lemezeken rendelkezésre állási csoport együtt az Azure Backup segítségével. Nem felügyelt lemezt használ, ha a vész-Helyreállítási továbbra is használhatja a Azure Backup szolgáltatásnál. Ha nem használható az Azure biztonsági mentés, majd véve [alkalmazáskonzisztens pillanatképek](#alternative-solution-consistent-snapshots) leírtak szerint egy későbbi szakasz ismerteti a biztonsági mentési és vész-Helyreállítási alternatív megoldást-e.

A választott a magas rendelkezésre állás érdekében biztonsági mentési és vész-Helyreállítási alkalmazást vagy az infrastruktúra szintjén, az alábbiakban ábrázolhatók:

| *Szint* | Magas rendelkezésre állás   | Biztonsági mentés / DR |
| --- | --- | --- |
| *Alkalmazás* | Always On SQL | Azure Backup |
| *Infrastruktúra*  | Rendelkezésre állási csoport  | Az alkalmazáskonzisztens pillanatképek Georedundáns |

### <a name="using-the-azure-backup-service"></a>Az Azure biztonsági mentési szolgáltatás használatával

[Azure biztonsági mentés](../backup/backup-azure-vms-introduction.md) tud biztonsági másolatot készíteni a Windows vagy Linux rendszerű futtató az Azure Recovery Services-tároló virtuális gépek. Biztonsági mentése és visszaállítása az üzleti szempontból kritikus fontosságú adatok bonyolítja van arra, hogy üzleti szempontból kritikus fontosságú adatok kell készülni, az alkalmazások az adatokat előállító működése közben. Orvoslása érdekében Azure biztonsági mentés alkalmazáskonzisztens biztonsági mentések Microsoft-munkaterhelések segítségével biztosítja a kötet árnyékmásolata szolgáltatás (VSS) annak érdekében, hogy adatot ír megfelelően tároló. Linux virtuális gépekhez csak a fájlkonzisztens biztonsági mentések is előfordulhatnak, Linux nem tartozik funkció egyenértékű VSS-t.

![][1]

Azure biztonsági mentési indít el a biztonsági mentési feladatot a megadott időpontban, amikor elindítja a biztonságimásolat-kiterjesztés telepítése pont időponthoz kötött pillanatképet készít a virtuális gépen. A rendszer pillanatképet készít VSS együtt a lemezek konzisztens pillanatképének beolvasása a virtuális gép nem kell leállítani. A biztonsági mentési bővítményt a virtuális gép összes írása előtt a konzisztens pillanatképének készítése a lemezek kiürítése. Miután a rendszer pillanatképet készít, az adatátvitel Azure Backup szolgáltatás a biztonsági mentési tárolóba. A biztonsági mentési folyamat hatékonyabbá teheti, hogy a szolgáltatás azonosítja, és csak az adatok utolsó biztonsági mentés óta módosult blokkok átvitele.


Visszaállításához a rendelkezésre álló biztonsági mentések Azure Backup segítségével tekintheti meg és majd indítsa el a visszaállítást. Hozhat létre és állítsa vissza az Azure biztonsági mentések keresztül a [Azure-portálon](https://portal.azure.com/), [PowerShell-lel](../backup/backup-azure-vms-automation.md ), vagy használja a [Azure CLI](https://docs.microsoft.com/azure/xplat-cli-install). További információkért tekintse meg az Azure biztonsági mentés.

### <a name="steps-to-enable-backup"></a>Biztonsági mentés engedélyezésének lépései

Az alábbi lépéseket általában használt biztonsági mentéseket a virtuális gépek használata a [Azure Portal](https://portal.azure.com/). A pontos forgatókönyvtől függően változata lesz. Tekintse meg a [Azure biztonsági mentés](../backup/backup-azure-vms-introduction.md) teljes dokumentációjában. Azure biztonsági mentés is [támogatja a felügyelt lemezzel rendelkező virtuális gépek](https://azure.microsoft.com/blog/azure-managed-disk-backup/).

1.  A recovery services-tároló létrehozása a virtuális gépek az alábbi lépéseket követve:

    a.  Használja a [Azure-portálon](https://portal.azure.com/), keresse meg az összes erőforrást és a "Recovery Services-tárolók" található.

    b.  A Recovery Services-tárolók menüben kattintson a Hozzáadás gombra, és a lépéseket követve létrehozhat egy új tárolót a virtuális gép ugyanabban a régióban. Például ha a virtuális gép USA nyugati régiója régióban, válasszon USA nyugati régiója a tároló.

2.  Ellenőrizze a tárolási replikáció, az újonnan létrehozott tároló. Ehhez a Recovery Services-tárolók panel hozzáférést a tárolóhoz, és navigáljon a beállítások/biztonsági konfiguráció. Győződjön meg arról, a Georedundáns beállítás alapértelmezés szerint. Ezzel biztosíthatja, hogy a rendszer automatikusan replikálja a tároló egy másodlagos adatközpontba. Például a tároló az USA nyugati régiója a rendszer automatikusan replikálja USA keleti régiója.

3.  Konfigurálja a biztonsági mentési házirend, és válassza ki a virtuális gép ugyanabban a szövegben.

4.  Győződjön meg arról, hogy a biztonsági mentési ügynök telepítve van a virtuális Gépen. Ha a virtuális gép létrehozása az Azure katalógusában lemezképpel, majd a biztonsági mentési ügynök már telepítve van. Egyéb (Ez azt jelenti, hogy ha egyéni lemezkép használata esetén), kövesse az utasításokat [a Virtuálisgép-ügynök telepítése a virtuális gépen](../backup/backup-azure-arm-vms-prepare.md#install-the-vm-agent-on-the-virtual-machine).

5.  Győződjön meg arról, hogy a virtuális gép hálózati kapcsolattal a biztonsági mentési szolgáltatás működéséhez. Kövesse ezeket az utasításokat a [hálózati kapcsolatra](../backup/backup-azure-arm-vms-prepare.md#network-connectivity).

6.  Miután a fenti lépéseket, a biztonsági mentés a biztonsági mentési házirend rendszeres időközönként indul el. Szükség esetén manuálisan az irányítópultról tárolóban az Azure portálon az első biztonsági mentés indíthat el.

A automatizálása Azure biztonsági mentési parancsfájlok segítségével, tekintse meg [PowerShell-parancsmagok a virtuális gép biztonsági mentése](../backup/backup-azure-vms-automation.md).

### <a name="steps-for-recovery"></a>A helyreállítás lépései

Ha tudja javítani, illetve építse újra a virtuális gép van szüksége, visszaállíthatja a virtuális gép bármelyik a biztonsági mentések helyreállítási pontjait a tárolóban lévő állapottal. Többféle helyreállításhoz különböző lehetőségek közül:

1.  A biztonsági másolat virtuális Gépet egy időpontban ábrázolása, hozhat létre egy új virtuális Gépet.

2.  Visszaállíthatja a lemezeket, és majd használni a virtuális gép testreszabása, és építse újra a visszaállított virtuális Géphez. 

Az utasítások [visszaállítani a virtuális gépek használata Azure-portálon](../backup/backup-azure-arm-restore-vms.md#restoring-a-vm-during-azure-datacenter-disaster). A dokumentum is ismerteti az adott biztonsági másolat virtuális gépek visszaállítására a párosított adatközpontba, a georedundáns biztonsági mentési tároló katasztrófa esetén használja az elsődleges adatközpont. Ebben az esetben Azure Backup segítségével a számítási szolgáltatás másodlagos régióban hozza létre a visszaállított virtuális gépet.

Használhatja a PowerShell [visszaállítását egy virtuális gép](../backup/backup-azure-vms-automation.md#restore-an-azure-vm) vagy [lemezeket az új virtuális gép létrehozása vissza](../backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

## <a name="alternative-solution-consistent-snapshots"></a>Alternatív megoldás: Alkalmazáskonzisztens pillanatképek

Ha nem használható az Azure Backup szolgáltatás, a saját készített pillanatfelvételek segítségével történő biztonsági mentési mechanizmus valósíthat meg. Egy virtuális gép által használt lemezek alkalmazáskonzisztens pillanatképeket, és egy másik régióban majd replikálja ezeket a pillanatképeket bonyolult. Emiatt Azure figyelembe veszi, kihasználva a biztonsági mentési szolgáltatás egy jobb megoldás, mint egy egyéni megoldás összeállításakor. Ha RA-GRS vagy GRS-tároló lemezt használ, pillanatképeket a rendszer automatikusan replikálja egy másodlagos adatközpontba. Ha LRS-tároló lemezt használ, akkor replikálja az adatokat. További információkért lásd: [készítsen biztonsági másolatot az Azure nem felügyelt méretű lemezek növekményes pillanatképek](storage-incremental-snapshots.md).

Pillanatkép egy időben egy adott időpontban objektum ábrázolása. Pillanatkép gyakorisága számlázási adatok növekményes méretének azt tartalmazza. További információkért lásd: [blob pillanatkép létrehozása](storage-blob-snapshots.md).

### <a name="creating-snapshots-while-the-vm-is-running"></a>Pillanatképeinek, a virtuális gép futása közben

Bármikor is készítsen pillanatképet, ha a virtuális gép fut, miközben továbbra is fennáll a lemezek részére küldött adatok, és a pillanatképek tartalmazhat részleges műveletek, melyeket a felhőszolgáltató közötti átviteléhez. Is ha több lemez használnak, különböző lemezek pillanatkép-készítési azért fordulhatott elő különböző időpontokban, ami azt jelenti, hogy ezeket a pillanatképeket nem lehet koordinált. Ez a különösen akkor jelent problémát, amelynek fájlok megsérülhetnek, ha éppen módosítva biztonsági mentés során csíkozott kötetek.

Ebben a helyzetben elkerülése érdekében a biztonsági mentési folyamat implementálnia kell az alábbi lépéseket:

1.  A lemezek rögzítése

2.  Ürítse ki az összes függőben lévő írási műveletek

3.  Ezt követően [blob pillanatkép létrehozása](storage-blob-snapshots.md) lemezein

Egyes Windows-alkalmazások, például az SQL Server egy olyan mechanizmust koordinált biztonsági mentési keresztül VSS alkalmazáskonzisztens biztonsági mentéseinek létrehozását. Linux a lemezeken, amely előállít fájlkonzisztens biztonsági mentések, de nem alkalmazáskonzisztens pillanatképeket összehangolására egy eszköz, például fsfreeze használhatja. Ez a folyamat bonyolult, így érdemes használni, és [Azure biztonsági mentési](../backup/backup-azure-vms-introduction.md) vagy egy külső biztonsági mentési megoldást, amely már megvalósításának.

A fenti eljárás koordinált pillanatképek a virtuális gép lemezein, a virtuális gép egy adott időpontban nézet képviselő gyűjteménye eredményez. Ez a biztonsági mentés visszaállítási pont a virtuális gép számára. Az eljárás megismétlésével rendszeres időközönként a rendszeres biztonsági mentések létrehozását. Lásd az alábbi lépéseit [másolja át a biztonsági mentés egy másik régióban](#copy-the-snapshots-to-another-region) Dr.

### <a name="creating-snapshots-while-the-vm-is-offline"></a>Pillanatképeinek, miközben a virtuális gép offline állapotban.

Egy másik lehetőség a konzisztens biztonsági mentések létrehozását a virtuális gép leállítása vagy blob pillanatképek készítése a lemezek. Ez az egyszerűbb, mint a futó virtuális gépek koordináló pillanatképek, de néhány perc leállás mellett van szükség. Ez a folyamat kövesse az alábbi lépéseket:

1. Állítsa le a virtuális Gépet.

2. Pillanatkép létrehozása a minden Virtuálismerevlemez-blobot, amely csak a néhány másodpercet vesz igénybe.

    Pillanatkép létrehozása, használhatja a [PowerShell](storage-powershell-guide-full.md#how-to-manage-azure-blob-snapshots), a [Azure Storage Rest API](https://msdn.microsoft.com/library/azure/ee691971.aspx), [Azure CLI](https://docs.microsoft.com/azure/xplat-cli-install), vagy az Azure Storage Ügyfélkódtáraival például egyik [a Storage ügyféloldali kódtára a .NET](https://msdn.microsoft.com/library/azure/hh488361.aspx).

3. Indítsa el a virtuális Gépet, ami befejezi az állásidő. Általában a teljes folyamat néhány percen belül befejeződik.

Ez a folyamat egy gyűjteményhez tartozó összes lemezt, a biztonsági mentés visszaállítási pontot biztosít a virtuális gép alkalmazáskonzisztens pillanatképek adja eredményül. A pillanatképek másolása egy másik régióban Dr lépéseit lásd az alábbi.

### <a name="copy-the-snapshots-to-another-region"></a>A pillanatképek másolása egy másik régióban

Kizárólag pillanatképek létrehozásának nem elegendő a vész-Helyreállítási lehet. Egy másik régióban is a pillanatkép biztonsági mentést kell készítenie.

Ha grs-re vagy RA-GRS a lemezek használ, majd a pillanatképek replikálva vannak a másodlagos régióba automatikusan. Nem határozható meg, a replikáció előtt lag néhány percet, és ha az elsődleges adatközpont leáll, mielőtt a pillanatképek Befejezés replikálása, nem lesz fér hozzá a pillanatképek a másodlagos adatközpont. Ez a valószínűségét nem nagy.

> [!Note] 
> Amelyek csak a lemezeket a grs-re vagy RA-GRS-fiók nem nyújt védelmet a virtuális gép a vészhelyzetek esetére. Kell koordinált pillanatképeket, vagy az Azure Backupot használni. Ez a konzisztens állapotú virtuális gépek helyreállítása szükséges.

Ha LRS használ, akkor át kell másolnia a pillanatképek során eltérő tárfiók a pillanatkép létrehozása után azonnal. A Másolás célja az LRS tárfiók más régióban, ami azt eredményezi, a példány éppen egy távoli régióban lehet. A pillanatkép is másolhatja az RA-GRS-tárfiókot, ugyanabban a régióban. Ebben az esetben a pillanatkép lazily replikálja a távoli másodlagos régióba. A biztonsági mentés védve van katasztrófák egyszer a másolása az elsődleges helyen, és a replikálás befejeződik.

A növekményes pillanatképek hatékony vész-Helyreállítási másolása, tekintse át a utasításait [készítsen biztonsági másolatot az Azure nem felügyelt méretű lemezek növekményes pillanatképek](storage-incremental-snapshots.md).

![][2]

### <a name="recovery-from-snapshots"></a>A pillanatképek helyreállítás

Pillanatkép lekéréséhez másolja azt, hogy egy új blob legyen. A pillanatkép másolása az elsődleges fiókból, másolhatja a pillanatkép keresztül az alap blob a pillanatkép, így, akkor a gyermekrekordokra a lemezen a pillanatkép; Ez más néven a pillanatkép támogatása. A pillanatkép biztonsági másolatából másol egy másodlagos fiókból (RA-GRS) esetén, ha egy elsődleges fiókhoz kell másolni. Pillanatkép másolhatja [PowerShell-lel](storage-powershell-guide-full.md#how-to-copy-a-snapshot-of-a-blob) vagy az AzCopy segédprogram használatával. További információkért lásd: [adatátvitel az AzCopy parancssori segédprogram a](storage-use-azcopy.md).

Esetén több lemezzel rendelkező virtuális gépek át kell másolnia a azonos koordinált visszaállítási pont részét képező összes pillanatképet. A pillanatképek írható VHD-blobok másolja, miután a blobok segítségével hozza létre újra a virtuális Gépet a sablon használatával a virtuális gép.

## <a name="other-options"></a>Egyéb beállítások

### <a name="sql-server"></a>SQL Server

A virtuális gépen futó SQL Server biztonsági mentés az SQL Server adatbázis Azure Blob-tároló vagy egy fájlmegosztást a saját beépített funkciókkal rendelkezik. Ha a tárfiók grs-re vagy RA-GRS, elérheti azokat a biztonsági másolatok a tárfiók másodlagos adatközpont legyen katasztrófahelyzet esetén, a ugyanazok a korlátozások, korábban bemutatott. További információkért lásd: [biztonsági mentése és visszaállítása az SQL Server Azure virtuális gépek](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md). Biztonsági mentés és helyreállítás, valamint [SQL Server Always On rendelkezésre állási csoportok](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md) is fenntartható a másodlagos replikákat az adatbázisok pedig jelentősen csökkenti a vész-helyreállítási idő.

## <a name="other-considerations"></a>Egyéb szempontok

Ez a cikk a biztonsági mentéshez, vagy a pillanatképek készítése a virtuális gépek és a lemezek katasztrófa utáni helyreállítás támogatásához, és ezek segítségével helyreállíthat rendelkezik ismertetése. Az Azure Resource Manager modellt sokan sablonokkal a virtuális gépek és egyéb infrastruktúrával létrehozása az Azure-ban. Egy sablon használatával hozzon létre egy virtuális Gépet, amely minden esetben azonos konfigurációval rendelkezik. Egyéni lemezképek létrehozásához a virtuális gépek használatakor is meg kell győződnie, hogy a képek védett RA-GRS fiókkal tárolja azokat.

Ezért a biztonsági mentési folyamat két dolgot kombinációja lehet:

1. Biztonsági mentés az adatokat (lemez)
2. A konfiguráció (sablonok, egyéni lemezképek) biztonsági mentése

Attól függően, hogy a biztonsági mentési lehetőséget választja előfordulhat, hogy a biztonsági mentés az adatok és a konfiguráció kezeléséhez, vagy a biztonsági mentési szolgáltatás előfordulhat, hogy kezelni tudja, hogy az Ön.

## <a name="appendix---understanding-the-impact-of-lrs-grs-and-ra-grs"></a>Függelék – LRS, GRS és RA-GRS hatásának ismertetése

Az Azure storage-fiókok, háromféle adatredundanciát is figyelembe kell vennie az vonatkozó vész-helyreállítási – helyileg redundáns (LRS), georedundáns (GRS), vagy georedundáns az olvasási hozzáférést (RA-GRS). 

Megtartja a helyileg redundáns tárolás (LRS) három másolatot ugyanabban az adatközpontban lévő adatokat. Az adatok írásakor, minden három másolatot frissítése előtt, hogy azok egyezését visszaérkezik a hívóhoz sikeres van. A lemez védett a helyi hibák, mivel nem valószínű, hogy szeretné-e minden három másolatot hatása egyszerre. Esetén az LRS nincs redundancia, földrajzi, a lemez nem védett a végzetes hibák, amely hatással lehet egy teljes data center vagy tárolási egység.

Georedundáns és RA-GRS három másolatot az adatok megmaradnak az elsődleges régióban (az Ön által kiválasztott), valamint az adatok három további példányait a megfelelő másodlagos régióban (beállítása az Azure-ban) megmaradnak. Például ha USA nyugati régiója adatokat tárolja, az adatok replikálja az USA keleti régiója. Ez a aszinkron módon történik, és frissítései az elsődleges és másodlagos kis késleltetés. A lemezek, a másodlagos hely replika konzisztens lemez alapú (a késés), de a több aktív lemez replikái nem lehet szinkronban **egymással**. Ahhoz, hogy konzisztens replikák több lemezre, alkalmazáskonzisztens pillanatképek van szükség.

A GRS és az RA-GRS közötti fő különbség a, hogy az RA-GRS, a másodlagos másolat olvasható bármikor. Az Azure-csapat megjelenítő nem érhető el az adatokat az elsődleges régióban probléma merül fel, minden erőfeszítést állítsa vissza a hozzáférést. Amíg az elsődleges nem működik, ha az RA-GRS engedélyezve van, hozzáférhet-e az adatokat a másodlagos adatközpont. Ezért ha azt tervezi, hogy olvassa el a replikából, amíg az elsődleges kiszolgáló nem érhető el, majd RA-GRS-érdemes figyelembe venni.

Ha egy jelentős leállás kell, az Azure-csapat egy földrajzi feladatátvételt indít, és módosítsa az elsődleges DNS-bejegyzéseket, mutasson a másodlagos tárterületre. Ezen a ponton Ha bármelyik Georedundáns vagy RA-GRS engedélyezve van, akkor férhet hozzá az adatokhoz a régióban kell lennie a másodlagos használt. Ez azt jelenti Ha a tárfiók Georedundáns és probléma merül fel, végezheti el a másodlagos tárterületre csak, ha egy földrajzi feladatátvételt.

További információkért lásd: [Mi a teendő, ha egy Azure Storage esetleges leálláskor](storage-disaster-recovery-guidance.md). 

Figyelje meg, hogy a Microsoft meghatározza, hogy egy feladatátvétel történik. Feladatátvétel nem szabályozott egy tárfiókot, ezért úgy nem határoznak egyéni ügyfelek. A speciális tárolási és virtuális gépek lemezeit katasztrófa utáni helyreállítás végrehajtásához a jelen cikkben korábban ismertetett módszerek kell használnia.



[1]: ./media/storage-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-1.png
[2]: ./media/storage-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-2.png