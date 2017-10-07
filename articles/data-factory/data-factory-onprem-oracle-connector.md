---
title: "az Oracle Data Factory használatával aaaCopy adatok |} Microsoft Docs"
description: "Ismerje meg, hogyan toocopy való/Oracle-adatbázis adatait, amely a helyszíni Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: adb6d5fbe38e18791616ac77e8179970bbea37fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Adatok másolása az Azure Data Factory használatával a helyszíni Oracle és a
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatok helyszíni Oracle-adatbázishoz és a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

## <a name="supported-scenarios"></a>Támogatott helyzetek
Adatokat másolhat **az Oracle-adatbázishoz** toohello a következő adatokat tárolja:

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

Adatok másolása a következő adatokat tárolja hello **tooan Oracle-adatbázishoz**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a>Előfeltételek
Adat-előállító csatlakozó tooon helyszíni Oracle adatforrások az adatkezelési átjáró hello segítségével támogatja. Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) az adatkezelési átjáró kapcsolatos cikk toolearn és [tárolt adatok mozgatása a helyszíni toocloud](data-factory-move-data-between-onprem-and-cloud.md) cikk lépésenkénti adatok adatcsatorna hello átjáró beállítása toomove adatokat.

Átjáróra szükség, akkor is, ha hello Oracle van tárolva az Azure infrastruktúra-szolgáltatási virtuális gép. Ugyanaz, infrastruktúra-szolgáltatási virtuális gép hello adatként tárolja, vagy egy másik virtuális gép mindaddig hello átjáró kapcsolódhatnak toohello adatbázis hello hello átjáró telepíthető.

> [!NOTE]
> Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.

## <a name="supported-versions-and-installation"></a>Támogatott verziók és telepítés
Az Oracle-összekötő illesztőprogramok két verziója támogatja:

- **Microsoft-illesztőprogram (ajánlott) Oracle**: kiindulva az adatkezelési átjáró verziója 2.7, a Microsoft illesztőprogram hello átjáró együtt automatikusan telepíti a Oracle, így nem kell tooadditionally leíró hello illesztőprogram sorrendben tooestablish kapcsolat tooOracle, és is tapasztalhat, az illesztőprogram jobb másolási teljesítményt. Oracle verziói alatti adatbázisok támogatottak:
    - Oracle 12c R1 (12.1)
    - Oracle 11g R1 vagy R2 (11.1, 11.2)
    - Oracle 10g R1 vagy R2 (10.1, 10,2)
    - Oracle 9i R1 vagy R2 (9.0.1, 9.2)
    - Oracle 8i R3 (8.1.7-es)

> [!IMPORTANT]
> Jelenleg Microsoft Oracle-illesztőprogram csak az adatok másolását a Oracle, de nincs írás tooOracle támogatja. És megjegyzés hello teszt kapcsolat funkció adatok felügyeleti átjáró Diagnosztika lap nem támogatja az illesztőprogramot. Másik lehetőségként hello másolása varázsló toovalidate hello kapcsolatot is használhatja.
>

- **.NET-keretrendszerhez készült Oracle-adatszolgáltatóban:** toouse Oracle-adatszolgáltatóban toocopy adatait is beállíthatja / tooOracle. Ez az összetevő megtalálható [Oracle Data Access Windows összetevők](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Telepítse a hello megfelelő verziója (32 vagy 64 bit) hello hello átjárót futtató gépen. [Oracle-adatszolgáltatóban .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) tooOracle Database 10 g, 2 vagy újabb kiadás hozzáférhet.

    Ha úgy dönt, hogy a "XCopy telepítés", hajtsa végre a hello readme.htm. Azt javasoljuk, hogy úgy dönt, hogy hello telepítőjét a felhasználói felület (nem-XCopy egy).

    Hello szolgáltató telepítése után **indítsa újra a** hello az adatkezelési átjáró gazdaszolgáltatás a gépen a szolgáltatások kisalkalmazásával (vagy) az adatkezelési átjáró konfigurációkezelőjének használatával.  

Ha másolása varázsló tooauthor hello másolási folyamat használja, a hello illesztőprogram típus lesz automatikus határozza meg. Microsoft illesztőprogram által használható alapértelmezett, kivéve, ha az átjáró verziója alacsonyabb, mint 2.7, vagy ha úgy dönt, Oracle, a fogadó.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, amely helyezi át az adatokat a helyszíni Oracle-adatbázishoz és a különböző eszközök/API-k használatával létrehozhat egy folyamatot.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre egy **adat-előállító**. Egy adat-előállító tartalmazhat egy vagy több folyamatok. 
2. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban. Adatok másolása az egy Oralce adatbázis tooan Azure blob Storage tárolóban, akkor hozzon létre például két összekapcsolt szolgáltatások toolink az Oracle-adatbázishoz és az Azure storage tooyour adat-előállítóban. Csatolt szolgáltatás tulajdonságait, amelyek adott tooOracle, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.
3. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. Hello utolsó lépésében említett hello példában az Oracle-adatbázis hello bemeneti adatokat tartalmazó hoz létre egy adatkészlet toospecify hello tábla. Egy másik dataset toospecify hello blob tároló létrehozása és a hello adatokat tartalmazó hello mappába másolta át hello Oracle-adatbázishoz. Adatkészlet tulajdonságai, amelyek adott tooOracle, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.
4. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. A korábban említett hello példában OracleSource forrás-és BlobSink akár használhatja a fogadó hello másolási tevékenységhez. Ehhez hasonlóan az Azure Blob Storage tooOracle adatbázis másolása, használható BlobSource és OracleSink hello másolási tevékenység. Másolási tevékenység tulajdonságok adott tooOracle adatbázis esetében, tekintse meg a [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz. További információkért hogyan toouse egy adatok tárolót, mint a forrás- és a fogadó hivatkozásra hello az adattároló hello előző szakaszban. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek az egy helyszíni Oracle-adatbázishoz használt toocopy adatok JSON-definíciók minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-oracle-database) című szakaszát.

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások részleteit tartalmazzák:

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
a következő táblázat hello biztosít JSON elemek adott tooOracle kapcsolódó szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **OnPremisesOracle** |Igen |
| driverType | Adja meg, mely illesztőprogram toouse toocopy adatait / tooOracle adatbázis. Két érték engedélyezett **Microsoft** vagy **ODP** (alapértelmezett). Lásd: [verziójától és a telepítés támogatott](#supported-versions-and-installation) illesztőprogram adatai szakaszban. | Nem |
| connectionString | Adjon meg információt tooconnect toohello Oracle adatbázispéldány hello connectionString tulajdonság szükséges. | Igen |
| gatewayName | Hello átjáró, amely használt tooconnect toohello helyszíni Oracle-kiszolgáló neve |Igen |

**Példa: Microsoft-illesztőprogramot használ:**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Példa: ODP illesztőprogramot használja.**

Tekintse meg a túl[ezen a helyen](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) az engedélyezett formátumokat hello.

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Oracle, az Azure blob, Azure-tábla, stb.).

hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. hello typeProperties szakasz hello adatkészlet OracleTable típus következő tulajdonságai hello rendelkezik:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Oracle-adatbázishoz kapcsolódó szolgáltatás hello hello hello tábla neve hivatkozik. |Nem (Ha **oracleReaderQuery** a **OracleSource** van megadva) |

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.

> [!NOTE]
> hello másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.

Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

### <a name="oraclesource"></a>OracleSource
A másolási tevékenység, ha hello adatforrás típusú **OracleSource** hello a következő tulajdonságok érhetők el **typeProperties** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| oracleReaderQuery |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: Válasszon * from tábla <br/><br/>Ha nincs megadva, az SQL-utasítás végrehajtása hello: Válasszon * from tábla |Nem (Ha **tableName** a **dataset** van megadva) |

### <a name="oraclesink"></a>OracleSink
**OracleSink** következő tulajdonságai hello támogatja:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| writeBatchTimeout |Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot. |A TimeSpan<br/><br/> . Példa: 00:30:00 (30 perc). |Nem |
| WriteBatchSize |Amikor hello puffer mérete eléri writeBatchSize adatok beillesztése hello SQL táblázat. |Egész szám (sorok száma) |Nem (alapértelmezett: 100) |
| sqlWriterCleanupScript |Adja meg a másolási tevékenység tooexecute vonatkozó lekérdezést úgy, hogy egy adott szelet adatait. |A lekérdezési utasítást. |Nem |
| sliceIdentifierColumnName |Adja meg, a másolási tevékenység toofill oszlopnév automatikusan létrejön szelet azonosítóval, amely adatokat egy adott szelet, amikor futtassa újra a használt tooclean. |Egy oszlop binary(32) adattípusú oszlop neve. |Nem |

## <a name="json-examples-for-copying-data-tooand-from-oracle-database"></a>JSON Példák adatok tooand másolását Oracle-adatbázishoz
hello alábbi példa minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azok hogyan toocopy adatait / tooan Oracle adatbázis, az Azure Blob Storage tárolóban. Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.   

## <a name="example-copy-data-from-oracle-tooazure-blob"></a>Példa: Adatok másolása az Oracle tooAzure Blob

hello minta a következő data factory entitások hello rendelkezik:

1. A társított szolgáltatás típusa [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) forrásként és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) mint fogadó.

hello minta másol adatokat egy helyszíni Oracle adatbázis tooa blob táblához óránként. További információ a különböző hello minta használt tulajdonságok hello mintát a következő szakaszok dokumentációjában olvasható.

**Oracle kapcsolódó szolgáltatás:**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Az Azure Blob storage társított szolgáltatásnak:**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Oracle bemeneti adatkészlet:**

hello példa azt feltételezi, hogy létrehozott egy tábla "MyTable" Oracle és egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.

"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2014-02-27T12:00:00",
            "frequency": "Hour"
        },
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

**Az Azure Blob kimeneti adatkészlet:**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": "\t",
                "rowDelimiter": "\n"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**A másolási tevékenység során a következő feldolgozási sorban:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**OracleSource** és **fogadó** típusuk értéke túl**BlobSink**.  hello SQL-lekérdezésben megadott **oracleReaderQuery** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "OracletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": " OracleInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "OracleSource",
                        "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

## <a name="example-copy-data-from-azure-blob-toooracle"></a>Példa: Adatok másolása az Azure Blob tooOracle
Ez a példa bemutatja, hogyan toocopy adatait az Azure Blob Storage tooan helyszíni Oracle-adatbázishoz. Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott hello források [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.  

hello minta a következő data factory entitások hello rendelkezik:

1. A társított szolgáltatás típusa [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) forrásaként [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) mint fogadó.

hello minta másol adatokat a helyszíni Oracle-adatbázisban egy blob tooa tábla óránként. További információ a különböző hello minta használt tulajdonságok hello mintát a következő szakaszok dokumentációjában olvasható.

**Oracle kapcsolódó szolgáltatás:**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Az Azure Blob storage társított szolgáltatásnak:**
```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Az Azure Blob bemeneti adatkészlet**

Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján. hello mappa elérési útját használja év, hónap és nap részét hello kezdési ideje, valamint fájlnév hello kezdő időpontja óra részét hello. "external": "true" beállítás arról értesíti az, hogy ez a táblázat külső toohello adat-előállító és hello adat-előállítóban tevékenység nem hozzák hello Data Factory szolgáltatásnak.

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
                }
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
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

**Oracle kimeneti adatkészlet:**

hello példa feltételezi, hogy létrehozott egy "MyTable" táblát az Oracle. Hello tábla létrehozása az Oracle azonos számú oszlopot hello hello Blob CSV-fájl toocontain várt. Új sorok hozzáadásakor toohello tábla óránként.

```json
{
    "name": "OracleOutput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Day",
            "interval": "1"
        }
    }
}
```

**A másolási tevékenység során a következő feldolgozási sorban:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**BlobSource** és hello **fogadó** típusuk értéke túl**OracleSink**.  

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
            {
                "name": "AzureBlobtoOracle",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "OracleOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "OracleSink"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
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


## <a name="troubleshooting-tips"></a>Hibaelhárítási tippek
### <a name="problem-1-net-framework-data-provider"></a>1. hiba: A .NET-keretrendszer adatszolgáltatója

Lásd a következő hello **hibaüzenet**:

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable toofind hello requested .Net Framework Data Provider. It may not be installed”.  

**Lehetséges okok:**

1. .NET Framework Data Provider – Oracle hello nem lett telepítve.
2. .NET Framework Data Provider – Oracle hello telepített too.NET keretrendszer 2.0-s volt, és nem található a .NET Framework 4.0 hello mappákban.

**Megoldás vagy megoldás:**

1. Ha nem telepített .NET-szolgáltató az Oracle, hello [telepítse](http://www.oracle.com/technetwork/topics/dotnet/downloads/) , majd próbálja megismételni a hello forgatókönyv.
2. Ha hibaüzenet hello hello szolgáltató telepítése után is, hello lépéseket követve:
   1. Nyissa meg a .NET 2.0 gép config hello mappából: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
   2. Keresse meg **.NET-keretrendszerhez készült Oracle-adatszolgáltatóban**, és képes toofind bejegyzést kell lennie, ahogy az a következő minta alapján hello **system.data** -> **DbProviderFactories**: "<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description=".NET oracle-adatszolgáltatója" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
3. A bejegyzés toohello machine.config fájl másolása a következő v4.0 mappa hello: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config és módosítási hello verzió too4.xxx.x.x.
4. "< ODP.NET telepített elérési útja > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" hello globális szerelvény-gyorsítótárban (GAC) futtatásával telepítse `gacutil /i [provider path]`. ## hibaelhárítási tippek

### <a name="problem-2-datetime-formatting"></a>2. hiba: dátum és idő formázása

Lásd a következő hello **hibaüzenet**:

    Message=Operation failed in Oracle Database with hello following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

**Megoldás vagy megoldás:**

A másolási tevékenység alapján dátumok hogyan vannak konfigurálva az Oracle-adatbázishoz, ahogy az alábbi hello esetleg tooadjust hello lekérdezési karakterlánc minta (hello to_date függvény használatával):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a>Oracle típusú leképezése
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység az automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello hajtja végre:

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Ha adatok áthelyezése az Oracle, leképezéseket a következő hello használ az Oracle típusú too.NET típusánál, és ez fordítva is igaz.

| Oracle-adattípusra | .NET-keretrendszer adattípus |
| --- | --- |
| BFÁJL |Byte] |
| A BLOB |Byte] |
| KARAKTER |Karakterlánc |
| CLOB |Karakterlánc |
| DÁTUM |Dátum és idő |
| LEBEGŐPONTOS |Decimális, karakterlánc (Ha pontosság > 28) |
| EGÉSZ SZÁM |Decimális, karakterlánc (Ha pontosság > 28) |
| IDŐKÖZ év tooMONTH |Int32 |
| IDŐKÖZ nap tooSECOND |A TimeSpan |
| HOSSZÚ |Karakterlánc |
| HOSSZÚ NYERS |Byte] |
| NCHAR |Karakterlánc |
| NCLOB |Karakterlánc |
| SZÁM |Decimális, karakterlánc (Ha pontosság > 28) |
| NVARCHAR2 |Karakterlánc |
| NYERS |Byte] |
| ROWID |Karakterlánc |
| IDŐBÉLYEG |Dátum és idő |
| A HELYI IDŐZÓNÁRA IDŐBÉLYEG |Dátum és idő |
| AZ IDŐZÓNA IDŐBÉLYEG |Dátum és idő |
| ELŐJEL NÉLKÜLI EGÉSZKÉNT. |Szám |
| VARCHAR2 |Karakterlánc |
| XML |Karakterlánc |

> [!NOTE]
> Adattípus **IDŐKÖZ év tooMONTH** és **IDŐKÖZ nap tooSECOND** Microsoft illesztőprogram használata esetén nem támogatottak.

## <a name="map-source-toosink-columns"></a>A forrásoszlopokat toosink leképezése
toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>A relációs források ismételhető Olvasás
Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása. Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
