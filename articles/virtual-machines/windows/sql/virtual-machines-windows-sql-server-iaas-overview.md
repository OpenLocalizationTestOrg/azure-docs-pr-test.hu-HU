---
title: "Az SQL Server használatának áttekintése az Azure Virtual Machines szolgáltatásban (Windows rendszeren) | Microsoft Docs"
description: "Ismerje meg, hogyan futtathat teljes SQL Server-kiadásokat az Azure Virtual Machines szolgáltatásban."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
tags: azure-service-management
ms.assetid: c505089e-6bbf-4d14-af0e-dd39a1872767
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/14/2017
ms.author: jroth
ms.openlocfilehash: 95a18e4b2437ca628e84f6d6f580e12f2f26e111
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/16/2017
---
# <a name="what-is-sql-server-on-azure-virtual-machines-windows"></a>Mi az az Azure Virtual Machines szolgáltatásban futó SQL Server? (Windows)

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-overview.md)
> * [Linux](../../linux/sql/sql-server-linux-virtual-machines-overview.md)

Az [Azure virtuális gépeken futó SQL Serverrel](https://azure.microsoft.com/services/virtual-machines/sql-server/) teljes SQL Server-verziókat használhat a felhőben anélkül, hogy bármilyen helyszíni hardvert kellene fenntartania. Az SQL Servert futtató virtuális gépekkel a használatalapú licencelési költségek is leegyszerűsödnek.

Világszerte számos [földrajzi régióban](https://azure.microsoft.com/regions/) üzemelnek Azure virtuális gépek. Több különböző [gépméret](../sizes.md) is elérhető. A virtuális gépek rendszerkép-katalógusából a megfelelő verziójú, kiadású és operációs rendszerű, SQL Servert futtató virtuális gépet hozhat létre. Ez számos különböző SQL Server számítási feladatra teszi alkalmassá a virtuális gépeket.

## <a name="automated-updates"></a>Automatikus frissítések

Az SQL Server Azure virtuális gépek az [automatikus javítás](virtual-machines-windows-sql-automated-patching.md) segítségével ütemezhetnek egy karbantartási időszakot a fontos Windows- és SQL Server-frissítések automatikus telepítéséhez.

## <a name="automated-backups"></a>Automatikus biztonsági mentések

Az SQL Server Azure virtuális gépek használni képesek az [automatikus biztonsági mentés](virtual-machines-windows-sql-automated-backup-v2.md) előnyeit, amely rendszeresen biztonsági másolatokat készít az adatbázisról a Blob Storage tárhelyen. Ezt a technikát manuálisan is alkalmazhatja. További információ: [Use Azure Storage for SQL Server Backup and Restore](virtual-machines-windows-use-storage-sql-server-backup-restore.md) (Az Azure Storage használata az SQL Server biztonsági mentéséhez és helyreállításához).

## <a name="high-availability"></a>Magas rendelkezésre állás

Ha magas rendelkezésre állásra van szüksége, fontolja meg az SQL Server rendelkezésre állási csoportok konfigurálását. Ehhez több SQL Server Azure virtuális gépre van szükség egy virtuális hálózaton. Konfigurálhatja magas rendelkezésre állású megoldását manuálisan, vagy használhat sablonokat az Azure Portalról az automatikus konfiguráláshoz. A magas rendelkezésre állási lehetőségekről szóló áttekintésért lásd az [Azure Virtual Machines szolgáltatásban futó SQL Server magas rendelkezésre állását és vészhelyreállítását](virtual-machines-windows-sql-high-availability-dr.md) bemutató cikkben.

## <a name="performance"></a>Teljesítmény

Az Azure-beli virtuális gépek a számítási feladatokkal kapcsolatos különböző igényeknek megfelelően eltérő gépméretekkel érhetők el. Az SQL virtuális gépek automatikus, a teljesítménnyel kapcsolatos követelményekhez optimalizált tárolókonfigurálást is nyújtanak. További információ az SQL virtuális gépek konfigurálásáról: [Tárolókonfigurálás SQL Server rendszerű virtuális gépekhez](virtual-machines-windows-sql-server-storage-configuration.md). További információ a teljesítmény finomhangolásáról: [Ajánlott eljárások az SQL Server teljesítményének Azure Virtual Machines szolgáltatásbeli növeléséhez](virtual-machines-windows-sql-performance.md).

## <a name="get-started-with-sql-vms"></a>SQL virtuális gépek – első lépések

Első lépésként válassza ki a megfelelő verziójú, kiadású és operációs rendszerű SQL Server virtuális gép rendszerképét. Az alábbi szakaszokban közvetlen hivatkozások találhatók az Azure Portalra az SQL Server virtuálisgép-katalógus rendszerképeinek letöltéséhez.

> [!TIP]
> Az ezekhez a rendszerképekhez tartozó virtuális gépek és SQL díjszabásának megismeréséhez tekintse meg [az SQL Server Azure virtuális gépek díjszabási útmutatóját](virtual-machines-windows-sql-server-pricing-guidance.md).

> [!TIP]
> Az SQL Server virtuálisgép-katalógusbeli rendszerképeinek frissítési és életciklus-szabályzatárólaz [SQL Server virtuális gépek GYIK](virtual-machines-windows-sql-server-iaas-faq.md#images) dokumentumában olvashat.

### <a id="payasyougo"></a> Használatalapú fizetés
A következő táblázat a használatalapú fizetéssel elérhető SQL Server-rendszerképeket foglalja össze.

| Verzió | Operációs rendszer | Kiadás |
| --- | --- | --- |
| **SQL Server 2017** |Windows Server 2016 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonWindowsServer2016), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonWindowsServer2016), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016) |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1ExpressWindowsServer2016), [Developer](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1DeveloperWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2ExpressWindowsServer2012R2) |
| **SQL Server 2012 SP3** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2) |
| **SQL Server 2008 R2 SP3** |Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2008R2) |

A Linux rendszerhez elérhető SQL Server-virtuálisgépek rendszerképeivel kapcsolatban lásd [az SQL Server használatának áttekintését az Azure Virtual Machines szolgáltatásban (Linux rendszeren)](../../linux/sql/sql-server-linux-virtual-machines-overview.md).

### <a id="BYOL"></a> Saját licenc használata
Saját licencet is használhat (BYOL). Ebben az esetben csak a virtuális gépért kell fizetnie, az SQL Server licencelésével kapcsolatos egyéb költségek nélkül.  Saját licence használatával hosszú távon pénzt takaríthat meg a folyamatos éles számítási feladatok esetében. A lehetőség követelményeivel kapcsolatos információkért tekintse meg [az SQL Server Azure virtuális gépek díjszabási útmutatóját](virtual-machines-windows-sql-server-pricing-guidance.md).

| Verzió | Operációs rendszer | Kiadás |
| --- | --- | --- |
| **SQL Server 2017** |Windows Server 2016 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017EnterpriseWindowsServer2016), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017StandardonWindowsServer2016) |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1EnterpriseWindowsServer2016), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2StandardWindowsServer2012R2) |
| **SQL Server 2012 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2) |

A portálon ezek a rendszerképnevek **{BYOL}** előtagot kapnak.

### <a name="connect-to-the-vm"></a>Kapcsolódás a virtuális géphez
SQL Server virtuális gépe létrehozását követően csatlakozhat hozzá olyan eszközök és alkalmazások segítségével, mint az SQL Server Management Studio (SSMS). Részletes információkért lásd: [Csatlakozás SQL Servert futtató virtuális gépekhez az Azure-ban](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Adatok áttelepítése
Ha van meglévő adatbázisa, érdemes áthelyeznie az újonnan kiépített SQL virtuális gépre. Az áttelepítési lehetőségekért és útmutatásért lásd: [Migrating a Database to SQL Server on an Azure VM](virtual-machines-windows-migrate-sql.md) (Adatbázis áttelepítése egy Azure virtuális gépen SQL Serverre).

## <a name="customer-experience-improvement-program-ceip"></a>Felhasználói élmény fokozása program (CEIP)
A Felhasználói élmény fokozása program (CEIP) alapértelmezés szerint engedélyezve van. Ez a program rendszeres időközönként jelentéseket küld a Microsoftnak az SQL Server fejlesztése érdekében. A CEIP-pel nem kell felügyeleti feladatokat végezni, hacsak nem kívánja letiltani az üzembe helyezés után. A CEIP testreszabásához vagy letiltásához csatlakozzon a virtuális géphez a távoli asztalról. Ezután futtassa az **SQL Server hiba- és használatai jelentések** segédprogramot. A jelentések letiltásához kövesse az utasításokat. Az adatgyűjtésről további információért tekintse meg az [SQL Server adatvédelmi nyilatkozatát](https://www.microsoft.com/EN-US/privacystatement/SQLServer/Default.aspx).

## <a name="related-products-and-services"></a>Kapcsolódó termékek és szolgáltatások
### <a name="windows-virtual-machines"></a>Windows rendszerű virtuális gépek
* [Virtual Machines – áttekintés](../overview.md)

### <a name="storage"></a>Storage
* [A Microsoft Azure Storage bemutatása](../../../storage/common/storage-introduction.md)

### <a name="networking"></a>Hálózat
* [A Virtual Network áttekintése](../../../virtual-network/virtual-networks-overview.md)
* [IP-címek az Azure-ban](../../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [Teljes tartománynév létrehozása az Azure Portalon](../portal-create-fqdn.md)

### <a name="sql"></a>SQL
* [Az SQL Server dokumentációja](https://docs.microsoft.com/sql/index)
* [Az Azure SQL Database összehasonlítása](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md)

## <a name="next-steps"></a>Következő lépések

Az SQL Server használatának első lépései az Azure virtuális gépeken:

* [SQL Servert futtató virtuális gép létrehozása az Azure Portalon](quickstart-sql-vm-create-portal.md)

Választ találhat az SQL virtuális gépek használatával kapcsolatos gyakori kérdésekre:

* [Az SQL Server használata az Azure Virtual Machinesben – gyakori kérdések](virtual-machines-windows-sql-server-iaas-faq.md)
