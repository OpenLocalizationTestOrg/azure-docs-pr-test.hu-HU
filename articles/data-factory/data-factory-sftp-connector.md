---
title: "Azure Data Factory használatával SFTP kiszolgálóról aaaMove adatok |} Microsoft Docs"
description: "Megtudhatja, hogyan egy helyszíni vagy egy Azure Data Factory használatával felhő SFTP server toomove adatait."
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
ms.date: 06/05/2017
ms.author: jingwang
ms.openlocfilehash: b976289e2c1b1899634bb5565b375499077fa554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-sftp-server-using-azure-data-factory"></a>Adatok áthelyezése az Azure Data Factory használatával SFTP-kiszolgálóról
Ez a cikk ismerteti, hogyan toouse hello másolási tevékenység az Azure Data Factory toomove adatait egy helyszíni/felhőbeli SFTP server tooa támogatott fogadó adattár. Ez a cikk épít, hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely megadja a másolási tevékenység és hello listáját támogatott adatforrások/mosdók adattárolókhoz adatmozgás általános áttekintést.

Adat-előállító jelenleg csak áthelyezése egy SFTP kiszolgálóadatok tooother adatait tárolja, de nem támogatja az adatok áthelyezését más adatok tooan SFTP kiszolgáló tárolja. Az támogatja-e mind a helyszíni és felhőalapú SFTP kiszolgálók.

> [!NOTE]
> Másolási tevékenység nem hello forrásfájl törlése, miután sikeresen másolt toohello cél. Ha toodelete hello forrásfájl után sikeres másolatát, hozzon létre egy egyéni tevékenység toodelete hello fájlt, és a hello tevékenység hello folyamat. 

## <a name="supported-scenarios-and-authentication-types"></a>Támogatott esetek és hitelesítési típusok
Használhatja az SFTP összekötő toocopy adatait **mind a felhőalapú SFTP és a helyszíni SFTP kiszolgálók**. **Alapszintű** és **SshPublicKey** hitelesítési típusok támogatottak toohello SFTP kiszolgálóhoz kapcsolódáskor.

Adatok áttelepítését egy helyszíni SFTP másolásakor hello a helyszíni környezetben vagy az Azure virtuális gép adatkezelési átjárót kell telepítenie. Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) hello átjáró leírását. Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) lépésenként hello átjáró beállításával és használatával, a cikkben találhat.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, amely helyezi át az adatokat SFTP forrásból származó különböző eszközök/API-k használatával hozhatja létre egy folyamatot.

- hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

- Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. JSON-minták SFTP server tooAzure Blob Storage toocopy adatait, a következő témakörben: [JSON-példa: adatok másolása az SFTP server tooAzure blobból](#json-example-copy-data-from-sftp-server-to-azure-blob) című szakaszát.

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
a következő táblázat hello biztosít JSON elemek adott tooFTP kapcsolódó szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- | --- |
| type | hello type tulajdonság túl be kell állítani`Sftp`. |Igen |
| állomás | Hello SFTP kiszolgáló neve vagy IP-címét. |Igen |
| port |Az port, mely hello SFTP kiszolgáló figyel. hello alapértelmezett érték: 21. |Nem |
| AuthenticationType |Adja meg a hitelesítés típusát. Megengedett értékek: **alapvető**, **SshPublicKey**. <br><br> Tekintse meg a túl[használja az egyszerű hitelesítés](#using-basic-authentication) és [használatával SSH nyilvános kulcsos hitelesítés](#using-ssh-public-key-authentication) további tulajdonságokat és JSON-minták szakasz. |Igen |
| skipHostKeyValidation | Adja meg, hogy tooskip gazdagép kulcs érvényesítése. | Nem. alapértelmezett érték hello: hamis |
| hostKeyFingerprint | Adja meg a hello ujjlenyomat hello állomás kulcs. | Igen, ha a hello `skipHostKeyValidation` toofalse van beállítva.  |
| gatewayName |Hello az adatkezelési átjáró tooconnect tooan nevét a helyszíni SFTP kiszolgáló. | Igen, ha az adatok másolása egy helyszíni SFTP-kiszolgálón. |
| encryptedCredential | Titkosított hitelesítő adatokban tooaccess hello SFTP kiszolgáló. Automatikusan létrehozott Ha megadja az egyszerű hitelesítés (felhasználónév + jelszó) vagy az SshPublicKey hitelesítési (felhasználónév + titkos kulcs elérési útja vagy tartalom) másolása varázsló vagy a hello ClickOnce előugró párbeszédpanelen. | Nem. Csak akkor, ha az adatok másolása egy helyszíni SFTP kiszolgáló alkalmazni. |

### <a name="using-basic-authentication"></a>Alapszintű hitelesítést használó

Alapszintű hitelesítés toouse, állítsa be `authenticationType` , `Basic`, és adja meg a következő tulajdonságai módosításokon kívül SFTP összekötő hello utolsó szakaszában bevezetett általános ők hello hello:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- | --- |
| felhasználónév | Az felhasználó, aki rendelkezik toohello SFTP kiszolgálót. |Igen |
| jelszó | (Felhasználónév) hello felhasználó jelszavát. | Igen |

#### <a name="example-basic-authentication"></a>Példa: Az egyszerű hitelesítés
```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Példa: Alapszintű hitelesítési titkosított hitelesítő adat

```JSON
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
      }
}
```

### <a name="using-ssh-public-key-authentication"></a>SSH nyilvános kulcsos hitelesítés használatával

toouse SSH nyilvános kulcsos hitelesítés beállítása `authenticationType` , `SshPublicKey`, és adja meg a következő tulajdonságai módosításokon kívül SFTP összekötő hello utolsó szakaszában bevezetett általános ők hello hello:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- | --- |
| felhasználónév |Felhasználó, aki rendelkezik toohello SFTP kiszolgáló |Igen |
| privateKeyPath | Adjon meg abszolút elérési út toohello titkos kulcsot tartalmazó fájlt, hogy az átjáró férhetnek hozzá. | Adja meg vagy hello `privateKeyPath` vagy `privateKeyContent`. <br><br> Csak akkor, ha az adatok másolása egy helyszíni SFTP kiszolgáló alkalmazni. |
| privateKeyContent | Hello titkos kulcs tartalmát, mivel a szerializált karakterlánc. hello másolása varázsló olvashatja hello titkos kulcsot tartalmazó fájlt, és bontsa ki a titkos kulcs tartalmát hello automatikusan. Minden egyéb eszköz/SDK használatakor használja hello privateKeyPath tulajdonságot. | Adja meg vagy hello `privateKeyPath` vagy `privateKeyContent`. |
| hozzáférési kód | Adja meg hello hozzáférési kódot vagy jelszót toodecrypt hello titkos kulcsot, ha hello kulcsfájl védi egy hozzáférési kódot. | Igen, ha a hozzáférési kód hello titkos kulcsot tartalmazó fájlt védi. |

> [!NOTE]
> SFTP-összekötő csak támogatja a protokoll OpenSSH-kulcsot. Ellenőrizze, hogy a kulcsfájl hello megfelelő formátumban van. Putty eszközt tooconvert .ppk tooOpenSSH formátumból is használhatja.

#### <a name="example-sshpublickey-authentication-using-private-key-filepath"></a>Példa: Az SshPublicKey hitelesítés használata a titkos kulcs fájl elérési útja

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Példa: Az SshPublicKey hitelesítés használata a titkos kulcs tartalmát

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of hello private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében.

Hello **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú. Adott toohello adathalmaztípushoz információkat biztosít. hello typeProperties szakasz egy adatkészlet típusú **fájlmegosztási** dataset adatkészletben hello következő tulajdonságai:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| folderPath |Elérési út toohello almappa. Használja az escape-karakter "\" hello karakterlánc speciális karakter. Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.<br/><br/>Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdő és záró dátum-idő. |Igen |
| fileName |Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában. Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.<br/><br/>Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello hello létrehozott fájl neve a következő formátumban hello lenne: <br/><br/>Adatok. <Guid>.txt (Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nem |
| fileFilter |Adja meg a szűrő toobe tooselect használja, minden fájl helyett a hello folderPath fájlok egy részét.<br/><br/>Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).<br/><br/>1. példa:`"fileFilter": "*.log"`<br/>2. példa:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter is alkalmazható egy bemeneti fájlmegosztási az adatkészlethez. Ez a tulajdonság a HDFS nem támogatott. |Nem |
| partitionedBy |partitionedBy lehet használt toospecify egy dinamikus folderPath időadatok adatsorozat nevét. Például folderPath adatok óránkénti paraméteres. |Nem |
| Formátumban | a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket. További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. <br><br> Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban. |Nem |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**. Támogatott szintek a következők: **Optimal** és **leggyorsabb**. További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem |
| useBinaryTransfer |Adja meg, hogy a bináris átviteli mód használata. A bináris mód és a hamis értéket ASCII igaz. Alapértelmezett érték: igaz. A tulajdonság csak akkor használható, típusú társított kapcsolódószolgáltatás-típus esetén: FTP-kiszolgáló. |Nem |

> [!NOTE]
> fájlnév és fileFilter nem használható egyszerre.

### <a name="using-partionedby-property"></a>PartionedBy tulajdonság használatával
Hello előző szakaszban említett, megadhat egy dinamikus folderPath idő adatsorozat adatok partitionedBy fájlnevét. Megteheti hello adat-előállító makrók és hello rendszerváltozó SliceStart, hello jelző logikai időszakban egy adott adatszelet a SliceEnd.

toolearn idő adatsorozat adatkészleteket, ütemezés és szeletek, lásd: [létrehozása adatkészletek](data-factory-create-datasets.md), [ütemezés & végrehajtási](data-factory-scheduling-and-execution.md), és [létrehozása folyamatok](data-factory-create-pipelines.md) cikkeket.

#### <a name="sample-1"></a>1. példa:

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
Ebben a példában {szelet} hello adat-előállító rendszer változó SliceStart hello formátumban (YYYYMMDDHH) megadott érték helyére. hello SliceStart toostart idő hello szelet hivatkozik. hello folderPath nem azonos az egyes szeletek. Példa: wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>2. példa:

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
Ebben a példában év, hónap, nap és SliceStart idején ki kell olvasni a külön változókat, amelyek folderPath és a fájlnév tulajdonság.

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.

Mivel a hello hello tevékenység részében typeProperties elérhető hello tulajdonságok tevékenységek minden típusának függenek. A másolási tevékenység során hello típustulajdonságokat hello típusú források és mosdók függenek.

[!INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]

## <a name="supported-file-and-compression-formats"></a>Támogatott formátumú és tömörítés
Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk részletei.

## <a name="json-example-copy-data-from-sftp-server-tooazure-blob"></a>JSON-NÁ. példa: Adatok másolása az SFTP server tooAzure blobból
hello alábbi példa minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azok bemutatják, hogyan toocopy SFTP az adatforrás-tooAzure Blob-tároló. Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott hello nyelő források tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.

> [!IMPORTANT]
> Ez a minta JSON kódtöredékek biztosít. Nem tartalmazza a hello adat-előállító létrehozásának részletes leírása. Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.

hello minta a következő data factory entitások hello rendelkezik:

* A társított szolgáltatás típusa [sftp](#linked-service-properties).
* A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bemeneti [dataset](data-factory-create-datasets.md) típusú [fájlmegosztási](#dataset-properties).
* Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [FileSystemSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másol adatokat az SFTP server tooan Azure blob minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

**Kapcsolódó SFTP szolgáltatás**

Ebben a példában a felhasználó nevét és a jelszót egyszerű szövegként hello egyszerű hitelesítést használ. A következő módokon hello egyikét is használhatja:

* Egyszerű hitelesítés titkosított hitelesítő adatokkal
* SSH nyilvános kulcsos hitelesítés

Lásd: [FTP társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.

```JSON

{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "myuser",
            "password": "mypassword",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```
**Azure Storage társított szolgáltatás**

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
**SFTP bemeneti adatkészlet**

Ez az adatkészlet hivatkozik toohello SFTP mappa `mysharedfolder` és fájl `test.csv`. hello csővezeték hello fájl toohello cél másolja.

"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```JSON
{
  "name": "SFTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "SftpLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Az Azure Blob kimeneti adatkészlet**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**A másolási tevékenység-feldolgozási folyamat**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**FileSystemSource** és **fogadó** típusuk értéke túl**BlobSink**.

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
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
        "start": "2017-02-20T18:00:00Z",
        "end": "2017-02-20T19:00:00Z"
    }
}
```

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.

## <a name="next-steps"></a>Következő lépések
Tekintse meg a következő cikkek hello:

* [Másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) való a másolási tevékenység során a folyamat létrehozásának lépéseit.
