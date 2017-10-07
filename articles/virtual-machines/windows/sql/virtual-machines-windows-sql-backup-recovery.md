---
title: "aaaBackup és az SQL Server helyreállítása |} Microsoft Docs"
description: "Az Azure virtuális gépeken futó SQL Server-adatbázisok biztonsági mentése és visszaállítása szempontokat ismerteti."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-resource-management
ms.assetid: 95a89072-0edf-49b5-88ed-584891c0e066
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/15/2016
ms.author: mikeray
ms.openlocfilehash: f85248fecdd5867d91d09650a1a34ad7c7caa920
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Biztonsági mentés és visszaállítás Azure-beli SQL Server-alapú virtuális gépeken
## <a name="overview"></a>Áttekintés
Az Azure Storage minden Azure VM-lemez tooguarantee adatvesztés vagy fizikai adatsérülés elleni védelem 3 másolatot tart fenn. Ebből kifolyólag eltérően a helyszínen, nincs szükség a ezekről tooworry. Azonban készítsen továbbra is biztonsági másolatot az SQL Server adatbázisok tooprotect elleni alkalmazás vagy felhasználó hibákat (pl. helytelen adatokat beszúrni, vagy törölje a táblázat), és képes toorestore alatt tooa időpontra történő.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Az Azure virtuális gépeken futó SQL Server, a natív biztonsági mentéssel, és technikák használatával csatlakoztatott lemezek hello cél hello biztonsági mentési fájlok visszaállítása. Van azonban tooan Azure virtuális gép alapján hello csatolható lemezek számának korlátozása toohello [hello virtuális gép mérete](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Lemez felügyeleti tooconsider hello terhek is van.

Az SQL Server 2014-kezdve biztonsági mentése és visszaállítása a tooMicrosoft Azure Blob Storage tárolóban. SQL Server 2016-fejlesztéseket jeleníti meg ezt a lehetőséget is biztosít. Az adatbázis a Microsoft Azure Blob storage-ban tárolt fájlok, SQL Server 2016 emellett beállítást szinte azonnali biztonsági mentések és a gyors visszaállítások Azure pillanatképek. Ez a cikk áttekintést ezeket a beállításokat, és további információk találhatók [SQL Server biztonsági másolat és helyreállítás a Microsoft Azure Blob Storage szolgáltatás](https://msdn.microsoft.com/library/jj919148.aspx).

> [!NOTE]
> A nagyon nagy adatbázisok biztonsági mentésével hello lehetőségek tárgyalását lásd: [több terabájtos SQL Server adatbázis biztonsági stratégiák az Azure virtuális gépek](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).
> 
> 

hello lentebbi például információt adott toohello másik SQL Server egy Azure virtuális gépen támogatott.

## <a name="sql-server-virtual-machines"></a>SQL Server virtuális gépek
Ha az SQL Server-példány egy Azure virtuális gép fut, az adatbázisfájlok már található adatlemezek az Azure-ban. Ezek a lemezek helyezkednek el az Azure Blob Storage tárolóban. Ezért hello okai az biztonsági másolatot az adatbázis és hello megközelítések kissé a módosítás érvénybe. Vegye figyelembe a következőket hello. 

* Mivel a Microsoft Azure biztosít a védelem hello Microsoft Azure szolgáltatás részeként már nincs szüksége tooperform adatbázis biztonsági mentések tooprovide védelmet biztosít a hardver-vagy adathordozót.
* Szüksége van az tooperform az adatbázis biztonsági mentések tooprovide védelme felhasználói hibák ellen, vagy archiválási célokból, a szabályozási okok vagy a felügyeleti célokra.
* Közvetlenül az Azure-ban tárolhatja hello biztonságimásolat-fájlt. További információkért tekintse meg a következő szakaszokban szereplő útmutatás nyújtása a hello az SQL Server különböző verzióit hello.

## <a name="sql-server-2016"></a>SQL Server 2016
Támogatja a Microsoft SQL Server 2016 [biztonsági mentése és visszaállítása a Azure BLOB](https://msdn.microsoft.com/library/jj919148.aspx) szolgáltatások található az SQL Server 2014. De a következő fejlesztések hello is tartalmaz:

| 2016 továbbfejlesztése | Részletek |
| --- | --- |
| **Csíkozást** |A biztonsági másolatot az Azure blob storage tooMicrosoft, SQL Server 2016 támogatja a biztonsági mentése toomultiple blobok tooenable 12.8 TB tooa legfeljebb fel a nagy adatbázisainak biztonsági mentése. |
| **Pillanatkép biztonsági másolatából** |Hello Azure pillanatképek használata a SQL Server-fájl-pillanatkép biztonsági másolatából szinte azonnali biztonsági mentéseit és az adatbázis-fájlok tárolt hello Azure Blob storage szolgáltatással gyors helyreállítást biztosít. Ez a funkció lehetővé teszi, hogy Ön toosimplify a biztonsági mentési és visszaállítási házirendek. Fájl-pillanatkép biztonsági másolatából a visszaállítás egy korábbi időpontra pont is támogatja. További információkért lásd: [pillanatképes biztonsági adatbázis-fájlok az Azure-ban](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx). |
| **Kezeli a biztonsági mentési ütemezés** |SQL Server való felügyelt biztonsági mentésének tooAzure mostantól támogatja az egyéni ütemezések. További információkért lásd: [Azure SQL Server való felügyelt biztonsági mentésének tooMicrosoft](https://msdn.microsoft.com/library/dn449496.aspx). |

Az SQL Server 2016 Azure Blob storage használata esetén hello képességek oktatóanyagban lásd: [oktatóanyag: hello Microsoft Azure Blob storage szolgáltatást használó adatbázisok SQL Server 2016](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL Server 2014
Az SQL Server 2014 hello a következő fejlesztéseket tartalmazza:

1. **Biztonsági mentési és visszaállítási tooAzure**:
   
   * *SQL Server biztonsági másolat tooURL* most már támogatja az SQL Server Management Studio. biztonsági mentési vagy helyreállítási feladat, vagy a karbantartási terv varázsló használata az SQL Server Management Studio, a hello beállítás tooback tooAzure be most érhető el. További információkért lásd: [SQL Server biztonsági másolat tooURL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
   * *SQL Server felügyelt biztonsági tooAzure* , amely lehetővé teszi az automatikus biztonsági mentés új funkciókkal rendelkezik. Ez különösen fontos a egy Azure gépen futó SQL Server 2014-példányok biztonsági felügyeletének automatizálására. További információkért lásd: [Azure SQL Server való felügyelt biztonsági mentésének tooMicrosoft](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
   * *Automatikus biztonsági mentés* biztosít további automation tooautomatically engedélyezése *SQL Server való felügyelt biztonsági mentésének tooAzure* összes meglévő és új adatbázishoz az SQL Server virtuális gép az Azure-ban. További információk: [Automated Backup for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md) (Az SQL Server automatikus biztonsági mentése Azure virtuális gépeken).
   * A SQL Server 2014 biztonsági másolat tooAzure összes hello lehetőségek áttekintéséért lásd: [SQL Server biztonsági másolat és helyreállítás a Microsoft Azure Blob Storage szolgáltatás](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
2. **Titkosítási**: SQL Server 2014 támogatja a titkosított adatok, a biztonsági másolat létrehozásakor. Számos titkosítási algoritmus támogatja, és hello használata osf tanúsítványhoz vagy aszimmetrikus kulcshoz. További információkért lásd: [biztonsági másolatok titkosításának](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQL Server 2012-ben
Az SQL Server biztonsági mentése és visszaállítása az SQL Server 2012 részletes információkért lásd: [biztonsági mentése és visszaállítása az SQL Server-adatbázisok (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Az SQL Server 2012 SP1 kumulatív frissítés 2-től kezdődően biztonsági másolatot készíthet tooand visszaállítási a hello Azure Blob Storage szolgáltatást. Ez a fejlesztés SQL-kiszolgálón futó Azure virtuális gépeket vagy egy helyszíni példányát az SQL Server adatbázisok használt tooback lehet. További információkért lásd: [SQL Server biztonsági mentése és visszaállítása az Azure Blob Storage Service](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Hello Azure Blob storage szolgáltatással hello előnyei közé hello képességét toobypass hello 16 lemez korlátot csatlakoztatott lemezek, a könnyű kezelhetőséget, hello közvetlen rendelkezésre állási hello biztonságimásolat-fájl tooanother példány egy Azure-on futó SQL Server-példány virtuális gép vagy egy helyszíni példányát az áttelepítési vagy katasztrófa utáni helyreállítás céljából. Előnyöket toousing egy Azure blob storage szolgáltatás az SQL Server biztonsági mentések teljes listáját lásd: hello *előnyöket* szakasz [SQL Server biztonsági mentése és visszaállítása az Azure Blob Storage Service](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Ajánlott eljárás javaslatok és hibaelhárítási információkat lásd: [biztonsági mentés és visszaállítás az ajánlott eljárások (Azure Blob Storage szolgáltatás)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008
SQL Server biztonsági másolat és helyreállítás az SQL Server 2008 R2: [biztonsági mentése és az adatbázisok visszaállítása az SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

SQL Server biztonsági mentése és visszaállítása az SQL Server 2008, lásd: [biztonsági mentése és az adatbázisok visszaállítása az SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Következő lépések
Ha azt tervezi, az adott SQL Server egy Azure virtuális gép, az oktatóanyag következő hello üzembe helyezési útmutató található: [az Azure Resource Manager Azure SQL Server virtuális gépek kiépítése](virtual-machines-windows-portal-sql-server-provision.md).

Bár a biztonsági mentése és visszaállítása az adatok lehet használt toomigrate, nincsenek potenciálisan könnyebb adatok áttelepítési útvonalak tooSQL kiszolgálót egy Azure virtuális gépen. Beállítások és javaslatok ismertetését, lásd: [egy adatbázis tooSQL kiszolgálót egy Azure virtuális gép áttelepítése](virtual-machines-windows-migrate-sql.md).

Tekintse át az egyéb [SQL Server Azure virtuális gépek futtatásához szükséges erőforrások](virtual-machines-windows-sql-server-iaas-overview.md).

