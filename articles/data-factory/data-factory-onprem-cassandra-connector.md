---
title: "a Data Factory használatával Cassandra aaaMove adatok |} Microsoft Docs"
description: "További információk a hogyan toomove adatait egy helyszíni Cassandra adatbázis-Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 085cc312-42ca-4f43-aa35-535b35a102d5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: 0e265d3a8439d0a2cb2a5c32e5ea8348a1617621
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Adatok áthelyezése az Azure Data Factory használatával a helyszíni Cassandra adatbázisból
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove az adatbázisból a helyszíni Cassandra a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

Egy helyszíni Cassandra adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja. Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. Adat-előállító jelenleg csak egy Cassandra adatokból adattároló tooother adattárolókhoz áthelyezése, de nem adatok áthelyezését más adatok tárolók tooa Cassandra adatok tárolására. 

## <a name="supported-versions"></a>Támogatott verziók
hello Cassandra összekötő támogatja a következő verziói Cassandra hello: 2.X.

## <a name="prerequisites"></a>Előfeltételek
Hello Azure Data Factory szolgáltatás toobe képes tooconnect tooyour helyszíni Cassandra adatbázisához, adatkezelési átjárót kell telepítenie a hello azonos számítógépre, hogy az állomások hello adatbázis vagy egy másik számítógépre tooavoid versengő hello erőforrás az adatbázis. Az adatkezelési átjáró összetevő, amely összeköti a helyszíni adatok források toocloud szolgáltatások biztonságának és kezelésének módja. Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) szóló cikkben olvashat az adatkezelési átjáró. Lásd: [tárolt adatok mozgatása a helyszíni toocloud](data-factory-move-data-between-onprem-and-cloud.md) cikk hello átjáró egy adatok adatcsatorna toomove adatok beállításának lépéseit.

Hello átjáró tooconnect tooa Cassandra adatbázis kell használnia, akkor is, ha hello adatbázis egy hello felhőben, például egy Azure IaaS virtuális gépen. Hello átjáró lehet Y hello ugyanazon VM állomások hello adatbázis, vagy egy külön virtuális gépre mindaddig hello átjáró képes kapcsolódni toohello adatbázis.  

Hello átjáró telepítésekor automatikusan telepíti a Microsoft Cassandra ODBC használt tooconnect tooCassandra-adatbázishoz. Emiatt nem kell toomanually hello-átjáró számítógépén bármely illesztőprogram telepítése a hello Cassandra adatbázisból származó adatok másolásakor. 

> [!NOTE]
> Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot. 

- hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást. 
- Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek egy helyszíni Cassandra adattárolóból használt toocopy adatok JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Cassandra tooAzure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) című szakaszát. 

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooa Cassandra adattár részleteit tartalmazzák:

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
a következő táblázat hello biztosít JSON elemek adott tooCassandra kapcsolódó szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **OnPremisesCassandra** |Igen |
| állomás |Egy vagy több IP-címek vagy Cassandra kiszolgálók állomás nevét.<br/><br/>IP-címek vagy nevek tooconnect tooall kiszolgálók vesszővel tagolt listáját adja meg a egyidejűleg. |Igen |
| port |hello hello Cassandra server TCP-port toolisten ügyfél-kommunikációhoz használ. |Nem, alapértelmezett érték: 9042 |
| AuthenticationType |Basic vagy Anonymous |Igen |
| felhasználónév |Adja meg a hello felhasználói fiókhoz tartozó felhasználónevet. |Igen, ha authenticationType tooBasic van beállítva. |
| jelszó |Adja meg a hello felhasználói fiókhoz tartozó jelszót. |Igen, ha authenticationType tooBasic van beállítva. |
| gatewayName |hello hello átjáró, amely használt tooconnect toohello helyszíni Cassandra adatbázis neve. |Igen |
| encryptedCredential |A hitelesítő adatok hello átjáró titkosítja. |Nem |

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. hello typeProperties szakasz típusú adatkészlet **CassandraTable** rendelkezik hello következő tulajdonságai

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| kulcstérértesítések használatával |Hello kulcstérértesítések használatával vagy a séma Cassandra adatbázis neve. |Igen (Ha **lekérdezés** a **CassandraSource** nincs definiálva). |
| tableName |Hello tábla Cassandra adatbázis neve. |Igen (Ha **lekérdezés** a **CassandraSource** nincs definiálva). |

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.

Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

Ha a forrás típusa van **CassandraSource**, typeProperties szakaszában érhetők hello következő tulajdonságai:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-92 vagy CQL lekérdezés. Lásd: [CQL hivatkozás](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>SQL-lekérdezés használata esetén adja meg a **kulcstérértesítések használatával name.table neve** tooquery kívánt toorepresent hello tábla. |Nem (ha van megadva a tableName és a dataset kulcstérértesítések használatával). |
| consistencyLevel |hello konzisztencia szint határozza meg, hány replikák tooa olvasási kérelem kell válaszolnia kell a visszatérésre adatok toohello ügyfélalkalmazás. Cassandra ellenőrzések hello replikák megadott számú, az adatok toosatisfy hello olvasni a kérelmet. |EGY, KETTŐ, HÁROM, KVÓRUM, AZ ÖSSZES, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Lásd: [konfigurálása az adatok konzisztenciájának](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) részleteiről. |Nem. Alapértelmezett érték: egyet. |

## <a name="json-example-copy-data-from-cassandra-tooazure-blob"></a>JSON-példa: adatok másolása az Cassandra tooAzure Blob
Ebben a példában a minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azt illusztrálja, hogyan toocopy adatait egy helyszíni Cassandra adatbázis tooan Azure Blob Storage tárolóban. Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.

> [!IMPORTANT]
> Ez a minta JSON kódtöredékek biztosít. Nem tartalmazza a hello adat-előállító létrehozásának részletes leírása. Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.

hello minta a következő data factory entitások hello rendelkezik:

* A társított szolgáltatás típusa [OnPremisesCassandra](#linked-service-properties).
* A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bemeneti [dataset](data-factory-create-datasets.md) típusú [CassandraTable](#dataset-properties).
* Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [CassandraSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

**Cassandra társított szolgáltatáshoz:**

Ez a példa hello **Cassandra** társított szolgáltatás. Lásd: [Cassandra társított szolgáltatás](#linked-service-properties) szolgáltatásnak által támogatott hello tulajdonságok szakaszát.  

```json
{
    "name": "CassandraLinkedService",
    "properties":
    {
        "type": "OnPremisesCassandra",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "host": "mycassandraserver",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "mygateway"
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

**Cassandra bemeneti adatkészlet:**

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "mykeyspace"
        },
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

Beállítás **külső** túl**igaz** hello Data Factory szolgáltatásnak tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

**Az Azure Blob kimeneti adatkészlet:**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/fromcassandra"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Másolási tevékenység Cassandra és Blob fogadó egy folyamaton belül:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**CassandraSource** és **fogadó** típusuk értéke túl**BlobSink**.

Lásd: [RelationalSource típustulajdonságokat](#copy-activity-properties) hello RelationalSource által támogatott tulajdonságokról hello listáját.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra tooan Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "CassandraInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"

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

### <a name="type-mapping-for-cassandra"></a>Típus identitástagok esetén Cassandra
| Cassandra típusa | .NET-alapú típusa |
| --- | --- |
| ASCII |Karakterlánc |
| BIGINT |Int64 |
| A BLOB |Byte] |
| LOGIKAI ÉRTÉK |Logikai érték |
| DECIMÁLIS |Decimális |
| DUPLA |Dupla |
| LEBEGŐPONTOS |Egyetlen |
| INET |Karakterlánc |
| INT |Int32 |
| SZÖVEG |Karakterlánc |
| IDŐBÉLYEG |Dátum és idő |
| TIMEUUID |GUID |
| UUID |GUID |
| VARCHAR |Karakterlánc |
| VARINT |Decimális |

> [!NOTE]
> A gyűjtemény típusa (térkép, set, lista stb.), tekintse meg a túl[virtuális tábla használatával Cassandra gyűjteménytípusok együttműködve](#work-with-collections-using-virtual-table) szakasz.
>
> Felhasználó által definiált típusok nem támogatottak.
>
> Bináris oszlop és karakterlánc-oszlopnak hosszúságú hello hossza nem lehet nagyobb, mint 4000.
>
>

## <a name="work-with-collections-using-virtual-table"></a>Virtuális tábla használatával gyűjtemények használata
Az Azure Data Factory használ egy beépített ODBC illesztőprogram tooconnect tooand másolása az Cassandra adatbázis adatait. A térkép, a készlet és a lista gyűjtemény típusú hello illesztőprogram hello adatok renormalizes megfelelő virtuális táblákba. Pontosabban Ha egy tábla gyűjtemény oszlopot tartalmaz, a hello illesztőprogram a következő virtuális táblák hello állít elő:

* A **alaptábla**, amely tartalmazza hello hello valós táblából hello gyűjtemény oszlopok ugyanazokat az adatokat. hello alaptábla hello ugyanazt a nevet használja, mint hello valós táblázatot, amely azt jelöli.
* A **virtuális tábla** gyűjtemény oszlopok, amely bővíti a beágyazott hello adatok. hello virtuális táblákat, amelyek megfelelnek a gyűjtemények használatával hello hello valós tábla neve, az elválasztó elnevezése "*vt*" és a hello hello oszlop neve.

Virtuális táblák tekintse meg a hello valós tábla toohello adatokat, lehetővé teszi hello illesztőprogram tooaccess hello nem normalizált adatok. Lásd például című szakaszban talál információt. Cassandra gyűjtemények hello tartalmának lekérdezése és hello virtuális tábla érheti el.

Használhatja a hello [másolása varázsló](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively nézet hello hello virtuális táblázatot is Cassandra adatbázis táblák listáját és hello található adatok megtekintésére. Is hello másolása varázsló az olyan lekérdezést, és toosee hello eredmény ellenőrzése.

### <a name="example"></a>Példa
Például hello következő "ExampleTable", amely tartalmazza az egész elsődleges kulcsként megadott oszlop "pk_int", érték nevű szöveges oszlop, egy listaoszlop, térkép oszlop és egy oszlopkészlet ("StringSet" nevű) nevű Cassandra adatbázistábla.

| pk_int | Érték | Lista | térkép | StringSet |
| --- | --- | --- | --- | --- |
| 1 |"a minta érték 1" |["1", "2", "3"] |{"S1": "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"a minta 3. érték" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

hello illesztőprogram több virtuális táblák toorepresent hoz létre a egyetlen tábla. hello külső kulcs oszlopokra hello virtuális táblák elsődleges kulcsát alkotó hello hello valós tábla hivatkozik, és jelezze mely valós tábla sor hello virtuális tábla sorainak felel meg.

első virtuális tábla hello hello alaptábla "ExampleTable" nevű hello a következő táblázatban látható. hello alaptábla tartalmaz hello hello eredeti adatbázis táblából hello gyűjteményeket, amelyek ebből a táblázatból nincs megadva, és más virtuális táblák kibontva ugyanazokat az adatokat.

| pk_int | Érték |
| --- | --- |
| 1 |"a minta érték 1" |
| 3 |"a minta 3. érték" |

hello alábbi táblázatokban hello virtuális táblákhoz renormalize hello lista, térkép és StringSet oszlopok hello adatait. hello oszlopok kiderül, hogy a "_index" vagy "_kulcsvédelmi" adja meg hello adatainak hello eredeti lista vagy a térkép hello pozícióját. hello oszlopok kiderül, hogy a "_value" végződhet hello gyűjtemény kibontva hello adatait tartalmazzák.

#### <a name="table-exampletablevtlist"></a>"ExampleTable_vt_List". tábla:
| pk_int | List_index | List_value |
| --- | --- | --- |
| 1 |0 |1 |
| 1 |1 |2 |
| 1 |2 |3 |
| 3 |0 |100 |
| 3 |1 |101 |
| 3 |2 |102 |
| 3 |3 |103 |

#### <a name="table-exampletablevtmap"></a>"ExampleTable_vt_Map". tábla:
| pk_int | Map_key | Map_value |
| --- | --- | --- |
| 1 |S1 |A |
| 1 |S2 |B |
| 3 |S1 |T |

#### <a name="table-exampletablevtstringset"></a>"ExampleTable_vt_StringSet". tábla:
| pk_int | StringSet_value |
| --- | --- |
| 1 |A |
| 1 |B |
| 1 |C# |
| 3 |A |
| 3 |E |

## <a name="map-source-toosink-columns"></a>A forrásoszlopokat toosink leképezése
toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>A relációs források ismételhető Olvasás
Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása. Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
