---
title: "Adatok áthelyezése az Amazon Redshift Data Factory használatával |} Microsoft Docs"
description: "További tudnivalók az Azure Data Factory használatával Amazon Redshift áthelyezni az adatokat."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 01d15078-58dc-455c-9d9d-98fbdf4ea51e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: bccb941363952bb2251629240a88148a6527d62e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Helyezze át az adatokat az Amazon Redshift Azure Data Factory használatával
Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factoryben az adatok mozgatása Amazon Redshift. A cikk épít, a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést. 

Amazon Redshift adatok bármely támogatott fogadó adattárolóhoz másolhatja. A másolási tevékenység által támogatott mosdók adattárolókhoz listájáért lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Adat-előállító jelenleg a mozgóátlag adatait Amazon Redshift egyéb adattárakhoz, de nem az adatok áthelyezése az egyéb adattárakhoz Amazon Redshift támogatja.

## <a name="prerequisites"></a>Előfeltételek
* Ha a helyszíni adattárolóihoz adatokat helyez át, telepítse [az adatkezelési átjáró](data-factory-data-management-gateway.md) a helyi gépen. Ezt követően hozzáférést adatkezelési átjáró (használata IP-cím a gép) az Amazon Redshift fürt. Lásd: [engedélyezi a hozzáférést a fürthöz](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) utasításokat.
* Ha egy Azure data Store helyez át adatokat, lásd: [Azure Data Center IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653) számítási IP-cím és az Azure-adatok által használt SQL-címtartományok szolgáltatásban.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, amely helyezi át az adatokat Amazon Redshift forrásból származó különböző eszközök/API-k használatával hozhatja létre egy folyamatot.

Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.

Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját. 

Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban: 

1. Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.
2. Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.  Adatok másolása az Amazon Redshift adattár használt adat-előállító entitások JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Amazon Redshift az Azure Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob) című szakaszát. 

A következő szakaszok részletesen bemutatják, amely segítségével az Amazon Redshift megadása a Data Factory tartozó entitások JSON-tulajdonságok: 

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
A következő táblázat a JSON-elemek szerepelnek Amazon Redshift kapcsolódó szolgáltatásra vonatkozó leírást.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |A type tulajdonságot kell beállítani: **AmazonRedshift**. |Igen |
| kiszolgáló |Kiszolgáló IP-címét vagy állomásnevét kiszolgálónevét az Amazon Redshift. |Igen |
| port |A TCP-portot, amelyen az Amazon Redshift kiszolgáló ügyfélkapcsolatokat száma. |Nem, alapértelmezett érték: 5439 |
| adatbázis |Az Amazon Redshift adatbázis nevét. |Igen |
| felhasználónév |Felhasználó, aki hozzáfér az adatbázis neve. |Igen |
| jelszó |A felhasználói fiók jelszavát. |Igen |

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

A **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú. Tartalmazza az adatokat az adattár a helyére vonatkozó adatokat. A typeProperties szakasz típusú adatkészlet **RelationalTable** (amely tartalmazza az Amazon Redshift dataset) a következő tulajdonságokkal rendelkezik.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |A tábla az Amazon Redshift adatbázisban, amelyre a társított szolgáltatás neve hivatkozik. |Nem (Ha **lekérdezés** a **RelationalSource** van megadva) |

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.

Mivel a tulajdonságok érhetők el a **typeProperties** szakasz a tevékenység tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók típusától függően.

Ha másolási tevékenység forrása típusú **RelationalSource** (amely tartalmazza az Amazon Redshift), a következő tulajdonságok érhetők el typeProperties szakaszában:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Az egyéni lekérdezés segítségével adatokat olvasni. |SQL-lekérdezési karakterlánc. Például: Válasszon * from tábla. |Nem (Ha **tableName** a **dataset** van megadva) |

## <a name="json-example-copy-data-from-amazon-redshift-to-azure-blob"></a>JSON-példa: adatok másolása az Amazon Redshift az Azure-Blobba
Ez a példa bemutatja, hogyan Amazon Redshift adatbázisból származó adatok másolása az Azure Blob Storage tárolóban. Azonban az adatok átmásolhatók **közvetlenül** bármely, a megadott nyelő [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.  

A minta a következő data factory entitások rendelkezik:

* A társított szolgáltatás típusa [AmazonRedshift](#linked-service-properties).
* A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties).
* Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).

A minta másol adatokat az Amazon Redshift egy lekérdezés eredményét blob minden órában. A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.

**Amazon Redshift társított szolgáltatáshoz:**

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "< The IP address or host name of the Amazon Redshift server >",
            "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
            "database": "<The database name of the Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
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
**Amazon Redshift bemeneti adatkészlet:**

Beállítás `"external": true` tájékoztatja a Data Factory szolgáltatásnak, hogy az adatkészlet data factoryval való külső, és egy tevékenység adat-előállító nem hozzák. Ez a tulajdonság igaz értékre be egy bemeneti adatkészlet nem a feldolgozási tevékenység által létrehozott.

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
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

Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1). A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik. A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**A folyamat Azure Redshift (RelationalSource) és a fogadó Blob másolási tevékenység:**

A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték. Az adatcsatorna JSON-definícióból a **forrás** típusúra **RelationalSource** és **fogadó** típusúra **BlobSink**. A megadott SQL-lekérdezést a **lekérdezés** tulajdonság kiválasztása az adatok másolása az elmúlt órában.

```json
{
    "name": "CopyAmazonRedshiftToBlob",
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
                        "name": "AmazonRedshiftInputDataset"
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
                "name": "AmazonRedshiftToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-amazon-redshift"></a>Az Amazon Redshift leképezésének
Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység az eseményforrás-típusnak a következő kétlépéses módszert típusok gyűjtése automatikus típuskonverziók hajtja végre:

1. A natív eseményforrás-típusnak átalakítása .NET-típusa
2. .NET-típus konvertálása natív a fogadó típusa

Ha adatok áthelyezése Amazon Redshift, a következő megfeleltetéseket használ Amazon Redshift való .NET típusú.

| Amazon Redshift típusa | .NET-alapú típusa |
| --- | --- |
| SMALLINT |Int16 |
| EGÉSZ SZÁM |Int32 |
| BIGINT |Int64 |
| DECIMÁLIS |Decimális |
| VALÓS |Egyetlen |
| A KÉTSZERES PONTOSSÁG |Dupla |
| LOGIKAI ÉRTÉK |Karakterlánc |
| KARAKTER |Karakterlánc |
| VARCHAR |Karakterlánc |
| DÁTUM |Dátum és idő |
| IDŐBÉLYEG |Dátum és idő |
| SZÖVEG |Karakterlánc |

## <a name="map-source-to-sink-columns"></a>Térkép forrás oszlopok gyűjtése
A forrás oszlop szerepel a fogadó dataset adatkészlet leképezési oszlopok, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>A relációs források ismételhető Olvasás
Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell. Lásd: [Repeatable olvasni a relációs források](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.

## <a name="next-steps"></a>Következő lépések
Lásd az alábbi cikkeket:

* [Másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) való a másolási tevékenység során a folyamat létrehozásának lépéseit.
