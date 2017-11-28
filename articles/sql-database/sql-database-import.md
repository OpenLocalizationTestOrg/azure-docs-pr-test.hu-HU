---
title: "egy BACPAC aaaImport fájl toocreate Azure SQL-adatbázis |} Microsoft Docs"
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
ms.openlocfilehash: 0d5fc93acf27b79502969fcd6199d11161915b19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="import-a-bacpac-file-tooa-new-azure-sql-database"></a><span data-ttu-id="56c52-103">Importálja a BACPAC fájl tooa új Azure SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="56c52-103">Import a BACPAC file tooa new Azure SQL Database</span></span>

<span data-ttu-id="56c52-104">Ha tooimport archívumból adatbázis van szüksége, vagy egy másik platformról áttelepítésekor hello adatbázisséma és adatainak áttelepítését importálni egy [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fájlt.</span><span class="sxs-lookup"><span data-stu-id="56c52-104">When you need tooimport a database from an archive or when migrating from another platform, you can import hello database schema and data from a [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) file.</span></span> <span data-ttu-id="56c52-105">Egy BACPAC egy ZIP-fájlt tartalmazó hello metaadatok és adatainak áttelepítését egy SQL Server-adatbázis BACPAC kiterjesztésű fájl.</span><span class="sxs-lookup"><span data-stu-id="56c52-105">A BACPAC file is a ZIP file with an extension of BACPAC containing hello metadata and data from a SQL Server database.</span></span> <span data-ttu-id="56c52-106">Egy BACPAC fájl importálhatók az Azure blob storage (csak a standard tárolási) vagy egy helyszíni hely a helyi tárolóból.</span><span class="sxs-lookup"><span data-stu-id="56c52-106">A BACPAC file can be imported from Azure blob storage (standard storage only) or from local storage in an on-premises location.</span></span> <span data-ttu-id="56c52-107">Adjon meg egy magasabb és teljesítményszintet szolgáltatásszintet, például egy P6, és szükség szerint toodown majd méretezhető, hello importálási befejezését követően ajánlott toomaximize hello importálási sebessége.</span><span class="sxs-lookup"><span data-stu-id="56c52-107">toomaximize hello import speed, we recommend that you specify a higher service tier and performance level, such as a P6, and then scale toodown as appropriate after hello import is successful.</span></span> <span data-ttu-id="56c52-108">Hello adatbázis kompatibilitási szintje hello importálás után is, hello source adatbázis kompatibilitási szintje hello alapul.</span><span class="sxs-lookup"><span data-stu-id="56c52-108">Also, hello database compatibility level after hello import is based on hello compatibility level of hello source database.</span></span> 

> [!IMPORTANT] 
> <span data-ttu-id="56c52-109">Az adatbázis tooAzure SQL-adatbázis az áttelepítés után dönthet úgy, hogy a jelenlegi kompatibilitási szinten (100. szint hello AdventureWorks2008R2 adatbázis) vagy magasabb szintű toooperate hello adatbázis ki.</span><span class="sxs-lookup"><span data-stu-id="56c52-109">After you migrate your database tooAzure SQL Database, you can choose toooperate hello database at its current compatibility level (level 100 for hello AdventureWorks2008R2 database) or at a higher level.</span></span> <span data-ttu-id="56c52-110">Hello hatással vannak, és egy adatbázis kompatibilitási szintű működő beállításainak további információkért lásd: [adatbázis kompatibilitási szintje ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span><span class="sxs-lookup"><span data-stu-id="56c52-110">For more information on hello implications and options for operating a database at a specific compatibility level, see [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span></span> <span data-ttu-id="56c52-111">Lásd még: [adatbázis HATÓKÖRŰ konfiguráció ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) további adatbázis-szintű beállítással kapcsolatos információkat a vonatkozó toocompatibility szintjének.</span><span class="sxs-lookup"><span data-stu-id="56c52-111">See also [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) for information about additional database-level settings related toocompatibility levels.</span></span>   >

> [!NOTE]
> <span data-ttu-id="56c52-112">tooimport BACPAC tooa új adatbázist, akkor először létre kell hoznia egy Azure SQL Database logikai kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="56c52-112">tooimport a BACPAC tooa new database, you must first create an Azure SQL Database logical server.</span></span> <span data-ttu-id="56c52-113">Egy oktatóanyag, hogy bemutatja, hogyan toomigrate egy SQL Server adatbázis-tooAzure SQL-adatbázis használata SQLPackage, lásd: [SQL Server-adatbázis áttelepítése](sql-database-migrate-your-sql-server-database.md)</span><span class="sxs-lookup"><span data-stu-id="56c52-113">For a tutorial showing you how toomigrate a SQL Server database tooAzure SQL Database using SQLPackage, see [Migrate a SQL Server Database](sql-database-migrate-your-sql-server-database.md)</span></span>
>

## <a name="import-from-a-bacpac-file-using-azure-portal"></a><span data-ttu-id="56c52-114">Azure-portál használatával BACPAC-fájlból való importálása</span><span class="sxs-lookup"><span data-stu-id="56c52-114">Import from a BACPAC file using Azure portal</span></span>

<span data-ttu-id="56c52-115">Ez a cikk ismerteti az Azure SQL-adatbázis hello használata az Azure blob storage-ban tárolt BACPAC-fájlból való létrehozására vonatkozó utasításokat [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="56c52-115">This article provides directions for creating an Azure SQL database from a BACPAC file stored in Azure blob storage using hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="56c52-116">Hello csak az Azure portál használatával importálhatók BACPAC fájlok importálása az Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="56c52-116">Import using hello Azure portal only supports importing a BACPAC file from Azure blob storage.</span></span>

<span data-ttu-id="56c52-117">egy adatbázis használatával tooimport hello Azure-portálon, az adatbázisról, és kattintson a Megnyitás hello lap **importálási** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="56c52-117">tooimport a database using hello Azure portal, open hello page for your database and click **Import** on hello toolbar.</span></span> <span data-ttu-id="56c52-118">Adja meg a hello tárfiók és tároló, és válassza ki a kívánt tooimport hello BACPAC-fájl.</span><span class="sxs-lookup"><span data-stu-id="56c52-118">Specify hello storage account and container, and select hello BACPAC file you want tooimport.</span></span> <span data-ttu-id="56c52-119">Válassza ki az új adatbázis hello hello méretet (általában hello ugyanaz, mint a forrás) és hello cél SQL Server hitelesítő adatok megadásához.</span><span class="sxs-lookup"><span data-stu-id="56c52-119">Select hello size of hello new database (usually hello same as origin) and provide hello destination SQL Server credentials.</span></span>  

   ![Adatbázis importálása](./media/sql-database-import/import.png)

<span data-ttu-id="56c52-121">hello toomonitor hello előrehaladását az importálási művelet, hello logikai kiszolgáló tartalmazó hello adatbázisának importált hello lap megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="56c52-121">toomonitor hello progress of hello import operation, open hello page for hello logical server containing hello database being imported.</span></span> <span data-ttu-id="56c52-122">Görgessen lefelé, túl**műveletek** , majd **Import/Export** előzményeit.</span><span class="sxs-lookup"><span data-stu-id="56c52-122">Scroll down too**Operations** and then click **Import/Export** history.</span></span>

### <a name="monitor-hello-progress-of-an-import-operation"></a><span data-ttu-id="56c52-123">Az importálási művelet hello állapotának figyelése</span><span class="sxs-lookup"><span data-stu-id="56c52-123">Monitor hello progress of an import operation</span></span>

<span data-ttu-id="56c52-124">hello toomonitor hello előrehaladását az importálási művelet, hello logikai kiszolgáló mely hello az adatbázis importált importált hello lap megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="56c52-124">toomonitor hello progress of hello import operation, open hello page for hello logical server into which hello database is being imported imported.</span></span> <span data-ttu-id="56c52-125">Görgessen lefelé, túl**műveletek** , majd **Import/Export** előzményeit.</span><span class="sxs-lookup"><span data-stu-id="56c52-125">Scroll down too**Operations** and then click **Import/Export** history.</span></span>
   
   <span data-ttu-id="56c52-126">![Importálás](./media/sql-database-import/import-history.png) ![importálás állapota](./media/sql-database-import/import-status.png)</span><span class="sxs-lookup"><span data-stu-id="56c52-126">![import](./media/sql-database-import/import-history.png) ![import status](./media/sql-database-import/import-status.png)</span></span>

<span data-ttu-id="56c52-127">tooverify hello adatbázis élő hello kiszolgálón, kattintson a **SQL-adatbázisok** és ellenőrizze, hogy új adatbázist hello **Online**.</span><span class="sxs-lookup"><span data-stu-id="56c52-127">tooverify hello database is live on hello server, click **SQL databases** and verify hello new database is **Online**.</span></span>

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a><span data-ttu-id="56c52-128">SQLPackage használatával BACPAC-fájlból való importálása</span><span class="sxs-lookup"><span data-stu-id="56c52-128">Import from a BACPAC file using SQLPackage</span></span>

<span data-ttu-id="56c52-129">tooimport egy SQL-adatbázis használatával hello [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) parancssori segédprogram, lásd: [importálja a paraméterek és a Tulajdonságok](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties).</span><span class="sxs-lookup"><span data-stu-id="56c52-129">tooimport a SQL database using hello [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) command-line utility, see [Import parameters and properties](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties).</span></span> <span data-ttu-id="56c52-130">hello SQLPackage segédprogram hello legfrissebb változatának részét képező [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) és [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), vagy letöltheti a legújabb verziót hello [ SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) közvetlenül a hello Microsoft letöltőközpontból.</span><span class="sxs-lookup"><span data-stu-id="56c52-130">hello SQLPackage utility ships with hello latest versions of [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) and [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), or you can download hello latest version of [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) directly from hello Microsoft download center.</span></span>

<span data-ttu-id="56c52-131">A méretezés és teljesítmény a legtöbb éles környezetben hello hello SQLPackage segédprogram használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="56c52-131">We recommend hello use of hello SQLPackage utility for scale and performance in most production environments.</span></span> <span data-ttu-id="56c52-132">Egy SQL Server Ügyféltanácsadói csapatának blogja áttelepítéssel kapcsolatos BACPAC-fájlokkal, lásd: [áttelepítése az SQL Server tooAzure SQL-adatbázis BACPAC-fájlokat használ](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="56c52-132">For a SQL Server Customer Advisory Team blog about migrating using BACPAC files, see [Migrating from SQL Server tooAzure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>

<span data-ttu-id="56c52-133">Tekintse meg a következő SQLPackage parancs olyan parancsfájl például arról, hogyan hello tooimport hello **AdventureWorks2008R2** helyi tároló tooan Azure SQL Database logikai kiszolgáló, nevű adatbázis **mynewserver20170403** ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="56c52-133">See hello following SQLPackage command for a script example for how tooimport hello **AdventureWorks2008R2** database from local storage tooan Azure SQL Database logical server, called **mynewserver20170403** in this example.</span></span> <span data-ttu-id="56c52-134">A parancsfájl jeleníti meg az új adatbázis létrehozása hello **myMigratedDatabase**, az egy szolgáltatási szint **prémium**, és a szolgáltatás célja **P6**.</span><span class="sxs-lookup"><span data-stu-id="56c52-134">This script shows hello creation of a new database called **myMigratedDatabase**, with a service tier of **Premium**, and a Service Objective of **P6**.</span></span> <span data-ttu-id="56c52-135">Megfelelő tooyour környezet módosítani ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="56c52-135">Change these values as appropriate tooyour environment.</span></span>

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

   ![sqlpackage importálása](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> <span data-ttu-id="56c52-137">Egy Azure SQL Database logikai kiszolgáló figyel az 1433-as porton.</span><span class="sxs-lookup"><span data-stu-id="56c52-137">An Azure SQL Database logical server listens on port 1433.</span></span> <span data-ttu-id="56c52-138">Próbált tooconnect tooan Azure SQL Database logikai kiszolgáló a vállalati tűzfalon belül, ha ezt a portot kell megnyitni hello vállalati tűzfal az Ön toosuccessfully csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="56c52-138">If you are attempting tooconnect tooan Azure SQL Database logical server from within a corporate firewall, this port must be open in hello corporate firewall for you toosuccessfully connect.</span></span>
>

<span data-ttu-id="56c52-139">A példa bemutatja, hogyan tooimport egy adatbázist az Active Directory univerzális hitelesítéssel SqlPackage.exe használatával:</span><span class="sxs-lookup"><span data-stu-id="56c52-139">This example shows how tooimport a database using SqlPackage.exe with Active Directory Universal Authentication:</span></span>

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a><span data-ttu-id="56c52-140">PowerShell-lel BACPAC-fájlból való importálása</span><span class="sxs-lookup"><span data-stu-id="56c52-140">Import from a BACPAC file using PowerShell</span></span>

<span data-ttu-id="56c52-141">Használjon hello [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) parancsmag toosubmit egy importálási adatbázis kérelem toohello Azure SQL Database szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="56c52-141">Use hello [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet toosubmit an import database request toohello Azure SQL Database service.</span></span> <span data-ttu-id="56c52-142">Attól függően, hogy az adatbázis méretét hello hello importálási művelet eltarthat néhány alkalommal toocomplete.</span><span class="sxs-lookup"><span data-stu-id="56c52-142">Depending on hello size of your database, hello import operation may take some time toocomplete.</span></span>

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

<span data-ttu-id="56c52-143">hello toocheck hello állapotának-importálási kérelem, használja a hello [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="56c52-143">toocheck hello status of hello import request, use hello [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet.</span></span> <span data-ttu-id="56c52-144">Ez közvetlenül a hello után fut kérelmek általában értéket ad vissza **állapota: esetbejegyzések**.</span><span class="sxs-lookup"><span data-stu-id="56c52-144">Running this immediately after hello request usually returns **Status: InProgress**.</span></span> <span data-ttu-id="56c52-145">Amikor látja **állapota: sikeres** hello importálás befejeződött.</span><span class="sxs-lookup"><span data-stu-id="56c52-145">When you see **Status: Succeeded** hello import is complete.</span></span>

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
<span data-ttu-id="56c52-146">Egy másik mintaparancsfájl, lásd: [adatbázis BACPAC-fájlból való importálása](scripts/sql-database-import-from-bacpac-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="56c52-146">For another script example, see [Import a database from a BACPAC file](scripts/sql-database-import-from-bacpac-powershell.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="56c52-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="56c52-147">Next steps</span></span>
* <span data-ttu-id="56c52-148">toolearn hogyan tooconnect tooand lekérdezni egy importált SQL-adatbázis, lásd: [tooSQL adatbázis csatlakozzon az SQL Server Management Studio eszközt, és minta T-SQL-lekérdezést végrehajtani a](sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="56c52-148">toolearn how tooconnect tooand query an imported SQL Database, see [Connect tooSQL Database with SQL Server Management Studio and perform a sample T-SQL query](sql-database-connect-query-ssms.md).</span></span>
* <span data-ttu-id="56c52-149">Egy SQL Server Ügyféltanácsadói csapatának blogja áttelepítéssel kapcsolatos BACPAC-fájlokkal, lásd: [áttelepítése az SQL Server tooAzure SQL-adatbázis BACPAC-fájlokat használ](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="56c52-149">For a SQL Server Customer Advisory Team blog about migrating using BACPAC files, see [Migrating from SQL Server tooAzure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>
* <span data-ttu-id="56c52-150">Hello teljes SQL Server adatbázis áttelepítési folyamat, többek között teljesítmény javaslatok leírását lásd: [egy SQL Server adatbázis tooAzure SQL-adatbázis áttelepítése](sql-database-cloud-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="56c52-150">For a discussion of hello entire SQL Server database migration process, including performance recommendations, see [Migrate a SQL Server database tooAzure SQL Database](sql-database-cloud-migrate.md).</span></span>



