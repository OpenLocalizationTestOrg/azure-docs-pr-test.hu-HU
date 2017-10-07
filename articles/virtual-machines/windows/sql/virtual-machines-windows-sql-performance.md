---
title: "az Azure SQL Server ajánlott eljárásai aaaPerformance |} Microsoft Docs"
description: "Gyakorlati tanácsokat megfelelően a Microsoft Azure virtuális gépeken futó SQL Server teljesítményének optimalizálásához."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a0c85092-2113-4982-b73a-4e80160bac36
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/27/2017
ms.author: jroth
ms.openlocfilehash: 42ec9fbeb2dec3a654b93bbd08d666369835ee73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performance-best-practices-for-sql-server-in-azure-virtual-machines"></a>Ajánlott eljárások az SQL Server teljesítményének Azure Virtual Machines szolgáltatásbeli növeléséhez

## <a name="overview"></a>Áttekintés

Ez a témakör gyakorlati tanácsokat megfelelően a Microsoft Azure virtuális gép az SQL Server teljesítményének optimalizálásához. SQL Server rendszert futtató Azure virtuális gépeken, miközben ajánlott használatának folytatásához hello ugyanazon adatbázis teljesítményének hangolása a helyszíni környezetben alkalmazható tooSQL kiszolgálói beállításokat. Egy relációs adatbázisban, egy nyilvános felhőben hello teljesítményét, például egy virtuális gépet, és hello adatlemezek hello konfigurációjának hello méret számos tényezőtől függ.

SQL Server-lemezképek létrehozásakor [fontolja meg a virtuális gépek hello Azure-portálon a kiépítés](virtual-machines-windows-portal-sql-server-provision.md). SQL Server virtuális gépen a Resource Manager Portal hello kiépítve valósítja meg az alábbi gyakorlati tanácsok, például hello tárolási konfiguráció.

Ez a cikk összpontosít hello első *legjobb* az Azure virtuális gépeken futó SQL Server teljesítményét. Ha a terhelést szabadulnak, minden alábbi optimalizálási nem lehet szükség. Vegye figyelembe a teljesítményigény és a terhelési mintázatok, ezek a javaslatok értékeli.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="quick-check-list"></a>Gyors ellenőrzőlista tételeit

hello alábbiakban felsoroljuk a Gyorsellenőrzés optimális teljesítmény érdekében az SQL Server Azure virtuális gépeken:

| Terület | Optimalizálás. |
| --- | --- |
| [Virtuálisgép-mérettel](#vm-size-guidance) |[DS3](../../virtual-machines-windows-sizes-memory.md) vagy újabb SQL Enterprise Edition.<br/><br/>[DS2](../../virtual-machines-windows-sizes-memory.md) vagy újabb SQL Standard és Web kiadások. |
| [Tárolás](#storage-guidance) |Használjon [prémium szintű Storage](../../../storage/common/storage-premium-storage.md). Standard szintű tárolót csak fejlesztési és tesztelési célú ajánlott.<br/><br/>Tartsa hello [tárfiók](../../../storage/common/storage-create-storage-account.md) és az SQL Server virtuális gép hello ugyanabban a régióban.<br/><br/>Tiltsa le az Azure [georedundáns tárolás](../../../storage/common/storage-redundancy.md) (georeplikáció) hello tárfiók. |
| [Lemezek](#disks-guidance) |Legalább 2 használja [P30 lemezek](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets) (1. a naplófájlok; 1. az adatfájlok és a TempDB).<br/><br/>Ne használja az operációs rendszer vagy ideiglenes lemezek adatbázistár vagy naplózás.<br/><br/>Az üzemeltető hello az adatfájlok és a TempDB hello lemez(ek) olvasási gyorsítótárazás engedélyezése.<br/><br/>Ne engedélyezze a gyorsítótárazást az üzemeltető hello naplófájl lemez(ek).<br/><br/>Fontos: Hello SQL Server szolgáltatás leállítása egy Azure virtuális lemezt hello gyorsítótár beállításainak módosításakor.<br/><br/>Paritásos több Azure lemezek nőtt tooget IO adatátvitelt.<br/><br/>Formázza a dokumentált lemezfoglalás méretét. |
| [I/O](#io-guidance) |Adatbázis lap tömörítésének engedélyezéséhez.<br/><br/>Az adatfájlok azonnali fájlinicializálása engedélyezése.<br/><br/>Korlátozható, vagy tiltsa le a hello adatbázis automatikus növekedésre.<br/><br/>Tiltsa le a autoshrink hello adatbázison.<br/><br/>Helyezze át az összes adatbázisok toodata lemez, beleértve a rendszer-adatbázisokat.<br/><br/>SQL Server hiba naplózásához és követéséhez fájl könyvtárak toodata lemezek át.<br/><br/>A telepítő biztonsági másolat és az adatbázis alapértelmezett tárolási helyeit.<br/><br/>Zárolt lapok engedélyezése.<br/><br/>SQL Server teljesítményét javítások alkalmazása. |
| [Funkcióspecifikus](#feature-specific-guidance) |Készítsen biztonsági másolatot közvetlenül tooblob tároló. |

További információ a *hogyan* és *miért* toomake ezek az optimalizálások, tekintse át hello részleteit és a következő szakaszokban található útmutatást.

## <a name="vm-size-guidance"></a>Virtuális gép mérete útmutató

Bizalmas alkalmazások teljesítménye, javasoljuk, hogy használja-e hello következő [virtuális gépek méretét](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json):

* **SQL Server Enterprise Edition**: DS3 vagy újabb
* **SQL Server Standard és Web kiadások**: DS2 vagy újabb

## <a name="storage-guidance"></a>Tárolási útmutatásokkal

DS sorozatnak (valamint sorozatú DSv2 és GS sorozatnak) virtuális gépek támogatási [prémium szintű Storage](../../../storage/common/storage-premium-storage.md). Prémium szintű Storage ajánlott minden termelési számítási feladatokhoz.

> [!WARNING]
> Standard szintű tárolást különböző késések és sávszélesség rendelkezik, és csak ajánlott fejlesztési/tesztelési feladatokat. Termelési számítási feladatokhoz a prémium szintű Storage kell használnia.

Emellett azt javasoljuk, hogy a hello hozzon létre az Azure storage-fiók azonos adatközpontba, ahol az SQL Server virtuális gépek tooreduce átviteli késést. A storage-fiók létrehozásakor tiltsa le a georeplikáció, több lemezre konzisztens írási sorrendje nem garantált. Ehelyett fontolja meg, hogy egy SQL Server katasztrófa utáni helyreállítás technológia két Azure-adatközpont között. További információkért lásd: [magas rendelkezésre álláshoz és Vészhelyreállításhoz az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Lemezek útmutató

Egy Azure virtuális gépen három fő lemez típusa van:

* **Az operációsrendszer-lemez**: az Azure virtuális gép létrehozásakor hello platform kapcsolódni fog-e legalább egy lemez (hello címkézve **C** meghajtó) az operációsrendszer-lemez a virtuális gép toohello. Ez a lemez tárolási oldalakra vonatkozó blob tárolt virtuális merevlemez.
* **Ideiglenes lemez**: Azure virtuális gépek tartalmazhat hello mennyiségű ideiglenes lemezes nevű másik lemezre (hello címkézve **D**: meghajtó). Ez az egy lemezt, amely nem használható ideiglenes terület hello csomóponton.
* **Az adatlemezek**: adatok lemezként is csatolhat további lemezek tooyour virtuális gép, és ezek fogja tárolni Storage lapblobokat.

hello következő szakaszok ismertetik a különböző lemezek használatához.

### <a name="operating-system-disk"></a>Operációsrendszer-lemez

Operációsrendszer-lemez a virtuális Merevlemezt, amely a rendszerindító és a csatlakoztatási az operációs rendszer fut verzióként és van-e címkézve **C** meghajtó.

Alapértelmezés szerint a gyorsítótárazási házirend hello operációsrendszer-lemez a **olvasási/írási**. Érzékeny alkalmazások teljesítménye azt javasoljuk, hogy használja-e adatlemezek hello operációsrendszer-lemez helyett. Című rész hello az alábbi adatok lemezeken.

### <a name="temporary-disk"></a>Ideiglenes lemez

ideiglenes tárolómeghajtókhoz hello címkézve hello **D**: meghajtó, nem megőrzött tooAzure blob Storage tárolóban. Ne tárolja a felhasználói adatbázis-fájlokat vagy a felhasználói tranzakció naplófájlokat hello **D**: meghajtó.

D sorozatú Dv2-sorozat és G sorozatú virtuális gépek virtuális gépeken ideiglenes meghajtóján hello esetén SSD-alapú. Ha a terhelést a TempDB nehéz használatát (pl. az ideiglenes objektumok vagy bonyolult illesztésekre), a TempDB tároló hello **D** meghajtó nem sikerült újabb TempDB átviteli sebességet eredményez, és a TempDB késés csökkentése.

Virtuális gépekhez, amely támogatja a prémium szintű Storage (DS-méretek, DSv2-sorozat és GS sorozatnak) azt javasoljuk, amely támogatja a prémium szintű Storage engedélyezve olvasási gyorsítótárazás lemezre TempDB tárolására. Van egy kivétel toothis javaslat; Ha a TempDB használata írási-igényes, nagyobb teljesítményt érhet el, a TempDB hello helyi tárolása **D** meghajtót, amely egyben a ezek méreteket SSD-alapú.

### <a name="data-disks"></a>Az adatlemezek

* **Adatlemezek használja az adatok és a naplófájlok**: legalább 2 prémium szintű Storage használata [P30 lemezek](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets) ahol egy hello naplófájl (oka) t tartalmaz, és más hello hello adatok és a TempDB adatbázis (oka) t tartalmaz. Minden prémium szintű tároló lemez számos iops-érték és a sávszélesség (MB/s) attól függően, hogy annak méretét, a következő cikket hello: [prémium szintű Storage használatával lemezek](../../../storage/common/storage-premium-storage.md).

* **Lemez csíkozást**: további átviteli sebesség eléréséhez, adhat hozzá további adatlemezt és lemez csíkozást használja. adatlemezek toodetermine hello száma kell tooanalyze hello száma iops-érték és a sávszélességre van szüksége a naplófájl (oka) t, és az adatok és a TempDB adatbázis (oka) t. Figyelje meg, hogy a különböző Virtuálisgép-méretek különböző korlátokkal rendelkeznek iops-érték és a támogatott sávszélesség hello számára, lásd: hello táblák iops-értéket a [Virtuálisgép-méretet](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). A következő irányelveket hello használata:

  * A Windows 8 és Windows Server 2012 vagy újabb, használjon [tárolóhelyek](https://technet.microsoft.com/library/hh831739.aspx) a hello a következő irányelveket:

      1. Set hello interleave (paritásos méret) too64 KB (65 536 bájt) az OLTP-munkaterhelések és az adatraktározási munkaterhelések tooavoid teljesítményre gyakorolt hatás miatt toopartition hibás illesztés hibákat 256 KB (262 144 bájt). Ez a PowerShell használatával kell beállítani.
      1. Állítsa be az oszlopok száma = fizikai lemezek számát. A PowerShell szolgáltatás több mint 8 lemez (nem a kiszolgáló-kezelő felhasználói felületén) konfigurálásakor. 

    Például a következő PowerShell hello létrehoz egy új hello ellátott tárolókészlet méretét too64 KB és hello száma oszlopok too2 interleave:

    ```powershell
    $PoolCount = Get-PhysicalDisk -CanPool $True
    $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

    New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false 
    ```

  * Windows 2008 R2 vagy korábbi használhatja a dinamikus lemezek (az operációs rendszer csíkozott kötetek) és hello paritásos mérete mindig 64 KB. Vegye figyelembe, hogy ez a beállítás Windows 8 és Windows Server 2012-től elavult. Információ: hello támogatási nyilatkozattal: [virtuális lemezszolgáltatás tooWindows tárolókezelési API tér át](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

  * Ha a számítási feladatok nem intenzív naplót, és nem kell külön IOPs, beállíthatja a csak egy tárolókészletet. Máskülönben hozzon létre két tárolókészletek, egy a hello naplófájlokhoz és egy másik tárolókészlettel hello adatfájllal és a TempDB adatbázisban. Minden egyes betöltés elvárásainak alapuló tárolókészlet hozzárendelt hello számának meghatározása. Ne feledje, hogy más Virtuálisgép-méretek lehetővé teszik a különböző számú adatlemezt csatolni. További információkért lásd: [virtuális gépek méretei](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

  * Ha nem használ (fejlesztési és tesztelési célú forgatókönyvek) prémium szintű Storage, hello ajánlás tooadd hello maximális száma érték az adatlemezek támogatja a [Virtuálisgép-méretet](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és lemez csíkozást használja.

* **Gyorsítótárazási házirend**: adatlemezek a prémium szintű Storage, engedélyezze az adatfájlok és a TempDB csak üzemeltető hello adatlemezek olvasási gyorsítótárazás. Használatakor nem prémium szintű Storage, nem engedélyezi a gyorsítótárazást az adatok lemezzel. A lemez gyorsítótárazás, konfigurálásával kapcsolatban lásd: a következő témakörök hello: [Set-AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) és [Set-AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx).

  > [!WARNING]
  > Hello SQL Server szolgáltatás leállítása, Azure virtuális lemezek tooavoid hello lehetőségét, amely bármilyen adatbázis-sérülés hello gyorsítótár-beállításainak módosításakor.

* **NTFS foglalásiegység-méret**: hello adatok lemez formázása esetén ajánlott használni, amikor egy 64 KB-os foglalásiegység-méret adatok és a naplófájlok, valamint a TempDB adatbázisban.

* **Gyakorlati tanácsok a jelszókezeléshez lemez**: Ha adatlemezt vagy nem módosíthatja a gyorsítótár írja, hello SQL Server szolgáltatás leállítása hello módosítása során. Amikor hello gyorsítótárazási beállítások vannak hello OS lemezen Azure hello virtuális gép leáll, hello gyorsítótártípust változik, és hello virtuális gép újraindul. Adatlemez hello gyorsítótár beállításainak megváltozásakor hello virtuális gép nem állt le, de hello adatlemez leválasztása a virtuális gép során hello módosítása, és majd objektumkörnyezetben hello.

  > [!WARNING]
  > Hiba toostop hello ezek a műveletek során az SQL Server szolgáltatás sérülésével járhat.

## <a name="io-guidance"></a>I/o-útmutató

* hello legjobb eredmények prémium szintű Storage érhetők el, ha az alkalmazás és a kérelmek parallelize. Prémium szintű Storage a forgatókönyvekben, ahol hello IO várólistamélység nagyobb, mint 1, így látni fogja a soros kérelmek egyszálas kevéssé vagy egyáltalán ne teljesítménynövekedéshez (még akkor is, ha azok a tárolási intenzív) szolgál. Ide tartozhat például teljesítmény elemzésére szolgáló eszközöket, például SQLIO hello egyszálas vizsgálat eredményeit.

* Érdemes lehet [adatbázis-oldal tömörítési](https://msdn.microsoft.com/library/cc280449.aspx) , akkor javíthatja intenzív i/o-munkaterhelések teljesítményét. Azonban a hello adattömörítés növelheti hello CPU-felhasználás hello adatbázis-kiszolgálón.

* Érdemes megfontolni a azonnali fájl inicializálási tooreduce hello szükséges idő kezdeti fájl kiosztható. tootake előny az azonnali fájlinicializálása, hello SQL Server (MSSQLSERVER) szolgáltatás fiók a SE_MANAGE_VOLUME_NAME adja meg, és adja hozzá toohello **kötet karbantartási feladatok elvégzése** biztonsági házirend. Ha egy SQL Server platformlemezképet használ az Azure-ba, hello alapértelmezett szolgáltatásfiók (NT Service\MSSQLSERVER) nem adódik toohello **kötet karbantartási feladatok elvégzése** biztonsági házirend. Ez azt jelenti azonnali fájlinicializálása nincs engedélyezve az SQL Server Azure platform lemezképek. A felvett hello SQL Server szolgáltatás fiók toohello **kötet karbantartási feladatok elvégzése** biztonsági házirend, indítsa újra a hello SQL Server szolgáltatást. Ez a szolgáltatás használatára vonatkozó biztonsági szempontokat is lehet. További információkért lásd: [adatbázis Fájlinicializálása](https://msdn.microsoft.com/library/ms175935.aspx).

* **automatikus növekedésre** tekinthető toobe csupán a váratlan növekedésre eseményre. Az automatikus növekedésre folyamatosan a adatainak és naplókönyvtárainak növekedési nem kezeli. Automatikus növekedésre használatos, ha előre hello mérete kapcsoló használatával hello fájl megcélzott.

* Győződjön meg arról, hogy **autoshrink** van letiltva tooavoid szükségtelen terhet negatívan befolyásolhatja a teljesítményt.

* Helyezze át az összes adatbázisok toodata lemez, beleértve a rendszer-adatbázisokat. További információkért lásd: [rendszer adatbázisok áthelyezése](https://msdn.microsoft.com/library/ms345408.aspx).

* SQL Server hiba naplózásához és követéséhez fájl könyvtárak toodata lemezek át. Ezt megteheti az SQL Server Configuration Manager kattintson a jobb gombbal az SQL Server-példányt, majd válassza a tulajdonságok. hello hiba naplózásához és követéséhez beállításainak módosítására a hello **indítási paraméterek** memóriakép könyvtára megadott hello külön-külön hello **speciális** a következő képernyőkép külön-külön hello azt a toolook hello hiba napló indítási paraméter.

    ![Az SQL hibanaplóban képernyőképe](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

* A telepítő biztonsági másolat és az adatbázis alapértelmezett tárolási helyeit. Hello javaslatokat használja ebben a témakörben, és hello módosításokat hello kiszolgáló tulajdonságai ablakban. Útmutatásért lásd: [megtekintése és módosítása hello alapértelmezett helyek az adatok és naplófájlok (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). hello alábbi képernyőfelvételen azt mutatja be, ha toomake ezeket a módosításokat.

    ![SQL Data napló-és biztonsági mentése](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)
* Engedélyezése zárolt lapok tooreduce IO és lapozás tevékenységeket. További információkért lásd: [hello zárolási lapok memória beállítás (Windows) engedélyezése](https://msdn.microsoft.com/library/ms190730.aspx).

* Ha SQL Server 2012 rendszer használata esetén telepítse a Service Pack 1 összegző frissítés 10. A frissítés tartalmazza a hello javítás i/o gyenge teljesítményt, válassza ki azokat az SQL Server 2012 ideiglenes table utasítás végrehajtása esetén. További információ: Ez [Tudásbázis](http://support.microsoft.com/kb/2958012).

* Vegye figyelembe az adatfájlokról tömörítés vagy kikapcsolása átviteléhez az Azure.

## <a name="feature-specific-guidance"></a>A szolgáltatás konkrét útmutatást

Egyes központi telepítések további teljesítménybeli előnyökben speciális konfigurációs technikák segítségével elérése érdekében. hello alábbi kiemeli néhány SQL Server-szolgáltatásokat, melyek segíthetnek tooachieve jobb teljesítményt:

* **Biztonsági mentés tooAzure tárolási**: az Azure virtuális gépeken futó SQL Server biztonsági mentés végrehajtásához, használhatja [SQL Server biztonsági másolat tooURL](https://msdn.microsoft.com/library/dn435916.aspx). Ez a szolgáltatás elérhető SQL Server 2012 SP1 CU2 kezdve és ajánlott biztonsági mentéséhez kapcsolódó toohello adatlemezek. Ha Ön biztonsági mentés/visszaállítás onnan az Azure storage kövesse hello megadott [SQL Server biztonsági másolat tooURL ajánlott eljárások és hibaelhárítás és az Azure Storage-ban tárolt biztonsági másolatok visszaállítása](https://msdn.microsoft.com/library/jj919149.aspx). Ezek a biztonsági mentéseket is automatizálhatja [automatikus biztonsági mentés az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-automated-backup.md).

    Előzetes tooSQL Server 2012, használhat [SQL Server biztonsági másolat tooAzure eszköz](https://www.microsoft.com/download/details.aspx?id=40740). Ez az eszköz segítségével tooincrease biztonsági mentési átviteli több biztonsági mentési paritásos cél használatával.

* **SQL Server-adatfájlok az Azure-ban**: ezen új szolgáltatás [SQL Server-adatfájlok az Azure-ban](https://msdn.microsoft.com/library/dn385720.aspx), az SQL Server 2014 kezdve érhető el. Hasonló teljesítményt nyújt, mint az Azure data lemezek használata az Azure-adatfájlok SQL Servert futtató mutatja be.

## <a name="next-steps"></a>Következő lépések

Ajánlott biztonsági eljárások, lásd: [biztonsági szempontok az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-security.md).

Tekintse át a többi SQL Server virtuális gép témakörei [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).
