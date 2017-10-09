---
title: "Azure Data Factory használatával MySQL aaaMove adatait |} Microsoft Docs"
description: "További információk a hogyan toomove adatokat a MySQL-adatbázis használati ideje Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 452f4fce-9eb5-40a0-92f8-1e98691bea4c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: jingwang
ms.openlocfilehash: 3ffe969e42ce1a54b265c4739df43fdc594ea891
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-mysql-using-azure-data-factory"></a>Helyezze át az adatokat a MySQL Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatokat a helyszíni MySQL-adatbázis a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

Egy helyszíni MySQL adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja. Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. Adat-előállító jelenleg csak egy MySQL adatokból adattároló tooother adattárolókhoz áthelyezése, de nem adatok áthelyezését más adatok tárolók tooan MySQL adatok tárolására. 

## <a name="prerequisites"></a>Előfeltételek
Data Factory szolgáltatásnak csatlakozó tooon helyszíni MySQL adatforrások az adatkezelési átjáró hello segítségével támogatja. Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat.

Átjáróra szükség, akkor is, ha hello MySQL adatbázis egy Azure IaaS virtuális gépre (VM). Hello ugyanazon VM hello adatként tanúsítványtárolójában, vagy egy másik virtuális gép mindaddig hello átjáró kapcsolódhatnak toohello adatbázis hello átjáró telepíthető.

> [!NOTE]
> Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.

## <a name="supported-versions-and-installation"></a>Támogatott verziók és telepítés
Az adatkezelési átjáró tooconnect toohello MySQL-adatbázis, a tooinstall hello kell [MySQL összekötő/Net számára a Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (6.6.5 verzió vagy újabb) a hello ugyanaz az adatkezelési átjáró hello rendszer. 5.1-es verzió MySQL és az újabb verzió esetén támogatott.

> [!TIP]
> Ha kattintson a hiba "Hitelesítés sikertelen, mert hello távoli fél bezárta hello transport stream.", akkor fontolja meg a tooupgrade hello MySQL összekötő/Net toohigher verziója.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot. 

- hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást. 
- Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek egy helyszíni MySQL adattárolóból használt toocopy adatok JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az MySQL tooAzure Blob](#json-example-copy-data-from-mysql-to-azure-blob) című szakaszát. 

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooa MySQL adattár részleteit tartalmazzák:

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
a következő táblázat hello biztosít JSON elemek adott tooMySQL kapcsolódó szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **OnPremisesMySql** |Igen |
| kiszolgáló |Hello MySQL kiszolgáló neve. |Igen |
| adatbázis |Hello MySQL-adatbázis neve. |Igen |
| Séma |Hello séma hello adatbázis neve. |Nem |
| AuthenticationType |Tooconnect toohello MySQL-adatbázis használt hitelesítés típusa. Lehetséges értékek a következők: `Basic`. |Igen |
| felhasználónév |Adja meg a felhasználó nevét tooconnect toohello MySQL-adatbázis. |Igen |
| jelszó |Adja meg a megadott hello felhasználói fiók jelszavát. |Igen |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét kell tooconnect toohello helyszíni MySQL-adatbázis használata. |Igen |

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. hello typeProperties szakasz típusú adatkészlet **RelationalTable** következő tulajdonságai hello (amely tartalmazza a MySQL dataset) rendelkezik.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello tábla hello MySQL-adatbázis-példány, amelyre a társított szolgáltatás neve hivatkozik. |Nem (Ha **lekérdezés** a **RelationalSource** van megadva) |

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. A rendszer például név, leírás, a bemeneti és kimeneti táblák tulajdonságokat olyan szabályzatok állnak rendelkezésre az összes tevékenység.

Mivel tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

Ha a másolási tevékenység forrása típusa **RelationalSource** (amely tartalmazza a MySQL), a következő tulajdonságok hello typeProperties szakaszában érhetők:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: Válasszon * from tábla. |Nem (Ha **tableName** a **dataset** van megadva) |


## <a name="json-example-copy-data-from-mysql-tooazure-blob"></a>JSON-példa: adatok másolása az MySQL tooAzure Blob
Ebben a példában a minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azt illusztrálja, hogyan toocopy adatait egy helyszíni MySQL-adatbázis használati ideje tooan Azure Blob Storage tárolóban. Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.

> [!IMPORTANT]
> Ez a minta JSON kódtöredékek biztosít. Nem tartalmazza a hello adat-előállító létrehozásának részletes leírása. Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.

hello minta a következő data factory entitások hello rendelkezik:

1. A társított szolgáltatás típusa [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másol adatokat a MySQL adatbázis tooa blob egy lekérdezés eredményét óránként. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

Első lépésként a telepítő hello az adatkezelési átjáró. hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.

**MySQL társított szolgáltatáshoz:**

```JSON
    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
        }
      }
    }
```

**Az Azure tárolás társított szolgáltatásának:**

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

**MySQL bemeneti adatkészlet:**

hello példa azt feltételezi, hogy létrehozott egy tábla "MyTable" MySQL és egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.

"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello tábla külső toohello adat-előállító, ezért nem hozzák hello adat-előállítóban tevékenységgel.

```JSON
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
            "typeProperties": {},
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

**Az Azure Blob kimeneti adatkészlet:**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```JSON
    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**A másolási tevékenység során a következő feldolgozási sorban:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.

```JSON
    {
        "name": "CopyMySqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }
```


### <a name="type-mapping-for-mysql"></a>A MySQL leképezésének
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello:

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Adatok tooMySQL áthelyezésekor hello következő megfeleltetéseket használtak MySQL típusok too.NET típusok.

| A MySQL-adatbázis típusa | .NET-keretrendszer típusa |
| --- | --- |
| aláíratlan bigint |Decimális |
| bigint |Int64 |
| bit |Decimális |
| A BLOB |Byte] |
| logikai érték |Logikai érték |
| Karakter |Karakterlánc |
| Dátum |Dátum és idő |
| Dátum és idő |Dátum és idő |
| Decimális |Decimális |
| a kétszeres pontosság |Dupla |
| Dupla |Dupla |
| Enum |Karakterlánc |
| Lebegőpontos |Egyetlen |
| aláíratlan int |Int64 |
| int |Int32 |
| aláíratlan egész szám |Int64 |
| egész szám |Int32 |
| hosszú varbinary |Byte] |
| hosszú varchar |Karakterlánc |
| longblob |Byte] |
| LONGTEXT |Karakterlánc |
| mediumblob |Byte] |
| aláíratlan mediumint |Int64 |
| mediumint |Int32 |
| mediumtext |Karakterlánc |
| Numerikus |Decimális |
| valós |Dupla |
| Állítsa be |Karakterlánc |
| aláíratlan smallint |Int32 |
| smallint |Int16 |
| Szöveg |Karakterlánc |
| time |A TimeSpan |
| időbélyeg |Dátum és idő |
| tinyblob |Byte] |
| aláíratlan tinyint |Int16 |
| tinyint |Int16 |
| tinytext |Karakterlánc |
| varchar |Karakterlánc |
| Év |int |

## <a name="map-source-toosink-columns"></a>A forrásoszlopokat toosink leképezése
toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>A relációs források ismételhető Olvasás
Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása. Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
