---
title: "aaaMove adatokat OData forrásból |} Microsoft Docs"
description: "További információk a hogyan toomove adatokat OData adatforrásokat, Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: de28fa56-3204-4546-a4df-21a21de43ed7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 328efe4fd274fb3e54c1d2f209e4614c77c1ff37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a>Helyezi át az adatokat a egy OData-forrásra Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatokat OData forrásból származó a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

Egy OData forrás támogatott tooany fogadó adattároló adatainak másolhatja. Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. Adat-előállító jelenleg csak áthelyezése egy OData forrásadatok tooother adatait tárolja, de nem támogatja az adatok áthelyezését más adatok tooan OData-forrásra tárolja. 

## <a name="supported-versions-and-authentication-types"></a>Támogatott verziók és hitelesítési típusok
Az OData-összekötő 3.0 és 4.0-s verzióját, és mindkét felhőbeli OData adatait és a helyszíni OData források másolhatja OData-verziót támogatja. Ez utóbbi hello kell tooinstall hello az adatkezelési átjáró. Lásd: [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) szóló cikkben olvashat az adatkezelési átjáró.

Alább hitelesítési típusok támogatottak:

* tooaccess **felhő** OData-adatcsatorna, használhatja, hogy a névtelen, alapszintű (felhasználónév és jelszó), vagy az Azure Active Directory-alapú OAuth-hitelesítést.
* tooaccess **helyszíni** OData-adatcsatorna, használja a névtelen, alapszintű (felhasználónév és jelszó), vagy a Windows-hitelesítést.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, amely helyezi át az adatokat OData forrásból származó különböző eszközök/API-k használatával létrehozhat egy folyamatot.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello: 

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Egy minta JSON-definíciók, amelyek használt toocopy adatokat OData forrásból származó adat-előállító entitások, lásd: [JSON-példa: adatok másolása az OData-forrás tooAzure Blob](#json-example-copy-data-from-odata-source-to-azure-blob) című szakaszát. 

a következő szakaszok hello használt toodefine adat-előállító entitások adott tooOData forrás JSON-tulajdonághoz részleteit tartalmazzák:

## <a name="linked-service-properties"></a>Csatolt szolgáltatás tulajdonságai
a következő táblázat hello biztosít JSON elemek adott tooOData kapcsolódó szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **OData** |Igen |
| URL-címe |Hello OData-szolgáltatás URL-címét. |Igen |
| AuthenticationType |Tooconnect toohello OData-forrásra használt hitelesítés típusa. <br/><br/> A felhőbeli OData a lehetséges értékek: névtelen, alapszintű és OAuth (Megjegyzés: jelenleg csak Azure Data Factory támogatási Azure Active Directory-alapú OAuth). <br/><br/> A helyszíni OData lehetséges értékei a névtelen, alapszintű és a Windows. |Igen |
| felhasználónév |Ha egyszerű hitelesítést használ, adja meg a felhasználónevet. |Igen (Ha csak az egyszerű hitelesítés használata esetén) |
| jelszó |Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót. |Igen (Ha csak az egyszerű hitelesítés használata esetén) |
| authorizedCredential |Ha OAuth használ, kattintson a **engedélyezés** hello Data Factory másolása varázsló vagy a szerkesztő gombra, és adja meg a hitelesítő adatok, akkor a tulajdonság értékének hello lesz automatikusan generált. |Igen (csak ha OAuth-hitelesítés használata esetén) |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello helyszíni OData-szolgáltatás. Csak adja meg, ha a másolt adatokat a helyszíni OData-forrásra. |Nem |

### <a name="using-basic-authentication"></a>Alapszintű hitelesítést használó
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a>A névtelen hitelesítés segítségével
```json
{
    "name": "ODataLinkedService",
        "properties":
    {
        "type": "OData",
        "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a>Windows-hitelesítéssel fér hozzá a helyszíni OData-forrásra
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-oauth-authentication-accessing-cloud-odata-source"></a>OAuth hitelesítési elérése során a felhővel OData-forrásra
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source e.g. https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking hello Authorize button on UI>"
        }
    }
}
```

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. hello typeProperties szakasz típusú adatkészlet **ODataResource** következő tulajdonságai hello (amely tartalmazza a OData dataset) rendelkezik.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| Elérési út |Elérési út toohello OData-erőforrás |Nem |

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.

Tulajdonságok tevékenységek minden típusának hello hello tevékenységekre hello typeProperties szakaszában érhető el ugyanakkor függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

Ha forrás típusa nem **RelationalSource** (amely magában foglalja az OData) typeProperties szakaszában érhetők hello következő tulajdonságai:

| Tulajdonság | Leírás | Példa | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |"? $select neve, leírása és $top = = 5" |Nem |

## <a name="type-mapping-for-odata"></a>Az OData-leképezésének
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello.

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Amikor adatokat OData, hello következő megfeleltetéseket használt OData típusok too.NET típusból.

| Az OData-adattípus | .NET-típusa |
| --- | --- |
| Edm.Binary |Byte] |
| Edm.Boolean |logikai érték |
| Edm.Byte |Byte] |
| Edm.DateTime |Dátum és idő |
| Edm.Decimal |Decimális |
| Edm.Double |Dupla |
| Edm.Single |Egyetlen |
| Edm.Guid |GUID |
| Edm.Int16 |Int16 |
| Edm.Int32 |Int32 |
| Edm.Int64 |Int64 |
| Edm.SByte |Int16 |
| Edm.String |Karakterlánc |
| Edm.Time |A TimeSpan |
| Edm.DateTimeOffset |DateTimeOffset |

> [!Note]
> OData összetett adattípusok pl. objektum nem támogatottak.

## <a name="json-example-copy-data-from-odata-source-tooazure-blob"></a>JSON-példa: adatok másolása az OData-forrás tooAzure Blob
Ebben a példában a minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ezek bemutatják, hogyan toocopy egy OData az adatforrás-tooan Azure Blob Storage. Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello. hello minta a következő adat-előállító entitások hello rendelkezik:

1. A társított szolgáltatás típusa [OData](#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [ODataResource](#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másol adatokat kérdez le egy OData-forrás tooan szemben Azure blob minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

**OData társított szolgáltatás:** ebben a példában használt hello névtelen hitelesítést. Lásd: [OData társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.

```json
{
    "name": "ODataLinkedService",
        "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
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

**OData bemeneti adatkészlet:**

"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```json
{
    "name": "ODataDataset",
    "properties":
    {
        "type": "ODataResource",
        "typeProperties":
        {
                "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3                
        }
    }
}
```

Adja meg **elérési** hello adatkészlet definíciója nem kötelező megadni.

**Az Azure Blob kimeneti adatkészlet:**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```json
{
    "name": "AzureBlobODataDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


**Másolási tevékenység az OData-forrásra és Blob fogadó egy folyamaton belül:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság hello legújabb (legújabb) adatokat kiválaszt hello OData-forrásra.

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "?$select=Name, Description&$top=5",
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "ODataDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobODataDataSet"
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
                "name": "ODataToBlob"
            }
        ],
        "start": "2017-02-01T18:00:00Z",
        "end": "2017-02-03T19:00:00Z"
    }
}
```

Adja meg **lekérdezés** hello folyamat definíciója nem kötelező megadni. Hello **URL-cím** van, hogy a hello Data Factory szolgáltatásnak tooretrieve adatokat használja: társított szolgáltatás (kötelező) + hello adatkészletet (nem kötelező) a megadott elérési út hello + hello folyamat (nem kötelező) lekérdezés megadott URL-CÍMÉT.


### <a name="type-mapping-for-odata"></a>Az OData-leképezésének
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello:

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Adatok áthelyezése az OData-adattároló, amikor a OData adattípusok olyan csatlakoztatott too.NET típusok.

## <a name="map-source-toosink-columns"></a>A forrásoszlopokat toosink leképezése
toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>A relációs források ismételhető Olvasás
Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása. Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
