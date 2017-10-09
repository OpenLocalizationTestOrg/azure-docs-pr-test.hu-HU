---
title: "Azure Data Factory használatával az FTP-kiszolgáló adatait aaaMove |} Microsoft Docs"
description: "Megtudhatja, hogyan toomove adatokat az Azure Data Factory használatával FTP-kiszolgálóhoz."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: eea3bab0-a6e4-4045-ad44-9ce06229c718
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: jingwang
ms.openlocfilehash: c707e29532b2a8a870603948cb6150ab857bd6ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-ftp-server-by-using-azure-data-factory"></a>Adatok áthelyezése az FTP-kiszolgáló Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse hello másolási tevékenység az Azure Data Factory toomove adatok az FTP-kiszolgálóhoz. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

Az FTP kiszolgáló támogatott tooany fogadó adattár adatainak másolhatja. Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. Adat-előállító jelenleg csak egy FTP-kiszolgáló tooother adatok áthelyezése adatait tárolja, de nem az adatok áthelyezése más adatok tooan FTP-kiszolgáló tárolja. Az támogatja-e mind a helyszíni és felhőalapú FTP-kiszolgálók.

> [!NOTE]
> hello másolási tevékenység nem hello forrásfájl törlése, miután sikeresen másolt toohello cél. Ha egy sikeres másolása után toodelete hello forrásfájl van szüksége, hozzon létre egy egyéni tevékenység toodelete hello fájlt, és hello tevékenység hello folyamat. 

## <a name="enable-connectivity"></a>Kapcsolat engedélyezése
Ha áthelyezi adatait egy **helyszíni** FTP server tooa felhőalapú adattároló (már például tooAzure Blob Storage tárolóban van hátra), telepítéséhez, és az adatkezelési átjáró használatához. Az adatkezelési átjáró hello olyan ügyfélügynök, amelynek a helyszíni gépre van telepítve, és lehetővé teszi a felhőalapú szolgáltatások tooconnect tooan helyszíni erőforrás. További információkért lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md). Hello átjáró beállításával és használatával, akkor a részletes útmutatót lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md). Akkor is, ha hello server az Azure-infrastruktúra (IaaS) szolgáltatás virtuális gépként (VM) hello átjáró tooconnect tooan FTP-kiszolgáló használ.

Már lehetséges tooinstall hello átjáró hello megegyezik a helyszíni gépen vagy infrastruktúra-szolgáltatási virtuális gép hello FTP-kiszolgálóhoz. Azt javasoljuk azonban hello átjáró telepítése egy másik számítógépre, vagy az infrastruktúra-szolgáltatási virtuális gép tooavoid Erőforrásverseny, és a jobb teljesítmény érdekében. Hello átjáró egy külön számítógépen való telepítésekor hello gépnek képes tooaccess hello FTP-kiszolgálón kell lennie.

## <a name="get-started"></a>Bevezetés
A másolási tevékenység, amely FTP forrásból származó adatokat a különböző eszközök vagy API-k használatával helyezi át a feldolgozási sor hozhatja létre.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **Data Factory másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök vagy API-k (kivéve a .NET API-t) használ, akkor határozzák meg a Data Factory entitások hello JSON formátumban. Az adat-előállító entitások, amelyek egy FTP-adattároló használt toocopy adatait JSON-definíciók minta, lásd: hello [JSON-példa: adatok másolása az FTP-kiszolgáló tooAzure blobból](#json-example-copy-data-from-ftp-server-to-azure-blob) című szakaszát.

> [!NOTE]
> Fájl- és tömörítési formátum támogatott toouse kapcsolatos részletekért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md).

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooFTP részleteit tartalmazzák.

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
hello a következő táblázat ismerteti a JSON elemek adott tooan kapcsolódó FTP-szolgáltatás.

| Tulajdonság | Leírás | Szükséges | Alapértelmezett |
| --- | --- | --- | --- |
| type |Állítsa be a tooFtpServer. |Igen |&nbsp; |
| állomás |Adja meg a hello nevét vagy IP-cím hello FTP-kiszolgáló. |Igen |&nbsp; |
| AuthenticationType |Adja meg a hello hitelesítési típus. |Igen |Alapszintű, a névtelen |
| felhasználónév |Adja meg a hello felhasználó, aki rendelkezik hozzáférési toohello FTP-kiszolgálóhoz. |Nem |&nbsp; |
| jelszó |Adja meg (felhasználónév) hello felhasználó hello jelszavát. |Nem |&nbsp; |
| encryptedCredential |Adja meg a hello titkosított hitelesítő adat tooaccess hello FTP-kiszolgálót. |Nem |&nbsp; |
| gatewayName |Adja meg hello hello átjárót az adatkezelési átjáró tooconnect tooan helyszíni FTP-kiszolgáló. |Nem |&nbsp; |
| port |Adja meg, melyik hello FTP-kiszolgáló figyel hello port. |Nem |21 |
| enableSsl |Adja meg, hogy toouse FTP SSL/TLS-csatornán keresztül. |Nem |Igaz |
| enableServerCertificateValidation |Adja meg, hogy tooenable server SSL tanúsítvány érvényesítése a TLS/SSL csatornán keresztül FTP használata esetén. |Nem |Igaz |

### <a name="use-anonymous-authentication"></a>Névtelen hitelesítés

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {        
            "authenticationType": "Anonymous",
              "host": "myftpserver.com"
        }
    }
}
```

### <a name="use-username-and-password-in-plain-text-for-basic-authentication"></a>Felhasználónevet és jelszót egyszerű szövegként használja az egyszerű hitelesítés

```JSON
{
    "name": "FTPLinkedService",
      "properties": {
    "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
}
```

### <a name="use-port-enablessl-enableservercertificatevalidation"></a>Használja a portot, enableSsl, enableServerCertificateValidation

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

### <a name="use-encryptedcredential-for-authentication-and-gateway"></a>A hitelesítés és az átjáró encryptedCredential használata

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "mygateway"
        }
      }
}
```

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok és meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: [adatkészletek létrehozása](data-factory-create-datasets.md). Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében.

Hello **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú. Adott toohello adathalmaztípushoz információkat biztosít. Hello **typeProperties** szakasz egy adatkészlet típusú **fájlmegosztási** rendelkezik hello következő tulajdonságai:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| folderPath |Részleges toohello mappát. Használja az escape-karakter "\" hello karakterlánc speciális karakter. Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.<br/><br/>Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdési és befejezési dátum-idő. |Igen |
| fileName |Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában. Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.<br/><br/>Ha **Fájlnév** nincs megadva egy kimeneti adatkészlet hello név hello létrehozott fájl formátuma a következő hello van: <br/><br/>Adatok. <Guid>.txt (Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Nem |
| fileFilter |Adja meg a szűrő használt toobe tooselect fájlok egy részének hello **folderPath**, ahelyett, hogy minden fájl.<br/><br/>Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).<br/><br/>1. példa:`"fileFilter": "*.log"`<br/>2. példa:`"fileFilter": 2014-1-?.txt"`<br/><br/> **fileFilter** egy bemeneti fájlmegosztási adatkészlet esetében alkalmazható. Ez a tulajdonság nem támogatott a Hadoop elosztott fájlrendszerrel (HDFS). |Nem |
| partitionedBy |Dinamikus toospecify használt **folderPath** és **Fájlnév** idő adatsorozat adatok. Megadhat például egy **folderPath** , amely az adatok óránkénti paraméteres. |Nem |
| Formátumban | a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket. További információkért lásd: hello [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátumban ](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. <br><br> Ha azt szeretné toocopy fájlok, mivel ezek között a fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban. |Nem |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**, és a támogatott szintek a következők **Optimal** és **leggyorsabb**. További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem |
| useBinaryTransfer |Adja meg, hogy toouse hello bináris átviteli módot. hello értékek: true bináris mód (Ez az alapértelmezett érték hello), és hamis értéket ASCII. Ez a tulajdonság csak használható, ha hello társított társított szolgáltatás típusa típusú: FTP-kiszolgáló. |Nem |

> [!NOTE]
> **Fájlnév** és **fileFilter** nem használható egyszerre.

### <a name="use-hello-partionedby-property"></a>Hello partionedBy tulajdonság
Hello előző szakaszban említett, megadhat egy dinamikus **folderPath** és **Fájlnév** idő adatsorozat adatokhoz hello **partitionedBy** tulajdonság.

toolearn idő adatsorozat adatkészleteket, ütemezés és szeletek, lásd: [adatkészletek létrehozása](data-factory-create-datasets.md), [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md), és [folyamatok létrehozása](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>1. példa

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
Ebben a példában {szelet} adat-előállító rendszer változó SliceStart hello érték helyére, a hello formázza a megadott (YYYYMMDDHH). hello SliceStart toostart idő hello szelet hivatkozik. hello mappa elérési útja eltér az egyes szeletek. (Például wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104.)

#### <a name="sample-2"></a>2. példa

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
Ebben a példában hello év, hónap, nap és SliceStart idején ki kell olvasni a külön változók hello által használt **folderPath** és **Fájlnév** tulajdonságait.

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok és a rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: [folyamatok létrehozása](data-factory-create-pipelines.md). Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.

Tulajdonságok érhetők el hello **typeProperties** szakasza hello tevékenységet, hello ugyanakkor, tevékenységek minden típusának függenek. Hello másolási tevékenységhez hello típustulajdonságokat hello típusú források és mosdók függenek.

A másolási tevékenység, ha hello adatforrás típusú **FileSystemSource**, a következő tulajdonság hello érhető el **typeProperties** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| Rekurzív |Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák, vagy csak a megadott mappa hello. |IGAZ, hamis (alapértelmezés) |Nem |

## <a name="json-example-copy-data-from-ftp-server-tooazure-blob"></a>JSON-példa: adatok másolása az FTP-kiszolgáló tooAzure Blob
Ez a példa bemutatja, hogyan egy FTP-kiszolgáló tooAzure Blob-tároló toocopy adatait. Azonban adatok átmásolhatók, közvetlenül a hello tooany fogadók esetében a megadott hello [adatokról és formátumok támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats), adat-előállítóban hello másolási tevékenység használatával.  

hello alábbi példák megadják minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):

* A társított szolgáltatás típusa [FTP-kiszolgáló](#linked-service-properties)
* A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
* Bemeneti [dataset](data-factory-create-datasets.md) típusú [fájlmegosztás](#dataset-properties)
* Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [FileSystemSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)

hello minta másol adatokat az FTP-kiszolgáló tooan Azure blob minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

### <a name="ftp-linked-service"></a>Kapcsolódó FTP-szolgáltatás

Ebben a példában hello felhasználónevet és jelszót a szövegként egyszerű hitelesítést használ. A következő módokon hello egyikét is használhatja:

* A névtelen hitelesítés
* Egyszerű hitelesítés titkosított hitelesítő adatokkal
* FTP-keresztül SSL/TLS (ftps-t)

Lásd: hello [FTP társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
    "type": "FtpServer",
    "typeProperties": {
        "host": "myftpserver.com",           
        "authenticationType": "Basic",
        "username": "Admin",
        "password": "123456"
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
### <a name="ftp-input-dataset"></a>FTP-bemeneti adatkészlet

Ez az adatkészlet hivatkozik toohello FTP mappa `mysharedfolder` és fájl `test.csv`. hello csővezeték hello fájl toohello cél másolja.

Beállítás **külső** túl**igaz** hello Data Factory szolgáltatásnak tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```JSON
{
  "name": "FTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "FTPLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv",
      "useBinaryTransfer": true
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

### <a name="azure-blob-output-dataset"></a>Azure Blob kimeneti adatkészlet

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli ki, hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útját használja hello év, hónap, nap és óra részei hello kezdési ideje.

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="a-copy-activity-in-a-pipeline-with-file-system-source-and-blob-sink"></a>A másolási tevékenység során a rendszer forrás- és a blob fájlgyűjtő egy folyamaton belül

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek, és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**FileSystemSource**, és hello **fogadó** típusuk értéke túl**BlobSink**.

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
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
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00Z",
        "end": "2016-08-24T19:00:00Z"
    }
}
```
> [!NOTE]
> Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="next-steps"></a>Következő lépések
Tekintse meg a következő cikkek hello:

* toolearn kulccsal kapcsolatos tényezők adatátvitelt jelölik a (másolási tevékenység) az adat-előállítót, és különböző módokon toooptimize hatás teljesítmény, lásd: hello [másolása tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md).

* A másolási tevékenység során a folyamat létrehozása részletes ismertetését lásd: hello [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
