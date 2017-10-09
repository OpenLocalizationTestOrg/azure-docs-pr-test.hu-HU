---
title: "az Azure infrastruktúra-szolgáltatási lemezek aaaBackup, valamint a vész helyreállítási |} Microsoft Docs"
description: "Ebben a cikkben ismertetjük hogyan tooplan a biztonsági mentés és katasztrófa utáni helyreállítás (DR) az infrastruktúra-szolgáltatási virtuális gépeket (VM) és az Azure-lemezeket. Ez a dokumentum ismerteti a felügyelt és a nem felügyelt lemezek"
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
ms.openlocfilehash: 49b0e7732d6df9407e1e44d9af2500c99a85b37e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="backup-and-disaster-recovery-for-azure-iaas-disks"></a>Az Azure infrastruktúra-szolgáltatási lemezek a biztonsági mentés és katasztrófa-helyreállítás

Ebben a cikkben ismertetjük hogyan tooplan a biztonsági mentés és katasztrófa utáni helyreállítás (DR) az infrastruktúra-szolgáltatási virtuális gépeket (VM) és az Azure-lemezeket. Ez a dokumentum ismerteti a felügyelt és a nem felügyelt lemezek.

Lesz először döntésről bővebben hello beépített tartalék tolerancia képességei hello Azure platformon, amely segítségével védelmet biztosítanak a helyi hibák. Majd mutatjuk hello vészhelyreállítási forgatókönyvek nem teljes mértékben hello beépített képességei, amelyek ebben a dokumentumban tárgyalt hello fő témakört. Néhány példa munkaterhelés forgatókönyvek, ahol különböző biztonsági mentési és vész-Helyreállítási feltételek vonatkoznak, előfordulhat, hogy is mutat. Azt majd áttekinti a lehetséges megoldásokról az infrastruktúra-szolgáltatási lemezek vész-Helyreállítási. 

## <a name="introduction"></a>Bevezetés

hello Azure platformon a redundancia érdekében különböző módszereket használja, és tartalék tolerancia toohelp az ügyfelek a előforduló honosított hardver meghibásodásával szembeni védelemhez. Helyi hibák tartalmazhatják egy tároló virtuális lemez hello adatok egy részét vagy az SSD és HDD sikertelen ezen a kiszolgálón az Azure storage server gépével kapcsolatos probléma. Ilyen elkülönített hardver összetevőinek meghibásodása esetén fordulhat elő, a normál működés során, és hello platform tervezett toobe rugalmas toothese hibák. Jelentős katasztrófa hibák vagy a tároló kiszolgálók vagy a teljes adatközpont nagy számú inaccessibility eredményezhetnek. Miközben a virtuális gépek és a lemezek megfelelően védettek a helyi hibák, további lépések végrehajtására-e a szükséges tooprotect az érdekében, hogy a régió kiterjedő súlyos hibák (például egy jelentős katasztrófa), amelyek hatással lehetnek a virtuális gép és a lemezek.

Ezenkívül platform hibák toohello lehetőségét, problémák hello ügyfél alkalmazáshoz vagy az adatok akkor fordulhat elő. Például az alkalmazás új verziójának véletlenül teheti a legfrissebb toohello adatok módosítása. Ebben az esetben érdemes lehet toorevert hello alkalmazás és hello adatok tooa előzetes verzióját tartalmazó hello utolsó ismerten jó állapot. Ehhez a rendszeres biztonsági mentések karbantartása.

Regionális katasztrófa utáni helyreállítás biztonsági másolatot az infrastruktúra-szolgáltatási virtuális gép lemezek tooa más régióban. 

Mielőtt úgy tekintünk, biztonsági mentési és vész-Helyreállítási beállítások, most recap néhány módszer áll rendelkezésre helyi hibák kezelése.

### <a name="azure-iaas-resiliency"></a>Az Azure IaaS rugalmasság

*Rugalmasság* toohello tolerancia hivatkozik a normál a hardverösszetevők előforduló hibákat. Rugalmasság hello képességét toorecover a hibák és toofunction folytatni. Nincs kapcsolatos hibák elkerülése, de úgy, hogy elkerülhető az állásidő és adatvesztés toofailures válaszol. hello rugalmassági célja tooreturn hello tooa teljes mértékben működő alkalmazásállapot hibát követően. Az Azure virtuális gépek és lemezek a tervezett toobe rugalmas toocommon hardveres hibák. Ossza meg velünk tekintse meg hogyan hello Azure IaaS platform ez rugalmasságot biztosít.

A virtuális gépek általában két részből áll: (1) A kiszolgálón, és (2) hello állandó lemezek számítási. Hello hibatűrés egy virtuális gép is érinti.

Ha a hello Azure számítási gazdagép-kiszolgálón, amely a virtuális gép hardverhiba (amely ritka), Azure a tervezett tooautomatically visszaállítási hello virtuális gép egy másik kiszolgálón. Ez akkor fordul elő, ha újraindítás fog tapasztalni, és hello virtuális gép biztonsági mentése némi várakozás után lesz. Azure automatikusan észleli az ilyen hardverhibák, és végrehajtja a helyreállítások toohelp győződjön meg arról, minél hamarabb hello ügyfél virtuális gép lesz elérhető.

IaaS lemezekre vonatkozó adatok tartósságát fontos állandó tároló platform hello. Azure-ügyfél infrastruktúra-szolgáltatáson futó üzleti alkalmazásokkal rendelkezik, és hello megőrzése hello függenek. Azure tervek védelmét a három redundáns másolatok IaaS lemezeket helyileg tárolt adatok biztosít magas tartósság helyi hibák ellen. Ha egy hello hardverösszetevők, amely tárolja a lemez meghibásodik, a virtuális gép nem változik, mert nincsenek két további másolatokat toosupport kérésekre. Jól működik akkor is, ha a lemez támogató két különböző hardverösszetevők meghiúsulhatnak hello azonos idő (amely általában nagyon ritkán). toohelp győződjön meg arról, hogy mindig karbantartása három replikák, hello Azure Storage szolgáltatás automatikusan indít hello háttérben adatok egy új példányát, ha egy hello három példányban nem érhető el. Emiatt nem kell az Azure-lemezeket a hibatűrés szükséges toouse RAID. Egy egyszerű RAID 0 konfigurációs legyen elegendő hello lemezek szétosztott, ha a szükséges toocreate nagyobb méretű köteteket.

Ebben az architektúrában miatt **Azure következetesen rendelkezik kézbesíteni a vállalati szintű tartósságot, az infrastruktúra-szolgáltatási lemezek, egy iparágvezető a nulla % [Annualized hibaaránya](https://en.wikipedia.org/wiki/Annualized_failure_rate).**

Honosított hardver hibák hello a gazdagép vagy hello tárolási platform néha okozhat a virtuális gép, amely a hello hello átmeneti elérhetetlenség számítási [Azure garantált szolgáltatási szintje](https://azure.microsoft.com/support/legal/sla/virtual-machines/) a virtuális gép rendelkezésre állás érdekében. Azure iparágvezető szolgáltatásiszint-szerződésben garantált is biztosít, a prémium szintű Storage lemezeket használó egyetlen Virtuálisgép-példányok.

toosafeguard alkalmazások és szolgáltatások a lemez-vagy virtuális gép toohello elérhetetlenség miatt állásidő, az ügyfelek kihasználhatják a [rendelkezésre állási készletek](../../virtual-machines/windows/manage-availability.md). Két vagy több virtuális gépek rendelkezésre állási csoport számára biztosít redundanciát hello alkalmazáshoz. Azure majd hoz létre a virtuális gépek és a lemezek külön tartalék tartományok különböző tápellátáshoz, a hálózati és a kiszolgáló-összetevők. Ebből kifolyólag honosított hardver meghibásodása általában nem befolyásolják hello hello állítsa be a több virtuális gép magas rendelkezésre állás biztosítása az alkalmazás egy időben. Egy jó gyakorlat toouse rendelkezésre állási készletek, amikor szükség a magas rendelkezésre állású számít. További információkért lásd: hello vész-helyreállítási szempontok részletes alatt.

### <a name="backup-and-disaster-recovery"></a>Biztonsági mentés és katasztrófa utáni helyreállítás

Vészhelyreállítás (DR) hello képességét toorecover a ritka, de jelentős események: nem átmeneti, szinten hibák, például egy teljes terület érintő üzemkimaradás időtartamát. Vész-helyreállítási magában foglalja az adatok biztonsági mentése és az archiválásra, és előfordulhat, hogy tartalmazza a manuális beavatkozásra, például egy adatbázis biztonsági másolatból történő visszaállítását.

hello Azure platformon beépített védelmet biztosít a helyi hibák lehetséges, hogy nem teljes védelme érdekében hello virtuális gépek/lemezek hello esetében jelentős katasztrófa, amelyek nagy méretű kimaradások esetén. Ez magában foglalja a katasztrofális esemény, például egy Hurrikán, földrengés, tűz vagy nagy méretű méretezési egység hardverhibák alatt kattintson az adatközpontban. Ezenkívül tooapplication vagy adatok problémák miatt hibák fordulhatnak.

toohelp az IaaS munkaterhelések védelme a kimaradások, meg kell terveznie a redundancia és a biztonsági mentések tooenable helyreállításhoz. A vészhelyreállítás esetében meg kell terveznie a redundancia és a biztonsági mentés befejeződött hello elsődleges hely különböző földrajzi helyen. Ez biztosítja a biztonsági másolat nem érinti a hello ugyanarra az eseményre, amely eredetileg érintett hello VM vagy a lemezeket. További információkért lásd: [alkalmazások vész-helyreállítási épülő Azure](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications).

A vész-Helyreállítási szempontok lehetnek hello a következő szempontokat:

1. Magas rendelkezésre állású (HA) hello alkalmazás toocontinue állapota kifogástalan, jelentős állásidőt nélkül fut hello képességét. "Kifogástalan állapot," által jelenleg: hello alkalmazás működik-e, és a felhasználók toohello alkalmazás csatlakozás és használhatja őket. Egyes kritikus fontosságú alkalmazások és az adatbázisok lehet szükséges toobe elérhető mindig, még akkor is, ha nincsenek hibák hello platform. A munkaterhelések esetén szükség lehet a tooplan redundancia hello alkalmazás, valamint a hello adatok.

2. Adatok tartóssága: Bizonyos esetekben hello legfontosabb szempont annak ellenőrzése hello adatok megmaradjanak hello esetében egy olyan vészhelyzet esetén. Ezért szükség lehet egy másik helyen az adatok biztonsági másolata. Ilyen munkaterhelések esetén előfordulhat, hogy nem kell teljes redundancia hello alkalmazáshoz, de hello lemezek rendszeres biztonsági másolatot.

## <a name="backup-and-dr-scenarios"></a>Biztonsági mentési és vész-Helyreállítási forgatókönyvek

Vizsgáljuk meg az alkalmazás munkaterhelés-forgatókönyvek és vész-helyreállítási tervezési szempontjai hello néhány tipikus példája.

### <a name="scenario-1-major-database-solutions"></a>1. forgatókönyv: Fő adatbázis megoldások

Vegye figyelembe például az SQL Server vagy Oracle, amely támogatja a magas rendelkezésre állású éles adatbázis-kiszolgálót. Kritikus termelési alkalmazások és a felhasználók ezt az adatbázist függ. a rendszer hello vész-helyreállítási terv esetleg toosupport hello követelményeknek:

1. Az adatok védett és helyreállítható kell lennie.
2.  Lehet, hogy a kiszolgáló használható.

Elképzelhető, hogy szükség karbantartása hello adatbázis biztonsági mentéséhez egy másik régióban. Attól függően, hogy a kiszolgáló rendelkezésre állása és az adat-helyreállítás hello követelményei hello megoldás terjedhet egy aktív-aktív vagy aktív-passzív replika hely tooperiodic offline biztonsági mentés hello adatok. Például az SQL Server és Oracle relációs adatbázisok replikációs különböző lehetőségeket biztosítanak. Az SQL Server [SQL Server Always On rendelkezésre állási csoportok](https://msdn.microsoft.com/library/hh510230.aspx) magas rendelkezésre állás is használható.

MongoDB hasonló NoSQL-adatbázisok is támogatja a [replikák](https://docs.mongodb.com/manual/replication/) a redundancia érdekében. a magas rendelkezésre állású hello replikák használható.

### <a name="scenario-2-a-cluster-of-redundant-vms"></a>2. forgatókönyv: Egy fürt redundáns virtuális gépek

Érdemes lehet a munkaterhelés, a fürt virtuális gépeket, a redundanciát és terheléselosztást kezeli. Egy példa erre egy Cassandra fürt üzembe helyezett egy régiót. Az ilyen típusú architektúra már biztosít magas szintű redundancia adott régión belül. Azonban tooprotect hello munkaterhelés, a regionális szintű meghibásodása, érdemes vagy a rendszeres biztonsági mentést tooanother régió hello fürt két régióban is terjednek.

### <a name="scenario-3-iaas-application-workload"></a>3. forgatókönyv: Infrastruktúra-szolgáltatási alkalmazás munkaterhelés

Ennek oka lehet egy tipikus környezet munkaterhelés egy Azure virtuális gépen. Például annak oka lehet egy webkiszolgáló vagy a fájlkiszolgáló hello tartalom és a hely más erőforrásokat. Egy egyedi üzleti alkalmazást, amely az adatok, erőforrások és alkalmazásállapot lemezen tárolja a hello méretű is lehet. Ebben az esetben is fontos toomake meg arról, hogy a biztonsági mentések rendszeresen igénybe vehet. Biztonsági mentési gyakoriság alapján hello VM munkaterhelés hello jellegét. Például ha hello alkalmazás naponta fut, és módosítja az adatokat, majd hello biztonsági mentést kell fordítani óránként.

Egy másik példa a jelentéskészítő kiszolgáló más forrásokból származó adatok lekéréses és összesített jelentéseket hoz létre. A virtuális gép vagy lemezek hello jelentések toohello megszűnését irányítja. Azonban lehetséges toorerun hello jelentéskészítési folyamat kimenet újragenerálása hello lehet. Ebben az esetben valóban nincs adatvesztést akkor is, ha egy olyan vészhelyzet esetén a találati hello által használandó jelentéskészítő kiszolgáló, így magasabb szintű tűréshatár elvesztése a jelentéskészítő kiszolgáló hello hello adatok egy részét sikerült kell. Ebben az esetben ritkábban biztonsági mentések egy beállítás tooreduce hello költséget.

### <a name="scenario-4-iaas-application-data-issues"></a>4. forgatókönyv: Az infrastruktúra-szolgáltatási alkalmazás adatok kapcsolatos problémákat

Van olyan alkalmazás, amely kiszámítja, tart fenn és kritikus kereskedelmi forgalomban beszerezhető adatok, például a díjszabásról szolgál. Az alkalmazás új verziójának szoftver programhiba helytelenül számított hello árképzési és hello hello platform által kiszolgált meglévő commerce adatok sérült volt. Itt, a helyes intézkedéseket hello lenne toorevert toohello hello alkalmazás és az adatok hello korábbi verziójának első. tooenable a, hajtsa végre a megfelelő rendszeres biztonsági mentést a rendszer.

## <a name="disaster-recovery-solution-azure-backup-service"></a>Vész-helyreállítási megoldást: Azure biztonsági mentési szolgáltatás

[Az Azure Backup szolgáltatás](https://azure.microsoft.com/services/backup/) biztonsági mentési és vész-Helyreállítási is használható, és működik együtt a [kezelt lemezek](../../virtual-machines/windows/managed-disks-overview.md) , valamint [nem felügyelt lemezek](../../virtual-machines/windows/about-disks-and-vhds.md#unmanaged-disks). Az idő-alapú biztonsági mentések, könnyű VM-helyreállítás és biztonsági másolatok megőrzésének házirendek segítségével hozhat létre egy biztonsági mentési feladat. 

Ha [prémium szintű Storage lemezek](storage-premium-storage.md), [kezelt lemezek](../../virtual-machines/windows/managed-disks-overview.md), vagy más lemeztípusok esetén a hello [helyileg redundáns tárolás (LRS)](storage-redundancy.md#locally-redundant-storage) beállítást, különösen fontos tooleverage DR rendszeres biztonsági mentést. Az Azure biztonsági mentést a Recovery Services-tároló, a hosszú távú megőrzési hello adatokat tárolja. Válassza ki a hello [georedundáns tárolás (GRS)](storage-redundancy.md#geo-redundant-storage) a hello biztonsági mentés Recovery Services-tároló lehetőséget. Amely biztosítja, biztonsági mentések replikált tooa különböző Azure-régiót a regionális katasztrófák védelmére.

A [nem felügyelt lemezek](../../virtual-machines/windows/about-disks-and-vhds.md#unmanaged-disks), az infrastruktúra-szolgáltatási lemezek hello LRS tárolási típust használja, de győződjön meg arról, hogy az Azure Backup szolgáltatás engedélyezve van-e a Recovery Services-tároló hello Georedundáns beállítást hello.

**Ha hello [Georedundáns](storage-redundancy.md#geo-redundant-storage)/[RA-GRS](storage-redundancy.md#read-access-geo-redundant-storage) beállítás megadása után még a nem felügyelt lemezek kell alkalmazáskonzisztens pillanatképeket vonatkozó biztonsági mentési és vész-Helyreállítási.** Egyikét kell használnia [Azure Backup szolgáltatás](https://azure.microsoft.com/services/backup/) vagy [alkalmazáskonzisztens pillanatképek](#alternative-solution-consistent-snapshots).

hello vész-Helyreállítási megoldások összefoglalása látható.

| Forgatókönyv | Automatikus replikáció | Vész-Helyreállítási megoldás |
| --- | --- | --- |
| *Prémium szintű Storage-lemezekhez* | Helyi ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Managed Disks* | Helyi ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Nem felügyelt LRS-lemezek* | Helyi ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Nem felügyelt Georedundáns lemezek* | Több régióban ([Georedundáns](storage-redundancy.md#geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Alkalmazáskonzisztens pillanatképek](#alternative-solution-consistent-snapshots) |
| *Nem felügyelt RA-GRS-lemezek* | Több régióban ([RA-GRS](storage-redundancy.md#read-access-geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Alkalmazáskonzisztens pillanatképek](#alternative-solution-consistent-snapshots) |

Magas rendelkezésre állású, legjobban érheti hello használata felügyelt lemezeinek rendelkezésre állási csoport együtt az Azure Backup szolgáltatásnál. Nem felügyelt lemezt használ, ha a vész-Helyreállítási továbbra is használhatja a Azure Backup szolgáltatásnál. Ha nem toouse Azure biztonsági mentés, majd véve [alkalmazáskonzisztens pillanatképek](#alternative-solution-consistent-snapshots) leírtak szerint egy későbbi szakasz ismerteti a biztonsági mentési és vész-Helyreállítási alternatív megoldást-e.

A választott a magas rendelkezésre állás érdekében biztonsági mentési és vész-Helyreállítási alkalmazást vagy az infrastruktúra szintjén, az alábbiakban ábrázolhatók:

| *Szint* | Magas rendelkezésre állás   | Biztonsági mentés / DR |
| --- | --- | --- |
| *Alkalmazás* | Always On SQL | Azure Backup |
| *Infrastruktúra*  | Rendelkezésre állási csoport  | Az alkalmazáskonzisztens pillanatképek Georedundáns |

### <a name="using-hello-azure-backup-service"></a>Hello Azure Backup szolgáltatás használatával

[Azure biztonsági mentés](../../backup/backup-azure-vms-introduction.md) tud biztonsági másolatot készíteni a Windows vagy Linux toohello Azure Recovery Services-tároló futó virtuális gépeket. Biztonsági mentése és visszaállítása az üzleti szempontból kritikus fontosságú adatok van bonyolítja hello tényt, hogy üzleti szempontból kritikus fontosságú adatok kell biztonsági mentése közben hello alkalmazások, amelyek hello adatok futnak egymással. tooaddress az Azure Backup alkalmazáskonzisztens biztonsági mentések Microsoft-munkaterhelések segítségével biztosítja, hogy adatot ír megfelelően toostorage hello kötet árnyékmásolata szolgáltatás (VSS) tooensure. Linux virtuális gépekhez csak a fájlkonzisztens biztonsági mentések is előfordulhatnak, Linux nem tartozik megfelelő tooVSS funkció.

![](./media/storage-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-1.png)   

Azure biztonsági mentési indít el egy biztonsági mentési feladat hello ütemezett időpontban, elindítja a telepített virtuális gép tootake időpontban pillanatkép hello hello tartalék mellék. A rendszer pillanatképet készít hello virtuális gép lemezeinek hello VSS tooget alkalmazáskonzisztens pillanatfelvételi együttműködve anélkül, hogy tooshut azt le. hello biztonsági mentési bővítményt a virtuális gép hello kiürítése minden írás előtt az összes hello lemez a hello konzisztens pillanatképének készítése. Miután hello pillanatképet készít, hello adatátvitel által toohello Azure Backup mentési tároló. toomake hello biztonsági mentési folyamat sokkal hatékonyabb, hello szolgáltatás azonosítja, és csak a hello utolsó biztonsági mentés óta megváltoztak adattípusnak hello blokkok átvitele.


toorestore, hello elérhető biztonsági másolatok Azure Backup segítségével tekintheti meg és majd indítsa el a visszaállítást. Hozzon létre, és állítsa vissza az Azure biztonsági mentések hello keresztül [Azure-portálon](https://portal.azure.com/), [PowerShell használatával](../../backup/backup-azure-vms-automation.md ), vagy hello segítségével [Azure CLI](https://docs.microsoft.com/azure/xplat-cli-install). További információkért tekintse meg az Azure biztonsági mentés.

### <a name="steps-tooenable-backup"></a>Lépéseket tooEnable biztonsági mentése

hello lépések megegyeznek a virtuális gépek hello általánosan használt tooenable biztonsági másolatainak [Azure Portal](https://portal.azure.com/). A pontos forgatókönyvtől függően változata lesz. Tekintse meg a toohello [Azure biztonsági mentés](../../backup/backup-azure-vms-introduction.md) teljes dokumentációjában. Azure biztonsági mentés is [támogatja a felügyelt lemezzel rendelkező virtuális gépek](https://azure.microsoft.com/blog/azure-managed-disk-backup/).

1.  A recovery services-tároló létrehozása a virtuális gépek lépések hello használata:

    a.  Hello segítségével [Azure-portálon](https://portal.azure.com/), keresse meg az összes erőforrást és a "Recovery Services-tárolók" található.

    b.  Hello a Recovery Services-tárolók menüben, kattintson a Hozzáadás gombra, és kövesse a hello lépéseket toocreate egy új tárolót a hello hello VM és ugyanabban a régióban. Például ha a virtuális gép USA nyugati régiója régióban, válasszon USA nyugati régiója hello tároló.

2.  Ellenőrizze a tárolóban található, újonnan létrehozott hello hello tárolóreplikálást. toodo, hozzáférési hello tárolóhoz hello Recovery Services tárolók panelen, majd a toohello beállítások/biztonsági másolatból konfigurációs. Győződjön meg arról, alapértelmezés szerint hello Georedundáns lehetőség van kiválasztva. Ez biztosítja, hogy a tároló automatikusan replikált tooa másodlagos adatközpont. Például a tároló az USA nyugati régiója lesz automatikusan replikált tooEast USA.

3.  Hello biztonsági mentési házirend konfigurálása, és válassza ki a virtuális gép hello hello a ugyanabban a szövegben.

4.  Győződjön meg arról, hogy hello hello VM Backup szolgáltatás ügynöke telepítve van. Ha a virtuális gép létrehozása az Azure katalógusában lemezkép használatával, majd hello Backup ügynök már telepítve van. Egyéb (Ez azt jelenti, hogy ha egyéni lemezkép használata esetén), kövesse az utasításokat, túl[hello virtuális gépen a Virtuálisgép-ügynök telepítése hello](../../backup/backup-azure-arm-vms-prepare.md#install-the-vm-agent-on-the-virtual-machine).

5.  Győződjön meg arról, hogy hello VM hello biztonsági mentési szolgáltatás toofunction hálózati kapcsolattal. Kövesse ezeket az utasításokat a [hálózati kapcsolatra](../../backup/backup-azure-arm-vms-prepare.md#network-connectivity).

6.  Hello a fenti lépések elvégzése után hello biztonsági mentés a biztonsági mentési házirend hello rendszeres időközönként indul el. Ha szükséges, hello manuálisan irányítópultról hello tároló hello Azure-portálon az első biztonsági mentés indíthat el.

A automatizálása Azure biztonsági mentési parancsfájlok segítségével, tekintse meg az túl[PowerShell-parancsmagok a virtuális gép biztonsági mentése](../../backup/backup-azure-vms-automation.md).

### <a name="steps-for-recovery"></a>A helyreállítás lépései

Ha toorepair kell, vagy építse újra a virtuális gépek, helyreállíthatja a hello VM bármelyik hello biztonsági mentések helyreállítási pontjait hello tárolóban lévő állapottal. Többféle hello helyreállítás végrehajtását különböző lehetőségek közül:

1.  A biztonsági másolat virtuális Gépet egy időpontban ábrázolása, hozhat létre egy új virtuális Gépet.

2.  Visszaállíthatja a hello lemezeket, és ezután használja az hello sablon hello VM toocustomize és rebuild hello visszaállítani a virtuális gép. 

Kövesse az utasításokat túl[használata Azure portál toorestore virtuális gépek](../../backup/backup-azure-arm-restore-vms.md#restoring-a-vm-during-azure-datacenter-disaster). hello dokumentum is ismerteti hello adott visszaállítására a biztonsági másolat virtuális gépek toohello párosított adatközpont a georedundáns biztonsági mentési tároló használatával egy olyan vészhelyzet esetén, az elsődleges adatközpont hello hello esetében. Ebben az esetben az Azure Backup hello számítási szolgáltatást hello másodlagos régióba toocreate hello visszaállított virtuális gépről használja.

Használhatja a PowerShell [visszaállítását egy virtuális gép](../../backup/backup-azure-vms-automation.md#restore-an-azure-vm) vagy [lemezeket az új virtuális gép létrehozása vissza](../../backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

## <a name="alternative-solution-consistent-snapshots"></a>Alternatív megoldás: Alkalmazáskonzisztens pillanatképek

Ha nem toouse hello Azure Backup szolgáltatás, a saját készített pillanatfelvételek segítségével történő biztonsági mentési mechanizmus valósíthat meg. Bonyolult toocreate alkalmazáskonzisztens pillanatképek egy virtuális gép által használt lemezek, és ezek a pillanatképek tooanother régió majd replikálja. Emiatt Azure úgy ítéli meg emelés hello biztonsági mentési szolgáltatás egy jobb megoldás, mint egy egyéni megoldás összeállításakor. RA-GRS/Georedundáns tárolás használatakor a lemezeket, akkor a pillanatképeket automatikusan replikált tooa másodlagos adatközpont. LRS-tároló lemez használja, ha szüksége lesz tooreplicate hello adatok magát. További információkért lásd: [készítsen biztonsági másolatot az Azure nem felügyelt méretű lemezek növekményes pillanatképek](../../virtual-machines/windows/incremental-snapshots.md).

Pillanatkép egy időben egy adott időpontban objektum ábrázolása. Pillanatkép gyakorisága számlázási hello növekményes méretéhez hello adatok azt tartalmazza. További információkért lásd: [blob pillanatkép létrehozása](../blobs/storage-blob-snapshots.md).

### <a name="creating-snapshots-while-hello-vm-is-running"></a>Hello közben pillanatképeinek virtuális gép fut.

Is pillanatképet készít, bármikor Ha hello virtuális gép fut, miközben továbbra is az adatfolyamként toohello lemezek adatai, és hello pillanatképek tartalmazhat részleges műveletek, melyeket a felhőszolgáltató közötti átviteléhez. Is ha több lemez használnak, különböző lemezek pillanatképei hello azért fordulhatott elő különböző időpontokban, ami azt jelenti, hogy ezeket a pillanatképeket nem lehet koordinált. Ez a különösen akkor jelent problémát, amelynek fájlok megsérülhetnek, ha éppen módosítva biztonsági mentés során csíkozott kötetek.

tooavoid ebben a helyzetben hello biztonsági mentési folyamat implementálnia kell a következő lépéseket hello:

1.  Az összes hello lemez rögzítése

2.  Ürítse ki az összes hello írása függőben

3.  Ezt követően [blob pillanatkép létrehozása](../blobs/storage-blob-snapshots.md) az összes hello lemezek

Egyes Windows-alkalmazások, például az SQL Server egy olyan mechanizmust koordinált biztonsági mentés VSS toocreate alkalmazáskonzisztens biztonsági mentések keresztül. Linux egy eszköz, például fsfreeze használható összehangolása a hello lemezek, amely előállít fájlkonzisztens biztonsági mentések, de nem alkalmazáskonzisztens pillanatképeket. Ez a folyamat bonyolult, így érdemes használni, és [Azure biztonsági mentési](../../backup/backup-azure-vms-introduction.md) vagy egy külső biztonsági mentési megoldást, amely már megvalósításának.

hello folyamat fent egy gyűjtemény összes hello VM lemezek, egy adott időpontban ábrázolása hello VM képviselő koordinált pillanatképek eredményez. Ez a hello virtuális gép egy biztonsági mentés visszaállítási pontot. Ütemezés szerint toocreate rendszeres biztonsági mentést, hello eljárás megismétlésével. Olvassa el az alábbi lépéseket hello túl[másolási hello biztonsági mentések tooanother régió](#copy-the-snapshots-to-another-region) Dr.

### <a name="creating-snapshots-while-hello-vm-is-offline"></a>Hello közben pillanatképeinek VM offline állapotban

Egy másik lehetőség toocreate konzisztens biztonsági másolatok leáll hello VM és véve blob egyes lemezek pillanatkép-készítési. Ez az egyszerűbb, mint a futó virtuális gépek koordináló pillanatképek, de néhány perc leállás mellett van szükség. Ez a folyamat kövesse az alábbi lépéseket:

1. Állítsa le a virtuális gép hello.

2. Pillanatkép létrehozása a minden Virtuálismerevlemez-blobot, amely csak a néhány másodpercet vesz igénybe.

    toocreate pillanatképet, használhat [PowerShell](storage-powershell-guide-full.md#how-to-manage-azure-blob-snapshots), hello [Azure Storage Rest API](https://msdn.microsoft.com/library/azure/ee691971.aspx), [Azure CLI](https://docs.microsoft.com/azure/xplat-cli-install), vagy egy hello Azure Storage Ügyfélkódtáraival például [ hello Storage ügyféloldali kódtára a .NET](https://msdn.microsoft.com/library/azure/hh488361.aspx).

3. Indítsa el a virtuális gép, amely hello állásidő vége hello. Általában hello teljes folyamat néhány percen belül befejeződik.

Ez a folyamat egy gyűjtemény összes hello lemezek, a biztonsági mentés visszaállítási pontot biztosítva hello VM alkalmazáskonzisztens pillanatképek adja eredményül. Lásd az alábbi hello lépéseket toocopy hello pillanatképek tooanother régió Dr.

### <a name="copy-hello-snapshots-tooanother-region"></a>Másolás hello pillanatképek tooanother régió

Hello pillanatképek önmagában létrehozásának nem elegendő a vész-Helyreállítási lehet. Hello pillanatkép biztonsági mentések tooanother régió replikálnia is kell.

Ha grs-re vagy RA-GRS a lemezek használ, majd hello pillanatképei replikált toohello másodlagos régióba automatikusan. Néhány percet, mielőtt hello replikációs késés lehet, és ha hello elsődleges adatközpont leáll, mielőtt hello pillanatképek Befejezés replikálása, csak akkor tudja tooaccess hello pillanatképeinek hello másodlagos adatközpont. hello annak valószínűségét, hogy ez nem nagy.

> [!Note] 
> Csak rendelkező hello lemezek grs-re vagy RA-GRS-fiók nem nyújt védelmet hello VM a vészhelyzetek esetére. Kell koordinált pillanatképeket, vagy az Azure Backupot használni. Ez azért szükség toorecover VM tooa konzisztens állapotú legyen.

LRS használatakor hello pillanatkép létrehozása után azonnal hello pillanatképek tooa eltérő tárfiók kell másolnia. hello másolás célja az LRS tárfiók más régióban, hello másolása folyamatban egy távoli régióban eredményezve lehet. Hello pillanatkép tooan RA-GRS-tárfiókot is másolhatja a hello ugyanabban a régióban. Ebben az esetben a hello pillanatkép lesz lazily replikált toohello távoli másodlagos régióba. A biztonsági mentés védett katasztrófák hello elsődleges helyen a hello fájlmásolási és -replikáció befejezése után.

toocopy a vész-Helyreállítási növekményes pillanatképek hatékonyan, tekintse át hello utasításait [készítsen biztonsági másolatot az Azure nem felügyelt méretű lemezek növekményes pillanatképek](../../virtual-machines/windows/incremental-snapshots.md).

![](./media/storage-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-2.png)   

### <a name="recovery-from-snapshots"></a>A pillanatképek helyreállítás

tooretrieve egy pillanatképet, egy új blob toomake másolja. Hello pillanatkép másolása hello elsődleges fiókból, másolhatja hello pillanatkép-keresztül toohello alap blob hello pillanatkép, így visszaállítási hello lemez toohello pillanatkép; Ez az úgynevezett hello pillanatkép támogatása. Hello pillanatkép biztonsági másolatából másolása (a hello esetben az RA-GRS) egy másodlagos fiókból, kell lennie tooa elsődleges fiók másolva. Pillanatkép másolhatja [PowerShell-lel](storage-powershell-guide-full.md#how-to-copy-a-snapshot-of-a-blob) vagy hello AzCopy segédprogram használatával. További információkért lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md).

Hello a több lemezzel rendelkező virtuális gépek esetében, át kell másolnia az összes azonos koordinált hello részét képező hello pillanatképek visszaállítási pont. Ha hello pillanatképek toowritable VHD-blobok másolja, a virtuális Gépet a virtuális gép hello hello sablonnal hello blobok toorecreate azonnal használható.

## <a name="other-options"></a>Egyéb beállítások

### <a name="sql-server"></a>SQL Server

Virtuális gépen futó SQL Server a saját beépített képességei toobackup rendelkezik, az SQL Server adatbázis tooAzure Blob-tároló vagy egy fájlmegosztáson. Ha hello tárfiók grs-re vagy RA-GRS, végezheti el ezeket a hello tárfiók másodlagos adatközpont hello esemény egy olyan vészhelyzet esetén a biztonsági másolatok a hello ugyanazok a korlátozások vonatkoznak, mint a korábban tárgyalt. További információkért lásd: [biztonsági mentése és visszaállítása az SQL Server Azure virtuális gépek](../../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md). Továbbá toobackup és visszaállítási [SQL Server Always On rendelkezésre állási csoportok](../../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md) is fenntartható a másodlagos replikáin adatbázisok toogreatly hello vész-helyreállítási idő csökkentése.

## <a name="other-considerations"></a>Egyéb szempontok

Ez a cikk rendelkezik tárgyalt hogyan toobackup vagy hajtsa végre a megfelelő pillanatképek a virtuális gépek és a lemezek toosupport vész-helyreállítási, és hogyan toouse e toorecover. Hello Azure Resource Manager modellt sokan használnak sablonok toocreate a virtuális gépek és egyéb infrastruktúrával az Azure-ban. Használhatja a sablont egy virtuális Gépet, amelynek hello toocreate minden alkalommal ugyanazt a konfigurációt. Egyéni lemezképek létrehozásához a virtuális gépek használatakor is meg kell győződnie, hogy a képek az RA-GRS fiók toostore-kóddal védett őket.

Ezért a biztonsági mentési folyamat két dolgot kombinációja lehet:

1. Biztonsági mentési hello adatok (lemez)
2. Biztonsági mentési hello konfigurációs (sablonok, egyéni lemezképek)

Attól függően, hogy hello biztonsági mentési lehetőséget választja előfordulhat, hogy toohandle hello biztonsági másolata mind hello és hello konfigurációs, vagy hello biztonsági mentési szolgáltatás előfordulhat, hogy kezelni tudja, hogy az Ön.

## <a name="appendix---understanding-hello-impact-of-lrs-grs-and-ra-grs"></a>Függelék – LRS, GRS és RA-GRS hello hatásának ismertetése

Az Azure storage-fiókok, háromféle adatredundanciát is figyelembe kell vennie az vonatkozó vész-helyreállítási – helyileg redundáns (LRS), georedundáns (GRS), vagy georedundáns az olvasási hozzáférést (RA-GRS). 

Helyileg redundáns tárolás (LRS) megőrzi az hello hello adatok három példányban ugyanabban az adatközpontban. Hello adatok írásakor, minden három másolatot frissítése előtt sikeres visszaadott toohello hívó, így megtudhatja, hogy azok egyezését. A lemez védett a helyi hibák, nem valószínű, hogy minden három másolatot hatással lenne az kell hello óta egy időben. LRS hello esetben nincs nincs georedundancia, így hello lemez nem védett a végzetes hibák, amely hatással lehet egy teljes data center vagy tárolási egység.

Georedundáns és RA-GRS az adatok három másolatot kerülnek be az elsődleges régióban hello (az Ön által kiválasztott), valamint az adatok három további példányait a megfelelő másodlagos régióban (beállítása az Azure-ban) megmaradnak. Például adat tárolása USA nyugati régiója, hello adatok replikált tooEast USA lesz. Ez aszinkron módon történik, és némi késés frissítések toohello elsődleges és másodlagos között van. Hello lemezeit hello másodlagos hely replika konzisztens lemez alapú (hello késleltetés), de a több aktív lemez replikái nem lehet szinkronban **egymással**. toohave konzisztens replikák több lemezre, alkalmazáskonzisztens pillanatképek van szükség.

hello fő Georedundáns és az RA-GRS közötti különbség, hogy az RA-GRS, hello másodlagos másolat olvasható bármikor. Ha a probléma jeleníti meg az elsődleges régióban hello hello adatok nem érhetők el, hello Azure-csapat minden erőfeszítést toorestore hozzáférés működésképtelenné teszi. Amíg elsődleges hello nem működik, ha az RA-GRS engedélyezve van, hozzáférhet-e hello adatok hello másodlagos adatközpont. Ezért ha hello replikából tooread elsődleges hello állapotában nem érhető el, majd RA-GRS-érdemes figyelembe venni.

Ha toobe egy jelentős leállás, hello Azure csapata egy földrajzi feladatátvételt indít és hello elsődleges DNS-bejegyzések toopoint toosecondary tárolás módosítása. Ezen a ponton Ha bármelyik Georedundáns vagy RA-GRS engedélyezve van, van-e hozzáférési hello adatok toobe hello másodlagos használt hello régióban. Ez azt jelenti Ha a tárfiók Georedundáns és probléma merül fel, végezheti el hello másodlagos tároló földrajzi-feladatátvétel esetén csak.

További információkért lásd: [egy Azure Storage tervezett kimaradás esetén milyen toodo](storage-disaster-recovery-guidance.md). 

Figyelje meg, hogy a Microsoft meghatározza, hogy egy feladatátvétel történik. Feladatátvétel nem szabályozott egy tárfiókot, ezért úgy nem határoznak egyéni ügyfelek. Vész-helyreállítási tooimplement meghatározott tárfiókok és a virtuális gépek lemezeit, az ebben a cikkben korábban ismertetett hello technikák kell használnia.
