---
title: "ODBC-adattároló aaaMove adatait |} Microsoft Docs"
description: "További információk a hogyan ODBC adatok toomove adatait tárolja az Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: ad70a598-c031-4339-a883-c6125403cb76
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jingwang
ms.openlocfilehash: bf96e71da449313b6144bb194205c572d2ca2030
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-odbc-data-stores-using-azure-data-factory"></a>Helyezze át a ODBC adattárolókhoz Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse hello másolási tevékenység az Azure Data Factory toomove adatait egy helyszíni ODBC adatok tárolásához. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

Egy ODBC adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja. Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. Adat-előállító jelenleg csak egy ODBC-adatokból adattároló tooother adattárolókhoz áthelyezése, de nem adatok áthelyezését más adatok tárolók tooan ODBC adatok tárolására. 

## <a name="enabling-connectivity"></a>Kapcsolat engedélyezése
Data Factory szolgáltatásnak csatlakozó tooon helyszíni ODBC adatforrások az adatkezelési átjáró hello segítségével támogatja. Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat. Hello átjáró tooconnect tooan ODBC adattár használata akkor is, ha egy Azure IaaS virtuális gép helyezkedik el.

Hello ugyanabban a helyi számítógép- vagy hello hello ODBC adatokat tároló Azure VM hello átjáró telepíthető. Azonban azt javasoljuk, hogy hello átjáró telepítése egy különálló számítógép vagy az Azure infrastruktúra-szolgáltatási virtuális gép tooavoid Erőforrásverseny és a jobb teljesítmény érdekében. Hello átjáró egy külön számítógépen való telepítésekor hello gépnek képes tooaccess hello gép hello ODBC adattár kell lennie.

Az adatkezelési átjáró hello, leszámítva szükség tooinstall hello ODBC-illesztőprogram hello adattároló hello-átjáró számítógépén.

> [!NOTE]
> Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, mely az adatok egy ODBC-adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello: 

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek egy ODBC-adattároló használt toocopy adatait JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az ODBC-adatok tárolására tooAzure Blob](#json-example-copy-data-from-odbc-data-store-to-azure-blob) című szakaszát. 

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooODBC adattár részleteit tartalmazzák:

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
a következő táblázat hello biztosít JSON elemek adott tooODBC kapcsolódó szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **OnPremisesOdbc** |Igen |
| connectionString |hello nem hozzáférési hitelesítő adatok része hello kapcsolati karakterláncot, és egy nem kötelező hitelesítő adat titkosítva. Példák a következő részekben hello. |Igen |
| hitelesítő adatok |hello hozzáférési hitelesítő adatok része, illesztőprogram-specifikus tulajdonság-érték formátumban megadott hello kapcsolati karakterlánc. Példa: "Uid =<user ID>; Pwd =<password>; RefreshToken =<secret refresh token>; ". |Nem |
| AuthenticationType |Tooconnect toohello ODBC adattár használt hitelesítés típusa. Lehetséges értékek a következők: névtelen és alapvető. |Igen |
| felhasználónév |Ha egyszerű hitelesítést használ, adja meg a felhasználónevet. |Nem |
| jelszó |Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót. |Nem |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello ODBC adattárat kell használnia. |Igen |

### <a name="using-basic-authentication"></a>Alapszintű hitelesítést használó

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```
### <a name="using-basic-authentication-with-encrypted-credentials"></a>Alapszintű hitelesítést használ, a titkosított hitelesítő adatokkal
Hello hitelesítő adatok hello segítségével titkosíthatja [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (Azure PowerShell 1.0-ás verziója) parancsmag vagy [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 vagy korábbi verziójú hello Az Azure PowerShell).  

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a>A névtelen hitelesítés segítségével

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "mygateway"
        }
    }
}
```


## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. hello typeProperties szakasz típusú adatkészlet **RelationalTable** következő tulajdonságai hello (amely tartalmazza a ODBC dataset) rendelkezik.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello ODBC adattár hello tábla neve. |Igen |

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.

Tulajdonságok érhetők el hello **typeProperties** szakasz hello hello tevékenységekre ugyanakkor tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

A másolási tevékenység során típusú forrás esetén **RelationalSource** (amely tartalmazza a ODBC), typeProperties szakaszában érhetők hello következő tulajdonságai:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: Válasszon * from tábla. |Igen |


## <a name="json-example-copy-data-from-odbc-data-store-tooazure-blob"></a>JSON-példa: adatok másolása az ODBC-adatok tárolására tooAzure Blob
Ez a példa biztosít JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azt illusztrálja, hogyan toocopy egy ODBC az adatforrás-tooan Azure Blob Storage tárolóban. Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.

hello minta a következő data factory entitások hello rendelkezik:

1. A társított szolgáltatás típusa [OnPremisesOdbc](#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másol adatokat egy lekérdezés eredményét, az ODBC adatokat tároló tooa BLOB minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

Első lépésként hello az adatkezelési átjáró beállítása. hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.

**ODBC társított szolgáltatás** ebben a példában, használja az egyszerű hitelesítés hello. Lásd: [ODBC társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.

```json
{
    "name": "OnPremOdbcLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
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

**ODBC bemeneti adatkészlet**

hello minta azt feltételezi, hogy létrehozott egy "MyTable" tábla egy ODBC-adatbázis és a "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.

"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "published": false,
        "type": "RelationalTable",
        "linkedServiceName": "OnPremOdbcLinkedService",
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

**Az Azure Blob kimeneti adatkészlet**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```json
{
    "name": "AzureBlobOdbcDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odbc/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


**Másolási tevékenység során a folyamat az ODBC-adatforrás (RelationalSource) és a Blob fogadó (BlobSink)**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse ezen bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.

```json
{
    "name": "CopyODBCToBlob",
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
                        "name": "OdbcDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOdbcDataSet"
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
                "name": "OdbcToBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-odbc"></a>Az ODBC leképezésének
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello:

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Adatok ODBC adatok áruházakból áthelyezésekor ODBC adattípusok-e a csatlakoztatott too.NET típusok, a hello [ODBC adattípus-leképezések alkalmazását](https://msdn.microsoft.com/library/cc668763.aspx) témakör.

## <a name="map-source-toosink-columns"></a>A forrásoszlopokat toosink leképezése
toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>A relációs források ismételhető Olvasás
Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása. Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="ge-historian-store"></a>GE történész tároló
Az ODBC kapcsolódó szolgáltatás toolink létrehozhat egy [GE Proficy történész (most GE történész)](http://www.geautomation.com/products/proficy-historian) az adattároló tooan az Azure data factory, ahogy az alábbi példa hello:

```json
{
    "name": "HistorianLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "connectionString": "DSN=<name of hello GE Historian store>",
            "gatewayName": "<gateway name>",
            "authenticationType": "Basic",
            "userName": "<user name>",
            "password": "<password>"
        }
    }
}
```

Az adatkezelési átjáró telepíthető egy helyszíni gépre, és hello átjáró regisztrálása hello portálon. a helyszíni számítógépre telepített hello átjáró GE történész tooconnect toohello GE történész adattár hello ODBC-illesztőprogram használ. Ezért telepítenie hello illesztőprogram, ha még nincs telepítve hello-átjáró számítógépén. Lásd: [kapcsolat engedélyezése](#enabling-connectivity) című szakaszban talál információt.

A Data Factory-megoldás tárolja GE történész hello használata előtt győződjön meg arról, hogy hello-átjáró képes kapcsolódni a útmutatást a következő szakaszban hello toohello adattár.

A másolási műveletek a forrás-adattároló hello a cikk elolvasása részletes áttekintés hello elejétől ODBC adatok használatával tárolja.  

## <a name="troubleshoot-connectivity-issues"></a>Csatlakozási problémák
tootroubleshoot csatlakozási problémáit, használja a hello **diagnosztika** lapján **az adatkezelési átjáró konfigurációkezelőjének**.

1. Indítsa el **az adatkezelési átjáró konfigurációkezelőjének**. Futtatásával "C:\Program Files\Microsoft Data felügyeleti Gateway\1.0\Shared\ConfigManager.exe" közvetlenül (vagy) keresés a **átjáró** toofind hivatkozás túl**Microsoft adatkezelési átjáró** ahogy az a következő kép hello alkalmazás.

    ![Keresési átjáró](./media/data-factory-odbc-connector/search-gateway.png)
2. Váltás toohello **diagnosztika** fülre.

    ![Átjáró diagnosztika](./media/data-factory-odbc-connector/data-factory-gateway-diagnostics.png)
3. Jelölje be hello **típus** adatok tárolására (társított szolgáltatás).
4. Adja meg **hitelesítési** , és írja be **hitelesítő adatok** (vagy) megadása **kapcsolati karakterlánc** használt tooconnect toohello adattár ez.
5. Kattintson a **tesztkapcsolat** tootest hello kapcsolat toohello adattár.

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
