---
title: "U-SQL parancsfájl - Azure aaaTransform adatok |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprocess vagy átalakítási adatok Azure Data Lake Analytics U-SQL-parancsfájlok futtatásával számítási szolgáltatás."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e17c1255-62c2-4e2e-bb60-d25274903e80
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: 51fdb40334d0c131720f65c3a96b4c5045a98b24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a>Adatok átalakítása Azure Data Lake Analytics U-SQL-parancsfájlok futtatásával 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-tevékenység](data-factory-hive-activity.md) 
> * [A Pig-tevékenység](data-factory-pig-activity.md)
> * [MapReduce művelethez](data-factory-map-reduce.md)
> * [Hadoop Streamelési tevékenységben](data-factory-hadoop-streaming-activity.md)
> * [A Spark-tevékenység](data-factory-spark.md)
> * [Machine Learning kötegelt végrehajtási tevékenység](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Update-erőforrástevékenység](data-factory-azure-ml-update-resource-activity.md)
> * [Tárolt eljárási tevékenység](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-tevékenység](data-factory-usql-activity.md)
> * [.NET egyéni tevékenység](data-factory-use-custom-activities.md)

Egy folyamatot egy az Azure data factory az adatokat a csatolt tárolószolgáltatások csatolt számítási szolgáltatások használatával dolgozza fel. Ha minden tevékenység egyedi feldolgozása műveletet hajt végre tevékenységek sorrendje tartalmaz. Ez a cikk ismerteti a hello **Data Lake Analytics U-SQL tevékenység** , amelyen fut a **U-SQL** a parancsfájl egy **Azure Data Lake Analytics** számítási kapcsolódó szolgáltatás. 

> [!NOTE]
> Azure Data Lake Analytics-fiók létrehozása előtt hoz létre egy folyamatot egy Data Lake Analytics U-SQL-tevékenység. Azure Data Lake Analytics kapcsolatos toolearn lásd: [Ismerkedés az Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).
> 
> Felülvizsgálati hello [az adatcsatorna első oktatóanyaga Szerkesztés](data-factory-build-your-first-pipeline.md) részletes lépéseket toocreate egy adat-előállítót, a kapcsolódó szolgáltatások, az adatkészletek és a folyamat. JSON kódtöredékek használja a Data Factory Editor vagy a Visual Studio vagy az Azure PowerShell toocreate adat-előállító szervezeteknél.

## <a name="supported-authentication-types"></a>Támogatott hitelesítési típusok
U-SQL-tevékenység alábbi Data Lake Analytics elleni hitelesítési típust támogatja:
* Egyszerű szolgáltatásnév hitelesítése
* Felhasználói hitelesítő adatok (OAuth) hitelesítés 

Azt javasoljuk, hogy használja-e szolgáltatás egyszerű hitelesítést, különösen a egy ütemezett U-SQL-végrehajtást. Jogkivonat lejáratáról fordulhat elő a felhasználói hitelesítő adatok hitelesítéssel. További konfigurációs információkért lásd: hello [szolgáltatástulajdonságok kapcsolódó](#azure-data-lake-analytics-linked-service) szakasz.

## <a name="azure-data-lake-analytics-linked-service"></a>Az Azure Data Lake Analytics társított szolgáltatás
Létrehozhat egy **Azure Data Lake Analytics** szolgáltatás toolink egy Azure Data Lake Analytics számítási szolgáltatás tooan az Azure data factory kapcsolt. Data Lake Analytics U-SQL-tevékenység hello folyamat hello toothis kapcsolódó szolgáltatás hivatkozik. 

hello következő táblázat ismerteti hello hello JSON-definícióból használt általános tulajdonságok. További választhat egyszerű szolgáltatásnév és felhasználói hitelesítő adatok hitelesítése.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| **típusa** |hello tulajdonságra kell megadni: **AzureDataLakeAnalytics**. |Igen |
| **Fióknév** |Az Azure Data Lake Analytics-fiók neve. |Igen |
| **datalakeanalyticsuri paraméter** |Az Azure Data Lake Analytics URI. |Nem |
| **előfizetés-azonosító** |Az Azure előfizetés-azonosító |Nem (Ha nincs megadva, az adat-előállító használt hello előfizetés). |
| **erőforráscsoport-név** |Azure erőforráscsoport-név |Nem (Ha nincs megadva, az adat-előállító használt hello erőforráscsoport). |

### <a name="service-principal-authentication-recommended"></a>Szolgáltatás egyszerű hitelesítés (ajánlott)
toouse szolgáltatás egyszerű hitelesítési regisztrálása az Azure Active Directory (Azure AD) és támogatás azt hello alkalmazás entitás tooData Lake áruház eléréséhez. Részletes útmutató: [szolgáltatások közötti hitelesítési](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Jegyezze fel az értéket, amely a következő hello toodefine hello társított szolgáltatáshoz:
* Alkalmazásazonosító
* Alkalmazás kulcs 
* Bérlőazonosító

Szolgáltatás egyszerű hitelesítés használata a következő tulajdonságok hello megadásával:

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| **servicePrincipalId** | Adja meg a hello alkalmazás ügyfél-azonosítót. | Igen |
| **servicePrincipalKey** | Adja meg a hello kulcsát. | Igen |
| **Bérlői** | Adja meg a hello bérlői adatokat (tartomány nevét vagy a bérlő azonosító) alatt az alkalmazás található. Ez által rámutató hello egér hello Azure-portál jobb felső sarkában hello kérheti le. | Igen |

**Példa: Szolgáltatás egyszerű hitelesítés**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Felhasználói hitelesítő adatok hitelesítése
Alternatív megoldásként használható felhasználói hitelesítő adatok hitelesítése Data Lake Analytics megadásával következő tulajdonságai hello:

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| **engedélyezési** | Kattintson a hello **engedélyezés** hello Data Factory Editor gombra, és írja be a hitelesítő adat, amelyet hozzárendel hello automatikusan létrehozott engedélyezési URL-cím toothis tulajdonság. | Igen |
| **munkamenet-azonosító** | OAuth munkamenet-azonosító hello OAuth hitelesítési munkamenetből. Minden munkamenet-azonosító egyedi, és csak egyszer használható. Ez a beállítás automatikusan létrejön a Data Factory Editor hello használatakor. | Igen |

**Példa: Felhasználók hitelesítő adatok hitelesítése**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Jogkivonat lejáratáról
Ön hello segítségével létrehozott engedélyezési kód hello **engedélyezés** gomb némi várakozás után lejár. Tekintse meg a következő táblázat a különböző típusú felhasználói fiókok hello lejárati idejének hello. Hello a következő hibaüzenetet fog látni amikor hello hitelesítési **jogkivonat lejár**: hitelesítőadat-műveleti hiba: invalid_grant - AADSTS70002: Hiba történt a hitelesítő adatok ellenőrzése. AADSTS70008: hello megadott hozzáférés biztosítása lejárt vagy visszavonták. Nyomkövetési azonosító: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelációazonosító: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 időbélyegző: 2015-12-15 21:09:31Z

| Felhasználó típusa | Után lejár |
|:--- |:--- |
| Felhasználói fiókok Azure Active Directory által nem kezelt (@hotmail.com, @live.comstb.) |12 óra |
| Felhasználói fiókok felügyelete által Azure Active Directory (AAD) |Futtatás után utolsó szelet hello 14 nap. <br/><br/>90 nap, ha a szelet OAuth-alapú társított szolgáltatás fut legalább 14 naponta. |

tooavoid/hárítsa el a hiba, ismét engedélyezheti a hello segítségével **engedélyezés** gombbal hello **jogkivonat lejár** és telepítse újra a kapcsolódó hello szolgáltatást. Értékek is létrehozhat **sessionId** és **engedélyezési** programozott módon, az alábbiak szerint kód tulajdonságok:

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

Lásd: [AzureDataLakeStoreLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), és [AuthorizationSessionGetResponse osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) részletes kapcsolatos témakörök kapcsolatos hello adat-előállító osztályok hello kódban használt. Adjon hozzá egy hivatkozást,: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll hello WindowsFormsWebAuthenticationDialog osztály számára. 

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL-tevékenység
a következő JSON részlet hello egy Data Lake Analytics U-SQL-tevékenység az adatcsatorna határozza meg. hello tevékenység egy hivatkozást a korábban létrehozott társított Azure Data Lake Analytics szolgáltatás toohello van.   

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This is a pipeline toocompute events for en-gb locale and date less than 2012/02/19.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00Z",
        "end": "2015-08-08T01:00:00Z",
        "isPaused": false
    }
}
```

hello következő táblázatban neveit és leírásait, amelyek az adott toothis tevékenység. 

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type |hello type tulajdonság túl be kell állítani**DataLakeAnalyticsU-SQL**. |Igen |
| scriptPath |Elérési út toofolder hello U-SQL parancsfájlt tartalmazó. Hello fájl neve nem kis-és nagybetűket. |Nem (Ha a parancsfájl használata) |
| scriptLinkedService |Kapcsolódó szolgáltatás, amely a hello parancsfájl toohello adat-előállító tartalmazó hello tároló |Nem (Ha a parancsfájl használata) |
| Parancsfájl |Adja meg a beágyazott parancsfájlja scriptPath és a scriptLinkedService megadása helyett. Például: `"script": "CREATE DATABASE test"`. |Nem (Ha a ScriptPath tulajdonságot is és a scriptLinkedService használ) |
| degreeOfParallelism |csomópontok maximális száma hello egyidejűleg használt toorun hello feladat. |Nem |
| Prioritás |Meghatározza, hogy szereplő várólistáján szereplő feladatok közül kell lennie a kijelölt toorun először. hello hello kevesebb, hello hello elsőbbséget. |Nem |
| paraméterek |Hello U-SQL parancsfájl paramétereinek |Nem |
| runtimeVersion | U-SQL hello motor toouse futásidejű verzióját | Nem | 
| compilationMode | <p>Fordítási mód az U-SQL. A következő értékek egyike lehet:</p> <ul><li>**Szemantikus:** csak a szemantikai ellenőrzések és a szükséges megerősítések végrehajtani.</li><li>**Teljes:** hajtsa végre a hello teljes fordítási, beleértve a szintaxis-ellenőrzés, optimalizálás, kódgenerálás, stb.</li><li>**SingleBox:** hello teljes fordítási a TargetType beállítás tooSingleBox végrehajtani.</li></ul><p>Ez a tulajdonság értékét nem adja meg, ha a hello server hello optimális lefordítására határozza meg. </p>| Nem | 

Lásd: [SearchLogProcessing.txt parancsfájl Definition](#sample-u-sql-script) hello parancsfájl definíciójához. 

## <a name="sample-input-and-output-datasets"></a>Minta bemeneti és kimeneti adatkészletek
### <a name="input-dataset"></a>Bemeneti adatkészlet
Ebben a példában hello bemeneti adatokat az Azure Data Lake Store (searchlog.tsv fájl fájl hello datalake-/ bemeneti mappában) található. 

```json
{
    "name": "DataLakeTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}    
```

### <a name="output-dataset"></a>Kimeneti adatkészlet
Ebben a példában hello U-SQL-parancsfájl által előállított hello kimeneti adatok tárolódnak az Azure Data Lake Store (datalake-/ kimeneti mappa). 

```json
{
    "name": "EventsByRegionTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="sample-data-lake-store-linked-service"></a>A minta Data Lake Store társított szolgáltatás
Itt található hello definition hello minta Azure Data Lake Store társított szolgáltatás hello bemeneti/kimeneti adatkészletek használják. 

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
        }
    }
}
```

Lásd: [tooand adatok áthelyezése az Azure Data Lake Store](data-factory-azure-datalake-connector.md) cikk a JSON-tulajdonságok leírását. 

## <a name="sample-u-sql-script"></a>Minta U-SQL parancsfájl

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    too@out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

az értékek hello ** @in ** és ** @out ** hello U-SQL parancsfájl átadott paraméterek dinamikusan által ADF hello "parameters" szakaszában. Hello csővezeték-definícióban hello "parameters" című szakaszában talál.

Adhat meg egyéb tulajdonságait, például degreeOfParallelism és prioritását, valamint a feldolgozási sor hello Azure Data Lake Analytics szolgáltatásban futó feladatok hello definíciója.

## <a name="dynamic-parameters"></a>Dinamikus paraméterek
Hello minta csővezeték-definícióban és paraméterek vannak hozzárendelve kódolt értékekkel. 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

Ehelyett is lehetséges toouse dinamikus paraméterek. Példa: 

```json
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

Ebben az esetben a bemeneti fájlok továbbra is átveszik hello /datalake/input mappából, és a kimeneti fájlok jönnek létre hello /datalake/output mappában. hello fájlnevek dinamikusak hello szelet kezdési ideje alapján.  

