---
title: "egy SQL Server adatbázis tooSQL kiszolgáló a virtuális gép aaaMigrate |} Microsoft Docs"
description: "További információk a hogyan toomigrate egy helyi felhasználói adatbázis-kiszolgáló tooSQL egy Azure virtuális gépen."
services: virtual-machines-windows
documentationcenter: 
author: sabotta
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 00fd08c6-98fa-4d62-a3b8-ca20aa5246b1
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.tgt_pltfrm: vm-windows-sql-server
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: carlasab
ms.openlocfilehash: 9c7aba30304ea40796412d2ddc885f6d4a58be2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-a-sql-server-database-toosql-server-in-an-azure-vm"></a>Egy SQL Server adatbázis tooSQL kiszolgálót egy Azure virtuális gép áttelepítése

Nincsenek módszerek toomigrate számos egy helyi SQL Server felhasználói adatbázis tooSQL kiszolgálót egy Azure virtuális gép. Ez a cikk röviden ismertetik a különböző módszerekről, és hello legjobb módszer különböző forgatókönyvek esetén ajánlott.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="what-are-hello-primary-migration-methods"></a>Mik azok a hello elsődleges áttelepítési módszereinek?
hello elsődleges áttelepítési megoldások a következők:

* A helyi biztonsági mentés tömörítés, és manuálisan másolási hello biztonságimásolat-fájl használatával történő hello Azure virtuális gép
* Hajtsa végre a biztonsági mentési tooURL és hello hello URL-ről az Azure virtuális gép üzembe
* Válassza le és hello adatainak és naplókönyvtárainak fájlok tooAzure blob-tároló másolja, majd csatolja a URL-CÍMRŐL tooSQL Server Azure virtuális gépen
* Alakítsa át a helyszíni fizikai tooHyper-V virtuális gép, feltöltése a Blob-tároló tooAzure, és telepíteni, új virtuális gép használatával feltöltött virtuális merevlemez
* Küldje el a merevlemez-meghajtóról Windows Import/Export szolgáltatás használata
* Ha az AlwaysOn központi telepítése a helyszíni, használja a hello [replika Azure hozzáadása varázsló](../classic/sql-onprem-availability.md) toocreate replika Azure, majd a feladatátvétel után a felhasználók toohello Azure database-példányt mutat
* SQL Server használata [tranzakciós replikáció](https://msdn.microsoft.com/library/ms151176.aspx) tooconfigure hello Azure SQL Server-példány előfizetőként és majd tiltsa le a replikációt, válassza a felhasználók toohello Azure database-példányt

> [!TIP]
> Az azonos technikák toomove adatbázisok SQL Server virtuális gépek Azure-ban is használható. Például nincs nem támogatott módon tooupgrade egy SQL Server galéria-lemezkép virtuális gép egy verziójához/kiadásához tooanother. Ebben az esetben inkább hozzon létre egy új SQL Server virtuális gép hello új verziójához/kiadásához, és majd használatát hello áttelepítési módszerek egyikét a Ez a cikk toomove az adatbázisokat. 

## <a name="choosing-your-migration-method"></a>Az áttelepítési módszer kiválasztása
Az optimális adatátvitel alapteljesítményének át hello adatbázisfájlok hello Azure virtuális gép tömörített biztonságimásolat-fájl használatával.

hello adatbázis áttelepítési folyamat során toominimize állásidő hello AlwaysOn szolgáltatását vagy hello tranzakciós replikáció lehetőséget használhatja.

Ha nem lehetséges toouse hello a fenti módszerek, manuálisan telepítse át az adatbázist. Ezzel a módszerrel Ön általában elindítja a hello adatbázis biztonsági másolatának követi az Azure-adatbázis biztonsági másolatából, és végezze el egy adatbázis visszaállítása. Maguk hello adatbázisfájlokat másolni az Azure, és csatolja őket. Többféleképpen az adatbázis migrálása az Azure virtuális gép be ehhez a manuális eljáráshoz-megvalósításához.

> [!NOTE]
> Az SQL Server régebbi verzióit tooSQL Server 2014 vagy SQL Server 2016 frissít, gondolja át e módosítások szükségesek. Azt javasoljuk, hogy kezelje-e az áttelepítés projekt részeként hello új verziója az SQL Server nem támogatja a szolgáltatások összes függősége. Hello támogatott kiadásainak és forgatókönyvek további információkért lásd: [tooSQL Server frissítési](https://msdn.microsoft.com/library/bb677622.aspx).

hello a következő táblázat felsorolja az egyes hello elsődleges áttelepítési módszereinek, valamint ismerteti a legmegfelelőbb az egyes módszerek hello használata esetén.

| Módszer | Source adatbázis-verzió | Cél adatbázis-verzió | Source adatbázis biztonsági másolatának mérete megkötés | Megjegyzések |
| --- | --- | --- | --- | --- |
| [A helyi biztonsági mentés tömörítés, és manuálisan másolási hello biztonságimásolat-fájl használatával történő hello Azure virtuális gép](#backup-and-restore) |SQL Server 2005 vagy újabb |SQL Server 2005 vagy újabb |[Az Azure virtuális gép tárolási kapacitása](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Ez egy olyan nagyon egyszerű és tesztelt eljárás adatbázisok áthelyezése gépek között. |
| [Hajtsa végre a biztonsági mentési tooURL és hello hello URL-ről az Azure virtuális gép üzembe](#backup-to-url-and-restore) |SQL Server 2012 SP1 CU2 vagy gyorsabb |SQL Server 2012 SP1 CU2 vagy gyorsabb |< SQL Server 2016, egyébként < 1 TB-os 12.8 TB | Ez a módszer egy másik módja toomove hello biztonságimásolat-fájl toohello virtuális gép az Azure storage használatával. |
| [Válassza le és hello adatainak és naplókönyvtárainak fájlok tooAzure blob-tároló másolja, majd csatolja a URL-CÍMRŐL tooSQL Server Azure virtuális gépen](#detach-and-attach-from-url) |SQL Server 2005 vagy újabb |SQL Server 2014 vagy újabb |[Az Azure virtuális gép tárolási kapacitása](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Ezt a módszert használja, ha azt tervezi, túl[tárolja ezeket a fájlokat hello Azure Blob storage szolgáltatással](https://msdn.microsoft.com/library/dn385720.aspx) , és csatolja azokat tooSQL kiszolgáló fut egy Azure virtuális gép, különösen olyan nagyon nagy adatbázisok esetén |
| [A konvertálás helyszíni tooHyper-V virtuális gép feltöltése a Blob-tároló tooAzure és majd a feltöltött virtuális merevlemez használatával új virtuális gép telepítése](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) |SQL Server 2005 vagy újabb |SQL Server 2005 vagy újabb |[Az Azure virtuális gép tárolási kapacitása](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |A következő esetekben használja [állapotba hozása a saját SQL Server licence](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md), az áttelepítés alatt egy adatbázist, amely az SQL Server egy régebbi verzióját futtatja, vagy ha áttelepítése rendszer és a felhasználói adatbázisokat együtt részeként hello függ-e más adatbázis áttelepítése felhasználói adatbázisokat és/vagy rendszer-adatbázisokat. |
| [Küldje el a merevlemez-meghajtóról Windows Import/Export szolgáltatás használata](#ship-hard-drive) |SQL Server 2005 vagy újabb |SQL Server 2005 vagy újabb |[Az Azure virtuális gép tárolási kapacitása](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Használjon hello [Windows Import/Export szolgáltatás](../../../storage/common/storage-import-export-service.md) manuális másolásának módszere túl lassú, például a nagyon nagy adatbázisok esetén |
| [Használjon hello Azure replika hozzáadása varázsló](../classic/sql-onprem-availability.md) |SQL Server 2012-es vagy újabb |SQL Server 2012-es vagy újabb |[Az Azure virtuális gép tárolási kapacitása](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Állásidőt, akkor használja, ha az AlwaysOn helyszíni üzembe helyezéssel rendelkezik |
| [SQL Server tranzakciós replikáció](https://msdn.microsoft.com/library/ms151176.aspx) |SQL Server 2005 vagy újabb |SQL Server 2005 vagy újabb |[Az Azure virtuális gép tárolási kapacitása](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Ha a toominimize állásidő kell, és nem rendelkezik az AlwaysOn a helyszíni központi telepítése |

## <a name="backup-and-restore"></a>Biztonsági mentés és visszaállítás
Adatbázis biztonsági mentése a tömörítést, hello biztonsági mentési toohello virtuális gép másolása, majd majd a hello adatbázis visszaállítása. Ha a biztonságimásolat-fájl mérete nagyobb, mint 1 TB-os, mivel a Virtuálisgép-lemez maximális méretén hello 1 TB-os kell paritásos. A következő általános lépéseket toomigrate egy felhasználói adatbázis, a manuális módszerrel hello használata:

1. Hajtsa végre a teljes adatbázis biztonsági mentési tooan helyszíni helyét.
2. Hozzon létre, vagy töltse fel a virtuális gép hello verziójával, az SQL Server szükséges.
3. A telepítő kapcsolatot a követelmények alapján. Lásd: [csatlakozzon az SQL Server virtuális gépet az Azure (erőforrás-kezelő) tooa](virtual-machines-windows-sql-connect.md).
4. Másolja a biztonságimásolat-készítő (oka) t tooyour virtuális gép használata a távoli asztal, a Windows Explorer vagy a hello Másolás parancsot a parancssorból.

## <a name="backup-toourl-and-restore"></a>Biztonsági mentési tooURL és visszaállítása
Ahelyett, hogy a biztonsági másolatot tooa helyi fájlt, használhatja a hello [biztonsági mentési tooURL](https://msdn.microsoft.com/library/dn435916.aspx) majd állítsa vissza az URL-cím toohello virtuális gép. Az SQL Server 2016 csíkozott biztonságimásolat-készletek támogatottak, javasolt a teljesítmény és tooexceed hello méretkorlátait / blob szükséges. Nagyon nagy adatbázisok esetén hello hello használata [Windows Import/Export szolgáltatás](../../../storage/common/storage-import-export-service.md) ajánlott.

## <a name="detach-and-attach-from-url"></a>Válassza le, és csatlakoztassa az URL-címe
Válassza le az adatbázis és naplófájlok fájlokat, és helyezze túl[Azure Blob Storage tárolóban](https://msdn.microsoft.com/library/dn385720.aspx). Majd csatolja hello adatbázis a hello URL-CÍMRŐL az Azure virtuális gépen. Akkor használja, ha azt szeretné, hogy hello fizikai adatbázis fájlok tooreside a Blob Storage tárolóban. Ez akkor lehet hasznos, ha nagyon nagy méretű adatbázisokhoz. A következő általános lépéseket toomigrate egy felhasználói adatbázis, a manuális módszerrel hello használata:

1. Válassza le a hello adatbázisfájlja hello helyszíni adatbázispéldány.
2. Leválasztott hello adatbázisfájlok másolása az Azure blob storage hello használatának [AZCopy parancssori segédprogram](../../../storage/common/storage-use-azcopy.md).
3. Fájlcsatolás hello adatbázis a hello Azure URL-cím toohello SQL Server-példányok hello Azure virtuális Gépen.

## <a name="convert-toovm-and-upload-toourl-and-deploy-as-new-vm"></a>Alakítsa át a tooVM és tooURL feltöltése, új virtuális gép telepítése
Használja a metódus toomigrate összes rendszer és a felhasználói adatbázis egy helyi SQL Server példány tooAzure virtuális gépen. A következő általános lépéseket toomigrate teljes SQL Server-példány a manuális módszerrel hello használata:

1. A konvertálás fizikai vagy virtuális gépek tooHyper-V virtuális merevlemezek használatával [Microsoft Virtual Machine Converter](https://technet.microsoft.com/library/dn874008(v=ws.11).aspx).
2. Töltse fel a VHD-fájlok tooAzure tárolási hello segítségével [Add-AzureVHD parancsmag](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx).
3. Központi telepítése egy új virtuális gép hello segítségével feltöltött virtuális merevlemez.

> [!NOTE]
> Fontolja meg egy egész alkalmazás toomigrate [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Küldje el a merevlemez-meghajtó
Használjon hello [Windows Import/Export szolgáltatás metódus](../../../storage/common/storage-import-export-service.md) tootransfer nagy mennyiségű fájl adatok tooAzure olyan helyzetekben, ahol hello hálózaton feltöltése elfogadhatatlanul magas vagy nem valósítható meg a Blob-tároló. Ezzel a szolgáltatással egyik küldi el, vagy további merevlemez-meghajtókat tartalmazó adott tooan az Azure data adatközpont, amelyen az adatok lesznek feltöltve tooyour tárfiók.

## <a name="next-steps"></a>Következő lépések
További információ az SQL Servert futtató Azure virtuális gépeken: [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).

Egy Azure SQL Server virtuális gép létrehozása egy rögzített lemezképből, lásd: [tippek & a "klónozást" Azure SQL-virtuális gépek a rögzített lemezképeket trükkök](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) hello CSS SQL Server mérnökök blogjában.

