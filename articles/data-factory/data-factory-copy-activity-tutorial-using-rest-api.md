---
title: "Oktatóanyag: Használata REST API toocreate egy Azure Data Factory-folyamathoz |} Microsoft Docs"
description: "Ebben az oktatóanyagban a REST API toocreate egy Azure Data Factory-folyamat az az Azure blob storage Azure SQL-adatbázis a másolási tevékenység toocopy adatokkal használhatja."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1704cdf8-30ad-49bc-a71c-4057e26e7350
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: aa6c9b035101c4ff9acff90117ca6e3e7067f418
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-use-rest-api-toocreate-an-azure-data-factory-pipeline-toocopy-data"></a>Oktatóanyag: Használata REST API toocreate egy Azure Data Factory-folyamat toocopy adatok 
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

Ebből a cikkből megismerheti, hogyan toouse REST API toocreate egy adat-előállítót, és egy folyamatot, amely másol adatokat az Azure blob storage tooan Azure SQL-adatbázis. Ha új tooAzure adat-előállítót, olvassa végig hello [Data Factory bemutatása tooAzure](data-factory-introduction.md) cikk Ez az oktatóanyag elvégzése előtt.   

Az oktatóanyag segítségével egyetlen tevékenységgel (másolási tevékenységgel) rendelkező folyamatot hozhat létre. hello másolási tevékenység során a támogatott adatokat tároló tooa támogatott fogadó adatokat tároló másol adatokat. A forrásként és fogadóként támogatott adattárak listájáért lásd: [támogatott adattárak](data-factory-data-movement-activities.md#supported-data-stores-and-formats). hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva. További információ a másolási tevékenység hello: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).

Egy folyamathoz több tevékenység is tartozhat. És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását. További információért lásd: [egy folyamaton belüli több tevékenység](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

> [!NOTE]
> Ez a cikk nem foglalkozik az összes hello Data Factory REST API. A Data Factory-parancsmagokkal kapcsolatos átfogó dokumentációért tekintse meg a [Data Factory REST API Reference](/rest/api/datafactory/) (Data Factory REST API referenciája) című cikket.
>  
> hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat. Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: a folyamat tootransform adatokat Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Előfeltételek
* Lépkedjen végig [oktatóanyag – áttekintés](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) és teljes hello **előfeltétel** lépéseket.
* Telepítse gépére a [Curl](https://curl.haxx.se/dlwiz/) eszközt. A további parancsok toocreate egy adat-előállító hello Curl eszközt kell használni. 
* Az [ebben a cikkben](../azure-resource-manager/resource-group-create-service-principal-portal.md) szereplő utasításokat követve végezze el a következőket: 
  1. Hozzon létre egy **ADFCopyTutorialApp** nevű webalkalmazást az Azure Active Directoryban.
  2. Szerezze be az **ügyfél-azonosítót** és a **titkos kulcsot**. 
  3. Szerezze be a **bérlőazonosítót**. 
  4. Rendelje hozzá a hello **ADFCopyTutorialApp** alkalmazás toohello **Data Factory közreműködői** szerepkör.  
* Telepítse az [Azure PowerShellt](/powershell/azure/overview).  
* Indítsa el **PowerShell** és hello a következő lépéseket. Azure PowerShell nyitva hagyja az oktatóanyag hello végéig. Zárja be és nyissa meg újra, ha újra kell toorun hello parancsok.
  
  1. Futtassa a következő parancs hello, és írja be a hello felhasználónevet és jelszót toosign használhatja a toohello Azure-portálon:
    
    ```PowerShell 
    Login-AzureRmAccount
    ```   
  2. Futtassa a következő parancs tooview hello minden hello előfizetés ehhez a fiókhoz:

    ```PowerShell     
    Get-AzureRmSubscription
    ``` 
  3. Futtassa a következő parancs tooselect hello előfizetést, amelyet a toowork hello. Cserélje le  **&lt;NameOfAzureSubscription** &gt; hello nevet, az Azure-előfizetéshez. 
     
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
  4. Hozzon létre egy Azure erőforráscsoport nevű **ADFTutorialResourceGroup** hello hello PowerShell parancsot a következő futtatásával:  

    ```PowerShell     
      New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
     
      Ha hello erőforráscsoportban már létezik, akkor adja meg, hogy tooupdate (Y), vagy legyen (n). 
     
      Egyes ebben az oktatóanyagban hello lépések azt feltételezik, hogy hello erőforráscsoportot ADFTutorialResourceGroup használatát. Ha egy másik erőforráscsoportban található használja, ebben az oktatóanyagban kell toouse hello ADFTutorialResourceGroup helyett az erőforráscsoport nevét.

## <a name="create-json-definitions"></a>JSON-definíciók létrehozása
Következő hello mappáját curl.exe a JSON-fájlok létrehozása 

### <a name="datafactoryjson"></a>datafactory.json
> [!IMPORTANT]
> Egyedinek kell lennie globálisan, ezért érdemes tooprefix/utótag ADFCopyTutorialDF toomake azt egy egyedi nevet. 
> 
> 

```JSON
{  
    "name": "ADFCopyTutorialDF",  
    "location": "WestUS"
}  
```

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.json
> [!IMPORTANT]
> Az **accountname** és az **accountkey** kifejezés helyére írja be Azure Storage-tárfiókja nevére, illetve kulcsát. toolearn hogyan tooget tárhelyét férnek hozzá, tekintse meg [megtekintése, másolása és újragenerálása tárolási hívóbetűk](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).

```JSON
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

A JSON tulajdonságokról további részleteket tartalmaz az [Azure Storage társított szolgáltatás](data-factory-azure-blob-connector.md#azure-storage-linked-service) című cikk.

### <a name="azuersqllinkedservicejson"></a>azuersqllinkedservice.json
> [!IMPORTANT]
> Cserélje le **kiszolgálónév**, **databasename**, **felhasználónév**, és **jelszó** az Azure SQL Server, SQL-adatbázis neve nevű felhasználó fiók és hello fiókhoz tartozó jelszót.  
> 
>

```JSON
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

A JSON tulajdonságokról további részleteket tartalmaz az [Azure SQL társított szolgáltatás](data-factory-azure-sql-connector.md#linked-service-properties) című cikk.

### <a name="inputdatasetjson"></a>inputdataset.json

```JSON
{
  "name": "AzureBlobInput",
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
      "folderPath": "adftutorial/",
      "fileName": "emp.txt",
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

### <a name="outputdatasetjson"></a>outputdataset.json

```JSON
{
  "name": "AzureSqlOutput",
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

### <a name="pipelinejson"></a>pipeline.json

```JSON
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from a blob tooAzure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "description": "Push Regional Effectiveness Campaign data tooAzure SQL database",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
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
- Adjon meg a hello tevékenység értéke túl**AzureBlobInput** és hello tevékenység túl van-e állítva a kimeneti**AzureSqlOutput**. 
- A hello **typeProperties** szakaszban **BlobSource** hello forrás típusaként van megadva, és **SqlSink** hello a fogadó típusa van megadva. Források és mosdók hello másolási tevékenység által támogatott adattárolókhoz teljes listáját lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Hogyan toouse a támogatott adatokat tárolót, mint a forrás/fogadó toolearn hivatkozásra hello hello táblában.  
 
Cserélje le a hello hello értékének **start** tulajdonságát az aktuális nap hello és **end** hello másnap értéket. Adja meg, csak a hello dátumrész, és hagyja ki a hello idő összetevőjét hello időpontja. Például "2017-02-03", amely túl egyenértékű "2017-02-03T00:00:00Z"
 
Mind a kezdő, mind a befejező dátum-időpont értéket [ISO formátumban](http://en.wikipedia.org/wiki/ISO_8601) kell megadni. Például: 2016-10-14T16:32:41Z. Hello **end** idő megadása nem kötelező, de ebben az oktatóanyagban használjuk. 
 
Ha nem ad meg értéket a hello **end** tulajdonságot, akkor a program "**kezdés + 48 óra**". toorun hello folyamat határozatlan ideig, adja meg **9999-09-09** hello hello értékként **end** tulajdonság.
 
A fenti példa hello amelyeket 24 adatszeletek minden adatszelet rendszer óránként készít adatszeletet.

A folyamathoz tartozó definíció JSON-tulajdonságainak leírásáért lásd: [folyamatok létrehozása](data-factory-create-pipelines.md). A másolási tevékenységhez tartozó definíció JSON-tulajdonságainak leírásáért lásd: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md). A BlobSource által támogatott JSON-tulajdonságok leírásáért lásd: [Azure Blob-összekötő](data-factory-azure-blob-connector.md). Az SqlSink által támogatott JSON-tulajdonságok leírása az [Azure SQL Database-összekötő](data-factory-azure-sql-connector.md) című cikkben található.

## <a name="set-global-variables"></a>Globális változók beállítása
Az Azure PowerShell-lel hajtható végre hello parancsok után hello értékeket a saját cserélje le a következő:

> [!IMPORTANT]
> Az ügyfél-azonosító, a titkos ügyfélkód, a bérlőazonosító és az előfizetés-azonosító beszerzésével kapcsolatban olvassa el az [Előfeltételek](#prerequisites) című fejezetet.   
> 
> 

```JSON
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
```

Parancs hello adat-előállító nevét hello frissítése után a következő futtatási hello használata esetén: 

```
$adf = "ADFCopyTutorialDF"
```

## <a name="authenticate-with-aad"></a>Hitelesítés az AAD segítségével
Futtassa a következő parancs tooauthenticate az Azure Active Directory (AAD) hello: 

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken) 
```

## <a name="create-data-factory"></a>Data factory létrehozása
Ebben a lépésben egy **ADFCopyTutorialDF** nevű Azure-adatelőállítót fog létrehozni. A data factory egy vagy több folyamattal rendelkezhet. A folyamaton belül egy vagy több tevékenység lehet. Például egy tooa cél forrásadatok másolási tevékenység toocopy adatait tárolja. A HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform bemeneti adatok tooproduct kimeneti adatokat. Futtassa a következő parancsok toocreate hello adat-előállító hello: 

1. Rendelje hozzá a hello parancs toovariable nevű **cmd**. 
   
    > [!IMPORTANT]
    > Győződjön meg arról, hogy az itt megadott (ADFCopyTutorialDF) egyező hello hello megadott név hello adat-előállító hello neve **datafactory.json**. 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/ADFCopyTutorialDF0411?api-version=2015-10-01};
    ```
2. Hello parancs futtatásával **Invoke-Command**.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Hello eredményeinek megtekintése. Ha hello adat-előállító létrehozása sikeresen befejeződött, megjelenik az hello data factory hello a JSON hello **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.  
   
    ```
    Write-Host $results
    ```

Vegye figyelembe a következő pontok hello:

* Azure Data Factory hello hello nevének globálisan egyedi kell lennie. Ha az eredmények hello hibát látja: **nem érhető el adat-előállító "ADFCopyTutorialDF"**, hello a következő lépéseket:  
  
  1. Hello nevének módosítása (például yournameADFCopyTutorialDF) a hello **datafactory.json** fájlt.
  2. Az első parancs hello ahol hello **$cmd** változó értéket kapja, cserélje le a ADFCopyTutorialDF hello új névvel és hello parancsot. 
  3. Futtassa a hello következő két parancsok tooinvoke hello REST API toocreate hello data factory és nyomtatás hello hello művelet eredménye. 
     
     A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.
* toocreate adat-előállító esetekben kell toobe egy Azure-előfizetés hello közreműködői/rendszergazda
* hello adat-előállító nevét hello előfordulhat, hogy a jövőbeli hello DNS-névként regisztrált, és így nyilvánosan láthatóvá válhat.
* Ha hello hibaüzenet: "**ehhez az előfizetéshez nincs regisztrált toouse névtér Microsoft.DataFactory**", hello alábbi, és próbálja meg újra közzétenni: 
  
  * Az Azure PowerShell futtassa a következő parancs tooregister hello adat-előállító szolgáltató hello: 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    A következő parancs tooconfirm hello futtathatja, hogy hello Data Factory szolgáltató regisztrálva van. 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Bejelentkezés használatával hello Azure-előfizetéssel való hello [Azure-portálon](https://portal.azure.com) , és keresse meg a tooa adat-előállító panel (vagy) hozzon létre egy adat-előállító hello Azure-portálon. Ez a művelet automatikusan regisztrálja az Ön hello szolgáltató.

Mielőtt létrehozna egy folyamatot, kell toocreate néhány adat-előállító entitások először. Először létre kell hoznia összekapcsolt szolgáltatások toolink forrás és cél adatokat tárolja tooyour adatok tárolásához. Ezután határozzon meg a bemeneti és kimeneti adatkészletek toorepresent adatok csatolt adatok tárolja. Végezetül hozza létre egy tevékenységet, ezek az adatkészletek használó hello folyamat.

## <a name="create-linked-services"></a>Társított szolgáltatások létrehozása
A data factory toolink adatait tárolja, és számítási szolgáltatások toohello adat-előállító létrehozása társított szolgáltatások. Ebben az oktatóanyagban nem használunk számítási szolgáltatásokat (például Azure HDInsight vagy Azure Data Lake Analytics). Csak kétféle típusú adattárat használunk: Azure Storage (forrás) és Azure SQL Database (cél). Ezért két társított szolgáltatást fog létrehozni AzureStorageLinkedService és AzureSqlLinkedService néven (típus: AzureStorage és AzureSqlDatabase).  

hello AzureStorageLinkedService művelet ugyan összeköti a az Azure storage-fiók toohello adat-előállítóban. Ez a tárfiók egy hello akkor létrejött a tároló és hello adatok részeként feltöltött [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

AzureSqlLinkedService az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz. hello blobtárolóból másolt hello adatok az adatbázis tárolja. Hello üres tábla az adatbázisban részeként létrehozott [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).  

### <a name="create-azure-storage-linked-service"></a>Azure Storage társított szolgáltatás létrehozása
Ebben a lépésben a az Azure storage-fiók tooyour adat-előállító hivatkozásra. Ebben a szakaszban megadhatja hello nevét és az Azure storage-fiók kulcsát. Lásd: [Azure Storage társított szolgáltatásnak](data-factory-azure-blob-connector.md#azure-storage-linked-service) JSON használt tulajdonságok toodefine vonatkozó további információért egy Azure Storage társított szolgáltatásnak.  

1. Rendelje hozzá a hello parancs toovariable nevű **cmd**. 

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@azurestoragelinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. Hello parancs futtatásával **Invoke-Command**.

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Hello eredményeinek megtekintése. Ha hello társított szolgáltatás létrehozása sikeresen befejeződött úgy, hogy hello JSON hello szolgáltatáshoz kapcsolódó hello **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.

    ```PowerShell   
    Write-Host $results
    ```

### <a name="create-azure-sql-linked-service"></a>Azure SQL társított szolgáltatás létrehozása
Ebben a lépésben az Azure SQL adatbázis tooyour adat-előállító hivatkozásra. Ebben a szakaszban megadhatja hello Azure SQL-kiszolgáló neve, az adatbázis nevét, a felhasználónév és a felhasználó jelszavát. Lásd: [Azure SQL társított szolgáltatásnak](data-factory-azure-sql-connector.md#linked-service-properties) JSON használt tulajdonságok toodefine vonatkozó további információért az Azure SQL társított szolgáltatásnak.

1. Rendelje hozzá a hello parancs toovariable nevű **cmd**. 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azuresqllinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureSqlLinkedService?api-version=2015-10-01};
    ```
2. Hello parancs futtatásával **Invoke-Command**.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Hello eredményeinek megtekintése. Ha hello társított szolgáltatás létrehozása sikeresen befejeződött úgy, hogy hello JSON hello szolgáltatáshoz kapcsolódó hello **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.
   
    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a>Adatkészletek létrehozása
Hello előző lépésben létrehozott összekapcsolt szolgáltatások toolink az Azure Storage-fiók és az Azure SQL adatbázis tooyour adat-előállítóban. Ebben a lépésben AzureBlobInput és AzureSqlOutput, amelyek megfelelnek a bemeneti és kimeneti adatok AzureStorageLinkedService és AzureSqlLinkedService által hivatkozott hello adattárolókhoz tárolt nevű két adatkészletet határozza meg.

hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot. És hello bemeneti blob-adathalmazra (AzureBlobInput) hello tároló és hello bemeneti adatokat tartalmazó hello mappát adja meg.  

Hello csatolt Azure SQL Database szolgáltatáshoz hasonlóan hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg. És hello kimeneti SQL táblázat dataset (OututDataset) hello adatbázis toowhich hello hello blobtárolóból az adatok másolásakor hello tábla határozza meg. 

### <a name="create-input-dataset"></a>Bemeneti adatkészlet létrehozása
Ebben a lépésben hoz létre, mely tooa fájlját (emp.txt) AzureBlobInput nevű adatkészlete hello gyökérmappájában lévő mappának a blob-tároló (adftutorial) az Azure Storage hello AzureStorageLinkedService kapcsolódó szolgáltatás által képviselt hello. Ha nem hello fájlnév értéket (vagy hagyja ki), a hello bemeneti mappában található összes BLOB adatait is másolt toohello cél. Ebben az oktatóanyagban hello fájlnév értéket kell megadni. 

1. Rendelje hozzá a hello parancs toovariable nevű **cmd**. 

    ```PowerSHell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. Hello parancs futtatásával **Invoke-Command**.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Hello eredményeinek megtekintése. Ha hello adatkészlet sikeresen létrejött, a hello hello adatkészlet JSON hello látható **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.
   
    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a>Kimeneti adatkészlet létrehozása
hello csatolt Azure SQL Database szolgáltatás hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg. hello kimeneti SQL táblázat dataset (OututDataset) hoz létre ebben a lépésben meghatározza hello adatbázis toowhich hello adatokat a hello blob storage hello táblájában másolódik.

1. Rendelje hozzá a hello parancs toovariable nevű **cmd**.

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureSqlOutput?api-version=2015-10-01};
    ```
2. Hello parancs futtatásával **Invoke-Command**.
    
    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Hello eredményeinek megtekintése. Ha hello adatkészlet sikeresen létrejött, a hello hello adatkészlet JSON hello látható **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.
   
    ```PowerShell
    Write-Host $results
    ``` 

## <a name="create-pipeline"></a>Folyamat létrehozása
Ebben a lépésben létrehoz egy **másolási tevékenységgel** rendelkező folyamatot, amely bemenetként az **AzureBlobInput**, kimenetként az **AzureSqlOutput** adatkészletet használja.

Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezést. Ebben az oktatóanyagban a kimeneti adatkészlet óránként konfigurált tooproduce szelet. hello folyamat rendelkezik egy kezdési és befejezési időpontja, amelyek egy nap telhet el, amely 24 óra. Ezért a kimeneti adatkészlet 24 szeletek előállított hello folyamat. 

1. Rendelje hozzá a hello parancs toovariable nevű **cmd**.

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. Hello parancs futtatásával **Invoke-Command**.

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Hello eredményeinek megtekintése. Ha hello adatkészlet sikeresen létrejött, a hello hello adatkészlet JSON hello látható **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.  

    ```PowerShell   
    Write-Host $results
    ```

**Gratulálunk!** Sikeresen létrehozott egy Azure data factory a egy folyamatot, amely másolja az adatokat az Azure Blob Storage tooAzure SQL-adatbázisból.

## <a name="monitor-pipeline"></a>Folyamat figyelése
Ebben a lépésben a Data Factory REST API toomonitor szeletek hello folyamat alatt előállított funkcióval.

```PowerShell
$ds ="AzureSqlOutput"
```

> [!IMPORTANT] 
> Ellenőrizze, hogy hello kezdési és befejezési időpontokat a következő megadott hello hello indítása parancsot, és befejezési időpontja hello folyamatának. 

```PowerShell
$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=2017-05-11T00%3a00%3a00.0000000Z"&"end=2017-05-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};
```

```PowerShell
$results2 = Invoke-Command -scriptblock $cmd;
```

```PowerShell
IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

Végezzen hello Invoke-Command és hello egy látja, hogy a szelet **készen** állapota vagy **sikertelen** állapotát. Ha hello szelet készen állapotban van, ellenőrizze a hello **üres** hello kimeneti adatok az Azure SQL-adatbázis táblájában. 

Minden szelet két sornyi adatot hello forrásfájlból másolt toohello üres táblázat hello Azure SQL adatbázis esetén. Ezért akkor látni 24 új rekordok hello üres tábla összes hello szeletek sikeresen feldolgozott (üzemkész állapotban). 

## <a name="summary"></a>Összefoglalás
Ebben az oktatóanyagban használt REST API toocreate egy Azure blob tooan Azure SQL-adatbázis az Azure data factory toocopy adatait. Az alábbiakban a gépen hajtotta végre ebben az oktatóanyagban hello magas szintű lépéseket:  

1. Létrehozott egy Azure **data factoryt**.
2. **Társított szolgáltatásokat** hozott létre:
   1. Egy Azure Storage társított szolgáltatás toolink az Azure Storage-fiók, amely a bemeneti adatokat tárolja.     
   2. Egy Azure SQL társított szolgáltatás toolink az Azure SQL-adatbázis, amely tárolja a hello kimeneti adatokat. 
3. **Adatkészleteket** hozott létre, amelyek az adatcsatorna bemeneti és kimeneti adatait írják le.
4. Létrehozott egy másolási tevékenységgel ellátott **adatcsatornát**, ahol a BlobSource a forrás, az SqlSink pedig a fogadó. 

## <a name="next-steps"></a>Következő lépések
Ez az oktatóanyag egy olyan másolási műveletet mutatott be, amelynek a forrásadattára egy Azure Blob Storage-tár, a céladattára pedig egy Azure SQL-adatbázis volt. hello következő táblázat felsorolja az adatforrások és a célhelyek hello másolási tevékenység által támogatott adattárolókhoz: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Hogyan toocopy adatok belőle egy adatok tárolására, kapcsolatos toolearn hivatkozásra hello hello adattároló hello tábla.
