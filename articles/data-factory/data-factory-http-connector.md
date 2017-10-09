---
title: "aaaMove adatforrásból származó egy HTTP - Azure |} Microsoft Docs"
description: "További információk a hogyan toomove egy helyszíni vagy HTTP felhő adatforrás Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: e39b9cbff870aef4be91938cacff39a2fd12d64a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-http-source-using-azure-data-factory"></a>Adatok áthelyezése az Azure Data Factory használatával HTTP forrásból származó
Ez a cikk ismerteti, hogyan toouse hello másolási tevékenység az Azure Data Factory toomove adatait egy helyszíni/felhőbeli HTTP-végpont tooa támogatott fogadó adattár. Ez a cikk épít, hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely megadja a másolási tevékenység és hello listáját támogatott adatforrások/mosdók adattárolókhoz adatmozgás általános áttekintést.

Adat-előállító jelenleg csak az adatok áthelyezése HTTP által támogatott forrás tooother adattárolókhoz, de tooan HTTP-cél nem adatok áthelyezését más adatokat tárolja.

## <a name="supported-scenarios-and-authentication-types"></a>Támogatott esetek és hitelesítési típusok
A HTTP összekötő tooretrieve adatait is használhat **felhő- és a helyszíni HTTP/s-végpont** HTTP-n keresztül **beolvasása** vagy **POST** metódus. a következő hitelesítési típusok hello támogatottak: **névtelen**, **alapvető**, **kivonatoló**, **Windows**, és  **ClientCertificate**. Megjegyzés: az összekötő és hello hello különbségének [webes tábla összekötő](data-factory-web-table-connector.md) van: Ez utóbbi hello használt tooextract tábla HTML weblapról tartalom.

Amikor adatokat másol egy helyszíni HTTP-végpont, hello a helyszíni környezetben vagy az Azure virtuális gép adatkezelési átjárót kell telepítenie. Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, amely HTTP forrásból származó adatokat különböző eszközök/API-k használatával helyezi át a feldolgozási sor hozhatja létre.

- hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

- Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. JSON-minták HTTP forrás tooAzure Blob Storage toocopy adatait, a következő témakörben: [JSON példák](#json-examples) Ez a cikk szakasza.

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
a következő táblázat hello biztosít JSON elemek adott tooHTTP kapcsolódó szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type | hello type tulajdonságot kell beállítani: `Http`. | Igen |
| URL-címe | Alap URL-cím toohello webkiszolgáló | Igen |
| AuthenticationType | Hello hitelesítés típusát határozza meg. Két érték engedélyezett: **névtelen**, **alapvető**, **kivonatoló**, **Windows**, **ClientCertificate**. <br><br> További tulajdonságokat és JSON-minták a táblázat alatti toosections rendre adott hitelesítési típusok olvassa. | Igen |
| enableServerCertificateValidation | Adja meg, hogy tooenable server SSL tanúsítvány érvényesítése, ha forrás HTTPS webkiszolgáló | Nem, alapértelmezett érték true |
| gatewayName | Hello az adatkezelési átjáró tooconnect tooan nevét a helyszíni HTTP-forrás. | Igen, ha a helyszíni HTTP forrásból származó adatok másolása. |
| encryptedCredential | Titkosított hitelesítő adatokban tooaccess hello HTTP-végpont. Automatikusan létrehozott hello hitelesítési adatok másolása varázsló vagy a hello ClickOnce előugró párbeszédpanelen konfigurálásakor. | Nem. Csak akkor, ha az adatok másolása helyi HTTP-kiszolgáló alkalmazni. |

Lásd: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró hello felhő között](data-factory-move-data-between-onprem-and-cloud.md) helyszíni HTTP összekötő adatforráshoz tartozó hitelesítő adatok beállítása vonatkozó további információért.

### <a name="using-basic-digest-or-windows-authentication"></a>Basic, a kivonatoló vagy a Windows-hitelesítés használatával

Állítsa be `authenticationType` , `Basic`, `Digest`, vagy `Windows`, és adja meg a következő tulajdonságai módosításokon kívül HTTP összekötő általános azokat, a fenti bevezetett hello hello:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| felhasználónév | Felhasználónév tooaccess hello HTTP-végpont. | Igen |
| jelszó | (Felhasználónév) hello felhasználó jelszavát. | Igen |

#### <a name="example-using-basic-digest-or-windows-authentication"></a>Példa: Basic, a kivonatoló vagy a Windows-hitelesítés használatával

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "basic",
            "url" : "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a>ClientCertificate hitelesítéssel

Alapszintű hitelesítés toouse, állítsa be `authenticationType` , `ClientCertificate`, és adja meg a következő tulajdonságai módosításokon kívül HTTP összekötő általános azokat, a fenti bevezetett hello hello:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| embeddedCertData | hello Base64-kódolású tartalmak a bináris adatok hello személyes információcseréhez kapcsolódó (PFX) fájl. | Adja meg vagy hello `embeddedCertData` vagy `certThumbprint`. |
| CertThumbprint | hello telepítve lett az átjáró gépen tanúsítványtároló hello tanúsítvány ujjlenyomata. Csak akkor, ha a helyszíni HTTP forrásból származó adat másolása alkalmazni. | Adja meg vagy hello `embeddedCertData` vagy `certThumbprint`. |
| jelszó | Hello tanúsítványhoz tartozó jelszót. | Nem |

Ha `certThumbprint` hitelesítési és hello tanúsítvány telepítve van a hello hello helyi számítógép személyes tárolójában, kell toogrant hello olvasási hozzáférést toohello átjáró szolgáltatás:

1. Indítsa el a Microsoft Management Console (MMC). Adja hozzá a hello **tanúsítványok** beépülő modul a célok hello **helyi számítógép**.
2. Bontsa ki a **tanúsítványok**, **személyes**, és kattintson a **tanúsítványok**.
3. Kattintson a jobb gombbal a hello tanúsítványt hello személyes tárolójában, és válassza ki **feladataival**->**titkos kulcsok kezelése...**
3. A hello **biztonsági** lapon maradva adja hozzá a hello felhasználói fiók alatt az adatkezelési átjáró gazdaszolgáltatása fut. hello olvasási hozzáférés toohello tanúsítvánnyal.  

#### <a name="example-using-client-certificate"></a>Példa: ügyfél tanúsítványt használ.
Ez a data factory tooan helyszíni HTTP webkiszolgáló kapcsolódó szolgáltatás hivatkozásokat. Az adatkezelési átjáró telepített hello gépen telepített ügyféltanúsítványt használ.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"

        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a>Példa: ügyfél-tanúsítványt használ egy fájlban
Ez a data factory tooan helyszíni HTTP webkiszolgáló kapcsolódó szolgáltatás hivatkozásokat. Az adatkezelési átjáró telepítése egy ügyfél tanúsítványfájl hello gépen használ.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. hello typeProperties szakasz típusú adatkészlet **Http** rendelkezik hello következő tulajdonságai

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type | A megadott hello dataset hello típusú. be kell állítani túl`Http`. | Igen |
| relativeUrl | Relatív URL-cím toohello erőforrás hello adatokat tartalmaz. Ha nincs megadva elérési út, kapcsolódó hello szolgáltatásdefinícióban megadott csak hello URL szolgál. <br><br> tooconstruct dinamikus URL-címe, használhat [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md), pl. "relativeUrl": "$$Text.Format (" / személyes/jelentés? hónap = {0:yyyy}-{0:MM} & fmt = csv ", SliceStart)". | Nem |
| requestMethod | HTTP-metódus. Két érték engedélyezett **beolvasása** vagy **POST**. | Nem. Alapértelmezett érték a `GET`. |
| additionalHeaders | További HTTP-kérelemfejlécekben. | Nem |
| requestBody | A HTTP-kérelmek törzsében. | Nem |
| Formátumban | Ha azt szeretné, hogy toosimply **hello adatainak lekérése, a HTTP-végpont-van** nélkül elemzés azt, hagyja ki a formátumot beállítások. <br><br> Ha szeretné tooparse hello HTTP-válasz tartalom másolása során, a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. |Nem |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**. Támogatott szintek a következők: **Optimal** és **leggyorsabb**. További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem |

### <a name="example-using-hello-get-default-method"></a>Példa: hello (alapértelmezett) GET metódussal

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

### <a name="example-using-hello-post-method"></a>Példa: hello POST metódussal

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
           "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
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

Tulajdonságok érhetők el hello **typeProperties** szakasz hello hello tevékenységekre ugyanakkor tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

Ha a másolási tevékenység hello forrás jelenleg típusú **HttpSource**, a következő tulajdonságok hello támogatott.

| Tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| httpRequestTimeout | hello hello HTTP kérelem tooget választ (időtartam) időkorlátját. Hello időtúllépés tooget választ, hello időtúllépés tooread érkezett válasz adatait is. | Nem. Alapértelmezett érték: 00:01:40 |

## <a name="supported-file-and-compression-formats"></a>Támogatott formátumú és tömörítés
Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk részletei.

## <a name="json-examples"></a>JSON-példák
a következő példa hello adja meg a minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azok bemutatják, hogyan toocopy HTTP adatforrás tooAzure Blob-tároló. Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott hello nyelő források tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.

### <a name="example-copy-data-from-http-source-tooazure-blob-storage"></a>Példa: Adatok másolása az HTTP forrás tooAzure Blob-tároló
hello Ez a minta Data Factory-megoldást a következő adat-előállító entitások hello tartalmazza:

1. A társított szolgáltatás típusa [HTTP](#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [Http](#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [HttpSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másol adatokat egy HTTP-forrás tooan Azure blob minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

### <a name="http-linked-service"></a>Kapcsolódó HTTP-szolgáltatás
Ebben a példában használt hello HTTP társított szolgáltatás névtelen hitelesítést. Lásd: [HTTP társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Azure Storage társított szolgáltatás

```JSON
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

### <a name="http-input-dataset"></a>A bemeneti HTTP-adatkészlet
Beállítás **külső** túl**igaz** hello Data Factory szolgáltatásnak tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}

```

### <a name="azure-blob-output-dataset"></a>Azure Blob kimeneti adatkészlet

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).

```JSON
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

### <a name="pipeline-with-copy-activity"></a>A másolási tevékenység-feldolgozási folyamat

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**HttpSource** és **fogadó** típusuk értéke túl**BlobSink**.

Lásd: [HttpSource](#copy-activity-properties) hello HttpSource által támogatott tulajdonságokról hello listáját.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "HttpSourceToAzureBlob",
        "description": "Copy from an HTTP source tooan Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "HttpSourceDataInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "HttpSource"
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

> [!NOTE]
> Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
