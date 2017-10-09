---
title: "Azure Data Factory használatával webes tábla aaaMove adatait |} Microsoft Docs"
description: "További információk a hogyan egy tábla egy webes toomove adatait lapon Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f54a26a4-baa4-4255-9791-5a8f935898e2
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: e52216305583ebbe71ed896522f361bb22f01278
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a>Adatok áthelyezése az Azure Data Factory használatával webes tábla forrásból
Ez a cikk ismerteti, hogyan toouse hello másolási tevékenység az Azure Data Factory toomove adatok egy táblából egy weblap tooa támogatott fogadó adattár. Ez a cikk épít, hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely megadja a másolási tevékenység és hello listáját támogatott adatforrások/mosdók adattárolókhoz adatmozgás általános áttekintést.

Adat-előállító jelenleg csak egy webes tábla tooother adatok áthelyezése adatait tárolja, de nem az adatok áthelyezése más adatok tooa webes tábla cél tárolja.

> [!IMPORTANT]
> A webalkalmazás-összekötőjének jelenleg csak kibontása tábla tartalma HTML-lapon. a HTTP/s végpont tooretrieve adatait használja [HTTP összekötő](data-factory-http-connector.md) helyette.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot. 

- hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást. 
- Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek egy webes tábla használt toocopy adatait JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az webes tábla tooAzure Blob](#json-example-copy-data-from-web-table-to-azure-blob) című szakaszát. 

a következő szakaszok hello használt toodefine adat-előállító entitások adott tooa webes tábla JSON-tulajdonághoz részleteit tartalmazzák:

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
a következő táblázat hello biztosít JSON elemek adott tooWeb kapcsolódó szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **webes** |Igen |
| URL-cím |URL-cím toohello webes forrás |Igen |
| AuthenticationType |Névtelen. |Igen |

### <a name="using-anonymous-authentication"></a>A névtelen hitelesítés segítségével

```json
{
    "name": "web",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. hello typeProperties szakasz típusú adatkészlet **Webtábla** rendelkezik hello következő tulajdonságai

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type |Hello dataset típusa. be kell állítani túl**Webtábla** |Igen |
| Elérési út |Relatív URL-cím toohello erőforrás hello táblát tartalmaz. |Nem. Ha nincs megadva elérési út, kapcsolódó hello szolgáltatásdefinícióban megadott csak hello URL szolgál. |
| Index |hello tábla hello erőforrás hello indexe. Lásd: [Get index egy tábla egy HTML-lapon](#get-index-of-a-table-in-an-html-page) szakasz lépéseit toogetting index egy tábla egy HTML-lapon. |Igen |

**Példa**

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.

Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

Ha a másolási tevékenység hello forrás jelenleg típusú **WebSource**, további tulajdonságok nem támogatottak.


## <a name="json-example-copy-data-from-web-table-tooazure-blob"></a>JSON-példa: adatok másolása az webes tábla tooAzure Blob
a következő példa azt mutatja be hello:

1. A társított szolgáltatás típusa [webes](#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [Webtábla](#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [WebSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másol adatokat egy webes tábla tooan Azure blob minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

a következő minta hello jeleníti meg, hogyan toocopy adatait egy webes tábla tooan Azure blob-e. Azonban adatok átmásolhatók, közvetlenül a hello tooany fogadók esetében a megadott hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk hello másolási tevékenység az Azure Data Factory használatával.

**Webes társított szolgáltatás** ebben a példában használt webes hello társított szolgáltatás névtelen hitelesítést. Lásd: [webes társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.

```json
{
    "name": "WebLinkedService",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
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

**Webtábla bemeneti adatkészlet** beállítás **külső** túl**igaz** hello Data Factory szolgáltatásnak tájékoztatja, hogy hello dataset külső toohello adat-előállító, és nem hozzák hello tevékenységet adat-előállítóban.

> [!NOTE]
> Lásd: [Get index egy tábla egy HTML-lapon](#get-index-of-a-table-in-an-html-page) szakasz lépéseit toogetting index egy tábla egy HTML-lapon.  
>
>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```


**Az Azure Blob kimeneti adatkészlet**

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
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```



**A másolási tevékenység-feldolgozási folyamat**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**WebSource** és **fogadó** típusuk értéke túl**BlobSink**.

Lásd: [WebSource típustulajdonságokat](#copy-activity-type-properties) hello WebSource által támogatott tulajdonságokról hello listáját.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "WebTableToAzureBlob",
        "description": "Copy from a Web table tooan Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "WebTableInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "WebSource"
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

## <a name="get-index-of-a-table-in-an-html-page"></a>Egy tábla indexének lekérése egy HTML-weblap
1. Indítsa el **Excel 2016** és toohello **adatok** fülre.  
2. Kattintson a **új lekérdezés** hello eszköztár pont túl**egyéb forrásokból származó** kattintson **a webes**.

    ![A Power Query menü](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. A hello **a webes** párbeszédpanelen adja meg a **URL-cím** használható a társított szolgáltatás JSON (például: https://en.wikipedia.org/wiki/) elérési út lehet megadni hello adatkészlet együtt (például: AFI % 27s_100_Years... 100_Movies), és kattintson a **OK**.

    ![Webes párbeszédpanelről](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    Ebben a példában használt URL-cím: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. Ha látja **hozzáférés webes tartalom** párbeszédpanel megnyitásához, jelölje be hello jobb **URL-cím**, **hitelesítési**, és kattintson a **Connect**.

   ![Webes tartalom párbeszédpanel](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. Kattintson egy **tábla** hello fa nézet toosee tartalom hello táblából elemét, majd **szerkesztése** hello alsó gombra.  

   ![Navigator párbeszédpanel](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. A hello **Lekérdezésszerkesztő** ablak, kattintson a **speciális szerkesztő** hello gombjára.

    ![Speciális szerkesztő gomb](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. A hello speciális szerkesztő párbeszédpanel hello szám következő túl "Forrás" hello index.

    ![Speciális szerkesztő - Index](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

Ha az Excel 2013 használ, [Microsoft Power Query az Excel programhoz](https://www.microsoft.com/download/details.aspx?id=39379) tooget hello index. Lásd: [Connect tooa weblap](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) cikkben alább. hello lépések hasonlóak használata [Microsoft Power BI Desktop az](https://powerbi.microsoft.com/desktop/).

> [!NOTE]
> Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
