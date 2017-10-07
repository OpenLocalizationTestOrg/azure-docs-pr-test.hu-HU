---
title: "az SQL Server Azure virtuális gépeken aaaOverview |} Microsoft Docs"
description: "További információk a hogyan toorun teljes SQL Server kiadásai Azure virtuális gépeken. Közvetlen kapcsolatok tooall SQL Server Virtuálisgép-lemezképek és a kapcsolódó tartalom kapják meg."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: c505089e-6bbf-4d14-af0e-dd39a1872767
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.openlocfilehash: 07be567c76f4435961592fc0872fe41cd45bd79d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Az SQL Server használata az Azure Virtual Machines szolgáltatásban – áttekintés
Ez a témakör ismerteti a beállításokat az SQL Servert futtató Azure virtuális gépek (VM), valamint [tooportal képek hivatkozásait](#option-1-create-a-sql-vm-with-per-minute-licensing) és áttekintését [gyakori feladatok](#manage-your-sql-vm).

> [!NOTE]
> Ha már ismeri az SQL Server, és most szeretné, hogy hogyan toodeploy egy, az SQL Server virtuális gép: toosee [egy SQL Server rendszerű virtuális gép az Azure-portálon hello](virtual-machines-windows-portal-sql-server-provision.md).
> 
> 

## <a name="overview"></a>Áttekintés
Ha egy adatbázis-rendszergazda vagy egy fejlesztő, Azure virtuális gépek egy módon toomove forgathatók a helyszíni SQL Server számítási feladatok és alkalmazások toohello felhő adja meg. hello következő videó műszaki áttekintést nyújt az SQL Server Azure virtuális gépeken.

> [!VIDEO https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016/player]
> 
> 

hello videó ismerteti a következő területeken hello:

| Time | Terület |
| --- | --- |
| 00:21 |Mik azok az Azure virtuális gépek? |
| 01:45 |Biztonság |
| 02:50 |Kapcsolatok |
| 03:30 |Tárolómegbízhatóság és -teljesítmény |
| 05:20 |A virtuális gépek mérete |
| 05:54 |Magas rendelkezésre állás és szolgáltatásszint-szerződés |
| 07:30 |Konfigurációs támogatás |
| 08:00 |Figyelés |
| 08:32 |Bemutató: SQL Server 2016 virtuális gép létrehozása |

> [!NOTE]
> hello videó az SQL Server 2016 összpontosít, de Azure Virtuálisgép-rendszerképek biztosít az SQL Server 2012, 2014 és 2016 sok verziója. 
> 
> 

## <a name="scenarios"></a>Forgatókönyvek
Számos oka választása toohost az adatokat az Azure-ban. Ha az alkalmazás mozgatása tooAzure, ez növeli a tooalso áthelyezés hello teljesítményadatait. De a használata további előnyökkel is jár. Automatikusan hozzáférést toomultiple adatközponthoz is a globális jelenlét és katasztrófa-helyreállítás van. hello adata is rendkívül biztonságos és tartós.

Az Azure virtuális gépeken futó SQL Server a relációs adatok Azure-ban való tárolásának egyik módja. Ez számos forgatókönyv esetében jó választás. Például lehetséges tooan a helyszíni SQL Server számítógép hasonlóan érdemes tooconfigure hello Azure virtuális Gépen. Vagy a toorun további alkalmazásokat és szolgáltatásokat a hello ugyanazon adatbázis-kiszolgáló. Két fő erőforrás segítheti abban, hogy még több forgatókönyvet és megfontolást végiggondoljon:

* [Azure virtuális gépeken futó SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/) hello ajánlott forgatókönyvek áttekintést nyújt az Azure virtuális gépeken futó SQL Server használatával. 
* A [Felhőalapú SQL Server-verzió választása: Azure SQL Database (PaaS) adatbázis vagy az Azure virtuális gépeken futó SQL Server (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md) című témakör részletesen összehasonlítja az SQL Database-t és a virtuális gépeken futó SQL Servert.

## <a name="create-a-new-sql-vm"></a>Új SQL virtuális gép létrehozása
hello következő szakaszokban közvetlen hivatkozások toohello Azure-portálon a hello SQL Server virtuális gép a gyűjtemény lemezképei. Attól függően, hogy hello lemezképet választ vagy az SQL Server licencelési költségei perc alapon fizetési is, vagy helyezheti a saját licenc (használata BYOL).

Részletes útmutatás az új SQL virtuális gép létrehozása hello az oktatóanyagban található [egy SQL Server rendszerű virtuális gép az Azure-portálon hello](virtual-machines-windows-portal-sql-server-provision.md). Emellett tekintse át a hello [teljesítmény ajánlott eljárások az SQL Server VMs](virtual-machines-windows-sql-performance.md), amely ismerteti, hogyan tooselect hello megfelelő számítógép méret és más elérhető szolgáltatások kiépítése során.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>1. lehetőség: SQL virtuális gép létrehozása percalapú licenceléssel
hello alábbi táblázat foglalja össze hello legújabb SQL Server-rendszerképeit hello virtuálisgép-katalógusában. Kattintson az összes hivatkozás toobegin egy új SQL virtuális gép létrehozása az operációs rendszerrel, a edition és a megadott verzió. 

> [!TIP]
> toounderstand hello virtuális gép és az ezeket a lemezképeket díjszabása SQL [útmutatást az SQL Server Azure virtuális gépek díjszabása](virtual-machines-windows-sql-server-pricing-guidance.md).

| Verzió | Operációs rendszer | Kiadás |
| --- | --- | --- |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1ExpressWindowsServer2016), [Developer](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1DeveloperWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2ExpressWindowsServer2012R2) |
| **SQL Server 2012 SP3** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2) |

Ezenkívül toothis lista, más SQL Server-verziók és operációs rendszerek kombinációi érhetők el. Hello Azure-portálon keresztül piactér keresés más lemezképek megkeresése 

## <a id="BYOL"></a>2. lehetőség: SQL virtuális gép létrehozása meglévő licenchez
Saját licencet is használhat (BYOL). Ebben a forgatókönyvben csak kell fizetnie hello virtuális gép az SQL Server licencelési egyéb költségek nélkül. toouse saját licenc használata hello mátrix SQL Server-verziók, kiadásainak és operációs rendszereinek alábbi. Hello portálon, ezek előtagot a **{BYOL}**.

> [!TIP]
> Saját licence használatával hosszú távon pénzt takaríthat meg a folyamatos éles számítási feladatok esetében. További információkért tekintse meg [az SQL Server Azure virtuális gépek díjszabási útmutatóját](virtual-machines-windows-sql-server-pricing-guidance.md).

| Verzió | Operációs rendszer | Kiadás |
| --- | --- | --- |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2StandardWindowsServer2012R2) |
| **SQL Server 2012 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2) |

Ezenkívül toothis lista, más SQL Server-verziók és operációs rendszerek kombinációi érhetők el. A piactér keresési keresztül más képek található hello Azure portal (Keresés a "{BYOL} SQL Server").

> [!IMPORTANT]
> toouse BYOL VM-lemezképek, rendelkeznie kell egy nagyvállalati szerződéssel [Azure frissítési garancián keresztüli Licenchordozhatósági](https://azure.microsoft.com/pricing/license-mobility/). Érvényes licencre is kell hello verziójához/kiadásához, az SQL Server toouse keresi. Meg kell [adja meg a szükséges BYOL információk tooMicrosoft hello](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) belül **10** a virtuális gép kiépítésétől számított. 

> [!NOTE]
> Már nem lehetséges toochange hello licencelési egy perc fizetési SQL Server virtuális gép toouse márkája saját licenc. Ebben az esetben létre kell hoznia egy új BYOL VM és az adatbázisok toohello áttelepítése új virtuális Gépet. 

## <a name="manage-your-sql-vm"></a>Az SQL virtuális gép felügyelete
Az SQL Server virtuális gép kiépítése után több választható felügyeleti feladatot is végrehajthat. Bizonyos szempontokból az SQL Server konfigurálását és felügyeletét pontosan ugyanúgy kell elvégeznie, ahogy egy helyszíni SQL Server-példánnyal tenné. Azonban az egyes feladatok nem adott tooAzure. hello következő szakaszok kiemelnek néhány ilyen területet, és hivatkozások toomore információkat.

### <a name="connect-toohello-vm"></a>Csatlakozás a virtuális gép toohello
Hello legalapvetőbb felügyeleti lépések egyike tooconnect tooyour SQL Server rendszerű virtuális eszközök, például az SQL Server Management Studio (SSMS) keresztül. Hogyan tooconnect tooyour új SQL Server rendszerű virtuális Géphez, lásd: kapcsolatos utasításokat [csatlakozzon az SQL Server virtuális gépet az Azure tooa](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Adatok áttelepítése
Ha egy meglévő adatbázist használ, érdemes toomove adott toohello újonnan kiépített SQL virtuális gép. Az áttelepítési lehetőségekért és útmutatásért listájáért lásd: [egy adatbázis tooSQL kiszolgálót egy Azure virtuális gép áttelepítése](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Magas rendelkezésre állás konfigurálása
Ha magas rendelkezésre állásra van szüksége, fontolja meg az SQL Server rendelkezésre állási csoportok konfigurálását. Ehhez több Azure virtuális gépre van szükség egy virtuális hálózaton. hello Azure-portálon találhat egy sablont, amely beállítja ezt a konfigurációt. További információ: [Configure an AlwaysOn availability group in Azure Resource Manager virtual machines](virtual-machines-windows-portal-sql-alwayson-availability-groups.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (AlwaysOn rendelkezésre állási csoport konfigurálása Azure Resource Manager virtuális gépeken). Ha azt szeretné, toomanually konfigurálja a rendelkezésre állási csoport és a kapcsolódó figyelőt, lásd: [AlwaysOn rendelkezésre állási csoportok konfigurálása Azure virtuális gép](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

További magas rendelkezésre állási lehetőségekért lásd: [High Availability and Disaster Recovery for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md) (Magas rendelkezésre állás és vészhelyreállítás Azure virtuális gépeken futó SQL Serveren).

### <a name="back-up-your-data"></a>Az adatok biztonsági mentése
Azure virtuális gépek kihasználhatják a [automatikus biztonsági mentés](virtual-machines-windows-sql-automated-backup.md), amely rendszeresen hoz létre a adatbázistár tooblob biztonsági másolatait. Ezt a technikát manuálisan is alkalmazhatja. További információ: [Use Azure Storage for SQL Server Backup and Restore](virtual-machines-windows-use-storage-sql-server-backup-restore.md) (Az Azure Storage használata az SQL Server biztonsági mentéséhez és helyreállításához). A biztonsági mentési és helyreállítási lehetőségek teljes körű áttekintéséért lásd: [Backup and Restore for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md) (Az SQL Server biztonsági mentése és helyreállítása Azure virtuális gépeken).

### <a name="automate-updates"></a>Frissítések automatizálása
Azure virtuális gépek használható [automatikus javítás](virtual-machines-windows-sql-automated-patching.md) tooschedule automatikusan frissül, fontos windows- és SQL Server telepítésére szolgáló karbantartási időszakot.

### <a name="customer-experience-improvement-program-ceip"></a>Felhasználói élmény fokozása program (CEIP)
hello felhasználói élmény fokozása Program (CEIP) alapértelmezés szerint engedélyezve van. Ilyen időközönként küld jelentések tooMicrosoft toohelp javítása az SQL Server. Nincs szükség a felhasználói élmény fokozása program, kivéve, ha azt szeretné, hogy toodisable felügyeleti feladat azt kiépítése után. Testre szabhatja, vagy tiltsa le a felhasználói élmény fokozása program hello toohello VM csatlakozzon a távoli asztal. Futtassa a hello **SQL Server hiba- és használatai jelentések** segédprogram. Hajtsa végre a hello utasításokat toodisable reporting. 

További információkért lásd: hello hello CEIP szakasza [licencfeltételek elfogadása](https://msdn.microsoft.com/library/ms143343.aspx) témakör. 

## <a name="next-steps"></a>Következő lépések

Az árazással kapcsolatos kérdésekre, lásd: [útmutatást az SQL Server Azure virtuális gépek díjszabása](virtual-machines-windows-sql-server-pricing-guidance.md) és hello [Azure díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). Válassza ki a cél az SQL Server kiadása hello **operációsrendszer-szoftver** listája. Nézze meg, hello árak másképp méretű virtuális gépekhez.

További kérdései vannak? Először tekintse meg a hello [SQL Server Azure virtuális gépek – gyakori kérdések](virtual-machines-windows-sql-server-iaas-faq.md). De is vegye fel a kérdését vagy megjegyzését toohello alsó részén található bármely SQL virtuális gép témakörök toointeract Microsoft és hello Közösséggel.
