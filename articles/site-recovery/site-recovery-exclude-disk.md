---
title: "használja az Azure Site Recovery általi védelem aaaExclude lemezei |} Microsoft Docs"
description: "Ismerteti, miért és hogyan tooexclude VM lemezek VMware tooAzure és a Hyper-V tooAzure forgatókönyvek a replikációból."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2017
ms.author: nisoneji
ms.openlocfilehash: f47146bc57aeab3fce90123d0894fa86dde93417
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exclude-disks-from-replication"></a>Lemezek kizárása a replikációból
Ez a cikk ismerteti, hogyan tooexclude lemezek, a replikációból. Ez a kizárás felhasznált hello replikáció sávszélesség-optimalizálását, vagy ilyen lemezeket használó hello céloldali erőforrások optimalizálására. hello szolgáltatás támogatott VMware tooAzure és a Hyper-V tooAzure forgatókönyveit tartalmazza.

## <a name="prerequisites"></a>Előfeltételek

Alapértelmezés szerint a rendszer a gép minden lemezét replikálja. egy lemezt a replikációból tooexclude, manuálisan kell telepítenie hello mobilitási szolgáltatás hello gépen előtt a replikáció engedélyezése, ha a VMware tooAzure replikál.


## <a name="why-exclude-disks-from-replication"></a>Miért érdemes lemezeket kizárni a replikációból?
A lemezek kizárása a replikációból az alábbiak miatt lehet szükséges:

- a kizárt hello lemezen van churned hello adatok nem fontos, vagy nem szükséges a replikált toobe.

- Nem replikálja a forgalom kívánt toosave tárolási és hálózati erőforrásokat.

## <a name="what-are-hello-typical-scenarios"></a>Mik azok a jellemző forgatókönyvek hello?
Azonosíthatók az adatváltozások egyes típusai, amelyek a leginkább alkalmasak a kizárásra. Például előfordulhat, hogy beírja tooa lapozófájl (pagefile.sys), illetve adatokat ír ugyanebbe toohello tempdb fájl a Microsoft SQL Server. Attól függően, hogy hello munkaterhelés és a hello tárolóalrendszer hello lapozófájl jelentős mennyiségű forgalom tud regisztrálni. Azonban ezek az adatok replikálásához hello elsődleges hely tooAzure lenne erőforrás-igényesek. Így a következő lépéseket toooptimize replikációs egyetlen virtuális lemez, amelyen hello operációs rendszer és a hello lapozófájlt a virtuális gép hello is használhatja:

1. Egy virtuális lemezt hello felosztása két virtuális lemezt. Egy olyan virtuális lemezt hello operációs rendszerrel rendelkezik, és más hello van hello lapozófájl.
2. Hello lemezen a lapozófájl kizárása a replikációból.

Hasonlóképpen használja a következő lépéseket toooptimize egy lemezt, amelynek mindkét hello Microsoft SQL Server tempdb ező hello, és a rendszer adatbázisfájl hello:

1. Két különböző lemezek hello rendszeradatbázis és a tempdb tartani.
2. Hello tempdb lemez kizárása a replikációból.

## <a name="how-tooexclude-disks-from-replication"></a>Hogyan tooexclude a replikációból lemezek?

### <a name="vmware-tooazure"></a>VMware tooAzure
Hajtsa végre a hello [engedélyezze a replikálást](site-recovery-vmware-to-azure.md) munkafolyamat tooprotect egy virtuális gép hello Azure Site Recovery portálról. A hello negyedik lépése annak hello munkafolyamat, használja a hello **lemez tooREPLICATE** oszlop tooexclude lemezeket a replikációból. Alapértelmezés szerint az összes lemez ki van jelölve replikációra. Törölje a lemezeket, hogy szeretné-e a replikáció, és majd a teljes hello lépéseket tooenable replikációs tooexclude hello négyzetének jelölését.

![Lemezek kizárása a replikációból és VMware tooAzure feladat-visszavételi replikációs engedélyezése](./media/site-recovery-exclude-disk/v2a-enable-replication-exclude-disk1.png)


>[!NOTE]
>
> * Kizárhat, hogy csak azok a lemezek, amelyeket már hello mobilitási szolgáltatás telepítve van. Hello mobilitásiszolgáltatás csak telepítve van a replikáció engedélyezése után hello leküldéses mechanizmus használatával, ezért szüksége toomanually telepítés hello mobilitási szolgáltatás.
> * Csak az alaplemezek zárhatók ki a replikációból. Nem zárhatja ki az operációsrendszer-lemezeket és a dinamikus lemezeket.
> * A replikáció engedélyezése után már nem lehet ahhoz lemezeket hozzáadni, vagy lemezeket eltávolítani belőle. Ha szeretné, hogy tooadd, vagy zárja ki egy lemezt, akkor toodisable védelmi hello számítógéphez szükségesek, és majd engedélyezze újból.
> * Ha kizárja a olyan lemezzel, amelyet egy alkalmazás toooperate a feladatátvételi tooAzure után szükség van szüksége lesz toocreate hello lemez manuálisan az Azure-ban, hogy a replikált hello alkalmazás futtatható. Alternatív megoldásként integrálhatja az Azure automation helyreállítási terv toocreate hello lemezre hello gép feladatátvétele során.
> * Windows rendszerű virtuális gépek: Az Azure-ban manuálisan létrehozott lemezek nem vesznek részt a feladatátvételben. Ha például végrehajtja három lemez feladatátvételét, kettőt pedig közvetlenül az Azure Virtual Machinesben hoz létre, csak a feladatátvételben részt vevő három lemezen lesz végrehajtva a feladat-visszavétel. Manuálisan létrehozott feladat-visszavétel vagy a védelem-újrabeállítási helyszíni tooAzure a lemezek nem adhat meg.
> * Linux rendszerű virtuális gépek: Az Azure-ban manuálisan létrehozott lemezek részt vesznek a feladatátvételben. Ha például végrehajtja három lemez feladatátvételét, kettőt pedig közvetlenül az Azure Virtual Machinesben hoz létre, mind az öt lemezen végre lesz hajtva a feladat-visszavétel. A manuálisan létrehozott lemezek nem zárhatók ki a feladat-visszavételből.
>

### <a name="hyper-v-tooazure"></a>Hyper-V tooAzure
Hajtsa végre a hello [engedélyezze a replikálást](site-recovery-hyper-v-site-to-azure.md) munkafolyamat tooprotect egy virtuális gép hello Azure Site Recovery portálról. A hello negyedik lépése annak hello munkafolyamat, használja a hello **lemez tooREPLICATE** oszlop tooexclude lemezeket a replikációból. Alapértelmezés szerint az összes lemez ki van jelölve replikációra. Törölje a lemezeket, hogy szeretné-e a replikáció, és majd a teljes hello lépéseket tooenable replikációs tooexclude hello négyzetének jelölését.

![Lemezek kizárása a replikációból, és engedélyezze a replikálást a Hyper-V tooAzure feladat-visszavételre](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)

>[!NOTE]
>
> * Csak az alaplemezek zárhatók ki a replikációból. Nem zárhatja ki az operációsrendszer-lemezeket. Azt javasoljuk, hogy ne zárja ki a dinamikus lemezeket. Az Azure Site Recovery nem azonosítható melyik virtuális merevlemez (VHD) alapszintű vagy dinamikus hello Vendég virtuális gépen.  Ha az összes függő dinamikus kötet lemez nem kizárt, hello védett dinamikus lemez egy meghibásodott lemez a virtuális gépen a feladatátvevő válik, és hello a lemezen levő adatok nem érhető el.
> * A replikáció engedélyezése után már nem lehet ahhoz lemezeket hozzáadni, vagy lemezeket eltávolítani belőle. Ha szeretné, hogy tooadd, vagy zárja ki egy lemezt, akkor kell toodisable védelmi hello virtuális gép, és majd engedélyezze újból.
> * Ha kizárja a olyan lemezzel, amelyet egy alkalmazás toooperate van szükség, feladatátvevő tooAzure után lesz szüksége toocreate hello lemez manuálisan az Azure-ban, hogy a replikált hello alkalmazás futtatható. Alternatív megoldásként integrálhatja az Azure automation helyreállítási terv toocreate hello lemezre hello gép feladatátvétele során.
> * Az Azure-ban manuálisan létrehozott lemezek nem vesznek részt a feladatátvételben. Például ha sikertelen a több mint három lemezt, és két közvetlenül az Azure virtuális gépek létrehozásához, csak három lemezt, amely a feladatátvételt volt sikertelen lesz újból az Azure tooHyper-V. Manuálisan létrehozott feladat-visszavétel vagy a visszirányú replikálás a Hyper-V tooAzure lemezek nem adhat meg.



## <a name="end-to-end-scenarios-of-exclude-disks"></a>Lemezek kizárásának teljes körű forgatókönyvei
Mérlegeljük, két forgatókönyvek toounderstand hello kizárási lemez funkció:

- SQL Server tempdb lemeze
- Lapozófájl (pagefile.sys) lemeze

### <a name="exclude-hello-sql-server-tempdb-disk"></a>Hello SQL Server tempdb lemez kizárása
Vegyünk egy SQL Server virtuális gépet, amely tempdb-adatbázisa kizárható.

hello virtuális lemez neve hello SalesDB.

Hello forrás virtuális gép lemezeinek a következők:


**Lemez neve** | **Vendég operációsrendszer-lemez száma** | **Meghajtó betűjele** | **Adattípus hello lemezen**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Operációsrendszer-lemez
DB-Disk1| Disk1 | D:\ | SQL-rendszeradatbázis és 1. felhasználói adatbázis
Adatbázis-2. (a védelemből kizárt hello lemez) | Disk2 | E:\ | Ideiglenes fájlok
Adatbázis-Disk3 (a védelemből kizárt hello lemez) | Disk3 | F:\ | A tempdb adatbázis SQL (mappa elérési útja (F:\MSSQL\Data\) < /br/>< /br/> írja le a feladatátvétel előtt hello mappa elérési útját.
DB-Disk4 | Disk4 |G:\ |2. felhasználói adatbázis

Mivel a két lemez hello virtuális gép adatforgalommal ideiglenes, hello SalesDB virtuális gép védelme érdekében, nem 2. és Disk3 a replikációból. Az Azure Site Recovery nem replikálja ezeket a lemezeket. Feladatátvétel esetén lemezek nem kerülnek a hello feladatátvételi virtuális gépet az Azure.

Lemezek hello Azure virtuális gép a feladatátvételt követően a következők:

**Vendég operációsrendszer-lemez száma** | **Meghajtó betűjele** | **Adattípus hello lemezen**
--- | --- | ---
DISK0 | C:\ | Operációsrendszer-lemez
Disk1 | E:\ | Ideiglenes tárolási < /br / >< /br / > Azure ad hozzá a lemezt, és hozzárendeli a hello első elérhető meghajtóbetűjelet.
Disk2 | D:\ | SQL-rendszeradatbázis és 1. felhasználói adatbázis
Disk3 | G:\ | 2. felhasználói adatbázis

2. és Disk3 ki lettek zárva hello SalesDB virtuális gépről, mert a E: hello első meghajtóbetűjelet hello rendelkezésre listájában. Azure hozzárendel E: toohello ideiglenes tárolóköteten. Replikált hello lemezein levelek megmaradnak hello meghajtó hello azonos.

Disk3, amely hello SQL tempdb lemez volt (a tempdb mappa elérési útja F:\MSSQL\Data\), ki lett zárva a replikációból. hello lemez hello feladatátvételt a virtuális gép nem érhető el. Ennek eredményeképpen hello SQL szolgáltatás leállított állapotban van, ezért az hello F:\MSSQL\Data elérési útja.

Nincsenek két módon toocreate az elérési út:

- Új lemez hozzáadásával és a tempdb mappa elérési útjának hozzárendelésével.
- Meglévő ideiglenes tárolási lemez használata a hello tempdb mappa elérési útját.

#### <a name="add-a-new-disk"></a>Új lemez hozzáadása:

1. Jegyezze fel SQL tempdb.mdf és a feladatátvétel előtt tempdb.ldf hello elérési útját.
2. Hello Azure-portálon hozzá egy új lemezt toohello feladatátvételi virtuális gép hello ugyanaz vagy további mérete legyen, mint hello SQL tempdb forráslemez (Disk3).
3. Bejelentkezés toohello Azure virtuális géphez. A hello Lemezkezelés (diskmgmt.msc) konzolon inicializálása és formázása hello újonnan hozzáadott lemez.
4. Hozzárendelése hello hello SQL tempdb lemez (F:) által használt azonos meghajtóbetűjele.
5. Hozzon létre egy tempdb mappát hello F: kötet (F:\MSSQL\Data).
6. Hello SQL szolgáltatás hello szolgáltatás konzoljáról indítható el.

#### <a name="use-an-existing-temporary-storage-disk-for-hello-sql-tempdb-folder-path"></a>Meglévő ideiglenes tárolási lemez használata a hello SQL tempdb mappa elérési útja:

1. Nyisson meg egy parancssort.
2. SQL Server helyreállítási módban a hello parancssorból futtassa.

        Net start MSSQLSERVER /f / T3608

3. Futtassa a következő sqlcmd toochange hello tempdb toohello új útvonal hello.

        sqlcmd -A -S SalesDB        **Use your SQL DBname**
        USE master;     
        GO      
        ALTER DATABASE tempdb       
        MODIFY FILE (NAME = tempdev, FILENAME = 'E:\MSSQL\tempdata\tempdb.mdf');
        GO      
        ALTER DATABASE tempdb       
        MODIFY FILE (NAME = templog, FILENAME = 'E:\MSSQL\tempdata\templog.ldf');       
        GO


4. Hello Microsoft SQL Server szolgáltatás leállítása.

        Net stop MSSQLSERVER
5. Indítsa el a hello Microsoft SQL Server szolgáltatást.

        Net start MSSQLSERVER

Tekintse meg a következő ideiglenes tároló lemez Azure útmutatás toohello:

* [SSD-k használata az Azure virtuális gépek toostore SQL Server TempDB és a puffer készlet bővítmények](https://blogs.technet.microsoft.com/dataplatforminsider/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/)
* [Ajánlott eljárások az SQL Server teljesítményének Azure Virtual Machines szolgáltatásbeli növeléséhez](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance)

### <a name="failback-from-azure-tooan-on-premises-host"></a>Feladat-visszavétel (az Azure tooan a helyi gazdagép)
Most tegyük megértéséhez hello lemezek replikált történő feladatátadást követően Azure tooyour helyszíni VMware- vagy Hyper-V gazdagépen. Az Azure szolgáltatásban manuálisan létrehozott lemezek nem lesznek replikálva. Ha például végrehajtja három lemez feladatátvételét, kettőt pedig közvetlenül az Azure virtuális gépeken hoz létre, csak a feladatátvételben részt vevő három lemezen lesz végrehajtva a feladat-visszavétel. Manuálisan létrehozott feladat-visszavétel vagy a védelem-újrabeállítási helyszíni tooAzure a lemezek nem adhat meg. Azt is replikálódnak hello ideiglenes tárolási lemez tooon helyszíni gazdagépek.

#### <a name="failback-toooriginal-location-recovery"></a>Feladat-visszavétel toooriginal helyre történő helyreállítást

Hello előző példában hello Azure virtuális gép lemez konfigurációját a következőképpen történik:

**Vendég operációsrendszer-lemez száma** | **Meghajtó betűjele** | **Adattípus hello lemezen**
--- | --- | ---
DISK0 | C:\ | Operációsrendszer-lemez
Disk1 | E:\ | Ideiglenes tárolási < /br / >< /br / > Azure ad hozzá a lemezt, és hozzárendeli a hello első elérhető meghajtóbetűjelet.
Disk2 | D:\ | SQL-rendszeradatbázis és 1. felhasználói adatbázis
Disk3 | G:\ | 2. felhasználói adatbázis


#### <a name="vmware-tooazure"></a>VMware tooAzure
Feladat-visszavétel toohello eredeti helyre történik, amikor hello feladat-visszavételt a virtuális gép lemezkonfiguráció nincs kizárt lemezek. Lemezek, amelyek ki lettek zárva az VMware tooAzure nem lesz elérhető hello feladat-visszavételt a virtuális gépen.

Tervezett feladatátvétel az Azure tooon helyszíni VMware után lemezek hello VMWare virtuális gép (az eredeti helyre) a következők:

**Vendég operációsrendszer-lemez száma** | **Meghajtó betűjele** | **Adattípus hello lemezen**
--- | --- | ---
DISK0 | C:\ | Operációsrendszer-lemez
Disk1 | D:\ | SQL-rendszeradatbázis és 1. felhasználói adatbázis
Disk2 | G:\ | 2. felhasználói adatbázis

#### <a name="hyper-v-tooazure"></a>Hyper-V tooAzure
Ha feladat-visszavétel toohello eredeti helyére, hello feladat-visszavételt a virtuális gép lemez konfigurációs marad hello ugyanaz legyen, mint a Hyper-V eredeti virtuális gép lemezkonfigurációja. Lemezek, amelyek ki lettek zárva az Hyper-V hely tooAzure hello feladat-visszavételt a virtuális gép érhetők el.

Az Azure tooon helyszíni Hyper-V tervezett feladatátvétel után lemezek hello Hyper-V virtuális gépen (eredeti helyre) a következők:

**Lemez neve** | **Vendég operációsrendszer-lemez száma** | **Meghajtó betűjele** | **Adattípus hello lemezen**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 |   C:\ | Operációsrendszer-lemez
DB-Disk1 | Disk1 | D:\ | SQL-rendszeradatbázis és 1. felhasználói adatbázis
DB-Disk2 (kizárt lemez) | Disk2 | E:\ | Ideiglenes fájlok
DB-Disk3 (kizárt lemez) | Disk3 | F:\ | SQL tempdb-adatbázis (mappa elérési útja (F:\MSSQL\Data\)
DB-Disk4 | Disk4 | G:\ | 2. felhasználói adatbázis


#### <a name="exclude-hello-paging-file-pagefilesys-disk"></a>Hello lapozófájl (pagefile.sys) lemezen kizárása

Vegyünk egy virtuális gépet, amely lapozófájllemeze kizárható.
Két eset létezik.

#### <a name="case-1-hello-paging-file-is-configured-on-hello-d-drive"></a>1. eset: hello lapozófájlt a hello D: meghajtó
Hello lemez konfigurációját a következő:


**Lemez neve** | **Vendég operációsrendszer-lemez száma** | **Meghajtó betűjele** | **Adattípus hello lemezen**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Operációsrendszer-lemez
Adatbázis-1. lemez (hello védelemből kizárt hello lemez) | Disk1 | D:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Felhasználói adatok, 1
DB-Disk3 | Disk3 | F:\ | Felhasználói adatok, 2

Az alábbiakban hello lapozófájl-beállításokhoz hello forrás virtuális gépen:

![Lapozófájl-beállítások a forrás virtuális gépen](./media/site-recovery-exclude-disk/pagefile-on-d-drive-sourceVM.png)


Hello virtuális gép VMware tooAzure vagy a Hyper-V tooAzure feladatátvétel után hello Azure virtuális gép lemezeinek a következők:

**Lemez neve** | **Vendég operációsrendszer-lemez száma** | **Meghajtó betűjele** | **Adattípus hello lemezen**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Operációsrendszer-lemez
DB-Disk1 | Disk1 | D:\ | Ideiglenes tároló</br /> </br />pagefile.sys
DB-Disk2 | Disk2 | E:\ | Felhasználói adatok, 1
DB-Disk3 | Disk3 | F:\ | Felhasználói adatok, 2

1. lemez (D:) ki lett zárva, mert a D: hello első meghajtóbetűjelet hello rendelkezésre listájában. Azure hozzárendel D: toohello ideiglenes tárolóköteten. Hello Azure virtuális gép D: érhető el, mert a hello lapozási fájl beállítása hello virtuális gép marad hello azonos.

Az alábbiakban hello lapozófájl-beállításokhoz a hello Azure virtuális gépen:

![Lapozófájl-beállítások az Azure virtuális gépen](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover.png)

#### <a name="case-2-hello-paging-file-is-configured-on-another-drive-other-than-d-drive"></a>2. eset: hello lapozófájlt másik meghajtón (eltérő D:)

Hello forrás virtuális gép lemez konfigurációját a következő:

**Lemez neve** | **Vendég operációsrendszer-lemez száma** | **Meghajtó betűjele** | **Adattípus hello lemezen**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Operációsrendszer-lemez
Adatbázis-1. lemez (a védelemből kizárt hello lemez) | Disk1 | G:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Felhasználói adatok, 1
DB-Disk3 | Disk3 | F:\ | Felhasználói adatok, 2

Az alábbiakban hello lapozófájl-beállításokhoz hello a helyszíni virtuális gépen:

![Lapozás hello a helyszíni virtuális gép beállításai](./media/site-recovery-exclude-disk/pagefile-on-g-drive-sourceVM.png)

Hello virtuális gép VMware-vagy Hyper-V tooAzure a feladatátvétel után hello Azure virtuális gép lemezeinek a következők:

**Lemez neve**| **Vendég operációsrendszer-lemez száma**| **Meghajtó betűjele** | **Adattípus hello lemezen**
--- | --- | --- | ---
DB-Disk0-OS | DISK0  |C:\ |Operációsrendszer-lemez
DB-Disk1 | Disk1 | D:\ | Ideiglenes tároló</br /> </br />pagefile.sys
DB-Disk2 | Disk2 | E:\ | Felhasználói adatok, 1
DB-Disk3 | Disk3 | F:\ | Felhasználói adatok, 2

Mivel D: hello elérhető hello listából első meghajtóbetűjelet, Azure rendeli D: toohello ideiglenes tárolóköteten. Replikált hello lemezein hello meghajtó betűjele továbbra is hello azonos. Hello G: lemez nem érhető el, mert hello rendszer lapozófájl hello hello C: meghajtó fogja használni.

Az alábbiakban hello lapozófájl-beállításokhoz a hello Azure virtuális gépen:

![Lapozófájl-beállítások az Azure virtuális gépen](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover-2.png)

## <a name="next-steps"></a>Következő lépések
Ha sikerült beállítania és elindítani az üzemelő példányt, [ismerkedjen meg részletesebben](site-recovery-failover.md) a feladatátvételi különféle típusaival.
