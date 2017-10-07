---
title: "Oktatóanyag: Azure PowerShell használatával hozzon létre egy folyamat toomove adatok |} Microsoft Docs"
description: "Az oktatóanyag során létrehoz egy Azure Data Factory-folyamatot másolási tevékenységgel az Azure PowerShell használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 71087349-9365-4e95-9847-170658216ed8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 21406d7dfaa0c555b2538fbb52839d761c140fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-data-factory-pipeline-that-moves-data-by-using-azure-powershell"></a>Oktatóanyag: Data Factory-folyamat létrehozása adatok áthelyezéséhez az Azure PowerShell használatával
> [!div class="op_single_selector"]
> * [Áttekintés és előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Másolás varázsló](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-sablon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
>
>

Ebből a cikkből megismerheti, hogyan toouse PowerShell toocreate egy adat-előállítót, és egy folyamatot, amely másol adatokat az Azure blob storage tooan Azure SQL-adatbázis. Ha új tooAzure adat-előállítót, olvassa végig hello [Data Factory bemutatása tooAzure](data-factory-introduction.md) cikk Ez az oktatóanyag elvégzése előtt.   

Az oktatóanyag segítségével egyetlen tevékenységgel (másolási tevékenységgel) rendelkező folyamatot hozhat létre. hello másolási tevékenység során a támogatott adatokat tároló tooa támogatott fogadó adatokat tároló másol adatokat. A forrásként és fogadóként támogatott adattárak listájáért lásd: [támogatott adattárak](data-factory-data-movement-activities.md#supported-data-stores-and-formats). hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva. További információ a másolási tevékenység hello: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).

Egy folyamathoz több tevékenység is tartozhat. És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását. További információért lásd: [egy folyamaton belüli több tevékenység](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

> [!NOTE]
> Ez a cikk nem foglalkozik minden hello Data Factory-parancsmag. A parancsmagokkal kapcsolatos átfogó dokumentációért tekintse meg a [Data Factory-parancsmagok referenciáját](/powershell/module/azurerm.datafactories).
> 
> hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat. Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: a folyamat tootransform adatokat Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Előfeltételek
- Végezze el a témakörben ismertetett előfeltételeknek: hello [oktatóanyag előfeltételei](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) cikk.
- Telepítse az **Azure PowerShellt**. Hello utasításait követve [hogyan tooinstall és konfigurálja az Azure Powershellt](../powershell-install-configure.md).

## <a name="steps"></a>Lépések
Ez az oktatóanyag részeként végrehajtandó hello lépések a következők:

1. Hozzon létre egy folyamatot az **adat-előállítóban**. Ebben a lépésben egy adat-előállítót hoz létre ADFTutorialDataFactoryPSH néven. 
2. Hozzon létre **összekapcsolt szolgáltatások** hello adat-előállítóban. Ebben a lépésben a következő két típusú társított szolgáltatást hozza létre: Azure Storage és Azure SQL Database. 
    
    hello AzureStorageLinkedService művelet ugyan összeköti a az Azure storage-fiók toohello adat-előállítóban. Létrejött a tároló és részeként feltöltött adatok toothis tárfiók [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

    AzureSqlLinkedService az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz. hello blobtárolóból másolt hello adatok az adatbázis tárolja. Az [előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részeként létrehozta az SQL-táblát az adatbázisban.   
3. Hozzon létre a bemeneti és kimeneti **adatkészletek** hello adat-előállítóban.  
    
    hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot. És hello bemeneti blob-adathalmazra hello tároló és hello bemeneti adatokat tartalmazó hello mappát adja meg.  

    Hello csatolt Azure SQL Database szolgáltatáshoz hasonlóan hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg. És hello kimeneti SQL táblázat dataset megadja hello adatbázis toowhich hello adatokat a hello blob storage hello táblájában másolódik.
4. Hozzon létre egy **csővezeték** hello adat-előállítóban. Ebben a lépésben létre fog hozni egy másolási tevékenységgel rendelkező folyamatot.   
    
    hello másolási tevékenység hello Azure blob storage tooa tábla hello Azure SQL-adatbázis egy blobot másol adatokat. A másolási tevékenység használható egy folyamat toocopy adatokat bármely támogatott forrás támogatott tooany cél. A támogatott adattárak listájáért lásd: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md#supported-data-stores-and-formats). 
5. A figyelő hello folyamat. Ebben a lépésben meg **figyelő** hello szeletek bemeneti és kimeneti adatkészletek a PowerShell használatával.

## <a name="create-a-data-factory"></a>Data factory létrehozása
> [!IMPORTANT]
> Teljes [hello oktatóanyag előfeltételei](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) Ha még nem tette meg.   

A data factory egy vagy több folyamattal rendelkezhet. A folyamaton belül egy vagy több tevékenység lehet. Adja meg például a másolási tevékenység toocopy forrás tooa cél adattárat és adatait egy HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform adatok tooproduct kimeneti adatokat. Kezdjük, az ebben a lépésben hello adat-előállító létrehozása.

1. Indítsa el a **PowerShellt**. Azure PowerShell nyitva hagyja az oktatóanyag hello végéig. Zárja be és nyissa meg újra, ha újra kell toorun hello parancsok.

    Futtassa a következő parancs hello, és írja be a hello felhasználónevet és jelszót toosign használhatja a toohello Azure-portálon:

    ```PowerShell
    Login-AzureRmAccount
    ```   
   
    Futtassa a következő parancs tooview hello minden hello előfizetés ehhez a fiókhoz:

    ```PowerShell
    Get-AzureRmSubscription
    ```

    Futtassa a következő parancs tooselect hello előfizetést, amelyet a toowork hello. Cserélje le  **&lt;NameOfAzureSubscription** &gt; hello nevet, az Azure-előfizetéséhez:

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
2. Hozzon létre egy Azure erőforráscsoport nevű **ADFTutorialResourceGroup** hello a következő parancs futtatásával:

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    
    Egyes ebben az oktatóanyagban hello lépések azt feltételezik, hogy hello erőforráscsoportot használni **ADFTutorialResourceGroup**. Ha egy másik erőforráscsoportban található használatához szüksége van-e toouse helyett, ebben az oktatóanyagban ADFTutorialResourceGroup azt.
3. Futtassa a hello **New-AzureRmDataFactory** parancsmag toocreate nevű adat-előállító **ADFTutorialDataFactoryPSH**:  

    ```PowerShell
    $df=New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"
    ```
    Előfordulhat, hogy ez a név már foglalt. Ezért egyedivé hello hello adat-előállító nevét egy előtag vagy utótag hozzáadásával (például: ADFTutorialDataFactoryPSH05152017), és futtassa újra a hello parancsot.  

Vegye figyelembe a következő pontok hello:

* az Azure data factory hello hello nevének globálisan egyedi kell lennie. Ha hiba történt a következő hello, módosítsa a hello nevét (például yournameADFTutorialDataFactoryPSH). Használja ezt az ADFTutorialFactoryPSH helyett az oktatóanyag lépéseinek végrehajtása során. A Data Factory-összetevők részleteit a [Data Factory elnevezési szabályait](data-factory-naming-rules.md) ismertető témakörben találja.

    ```
    Data factory name “ADFTutorialDataFactoryPSH” is not available
    ```
* toocreate adat-előállító példányok, közreműködő vagy hello Azure-előfizetés rendszergazdájának kell lennie.
* hello adat-előállító nevét hello előfordulhat, hogy a jövőbeni hello DNS-névként regisztrált, és így nyilvánosan láthatóvá válhat.
* Hello a következő hiba jelenhet meg: "**ehhez az előfizetéshez nincs regisztrált toouse névtér Microsoft.DataFactory.**" Hello alábbi, és próbálja meg újra közzétenni:

  * Az Azure PowerShell futtassa a következő parancs tooregister hello adat-előállító szolgáltató hello:

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

    Futtassa a következő parancs tooconfirm hello adott hello Data Factory szolgáltató regisztrálva van:

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Jelentkezzen be a hello Azure-előfizetés toohello [Azure-portálon](https://portal.azure.com). Nyissa meg tooa adat-előállító panelen, vagy hozzon létre egy adat-előállító hello Azure-portálon. Ez a művelet automatikusan regisztrálja az Ön hello szolgáltató.

## <a name="create-linked-services"></a>Társított szolgáltatások létrehozása
A data factory toolink adatait tárolja, és számítási szolgáltatások toohello adat-előállító létrehozása társított szolgáltatások. Ebben az oktatóanyagban nem használunk számítási szolgáltatásokat (például Azure HDInsight vagy Azure Data Lake Analytics). Csak kétféle típusú adattárat használunk: Azure Storage (forrás) és Azure SQL Database (cél). 

Ezért két társított szolgáltatást fog létrehozni AzureStorageLinkedService és AzureSqlLinkedService néven (típus: AzureStorage és AzureSqlDatabase).  

hello AzureStorageLinkedService művelet ugyan összeköti a az Azure storage-fiók toohello adat-előállítóban. Ez a tárfiók egy hello akkor létrejött a tároló és hello adatok részeként feltöltött [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

AzureSqlLinkedService az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz. hello blobtárolóból másolt hello adatok az adatbázis tárolja. Hello üres tábla az adatbázisban részeként létrehozott [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

### <a name="create-a-linked-service-for-an-azure-storage-account"></a>Társított szolgáltatás létrehozása Azure Storage-fiókhoz
Ebben a lépésben a az Azure storage-fiók tooyour adat-előállító hivatkozásra.

1. Hozzon létre egy JSON fájlt **AzureStorageLinkedService.json** a **C:\ADFGetStartedPSH** hello tartalom a következő mappában: (hello mappa létrehozása ADFGetStartedPSH, ha még nem létezik.)

    > [!IMPORTANT]
    > Cserélje le &lt;accountname&gt; és &lt;accountkey&gt; nevű és hello fájl mentése előtt az Azure storage-fiók kulcsát. 

    ```json
    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
     }
    ``` 
2. A **Azure PowerShell**, toohello kapcsoló **ADFGetStartedPSH** mappa.
4. Futtassa a hello **New-AzureRmDataFactoryLinkedService** parancsmag toocreate hello társított szolgáltatás: **AzureStorageLinkedService**. Ez a parancsmag és egyéb adat-előállító parancsmagok ebben az oktatóanyagban használata akkor toopass paraméterértékek szükségesek hello **ResourceGroupName** és **DataFactoryName** paraméterek. Másik lehetőségként át ResourceGroupName és DataFactoryName parancsmag minden futtatásakor beírása nélkül hello New-AzureRmDataFactory parancsmag által visszaadott hello DataFactory objektum. 

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureStorageLinkedService.json
    ```
    Itt egy hello minta kimenet:

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ``` 

    Más szolgáltatásnak létrehozásának módja a toospecify erőforráscsoport-név és az adat-előállító hello DataFactory objektum megadása helyett.  

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName <Name of your data factory> -File .\AzureStorageLinkedService.json
    ```

### <a name="create-a-linked-service-for-an-azure-sql-database"></a>Társított szolgáltatás létrehozása Azure SQL-adatbázishoz
Ebben a lépésben az Azure SQL adatbázis tooyour adat-előállító hivatkozásra.

1. A következő hello C:\ADFGetStartedPSH mappában AzureSqlLinkedService.json nevű JSON-fájl tartalmának létrehozása:

    > [!IMPORTANT]
    > A &lt;servername&gt;, &lt;databasename&gt;, &lt;username@servername&gt; és &lt;password&gt; paraméterek értékét cserélje le az Azure SQL-kiszolgáló, az adatbázis és a felhasználói fiók nevére, valamint a jelszóra.
    
    ```json
    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<server>.database.windows.net,1433;Database=<databasename>;User ID=<user>@<server>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
     }
    ```
2. Futtassa a következő parancs toocreate hello összekapcsolt szolgáltatás:

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json
    ```
    
    Itt egy hello minta kimenet:

    ```
    LinkedServiceName : AzureSqlLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ```

   Ellenőrizze, hogy **hozzáférést tooAzure szolgáltatások** beállítás engedélyezve van az SQL adatbázis-kiszolgáló. tooverify, és kapcsolja be, hello a következő lépéseket:

    1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com)
    2. Kattintson a **további szolgáltatások >** hello balra, majd kattintson a **SQL Server-kiszolgálók** a hello **ADATBÁZISOK** kategóriát.
    3. Az SQL-kiszolgálók hello listában jelölje ki a kiszolgálót.
    4. Hello SQL server paneljén kattintson **tűzfal beállításainak megjelenítése** hivatkozásra.
    5. A hello **tűzfalbeállítások** panelen kattintson a **ON** a **hozzáférést tooAzure szolgáltatások**.
    6. Kattintson a **mentése** hello eszköztáron. 

## <a name="create-datasets"></a>Adatkészletek létrehozása
Hello előző lépésben létrehozott összekapcsolt szolgáltatások toolink az Azure Storage-fiók és az Azure SQL adatbázis tooyour adat-előállítóban. Ebben a lépésben InputDataset és OutputDataset, amelyek megfelelnek a bemeneti és kimeneti adatok AzureStorageLinkedService és AzureSqlLinkedService által hivatkozott hello adattárolókhoz tárolt nevű két adatkészletet határozza meg.

hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot. És hello bemeneti blob-adathalmazra (InputDataset) hello tároló és hello bemeneti adatokat tartalmazó hello mappát adja meg.  

Hello csatolt Azure SQL Database szolgáltatáshoz hasonlóan hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg. És hello kimeneti SQL táblázat dataset (OututDataset) hello adatbázis toowhich hello hello blobtárolóból az adatok másolásakor hello tábla határozza meg. 

### <a name="create-an-input-dataset"></a>Bemeneti adatkészlet létrehozása
Ebben a lépésben hoz létre, mely tooa fájlját (emp.txt) InputDataset nevű adatkészlete hello gyökérmappájában lévő mappának a blob-tároló (adftutorial) az Azure Storage hello AzureStorageLinkedService kapcsolódó szolgáltatás által képviselt hello. Ha nem hello fájlnév értéket (vagy hagyja ki), a hello bemeneti mappában található összes BLOB adatait is másolt toohello cél. Ebben az oktatóanyagban hello fájlnév értéket kell megadni.  

1. Hozzon létre egy JSON fájlt **InputDataset.json** a hello **C:\ADFGetStartedPSH** hello tartalom a következő mappába:

    ```json
    {
        "name": "InputDataset",
        "properties": {
            "structure": [
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "emp.txt",
                "folderPath": "adftutorial/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
     }
    ```

    hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:

    | Tulajdonság | Leírás |
    |:--- |:--- |
    | type | hello type tulajdonság beállítása túl**AzureBlob** , mert az adatok találhatók az Azure blob Storage tárolóban. |
    | linkedServiceName | Toohello hivatkozik **AzureStorageLinkedService** korábban létrehozott. |
    | folderPath | Adja meg a hello blob **tároló** és hello **mappa** , amely tartalmazza a bemeneti BLOB. Ebben az oktatóanyagban adftutorial hello blobtárolót és hello legfelső szintű mappa. | 
    | fileName | Ez a tulajdonság nem kötelező. Ha kihagyja ezt a tulajdonságot, leltárhoz hello folderPath lévő összes fájlt. Ebben az oktatóanyagban **emp.txt** hello fájlnév, hogy csak adott fájl van felvételre feldolgozásra számára megadott. |
    | formátum -> típus |hello bemeneti fájl hello szöveg formátumban van, így használjuk **szöveges**. |
    | columnDelimiter | hello bemeneti fájl hello oszlopai határolja **vesszővel karakter (`,`)**. |
    | frequency/interval | hello gyakoriságának beállítása túl**óra** és időköz értéke túl**1**, ami azt jelenti, hogy hello bemeneti szeletek érhetők el **óránkénti**. Más szóval hello Data Factory szolgáltatásnak megkeresi a bemeneti adatok óránként blob tároló hello gyökérmappájában (**adftutorial**) megadott. Hello adatainak hello folyamat kezdési és befejezési időpontokat, nem előtt vagy után ezekben az időszakokban keresi.  |
    | external | Ez a tulajdonság értéke túl**igaz** hello adatok nem jön létre, ez az adatcsatorna. Ebben az oktatóanyagban hello a bemeneti adatok nem jön létre a folyamat, így ez a tulajdonság tootrue hivatott hello emp.txt fájl van. |

    Ezekről a JSON-tulajdonságokról további tudnivalók az [Azure Blob-összekötőről](data-factory-azure-blob-connector.md#dataset-properties) szóló cikkben olvashatók.
2. Futtassa a következő parancs toocreate hello adat-előállító dataset hello.

    ```PowerShell  
    New-AzureRmDataFactoryDataset $df -File .\InputDataset.json
    ```
    Itt egy hello minta kimenet:

    ```
    DatasetName       : InputDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Availability      : Microsoft.Azure.Management.DataFactories.Common.Models.Availability
    Location          : Microsoft.Azure.Management.DataFactories.Models.AzureBlobDataset
    Policy            : Microsoft.Azure.Management.DataFactories.Common.Models.Policy
    Structure         : {FirstName, LastName}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DatasetProperties
    ProvisioningState : Succeeded
    ```

### <a name="create-an-output-dataset"></a>Kimeneti adatkészlet létrehozása
Ez a kijelző hello lépés hoz létre egy kimeneti adatkészlet nevű **OutputDataset**. Ez az adatkészlet mutat tooa SQL táblázat hello Azure SQL Database által képviselt **AzureSqlLinkedService**. 

1. Hozzon létre egy JSON fájlt **OutputDataset.json** a hello **C:\ADFGetStartedPSH** hello tartalom a következő mappában:

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "structure": [
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "emp"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

    hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:

    | Tulajdonság | Leírás |
    |:--- |:--- |
    | type | hello type tulajdonság beállítása túl**AzureSqlTable** mert adatok másolt tooa tábla Azure SQL-adatbázisban. |
    | linkedServiceName | Toohello hivatkozik **AzureSqlLinkedService** korábban létrehozott. |
    | tableName | Megadott hello **tábla** toowhich hello adatok másolását. | 
    | frequency/interval | hello gyakoriságának beállítása túl**óra** és időköz **1**, ami azt jelenti, hogy hello kimeneti szeletek előállítása **óránkénti** közötti hello folyamat kezdési és befejezési időpontokat, előtte vagy Miután ezekben az időszakokban.  |

    Három oszlop – **azonosító**, **Keresztnév**, és **Vezetéknév** – hello üres tábla hello adatbázisban. Azonosító: azonosító oszlopot, ezért meg kell, hogy csak toospecify **Keresztnév** és **Vezetéknév** itt.

    További információ ezekről a JSON-tulajdonságokról: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#dataset-properties).
2. Futtassa a következő parancs toocreate hello data factory dataset hello.

    ```PowerShell   
    New-AzureRmDataFactoryDataset $df -File .\OutputDataset.json
    ```

    Itt egy hello minta kimenet:

    ```
    DatasetName       : OutputDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Availability      : Microsoft.Azure.Management.DataFactories.Common.Models.Availability
    Location          : Microsoft.Azure.Management.DataFactories.Models.AzureSqlTableDataset
    Policy            :
    Structure         : {FirstName, LastName}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DatasetProperties
    ProvisioningState : Succeeded
    ```

## <a name="create-a-pipeline"></a>Folyamat létrehozása
Ebben a lépésben létrehoz egy **másolási tevékenységgel** rendelkező folyamatot, amely bemenetként az **InputDataset**, kimenetként pedig az **OutputDataset** adatkészletet használja.

Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezést. Ebben az oktatóanyagban a kimeneti adatkészlet óránként konfigurált tooproduce szelet. hello folyamat rendelkezik egy kezdési és befejezési időpontja, amelyek egy nap telhet el, amely 24 óra. Ezért a kimeneti adatkészlet 24 szeletek előállított hello folyamat. 


1. Hozzon létre egy JSON fájlt **ADFTutorialPipeline.json** a hello **C:\ADFGetStartedPSH** hello tartalom a következő mappába:

    ```json   
    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob tooAzure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2017-05-11T00:00:00Z",
        "end": "2017-05-12T00:00:00Z"
      }
    } 
    ```
    Vegye figyelembe a következő pontok hello:
   
    - Hello tevékenységek szakaszban csak egy tevékenység nincs amelynek **típus** értéke túl**másolási**. Hello másolási tevékenység kapcsolatos további információkért lásd: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md). A Data Factory megoldásaiban használhatja az [adatátalakítási tevékenységeket](data-factory-data-transformation-activities.md) is.
    - Adjon meg a hello tevékenység értéke túl**InputDataset** és hello tevékenység túl van-e állítva a kimeneti**OutputDataset**. 
    - A hello **typeProperties** szakaszban **BlobSource** hello forrás típusaként van megadva, és **SqlSink** hello a fogadó típusa van megadva. Források és mosdók hello másolási tevékenység által támogatott adattárolókhoz teljes listáját lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Hogyan toouse a támogatott adatokat tárolót, mint a forrás/fogadó toolearn hivatkozásra hello hello táblában.  
     
    Cserélje le a hello hello értékének **start** tulajdonságát az aktuális nap hello és **end** hello másnap értéket. Adja meg, csak a hello dátumrész, és hagyja ki a hello idő összetevőjét hello időpontja. Például "2016-02-03", amely túl egyenértékű "2016-02-03T00:00:00Z"
     
    Mind a kezdő, mind a befejező dátum-időpont értéket [ISO formátumban](http://en.wikipedia.org/wiki/ISO_8601) kell megadni. Például: 2016-10-14T16:32:41Z. Hello **end** idő megadása nem kötelező, de ebben az oktatóanyagban használjuk. 
     
    Ha nem ad meg értéket a hello **end** tulajdonságot, akkor a program "**kezdés + 48 óra**". toorun hello folyamat határozatlan ideig, adja meg **9999-09-09** hello hello értékként **end** tulajdonság.
     
    A fenti példa hello amelyeket 24 adatszeletek minden adatszelet rendszer óránként készít adatszeletet.

    A folyamathoz tartozó definíció JSON-tulajdonságainak leírásáért lásd: [folyamatok létrehozása](data-factory-create-pipelines.md). A másolási tevékenységhez tartozó definíció JSON-tulajdonságainak leírásáért lásd: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md). A BlobSource által támogatott JSON-tulajdonságok leírásáért lásd: [Azure Blob-összekötő](data-factory-azure-blob-connector.md). Az SqlSink által támogatott JSON-tulajdonságok leírásáért lásd: [Azure SQL Database-összekötő](data-factory-azure-sql-connector.md).
2. Futtassa a következő parancs toocreate hello data factory-tábla hello.

    ```PowerShell   
    New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json
    ```

    Itt egy hello minta kimenet: 

    ```
    PipelineName      : ADFTutorialPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.PipelinePropertie
    ProvisioningState : Succeeded
    ```

**Gratulálunk!** Sikeresen létrehozott egy az Azure data factory egy folyamat toocopy Azure blob storage tooan Azure SQL-adatbázis adatait. 

## <a name="monitor-hello-pipeline"></a>A figyelő hello folyamat
Ezt a lépést használhatja az Azure PowerShell toomonitor egy az Azure data factory lesz.

1. Cserélje le &lt;DataFactoryName&gt; az adat-előállító és a futtató hello nevű **Get-AzureRmDataFactory**, és rendelje hozzá a hello kimeneti tooa változó $df.

    ```PowerShell  
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name <DataFactoryName>
    ```

    Példa:
    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH0516
    ```
    
    Ezután futtassa a következő kimeneti $df toosee hello nyomtatási hello tartalma: 
    
    ```
    PS C:\ADFGetStartedPSH> $df
    
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DataFactoryId     : 6f194b34-03b3-49ab-8f03-9f8a7b9d3e30
    ResourceGroupName : ADFTutorialResourceGroup
    Location          : West US
    Tags              : {}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DataFactoryProperties
    ProvisioningState : Succeeded
    ```
2. Futtatás **Get-AzureRmDataFactorySlice** hello az összes szeletek tooget adatait **OutputDataset**, vagyis hello kimeneti adatkészlet hello folyamatának.  

    ```PowerShell   
    Get-AzureRmDataFactorySlice $df -DatasetName OutputDataset -StartDateTime 2017-05-11T00:00:00Z
    ```

   Ez a beállítás meg kell felelnie a hello **Start** hello adatcsatorna JSON értéket. 24 szeletek, megjelenik egy a minden órában az aktuális nap too12 hello éjféltől hello a VAGYOK másnap.

   Az alábbiakban a három minta szeletek hello kimenetből: 

    ``` 
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 11:00:00 PM
    End               : 5/12/2017 12:00:00 AM
    RetryCount        : 0
    State             : Ready
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0

    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 9:00:00 PM
    End               : 5/11/2017 10:00:00 PM
    RetryCount        : 0
    State             : InProgress
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0   

    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 8:00:00 PM
    End               : 5/11/2017 9:00:00 PM
    RetryCount        : 0
    State             : Waiting
    SubState          : ConcurrencyLimit
    LatencyStatus     :
    LongRetryCount    : 0
    ```
3. Futtatás **Get-AzureRmDataFactoryRun** tooget hello részletek tevékenység fut egy **adott** szelet. Hello dátum-idő érték másolása hello kimenete hello előző parancs toospecify hello hello StartDateTime paraméter értékét. 

    ```PowerShell  
    Get-AzureRmDataFactoryRun $df -DatasetName OutputDataset -StartDateTime "5/11/2017 09:00:00 PM"
    ```

   Itt egy hello minta kimenet: 

    ```
    Id                  : c0ddbd75-d0c7-4816-a775-704bbd7c7eab_636301332000000000_636301368000000000_OutputDataset
    ResourceGroupName   : ADFTutorialResourceGroup
    DataFactoryName     : ADFTutorialDataFactoryPSH0516
    DatasetName         : OutputDataset
    ProcessingStartTime : 5/16/2017 8:00:33 PM
    ProcessingEndTime   : 5/16/2017 8:01:36 PM
    PercentComplete     : 100
    DataSliceStart      : 5/11/2017 9:00:00 PM
    DataSliceEnd        : 5/11/2017 10:00:00 PM
    Status              : Succeeded
    Timestamp           : 5/16/2017 8:00:33 PM
    RetryAttempt        : 0
    Properties          : {}
    ErrorMessage        :
    ActivityName        : CopyFromBlobToSQL
    PipelineName        : ADFTutorialPipeline
    Type                : Copy  
    ```

A Data Factory-parancsmagokkal kapcsolatos átfogó dokumentációt a [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories) (Data Factory-parancsmagok referenciája) című cikk tartalmaz.

## <a name="summary"></a>Összefoglalás
Ebben az oktatóanyagban létrehozott egy Azure data factory toocopy adatok Azure blob tooan Azure SQL-adatbázis. PowerShell toocreate hello adat-előállító, a társított szolgáltatások, a adatkészletek és a folyamat használta. Az alábbiakban a gépen hajtotta végre ebben az oktatóanyagban hello magas szintű lépéseket:  

1. Létrehozott egy Azure **data factoryt**.
2. **Társított szolgáltatásokat** hozott létre:

   a. Egy **Azure Storage** kapcsolódó szolgáltatás toolink az Azure storage-fiók, amely a bemeneti adatokat tárolja.     
   b. Egy **Azure SQL** kapcsolódó szolgáltatás toolink az SQL-adatbázis, amely tárolja a hello kimeneti adatokat.
3. **Adatkészleteket** hozott létre, amelyek a folyamat bemeneti és kimeneti adatait írják le.
4. Létrehozott egy **csővezeték** a **másolási tevékenység**, a **BlobSource** hello forrásként és **SqlSink** , hello fogadó.

## <a name="next-steps"></a>Következő lépések
Ez az oktatóanyag egy olyan másolási műveletet mutatott be, amelynek a forrásadattára egy Azure Blob Storage-tár, a céladattára pedig egy Azure SQL-adatbázis volt. hello következő táblázat felsorolja az adatforrások és a célhelyek hello másolási tevékenység által támogatott adattárolókhoz: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Hogyan toocopy adatok belőle egy adatok tárolására, kapcsolatos toolearn hivatkozásra hello hello adattároló hello tábla. 

