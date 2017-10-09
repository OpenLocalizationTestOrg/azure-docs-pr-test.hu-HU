---
title: "egy Azure SQL adatbázis tooa BACPAC fájl aaaExport |} Microsoft Docs"
description: "Egy Azure SQL adatbázis tooa BACPAC-fájl exportálását hello Azure portál használatával"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 41d63a97-37db-4e40-b652-77c2fd1c09b7
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/15/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: cb3b4227318e0fd2114529c86c9792615fe7fd1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="export-an-azure-sql-database-tooa-bacpac-file"></a>Azure SQL adatbázis tooa BACPAC fájl exportálása

Ha tooexport adatbázis archiválásra vagy áthelyezése tooanother platform van szükség, exportálhatja hello adatbázisban séma- és tooa [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fájlt. Egy BACPAC egy ZIP-fájlt tartalmazó hello metaadatok és adatainak áttelepítését egy SQL Server-adatbázis BACPAC kiterjesztésű fájl. Egy BACPAC-fájl az Azure blob storage vagy a helyi tároló helyszíni helyen tárolt, és később importálható vissza az Azure SQL Database vagy a helyszíni SQL Server telepítéséhez. 

> [!IMPORTANT] 
> Az Azure SQL adatbázis automatikus exportálása 2017. március 1. a lett visszavonva. Használhat [hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-retention.md
) vagy [Azure Automation](https://github.com/Microsoft/azure-docs-pr/blob/2461f706f8fc1150e69312098640c0676206a531/articles/automation/automation-intro.md) tooperiodically archív SQL-adatbázisok az Ön által választott tooa ütemezés szerint PowerShell-lel. Mintát, töltse le a hello [PowerShell-mintaparancsfájl](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) a Githubról.
>

## <a name="considerations-when-exporting-an-azure-sql-database"></a>Azure SQL-adatbázis exportálása szempontjai

* Az Exportálás tranzakciós úton konzisztens, ügyelnie kell arra, vagy hogy nincs írási tevékenység hello exportálás során jelentkezett, vagy adott meg toobe exportálása egy [tranzakciós úton konzisztens másolat](sql-database-copy.md) az Azure SQL adatbázis.
* Tooblob tárolási exportálása, hello maximális BACPAC-fájl mérete 200 GB-os. tooarchive egy nagyobb BACPAC-fájl exportálása toolocal tároló.
* Egy BACPAC fájl tooAzure prémium szintű storage cikkben említett hello módszerekkel exportálása nem támogatott.
* Ha az Azure SQL Database hello az exportálási művelet meghaladja a 20 órát, előfordulhat, hogy lehet megszakítani. tooincrease teljesítmény exportálás során, a következőket teheti:
  * Ideiglenesen növelje meg a szolgáltatási szint.
  * Próbálkozást az összes olvasási és írási tevékenység hello exportálás során.
  * Használja a [fürtözött index](https://msdn.microsoft.com/library/ms190457.aspx) összes nagy táblák nem null értékekkel. Fürtözött indexek nélkül exportálása meghiúsulhat, ha 6 – 12 óránál hosszabb ideig tart. Ennek az az oka hello exportálási szolgáltatásnak kell toocomplete egy vizsgálat tootry tooexport teljes tábla. Egy jó módszer toodetermine, ha a táblák vannak optimalizálva, az Exportálás toorun **DBCC SHOW_STATISTICS** és győződjön meg arról, hogy hello *RANGE_HI_KEY* értéke nem null, és az értéke megfelelő terjesztési. További információkért lásd: [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> BACPACs nincsenek tervezett toobe biztonsági mentési és visszaállítási műveletek elvégzéséhez használható. Az Azure SQL Database biztonsági mentések minden felhasználói adatbázis automatikusan létrehozza. További információkért lásd: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md) és [SQL-adatbázis biztonsági másolatait](sql-database-automated-backups.md).  
> 

## <a name="export-tooa-bacpac-file-using-hello-azure-portal"></a>Tooa BACPAC exportfájl hello Azure-portál használatával

egy adatbázis használatával tooexport hello [Azure-portálon](https://portal.azure.com), nyissa meg az adatbázis hello lapját, és kattintson a **exportálása** hello eszköztáron. Adja meg a hello BACPAC fájlnév, hello Azure-tárfiók és tároló hello exportáláshoz, és hello hitelesítő adatok tooconnect toohello forrásadatbázist.  

![Adatbázis exportálása](./media/sql-database-export/database-export.png)

hello toomonitor hello előrehaladását exportálási művelet, hello logikai kiszolgáló tartalmazó hello adatbázisának exportálás alatt álló hello lap megnyitásához. Görgessen lefelé, túl**műveletek** , majd **Import/Export** előzményeit.

![exportálja az előzmények](./media/sql-database-export/export-history.png)
![exportálás előzmények állapota](./media/sql-database-export/export-history2.png)

## <a name="export-tooa-bacpac-file-using-hello-sqlpackage-utility"></a>Hello SQLPackage segédprogrammal tooa BACPAC-fájl exportálása

tooexport egy SQL-adatbázis használatával hello [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) parancssori segédprogram, lásd: [paraméterek és a Tulajdonságok exportálása](https://msdn.microsoft.com/library/hh550080.aspx#Export Parameters and Properties). hello SQLPackage segédprogram hello legfrissebb változatának részét képező [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) és [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), vagy letöltheti a legújabb verziót hello [ SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) közvetlenül a hello Microsoft letöltőközpontból.

A méretezés és teljesítmény a legtöbb éles környezetben hello hello SQLPackage segédprogram használatát javasoljuk. Egy SQL Server Ügyféltanácsadói csapatának blogja áttelepítéssel kapcsolatos BACPAC-fájlokkal, lásd: [áttelepítése az SQL Server tooAzure SQL-adatbázis BACPAC-fájlokat használ](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

A példa bemutatja, hogyan tooexport egy adatbázist az Active Directory univerzális hitelesítéssel SqlPackage.exe használatával:

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=apptestserver.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="export-tooa-bacpac-file-using-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) használatával tooa BACPAC-fájl exportálása

SQL Server Management Studio legújabb verziói hello is adja meg a varázsló tooexport egy Azure SQL Database tooa BACPAC-fájlját. Lásd: hello [egy adatrétegbeli alkalmazás exportálása](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).

## <a name="export-tooa-bacpac-file-using-powershell"></a>PowerShell-lel tooa BACPAC-fájl exportálása

Használjon hello [New-AzureRmSqlDatabaseExport](/powershell/module/azurerm.sql/new-azurermsqldatabaseexport) parancsmag toosubmit az Exportálás adatbázis kérelem toohello Azure SQL Database szolgáltatásban. Attól függően, hogy az adatbázis méretét hello hello az exportálási művelet eltarthat néhány alkalommal toocomplete.

 ```powershell
 $exportRequest = New-AzureRmSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
   -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
   -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
 ```

hello toocheck hello állapotának-exportálási kérelem, használja a hello [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) parancsmag. Ez közvetlenül a hello után fut kérelmek általában értéket ad vissza **állapota: esetbejegyzések**. Amikor látja **állapota: sikeres** hello exportálása befejeződött.

```powershell
$exportStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
[Console]::Write("Exporting")
while ($exportStatus.Status -eq "InProgress")
{
    $exportStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$exportStatus
```

## <a name="next-steps"></a>Következő lépések

* egy Azure SQL-adatbázis biztonsági másolatának egy alternatív tooexported archív célokra, adatbázis, a hosszú távú biztonsági másolatok megőrzésének kapcsolatos toolearn lásd [hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-retention.md).
- Egy SQL Server Ügyféltanácsadói csapatának blogja áttelepítéssel kapcsolatos BACPAC-fájlokkal, lásd: [áttelepítése az SQL Server tooAzure SQL-adatbázis BACPAC-fájlokat használ](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* Lásd az SQL Server adatbázis BACPAC tooa, importálás toolearn [BACPCAC tooa SQL Server adatbázis importálása](https://msdn.microsoft.com/library/hh710052.aspx).
* egy SQL Server-adatbázisból egy BACPAC exportálása toolearn lásd: [egy adatrétegbeli alkalmazás exportálása](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) és [telepítse át az első adatbázist](sql-database-migrate-your-sql-server-database.md).
* Ha az SQL Server egy prelude toomigration tooAzure SQL-adatbázis szerint exportálja, lásd: [egy SQL Server adatbázis tooAzure SQL-adatbázis áttelepítése](sql-database-cloud-migrate.md).
