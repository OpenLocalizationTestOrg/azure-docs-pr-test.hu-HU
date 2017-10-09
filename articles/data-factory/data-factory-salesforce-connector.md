---
title: a Data Factory aaaMove Salesforce adatokat |} Microsoft Docs
description: "Megtudhatja, hogyan toomove Azure Data Factory használatával Salesforce adatait."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: dbe3bfd6-fa6a-491a-9638-3a9a10d396d1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: c1bde2a333f5a3c0a995eb8c13ecf585132888b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a>Adatok áthelyezése Salesforce Azure Data Factory használatával
Ez a cikk ismerteti, hogyan használhatja az az Azure data factory toocopy adatok Salesforce tooany adattárból hello fogadó oszlopában az hello felsorolt másolási tevékenység [támogatott források és mosdók](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. Ez a cikk épít, hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amely adatmozgás általános áttekintést során másolási tevékenység és a támogatott adatokat tároló kombinációja.

Az Azure Data Factory jelenleg csak az adatok áthelyezése Salesforce túl[fogadó adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats), azonban nem támogatja egyéb adatok tárolók tooSalesforce áthelyezése adatait.

## <a name="supported-versions"></a>Támogatott verziók
Ez az összekötő támogatja a következő Salesforce kiadása hello: Developer Edition, Professional Edition, Enterprise Edition vagy korlátlan kiadását. És a Salesforce éles, védőfal és az egyéni tartomány másolását támogatja.

## <a name="prerequisites"></a>Előfeltételek
* API-engedély engedélyezve kell lennie. Lásd: [hogyan engedélyezhető az engedélycsoport által a Salesforce-ban API-hozzáférés?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)
* Salesforce tooon helyszíni adattárolókhoz toocopy adatait, rendelkeznie kell legalább adatok felügyeleti átjáró 2.0-s verziójával a helyszíni környezetben.

## <a name="salesforce-request-limits"></a>Salesforce kérelmekre vonatkozó korlátok
Salesforce rendelkezik mind az API-kérelmek teljes száma, és a egyidejű API-kérés. Vegye figyelembe a következő pontok hello:

- Ha hello egyidejű kérelmek száma meghaladja hello, sávszélesség-szabályozás következik be, és látni fogja a véletlen hibákat.
- Ha hello kérelmek teljes száma túllépi hello, hello Salesforce-fiókban le lesz tiltva 24 órán át.

Hello "REQUEST_LIMIT_EXCEEDED" hiba mindkét forgatókönyvet is akkor fordulhat elő. Hello hello "API kérelmekre vonatkozó korlátok" szakaszában talál [Salesforce fejlesztői korlátok](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) cikkben alább.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, mely az adatok Salesforce különböző eszközök/API-k használatával létrehozhat egy folyamatot.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello: 

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek a Salesforce adatait használt toocopy JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Salesforce tooAzure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) című szakaszát. 

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooSalesforce részleteit tartalmazzák: 

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
hello a következő táblázat ismerteti, amelyek adott toohello Salesforce társított szolgáltatás JSON-elemek szerepelnek.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **Salesforce**. |Igen |
| environmentUrl | Adja meg a hello URL-címet a Salesforce-példány. <br><br> -Alapértelmezett érték a "https://login.salesforce.com". <br> -toocopy adatok védőfalak, adja meg a "https://test.salesforce.com". <br> -toocopy adatait egyéni tartományt, adja meg, például "https://[domain].my.salesforce.com". |Nem |
| felhasználónév |Adjon meg egy felhasználónevet hello felhasználói fiókhoz. |Igen |
| jelszó |Adjon meg egy hello felhasználói fiók jelszavát. |Igen |
| securityToken |Adjon meg egy biztonsági jogkivonatot hello felhasználói fiókhoz. Lásd: [biztonsági jogkivonatának beszerzéséhez](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) útmutatást tooreset/get egy biztonsági jogkivonatot. biztonsági jogkivonatok kapcsolatos toolearn általában, lásd: [biztonsági és hello API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Igen |

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Illetve meghatározásához adatkészletek rendelkezésre álló tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla és így tovább).

Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. a DataSet hello típusú szakasz hello typeProperties **RelationalTable** rendelkezik hello következő tulajdonságai:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |A Salesforce-ban hello tábla neve. |Nem (Ha egy **lekérdezés** a **RelationalSource** van megadva) |

> [!IMPORTANT]
> hello API neve "__c" részét hello bármilyen egyéni objektum szükséges.

![Data Factory - kapcsolat Salesforce - API neve](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok és tevékenységek meghatározásához rendelkezésre álló tulajdonságok teljes listáját lásd: hello [folyamatok létrehozása](data-factory-create-pipelines.md) cikk. Tulajdonságok nevét, leírását, bemeneti és kimeneti táblák, valamint különböző házirendeket is tevékenységi érhető el.

hello tulajdonságok által biztosított hello typeProperties szakasz hello tevékenységekre, hello ugyanakkor, tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

A másolási tevékenység, ha hello adatforrás hello típusú **RelationalSource** (amely tartalmazza a Salesforce), typeProperties szakaszában érhetők hello következő tulajdonságai:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |Egy SQL-92 lekérdezés vagy [Salesforce objektum Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) lekérdezés. Például: `select * from MyTable__c`. |Nem (ha hello **tableName** a hello **dataset** van megadva) |

> [!IMPORTANT]
> hello API neve "__c" részét hello bármilyen egyéni objektum szükséges.

![Data Factory - kapcsolat Salesforce - API neve](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a>Lekérdezés tippek
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>Adatgyűjtés használata a where záradék található dátum és idő oszlop
Ha meg hello SOQL vagy SQL-lekérdezés, fizetési figyelmet toohello különbség a dátum és idő formátumban. Példa:

* **SOQL minta**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`
* **SQL-minta**:
    * **Másolása varázsló toospecify hello lekérdezéssel:**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`
    * **Toospecify hello lekérdezés szerkesztése JSON használatával (escape-karakter megfelelően):**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`

### <a name="retrieving-data-from-salesforce-report"></a>Adatok beolvasása a Salesforce-jelentés
Beolvasható adat Salesforce-jelentéseket lekérdezést megadásával `{call "<report name>"}`, például. `"query": "{call \"TestReport\"}"`.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>A Salesforce Lomtárból törölt rekordok visszanyerése
tooquery hello véglegesen törölve rögzíti a Salesforce Lomtárból, megadhat **"IsDeleted = 1"** a lekérdezésben. Például:

* csak hello törölt rekordok tooquery, adja meg "Válasszon * a MyTable__c **ahol IsDeleted = 1**"
* összes hello hello meglévő és hello törölni, beleértve rekordok tooquery adja meg "Válasszon * a MyTable__c **ahol IsDeleted = 0 vagy IsDeleted = 1**"

## <a name="json-example-copy-data-from-salesforce-tooazure-blob"></a>JSON-példa: adatok másolása az Salesforce tooAzure Blob
hello alábbi példa minta JSON definícióit tartalmazza használható toocreate adatcsatorna hello segítségével [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azok hogyan Salesforce tooAzure Blob Storage toocopy adatait. Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.   

Az alábbiakban hello adat-előállító összetevők, konfigurálnia kell toocreate tooimplement hello forgatókönyv. hello lista hello szakaszok ezeket a lépéseket részleteit tartalmazzák.

* Hello típusú társított szolgáltatás [Salesforce](#linked-service-properties)
* Hello típusú társított szolgáltatás [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
* Bemeneti [dataset](data-factory-create-datasets.md) hello típusú [RelationalTable](#dataset-properties)
* Egy kimeneti [dataset](data-factory-create-datasets.md) hello típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)

**A Salesforce társított szolgáltatás**

Ez a példa hello **Salesforce** társított szolgáltatás. Lásd: hello [Salesforce társított szolgáltatás](#linked-service-properties) szolgáltatásnak által támogatott hello tulajdonságok szakaszát.  Lásd: [biztonsági jogkivonatának beszerzéséhez](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) kapcsolatban, hogyan tooreset/get hello biztonsági jogkivonatot.

```json
{
    "name": "SalesforceLinkedService",
    "properties":
    {
        "type": "Salesforce",
        "typeProperties":
        {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```
**Azure Storage társított szolgáltatás**

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
**Bemeneti Salesforce-adatkészlet**

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"  
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Beállítás **külső** túl**igaz** hello Data Factory szolgáltatásnak tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

> [!IMPORTANT]
> hello API neve "__c" részét hello bármilyen egyéni objektum szükséges.

![Data Factory - kapcsolat Salesforce - API neve](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

**Azure blobkimeneti adatkészlet**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/alltypes_c"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**A másolási tevékenység folyamat**

hello csővezeték tartalmaz másolási tevékenység során beállított toouse hello bemeneti és kimeneti adatkészletek, és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource**, és hello **fogadó** típusuk értéke túl**BlobSink**.

Lásd: [RelationalSource típustulajdonságokat](#copy-activity-properties) hello hello RelationalSource által támogatott tulajdonságok listája.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce tooan Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "SalesforceInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"                
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }
        ]
    }
}
```
> [!IMPORTANT]
> hello API neve "__c" részét hello bármilyen egyéni objektum szükséges.

![Data Factory - kapcsolat Salesforce - API neve](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)


### <a name="type-mapping-for-salesforce"></a>A Salesforce leképezésének
| Salesforce-típus | . A NET-alapú típusa |
| --- | --- |
| Automatikus szám |Karakterlánc |
| Jelölőnégyzet |Logikai érték |
| Currency (Pénznem) |Dupla |
| Dátum |Dátum és idő |
| Dátum és idő |Dátum és idő |
| E-mail cím |Karakterlánc |
| Azonosító |Karakterlánc |
| Keresési kapcsolat |Karakterlánc |
| Többszörös kiválasztási lista |Karakterlánc |
| Szám |Dupla |
| Százalék |Dupla |
| Telefonszám |Karakterlánc |
| Választási lista |Karakterlánc |
| Szöveg |Karakterlánc |
| Szövegmező |Karakterlánc |
| Szövegmező (nagy) |Karakterlánc |
| Szövegmező (gazdag) |Karakterlánc |
| Szöveg (titkosítva) |Karakterlánc |
| URL-CÍME |Karakterlánc |

> [!NOTE]
> Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Teljesítmény és finomhangolás
Lásd: hello [másolási tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
