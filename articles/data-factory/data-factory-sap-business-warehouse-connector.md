---
title: "Azure Data Factory használatával SAP Business Warehouse aaaMove adatait |} Microsoft Docs"
description: "További tudnivalók az Azure Data Factory használatával SAP Business Warehouse toomove adatok."
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
ms.date: 05/16/2017
ms.author: jingwang
ms.openlocfilehash: 85df16f4759a846f578cad301e3cf918179143d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sap-business-warehouse-using-azure-data-factory"></a>Helyezze át az adatokat az SAP Business Warehouse Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatok egy helyszíni SAP Business adatraktár (BW) a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

Egy helyszíni SAP Business Warehouse adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja. Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. Adat-előállító jelenleg csak áthelyezése egy SAP Business Warehouse tooother adatok adatait tárolja, de nem támogatja az adatok áthelyezését más adatok tooan SAP Business Warehouse tárolja. 

## <a name="supported-versions-and-installation"></a>Támogatott verziók és telepítés
Ez az összekötő támogatja az SAP Business Warehouse verzió 7.x. Támogatja az adatok másolását előforduló infocubes-értékeket és QueryCubes (többek között a következőket BEx lekérdezések) segítségével MDX-lekérdezésekben.

tooenable hello kapcsolat toohello SAP BW-példány, a következő összetevők hello telepítése:
- **Az adatkezelési átjáró**: összekötő tooon helyszíni adatok adat-előállító támogatja (beleértve az SAP Business Warehouse) az adatkezelési átjáró összetevő használatával hívása. Az adatkezelési átjáró és hello átjáró beállításának lépéseit toolearn lásd: [között a helyszíni adatok áthelyezése az adattároló toocloud adattár](data-factory-move-data-between-onprem-and-cloud.md) cikk. Átjáróra szükség, akkor is, ha az SAP Business Warehouse hello Azure IaaS virtuális gépként (VM) van tárolva. Hello ugyanazon VM hello adatként tanúsítványtárolójában, vagy egy másik virtuális gép mindaddig hello átjáró kapcsolódhatnak toohello adatbázis hello átjáró telepíthető.
- **SAP NetWeaver könyvtár** hello-átjáró számítógépén. Az SAP rendszergazdájától, vagy közvetlenül a hello kaphat hello SAP Netweaver könyvtár [SAP szoftverletöltő központból](https://support.sap.com/swdc). Keresse meg a hello **SAP Megjegyzés #1025361** tooget hello letöltési helyét hello alkalmazás legújabb verziójára. Győződjön meg arról, hogy hello architektúra hello SAP NetWeaver szalagtár (32 bites vagy 64 bites) megegyezik-e az átjáró telepítése. Ezután telepítse hello SAP NetWeaver RFC SDK függően toohello SAP Megjegyzés szereplő összes fájlt. hello SAP NetWeaver könyvtár hello SAP Client Tools telepítését is megtalálhatók.

> [!TIP]
> Hello DLL-ek kibontani a NetWeaver RFC SDK hello a system32 mappába helyezze el.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot. 

- hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást. 
- Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek egy helyszíni SAP Business Warehouse használt toocopy adatok JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az SAP Business Warehouse tooAzure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) című szakaszát. 

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooan SAP BW adattár részleteit tartalmazzák:

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
a következő táblázat hello biztosít JSON-elemek adott tooSAP Business Warehouse (BW) kapcsolódó szolgáltatás leírását.

Tulajdonság | Leírás | Megengedett értékek | Szükséges
-------- | ----------- | -------------- | --------
kiszolgáló | Hello kiszolgálóra mely hello SAP BW példány neve. | Karakterlánc | Igen
systemNumber | Hello SAP BW rendszer rendszer száma. | Kétjegyű tizedes tört karakterláncból. | Igen
clientId | Hello ügyfél hello SAP W rendszer ügyfél-azonosító. | Három számjegyből tizedes tört karakterláncból. | Igen
felhasználónév | Hello toohello SAP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó neve | Karakterlánc | Igen
jelszó | Hello felhasználó jelszavát. | Karakterlánc | Igen
gatewayName | Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello helyszíni SAP BW példányát kell használnia. | Karakterlánc | Igen
encryptedCredential | hello titkosított hitelesítő adat karakterlánc. | Karakterlánc | Nem

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. Nincsenek hello SAP BW adatkészlet típusú támogatott típusra vonatkozó tulajdonságok **RelationalTable**. 


## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. A rendszer például név, leírás, a bemeneti és kimeneti táblák tulajdonságokat olyan szabályzatok állnak rendelkezésre az összes tevékenység.

Mivel tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

Ha a másolási tevékenység forrása típusa **RelationalSource** (amely tartalmazza az SAP BW), typeProperties szakaszában érhetők hello következő tulajdonságai:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés | Megadja a hello MDX tooread adatait hello SAP BW-példányból. | MDX-lekérdezés. | Igen |


## <a name="json-example-copy-data-from-sap-business-warehouse-tooazure-blob"></a>JSON-példa: adatok másolása az SAP Business Warehouse tooAzure Blob
hello alábbi példa minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ez a példa bemutatja, hogyan egy helyszíni SAP Business Warehouse tooan Azure Blob Storage toocopy adatait. Azonban az adatok átmásolhatók **közvetlenül** közölt hello nyelő tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.  

> [!IMPORTANT]
> Ez a minta JSON kódtöredékek biztosít. Nem tartalmazza a hello adat-előállító létrehozásának részletes leírása. Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.

hello minta a következő data factory entitások hello rendelkezik:

1. A társított szolgáltatás típusa [SapBw](#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másol adatokat az SAP Business Warehouse példány tooan Azure blob óránként. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

Első lépésként a telepítő hello az adatkezelési átjáró. hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.

### <a name="sap-business-warehouse-linked-service"></a>SAP Business Warehouse társított szolgáltatás
Ez a SAP BW példány toohello data factory kapcsolódó szolgáltatás hivatkozásokat. hello type tulajdonság beállítása túl**SapBw**. hello typeProperties szakaszból hello SAP BW példány-kapcsolódási információt. 

```json
{
    "name": "SapBwLinkedService",
    "properties":
    {
        "type": "SapBw",
        "typeProperties":
        {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
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

### <a name="sap-bw-input-dataset"></a>SAP BW bemeneti adatkészlet
Ez az adatkészlet hello SAP Business Warehouse adatkészlet meghatározása. Hello típusú hello adat-előállító adatkészlet túl beállítása**RelationalTable**. Jelenleg nincs megadva az SAP BW adatkészlethez típusra vonatkozó tulajdonsága. a másolási tevékenység definíciójának hello hello lekérdezés határozza meg, milyen adatok tooread hello SAP BW-példányból. 

Hello Data Factory szolgáltatásnak beállítása external tulajdonság tootrue arról értesíti az adott hello tábla külső toohello adat-előállító, ezért nem hozzák hello adat-előállítóban tevékenység.

Gyakoriság és időköz tulajdonságok hello ütemezése határozza meg. Ebben az esetben hello adatolvasás hello SAP BW példányból óránként. 

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
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
Ez az adatkészlet meghatározása hello kimeneti Azure Blob-adathalmazra. hello type tulajdonság beállítása tooAzureBlob. hello typeProperties rész másolta át hello SAP BW-példányból hello adatok tárolására. hello adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sapbw/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** (az SAP BW forrás) és **fogadó** típusuk értéke túl**BlobSink**. hello megadott hello lekérdezés **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<MDX query for SAP BW>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapBwDataset"
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
                "name": "SapBwToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```



### <a name="type-mapping-for-sap-bw"></a>Az SAP BW Programhoz leképezésének
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello:

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Adatok áthelyezése az SAP BW Programhoz, amikor a következő hozzárendelések hello SAP BW típusok too.NET típusok használtak.

Hello ABAP szótár típusú adatok | .NET-adattípus
-------------------------------- | --------------
ACCP |  int
KARAKTER | Karakterlánc
CLNT | Karakterlánc
PÉNZNEM | Decimális
CUKY | Karakterlánc
DEC | Decimális
FLTP | Dupla
INT1 | Bájt
INT2 | Int16
INT4 | int
LANG | Karakterlánc
LCHR | Karakterlánc
LRAW | Byte]
PREC | Int16
QUAN | Decimális
NYERS | Byte]
RAWSTRING | Byte]
KARAKTERLÁNC | Karakterlánc
EGYSÉG | Karakterlánc
DATS | Karakterlánc
NUMC | Karakterlánc
TIMS | Karakterlánc

> [!NOTE]
> Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).


## <a name="map-source-toosink-columns"></a>A forrásoszlopokat toosink leképezése
toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>A relációs források ismételhető Olvasás
Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása. Lásd: [Repeatable olvasni a relációs források](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
