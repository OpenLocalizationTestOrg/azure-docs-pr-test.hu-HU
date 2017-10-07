---
title: "aaaMove adatok a PostgreSQL Azure Data Factory használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toomove adatokat az Azure Data Factory használatával PostgreSQL-adatbázisból."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 888d9ebc-2500-4071-b6d1-0f6bd1b5997c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: ea384f4e06f7d7bedae2949e4ea727c8f8806614
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a>Adatok áthelyezése az Azure Data Factory használatával PostgreSQL
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatokat a helyszíni PostgreSQL-adatbázishoz a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

Egy helyszíni PostgreSQL adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja. Adattároló mosdók hello másolási tevékenység által támogatott listájáért lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Adat-előállító jelenleg adatbázis támogatja az adatok áthelyezése egy PostgreSQL-tooother adattárolókhoz, de nem az adatok áthelyezését más adatok tooan PostgreSQL-adatbázisban tárolja. 

## <a name="prerequisites"></a>Előfeltételek

Data Factory szolgáltatásnak csatlakozó tooon helyszíni PostgreSQL adatforrások az adatkezelési átjáró hello segítségével támogatja. Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat.

Átjáróra szükség, akkor is, ha egy Azure IaaS virtuális gép hello PostgreSQL-adatbázisban tárolódik. Ugyanaz, infrastruktúra-szolgáltatási virtuális gép hello adatként tárolja, vagy egy másik virtuális gép mindaddig hello átjáró kapcsolódhatnak toohello adatbázis hello átjáró telepíthető.

> [!NOTE]
> Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.

## <a name="supported-versions-and-installation"></a>Támogatott verziók és telepítés
Az adatkezelési átjáró tooconnect toohello PostgreSQL-adatbázisból, telepítse a hello [PostgreSQL-Ngpsql adatszolgáltatója](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 vagy fent a hello azonos az adatkezelési átjáró hello rendszer. 7.4 verzió PostgreSQL és újabb verzió esetén támogatott.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, mely az adatok egy helyszíni PostgreSQL adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot. 

- hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást. 
- A következő eszközök toocreate adatcsatorna hello is használhatja: 
    - Azure Portal
    - Visual Studio
    - Azure PowerShell
    - Azure Resource Manager-sablon
    - .NET API
    - REST API

     Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek egy a helyszíni PostgreSQL adattároló használt toocopy adatait JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az PostgreSQL tooAzure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) című szakaszát. 

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooa PostgreSQL adattár részleteit tartalmazzák:

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
a következő táblázat hello biztosít JSON elemek adott tooPostgreSQL kapcsolódó szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **OnPremisesPostgreSql** |Igen |
| kiszolgáló |Hello PostgreSQL-kiszolgáló neve. |Igen |
| adatbázis |Hello PostgreSQL-adatbázis neve. |Igen |
| Séma |Hello séma hello adatbázis neve. hello sémanév a kis-és nagybetűket. |Nem |
| AuthenticationType |Tooconnect toohello PostgreSQL-adatbázishoz használt hitelesítés típusa. Lehetséges értékek a következők: névtelen, alapszintű és a Windows. |Igen |
| felhasználónév |Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata. |Nem |
| jelszó |Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót. |Nem |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello a helyszíni PostgreSQL-adatbázisból. |Igen |

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében.

hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. hello typeProperties szakasz típusú adatkészlet **RelationalTable** következő tulajdonságai hello (amely tartalmazza a PostgreSQL dataset) rendelkezik:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello tábla hello PostgreSQL-adatbázispéldányt, amely a társított szolgáltatás neve hivatkozik. hello táblanév a kis-és nagybetűket. |Nem (Ha **lekérdezés** a **RelationalSource** van megadva) |

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.

Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

Ha a forrás típusa van **RelationalSource** (amely tartalmazza a PostgreSQL), typeProperties szakaszában érhetők hello következő tulajdonságai:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: "query": "Válasszon * a \"MySchema\".\" MyTable\"". |Nem (Ha **tableName** a **dataset** van megadva) |

> [!NOTE]
> Séma-és tábla-és nagybetűk. Tegye őket `""` (idézőjelek) hello lekérdezésben.  

**Példa**

 `"query": "select * from \"MySchema\".\"MyTable\""`

## <a name="json-example-copy-data-from-postgresql-tooazure-blob"></a>JSON-példa: adatok másolása az PostgreSQL tooAzure Blob
Ebben a példában a minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azok megjelenítése, hogyan PostgreSQL toocopy adatait adatbázis tooAzure Blob Storage tárolóban. Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.   

> [!IMPORTANT]
> Ez a minta JSON kódtöredékek biztosít. Nem tartalmazza a hello adat-előállító létrehozásának részletes leírása. Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.

hello minta a következő data factory entitások hello rendelkezik:

1. A társított szolgáltatás típusa [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Hello [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másol adatokat egy lekérdezés eredményét PostgreSQL adatbázis tooa BLOB minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

Első lépésként a telepítő hello az adatkezelési átjáró. hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.

**PostgreSQL társított szolgáltatáshoz:**

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
**Az Azure Blob storage társított szolgáltatásnak:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
    }
    }
}
```
**PostgreSQL bemeneti adatkészlet:**

hello példa azt feltételezi, hogy létrehozott egy tábla "MyTable" PostgreSQL és egy "időbélyeg" nevű adatsorozat időadatok oszlopot tartalmaz.

Beállítás `"external": true` hello Data Factory szolgáltatásnak tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
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

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```json
{
    "name": "AzureBlobPostgreSqlDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/postgresql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság kiválasztja hello adatokat hello public.usstates táblából hello PostgreSQL-adatbázisban.

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"public\".\"usstates\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "PostgreSqlDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobPostgreSqlDataSet"
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
                "name": "PostgreSqlToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
## <a name="type-mapping-for-postgresql"></a>Típus identitástagok esetén PostgreSQL
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység az automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello hajtja végre:

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Adatok tooPostgreSQL áthelyezésekor hello következő megfeleltetéseket használtak PostgreSQL too.NET típusának.

| PostgreSQL-adatbázisból típusa | PostgresSQL aliasok | .NET-keretrendszer típusa |
| --- | --- | --- |
| abstime | |Dátum és idő | &nbsp;
| bigint |int8 |Int64 |
| bigserial |serial8 |Int64 |
| bit [(n)] | |Byte [], karakterlánc | &nbsp;
| bit különböző [(n)] |varbit |Byte [], karakterlánc |
| Logikai érték |logikai érték |Logikai érték |
| Mezőbe | |Byte [], karakterlánc |&nbsp;
| bytea | |Byte [], karakterlánc |&nbsp;
| [(n)] karakter |a char [(n)] |Karakterlánc |
| [(n)] eltérő karaktert |varchar [(n)] |Karakterlánc |
| CID | |Karakterlánc |&nbsp;
| CIDR | |Karakterlánc |&nbsp;
| kör | |Byte [], karakterlánc |&nbsp;
| Dátum | |Dátum és idő |&nbsp;
| DateRange | |Karakterlánc |&nbsp;
| a kétszeres pontosság |FLOAT8 |Dupla |
| INet | |Byte [], karakterlánc |&nbsp;
| intarry | |Karakterlánc |&nbsp;
| int4range | |Karakterlánc |&nbsp;
| int8range | |Karakterlánc |&nbsp;
| egész szám |int, int4 |Int32 |
| időköz [mezők] [(p)] | |Időtartomány |&nbsp;
| JSON-ban | |Karakterlánc |&nbsp;
| jsonb | |Byte] |&nbsp;
| sor | |Byte [], karakterlánc |&nbsp;
| lseg | |Byte [], karakterlánc |&nbsp;
| macaddr | |Byte [], karakterlánc |&nbsp;
| pénz | |Decimális |&nbsp;
| numerikus [(p, s)] |Decimal [(p, s)] |Decimális |
| numrange | |Karakterlánc |&nbsp;
| OID | |Int32 |&nbsp;
| Elérési út | |Byte [], karakterlánc |&nbsp;
| pg_lsn | |Int64 |&nbsp;
| pont | |Byte [], karakterlánc |&nbsp;
| Sokszög | |Byte [], karakterlánc |&nbsp;
| valós |float4 |Egyetlen |
| smallint |int2 |Int16 |
| smallserial |serial2 |Int16 |
| Soros |serial4 |Int32 |
| Szöveg | |Karakterlánc |&nbsp;

## <a name="map-source-toosink-columns"></a>A forrásoszlopokat toosink leképezése
toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>A relációs források ismételhető Olvasás
Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása. Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
