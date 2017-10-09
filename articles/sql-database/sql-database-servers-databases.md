---
title: "aaaCreate & Azure SQL-kiszolgáló & adatbázisok kezelése |} Microsoft Docs"
description: "Tudnivalók Azure SQL Database-kiszolgálóhoz és az adatbázis fogalmait, illetve létrehozása és kezelése kiszolgálók és adatbázisok használatával hello Azure-portálon, a PowerShell, a hello Azure parancssori felület, a Transact-SQL és a hello REST API-t."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 0f526e388a5a620349f5a14e8d57a8355ac451ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-sql-database-servers-and-databases"></a>Azure SQL Database-kiszolgálók és adatbázisok létrehozása és kezelése

Azure SQL-adatbázis egy olyan felügyelt adatbázis a Microsoft Azure rendszerben, a rendszer létrehoz egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) meghatározott számú [számítási és tárolási erőforrásokat a különböző munkaterhelések](sql-database-service-tiers.md). Azure SQL-adatbázis nem tartozik egy Azure SQL Database logikai kiszolgáló, ami a rendszer létrehoz egy adott Azure-régiót. 

## <a name="an-azure-sql-database-can-be-a-single-pooled-or-partitioned-database"></a>Azure SQL-adatbázis egyetlen, a készletezett vagy a particionált adatbázis is lehet.

Egy Azure SQL adatbázis lehet:

- lehetnek önálló, [saját erőforráskészlettel](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) rendelkező adatbázisok (DTU);
- Része egy [SQL rugalmas készlet](sql-database-elastic-pool.md) , amely [közösen használja az erőforráscsoport](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) (edtu-k)
- részét képezhetik [horizontálisan skálázott adatbázisok kibővíthető készletének](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling), amely önálló vagy készletezett adatbázisokból állhat;
- részét képezhetik egy [több-bérlős SaaS kialakítási mintában](sql-database-design-patterns-multi-tenancy-saas-applications.md) szereplő adatbáziskészletnek, mely adatbázisok lehetnek önálló, készletezett vagy mindkétféle adatbázisok. 

> [!TIP]
> Az érvényes adatbázisnevekkel kapcsolatban lásd az [adatbázis-azonosítókat](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers) ismertető cikket. 
>
 
- hello alapértelmezett adatbázis rendezése a Microsoft Azure SQL Database által használt **SQL_LATIN1_GENERAL_CP1_CI_AS**, ahol **LATIN1_GENERAL** angol (Egyesült Államok), **CP1** kód lap 1252, **CI** nagybetűk között, és **AS** ékezet-érzékeny. További információ a hogyan tooset hello rendezése: [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).
- A Microsoft Azure SQL Database 7.3 vagy újabb tabulált adatfolyam (TDS) protokoll ügyfél verziója támogatja.
- Csak a TCP/IP-kapcsolatok engedélyezve vannak.

## <a name="what-is-an-azure-sql-logical-server"></a>Mi az az Azure SQL logikai kiszolgálóra?

Több adatbázis, beleértve a központi felügyeleti pontként működik a logikai kiszolgáló [SQL rugalmas készletek](sql-database-elastic-pool.md) [bejelentkezések](sql-database-manage-logins.md), [tűzfal-szabályok](sql-database-firewall-configure.md), [szabályok naplózás](sql-database-auditing.md), [szabályzatok fenyegetés](sql-database-threat-detection.md), és [feladatátvételi csoportok](sql-database-geo-replication-overview.md). Egy logikai kiszolgáló lehet egy másik régióban, mint az erőforráscsoportot. a logikai kiszolgáló hello léteznie kell a hello Azure SQL-adatbázis létrehozása előtt. A kiszolgálón lévő összes adatbázis jönnek létre hello belül azonos hello logikai kiszolgáló és a régióban. 


> [!IMPORTANT]
> Az SQL-adatbázis a kiszolgáló egy egy logikai szerkezet, amely nem egyezik, akkor előfordulhat, hogy ismernie kell a helyszíni world hello SQL Server-példányt. Pontosabban hello SQL adatbázis-szolgáltatás nincs hello adatbázisok helyre vonatkozó garanciák a kapcsolat lehetővé teszi a tootheir logikai kiszolgáló, és nincs példányszintű hozzáférés vagy funkciókat tesz elérhetővé.
> 

Amikor létrehoz egy logikai kiszolgáló, akkor adjon meg egy kiszolgálót bejelentkezési fiókot és jelszót, amely rendszergazdai jogosultságokkal toohello master adatbázis a kiszolgálón, és ezen a kiszolgálón létrehozott összes adatbázis. A kezdeti egy egy SQL-bejelentkezési fiók. Az Azure SQL Database hitelesítéshez támogatja az SQL-hitelesítést és az Azure Active Directory-hitelesítéssel. További információ a bejelentkezési és hitelesítési: [kezelése adatbázisok és bejelentkezések az Azure SQL Database](sql-database-manage-logins.md). A Windows-hitelesítés nem támogatott. 

> [!TIP]
> Érvényes erőforrás csoport és a kiszolgáló nevét, lásd: [elnevezési szabályokat és korlátozásokat](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).
>

Az Azure Database logikai kiszolgáló:

- Azure-előfizetés belül létrejött, de a benne lévő erőforrások tooanother előfizetéssel áthelyezhető
- Az adatbázisok rugalmas készletek és az adatraktárak hello szülő erőforrás
- Egy névtér biztosít az adatraktárak, adatbázisok és rugalmas készletek
- Az erős élettartama szemantika - törlési egy kiszolgálót, és törli a logikai tárolója hello tartalmazott adatbázisok rugalmas készletek és az adatraktárak
- Részt vesz [Azure szerepköralapú hozzáférés-vezérlés (RBAC)](/active-directory/role-based-access-control-what-is) -adatbázisok, a rugalmas készletek és a adatraktárak belül a kiszolgáló hozzáférési jogosultságok öröklése a hello kiszolgálóról
- Van egy magasrendű elem hello identitás adatbázisok, rugalmas készletek és az Azure-erőforrás adatraktárak felügyeleti szempontból (hello az URL-címet a séma adatbázisok és a készletek)
- Közösen helyezi el egy adott régió erőforrásait.
- Kapcsolódási végpontot biztosít az adatbázis-hozzáféréshez (<serverName>.database.windows.net)
- Dinamikus felügyeleti nézetek keresztül csatlakozó tooa fő adatbázis által tartalmazott erőforrások vonatkozó hozzáférési toometadata biztosít 
- Megadja a hello hatókört felügyeleti házirendek, amelyek tooits adatbázisok - bejelentkezések alkalmazni, tűzfal, naplózni, a fenyegetés észlelési stb. 
- Korlátozza a kvóta hello szülő előfizetésen belül (alapértelmezés szerint - előfizetésenként hat kiszolgáló [előfizetés korlátozza itt talál](../azure-subscription-service-limits.md))
- Hello hatókör adatbázis kvóta és a DTU-kvótát biztosít hello erőforrásokat tartalmaz (például 45 000 DTU)
- Hello versioning hatókör képességeinek engedélyezve a benne lévő erőforrások 
- Kiszolgálószintű rendszerbiztonsági tagként bejelentkezve a kiszolgáló minden adatbázisa felügyelhető.
- Bejelentkezések tartalmazhat hasonló toothose kapnak hozzáférést tooone vagy további adatbázisok hello kiszolgálón, és a helyszíni SQL Server-példány a megadott korlátozott rendszergazdai jogosultságokkal. További információk: [Bejelentkezések](sql-database-manage-logins.md).

## <a name="azure-sql-databases-protected-by-sql-database-firewall"></a>Az Azure SQL Database-tűzfal által védett SQL-adatbázisok

toohelp az adatok védelme érdekében a [SQL Database-tűzfal](sql-database-firewall-configure.md) megakadályozza, hogy minden tooyour adatbáziskiszolgáló vagy közvetlenül az Azure-előfizetés kapcsolaton keresztül a kapcsolat toohello kiszolgálón kívül az adatbázisok bármelyikét. tooenable további kapcsolatot, akkor kell [hozzon létre egy vagy több tűzfalszabályok](sql-database-firewall-configure.md#creating-and-managing-firewall-rules). Létrehozása és kezelése SQL rugalmas készletek: [rugalmas készletek](sql-database-elastic-pool.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-hello-azure-portal"></a>Kezelheti az Azure SQL-kiszolgálók, a adatbázisok és a tűzfalak hello Azure-portál használatával

Hello Azure SQL adatbázis erőforráscsoport időben vagy magán hello kiszolgálón létrehozásakor is létrehozhat. Több módon az első tooa új SQL server formában, vagy hozzon létre egy új SQL-kiszolgálót, vagy egy új adatbázis létrehozásának részeként. 

### <a name="create-a-blank-sql-server-logical-server"></a>Hozzon létre egy üres SQL-kiszolgáló (logikai kiszolgáló)

egy Azure SQL Database (adatbázis) nélkül server használatával toocreate hello [Azure-portálon](https://portal.azure.com), keresse meg a tooa üres SQL-kiszolgáló (logikai kiszolgáló) űrlap. hello következő képernyőfelvétel egy metódus egy űrlap toocreate nyithatók meg egy üres logikai SQL-kiszolgáló. 

   ![befejeződött a logikai kiszolgáló űrlap létrehozása](./media/sql-database-migrate-your-sql-server-database/logical-server-create-completed.png)

Ha más módszerrel toothis képernyő, hello hello űrlapon található azonos.

### <a name="create-a-blank-or-sample-sql-database"></a>Üres, vagy a minta SQL-adatbázis létrehozása

egy Azure SQL adatbázis használatával toocreate hello [Azure-portálon](https://portal.azure.com)tooa üres SQL Database űrlap váltson, és adja meg hello szükséges adatot. Hello Azure SQL adatbázis erőforráscsoport és logikai kiszolgáló időben, vagy maga hello adatbázis létrehozásakor is létrehozhat. Hozzon létre egy üres adatbázist, vagy az Adventure Works LT. alapú minta-adatbázis létrehozása 

  ![adatbázis létrehozása-1](./media/sql-database-get-started-portal/create-database-1.png)

> [FONTOS] Az adatbázis tarifacsomagjának hello kiválasztásával további információkért lásd: [szolgáltatásszintek](sql-database-service-tiers.md).
>

### <a name="manage-an-existing-sql-server"></a>Meglévő SQL server kezelése

toomanage egy meglévő kiszolgálót, nyissa meg a toohello kiszolgáló számos módszer – például adott SQL-adatbázis lap, hello **SQL Server-kiszolgálók** lap vagy hello **összes erőforrás** lap. a következő képernyőfelvételen látható hogyan hello egy kiszolgálószintű tűzfal beállítása a hello toobegin **áttekintése** a kiszolgálóhoz tartozó lapon. 

   ![a logikai kiszolgáló – áttekintés](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

toomanage egy meglévő adatbázist, keresse meg a toohello **SQL-adatbázisok** lapon, majd kattintson a hello adatbázis toomanage kívánja. a következő képernyőfelvételen látható hogyan hello adatbázis kiszolgálószintű tűzfal beállítása a hello toobegin **áttekintése** adatbázis lap. 

   ![kiszolgálói tűzfalszabály](./media/sql-database-get-started-portal/server-firewall-rule.png) 

> [!IMPORTANT]
> tooconfigure teljesítmény tulajdonságok adatbázis esetén lásd: [szolgáltatásszintek](sql-database-service-tiers.md).
>

> [!TIP]
> Tekintse meg az Azure portál – gyors üzembe helyezési oktatóanyag [hello Azure-portálon az Azure SQL-adatbázis létrehozása](sql-database-get-started-portal.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-powershell"></a>Azure SQL-kiszolgálók, a adatbázisok és a PowerShell-lel tűzfalak kezelése

toocreate és kezelése az Azure SQL-kiszolgáló, adatbázisok és tűzfalak az Azure PowerShell, a következő PowerShell-parancsmagok hello használata. Ha tooinstall kell, vagy PowerShell frissítése, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps). Létrehozása és kezelése SQL rugalmas készletek: [rugalmas készletek](sql-database-elastic-pool.md).

| Parancsmag | Leírás |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Létrehoz egy adatbázis |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Egy vagy több adatbázis beolvasása|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Az adatbázis tulajdonságainak megadása, vagy a meglévő adatbázis áthelyezése rugalmas készletbe|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Egy adatbázis eltávolítása|
|[Új-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Létrehoz egy erőforráscsoport]
|[Új AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|A kiszolgáló létrehozása|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Kiszolgálók adatait adja vissza|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.sql/set-azurermsqlserver)|Kiszolgáló tulajdonságainak módosítása|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Eltávolít egy kiszolgálót|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Létrehoz egy kiszolgálószintű tűzfalszabályt |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Kiszolgáló tűzfalszabályainak beolvasása|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Egy tűzfalszabály módosítása|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Egy tűzfalszabály törlése a kiszolgálóról.|

> [!TIP]
> Tekintse meg a PowerShell gyors üzembe helyezési útmutató [PowerShell használatával egyetlen Azure SQL-adatbázis létrehozása](sql-database-get-started-portal.md). PowerShell-példa parancsfájlok, lásd: [használja a Powershellt toocreate egyetlen Azure SQL-adatbázis és a tűzfalszabályok konfigurálása](scripts/sql-database-create-and-configure-database-powershell.md) és [figyelő és a skála egyetlen SQL adatbázis-PowerShell használatával](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-hello-azure-cli"></a>Kezelheti az Azure SQL-kiszolgálók, a adatbázisok és a tűzfalak hello Azure parancssori felület használatával

toocreate és kezelése az Azure SQL server, adatbázisok és tűzfalak az hello [Azure CLI](/cli/azure/overview), hello következő [Azure CLI SQL Database](/cli/azure/sql/db) parancsok. Használjon hello [felhő rendszerhéj](/azure/cloud-shell/overview) toorun hello CLI-t a böngészőben vagy [telepítése](/cli/azure/install-azure-cli) azt macOS, Linux vagy a Windows. Létrehozása és kezelése SQL rugalmas készletek: [rugalmas készletek](sql-database-elastic-pool.md).

| Parancsmag | Leírás |
| --- | --- |
|[az sql-adatbázis létrehozása](/cli/azure/sql/db#create) |Létrehoz egy adatbázis|
|[az sql db listája](/cli/azure/sql/db#list)|Felsorolja az összes adatbázist és az adatraktárak egy kiszolgálón, vagy minden adatbázisok rugalmas készlethez|
|[az sql db lista-verziók](/cli/azure/sql/db#list-editions)|Listák elérhető szolgáltatási célkitűzések és tárolási korlátai|
|[az sql db lista-módjait](/cli/azure/sql/db#list-usages)|Adatbázis-módjait beolvasása|
|[az sql db megjelenítése](/cli/azure/sql/db#show)|Egy adatbázis vagy adatraktár beolvasása|
|[az sql-adatbázis frissítése](/cli/azure/sql/db#update)|Frissíti az adatbázist|
|[az sql-adatbázis törlése](/cli/azure/sql/db#delete)|Egy adatbázis eltávolítása|
|[az csoport létrehozása](/cli/azure/group#create)|Létrehoz egy erőforráscsoportot|
|[az sql-kiszolgáló létrehozása](/cli/azure/sql/server#create)|A kiszolgáló létrehozása|
|[az sql server listája](/cli/azure/sql/server#list)|Kiszolgálók listája|
|[az sql server lista-módjait](/cli/azure/sql/server#list-usages)|Kiszolgáló módjait adja vissza|
|[az sql server megjelenítése](/cli/azure/sql/server#show)|A kiszolgáló beolvasása|
|[az sql server frissítése](/cli/azure/sql/server#update)|A kiszolgáló frissítése|
|[az sql server törlése](/cli/azure/sql/server#delete)|Kiszolgáló törlése|
|[az sql server-tűzfalszabály létrehozása](/cli/azure/sql/server/firewall-rule#create)|Létrehoz egy tűzfalszabály létrehozása|
|[az sql server tűzfal-szabályok listája](/cli/azure/sql/server/firewall-rule#list)|Listák hello tűzfalszabályok egy kiszolgálón|
|[az sql server-tűzfalszabály megjelenítése](/cli/azure/sql/server/firewall-rule#show)|A tűzfalszabályok hello részleteit tartalmazza|
|[az sql server tűzfal-szabály frissítése](/cli/azure/sql/server/firewall-rule#update)|A tűzfal szabály frissítése|
|[az sql server-tűzfalszabály törlése](/cli/azure/sql/server/firewall-rule#delete)|Egy tűzfalszabály törlése|

> [!TIP]
> Tekintse meg az Azure CLI gyors üzembe helyezési útmutató [hello Azure parancssori felület használatával egyetlen Azure SQL-adatbázis létrehozása](sql-database-get-started-cli.md). Az Azure parancssori felület parancsfájlpéldákat, lásd: [használata CLI toocreate egyetlen Azure SQL-adatbázis és a tűzfalszabályok konfigurálása](scripts/sql-database-create-and-configure-database-cli.md) és [használata CLI toomonitor és a skála egy SQL-adatbázis](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-transact-sql"></a>Azure SQL-kiszolgálók, a adatbázisok és a tűzfalak Transact-SQL használatával kezelése

toocreate és kezelése az Azure SQL-kiszolgáló, adatbázisok és a Transact-SQL tűzfalak, használja a következő T-SQL parancsokkal hello. Ezek a parancsok használata Azure-portálon hello kiadhatja [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), vagy bármely más programot, amely tooan Azure SQL Database-kiszolgálóhoz csatlakozhat, és adja át a Transact-SQL parancsok. SQL rugalmas készletek kezelése, lásd: [rugalmas készletek](sql-database-elastic-pool.md).

> [!IMPORTANT]
> Nem hozható létre vagy Transact-SQL használatával kiszolgáló törlése.
>

| Parancs | Leírás |
| --- | --- |
|[ADATBÁZIS (az Azure SQL Database) létrehozása](/sql/t-sql/statements/create-database-azure-sql-database)|Egy új adatbázist hoz létre. Csatlakoztatott toohello főadatbázis toocreate egy új adatbázist kell lennie.|
| [Az ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Azure SQL-adatbázis módosítása. |
|[Az ALTER DATABASE (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Egy Azure SQL Data Warehouse módosítja.|
|[ADATBÁZIS (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Adatbázis törlése.|
|[sys.database_service_objectives (az Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Értéket ad vissza a edition (szolgáltatási réteg), a szolgáltatási cél (IP-címek) és a rugalmas készlet nevét, ha van egy Azure SQL database vagy az Azure SQL Data Warehouse hello. Egy Azure SQL adatbázis-kiszolgáló toohello főadatbázis jelentkezik be, az összes adatbázis ad vissza adatokat. Az Azure SQL Data Warehouse csatlakoztatott toohello master adatbázisban kell lennie.|
|[sys.dm_db_resource_stats (az Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Az Azure SQL Database adatbázishoz CPU, a i/o és a memória-felhasználás adja vissza. Egy sor létezik 15 másodpercenként, akkor is, ha nincs tevékenység hello adatbázisban van.|
|[sys.resource_stats (az Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|CPU használatát és a tároló adatait jeleníti meg az Azure SQL-adatbázis. hello adatok gyűjtése és 5 perces időközönként belül.|
|[sys.database_connection_stats (az Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|SQL-adatbázis adatbázis csatlakozási eseményeket, és adatbázis-kapcsolat sikeres és sikertelen áttekintést nyújt a statisztikákat tartalmaz. |
|[sys.event_log (az Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Sikeres Azure SQL Database adatbázis-kapcsolatok, a csatlakozási hibák és a holtpontok adja vissza. Ezen információk tootrack használhatja, vagy az adatbázis-tevékenység az SQL Database hibaelhárítása.|
|[sp_set_firewall_rule (az Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Létrehozza vagy frissíti az SQL-adatbázis-kiszolgáló hello kiszolgálószintű tűzfal beállításait. Ez a tárolt eljárás hello főadatbázis toohello kiszolgálószintű fő bejelentkezéssel csak érhető el. Egy kiszolgálószintű tűzfalszabályt csak segítségével hozhatók létre Transact-SQL Azure-szintű engedélyekkel rendelkező felhasználó hello első kiszolgálószintű tűzfalszabály létrehozása után|
|[sys.firewall_rules (az Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Hello kiszolgálószintű tűzfal beállításait a Microsoft Azure SQL Database társított információt ad vissza.|
|[sp_delete_firewall_rule (az Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Az SQL Database-kiszolgálóhoz kiszolgálószintű tűzfal beállításainak eltávolítása. Ez a tárolt eljárás hello főadatbázis toohello kiszolgálószintű fő bejelentkezéssel csak érhető el.|
|[sp_set_database_firewall_rule (az Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Létrehozza vagy frissíti az adatbázis-szintű tűzfalszabályok hello az Azure SQL Database vagy az SQL Data Warehouse. Adatbázis tűzfalszabályainak hello főadatbázis, és az SQL Database felhasználói adatbázisok konfigurálható. Adatbázis tűzfalszabályainak használatával tartalmazott adatbázis-felhasználók esetén hasznos. |
|[sys.database_firewall_rules (az Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Hello adatbázis szintű tűzfal beállításait a Microsoft Azure SQL Database társított információt ad vissza. |
|[sp_delete_database_firewall_rule (az Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Az Azure SQL Database vagy az SQL Data Warehouse adatbázis szintű tűzfal beállítást eltávolítja. |


> [!TIP]
> Gyors üzembe helyezési útmutató a Microsoft Windows SQL Server Management Studio használatával, lásd: [Azure SQL Database: használja az SQL Server Management Studio tooconnect és lekérdezési adatok](sql-database-connect-query-ssms.md). A gyors üzembe helyezési útmutató segítségével a Visual Studio Code hello macOS, Linux vagy a Windows, lásd: [Azure SQL Database: használja a Visual Studio Code tooconnect és lekérdezési adatok](sql-database-connect-query-vscode.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-hello-rest-api"></a>Azure SQL-kiszolgálók, a adatbázisok és a tűzfalak hello REST API használatával kezelése

toocreate és kezelése az Azure SQL-kiszolgáló, adatbázisok és tűzfalak hello REST API használatával, lásd: [Azure SQL Database REST API](/rest/api/sql/).

## <a name="next-steps"></a>Következő lépések

- SQL rugalmas készleteket, használó adatbázisok készletezését kapcsolatos toolearn lásd: [rugalmas készletek](sql-database-elastic-pool.md).
- További információ az Azure SQL Database szolgáltatás hello: [Mi az SQL Database?](sql-database-technical-overview.md).
- toolearn egy SQL Server adatbázis tooAzure áttelepítésével kapcsolatban lásd: [tooAzure SQL-adatbázis áttelepítése](sql-database-cloud-migrate.md).
- A támogatott funkciókkal kapcsolatos tudnivalókat lásd: [Funkciók](sql-database-features.md).
