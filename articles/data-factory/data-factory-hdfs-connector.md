---
title: "a helyszíni HDFS aaaMove adatait |} Microsoft Docs"
description: "További információk a hogyan toomove adatokat a helyszíni HDFS Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3215b82d-291a-46db-8478-eac1a3219614
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 96387e5dd089099fc2e983ab26d67c2044b973b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Adatok áthelyezése az Azure Data Factory használatával a helyszíni HDFS
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatait egy helyszíni HDFS a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

HDFS támogatott tooany fogadó adattár adatainak másolhatja. Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. Adat-előállító jelenleg csak áthelyezése adatait egy helyszíni HDFS tooother adattárolókhoz támogatja, de a nem az adatok áthelyezését más adatokat tároló tooan a helyszíni HDFS.

> [!NOTE]
> Másolási tevékenység nem hello forrásfájl törlése, miután sikeresen másolt toohello cél. Ha toodelete hello forrásfájl után sikeres másolatát, hozzon létre egy egyéni tevékenység toodelete hello fájlt, és a hello tevékenység hello folyamat. 

## <a name="enabling-connectivity"></a>Kapcsolat engedélyezése
Data Factory szolgáltatásnak hello az adatkezelési átjáró használatával csatlakozó tooon helyszíni HDFS támogatja. Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat. Hello átjáró tooconnect tooHDFS használja, akkor is, ha egy Azure IaaS virtuális gép helyezkedik el.

> [!NOTE]
> Győződjön meg arról, hogy hello túl férhetnek hozzá az adatkezelési átjáró**összes** hello [névkiszolgáló csomópont]: [csomópont port name] és [adatok működő kiszolgálók]: [adatok csomópont port] hello Hadoop-fürt. Alapértelmezés szerint a [name csomópont port] 50070, és alapértelmezett [adatok csomópont port] 50075.

Amíg az átjáró telepíthető hello azonos helyszíni gépet vagy hello Azure virtuális gép hello HDFS, azt javasoljuk, hogy hello átjáró telepítése egy különálló számítógép vagy az Azure infrastruktúra-szolgáltatási virtuális gép. Átjáró egy külön számítógépen Erőforrásverseny csökkenti, és javítja a teljesítményt. Hello átjáró egy külön számítógépen való telepítésekor hello gépnek képes tooaccess hello gép hello HDFS kell lennie.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, amely helyezi át az adatokat HDFS forrásból származó különböző eszközök/API-k használatával létrehozhat egy folyamatot.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Egy minta JSON-definíciók az adat-előállító entitások, amelyek a HDFS-tárolóban használt toocopy adatait, tekintse meg [JSON-példa: adatok másolása a helyszíni HDFS tooAzure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) című szakaszát.

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooHDFS részleteit tartalmazzák:

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
A társított szolgáltatás adatokat tároló tooa adat-előállító hivatkozásokat tartalmaz. Típusú társított szolgáltatás létrehozása **Hdfs** toolink egy helyszíni HDFS tooyour adat-előállítóban. a következő táblázat hello biztosít JSON elemek adott tooHDFS kapcsolódó szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **Hdfs** |Igen |
| URL-cím |URL-cím toohello HDFS |Igen |
| AuthenticationType |Névtelen, vagy a Windows. <br><br> toouse **Kerberos-hitelesítés** HDFS-összekötőhöz, tekintse meg túl[ebben a szakaszban](#use-kerberos-authentication-for-hdfs-connector) a helyszíni környezet tooset ennek megfelelően. |Igen |
| Felhasználónév |Felhasználónév a Windows-hitelesítést. |Igen (a Windows-hitelesítés) |
| jelszó |A Windows-hitelesítés jelszót. |Igen (a Windows-hitelesítés) |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello HDFS kell használnia. |Igen |
| encryptedCredential |[Új AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) hello hozzáférési hitelesítő adatok kimenetét. |Nem |

### <a name="using-anonymous-authentication"></a>A névtelen hitelesítés segítségével

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-windows-authentication"></a>Windows-hitelesítés használatával

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```
## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. hello typeProperties szakasz típusú adatkészlet **fájlmegosztási** következő tulajdonságai hello (amely tartalmazza a HDFS dataset) rendelkezik.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| folderPath |Toohello mappa elérési útja. Példa:`myfolder`<br/><br/>Használja az escape-karakter "\" hello karakterlánc speciális karakter. Például: folder\subfolder, adja meg a mappa\\\\almappa és d:\samplefolder, adja meg a d:\\\\mappába.<br/><br/>Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdő és záró dátum-idő. |Igen |
| fileName |Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában. Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.<br/><br/>Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello hello létrehozott fájl neve a következő formátumban hello lenne: <br/><br/>Adatok. <Guid>.txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nem |
| partitionedBy |partitionedBy lehet használt toospecify egy dinamikus folderPath időadatok adatsorozat nevét. Példa: folderPath adatok óránkénti paraméteres. |Nem |
| Formátumban | a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket. További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. <br><br> Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban. |Nem |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**. Támogatott szintek a következők: **Optimal** és **leggyorsabb**. További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem |

> [!NOTE]
> fájlnév és fileFilter nem használható egyszerre.

### <a name="using-partionedby-property"></a>PartionedBy tulajdonság használatával
Hello előző szakaszban említett, megadhatja a dinamikus folderPath és a sorozat időadatok fájlnevét hello **partitionedBy** tulajdonság, [adat-előállító funkciók és hello rendszerváltozók](data-factory-functions-variables.md).

toolearn bővebben idő adatsorozat adatkészleteket, az ütemezés és a szeletek, lásd: [létrehozása adatkészletek](data-factory-create-datasets.md), [ütemezés & végrehajtási](data-factory-scheduling-and-execution.md), és [létrehozása folyamatok](data-factory-create-pipelines.md) cikkeket.

#### <a name="sample-1"></a>1. példa:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
Ebben a példában {szelet} hello adat-előállító rendszer változó SliceStart hello formátumban (YYYYMMDDHH) megadott érték helyére. hello SliceStart toostart idő hello szelet hivatkozik. hello folderPath nem azonos az egyes szeletek. Például: wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>2. példa:

```JSON
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

Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

A másolási tevékenység, ha a forrás típusa nem **FileSystemSource** typeProperties szakaszában érhetők hello következő tulajdonságai:

**FileSystemSource** következő tulajdonságai hello támogatja:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| Rekurzív |Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello. |IGAZ, hamis (alapértelmezés) |Nem |

## <a name="supported-file-and-compression-formats"></a>Támogatott formátumú és tömörítés
Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk részletei.

## <a name="json-example-copy-data-from-on-premises-hdfs-tooazure-blob"></a>JSON-példa: adatok másolása a helyszíni HDFS tooAzure Blob
Ez a példa bemutatja, hogyan egy helyszíni HDFS tooAzure Blob Storage toocopy adatait. Azonban az adatok átmásolhatók **közvetlenül** közölt hello nyelő tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.  

hello minta biztosít a következő adat-előállító entitások hello JSON jelentésdefiníciókat. A definíciók toocreate egy folyamat toocopy adatokat HDFS tooAzure Blob Storage segítségével is használhatók [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).

1. A társított szolgáltatás típusa [OnPremisesHdfs](#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [fájlmegosztási](#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [FileSystemSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másol adatokat egy helyszíni HDFS tooan Azure blob minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

Első lépésként hello az adatkezelési átjáró beállítása. hello utasításait hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.

**HDFS társított szolgáltatás:** ebben a példában használt hello Windows-hitelesítést. Lásd: [HDFS társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.

```JSON
{
    "name": "HDFSLinkedService",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

**Az Azure tárolás társított szolgáltatásának:**

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

**HDFS bemeneti adatkészletéből:** Ez az adatkészlet hivatkozik toohello HDFS mappa DataTransfer/UnitTest /. hello folyamat összes hello fájlt másolja a következő mappa toohello célra.

"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```JSON
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

**Az Azure Blob kimeneti adatkészlet:**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```JSON
{
    "name": "OutputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**A másolási tevékenység során a fájlrendszer és a Blob fogadó folyamat:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse ezen bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**FileSystemSource** és **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.

```JSON
{
    "name": "pipeline",
    "properties":
    {
        "activities":
        [
            {
                "name": "HdfsToBlobCopy",
                "inputs": [ {"name": "InputDataset"} ],
                "outputs": [ {"name": "OutputDataset"} ],
                "type": "Copy",
                "typeProperties":
                {
                    "source":
                    {
                        "type": "FileSystemSource"
                    },
                    "sink":
                    {
                        "type": "BlobSink"
                    }
                },
                "policy":
                {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>A HDFS-összekötő Kerberos-hitelesítés használata
Nincsenek a HDFS-összekötő Kerberos-hitelesítés toouse, így a két beállítások tooset hello a helyszíni környezet létrehozása. Választhat egy hello legjobban az esethez.
* 1. lehetőség: [illesztési átjárót működtető gépen, a Kerberos-tartomány](#kerberos-join-realm)
* 2. lehetőség: [kölcsönös, a Windows-tartomány és a Kerberos-tartomány közötti megbízhatósági kapcsolat engedélyezése](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>1. lehetőség: Illesztés átjárót működtető gépen, Kerberos-tartomány

#### <a name="requirement"></a>Követelmény:

* hello átjárót futtató gép toojoin hello Kerberos-tartomány van szüksége, és nem tud csatlakozni a Windows-tartományhoz.

#### <a name="how-tooconfigure"></a>Hogyan tooconfigure:

**Az átjáró számítógépén:**

1.  Futtassa a hello **Ksetup** segédprogram tooconfigure hello Kerberos KDC-kiszolgáló és a tartomány.

    hello gép be kell állítani egy munkacsoport tagjaként óta egy Kerberos-tartomány eltér a Windows-tartományhoz. Ez megvalósítható hello Kerberos-tartomány és a KDC-kiszolgáló hozzáadása az alábbiak szerint. Cserélje le *REALM.COM* a saját megfelelő terület szükséges.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    **Indítsa újra a** hello gép 2 parancsok végrehajtása után.

2.  Ellenőrizze a hello-konfiguráció **Ksetup** parancsot. hello kimeneti kell lennie, mint:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**Az Azure Data Factoryben:**

* Konfigurálja a hello HDFS-összekötő használatával **Windows-hitelesítés** a Kerberos egyszerű neve és jelszava tooconnect toohello HDFS adatforrás együtt. Ellenőrizze [HDFS társított szolgáltatás Tulajdonságok](#linked-service-properties) konfiguráció részletei című szakaszban.

### <a name="kerberos-mutual-trust"></a>2. lehetőség: A Windows-tartomány és a Kerberos-tartomány közötti kölcsönös megbízhatósági engedélyezése

#### <a name="requirement"></a>Követelmény:
*   hello átjárót működtető gépen Windows-tartományhoz kell csatlakoztatni.
*   Engedély tooupdate hello tartományvezérlő-beállítások van szüksége.

#### <a name="how-tooconfigure"></a>Hogyan tooconfigure:

> [!NOTE]
> Igény szerint a saját megfelelő tartomány és a tartományvezérlő oktatóanyag következő hello REALM.COM és AD.COM cserélni.

**KDC-kiszolgálón:**

1.  Hello KDC konfigurációjának szerkesztése **krb5.conf** fájl toolet KDC megbízható hivatkozik a következő konfigurációs sablon toohello Windows-tartományhoz. Alapértelmezés szerint hello konfigurációs itt található: **/etc/krb5.conf**.

            [logging]
             default = FILE:/var/log/krb5libs.log
             kdc = FILE:/var/log/krb5kdc.log
             admin_server = FILE:/var/log/kadmind.log

            [libdefaults]
             default_realm = REALM.COM
             dns_lookup_realm = false
             dns_lookup_kdc = false
             ticket_lifetime = 24h
             renew_lifetime = 7d
             forwardable = true

            [realms]
             REALM.COM = {
              kdc = node.REALM.COM
              admin_server = node.REALM.COM
             }
            AD.COM = {
             kdc = windc.ad.com
             admin_server = windc.ad.com
            }

            [domain_realm]
             .REALM.COM = REALM.COM
             REALM.COM = REALM.COM
             .ad.com = AD.COM
             ad.com = AD.COM

            [capaths]
             AD.COM = {
              REALM.COM = .
             }

  **Indítsa újra a** KDC-szolgáltatás hello konfigurálása után.

2.  Készítse elő a rendszerbiztonsági tag nevű  **krbtgt/REALM.COM@AD.COM**  a KDC kiszolgáló hello a következő parancsot:

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  A **hadoop.security.auth_to_local** HDFS-szolgáltatás konfigurációs fájlt, adja hozzá `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.

**A tartományvezérlőn:**

1.  Futtassa a következő hello **Ksetup** tooadd egy tartomány bejegyzés parancsokat:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  A Windows-tartomány tooKerberos tartomány megbízhatósági kapcsolat létrehozása. [jelszava] hello egyszerű hello jelszavát  **krbtgt/REALM.COM@AD.COM** .

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Válassza ki a Kerberos használt titkosítási algoritmus.

    1. Nyissa meg tooServer Manager > csoportházirend-kezelés > tartomány > csoportházirend-objektumok > alapértelmezett vagy az aktív tartományi házirend és a Szerkesztés.

    2. A hello **Csoportházirendkezelés-szerkesztő** előugró ablakban, keresse tooComputer konfigurációs > házirendek > Windows-beállítások > biztonsági beállítások > helyi házirend > biztonsági beállítások, és konfigurálja **hálózati biztonsági: konfigurálja a Kerberos engedélyezett titkosítási típusok**.

    3. Jelölje be hello titkosítási algoritmus toouse kívánt tooKDC amikor kapcsolódik. Gyakran, egyszerűen minden hello lehetőségek közül választhat.

        ![A Kerberos config titkosítási típusok](media/data-factory-hdfs-connector/config-encryption-types-for-kerberos.png)

    4. Használjon **Ksetup** parancs toospecify hello titkosítási algoritmus toobe hello használt adott tartomány.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Hello leképezés közötti hello tartományi fiók és a Kerberos egyszerű, a rendelés toouse Kerberos egyszerű a Windows-tartomány létrehozása.

    1. Indítsa el a felügyeleti eszközök hello > **Active Directory – felhasználók és számítógépek**.

    2. Kattintson a speciális szolgáltatások konfigurálása **nézet** > **speciális funkciók**.

    3. Keresse meg a hello fiók toowhich szeretné, hogy toocreate hozzárendeléseket, és kattintson a jobb gombbal a tooview **a felhasználónév-leképezések** > kattintson **Kerberos-nevek** fülre.

    4. Adjon hozzá egy egyszerű hello tartomány.

        ![Térkép biztonsági azonosító](media/data-factory-hdfs-connector/map-security-identity.png)

**Az átjáró számítógépén:**

* Futtassa a következő hello **Ksetup** tooadd egy tartomány bejegyzést a parancsokat.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**Az Azure Data Factoryben:**

* Konfigurálja a hello HDFS-összekötő használatával **Windows-hitelesítés** tartományi fiók vagy a Kerberos egyszerű tooconnect toohello HDFS adatforrás együtt. Ellenőrizze [HDFS társított szolgáltatás Tulajdonságok](#linked-service-properties) konfiguráció részletei című szakaszban.

> [!NOTE]
> Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).


## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
