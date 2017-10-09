---
title: "a Blob Storage tooSQL adatbázis - Azure aaaCopy adatait |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan toouse másolási tevékenység során az Azure Data Factory a csővezeték-e a Blob storage tooSQL adatbázis toocopy adatait."
keywords: "a BLOB sql, a blob storage adatok másolása"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e4035060-93bf-4e8d-bf35-35e2d15c51e0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: a2c3fb8a4ddd63b0b6b3e75903b7a7eaf188fda4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-copy-data-from-blob-storage-toosql-database-using-data-factory"></a>Oktatóanyag: Adatok másolása az Blob Storage tooSQL adatbázist Data Factory használatával
> [!div class="op_single_selector"]
> * [Áttekintés és előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Másolás varázsló](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-sablon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

Ebben az oktatóanyagban létrehoz egy adat-előállító egy Blob storage tooSQL adatbázisból adatcsatorna toocopy adatokkal.

hello másolási tevékenység hello adatmozgás Azure Data Factory hajt végre. Egy olyan, globálisan elérhető szolgáltatás működteti, amely biztonságos, megbízható és méretezhető módon másolja át az adatokat a különböző adattárak között. Lásd: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) szóló cikkben olvashat hello másolási tevékenység.  

> [!NOTE]
> A Data Factory szolgáltatásnak hello részletes áttekintése, lásd: hello [Data Factory bemutatása tooAzure](data-factory-introduction.md) cikk.
>
>

## <a name="prerequisites-for-hello-tutorial"></a>Hello oktatóanyag előfeltételei
Ez az oktatóanyag elkezdéséhez a következő előfeltételek hello kell rendelkeznie:

* **Azure-előfizetés**.  Ha nem rendelkezik előfizetéssel, mindössze néhány perc alatt létrehozhat egy ingyenes próbafiókot. Lásd: hello [ingyenes](http://azure.microsoft.com/pricing/free-trial/) cikkben alább.
* **Azure Storage-fiók**. Hello blob Storage tárolót használja a **forrás** ebben az oktatóanyagban az adattároló. Ha egy Azure storage-fiók nem rendelkezik, tekintse meg a hello [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) lépéseket toocreate egyet a cikkben találhat.
* **Azure SQL Database** Azure SQL-adatbázis, használja a **cél** ebben az oktatóanyagban az adattároló. Ha még nem rendelkezik, amelyek segítségével is hello oktatóanyag, lásd: Azure SQL-adatbázis [hogyan toocreate és konfigurálása az Azure SQL Database](../sql-database/sql-database-get-started.md) toocreate egyet.
* **Az SQL Server 2012 vagy 2014 vagy a Visual Studio 2013**. SQL Server Management Studio vagy Visual Studio toocreate mintaadatbázis és tooview hello eredményadatokat hello adatbázis használható.  

## <a name="collect-blob-storage-account-name-and-key"></a>A blob storage fióknevet és kulcsot gyűjtése
Név és fiókkulcs kulcsát az Azure storage-fiók toodo ebben az oktatóanyagban hello fiók szükséges. Jegyezze fel **fióknév** és **fiókkulcs** az Azure-tárfiókot.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **további szolgáltatások** hello bal oldali menüben, és válassza ki a **Tárfiókok**.

    ![Tallózás - Storage-fiókok](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/browse-storage-accounts.png)
3. A hello **Tárfiókok** panelen, jelölje be hello **Azure storage-fiók** megjeleníteni kívánt toouse ebben az oktatóanyagban.
4. Válassza ki **hívóbetűk** hivatkozásra **beállítások**.
5. Kattintson a **másolási** (kép) gomb melletti túl**tárfióknév** szöveg mezőbe, a Mentés és illessze be azt valahol (például: a fájlt).
6. Ismételje meg a hello előző lépés toocopy vagy jegyezze fel a hello **key1**.

    ![Tárelérési kulcs](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/storage-access-key.png)
7. Zárja be az összes hello paneleken kattintva **X**.

## <a name="collect-sql-server-database-user-names"></a>SQL server, az adatbázis, a felhasználói neveket gyűjtése
Ez az oktatóanyag szüksége van Azure SQL server, az adatbázis és a felhasználó toodo hello nevét. Jegyezze fel a neveket **server**, **adatbázis**, és **felhasználói** az Azure SQL adatbázishoz.

1. A hello **Azure-portálon**, kattintson a **további szolgáltatások** hello bal, és válassza a **SQL-adatbázisok**.
2. A hello **SQL adatbázisok panel**, jelölje be hello **adatbázis** megjeleníteni kívánt toouse ebben az oktatóanyagban. Jegyezze fel a hello **adatbázisnév**.  
3. A hello **SQL-adatbázis** panelen kattintson a **tulajdonságok** alatt **beállítások**.
4. Jegyezze fel a hello értékek **kiszolgálónév** és **kiszolgáló-rendszergazdai bejelentkezés**.
5. Zárja be az összes hello paneleken kattintva **X**.

## <a name="allow-azure-services-tooaccess-sql-server"></a>Azure-szolgáltatások tooaccess SQL server engedélyezése
Győződjön meg arról, hogy **hozzáférést tooAzure szolgáltatások** beállítás kikapcsolt **ON** az Azure SQL-kiszolgáló, a Data Factory szolgáltatásnak hello férhetnek hozzá az Azure SQL server. tooverify, és kapcsolja be ezt a beállítást, a következő lépéseket hello:

1. Kattintson a **további szolgáltatások** elemre, majd hello balra hub **SQL Server-kiszolgálók**.
2. Válassza ki a kiszolgálót, és kattintson a **BEÁLLÍTÁSOK** területen a **Tűzfal** elemre.
3. A hello **tűzfalbeállítások** panelen kattintson a **ON** a **hozzáférést tooAzure szolgáltatások**.
4. Zárja be az összes hello paneleken kattintva **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>A Blob Storage és az SQL-adatbázis előkészítése
Most, az Azure blob storage és előkészítése Azure SQL adatbázis hello oktatóanyag hello lépések végrehajtásával:  

1. Indítsa el a Jegyzettömböt. A következő szöveg hello másolja, majd mentse a fájt **emp.txt** túl**C:\ADFGetStarted** mappa a merevlemezen.

    ```
    John, Doe
    Jane, Doe
    ```
2. Használjon például az eszközök [Azure Tártallózó](http://storageexplorer.com/) toocreate hello **adftutorial** tárolóhoz és annak tooupload hello **emp.txt** fájl toohello tárolót.

    ![Az Azure Storage Explorer. Másolja az adatokat a Blob storage tooSQL adatbázisból](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. A következő SQL-parancsfájl toocreate hello használata hello **üres** az Azure SQL-adatbázis táblájában.  

    ```SQL
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

    **Ha az SQL Server 2012 vagy 2014 a számítógépen telepítve van:** kövesse az utasításokat [kezelése az Azure SQL Database SQL Server Management Studio használatával](../sql-database/sql-database-manage-azure-ssms.md) tooconnect tooyour Azure SQL server és a Futtatás hello SQL-parancsfájlt. Ebben a cikkben az hello [klasszikus Azure portálon](http://manage.windowsazure.com), nem hello [új Azure-portálon](https://portal.azure.com), tooconfigure tűzfal Azure SQL-kiszolgáló.

    Ha az ügyfél számára nem engedélyezett tooaccess hello Azure SQL-kiszolgálót, meg kell tooconfigure tűzfal az Azure SQL server tooallow hozzáférés a számítógépről (IP-cím). Lásd: [Ez a cikk](../sql-database/sql-database-configure-firewall-settings.md) lépéseket tooconfigure hello tűzfal az Azure SQL-kiszolgáló.

## <a name="create-a-data-factory"></a>Data factory létrehozása
Hello Előfeltételek befejeződött. Egy adat-előállító valamelyik a következő módokon hello segítségével hozhat létre. Kattintson a hello legördülő lista hello tetejéhez vagy a következő hivatkozások tooperform hello oktatóanyag hello hello lehetőségek közül.     

* [Másolás varázsló](data-factory-copy-data-wizard-tutorial.md)
* [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
* [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
* [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
* [Azure Resource Manager-sablon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
* [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat. Nem alakítja át a bemeneti adatok tooproduce kimeneti adatokat. Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: az első adatcsatorna tootransform adatok Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).
> 
> Hello kimeneti adatkészlet egy tevékenység beállítását hello bemeneti hello az adatkészlet többi tevékenység által láncolt lehessen két tevékenység (egy tevékenység egymás után). Lásd [a Data Factorybeli ütemezést és végrehajtást](data-factory-scheduling-and-execution.md) ismertető cikket. 
