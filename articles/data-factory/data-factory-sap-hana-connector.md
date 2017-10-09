---
title: "Azure Data Factory használatával SAP HANA aaaMove adatait |} Microsoft Docs"
description: "További tudnivalók az Azure Data Factory használatával SAP HANA toomove adatok."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 5cefe4c8ed01ea4e86e02496b2f8a9083d0b949c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sap-hana-using-azure-data-factory"></a>Helyezze át az adatokat az SAP HANA Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatait egy helyszíni SAP HANA a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

Egy helyszíni SAP HANA adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja. Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. Adat-előállító jelenleg csak áthelyezése egy SAP HANA-tooother adatok adatait tárolja, de nem támogatja az adatok áthelyezését más adatok tooan SAP HANA tárolja.

## <a name="supported-versions-and-installation"></a>Támogatott verziók és telepítés
Ez az összekötő támogatja az SAP HANA-adatbázisból bármely verzióját. Támogatja az adatok másolását a HANA információk modellek (például az elemzési és a számítási nézetek) és a sorhoz/oszlophoz táblák SQL-lekérdezések használatával.

tooenable hello kapcsolat toohello SAP HANA-példány, a következő összetevők hello telepítése:
- **Az adatkezelési átjáró**: összekötő tooon helyszíni adatok adat-előállító támogatja (beleértve az SAP HANA) az adatkezelési átjáró összetevő használatával hívása. Az adatkezelési átjáró és hello átjáró beállításának lépéseit toolearn lásd: [között a helyszíni adatok áthelyezése az adattároló toocloud adattár](data-factory-move-data-between-onprem-and-cloud.md) cikk. Átjáróra szükség, akkor is, ha az SAP HANA hello Azure IaaS virtuális gépre (VM) tárolódik. Hello ugyanazon VM hello adatként tanúsítványtárolójában, vagy egy másik virtuális gép mindaddig hello átjáró kapcsolódhatnak toohello adatbázis hello átjáró telepíthető.
- **SAP HANA ODBC-illesztőprogram** hello-átjáró számítógépén. Hello SAP HANA ODBC-illesztőprogram letölthető hello [SAP szoftverletöltő központból](https://support.sap.com/swdc). Hello kulcsszavas keresés **SAP HANA ügyfél Windows**. 

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot. 

- hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást. 
- Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek egy helyszíni SAP HANA használt toocopy adatok JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az SAP HANA tooAzure Blob](#json-example-copy-data-from-sap-hana-to-azure-blob) című szakaszát. 

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooan SAP HANA adattár részleteit tartalmazzák:

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
a következő táblázat hello biztosít a társított szolgáltatás JSON-elemek adott tooSAP HANA leírását.

Tulajdonság | Leírás | Megengedett értékek | Szükséges
-------- | ----------- | -------------- | --------
kiszolgáló | Hello kiszolgálóra mely hello SAP HANA példány neve. Ha a kiszolgáló egy testreszabott portot használ, adja meg a `server:port`. | Karakterlánc | Igen
AuthenticationType | Hitelesítés típusa. | Karakterlánc. "Basic" vagy "Windows" | Igen 
felhasználónév | Hello toohello SAP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó neve | Karakterlánc | Igen
jelszó | Hello felhasználó jelszavát. | Karakterlánc | Igen
gatewayName | Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello helyszíni SAP HANA-példányt kell használnia. | Karakterlánc | Igen
encryptedCredential | hello titkosított hitelesítő adat karakterlánc. | Karakterlánc | Nem

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. Nincsenek hello SAP HANA-adatkészlet típusú támogatott típusra vonatkozó tulajdonságok **RelationalTable**. 


## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. A rendszer például név, leírás, a bemeneti és kimeneti táblák tulajdonságokat olyan szabályzatok állnak rendelkezésre az összes tevékenység.

Mivel tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

Ha a másolási tevékenység forrása típusa **RelationalSource** (amely tartalmazza a SAP HANA), a következő tulajdonságok hello typeProperties szakaszában érhetők:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés | Megadja a hello SQL lekérdezés tooread adatait hello SAP HANA-példányból. | SQL-lekérdezésben. | Igen |

## <a name="json-example-copy-data-from-sap-hana-tooazure-blob"></a>JSON-példa: adatok másolása az SAP HANA tooAzure Blob
hello következő minta nyújt minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ez a példa bemutatja, hogyan egy helyszíni SAP HANA tooan Azure Blob Storage toocopy adatait. Azonban az adatok átmásolhatók **közvetlenül** felsorolt hello nyelő tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.  

> [!IMPORTANT]
> Ez a minta JSON kódtöredékek biztosít. Nem tartalmazza a hello adat-előállító létrehozásának részletes leírása. Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.

hello minta a következő data factory entitások hello rendelkezik:

1. A társított szolgáltatás típusa [SapHana](#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másol adatokat egy SAP HANA-példány tooan Azure blob óránként. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

Első lépésként a telepítő hello az adatkezelési átjáró. hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.

### <a name="sap-hana-linked-service"></a>SAP HANA társított szolgáltatás
Ez a SAP HANA-példány toohello adat-előállító kapcsolódó szolgáltatás hivatkozásokat. hello type tulajdonság beállítása túl**SapHana**. hello typeProperties szakaszból hello SAP HANA-példány-kapcsolódási információt.

```json
{
    "name": "SapHanaLinkedService",
    "properties":
    {
        "type": "SapHana",
        "typeProperties":
        {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```

### <a name="azure-storage-linked-service"></a>Azure Storage társított szolgáltatás
Ez az Azure Storage-fiók toohello adat-előállító kapcsolódó szolgáltatás hivatkozásokat. hello type tulajdonság beállítása túl**AzureStorage**. hello typeProperties szakaszból hello Azure Storage-fiók kapcsolódási információt.

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

### <a name="sap-hana-input-dataset"></a>SAP HANA-bemeneti adatkészlet

Ez az adatkészlet meghatározása hello SAP HANA-adatkészlet. Hello típusú hello adat-előállító adatkészlet túl beállítása**RelationalTable**. Jelenleg nincs megadva egy SAP HANA-adatkészlet típusra vonatkozó tulajdonsága. a másolási tevékenység definíciójának hello hello lekérdezés határozza meg, hogy milyen adatok tooread hello SAP HANA-példányból. 

Hello Data Factory szolgáltatásnak beállítása external tulajdonság tootrue arról értesíti az adott hello tábla külső toohello adat-előállító, ezért nem hozzák hello adat-előállítóban tevékenység.

Gyakoriság és időköz tulajdonságok hello ütemezése határozza meg. Ebben az esetben hello adatolvasás hello SAP HANA-példányból óránként. 

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="azure-blob-output-dataset"></a>Azure Blob kimeneti adatkészlet
Ez az adatkészlet meghatározása hello kimeneti Azure Blob-adathalmazra. hello type tulajdonság beállítása tooAzureBlob. hello typeProperties rész másolta át hello SAP HANA-példányból hello adatok tárolására. hello adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/saphana/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="pipeline-with-copy-activity"></a>A másolási tevékenység-feldolgozási folyamat

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** (az SAP HANA-forrás) és **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.

```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<SQL Query for HANA>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapHanaDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDataSet"
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
                "name": "SapHanaToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```


### <a name="type-mapping-for-sap-hana"></a>SAP Hana leképezésének
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello:

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Adatok áthelyezése SAP HANA, amikor a következő hozzárendelések hello SAP HANA-típusok too.NET típusok használtak.

SAP HANA-típus | .NET-alapú típusa
------------- | ---------------
TINYINT | Bájt
SMALLINT | Int16
INT | Int32
BIGINT | Int64
VALÓS | Egyetlen
DUPLA | Egyetlen
DECIMÁLIS | Decimális
LOGIKAI ÉRTÉK | Bájt
VARCHAR | Karakterlánc
NVARCHAR | Karakterlánc
CLOB | Byte]
ALPHANUM | Karakterlánc
A BLOB | Byte]
DÁTUM | Dátum és idő
IDŐ | A TimeSpan
IDŐBÉLYEG | Dátum és idő
SECONDDATE | Dátum és idő

## <a name="known-limitations"></a>Ismert korlátozásai
Ha az adatok másolása SAP HANA, van néhány ismert korlátozásai:

- NVARCHAR értékek a következők: 4000 Unicode-karaktereket csonkolt toomaximum hossza
- SMALLDECIMAL nem támogatott.
- VARBINARY nem támogatott.
- Érvényes dátumok csak közötti 1899 – 12/30 és 31-9999-12

## <a name="map-source-toosink-columns"></a>A forrásoszlopokat toosink leképezése
toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>A relációs források ismételhető Olvasás
Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása. Lásd: [Repeatable olvasni a relációs források](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
