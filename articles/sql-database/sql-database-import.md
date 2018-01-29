---
title: "Az Azure SQL-adatbázis létrehozásához BACPAC fájl importálása |} Microsoft Docs"
description: "Hozzon létre egy newAzure SQL-adatbázis BACPAC fájl importálásával."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: cf9a9631-56aa-4985-a565-1cacc297871d
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/26/2017
ms.author: carlrab
ms.workload: Active
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 34dee9511822acec46ba4854729939b84f3c06c6
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/31/2017
---
# <a name="import-a-bacpac-file-to-a-new-azure-sql-database"></a>Új Azure SQL-adatbázis BACPAC fájl importálása

Ha egy adatbázis archívumból importálnia kell vagy egy másik platformról áttelepítésekor importálhatja az adatbázisséma és adatainak áttelepítését egy [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fájlt. Egy BACPAC egy ZIP-fájlt tartalmazó a metaadatokat és az SQL Server-adatbázis BACPAC kiterjesztésű fájl. Egy BACPAC fájl importálhatók az Azure blob storage (csak a standard tárolási) vagy egy helyszíni hely a helyi tárolóból. Az importálási sebesség maximalizálása érdekében azt javasoljuk, hogy szintet adjon meg magasabb szolgáltatás és teljesítményszintet, például egy P6, és majd méretezhető, az importálás sikeres után szükség szerint le. Az adatbázis kompatibilitási szintjét az importálás után is, a forrás-adatbázis kompatibilitási szintjének alapul. 

> [!IMPORTANT] 
> Az adatbázis az Azure SQL Database az áttelepítés után válassza ki az adatbázist, a jelenlegi kompatibilitási szinten (100. szint a AdventureWorks2008R2 adatbázis) vagy magasabb szinten működik. A implications és a beállításokat egy adatbázis kompatibilitási szintű működő további információkért lásd: [adatbázis kompatibilitási szintje ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Lásd még: [adatbázis HATÓKÖRŰ konfiguráció ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) kompatibilitási szintre vonatkozó további adatbázis-szintű beállítással kapcsolatos információkat.   >

> [!NOTE]
> Egy új adatbázist egy BACPAC importálni, először létre kell hoznia egy Azure SQL Database logikai kiszolgálóhoz. Az oktatóanyag bemutatja, hogyan egy SQL Server-adatbázis áttelepítése az Azure SQL Database szolgáltatásba SQLPackage, lásd: [SQL Server-adatbázis áttelepítése](sql-database-migrate-your-sql-server-database.md)
>

## <a name="import-from-a-bacpac-file-using-azure-portal"></a>Azure-portál használatával BACPAC-fájlból való importálása

Ez a cikk ismerteti az Azure SQL-adatbázis tárolja az Azure blob storage használatával BACPAC-fájlból való létrehozására vonatkozó utasításokat a [Azure-portálon](https://portal.azure.com). Importálás csak az Azure portál használatával támogatja a BACPAC-fájl importálását az Azure blob storage.

Az Azure portál használatával adatbázis importálásához nyissa meg a társítsa az adatbázist, és kattintson a kiszolgálóhoz tartozó lapon **importálása** az eszköztáron. Adja meg a tárfiók és tároló, és válassza ki az importálni kívánt BACPAC. Adja meg az új adatbázist (általában a megegyezik az eredeti), és adja meg a célként megadott SQL Server hitelesítő adatokat.  

   ![Adatbázis importálása](./media/sql-database-import/import.png)

Az importálási művelet előrehaladásának figyeléséhez, nyissa meg az importált adatbázist tartalmazó logikai kiszolgálóhoz tartozó lapon. Görgessen le a **műveletek** majd **Import/Export** előzményeit.

### <a name="monitor-the-progress-of-an-import-operation"></a>Az importálási művelet előrehaladásának figyeléséhez

Az importálási művelet előrehaladásának figyeléséhez, nyissa meg a logikai kiszolgáló amelybe az adatbázis importált importált lapját. Görgessen le a **műveletek** majd **Import/Export** előzményeit.
   
   ![Importálás](./media/sql-database-import/import-history.png) ![importálás állapota](./media/sql-database-import/import-status.png)

Ellenőrizze, hogy az adatbázis a kiszolgálón élő, kattintson a **SQL-adatbázisok** és ellenőrizze, hogy az új adatbázis **Online**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>SQLPackage használatával BACPAC-fájlból való importálása

Egy SQL adatbázis használatával importálhatja a [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) parancssori segédprogram, lásd: [importálja a paraméterek és a Tulajdonságok](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties). A SQLPackage segédprogram érhető el a legújabb verziójú [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) és [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), vagy letöltheti a legújabb [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) közvetlenül a Microsoft letöltőközpontból.

Méretezés és teljesítmény a legtöbb éles környezetben a SQLPackage segédprogram használatát javasoljuk. További információ a BACPAC-fájlokkal végzett migrálásról az SQL Server ügyféltanácsadói csapat blogján: [Migrálás SQL Serverről az Azure SQL Database-re BACPAC-fájlokkal](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Az alábbi SQLPackage parancsot a parancsfájl például importálásáról lásd a **AdventureWorks2008R2** Azure SQL Database logikai kiszolgálóhoz, nevű a helyi tároló adatbázis **mynewserver20170403** ebben a példában. Ez a parancsfájl jeleníti meg az új adatbázis létrehozása **myMigratedDatabase**, egy szolgáltatási szint a **prémium**, és a szolgáltatás célkitűzése **P6**. Ezeket az értékeket a környezet szükség szerint módosítható.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Egy Azure SQL Database logikai kiszolgáló figyel az 1433-as porton. Ha vállalati tűzfalon belülről szeretne csatlakozni egy Azure SQL Database logikai kiszolgálóhoz, ennek a portnak nyitva kell lennie a vállalati tűzfalon a sikeres csatlakozáshoz.
>

Ez a példa bemutatja, hogyan importálása az Active Directory univerzális hitelesítéssel SqlPackage.exe egy adatbázist:

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>PowerShell-lel BACPAC-fájlból való importálása

Használja a [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) parancsmagot, hogy küldje el az importálási adatbázis kérelmet az Azure SQL Database szolgáltatásban. Az adatbázis méretétől függően az importálási művelet eltarthat egy ideig.

 ```powershell
 $importRequest = New-AzureRmSqlDatabaseImport -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName "MyImportSample" `
    -DatabaseMaxSizeBytes "262144000" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzureRmStorageAccountKey -ResourceGroupName "myResourceGroup" -StorageAccountName $storageaccountname).Value[0] `
    -StorageUri "http://$storageaccountname.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "ServerAdmin" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "ASecureP@assw0rd" -AsPlainText -Force)

 ```

A kérést állapotának ellenőrzéséhez használja a [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) parancsmag. Fut a kérelem után azonnal általában értéket ad vissza **állapota: esetbejegyzések**. Amikor látja **állapota: sikeres** az importálás befejeződött.

```powershell
$importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
[Console]::Write("Importing")
while ($importStatus.Status -eq "InProgress")
{
    $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$importStatus
```

> [!TIP]
Egy másik mintaparancsfájl, lásd: [adatbázis BACPAC-fájlból való importálása](scripts/sql-database-import-from-bacpac-powershell.md).

## <a name="next-steps"></a>Következő lépések
* Megtudhatja, hogyan csatlakozhat, és az importált SQL-adatbázis lekérdezése, lásd: [Csatlakozás SQL Database adatbázishoz az SQL Server Management Studio eszközt, és végezze el a T-SQL-mintalekérdezés](sql-database-connect-query-ssms.md).
* További információ a BACPAC-fájlokkal végzett migrálásról az SQL Server ügyféltanácsadói csapat blogján: [Migrálás SQL Serverről az Azure SQL Database-re BACPAC-fájlokkal](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* A teljes SQL Server adatbázis áttelepítési folyamat, többek között teljesítmény javaslatok leírását lásd: [egy SQL Server-adatbázis áttelepítése az Azure SQL Database](sql-database-cloud-migrate.md).



