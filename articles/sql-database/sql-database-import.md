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
# <a name="import-a-bacpac-file-tooa-new-azure-sql-database"></a>Importálja a BACPAC fájl tooa új Azure SQL-adatbázis

Ha tooimport archívumból adatbázis van szüksége, vagy egy másik platformról áttelepítésekor hello adatbázisséma és adatainak áttelepítését importálni egy [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fájlt. Egy BACPAC egy ZIP-fájlt tartalmazó hello metaadatok és adatainak áttelepítését egy SQL Server-adatbázis BACPAC kiterjesztésű fájl. Egy BACPAC fájl importálhatók az Azure blob storage (csak a standard tárolási) vagy egy helyszíni hely a helyi tárolóból. Adjon meg egy magasabb és teljesítményszintet szolgáltatásszintet, például egy P6, és szükség szerint toodown majd méretezhető, hello importálási befejezését követően ajánlott toomaximize hello importálási sebessége. Hello adatbázis kompatibilitási szintje hello importálás után is, hello source adatbázis kompatibilitási szintje hello alapul. 

> [!IMPORTANT] 
> Az adatbázis tooAzure SQL-adatbázis az áttelepítés után dönthet úgy, hogy a jelenlegi kompatibilitási szinten (100. szint hello AdventureWorks2008R2 adatbázis) vagy magasabb szintű toooperate hello adatbázis ki. Hello hatással vannak, és egy adatbázis kompatibilitási szintű működő beállításainak további információkért lásd: [adatbázis kompatibilitási szintje ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Lásd még: [adatbázis HATÓKÖRŰ konfiguráció ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) további adatbázis-szintű beállítással kapcsolatos információkat a vonatkozó toocompatibility szintjének.   >

> [!NOTE]
> tooimport BACPAC tooa új adatbázist, akkor először létre kell hoznia egy Azure SQL Database logikai kiszolgálóhoz. Egy oktatóanyag, hogy bemutatja, hogyan toomigrate egy SQL Server adatbázis-tooAzure SQL-adatbázis használata SQLPackage, lásd: [SQL Server-adatbázis áttelepítése](sql-database-migrate-your-sql-server-database.md)
>

## <a name="import-from-a-bacpac-file-using-azure-portal"></a>Azure-portál használatával BACPAC-fájlból való importálása

Ez a cikk ismerteti az Azure SQL-adatbázis hello használata az Azure blob storage-ban tárolt BACPAC-fájlból való létrehozására vonatkozó utasításokat [Azure-portálon](https://portal.azure.com). Hello csak az Azure portál használatával importálhatók BACPAC fájlok importálása az Azure blob storage.

egy adatbázis használatával tooimport hello Azure-portálon, az adatbázisról, és kattintson a Megnyitás hello lap **importálási** hello eszköztáron. Adja meg a hello tárfiók és tároló, és válassza ki a kívánt tooimport hello BACPAC-fájl. Válassza ki az új adatbázis hello hello méretet (általában hello ugyanaz, mint a forrás) és hello cél SQL Server hitelesítő adatok megadásához.  

   ![Adatbázis importálása](./media/sql-database-import/import.png)

hello toomonitor hello előrehaladását az importálási művelet, hello logikai kiszolgáló tartalmazó hello adatbázisának importált hello lap megnyitásához. Görgessen lefelé, túl**műveletek** , majd **Import/Export** előzményeit.

### <a name="monitor-hello-progress-of-an-import-operation"></a>Az importálási művelet hello állapotának figyelése

hello toomonitor hello előrehaladását az importálási művelet, hello logikai kiszolgáló mely hello az adatbázis importált importált hello lap megnyitásához. Görgessen lefelé, túl**műveletek** , majd **Import/Export** előzményeit.
   
   ![Importálás](./media/sql-database-import/import-history.png) ![importálás állapota](./media/sql-database-import/import-status.png)

tooverify hello adatbázis élő hello kiszolgálón, kattintson a **SQL-adatbázisok** és ellenőrizze, hogy új adatbázist hello **Online**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>SQLPackage használatával BACPAC-fájlból való importálása

tooimport egy SQL-adatbázis használatával hello [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) parancssori segédprogram, lásd: [importálja a paraméterek és a Tulajdonságok](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties). hello SQLPackage segédprogram hello legfrissebb változatának részét képező [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) és [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), vagy letöltheti a legújabb verziót hello [ SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) közvetlenül a hello Microsoft letöltőközpontból.

A méretezés és teljesítmény a legtöbb éles környezetben hello hello SQLPackage segédprogram használatát javasoljuk. Egy SQL Server Ügyféltanácsadói csapatának blogja áttelepítéssel kapcsolatos BACPAC-fájlokkal, lásd: [áttelepítése az SQL Server tooAzure SQL-adatbázis BACPAC-fájlokat használ](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Tekintse meg a következő SQLPackage parancs olyan parancsfájl például arról, hogyan hello tooimport hello **AdventureWorks2008R2** helyi tároló tooan Azure SQL Database logikai kiszolgáló, nevű adatbázis **mynewserver20170403** ebben a példában. A parancsfájl jeleníti meg az új adatbázis létrehozása hello **myMigratedDatabase**, az egy szolgáltatási szint **prémium**, és a szolgáltatás célja **P6**. Megfelelő tooyour környezet módosítani ezeket az értékeket.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

   ![sqlpackage importálása](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> Egy Azure SQL Database logikai kiszolgáló figyel az 1433-as porton. Próbált tooconnect tooan Azure SQL Database logikai kiszolgáló a vállalati tűzfalon belül, ha ezt a portot kell megnyitni hello vállalati tűzfal az Ön toosuccessfully csatlakozzon.
>

A példa bemutatja, hogyan tooimport egy adatbázist az Active Directory univerzális hitelesítéssel SqlPackage.exe használatával:

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>PowerShell-lel BACPAC-fájlból való importálása

Használjon hello [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) parancsmag toosubmit egy importálási adatbázis kérelem toohello Azure SQL Database szolgáltatásban. Attól függően, hogy az adatbázis méretét hello hello importálási művelet eltarthat néhány alkalommal toocomplete.

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

hello toocheck hello állapotának-importálási kérelem, használja a hello [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) parancsmag. Ez közvetlenül a hello után fut kérelmek általában értéket ad vissza **állapota: esetbejegyzések**. Amikor látja **állapota: sikeres** hello importálás befejeződött.

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
* toolearn hogyan tooconnect tooand lekérdezni egy importált SQL-adatbázis, lásd: [tooSQL adatbázis csatlakozzon az SQL Server Management Studio eszközt, és minta T-SQL-lekérdezést végrehajtani a](sql-database-connect-query-ssms.md).
* Egy SQL Server Ügyféltanácsadói csapatának blogja áttelepítéssel kapcsolatos BACPAC-fájlokkal, lásd: [áttelepítése az SQL Server tooAzure SQL-adatbázis BACPAC-fájlokat használ](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* Hello teljes SQL Server adatbázis áttelepítési folyamat, többek között teljesítmény javaslatok leírását lásd: [egy SQL Server adatbázis tooAzure SQL-adatbázis áttelepítése](sql-database-cloud-migrate.md).



