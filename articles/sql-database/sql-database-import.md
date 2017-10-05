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
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 285e17ed6d0ce700cb518864df7a3b5f5e55bee5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="import-a-bacpac-file-to-a-new-azure-sql-database"></a><span data-ttu-id="c149e-103">Új Azure SQL-adatbázis BACPAC fájl importálása</span><span class="sxs-lookup"><span data-stu-id="c149e-103">Import a BACPAC file to a new Azure SQL Database</span></span>

<span data-ttu-id="c149e-104">Ha egy adatbázis archívumból importálnia kell vagy egy másik platformról áttelepítésekor importálhatja az adatbázisséma és adatainak áttelepítését egy [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fájlt.</span><span class="sxs-lookup"><span data-stu-id="c149e-104">When you need to import a database from an archive or when migrating from another platform, you can import the database schema and data from a [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) file.</span></span> <span data-ttu-id="c149e-105">Egy BACPAC egy ZIP-fájlt tartalmazó a metaadatokat és az SQL Server-adatbázis BACPAC kiterjesztésű fájl.</span><span class="sxs-lookup"><span data-stu-id="c149e-105">A BACPAC file is a ZIP file with an extension of BACPAC containing the metadata and data from a SQL Server database.</span></span> <span data-ttu-id="c149e-106">Egy BACPAC fájl importálhatók az Azure blob storage (csak a standard tárolási) vagy egy helyszíni hely a helyi tárolóból.</span><span class="sxs-lookup"><span data-stu-id="c149e-106">A BACPAC file can be imported from Azure blob storage (standard storage only) or from local storage in an on-premises location.</span></span> <span data-ttu-id="c149e-107">Az importálási sebesség maximalizálása érdekében azt javasoljuk, hogy szintet adjon meg magasabb szolgáltatás és teljesítményszintet, például egy P6, és majd méretezhető, az importálás sikeres után szükség szerint le.</span><span class="sxs-lookup"><span data-stu-id="c149e-107">To maximize the import speed, we recommend that you specify a higher service tier and performance level, such as a P6, and then scale to down as appropriate after the import is successful.</span></span> <span data-ttu-id="c149e-108">Az adatbázis kompatibilitási szintjét az importálás után is, a forrás-adatbázis kompatibilitási szintjének alapul.</span><span class="sxs-lookup"><span data-stu-id="c149e-108">Also, the database compatibility level after the import is based on the compatibility level of the source database.</span></span> 

> [!IMPORTANT] 
> <span data-ttu-id="c149e-109">Az adatbázis az Azure SQL Database az áttelepítés után válassza ki az adatbázist, a jelenlegi kompatibilitási szinten (100. szint a AdventureWorks2008R2 adatbázis) vagy magasabb szinten működik.</span><span class="sxs-lookup"><span data-stu-id="c149e-109">After you migrate your database to Azure SQL Database, you can choose to operate the database at its current compatibility level (level 100 for the AdventureWorks2008R2 database) or at a higher level.</span></span> <span data-ttu-id="c149e-110">A implications és a beállításokat egy adatbázis kompatibilitási szintű működő további információkért lásd: [adatbázis kompatibilitási szintje ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span><span class="sxs-lookup"><span data-stu-id="c149e-110">For more information on the implications and options for operating a database at a specific compatibility level, see [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span></span> <span data-ttu-id="c149e-111">Lásd még: [adatbázis HATÓKÖRŰ konfiguráció ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) kompatibilitási szintre vonatkozó további adatbázis-szintű beállítással kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="c149e-111">See also [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) for information about additional database-level settings related to compatibility levels.</span></span>   >

> [!NOTE]
> <span data-ttu-id="c149e-112">Egy új adatbázist egy BACPAC importálni, először létre kell hoznia egy Azure SQL Database logikai kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="c149e-112">To import a BACPAC to a new database, you must first create an Azure SQL Database logical server.</span></span> <span data-ttu-id="c149e-113">Az oktatóanyag bemutatja, hogyan egy SQL Server-adatbázis áttelepítése az Azure SQL Database szolgáltatásba SQLPackage, lásd: [SQL Server-adatbázis áttelepítése](sql-database-migrate-your-sql-server-database.md)</span><span class="sxs-lookup"><span data-stu-id="c149e-113">For a tutorial showing you how to migrate a SQL Server database to Azure SQL Database using SQLPackage, see [Migrate a SQL Server Database](sql-database-migrate-your-sql-server-database.md)</span></span>
>

## <a name="import-from-a-bacpac-file-using-azure-portal"></a><span data-ttu-id="c149e-114">Azure-portál használatával BACPAC-fájlból való importálása</span><span class="sxs-lookup"><span data-stu-id="c149e-114">Import from a BACPAC file using Azure portal</span></span>

<span data-ttu-id="c149e-115">Ez a cikk ismerteti az Azure SQL-adatbázis tárolja az Azure blob storage használatával BACPAC-fájlból való létrehozására vonatkozó utasításokat a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c149e-115">This article provides directions for creating an Azure SQL database from a BACPAC file stored in Azure blob storage using the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c149e-116">Importálás csak az Azure portál használatával támogatja a BACPAC-fájl importálását az Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="c149e-116">Import using the Azure portal only supports importing a BACPAC file from Azure blob storage.</span></span>

<span data-ttu-id="c149e-117">Szeretne importálni egy adatbázist, az Azure portál használatával, nyissa meg az adatbázis lapját, és kattintson a **importálása** az eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="c149e-117">To import a database using the Azure portal, open the page for your database and click **Import** on the toolbar.</span></span> <span data-ttu-id="c149e-118">Adja meg a tárfiók és tároló, és válassza ki az importálni kívánt BACPAC.</span><span class="sxs-lookup"><span data-stu-id="c149e-118">Specify the storage account and container, and select the BACPAC file you want to import.</span></span> <span data-ttu-id="c149e-119">Adja meg az új adatbázist (általában a megegyezik az eredeti), és adja meg a célként megadott SQL Server hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="c149e-119">Select the size of the new database (usually the same as origin) and provide the destination SQL Server credentials.</span></span>  

   ![Adatbázis importálása](./media/sql-database-import/import.png)

<span data-ttu-id="c149e-121">Az importálási művelet előrehaladásának figyeléséhez, nyissa meg az importált adatbázist tartalmazó logikai kiszolgálóhoz tartozó lapon.</span><span class="sxs-lookup"><span data-stu-id="c149e-121">To monitor the progress of the import operation, open the page for the logical server containing the database being imported.</span></span> <span data-ttu-id="c149e-122">Görgessen le a **műveletek** majd **Import/Export** előzményeit.</span><span class="sxs-lookup"><span data-stu-id="c149e-122">Scroll down to **Operations** and then click **Import/Export** history.</span></span>

### <a name="monitor-the-progress-of-an-import-operation"></a><span data-ttu-id="c149e-123">Az importálási művelet előrehaladásának figyeléséhez</span><span class="sxs-lookup"><span data-stu-id="c149e-123">Monitor the progress of an import operation</span></span>

<span data-ttu-id="c149e-124">Az importálási művelet előrehaladásának figyeléséhez, nyissa meg a logikai kiszolgáló amelybe az adatbázis importált importált lapját.</span><span class="sxs-lookup"><span data-stu-id="c149e-124">To monitor the progress of the import operation, open the page for the logical server into which the database is being imported imported.</span></span> <span data-ttu-id="c149e-125">Görgessen le a **műveletek** majd **Import/Export** előzményeit.</span><span class="sxs-lookup"><span data-stu-id="c149e-125">Scroll down to **Operations** and then click **Import/Export** history.</span></span>
   
   <span data-ttu-id="c149e-126">![Importálás](./media/sql-database-import/import-history.png) ![importálás állapota](./media/sql-database-import/import-status.png)</span><span class="sxs-lookup"><span data-stu-id="c149e-126">![import](./media/sql-database-import/import-history.png) ![import status](./media/sql-database-import/import-status.png)</span></span>

<span data-ttu-id="c149e-127">Ellenőrizze, hogy az adatbázis a kiszolgálón élő, kattintson a **SQL-adatbázisok** és ellenőrizze, hogy az új adatbázis **Online**.</span><span class="sxs-lookup"><span data-stu-id="c149e-127">To verify the database is live on the server, click **SQL databases** and verify the new database is **Online**.</span></span>

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a><span data-ttu-id="c149e-128">SQLPackage használatával BACPAC-fájlból való importálása</span><span class="sxs-lookup"><span data-stu-id="c149e-128">Import from a BACPAC file using SQLPackage</span></span>

<span data-ttu-id="c149e-129">Egy SQL adatbázis használatával importálhatja a [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) parancssori segédprogram, lásd: [importálja a paraméterek és a Tulajdonságok](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties).</span><span class="sxs-lookup"><span data-stu-id="c149e-129">To import a SQL database using the [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) command-line utility, see [Import parameters and properties](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties).</span></span> <span data-ttu-id="c149e-130">A SQLPackage segédprogram érhető el a legújabb verziójú [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) és [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), vagy letöltheti a legújabb [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) közvetlenül a Microsoft letöltőközpontból.</span><span class="sxs-lookup"><span data-stu-id="c149e-130">The SQLPackage utility ships with the latest versions of [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) and [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), or you can download the latest version of [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) directly from the Microsoft download center.</span></span>

<span data-ttu-id="c149e-131">Méretezés és teljesítmény a legtöbb éles környezetben a SQLPackage segédprogram használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="c149e-131">We recommend the use of the SQLPackage utility for scale and performance in most production environments.</span></span> <span data-ttu-id="c149e-132">További információ a BACPAC-fájlokkal végzett migrálásról az SQL Server ügyféltanácsadói csapat blogján: [Migrálás SQL Serverről az Azure SQL Database-re BACPAC-fájlokkal](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="c149e-132">For a SQL Server Customer Advisory Team blog about migrating using BACPAC files, see [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>

<span data-ttu-id="c149e-133">Az alábbi SQLPackage parancsot a parancsfájl például importálásáról lásd a **AdventureWorks2008R2** Azure SQL Database logikai kiszolgálóhoz, nevű a helyi tároló adatbázis **mynewserver20170403** ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="c149e-133">See the following SQLPackage command for a script example for how to import the **AdventureWorks2008R2** database from local storage to an Azure SQL Database logical server, called **mynewserver20170403** in this example.</span></span> <span data-ttu-id="c149e-134">Ez a parancsfájl jeleníti meg az új adatbázis létrehozása **myMigratedDatabase**, egy szolgáltatási szint a **prémium**, és a szolgáltatás célkitűzése **P6**.</span><span class="sxs-lookup"><span data-stu-id="c149e-134">This script shows the creation of a new database called **myMigratedDatabase**, with a service tier of **Premium**, and a Service Objective of **P6**.</span></span> <span data-ttu-id="c149e-135">Ezeket az értékeket a környezet szükség szerint módosítható.</span><span class="sxs-lookup"><span data-stu-id="c149e-135">Change these values as appropriate to your environment.</span></span>

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

   ![sqlpackage importálása](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> <span data-ttu-id="c149e-137">Egy Azure SQL Database logikai kiszolgáló figyel az 1433-as porton.</span><span class="sxs-lookup"><span data-stu-id="c149e-137">An Azure SQL Database logical server listens on port 1433.</span></span> <span data-ttu-id="c149e-138">Ha vállalati tűzfalon belülről szeretne csatlakozni egy Azure SQL Database logikai kiszolgálóhoz, ennek a portnak nyitva kell lennie a vállalati tűzfalon a sikeres csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="c149e-138">If you are attempting to connect to an Azure SQL Database logical server from within a corporate firewall, this port must be open in the corporate firewall for you to successfully connect.</span></span>
>

<span data-ttu-id="c149e-139">Ez a példa bemutatja, hogyan importálása az Active Directory univerzális hitelesítéssel SqlPackage.exe egy adatbázist:</span><span class="sxs-lookup"><span data-stu-id="c149e-139">This example shows how to import a database using SqlPackage.exe with Active Directory Universal Authentication:</span></span>

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a><span data-ttu-id="c149e-140">PowerShell-lel BACPAC-fájlból való importálása</span><span class="sxs-lookup"><span data-stu-id="c149e-140">Import from a BACPAC file using PowerShell</span></span>

<span data-ttu-id="c149e-141">Használja a [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) parancsmagot, hogy küldje el az importálási adatbázis kérelmet az Azure SQL Database szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="c149e-141">Use the [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet to submit an import database request to the Azure SQL Database service.</span></span> <span data-ttu-id="c149e-142">Az adatbázis méretétől függően az importálási művelet eltarthat egy ideig.</span><span class="sxs-lookup"><span data-stu-id="c149e-142">Depending on the size of your database, the import operation may take some time to complete.</span></span>

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

<span data-ttu-id="c149e-143">A kérést állapotának ellenőrzéséhez használja a [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c149e-143">To check the status of the import request, use the [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet.</span></span> <span data-ttu-id="c149e-144">Fut a kérelem után azonnal általában értéket ad vissza **állapota: esetbejegyzések**.</span><span class="sxs-lookup"><span data-stu-id="c149e-144">Running this immediately after the request usually returns **Status: InProgress**.</span></span> <span data-ttu-id="c149e-145">Amikor látja **állapota: sikeres** az importálás befejeződött.</span><span class="sxs-lookup"><span data-stu-id="c149e-145">When you see **Status: Succeeded** the import is complete.</span></span>

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
<span data-ttu-id="c149e-146">Egy másik mintaparancsfájl, lásd: [adatbázis BACPAC-fájlból való importálása](scripts/sql-database-import-from-bacpac-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="c149e-146">For another script example, see [Import a database from a BACPAC file](scripts/sql-database-import-from-bacpac-powershell.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c149e-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c149e-147">Next steps</span></span>
* <span data-ttu-id="c149e-148">Megtudhatja, hogyan csatlakozhat, és az importált SQL-adatbázis lekérdezése, lásd: [Csatlakozás SQL Database adatbázishoz az SQL Server Management Studio eszközt, és végezze el a T-SQL-mintalekérdezés](sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="c149e-148">To learn how to connect to and query an imported SQL Database, see [Connect to SQL Database with SQL Server Management Studio and perform a sample T-SQL query](sql-database-connect-query-ssms.md).</span></span>
* <span data-ttu-id="c149e-149">További információ a BACPAC-fájlokkal végzett migrálásról az SQL Server ügyféltanácsadói csapat blogján: [Migrálás SQL Serverről az Azure SQL Database-re BACPAC-fájlokkal](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="c149e-149">For a SQL Server Customer Advisory Team blog about migrating using BACPAC files, see [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>
* <span data-ttu-id="c149e-150">A teljes SQL Server adatbázis áttelepítési folyamat, többek között teljesítmény javaslatok leírását lásd: [egy SQL Server-adatbázis áttelepítése az Azure SQL Database](sql-database-cloud-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="c149e-150">For a discussion of the entire SQL Server database migration process, including performance recommendations, see [Migrate a SQL Server database to Azure SQL Database](sql-database-cloud-migrate.md).</span></span>



