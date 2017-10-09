---
title: az Azure Cosmos DB aaaMove adatok |} Microsoft Docs
description: "Megtudhatja, hogyan helyezi át az adatokat az Azure Data Factory használatához Azure Cosmos DB gyűjtemény"
services: data-factory, cosmosdb
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c9297b71-1bb4-4b29-ba3c-4cf1f5575fac
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: bd23ce4e004a972ce6f3e4165cfdea4f0c18fecc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-cosmos-db-using-azure-data-factory"></a>Adatok tooand áthelyezése az Azure Data Factory használatához Azure Cosmos-Adatbázisból
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatok Azure Cosmos DB (a DocumentDB API) és a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során. 

Bármely támogatott forráshierarchiából adatokat tooAzure Cosmos-adatbázis tárolásához, vagy az Azure Cosmos DB támogatott tooany fogadó adatok tárolására adatainak másolhatja. Adatforrások vagy mosdók hello másolási tevékenység által támogatott adattárolókhoz listáját lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. 

> [!IMPORTANT]
> Azure Cosmos DB összekötő csak a DocumentDB API támogatja.

az adatok toocopy-van/JSON-fájlokat vagy egy másik Cosmos DB gyűjteményhez, lásd: [Import/Export JSON-dokumentumok](#importexport-json-documents).

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, mely az adatok Azure Cosmos DB és a különböző eszközök/API-k használatával létrehozhat egy folyamatot.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello: 

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Minták használt toocopy adatok Cosmos DB az adat-előállító entitások JSON-definíciók, lásd: [JSON példák](#json-examples) című szakaszát. 

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooCosmos DB részleteit tartalmazzák: 

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
a következő táblázat hello biztosít JSON-elemek adott tooAzure Cosmos DB kapcsolódó szolgáltatás leírását.

| **Tulajdonság** | **Leírás** | **Szükséges** |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **DocumentDb** |Igen |
| connectionString |Adja meg a szükséges adatok tooconnect tooAzure Cosmos DB adatbázisban. |Igen |

Példa:

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
A szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listájáért tekintse meg az toohello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például a struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. hello typeProperties szakasz hello adatkészlet típusú **DocumentDbCollection** hello alábbi tulajdonságokkal rendelkezik.

| **Tulajdonság** | **Leírás** | **Szükséges** |
| --- | --- | --- |
| CollectionName |Hello Cosmos DB dokumentumgyűjteményt neve. |Igen |

Példa:

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
### <a name="schema-by-data-factory"></a>Adat-előállító sémája
Sémamentesadat-tárolókhoz, például az Azure Cosmos DB a Data Factory szolgáltatásnak hello kikövetkezteti hello séma a következő módokon hello egyikében:  

1. Ha megadja az adatok szerkezete hello hello segítségével **struktúra** hello adatkészlet-definícióban hello Data Factory szolgáltatásnak tulajdonság eleget tegyen hello séma szerint ez a struktúra. Ebben az esetben ha egy sort tartalmaz egy olyan oszlop értékét, null értékű nyújtanak az.
2. Ha nem ad meg adatok szerkezete hello hello segítségével **struktúra** hello adatkészlet-definícióban hello Data Factory szolgáltatásnak tulajdonság hello séma kikövetkezteti hello adatok első sorának hello segítségével. Ebben az esetben ha hello első sor nem tartalmaz teljes séma hello, néhány oszlop nem érhető el a másolási művelet hello eredményét.

Sémamentes adatforrások hello célszerű ezért hello segítségével adatok szerkezete toospecify hello **struktúra** tulajdonság.

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
A szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listájáért tekintse meg az toohello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.

> [!NOTE]
> hello másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.

Tulajdonságok hello hello tevékenységekre hello typeProperties szakaszában érhető el ugyanakkor eltérők lehetnek a tevékenységek minden típusának és források és mosdók hello típusától függően változik, a másolási tevékenység esetén.

Ha forrás típusa másolási tevékenység esetén **DocumentDbCollectionSource** hello a következő tulajdonságok érhetők el **typeProperties** szakasz:

| **Tulajdonság** | **Leírás** | **Megengedett értékek** | **Szükséges** |
| --- | --- | --- | --- |
| lekérdezés |Adja meg a hello tooread adatait kérdezi le. |Lekérdezés-karakterlánc hossza Azure Cosmos DB által támogatott. <br/><br/>Példa:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Nem <br/><br/>Ha nincs megadva, hello végrehajtott SQL-utasítást:`select <columns defined in structure> from mycollection` |
| nestingSeparator |Speciális karakter tooindicate, hogy a dokumentum hello van beágyazva. |Bármely karakter. <br/><br/>Azure Cosmos-adatbázis egy NoSQL-tároló JSON-dokumentumok, amelyben beágyazott struktúrákat engedélyezett. Az Azure Data Factory lehetővé teszi, hogy a felhasználó toodenote hierarchia keresztül nestingSeparator, amely "." a fenti példák hello. Hello elválasztóval hello másolási tevékenység hello "Name" objektum három gyermekek elemekkel hozza létre első, középső és utolsó, függően too"Name.First", "Name.Middle" és "Name.Last" hello a tábla megadása. |Nem |

**DocumentDbCollectionSink** következő tulajdonságai hello támogatja:

| **Tulajdonság** | **Leírás** | **Megengedett értékek** | **Szükséges** |
| --- | --- | --- | --- |
| nestingSeparator |A különleges karakterek hello forrás oszlop neve tooindicate, amely a beágyazott dokumentumok van szükség. <br/><br/>Például fent: `Name.First` hello kimeneti táblát hoz létre hello JSON struktúrában hello Cosmos DB dokumentumban a következő:<br/><br/>"Name": {<br/>    "Első": "John"<br/>}, |Az karakter, amely használt tooseparate beágyazási szinttel.<br/><br/>Alapértelmezett érték `.` (pont). |Az karakter, amely használt tooseparate beágyazási szinttel. <br/><br/>Alapértelmezett érték `.` (pont). |
| WriteBatchSize |A lekérdezések tooAzure Cosmos DB toocreate dokumentumok párhuzamos száma.<br/><br/>Hello teljesítmény úgy finomhangolhatja, és a Cosmos DB adatok másolásakor e tulajdonság használatával. A jobb teljesítmény számíthat, mivel a rendszer további kérelmeket párhuzamos tooCosmos DB elküldi a writeBatchSize növelésével. Azonban szüksége lesz, amelyek sávszélesség-szabályozás tooavoid is throw hello hibaüzenet: "Ez nagy lekérő".<br/><br/>Sávszélesség-szabályozás tényező, beleértve a dokumentumok, a dokumentumok számát méretét, indexelő házirend célgyűjteményt stb határoz meg. A másolási műveletek, használhat egy jobb gyűjtemény (pl. S3) toohave hello legtöbb átviteli sebesség érhető el (2500 kérés egység/másodperc). |Egész szám |Nem (alapértelmezett: 5) |
| writeBatchTimeout |Várnia kell az hello művelet toocomplete előtt azt az időkorlátot. |A TimeSpan<br/><br/> Példa: "00: 30:00" (30 perc). |Nem |

## <a name="importexport-json-documents"></a>Importálási/exportálási JSON-dokumentumok
A Cosmos DB összekötő használatával egyszerűen

* Importálja a JSON-dokumentumok különböző forrásokból Cosmos DB Azure Blob, beleértve az Azure Data Lake, a helyszíni fájlrendszer vagy egyéb fájlalapú tárolók Azure Data Factory által támogatott.
* A Cosmos DB collecton JSON-dokumentumok exportálása különböző fájlalapú tárolók.
* Adatok áttelepítése között két Cosmos DB gyűjteményeket-van.

tooachieve ilyen séma-független másolja, 
* Másolása varázsló segítségével, hogy hello **"exportálni-tooJSON fájlok vagy Cosmos DB gyűjtemény"** lehetőséget.
* Ha JSON szerkesztésére használatával nem adnak meg hello "structure" szakasz Cosmos DB adatkészlet(ek), sem "nestingSeparator" tulajdonságának Cosmos DB forrás/fogadó a másolási tevékenység. a tooimport / tooJSON fájlok, hello fájl tároló adatkészlet adja meg formázási típusa "JsonFormat", "filePattern" konfigurációs és kihagyása hello rest formátum beállításainak exportálásához lásd: [JSON formátumban](data-factory-supported-file-and-compression-formats.md#json-format) részletei szakaszban.

## <a name="json-examples"></a>JSON-példák
hello alábbi példák megadják minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azok hogyan toocopy adatok tooand Azure Cosmos DB és az Azure Blob Storage tárolóban. Azonban az adatok átmásolhatók **közvetlenül** bármelyik hello források tooany közölt hello nyelő [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.

## <a name="example-copy-data-from-azure-cosmos-db-tooazure-blob"></a>Példa: Adatok másolása az Azure Cosmos DB tooAzure Blob
az alábbi hello minta mutatja:

1. A társított szolgáltatás típusa [DocumentDb](#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [DocumentDbCollection](#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [DocumentDbCollectionSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta Azure Cosmos DB tooAzure Blob másolja az adatokat. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

**A társított szolgáltatásnak Azure Cosmos DB:**

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
**Az Azure Blob storage társított szolgáltatásnak:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Az Azure Document DB rendszerbe bemeneti adatkészletet:**

hello példa feltételezi, hogy rendelkezik-e egy nevű gyűjtemény **személy** Azure Cosmos DB adatbázisban.

"External" beállítása: "true", és megadása externalData házirenddel kapcsolatos információk az Azure Data Factory hello szolgáltatás hello tábla külső toohello adat-előállítót, adat-előállítóban hello tevékenység nem eredményezett.

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Az Azure Blob kimeneti adatkészlet:**

Adata másolt tooa új blob tükröző hello adott dátum és idő órában lépésköz hello BLOB hello elérési úttal rendelkező óránként.

```JSON
{
  "name": "PersonBlobTableOut",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
A minta JSON-dokumentum a hello személy gyűjtemény egy Cosmos DB adatbázisban:

```JSON
{
  "PersonId": 2,
  "Name": {
    "First": "Jane",
    "Middle": "",
    "Last": "Doe"
  }
}
```
Cosmos DB támogatja a JSON-dokumentumok hierarchikus over szintaxis például egy SQL használatával dokumentumok lekérdezését.

Példa: 

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

hello következő csővezeték-másolatok adatait hello személy hello Azure Cosmos DB adatbázis tooan Azure blob-gyűjteményt érinti. Hello másolási tevékenység hello részeként bemeneti és kimeneti adatkészletek van megadva.  

```JSON
{
  "name": "DocDbToBlobPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "DocumentDbCollectionSource",
            "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
            "nestingSeparator": "."
          },
          "sink": {
            "type": "BlobSink",
            "blobWriterAddHeader": true,
            "writeBatchSize": 1000,
            "writeBatchTimeout": "00:00:59"
          }
        },
        "inputs": [
          {
            "name": "PersonCosmosDbTable"
          }
        ],
        "outputs": [
          {
            "name": "PersonBlobTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromDocDbToBlob"
      }
    ],
    "start": "2015-04-01T00:00:00Z",
    "end": "2015-04-02T00:00:00Z"
  }
}
```
## <a name="example-copy-data-from-azure-blob-tooazure-cosmos-db"></a>Példa: Adatok másolása az Azure Blob tooAzure Cosmos DB 
az alábbi hello minta mutatja:

1. A társított szolgáltatás típusa [DocumentDb](#azure-documentdb-linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [DocumentDbCollection](#azure-documentdb-dataset-type-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).

hello minta Azure blob tooAzure Cosmos DB másol adatokat. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

**Az Azure Blob storage társított szolgáltatásnak:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**A társított szolgáltatásnak Azure Cosmos DB:**

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
**Az Azure Blob bemeneti adatkészletet:**

```JSON
{
  "name": "PersonBlobTableIn",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "MiddleName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "fileName": "input.csv",
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
**Az Azure Cosmos DB kimeneti adatkészlet:**

hello minta "Személy" nevű tooa adatgyűjtés másolja.

```JSON
{
  "name": "PersonCosmosDbTableOut",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "Name.First",
        "type": "String"
      },
      {
        "name": "Name.Middle",
        "type": "String"
      },
      {
        "name": "Name.Last",
        "type": "String"
      }
    ],
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
hello következő Azure Blob toohello hello Cosmos DB személy gyűjtemény másolatok adatait a következő feldolgozási sorban. Hello másolási tevékenység hello részeként bemeneti és kimeneti adatkészletek van megadva.

```JSON
{
  "name": "BlobToDocDbPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "DocumentDbCollectionSink",
            "nestingSeparator": ".",
            "writeBatchSize": 2,
            "writeBatchTimeout": "00:00:00"
          }
          "translator": {
              "type": "TabularTranslator",
              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, title: aaaTitle, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
          }
        },
        "inputs": [
          {
            "name": "PersonBlobTableIn"
          }
        ],
        "outputs": [
          {
            "name": "PersonCosmosDbTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromBlobToDocDb"
      }
    ],
    "start": "2015-04-14T00:00:00Z",
    "end": "2015-04-15T00:00:00Z"
  }
}
```
Ha hello minta blob bemeneti van, mint

```
1,John,,Doe
```
Hello kimeneti JSON-t Cosmos DB lesz majd megfelelően:

```JSON
{
  "Id": 1,
  "Name": {
    "First": "John",
    "Middle": null,
    "Last": "Doe"
  },
  "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
}
```
Azure Cosmos-adatbázis egy NoSQL-tároló JSON-dokumentumok, amelyben beágyazott struktúrákat engedélyezett. Az Azure Data Factory lehetővé teszi, hogy a felhasználó toodenote hierarchia keresztül **nestingSeparator**, amely "." Ebben a példában. Hello elválasztóval hello másolási tevékenység hello "Name" objektum három gyermekek elemekkel hozza létre első, középső és utolsó, függően too"Name.First", "Name.Middle" és "Name.Last" hello a tábla megadása.

## <a name="appendix"></a>Függelék:
1. **Kérdés:** hello másolási tevékenység támogatási frissítés a meglévő rekordok?

    **Válasz:** nem.
2. **Kérdés:** hogyan működik már egy másolás tooAzure Cosmos DB foglalkozzon ismétlését másolt rekordok?

    **Válasz:** ha rögzíti egy "ID" mezőt és hello másolási művelet megkísérli tooinsert hello rekord azonos azonosítója, hello másolási művelet hibát jelez.  
3. **Kérdés:** nem támogatja a Data Factory [tartományt vagy a kivonat-alapú adatparticionálás](../documentdb/documentdb-partition-data.md)?

    **Válasz:** nem.
4. **Kérdés:** állítható be egy táblához több Azure Cosmos DB gyűjtemény?

    **Válasz:** nem. Jelenleg csak egy gyűjteményhez adható meg.

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
