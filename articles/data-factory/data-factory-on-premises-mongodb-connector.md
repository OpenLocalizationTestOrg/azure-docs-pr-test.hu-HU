---
title: "használatával a Data Factory MongoDB aaaMove adatait |} Microsoft Docs"
description: "További információk a hogyan toomove adatait MongoDB adatbázis-Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 10ca7d9a-7715-4446-bf59-2d2876584550
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: 154e85712f27b978976c7499c43dde9429f124c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-mongodb-using-azure-data-factory"></a>Helyezze át az adatokat a MongoDB Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove az adatbázisból a helyszíni MongoDB a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

Egy helyszíni MongoDB adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja. Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. Adat-előállító jelenleg csak a MongoDB-adatokból adattároló tooother adattárolókhoz áthelyezése, de nem adatok áthelyezését más adatokat tároló tooan MongoDB adattárolót. 

## <a name="prerequisites"></a>Előfeltételek
Hello Azure Data Factory szolgáltatás toobe képes tooconnect tooyour helyszíni MongoDB adatbázis telepítenie kell a következő összetevők hello:

- MongoDB-verziók a következők: 2.4, 2.6, 3.0-s és 3.2-es verzióját.
- Az adatkezelési átjáró hello ugyanaz a gép állomások hello adatbázis vagy egy másik számítógépre tooavoid hello adatbázis erőforrás használják. Az adatkezelési átjáró egy szoftver, amely összeköti a helyszíni adatok források toocloud szolgáltatások biztonságának és kezelésének módja. Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) szóló cikkben olvashat az adatkezelési átjáró. Lásd: [tárolt adatok mozgatása a helyszíni toocloud](data-factory-move-data-between-onprem-and-cloud.md) cikk hello átjáró egy adatok adatcsatorna toomove adatok beállításának lépéseit.

    Hello átjáró telepítésekor automatikusan telepíti a Microsoft MongoDB ODBC-illesztőprogram használt tooconnect tooMongoDB.

    > [!NOTE]
    > Akkor is, ha az Azure IaaS virtuális gépeken fut. toouse hello átjáró tooconnect tooMongoDB kell. Ha a felhőben üzemeltetett MongoDB tooconnect tooan példányának próbált, hello infrastruktúra-szolgáltatási virtuális gép is hello átjárópéldány is telepítheti.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, mely az adatok egy helyszíni MongoDB adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello: 

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek egy helyszíni MongoDB adattárolóból használt toocopy adatok JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az MongoDB tooAzure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) című szakaszát. 

a következő szakaszok hello használt toodefine adat-előállító entitások adott tooMongoDB forrás JSON-tulajdonághoz részleteit tartalmazzák:

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
hello alábbi táblázatban megadott JSON-elemek leírása túl**OnPremisesMongoDB** társított szolgáltatás.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **OnPremisesMongoDb** |Igen |
| kiszolgáló |Kiszolgáló IP-címét vagy állomásnevét kiszolgálónevét hello MongoDB. |Igen |
| port |TCP-portot, amely a MongoDB-kiszolgálóhoz hello toolisten ügyfél-kommunikációhoz használ. |Nem kötelező, alapértelmezett érték: 27017 |
| AuthenticationType |Alapszintű, vagy névtelen. |Igen |
| felhasználónév |Felhasználói fiók MongoDB tooaccess. |Igen (Ha alapszintű hitelesítést használ). |
| jelszó |Hello felhasználó jelszavát. |Igen (Ha alapszintű hitelesítést használ). |
| authSource |Hello MongoDB-adatbázist, amelyet toouse toocheck a hitelesítő adatok hitelesítéshez neve. |Választható (Ha alapszintű hitelesítést használ). alapértelmezett: hello rendszergazdai fiókot és a databaseName tulajdonsággal megadott hello adatbázis használ. |
| DatabaseName |Hello MongoDB-adatbázist, amelyet az tooaccess neve. |Igen |
| gatewayName |Hello adattár hozzáférő hello átjáró neve. |Igen |
| encryptedCredential |A hitelesítőadat-átjáró által titkosított. |Optional |

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. hello typeProperties szakasz típusú adatkészlet **MongoDbCollection** rendelkezik hello következő tulajdonságai:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| CollectionName |A MongoDB adatbázis hello gyűjtemény nevét. |Igen |

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.

Tulajdonságok érhetők el hello **typeProperties** szakasz hello hello tevékenységekre ugyanakkor tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

Ha hello forrás típusa nem **MongoDbSource** typeProperties szakaszában érhetők hello következő tulajdonságai:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-92 lekérdezési karakterlánc. Például: Válasszon * from tábla. |Nem (Ha **collectionName** a **dataset** van megadva) |



## <a name="json-example-copy-data-from-mongodb-tooazure-blob"></a>JSON-példa: adatok másolása az MongoDB tooAzure Blob
Ebben a példában a minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azt illusztrálja, hogyan egy helyszíni MongoDB tooan Azure Blob Storage toocopy adatait. Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.

hello minta a következő data factory entitások hello rendelkezik:

1. A társított szolgáltatás típusa [OnPremisesMongoDb](#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [MongoDbCollection](#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [MongoDbSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másol adatokat egy lekérdezés eredményét a MongoDB adatbázis tooa blob minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

Első lépésként, a telepítő hello az adatkezelési átjáró szerint hello hello utasításait [az adatkezelési átjáró](data-factory-data-management-gateway.md) cikk.

**MongoDB társított szolgáltatáshoz:**

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties":
    {
        "type": "OnPremisesMongoDb",
        "typeProperties":
        {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< hello IP address or host name of hello MongoDB server >",  
            "port": "<hello number of hello TCP port that hello MongoDB server uses toolisten for client connections.>",
            "username": "<username>",
            "password": "<password>",
           "authSource": "< hello database that you want toouse toocheck your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<mygateway>"
        }
    }
}
```

**Az Azure tárolás társított szolgáltatásának:**

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

**MongoDB bemeneti adatkészlet:** "external" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello tábla külső toohello adat-előállító, ezért nem hozzák hello adat-előállítóban tevékenységgel.

```json
{
     "name":  "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"    
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

**Az Azure Blob kimeneti adatkészlet:**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Másolási tevékenység a MongoDB-forrás és a Blob fogadó egy folyamaton belül:**

hello folyamat, amely a bemeneti és kimeneti adatkészletek fent konfigurált toouse hello és ütemezett toorun óránként másolatot tevékenységet tartalmaz. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**MongoDbSource** és **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "MongoDbSource",
                        "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "MongoDbInputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "MongoDBToAzureBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```


## <a name="schema-by-data-factory"></a>Adat-előállító sémája
Az Azure Data Factory szolgáltatásnak kikövetkezteti a MongoDB-gyűjteményből séma hello legújabb 100 dokumentumok hello gyűjtemény használatával. A 100 dokumentumok teljes séma nem tartalmaznak, ha azokat az oszlopokat hello másolási művelet során előfordulhat, hogy figyelembe véve.

## <a name="type-mapping-for-mongodb"></a>Mongodb-protokolltámogatással leképezésének
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello:

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Amikor helyezi át a következő adatok tooMongoDB hello hozzárendelések MongoDB típusok too.NET típusok használtak.

| MongoDB-típus | .NET-keretrendszer típusa |
| --- | --- |
| Bináris |Byte] |
| Logikai érték |Logikai érték |
| Dátum |Dátum és idő |
| NumberDouble |Dupla |
| NumberInt |Int32 |
| NumberLong |Int64 |
| Objektumazonosító |Karakterlánc |
| Karakterlánc |Karakterlánc |
| UUID |GUID |
| Objektum |Renormalized történő egybesimítására "_" beágyazott elválasztójelként oszlopok |

> [!NOTE]
> virtuális táblák, tömbök-támogatással kapcsolatos toolearn tekintse meg a túl[támogatja az összetett típusok virtuális táblák használata](#support-for-complex-types-using-virtual-tables) az alábbi szakasz.

Jelenleg nem támogatottak a következő MongoDB adattípusok hello: DBPointer, JavaScript, Max percenkénti kulcs, reguláris kifejezés, szimbólum, Timestamp, meghatározatlan

## <a name="support-for-complex-types-using-virtual-tables"></a>Virtuális táblák használata összetett típusok támogatása
Az Azure Data Factory egy beépített ODBC illesztőprogram tooconnect tooand példány adatait használja a MongoDB-adatbázist. Hello dokumentumok között különböző típusú tömb, vagy objektumok például összetett típusok hello illesztőprogram újra normalizálja adatok megfelelő virtuális táblákba. Pontosabban Ha a tábla tartalmaz ilyen oszlopok, hello illesztőprogram állít elő, a következő virtuális táblák hello:

* A **alaptábla**, amely tartalmazza hello hello valós táblából hello összetett típusú oszlopok ugyanazokat az adatokat. hello alaptábla hello ugyanazt a nevet használja, mint hello valós táblázatot, amely azt jelöli.
* A **virtuális tábla** minden összetett típusú oszlophoz, amely kiterjeszti hello beágyazott adatok. hello virtuális táblák megnevezett hello valós tábla hello neve, "_" elválasztó és hello tömb vagy objektum hello neve.

Virtuális táblák tekintse meg a hello valós tábla toohello adatokat, lehetővé teszi hello illesztőprogram tooaccess hello nem normalizált adatok. Részletekért lásd: Példa szakaszban olvashatók. MongoDB-tömbök hello tartalmának lekérdezése és hello virtuális tábla érheti el.

Használhatja a hello [másolása varázsló](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively nézet hello hello virtuális táblázatot is MongoDB-adatbázist a táblák listáját és hello található adatok megtekintésére. Is hello másolása varázsló az olyan lekérdezést, és toosee hello eredmény ellenőrzése.

### <a name="example"></a>Példa
"ExampleTable" alatt például MongoDB tábla egy objektumokból álló tömb egy oszlopot az egyes cellák – számlákat és a skaláris típusok – minősítések tömbje egy oszlop.

| _id | Ügyfél neve | Számlák | Szolgáltatási szint | Minősítések |
| --- | --- | --- | --- | --- |
| 1111 |ABC |[{invoice_id: "123", cikk: "toaster", az ár: "456" kedvezményes: "0,2"}, {invoice_id: "124" elem: "helyezzük", az ár: "1235" kedvezményes: "0,2"}] |Ezüst |[5,6] |
| 2222 |XYZ |[{invoice_id: "135", cikk: "kombinált hűtőszekrények", az ár: "12543" kedvezménnyel: "0,0"}] |Arany |[1,2] |

hello illesztőprogram több virtuális táblák toorepresent hoz létre a egyetlen tábla. első virtuális tábla hello hello alaptábla "ExampleTable" alább látható nevű. hello alaptábla hello eredeti tábla összes hello adatokat tartalmaz, de hello tömbök hello adatait kimaradt, és ki van bontva hello virtuális táblában.

| _id | Ügyfél neve | Szolgáltatási szint |
| --- | --- | --- |
| 1111 |ABC |Ezüst |
| 2222 |XYZ |Arany |

hello táblázatokban hello virtuális táblák megjelenítése, amelyek megfelelnek az eredeti tömbök hello hello példában. Ezek a táblázatok hello alábbiakat tartalmazza:

* Hivatkozás biztonsági toohello eredeti elsődleges kulcsként megadott oszlop megfelelő toohello hello eredeti tömb sora (keresztül hello _id oszlop)
* Utalhat, hogy hello adatok hello eredeti tömbön belüli hello pozíciója
* hello kibontva az egyes elemekhez hello tömbön belüli adatok

"ExampleTable_Invoices". tábla:

| _id | ExampleTable_Invoices_dim1_idx | invoice_id | Elem | price | Kedvezmény |
| --- | --- | --- | --- | --- | --- |
| 1111 |0 |123 |a toaster |456 |0.2 |
| 1111 |1 |124 |Helyezzük |1235 |0.2 |
| 2222 |0 |135 |kombinált hűtőszekrények |12543 |0.0 |

"ExampleTable_Ratings". tábla:

| _id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings |
| --- | --- | --- |
| 1111 |0 |5 |
| 1111 |1 |6 |
| 2222 |0 |1 |
| 2222 |1 |2 |

## <a name="map-source-toosink-columns"></a>A forrásoszlopokat toosink leképezése
toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>A relációs források ismételhető Olvasás
Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása. Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.

## <a name="next-steps"></a>Következő lépések
Lásd: [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) cikk részletes utasításokat az adatok folyamat létrehozása, amely áthelyezi adatait egy a helyszíni adatok tooan az Azure data tárolni.
