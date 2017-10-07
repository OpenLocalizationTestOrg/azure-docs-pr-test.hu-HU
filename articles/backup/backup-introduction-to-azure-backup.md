---
title: aaaWhat az Azure Backup? | Microsoft Docs
description: "Azure biztonsági mentés tooback használja, és állítsa vissza az adatokat és a munkaterhelések Windows-kiszolgálók Windows munkaállomások, a System Center DPM-kiszolgáló és az Azure virtuális gépek."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "biztonsági mentés és visszaállítás; recovery services; biztonsági mentési megoldások"
ms.assetid: 0d2a7f08-8ade-443a-93af-440cbf7c36c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 8/11/2017
ms.author: markgal;trinadhk;anuragm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 953a19600f67a6b7451f71b1e3234d913816d18c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-hello-features-in-azure-backup"></a>Az Azure Backup hello funkcióinak áttekintése
Azure biztonsági mentés egy hello Azure-alapú szolgáltatás tooback elhasználja (vagy védelme) és az adatok a Microsoft cloud hello visszaállításához. Az Azure Backup megbízható, biztonságos és költséghatékony felhőalapú megoldással váltja fel a meglévő helyszíni vagy külső helyszínen lévő biztonsági mentési megoldást. Azure biztonsági mentés nyújt több olyan összetevőt, töltse le, és telepítését a számítógépen hello megfelelő, a kiszolgáló, vagy hello felhőben. hello összetevő vagy ügynök, központilag telepített attól függ, mit tooprotect. Minden Azure Backup szolgáltatás-összetevők (függetlenül attól, hogy számára kíván védelmet biztosítani a helyi vagy felhőalapú hello) be adatok tooa Recovery Services-tároló az Azure-ban használt tooback lehet. Lásd: hello [Azure biztonsági mentés összetevők tábla](backup-introduction-to-azure-backup.md#which-azure-backup-components-should-i-use) (Ez a cikk későbbi) melyik összetevő toouse tooprotect adott adatok, alkalmazások vagy munkaterhelések kapcsolatos információkat.

[Áttekintő videó megtekintése az Azure Backupról](https://azure.microsoft.com/documentation/videos/what-is-azure-backup/)

## <a name="why-use-azure-backup"></a>Miért érdemes az Azure Backupot használni?
Hagyományos biztonsági mentési megoldás tootreat hello felhő végpontot, vagy statikus célhelyet, hasonló toodisks vagy a szalaggal van továbbfejlődtek. Bár ez a megközelítés egyszerű, korlátozott, és nem teljes körű kihasználása egy alapul szolgáló felhőalapú platform, amely tooan költséges, nem hatékony megoldás. Más megoldások is költséges, mert a tároló-, és nem szükséges tárhelyet nem megfelelő típusú hello fizető fel. Más megoldások is gyakran nem hatékony, mert nem kínálnak hello típus vagy tárolókapacitást van szüksége, vagy a felügyeleti feladatok elvégzéséhez túl sok idő. Ezzel szemben az Azure Backup legfontosabb előnyei a következők:

**Automatikus tárolókezelési** - hibrid környezetekben gyakran megkövetelik a heterogén tárolási - bizonyos a helyszíni és az egyes hello felhő. Az Azure Backup szolgáltatással nem kell költenie helyszíni tárolóeszközökre. Az Azure Backup automatikusan foglalja le és kezeli a Backup-tárolót, és használatalapú modellt alkalmaz. Fizetési,-akkor-használható azt jelenti, hogy csak kell fizetnie hello tároló felhasznált. További információkért lásd: hello [Azure díjszabása cikk](https://azure.microsoft.com/pricing/details/backup).

**Korlátlan skálázás** - Azure biztonsági mentés használja az alapul szolgáló power hello és korlátlan méretezésének hello Azure cloud toodeliver magas rendelkezésre állás – nincs karbantartás vagy figyelési terhelést. Riasztások tooprovide eseményekről állíthat be, de nem kell az adatok hello felhőben tooworry kapcsolatos magas rendelkezésre állású.

**Többféle tárolási lehetőség** – A magas rendelkezésre állás egyik megoldása a tárolóreplikáció. Az Azure Backup két replikációtípust nyújt: [helyileg redundáns tárolást](../storage/common/storage-redundancy.md#locally-redundant-storage) és [georedundáns tárolást](../storage/common/storage-redundancy.md#geo-redundant-storage). A beállítással hello biztonsági másolatok tárolási igények alapján:

* Helyileg redundáns tárolás (LRS) replikálja az adatokat a hello párosított adatközpontban (létrehozza az adatok három példányban) három alkalommal ugyanabban a régióban. Az LRS egy alacsony költségű megoldás az adatok védelmére a helyi hardveres hibák esetén.

* Georedundáns tárolás (GRS) replikálja az adatokat tooa másodlagos régióba (több száz miles hello forrásadatok hello elsődleges helye el). A GRS módszer költségesebb, mint az LRS, de adatainak megőrzését magasabb szinten biztosítja, még regionális szolgáltatáskiesés esetére is.

**Korlátlan adatforgalom** - Azure biztonsági mentés nem korlátozásához hello a bejövő vagy kimenő adatok átvitele. Azure biztonsági mentés is nem terheli a hello továbbított adatokat. Azonban hello Azure Import/Export szolgáltatás tooimport nagy mennyiségű adatot használhat, ha nincs a bejövő adatok költsége. Ennek költségére vonatkozóan [az offline biztonsági mentésnek az Azure Backup szolgáltatásban alkalmazott munkafolyamatát](backup-azure-backup-import-export.md) ismertető cikkben talál bővebb információt. Kimenő adatforgalmat a Recovery Services-tároló a visszaállítási művelet során toodata hivatkozik.

**Adattitkosítás** -adattitkosítás lehetővé teszi a biztonságos adatátvitelhez és a nyilvános felhőben hello adatok tárolására. Hello titkosítási jelszó helyileg tárolja, és soha nem küldött vagy az Azure-ban tárolja. Ha szükséges toorestore minden hello adatokat, csak hogy titkosítási jelszót, vagy kulcsát.

**Alkalmazáskonzisztens biztonsági mentését** -adatok toorestore hello biztonsági másolatot egy fájlkiszolgálón, a virtuális gép vagy az SQL-adatbázis biztonsági másolatának, kell-e, hogy a helyreállítási pont rendelkezik az összes tooknow szükséges. Azure biztonsági mentés alkalmazáskonzisztens biztonsági mentések, amelyek biztosítani további javításokat nem szükséges toorestore hello adatokat biztosít. Alkalmazás konzisztens adat-visszaállítást a csökkenti a hello visszaállítás időt, így tooquickly visszatérési tooa futó állapotban.

**Hosszú távú megőrzési** -váltás lemez tootape és a mozgási hello szalag tooan telephelytől távoli helyszínen biztonsági másolatot, helyett használhat Azure rövid és hosszú távú megőrzési. Azure hello mennyi ideig adatok maradnak a biztonsági mentési vagy helyreállítási szolgáltatások tárolóban nem korlátozza. Tetszőleges ideig őrizheti meg az adatokat a tárolókban. Az Azure Backup védett példányonként 9999 helyreállítási pontos felső határral rendelkezik. Lásd: hello [biztonsági mentési és adatmegőrzési](backup-introduction-to-azure-backup.md#backup-and-retention) szakasz ebben a cikkben az annak magyarázatát, hogy ezt a határt hatással lehet a biztonsági mentési igényeit.  

## <a name="which-azure-backup-components-should-i-use"></a>Melyik Azure Backup-összetevőt használjam?
Ha nem biztos mely Azure biztonsági mentés összetevője működik-e az igényeinek, tekintse meg a következő táblázat az egyes összetevők is védeni információt hello. hello Azure-portálon biztosít, beépített hello portált, tooguide végig választva összetevő toodownload hello és központi telepítése varázsló. hello varázslót, amely része hello Recovery Services-tároló létrehozását, részletes útmutatást nyújt egy biztonsági mentési cél kiválasztásához, és hello adat vagy alkalmazás tooprotect kiválasztása hello lépéseket.

| Összetevő | Előnyök | Korlátok | Mi van védve? | Hol tárolja a biztonsági mentéseket? |
| --- | --- | --- | --- | --- |
| Azure Backup (MARS) ügynöke |<li>Elkészíti a fizikai vagy virtuális Windows operációs rendszereken lévő fájlok és mappák biztonsági másolatát (a virtuális gépek lehetnek helyszíniek, vagy lehetnek az Azure-ban is)<li>Nincs szükség különálló biztonsági mentési kiszolgálóra. |<li>Biztonsági mentés naponta 3-szor. <li>Nem alkalmazásfüggő; csak fájl-/mappa-/kötetszintű visszaállítás. <li>  Nincs Linux-támogatás. |<li>Fájlok <li>Mappák |Recovery Services-tároló |
| System Center DPM |<li>Alkalmazásfüggő pillanatképek (VSS)<li>A rugalmasság teljes tootake biztonsági mentések<li>Helyreállítás részletessége (összes)<li>Használható a Recovery Services-tároló<li>Linux-támogatás Hyper-V és VMware virtuális gépeken <li>VMware virtuális gépek biztonsági mentése és visszaállítása a DPM 2012 R2-es verziójával |Nem készíthető biztonsági mentés az Oracle számítási feladatról.|<li>Fájlok <li>Mappák<li> Kötetek <li>Virtuális gépek<li> Alkalmazások<li> Számítási feladatok |<li>Recovery Services-tároló,<li> Helyileg csatlakoztatott lemez,<li>  Szalag (csak helyszíni) |
| Azure Backup Server |<li>Alkalmazásfüggő pillanatképek (VSS)<li>A rugalmasság teljes tootake biztonsági mentések<li>Helyreállítás részletessége (összes)<li>Használható a Recovery Services-tároló<li>Linux-támogatás Hyper-V és VMware virtuális gépeken<li>VMware virtuális gépek biztonsági mentése és visszaállítása <li>Nincs szükség System Center-licencre |<li>Nem készíthető biztonsági mentés az Oracle számítási feladatról.<li>Mindig élő Azure-előfizetést igényel<li>A szalagos biztonsági mentés nem támogatott |<li>Fájlok <li>Mappák<li> Kötetek <li>Virtuális gépek<li> Alkalmazások<li> Számítási feladatok |<li>Recovery Services-tároló,<li> Helyileg csatlakoztatott lemez |
| Azure IaaS virtuális gép biztonsági mentése |<li>Natív biztonsági mentések Windowshoz/Linuxhoz<li>Nincs szükség speciális ügynök telepítésére<li>Hálószintű biztonsági mentés, nincs szükség biztonsági mentési infrastruktúrára |<li>Virtuális gépek napi biztonsági mentése <li>Virtuális gépek visszaállítása csak lemezszinten<li>Nem készíthető biztonsági mentés a helyszínen |<li>Virtuális gépek <li>Minden lemez (PowerShell használatával) |<p>Recovery Services-tároló</p> |

## <a name="what-are-hello-deployment-scenarios-for-each-component"></a>Mik azok a hello szituációkban az egyes összetevők?
| Összetevő | Üzembe helyezhető az Azure-ban? | Üzembe helyezhető a helyszínen? | A céltároló támogatott |
| --- | --- | --- | --- |
| Azure Backup (MARS) ügynöke |<p>**Igen**</p> <p>hello Azure Backup szolgáltatás ügynökének is telepíthető a Windows Server virtuális Gépen, az Azure-ban futó.</p> |<p>**Igen**</p> <p>hello Backup szolgáltatás ügynökének telepíthető bármilyen Windows Server virtuális gép vagy fizikai gépen.</p> |<p>Recovery Services-tároló</p> |
| System Center DPM |<p>**Igen**</p><p>További információ [hogyan tooprotect munkaterhelések az Azure-ban a System Center DPM](backup-azure-dpm-introduction.md).</p> |<p>**Igen**</p> <p>További információ [hogyan tooprotect számítási feladatok és az adatközpontban található virtuális gépek](https://technet.microsoft.com/system-center-docs/dpm/data-protection-manager).</p> |<p>Helyileg csatlakoztatott lemez,</p> <p>Recovery Services-tároló,</p> <p>szalag (csak helyszíni)</p> |
| Azure Backup Server |<p>**Igen**</p><p>További információ [hogyan tooprotect munkaterhelések az Azure-ban Azure Backup Server használatával](backup-azure-microsoft-azure-backup.md).</p> |<p>**Igen**</p> <p>További információ [hogyan tooprotect munkaterhelések az Azure-ban Azure Backup Server használatával](backup-azure-microsoft-azure-backup.md).</p> |<p>Helyileg csatlakoztatott lemez,</p> <p>Recovery Services-tároló</p> |
| Azure IaaS virtuális gép biztonsági mentése |<p>**Igen**</p><p>Az Azure-háló része</p><p>Az [Azure szolgáltatásként kínált infrastruktúra (IaaS) rendszerű virtuális gépek biztonsági mentéséhez](backup-azure-vms-introduction.md) készült.</p> |<p>**Nem**</p> <p>Használja a System Center DPM tooback virtuális gépek az adatközpontban található.</p> |<p>Recovery Services-tároló</p> |

## <a name="which-applications-and-workloads-can-be-backed-up"></a>Melyik alkalmazásokról és számítási feladatokról készíthető biztonsági mentés?
a következő táblázat hello foglalja össze hello adatok és az Azure Backup segítségével védhető munkaterhelések. hello Azure biztonsági mentési megoldás oszlop hivatkozások toohello központi telepítési dokumentációjában az adott megoldáshoz tartozó rendelkezik. Minden Azure Backup-összetevő klasszikus (Service Manager-alapú telepítés) és Resource Manager-alapú üzemi modell környezetben is telepíthető.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

| Adat vagy számítási feladat | Forráskörnyezet | Azure Backup-megoldás |
| --- | --- | --- |
| Fájlok és mappák |Windows Server |<p>[Az Azure Backup ügynöke](backup-configure-vault.md),</p> <p>[A System Center DPM](backup-azure-dpm-introduction.md) (+ hello Azure Backup szolgáltatás ügynökének),</p> <p>[Az Azure Backup Server](backup-azure-microsoft-azure-backup.md) (hello Azure Backup szolgáltatás ügynökének tartalmaz)</p> |
| Fájlok és mappák |Windows rendszerű számítógép |<p>[Az Azure Backup ügynöke](backup-configure-vault.md),</p> <p>[A System Center DPM](backup-azure-dpm-introduction.md) (+ hello Azure Backup szolgáltatás ügynökének),</p> <p>[Az Azure Backup Server](backup-azure-microsoft-azure-backup.md) (hello Azure Backup szolgáltatás ügynökének tartalmaz)</p> |
| Hyper-V virtuális gép (Windows) |Windows Server |<p>[A System Center DPM](backup-azure-backup-sql.md) (+ hello Azure Backup szolgáltatás ügynökének),</p> <p>[Az Azure Backup Server](backup-azure-microsoft-azure-backup.md) (hello Azure Backup szolgáltatás ügynökének tartalmaz)</p> |
| Hyper-V virtuális gép (Linux) |Windows Server |<p>[A System Center DPM](backup-azure-backup-sql.md) (+ hello Azure Backup szolgáltatás ügynökének),</p> <p>[Az Azure Backup Server](backup-azure-microsoft-azure-backup.md) (hello Azure Backup szolgáltatás ügynökének tartalmaz)</p> |
| Microsoft SQL Server |Windows Server |<p>[A System Center DPM](backup-azure-backup-sql.md) (+ hello Azure Backup szolgáltatás ügynökének),</p> <p>[Az Azure Backup Server](backup-azure-microsoft-azure-backup.md) (hello Azure Backup szolgáltatás ügynökének tartalmaz)</p> |
| Microsoft SharePoint |Windows Server |<p>[A System Center DPM](backup-azure-backup-sql.md) (+ hello Azure Backup szolgáltatás ügynökének),</p> <p>[Az Azure Backup Server](backup-azure-microsoft-azure-backup.md) (hello Azure Backup szolgáltatás ügynökének tartalmaz)</p> |
| Microsoft Exchange |Windows Server |<p>[A System Center DPM](backup-azure-backup-sql.md) (+ hello Azure Backup szolgáltatás ügynökének),</p> <p>[Az Azure Backup Server](backup-azure-microsoft-azure-backup.md) (hello Azure Backup szolgáltatás ügynökének tartalmaz)</p> |
| Azure IaaS virtuális gépek (Windows) |Azure-ban futó |[Azure Backup (virtuálisgép-bővítmény)](backup-azure-vms-introduction.md) |
| Azure IaaS virtuális gépek (Linux) |Azure-ban futó |[Azure Backup (virtuálisgép-bővítmény)](backup-azure-vms-introduction.md) |

## <a name="linux-support"></a>Linux-támogatás
hello alábbi táblázat ismerteti, amelyek támogatják a Linux hello Azure Backup szolgáltatás-összetevők.  

| Összetevő | Linux (Azure által támogatott) támogatása |
| --- | --- |
| Azure Backup (MARS) ügynöke |Nem (csak Windows-alapú ügynök) |
| System Center DPM |<li> Hyper-V és VMware virtuális gépek Linux rendszerű vendég virtuális gépeinek fájlkonzisztens biztonsági mentése<br/> <li> Hyper-V és VMware virtuális gépek Linux rendszerű vendég virtuális gépeinek visszaállítása </br> </br>  *Azure-beli virtuális gépekhez nem érhető el fájlkonzisztens biztonsági mentés* <br/> |
| Azure Backup Server |<li>Hyper-V és VMware virtuális gépek Linux rendszerű vendég virtuális gépeinek fájlkonzisztens biztonsági mentése<br/> <li> Hyper-V és VMware virtuális gépek Linux rendszerű vendég virtuális gépeinek visszaállítása </br></br> *Azure-beli virtuális gépekhez nem érhető el fájlkonzisztens biztonsági mentés*  |
| Azure IaaS virtuális gép biztonsági mentése |Alkalmazáskonzisztens biztonsági mentés [szkript előtti és utáni keretrendszerrel](backup-azure-linux-app-consistent.md)<br/> [Részletes fájlhelyreállítás](backup-azure-restore-files-from-vm.md)<br/> [Az összes virtuálisgép-lemez visszaállítása](backup-azure-arm-restore-vms.md#restore-backed-up-disks)<br/> [Virtuális gép visszaállítása](backup-azure-arm-restore-vms.md#create-a-new-vm-from-restore-point) |

## <a name="using-premium-storage-vms-with-azure-backup"></a>Premium Storage virtuális gépek használata az Azure Backup szolgáltatással
Az Azure Backup szolgáltatás a Premium Storage virtuális gépek védelmét is biztosítja. Prémium szintű Storage az SSD-meghajtót (SSD)-alapú tárolási tervezett toosupport I/O-igényes munkaterhelések. A Premium Storage a virtuális gépek számítási feladataihoz kínál vonzó megoldást. Prémium szintű Storage kapcsolatos további információkért tekintse meg a hello cikket, [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../storage/common/storage-premium-storage.md).

### <a name="back-up-premium-storage-vms"></a>A Premium Storage virtuális gépek biztonsági mentése
Prémium szintű tároló virtuális gépek, hello biztonsági mentési szolgáltatás biztonsági mentéséről hoz létre egy ideiglenes átmeneti helyre, miközben nevű "Azurebiztonsági mentés-", hello prémium szintű Storage-fiók. hello átmeneti helyre hello mérete hello helyreállítási pont pillanatképet egyenlő toohello méretét. Győződjön meg arról, hogy hello prémium szintű tárfiók van elegendő szabad terület tooaccommodate hello ideiglenes átmeneti helyre. További információkért lásd: hello cikk [prémium szintű storage korlátozások](../storage/common/storage-premium-storage.md#scalability-and-performance-targets). Hello biztonsági mentési feladat befejeződik, ha átmeneti helyre hello törlődik. hello átmeneti helyre hello használt tároló árának összhangban a összes [prémium szintű storage szolgáltatás díjszabása](../storage/common/storage-premium-storage.md#pricing-and-billing).

> [!NOTE]
> Nem módosíthatók átmeneti helyre hello szerkesztése.
>
>

### <a name="restore-premium-storage-vms"></a>A Premium Storage virtuális gépek visszaállítása
Prémium szintű tároló virtuális gépek visszaállított tooeither prémium szintű Storage vagy toonormal tároló lehet. A prémium szintű Storage virtuális gép helyreállítási pont hátsó tooPremium tárolási visszaállítása az hello tipikus folyamat visszaállítása. Azonban a prémium szintű Storage virtuális gép helyreállítási pont toostandard storage költséghatékony toorestore lehet. Visszaállítás az ilyen típusú használható, ha hello Virtuálisgép-fájlok részhalmazát kell.

## <a name="using-managed-disk-vms-with-azure-backup"></a>Felügyelt lemezes virtuális gépek használata az Azure Backuppal
Az Azure Backup védelmet biztosít a felügyelt lemezes virtuális gépek számára. A felügyelt lemezek használatával mentesül a virtuális gépek tárfiókjainak kezelése alól, és lényegesen leegyszerűsödik a virtuális gépek üzembe helyezése.

### <a name="back-up-managed-disk-vms"></a>Felügyelt lemezes virtuális gépek biztonsági mentése
A felügyelt lemezeken található virtuális gépek biztonsági mentése megegyezik a Resource Manager-alapú virtuális gépek biztonsági mentésével. Hello Azure-portálon, a biztonsági mentési feladat hello közvetlenül a virtuális gép nézet hello konfigurálása, vagy hello helyreállítási szolgáltatásokból tároló nézet. A felügyelt lemezeken található virtuális gépek biztonsági mentését a felügyelt lemezeken kiépített visszaállításipont-gyűjteményekkel végezheti el. Az Azure Backup támogatja az Azure Disk Encryption (ADE) használatával titkosított, felügyelt lemezes virtuális gépek biztonsági mentését is.

### <a name="restore-managed-disk-vms"></a>Felügyelt lemezes virtuális gépek visszaállítása
Azure biztonsági mentés lehetővé teszi teljes kezelt lemezzel rendelkező virtuális gép toorestore, vagy visszaállítási felügyelt lemezek tooa tárfiók. Azure által felügyelt hello lemezek kezelése hello visszaállítási folyamat során. (Az ügyfél hello) kezelheti az hello visszaállítási folyamat részeként létrehozott hello tárfiók. Ha titkosított virtuális gépek helyreállítása kezelése, hello a virtuális gép kulcsok és titkos léteznie kell hello kulcstároló előzetes toostarting hello visszaállítási művelet során.

## <a name="what-are-hello-features-of-each-backup-component"></a>Mik azok a hello szolgáltatásokat minden egyes biztonsági másolat összetevő?
a következő szakaszok hello adja meg a táblázatot, amely összefoglalója hello rendelkezésre állási vagy az egyes Azure biztonsági mentés összetevője különféle funkcióinak támogatásához. Tekintse meg a következő minden táblázat további támogatási vagy részletek hello információkat.

### <a name="storage"></a>Storage
| Szolgáltatás | Az Azure Backup ügynöke | System Center DPM | Azure Backup Server | Azure IaaS virtuális gép biztonsági mentése |
| --- | --- | --- | --- | --- |
| Recovery Services-tároló |![Igen][green] |![Igen][green] |![Igen][green] |![Igen][green] |
| Lemezes tárolás | |![Igen][green] |![Igen][green] | |
| Szalagos tárolás | |![Igen][green] | | |
| Tömörítés <br/>(a Recovery Services-tárolóban) |![Igen][green] |![Igen][green] |![Igen][green] | |
| Növekményes biztonsági mentés |![Igen][green] |![Igen][green] |![Igen][green] |![Igen][green] |
| Lemezdeduplikáció | |![Részlegesen][yellow] |![Részlegesen][yellow] | | |

![tábla kulcsa](./media/backup-introduction-to-azure-backup/table-key.png)

Recovery Services-tároló hello célja hello előnyben részesített tárolási termékcsalád összes tagjára. A System Center DPM, az Azure Backup Server is meg hello beállítás toohave az helyi átmásolni. Azonban csak a System Center DPM biztosít hello beállítás toowrite tooa szalagos adattároló eszköz.

#### <a name="compression"></a>Tömörítés
Biztonsági mentések tömörített tooreduce hello tárolóhely szükséges. hello csak tömörítés nem használó összetevője hello Virtuálisgép-bővítmény. a tárolási fiók toohello Recovery Services biztonsági mentési adatait tároló a hello VM bővítmény másolatok hello ugyanabban a régióban. Nincs tömörítés hello adatátvitel során használatos. Adatátvitel hello tömörítés nélkül némileg inflates hello tárhelyet. Azonban hello adatainak tárolásához tömörítési lehetővé teszi, hogy gyorsabb visszaállítás nélkül, szüksége van, az adott helyreállítási pont.


#### <a name="disk-deduplication"></a>Lemezdeduplikáció
A deduplikáció nyújtotta előnyöket a System Center DPM vagy az Azure Backup Server [Hyper-V virtuális gépeken](http://blogs.technet.com/b/dpm/archive/2015/01/06/deduplication-of-dpm-storage-reduce-dpm-storage-consumption.aspx) való üzembe helyezése esetén használhatja ki. Windows Server virtuális merevlemezeket (VHD), amelyek a virtuális géphez csatolt toohello biztonsági másolatok tárolásának végzi, az adatdeduplikáció (hello gazdagép szintjén).

> [!NOTE]
> A deduplikáció az Azure-ban egyik Backup-összetevőhöz sem érhető el. Ha a System Center DPM és a kiszolgáló biztonsági mentése az Azure-ban vannak telepítve, a hello tárolólemezek csatolt virtuális gép nem deduplikált toohello.
>
>

### <a name="incremental-backup-explained"></a>A növekményes biztonsági mentés bemutatása
Minden Azure biztonságimásolat-összetevő támogatja a növekményes biztonsági mentés függetlenül hello céltárolóhoz (lemez, szalag, Recovery Services-tároló). A növekményes biztonsági mentés biztosítja, hogy biztonsági mentések tárolására és az idő hatékony, úgy, hogy csak a hello utolsó biztonsági mentés óta végzett módosítások.

#### <a name="comparing-full-differential-and-incremental-backup"></a>A teljes, a különbségi és a növekményes biztonsági mentés összehasonlítása

A tárhelyhasználat, a helyreállítási időre vonatkozó célkitűzés (RTO), és a hálózatleterheltség eltér az egyes biztonsági mentési módszerek esetében. tookeep hello biztonsági mentési teljes birtoklási költség (TCO) le, kell toounderstand hogyan toochoose hello legjobb biztonsági megoldás. a következő kép hello összehasonlítja a teljes biztonsági mentés, a különbözeti biztonsági másolat és a növekményes biztonsági mentést. Hello kép az adatforrás A 10 tárhely áll blokkol A1-A10, amely havi biztonsági. Blokkok A2, a3 méretű, A4 és A9 változása hello első hónapja, és hello A5 változásai blokkolja a következő hónap.

![a biztonsági mentési módszerek összehasonlítását mutató kép](./media/backup-introduction-to-azure-backup/backup-method-comparison.png)

A **teljes biztonsági mentés**, a biztonsági másolat hello teljes adatforrást tartalmaz. A teljes biztonsági mentés minden egyes alkalommal nagy hálózati sávszélességet és tárterületet igényel a biztonsági másolat továbbításakor.

**Különbözeti biztonsági másolat** csak hello megakadályozza, hogy módosultak a hello kezdeti teljes biztonsági mentés óta, így ez a hálózat- és felhasználási kisebb mennyiségű tárolja. A különbségi biztonsági mentések nem őrzik meg a változatlan adatok redundáns másolatait. Azonban mivel további biztonsági mentések közötti változatlanok hello adatblokkok át, és a tárolt, különbözeti biztonsági mentések nem hatékony. A hello második hónap A2, a3 méretű, A4 és A9 a módosult blokkok készül biztonsági másolat. A hello harmadik hónapban, ezek azonos blokkok készül biztonsági másolat ebben az esetben módosított blokk A5 együtt. módosított blokkjait hello hello következő teljes biztonsági mentés akkor fordul elő, amíg a biztonsági toobe továbbra is.

**A növekményes biztonsági mentés** éri el a magas tárolási és hálózati hatékonyság csak hello blokkok hello korábbi biztonsági mentés óta megváltoztak adattípusnak elhelyezésével. Növekményes biztonsági mentéssel nincs szükség tootake rendszeres teljes biztonsági mentés. Hello példában után hello a teljes biztonsági mentés hello első hónapja, történik a A2, a3 méretű, A4 és A9 fel van tüntetve a módosult blokkok megváltozott, és második hónap során továbbított hello. A hello harmadik hónapban csak módosítani blokk A5 van megjelölve, és továbbítja. A kevesebb adat mozgatásával tárterület és hálózati erőforrások takaríthatóak meg, és így csökken a teljes birtoklási költség.   

### <a name="security"></a>Biztonság
| Szolgáltatás | Az Azure Backup ügynöke | System Center DPM | Azure Backup Server | Azure IaaS virtuális gép biztonsági mentése |
| --- | --- | --- | --- | --- |
| Hálózati biztonság<br/> (tooAzure) |![Igen][green] |![Igen][green] |![Igen][green] |![Részlegesen][yellow] |
| Adatbiztonság<br/> (az Azure-ban) |![Igen][green] |![Igen][green] |![Igen][green] |![Részlegesen][yellow] |

![tábla kulcsa](./media/backup-introduction-to-azure-backup/table-key.png)

#### <a name="network-security"></a>Hálózati biztonság
A Recovery Services-tároló kiszolgálók toohello teljes biztonsági mentés forgalmát Advanced Encryption Standard 256 titkosított. hello biztonsági mentési adatok a biztonságos HTTPS-kapcsolaton keresztül zajlik. hello biztonsági mentési adatok is hello Recovery Services-tároló titkosított formában tárolja. Csak, hello Azure ügyfél, hogy hello jelszót toounlock ezeket az adatokat. A Microsoft hello biztonsági mentési adatok bármikor nem fejthető vissza.

> [!WARNING]
> Miután hello Recovery Services-tároló, csak hogy hozzáférés toohello titkosítási kulcs. Microsoft soha nem tart fenn a titkosítási kulcs másolatával, és nem rendelkezik hozzáféréssel toohello kulccsal. Hello kulcs helytelen, ha az Microsoft hello biztonsági mentési adatok nem állíthatók vissza.
>
>

#### <a name="data-security"></a>Adatbiztonság
Azure virtuális gépek biztonsági mentéséről be kell állítani titkosítási *belül* hello virtuális gépet. Használja a BitLockert Windows rendszerű virtuális gépeken és a **dm-crypt**-et Linux rendszerű virtuális gépeken. Az Azure Backup nem titkosítja automatikusan az ezen az elérési úton bejövő biztonsági mentési adatokat.

### <a name="network"></a>Network (Hálózat)
| Szolgáltatás | Az Azure Backup ügynöke | System Center DPM | Azure Backup Server | Azure IaaS virtuális gép biztonsági mentése |
| --- | --- | --- | --- | --- |
| Hálózati tömörítés <br/>(túl**tartalékkiszolgálóra**) | |![Igen][green] |![Igen][green] | |
| Hálózati tömörítés <br/>(túl**Recovery Services-tároló**) |![Igen][green] |![Igen][green] |![Igen][green] | |
| Hálózati protokoll <br/>(túl**tartalékkiszolgálóra**) | |TCP |TCP | |
| Hálózati protokoll <br/>(túl**Recovery Services-tároló**) |HTTPS |HTTPS |HTTPS |HTTPS |

![tábla kulcsa](./media/backup-introduction-to-azure-backup/table-key-2.png)

Virtuálisgép-bővítmény (az infrastruktúra-szolgáltatási virtuális gép hello) hello hello adatokat olvas közvetlenül hello Azure storage-fiók hello tárolási hálózaton keresztül, így nem szükséges toocompress a forgalmat.

Ha használja a System Center DPM-kiszolgáló vagy az Azure Backup Server a másodlagos helykiszolgáló biztonsági mentése, a hello elsődleges kiszolgáló toohello tartalék kiszolgáló üzembe helyezésről hello adatok tömörítése. Az adatok tömörítése a biztonsági mentés tooDPM vagy az Azure Backup Server előtt, és a sávszélességet takaríthat meg.

#### <a name="network-throttling"></a>A hálózati sávszélesség szabályozása
hello Azure Backup szolgáltatás ügynökének kínál a hálózati sávszélesség-szabályozás, amellyel toocontrol hálózati sávszélesség használatának adatátvitel során. Sávszélesség-szabályozás akkor lehet hasznos, ha tooback adatokat kell munkaidőben, de nem hello biztonsági mentési folyamat toointerfere a más internetes forgalmat. Szabályozást az adatok átvitel mentése tooback vonatkozik, és állítsa vissza a tevékenységek.

## <a name="backup-and-retention"></a>Biztonsági mentés és megőrzés

Az Azure Backup *védett példányonként* 9999 helyreállítási pontos felső határral rendelkezik. Ezeket a helyreállítási pontokat biztonsági másolatoknak, illetve pillanatképeknek is hívják. Egy védett példány egy számítógép, a server (fizikai vagy virtuális) vagy a munkaterhelés konfigurált tooback adatok tooAzure fel. További információkért lásd: hello szakasz [Mi az, hogy a védett példánya](backup-introduction-to-azure-backup.md#what-is-a-protected-instance). Az egyes példányok védelméhez biztonsági másolatot kell készíteni az adatokról. adatok biztonsági másolatát hello hello védelmi. Ha hello forrásadatok megszakadt, vagy megsérült, hello biztonsági másolat visszaállítása volt hello forrásadatok. hello következő táblázat az egyes összetevők biztonsági mentés gyakoriságának maximális hello. A biztonsági mentési házirend konfigurációja határozza meg, milyen gyorsan felhasznált hello helyreállítási pontokat. Ha például minden nap létrehoz egy helyreállítási pontot, akkor 27 év után fogynának el a helyreállítási pontjai. Havi helyreállítási pont létrehozása igénybe vehet, ha a helyreállítási pontok megőrzése mellett 833 év futtatása előtt. biztonsági mentési szolgáltatás hello található helyreállítási pont nem állítja be egy lejárati idő korlátot.

|  | Az Azure Backup ügynöke | System Center DPM | Azure Backup Server | Azure IaaS virtuális gép biztonsági mentése |
| --- | --- | --- | --- | --- |
| Biztonsági mentés gyakorisága<br/> (tooRecovery szolgáltatások tároló) |Napi három biztonsági mentés |Napi két biztonsági mentés |Napi két biztonsági mentés |Napi egy biztonsági mentés |
| Biztonsági mentés gyakorisága<br/> (toodisk) |Nem alkalmazható |<li>15 percenként az SQL Serverhez <li>Minden órában más számítási feladatokhoz |<li>15 percenként az SQL Serverhez <li>Minden órában más számítási feladatokhoz</p> |Nem alkalmazható |
| Megőrzési beállítások |Napi, heti, havi, éves |Napi, heti, havi, éves |Napi, heti, havi, éves |Napi, heti, havi, éves |
| Helyreállítási pontok maximális száma védett példányonként |9999|9999|9999|9999|
| Maximális megőrzési időtartam |A biztonsági mentés gyakoriságától függően változik |A biztonsági mentés gyakoriságától függően változik |A biztonsági mentés gyakoriságától függően változik |A biztonsági mentés gyakoriságától függően változik |
| Helyreállítási pontok a helyi lemezen |Nem alkalmazható |<li>64 fájlkiszolgálókhoz,<li>448 alkalmazáskiszolgálókhoz |<li>64 fájlkiszolgálókhoz,<li>448 alkalmazáskiszolgálókhoz |Nem alkalmazható |
| Helyreállítási pontok a szalagon |Nem alkalmazható |Korlátlan |Nem alkalmazható |Nem alkalmazható |

## <a name="what-is-a-protected-instance"></a>Mi az a védett példány?
Egy védett példány általános referencia tooa Windows-számítógép, kiszolgáló (fizikai vagy virtuális) vagy SQL adatbázis töltött fel tooAzure konfigurált tooback. Egy példány védett hello számítógép, a kiszolgáló vagy az adatbázis biztonsági mentési házirend konfigurálása, és hello adatok biztonsági másolatának létrehozása után. További példányait hello biztonsági mentési adatokat, hogy védett-példány (amely nevezzük helyreállítási pontok), növelje a felhasznált tárhely hello mennyiségét. Létrehozhat too9999 helyreállítási pontokat a védett példány mentése. Ha a helyreállítási pont törlése a tárolóból, a számláló nem hello 9999 helyreállítási pont teljes ellen.
Néhány gyakori példán védett példányok a virtuális gépek, alkalmazás-kiszolgálókat, adatbázisok és személyi számítógépek hello Windows operációs rendszer. Példa:

* A Hyper-V vagy Azure IaaS hello hipervizor háló rendszerű virtuális gép. hello vendég operációs rendszerek hello virtuális gép Windows Server vagy Linux lehet.
* Az alkalmazáskiszolgáló: hello kiszolgáló lehet egy fizikai vagy virtuális géphez, Windows Server és a munkaterhelések, amelyet a biztonsági mentés toobe adatokkal. Általános munkaterheléseket a Microsoft SQL Server, Microsoft Exchange server, Microsoft SharePoint server és hello fájlkiszolgáló szerepkört a Windows Server rendszer. tooback fel az ilyen terhelések a System Center Data Protection Manager (DPM) vagy az Azure Backup Server szükséges.
* A személyes, munkaállomás, vagy hordozható számítógépre hello Windows operációs rendszer.


## <a name="what-is-a-recovery-services-vault"></a>Mi az a Recovery Services-tároló?
Recovery Services-tároló egy online tárolási entitás az Azure-ban használt toohold adatok, például a biztonsági másolat, a helyreállítási pontokat és a biztonsági mentési házirendek. Recovery Services-tárolók toohold biztonsági mentési adatok Azure-szolgáltatások és a helyszíni kiszolgálók és munkaállomások is használhatja. Helyreállítási szolgáltatások tárolók egyszerűen tooorganize révén a biztonsági mentési adatokat, ugyanakkor minimalizálja a kezelési terhelés mellett. Egy előfizetésen belül tetszőleges számú Recovery Services-tárolót hozhat létre.

Mentési tárolók, amely az Azure Service Manager alapul, hello első verzió hello tároló volt. Helyreállítási szolgáltatások tárolók, amelyek hello Azure Resource Manager modellt szolgáltatások hozzáadása, hello második verziója hello tárolóban. Lásd: hello [Recovery Services-tároló cikk áttekintése](backup-azure-recovery-services-vault-overview.md) hello szolgáltatások eltérései teljes leírását. Ön nem hozhat létre használata hello portál toocreate mentési tárolókban, de a biztonsági mentési tárolók továbbra is támogatottak.

> [!IMPORTANT]
> Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban. További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md). A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.<br/> **2017. október 15**, akkor nem fog tudni toouse PowerShell toocreate mentési tárolókban. <br/> **2017. november 1-től**:
>- A többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.
>- Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon. Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.
>

## <a name="how-does-azure-backup-differ-from-azure-site-recovery"></a>Miben különbözik az Azure Backup az Azure Site Recoverytől?
Az Azure Backup és az Azure Site Recovery közös jellemzője, hogy mindkét szolgáltatás használható az adatok biztonsági mentésére és visszaállítására. Különböző szerepet töltenek be azonban a vállalkozásban az üzleti folytonosság és a vészhelyreállítás biztosítása során. Azure biztonsági mentés tooprotect használja, és részletesebb szinten adatok helyreállítását. Például ha a hordozható bemutató sérülése, használna Azure biztonsági mentés toorestore hello bemutató. Ha tooreplicate hello konfigurációit és adatait a virtuális gép egy másik adatközpont között, használja az Azure Site Recovery.

Azure biztonsági mentés védi az adatokat a helyszíni és felhőben hello. Az Azure Site Recovery koordinálja a virtuális gépek és a fizikai kiszolgálók replikálását, feladatátvételét és feladat-visszavételét. Mindkét szolgáltatás fontosak, mert a vész-helyreállítási megoldást kell tookeep az adatok biztonságáról és helyreállíthatóságáról (biztonsági mentés) *és* biztosítsa a rendelkezésre álló (helyreállítás) kimaradások esetén.

a következő fogalmak hello segítségével biztonsági mentés és katasztrófa utáni helyreállítás körül fontos döntések.

| Fogalom | Részletek | Biztonsági mentés | Vészhelyreállítás (DR) |
| --- | --- | --- | --- |
| Helyreállítási időkorlát (RPO) |Ha a helyreállítás történik toobe elfogadható adatvesztés hello mennyisége. |A biztonsági mentési megoldások elfogadható RPO-ja nagyon változó. Virtuális gépek biztonsági mentései esetén általában egy nap az RPO, míg adatbázisok biztonsági mentései esetén akár 15 perc is lehet. |A vészhelyreállítási megoldások alacsony RPO-kkal rendelkeznek. hello vész-Helyreállítási másolási mögött lehet néhány másodpercen belül vagy néhány perc múlva. |
| Helyreállítási időre vonatkozó célkitűzés (RTO) |Hello, hogy mennyi ideig tart toocomplete egy helyreállítási vagy visszaállítás. |Hello miatt nagyobb RPO, hello adatmennyiséget, hogy a biztonsági mentési megoldás tooprocess kell, általában sokkal nagyobb, amely toolonger RTO vezet. Például is igénybe vehet nap toorestore adatok szalagokról, attól függően, hogy hello időt tootransport hello szalag egy telephelytől távoli helyről. |Vész-helyreállítási megoldások kisebb RTO rendelkezik, mert több szinkronban hello forrás. Kevesebb módosításokat kell toobe feldolgozása. |
| Megőrzés |Mennyi ideig kell az adatokat a tárolt toobe |A műveleti helyreállítást igénylő forgatókönyvekben (adatsérülés, véletlen fájltörlés, az operációs rendszer hibája) az adatok biztonsági másolatát általában legfeljebb 30 napig őrzi meg a rendszer.<br>Megfelelőségi szempontból adatok módosítania kell a hónap vagy akár évek tárolt toobe. Az adatok biztonsági másolata ideális az ilyen esetekben végzett archiváláshoz. |Vész helyreállítási igényeket csak műveleti helyreállítási adatok, ami általában néhány óra vagy tooa nap fel. Hello részletes adatrögzítés használt vész-Helyreállítási megoldások, miatt vész-Helyreállítási adatok hosszú távú megőrzési a használata nem ajánlott. |

## <a name="next-steps"></a>Következő lépések
Az Azure-ban a következő útmutatók részletes, lépésenkénti, utasítások a Windows Server-adatok védelme, vagy egy virtuális gép (VM) védelmének hello egyikét használja:

* [Fájlok és mappák biztonsági mentése](backup-try-azure-backup-in-10-mins.md)
* [Azure virtuális gépek biztonsági mentése](backup-azure-vms-first-look-arm.md)

Egyéb számítási feladatok védelméről az alábbi cikkekből tájékozódhat részletesebben:

* [A Windows Server biztonsági mentése](backup-configure-vault.md)
* [Alkalmazás számítási feladatainak biztonsági mentése](backup-azure-microsoft-azure-backup.md)
* [Azure IaaS virtuális gépek biztonsági mentése](backup-azure-vms-prepare.md)

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png
