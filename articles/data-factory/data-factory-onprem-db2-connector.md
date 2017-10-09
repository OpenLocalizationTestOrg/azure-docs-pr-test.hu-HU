---
title: "Azure Data Factory használatával aaaMove DB2 adatokat |} Microsoft Docs"
description: "Ismerje meg, hogyan toomove adatait egy helyszíni DB2-adatbázis használati ideje Azure Data Factory másolási tevékenység használatával"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c1644e17-4560-46bb-bf3c-b923126671f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.openlocfilehash: 696ac059be644cb3901c37d2fc746e0682c65a1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-db2-by-using-azure-data-factory-copy-activity"></a>Azure Data Factory másolási tevékenység segítségével DB2 tárolt adatok mozgatása
Ez a cikk ismerteti, hogyan használhatja ki egy a helyszíni DB2 adatbázis tooa adatok Azure Data Factory toocopy adatok másolási tevékenység. Másolhatja tooany adattároló, amely egy támogatott fogadó a hello blokkolandóként [adat-előállító adatok mozgása tevékenységek](data-factory-data-movement-activities.md#supported-data-stores-and-formats) cikk. Ez a témakör hello adat-előállító cikket, amely áttekintést nyújt az adatátvitelt jelölik a másolási tevékenység segítségével, és felsorolja a támogatott hello adatokat tároló kombinációk épül. 

Adat-előállító jelenleg csak áthelyezése adatait egy DB2-adatbázishoz tooa [támogatott fogadó adattár](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Adatok áthelyezése más adatokat tárolja tooa DB2-adatbázishoz nem támogatott.

## <a name="prerequisites"></a>Előfeltételek
Adat-előállító támogatja a helyszíni DB2-adatbázishoz kapcsolódó tooan hello [az adatkezelési átjáró](data-factory-data-management-gateway.md). Részletes útmutatás tooset hello átjáró adatok feldolgozási toomove az adatok című hello [tárolt adatok mozgatása a helyszíni toocloud](data-factory-move-data-between-onprem-and-cloud.md) cikk.

Átjáróra szükség, akkor is, ha hello DB2 üzemelteti Azure IaaS virtuális Gépen. Hello átjáró telepíthető hello ugyanabból az infrastruktúra-szolgáltatási virtuális hello adatok tárolóként. Ha hello-átjáró képes kapcsolódni toohello adatbázis, hello átjárót telepítheti egy másik virtuális gépen.

hello az adatkezelési átjáró egy beépített DB2-illesztőprogram biztosít, így nem toomanually telepíteni kell egy illesztőprogram toocopy adatok DB2.

> [!NOTE]
> Kapcsolat és az átjáró problémák hibaelhárításával kapcsolatos tippek, lásd: hello [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) cikk.


## <a name="supported-versions"></a>Támogatott verziók
hello Data Factory DB2-összekötő a következő IBM DB2-platformokat, 9, 10-es és 11 elosztott relációs adatbázis architektúra (DRDA) SQL hozzáférés-kezelési verzióival verziók hello támogatja:

* IBM DB2 z/os 11.1 verziója
* IBM DB2 z/OS 10.1 verziója
* IBM DB2-i (AS400) verziójának 7.2
* IBM DB2-i (AS400) 7.1-es verziójához
* IBM DB2 Linux, UNIX és a Windows (LUW) 11-es verzió
* IBM DB2 a LUW 10.5 verziója
* IBM DB2 a LUW 10.1 verziója

> [!TIP]
> Ha hello hibaüzenet jelenhet meg: "hello csomag megfelelő tooan SQL utasítás végrehajtási kérelem nem található. SQLSTATE 51002 SQLCODE =-805, = "hello ennek az oka, a szükséges csomag nem jön létre a normál felhasználói hello hello az operációs rendszer. tooresolve probléma, kövesse ezeket az utasításokat a DB2-kiszolgáló típusának:
> - A DB2 i (AS400): a másolási tevékenység futtatása előtt hello hello normál felhasználói gyűjtemény létrehozása kiemelt felhasználó segítségével. toocreate hello gyűjtemény, hello parancsot használja:`create collection <username>`
> - A z/OS- vagy LUW DB2: használja a magas jogosultságú fiók – egy kiemelt felhasználói vagy a felügyeleti csomag hitelesítésszolgáltatók és a kötési, BINDADD, ENGEDÉLYEKET EXECUTE tooPUBLIC – egyszer toorun hello másolása. szükséges csomag hello hello másolása során automatikusan létrejön. Ezt követően hátsó toohello normál felhasználói válthat a későbbi másolási kísérletekhez.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység toomove származó adatokkal egy helyszíni DB2 adattároló adatcsatorna különböző eszközöket és API-k használatával hozhat létre: 

- hello legegyszerűbb módja toocreate adatcsatorna toouse hello Azure Data Factory másolása varázsló. Egy folyamat létrehozásával hello másolása varázsló használatával gyorsan útmutatást lásd: hello [oktatóanyag: hozzon létre egy folyamatot hello másolása varázsló használatával](data-factory-copy-data-wizard-tutorial.md). 
- Is használhatja eszközök toocreate egy folyamatot, beleértve a hello Azure-portálon, a Visual Studio, Azure PowerShell, az Azure Resource Manager sablon, hello .NET API és hello REST API-t. Részletes útmutatás toocreate a másolási tevékenység során a folyamat, lásd: hello [másolási tevékenység az oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Társított szolgáltatások toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállító létrehozása.
2. Létrehozni adatkészletek toorepresent bemeneti és kimeneti adatai hello másolási művelet. 
3. Hozzon létre egy folyamatot, amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet a másolási tevékenység. 

Hello másolása varázsló JSON-definíciók hello Data Factory kapcsolt szolgáltatások esetén használatakor adatkészleteket, és a folyamat entitások automatikusan létrejönnek. (Kivéve a hello .NET API-t) eszközök vagy API-k használata esetén definiálni hello adat-előállító entitások hello JSON formátumban. Hello [JSON-példa: adatok másolása az DB2 tooAzure Blob-tároló](#json-example-copy-data-from-db2-to-azure-blob) hello JSON definícióit hello adat-előállító entitások, amelyek egy helyszíni DB2 adattároló használt toocopy adatait mutatja.

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine hello adat-előállító entitások, amelyek adott tooa DB2-adattár hello részleteit tartalmazzák.

## <a name="db2-linked-service-properties"></a>Kapcsolódó DB2 szolgáltatás tulajdonságai
a következő táblázat hello hello JSON tulajdonságokhoz adott tooa DB2 kapcsolódó szolgáltatás sorolja fel.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| **típusa** |Ez a tulajdonság túl be kell állítani**OnPremisesDB2**. |Igen |
| **kiszolgáló** |hello hello DB2-kiszolgáló nevét. |Igen |
| **adatbázis** |hello hello DB2-adatbázis neve. |Igen |
| **séma** |hello neve hello séma hello DB2-adatbázisban. Ez a tulajdonság a kis-és nagybetűket. |Nem |
| **authenticationType** |hello használt tooconnect toohello DB2 adatbázis hitelesítés típusát. hello lehetséges értékek a következők: névtelen, alapszintű és a Windows. |Igen |
| **felhasználónév** |hello felhasználói fiók egyszerű vagy Windows-hitelesítés használata esetén hello nevét. |Nem |
| **jelszó** |hello hello felhasználói fiókhoz tartozó jelszót. |Nem |
| **gatewayName** |hello nevét, amely a Data Factory szolgáltatásnak hello hello átjáró használjon tooconnect toohello helyszíni DB2-adatbázishoz. |Igen |

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Hello, illetve meghatározásához adatkészletek rendelkezésre álló tulajdonságok listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Szakasz, például a **struktúra**, **rendelkezésre állási**, és hello **házirend** adatkészlet JSON hasonlóak az összes adatkészlet esetében (Azure SQL, Azure Blob storage Azure Table tárolási, és így tovább).

Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. Hello **typeProperties** szakasz egy adatkészlet típusú **RelationalTable**, amely hello DB2 adatkészletet tartalmaz, akkor a következő tulajdonság hello rendelkezik:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| **Táblanév** |hello tábla társított szolgáltatás hello hello DB2-adatbázispéldány neve hello hivatkozik. Ez a tulajdonság a kis-és nagybetűket. |Nem (ha hello **lekérdezés** tulajdonság típusa másolási tevékenység **RelationalSource** van megadva) |

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Hello, illetve a másolási tevékenység meghatározásához rendelkezésre álló tulajdonságok listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Másolja a tevékenység tulajdonságai, például a **neve**, **leírása**, **bemenetek** tábla, **kimenete** tábla, és **házirend**, tevékenységi érhetők el. hello elérhető tulajdonságok hello **typeProperties** szakasz tevékenységek minden típusának hello tevékenységét. A másolási tevékenység során hello tulajdonságok hello típusú adatforrások és mosdók függenek.

Másolási tevékenységhez, ha hello adatforrás típusú **RelationalSource** (amely tartalmazza a DB2 rendszerhez), a következő tulajdonságok hello érhetők el hello **typeProperties** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| **lekérdezés** |Hello egyéni lekérdezés tooread hello adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például:`"query": "select * from "MySchema"."MyTable""` |Nem (ha hello **tableName** a DataSet adatkészlet tulajdonság meg van adva) |

> [!NOTE]
> Séma-és tábla-és nagybetűk. Hello lekérdezés utasításban, tegye a tulajdonságnevek "" (idézőjelek). Példa:
>
> ```sql
> "query": "select * from "DB2ADMIN"."Customers""
> ```

## <a name="json-example-copy-data-from-db2-tooazure-blob-storage"></a>JSON-példa: adatok másolása az DB2 tooAzure Blob-tároló
Ebben a példában a minta JSON definícióit tartalmazza használható toocreate adatcsatorna hello segítségével [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). hello példa bemutatja, hogyan adatbázis toocopy adatait egy DB2-tooBlob tároló. Azonban adatokat másolhat túl[bármely támogatott adatok tárolásához a fogadó típusa](data-factory-data-movement-activities.md#supported-data-stores-and-formats) Azure Data Factory másolási tevékenység használatával.

hello minta a következő adat-előállító entitások hello rendelkezik:

- Egy DB2 társított szolgáltatás típusa [OnPremisesDb2](data-factory-onprem-db2-connector.md#linked-service-properties)
- Egy Azure Blob storage társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
- Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)
- Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
- A [csővezeték](data-factory-create-pipelines.md) hello használó másolása tevékenységgel [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) tulajdonságai

hello minta másol adatokat egy DB2-adatbázishoz tooan Azure blob egy lekérdezés eredményt óránként. hello entitás definíciók hello szakaszok hello mintában használt hello JSON tulajdonságokat ismerteti.

Első lépésként telepítse, és konfigurálja a data gateway. Útmutatás a hello el [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.

**DB2 társított szolgáltatás**

```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
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

**Az Azure Blob storage társított szolgáltatás**

```json
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

**DB2 bemeneti adatkészlet**

hello példa feltételezi, hogy létrehozott egy táblát a "MyTable", oszlop, "időbélyeg" hello idő adatsorozat adatok feliratú nevű DB2.

Hello **külső** tulajdonsága túl "true." Ez a beállítás arról értesíti hello Data Factory szolgáltatásnak, hogy ez az adatkészlet külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák. Figyelje meg, hogy hello **típus** tulajdonsága túl**RelationalTable**.


```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
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

Adatok írják tooa új blob óránként hello beállítása **gyakoriság** tulajdonság túl "Hour", és hello **időköz** tulajdonság too1. Hello **folderPath** tulajdonság hello blob dinamikusan ki lesz értékelve az alapján a hello szelet által feldolgozott hello kezdési idejét. hello mappa elérési útját használja hello év, hónap, nap és óra részei hello kezdési ideje.

```json
{
    "name": "AzureBlobDb2DataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Feldolgozási sor hello másolási tevékenységhez**

hello feldolgozási sor tartalmazza a másolási tevékenység során konfigurált toouse megadott bemeneti és kimeneti adatkészletek, és amelyet ütemezett toorun óránként. A hello hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és hello **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság kiválasztja hello adatokat hello "Rendelések" táblából.

```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for hello copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"Orders\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "Db2DataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDb2DataSet"
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
                "name": "Db2ToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="type-mapping-for-db2"></a>Típusleképezés a DB2 rendszerhez
Hello a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység az automatikus típuskonverziók típus toosink forrástípus hello kétlépéses módszer a következő segítségével hajtja végre:

1. Egy natív típusa tooa .NET forrástípus konvertálása
2. A .NET típusú tooa natív a fogadó típusa konvertálása

hello következő megfeleltetéseket használata, amikor másolási tevékenység hello adatok konvertál egy DB2 tooa .NET típusának:

| DB2-adatbázishoz típusa | .NET-keretrendszer típusa |
| --- | --- |
| SmallInt |Int16 |
| Egész szám |Int32 |
| BigInt |Int64 |
| Real |Egyetlen |
| Dupla |Dupla |
| Lebegőpontos |Dupla |
| Decimális |Decimális |
| DecimalFloat |Decimális |
| Numerikus |Decimális |
| Dátum |Dátum és idő |
| Time |A TimeSpan |
| időbélyeg |Dátum és idő |
| XML |Byte] |
| Karakter |Karakterlánc |
| VarChar |Karakterlánc |
| LongVarChar |Karakterlánc |
| DB2DynArray |Karakterlánc |
| Bináris |Byte] |
| VarBinary |Byte] |
| LongVarBinary |Byte] |
| Kép |Karakterlánc |
| VarGraphic |Karakterlánc |
| LongVarGraphic |Karakterlánc |
| CLOB |Karakterlánc |
| Blob |Byte] |
| DbClob |Karakterlánc |
| SmallInt |Int16 |
| Egész szám |Int32 |
| BigInt |Int64 |
| Real |Egyetlen |
| Dupla |Dupla |
| Lebegőpontos |Dupla |
| Decimális |Decimális |
| DecimalFloat |Decimális |
| Numerikus |Decimális |
| Dátum |Dátum és idő |
| Time |A TimeSpan |
| időbélyeg |Dátum és idő |
| XML |Byte] |
| Karakter |Karakterlánc |

## <a name="map-source-toosink-columns"></a>A forrásoszlopokat toosink leképezése
Hogyan toomap hello forrás adatkészlet toocolumns hello fogadó adatkészletben, oszlopok: toolearn [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-reads-from-relational-sources"></a>A relációs források ismételhető olvasási műveletek
Amikor adatokat másolni relációs adattároló, ismételhetőség tartsa szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is hello újrapróbálkozási **házirend** tulajdonsága egy adatkészlet toorerun a szelet, ha hiba történik. Győződjön meg arról, hogy hello ugyanazokat az adatokat hogyan olvasható függetlenül attól, hogy hányszor hello szelet a rendszer Újrafuttatás, függetlenül attól, milyen hello szelet futtassa újra. További információkért lásd: [Repeatable olvassa be az relációs források](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Teljesítmény és finomhangolás
További tudnivalók a másolási tevékenység és módon toooptimize teljesítmény hello hello teljesítményét befolyásoló legfontosabb tényezők [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md).
