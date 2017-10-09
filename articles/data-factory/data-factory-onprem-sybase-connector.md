---
title: "Azure Data Factory használatával Sybase aaaMove adatait |} Microsoft Docs"
description: "Megtudhatja, hogyan toomove adatokat az Azure Data Factory használatával Sybase-adatbázisból."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: b379ee10-0ff5-4974-8c87-c95f82f1c5c6
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: ad003ec502028d56db9570fe08af329eb5b71817
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sybase-using-azure-data-factory"></a>Adatok áthelyezése az Azure Data Factory használatával Sybase
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatokat a helyszíni Sybase-adatbázishoz a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

Egy helyszíni Sybase adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja. Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. Adat-előállító jelenleg csak egy Sybase-adatokból adattároló tooother adattárolókhoz áthelyezése, de nem adatok áthelyezését más adatok tárolók tooa Sybase adatok tárolására. 

## <a name="prerequisites"></a>Előfeltételek
Data Factory szolgáltatásnak csatlakozó tooon helyszíni Sybase adatforrások az adatkezelési átjáró hello segítségével támogatja. Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat.

Átjáróra szükség, akkor is, ha egy Azure IaaS virtuális gépen található hello Sybase-adatbázishoz. Ugyanaz, infrastruktúra-szolgáltatási virtuális gép hello adatként tárolja, vagy egy másik virtuális gép mindaddig hello átjáró kapcsolódhatnak toohello adatbázis hello hello átjáró telepíthető.

> [!NOTE]
> Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.

## <a name="supported-versions-and-installation"></a>Támogatott verziók és telepítés
Az adatkezelési átjáró tooconnect toohello Sybase-adatbázishoz, a tooinstall hello kell [Sybase iAnywhere.Data.SQLAnywhere rendszerhez készült adatszolgáltatója](http://go.microsoft.com/fwlink/?linkid=324846) 16 vagy fent a hello ugyanaz az adatkezelési átjáró hello rendszer. Sybase verzió 16 és újabb verzió esetén támogatott.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot. 

- hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást. 
- Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek egy helyszíni Sybase adattároló használt toocopy adatait JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Sybase tooAzure Blob](#json-example-copy-data-from-sybase-to-azure-blob) című szakaszát. 

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooa Sybase adattár részleteit tartalmazzák:

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
a következő táblázat hello biztosít JSON elemek adott tooSybase kapcsolódó szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **OnPremisesSybase** |Igen |
| kiszolgáló |Hello Sybase-kiszolgáló neve. |Igen |
| adatbázis |Hello Sybase-adatbázis neve. |Igen |
| Séma |Hello séma hello adatbázis neve. |Nem |
| AuthenticationType |Tooconnect toohello Sybase-adatbázishoz használt hitelesítés típusa. Lehetséges értékek a következők: névtelen, alapszintű és a Windows. |Igen |
| felhasználónév |Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata. |Nem |
| jelszó |Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót. |Nem |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello helyszíni Sybase-adatbázishoz. |Igen |

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. Hello **typeProperties** típusú adatkészlet szakasz **RelationalTable** következő tulajdonságai hello (amely tartalmazza a Sybase dataset) rendelkezik:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello tábla hello Sybase-adatbázispéldányt, amely a társított szolgáltatás neve hivatkozik. |Nem (Ha **lekérdezés** a **RelationalSource** van megadva) |

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.

Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

Ha hello forrás típusa nem **RelationalSource** (amely tartalmazza a Sybase), a következő tulajdonságok hello érhetők el **typeProperties** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: Válasszon * from tábla. |Nem (Ha **tableName** a **dataset** van megadva) |


## <a name="json-example-copy-data-from-sybase-tooazure-blob"></a>JSON-példa: adatok másolása az Sybase tooAzure Blob
hello alábbi példa minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azok megjelenítése, hogyan Sybase toocopy adatait adatbázis tooAzure Blob Storage tárolóban. Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.   

hello minta a következő data factory entitások hello rendelkezik:

1. A társított szolgáltatás típusa [OnPremisesSybase](data-factory-onprem-sybase-connector.md#linked-service-properties).
2. A következő típusú liked szolgáltatást [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](data-factory-onprem-sybase-connector.md#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Hello [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](data-factory-onprem-sybase-connector.md#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másol adatokat egy lekérdezés eredményét Sybase adatbázis tooa BLOB minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

Első lépésként a telepítő hello az adatkezelési átjáró. hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.

**Sybase társított szolgáltatáshoz:**

```JSON
{
    "name": "OnPremSybaseLinkedService",
    "properties": {
        "type": "OnPremisesSybase",
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

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorageLinkedService",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```

**Sybase bemeneti adatkészlet:**

hello példa azt feltételezi, hogy létrehozott egy tábla "MyTable" Sybase és egy "időbélyeg" nevű adatsorozat időadatok oszlopot tartalmaz.

"External" beállítása: igaz arról értesíti az, hogy ez az adatkészlet külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák hello Data Factory szolgáltatásnak. Figyelje meg, hogy hello **típus** hello szolgáltatásnak van állítva: **RelationalTable**.

```JSON
{
    "name": "SybaseDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremSybaseLinkedService",
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
    "name": "AzureBlobSybaseDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság hello DBA hello adatok választja ki. Hello adatbázis rendelések táblájában.

```JSON
{
    "name": "CopySybaseToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from DBA.Orders"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "SybaseDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobSybaseDataSet"
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
                "name": "SybaseToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="type-mapping-for-sybase"></a>Típusleképezés a Sybase rendszerhez
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk hello másolási tevékenység hajt végre automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello:

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Sybase T-SQL és a T-SQL-típusokat támogatja. Az sql-típusok too.NET típusból hozzárendelési táblázatot [Azure SQL-összekötő](data-factory-azure-sql-connector.md) cikk.

## <a name="map-source-toosink-columns"></a>A forrásoszlopokat toosink leképezése
toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>A relációs források ismételhető Olvasás
Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása. Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
