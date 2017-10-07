---
title: "az Azure Data Factory használatával fájlrendszer aaaCopy adatok |} Microsoft Docs"
description: "Megtudhatja, hogyan toocopy adatok tooand egy helyszíni fájlrendszerből Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: ce19f1ae-358e-4ffc-8a80-d802505c9c84
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jingwang
ms.openlocfilehash: 201b8bc3ffa639df781443aa0c3f95c975d280be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-an-on-premises-file-system-by-using-azure-data-factory"></a>Adatok tooand másolása egy helyszíni fájlrendszer Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toocopy adatok belőle egy helyszíni fájlrendszer a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

## <a name="supported-scenarios"></a>Támogatott helyzetek
Adatokat másolhat **egy helyi fájl rendszerből** toohello a következő adatokat tárolja:

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

Adatok másolása a következő adatokat tárolja hello **tooan helyszíni fájlrendszer**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Másolási tevékenység nem hello forrásfájl törlése, miután sikeresen másolt toohello cél. Ha toodelete hello forrásfájl után sikeres másolatát, hozzon létre egy egyéni tevékenység toodelete hello fájlt, és a hello tevékenység hello folyamat. 

## <a name="enabling-connectivity"></a>Kapcsolat engedélyezése
Adat-előállító támogat egy helyszíni fájlrendszerből keresztül csatlakozó tooand **az adatkezelési átjáró**. Telepítenie kell az adatkezelési átjáró hello hello adat-előállító szolgáltatás tooconnect tooany támogatott helyszíni adattároló többek között a fájlrendszer a helyszíni környezetben. toolearn az adatkezelési átjáró és hello átjáró beállításának lépéseit lásd: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró hello felhő között](data-factory-move-data-between-onprem-and-cloud.md). Az adatkezelési átjáró, leszámítva más bináris fájlokat kell telepített toobe toocommunicate tooand egy helyi fájl rendszerből. Telepítse, és az adatkezelési átjáró hello használja, akkor is, ha fájlrendszer hello Azure IaaS virtuális gépen. Hello átjáró kapcsolatos részletes információkért lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md).

Linux fájlmegosztást, toouse telepítése [Samba](https://www.samba.org/) a Linux-kiszolgálón, telepítse az adatkezelési átjáró a Windows server. Az adatkezelési átjáró telepítése egy Linux-kiszolgálón nem támogatott.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, amely helyezi át az adatokat, vagy az operációs rendszer különböző eszközök/API-k használatával létrehozhat egy folyamatot.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon **, **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre egy **adat-előállító**. Egy adat-előállító tartalmazhat egy vagy több folyamatok. 
2. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban. Adatok másolása az Azure blob storage tooan helyszíni fájlrendszer a, akkor hozzon létre például két összekapcsolt szolgáltatások toolink a helyszíni fájlrendszerben és az Azure storage tooyour adat-előállítóban. Csatolt szolgáltatás tulajdonságait, amelyek adott tooan helyszíni fájlrendszer, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.
3. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. Hello utolsó lépésében említett hello például létrehoz egy adatkészlet toospecify hello blobtárolót és hello bemeneti adatokat tartalmazó mappát. És egy másik dataset toospecify hello mappa nevét és a fájlrendszert (nem kötelező) létrehozása. Adatkészlet tulajdonságai adott tooon helyszíni fájlrendszer, a következő témakörben: [adatkészlet tulajdonságai](#dataset-properties) szakasz.
4. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. A korábban említett hello példában BlobSource forrás-és FileSystemSink akár használhatja a fogadó hello másolási tevékenységhez. Hasonlóképpen a helyi fájl rendszer tooAzure Blob Storage másolása, használható FileSystemSource és BlobSink hello másolási tevékenység. A másolási tevékenység tulajdonságai adott tooon helyszíni fájlrendszer, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz. További információkért hogyan toouse egy adatok tárolót, mint a forrás- és a fogadó hivatkozásra hello az adattároló hello előző szakaszban.

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek az operációs rendszer használt toocopy adatok JSON-definíciók minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-file-system) című szakaszát.

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott toofile rendszer részleteit tartalmazzák:

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
Egy helyi fájl rendszer tooan az Azure data factory hello társíthatja **a helyi fájlkiszolgáló** társított szolgáltatás. hello a következő táblázat ismerteti, amelyek adott toohello a helyi fájlkiszolgáló társított szolgáltatás JSON-elemek szerepelnek.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |Győződjön meg arról, hogy hello típusú tulajdonsága túl**OnPremisesFileServer**. |Igen |
| állomás |Hello legfelső szintű megjeleníteni kívánt toocopy hello mappa elérési útját adja meg. Hello escape-karakter használata "\" hello karakterlánc speciális karakter. Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat. |Igen |
| felhasználói azonosítóját |Adja meg a toohello kiszolgáló hello felhasználó hello Azonosítót. |Nem (Ha úgy dönt, hogy encryptedCredential) |
| jelszó |Adja meg a hello jelszó hello (userid). |Nem (Ha úgy dönt, hogy encryptedCredential |
| encryptedCredential |Adja meg a hello titkosított hitelesítő adatokat, amelyek a parancsmagot a New-AzureRmDataFactoryEncryptValue hello kaphat. |Nem (Ha úgy dönt, toospecify felhasználói azonosítót és jelszót a szövegként) |
| gatewayName |Megadja, hogy a Data Factory kell használnia tooconnect toohello a helyi fájlkiszolgáló hello átjáró hello nevét. |Igen |


### <a name="sample-linked-service-and-dataset-definitions"></a>Példa társított szolgáltatás és a dataset definíciók
| Forgatókönyv | A társított szolgáltatás definíciójának üzemeltetéséhez | Az adatkészlet-definícióban folderPath |
| --- | --- | --- |
| Az adatkezelési átjáró gépen helyi mappában: <br/><br/>Példák: D:\\ \* vagy D:\folder\subfolder\\* |D:\\ \\ (az adatok felügyeleti átjáró 2.0-s és újabb verziók) <br/><br/> a localhost (korábbi verzióihoz mint adatok felügyeleti átjáró 2.0-s) |. \\ \\ vagy mappa\\\\almappa (az adatok felügyeleti átjáró 2.0-s és újabb verziók) <br/><br/>D:\\ \\ vagy D:\\\\mappa\\\\almappa (az átjáró verziója alatt 2.0-s) |
| Távoli megosztott mappa: <br/><br/>Példák: \\ \\myserver\\megosztása\\ \* vagy \\ \\myserver\\megosztása\\mappa\\almappa\\* |\\\\\\\\myserver\\\\megosztása |. \\ \\ vagy mappa\\\\almappa |


### <a name="example-using-username-and-password-in-plain-text"></a>Példa: Felhasználónév és jelszó használatával egyszerű szöveges

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

### <a name="example-using-encryptedcredential"></a>Példa: Encryptedcredential használatával

```JSON
{
  "Name": " OnPremisesFileServerLinkedService ",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "D:\\",
      "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
      "gatewayName": "mygateway"
    }
  }
}
```

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Illetve meghatározásához adatkészletek rendelkezésre álló tulajdonságok teljes listáját lásd: [adatkészletek létrehozása](data-factory-create-datasets.md). Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében.

hello typeProperties szakaszban nem egyezik az adatkészlet egyes típusú. Például a hello helyét és hello adattár hello adatok formátuma adatokat tartalmazza. hello typeProperties szakasz hello adatkészlet típusú **fájlmegosztási** rendelkezik hello következő tulajdonságai:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| folderPath |Hello részleges toohello mappáját adja meg. Hello escape-karakter használata "\" hello karakterlánc speciális karakter. Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.<br/><br/>Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdő és záró dátum-idő. |Igen |
| fileName |Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában. Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.<br/><br/>Ha **Fájlnév** nincs megadva egy kimeneti adatkészlet és **preserveHierarchy** nincs megadva a tevékenység fogadó hello név hello létrehozott fájl formátuma a következő hello van: <br/><br/>`Data.<Guid>.txt`(Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Nem |
| fileFilter |Adja meg a szűrő toobe tooselect használja, minden fájl helyett a hello folderPath fájlok egy részét. <br/><br/>Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).<br/><br/>1. példa: "fileFilter": "* .log"<br/>2. példa: "fileFilter": 2014 - 1-?. txt"<br/><br/>Vegye figyelembe, hogy fileFilter egy bemeneti fájlmegosztási adatkészlet esetében alkalmazható. |Nem |
| partitionedBy |Az idősorozat adatok partitionedBy toospecify dinamikus folderPath/fileName is használhatja. Példa: az adatok óránkénti paraméteres folderPath. |Nem |
| Formátumban | a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ** ParquetFormat**. Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket. További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. <br><br> Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban. |Nem |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**. Támogatott szintek a következők: **Optimal** és **leggyorsabb**. Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem |

> [!NOTE]
> Nem használható egyszerre fájlnév és fileFilter.

### <a name="using-partitionedby-property"></a>PartitionedBy tulajdonság használatával
Hello előző szakaszban említett, megadhatja a dinamikus folderPath és a sorozat időadatok fájlnevét hello **partitionedBy** tulajdonság, [adat-előállító funkciók és hello rendszerváltozók](data-factory-functions-variables.md).

toounderstand idősorozat adatkészleteket, az ütemezés és a szeletek, a további részletek: [adatkészletek létrehozása](data-factory-create-datasets.md), [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md), és [folyamatok létrehozása](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>1. példa:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

Ebben a példában {szelet} hello értékű hello adat-előállító rendszer változó SliceStart hello formátumban (YYYYMMDDHH) váltja fel. SliceStart toostart idő hello szelet hivatkozik. hello folderPath nem azonos az egyes szeletek. Például: wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104.

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

Ebben a példában év, hónap, nap és SliceStart idején ki kell olvasni a hello folderPath és a fájlnév tulajdonságot használó külön változók.

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti adatkészletek és házirendek érhetők el. Mivel tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek.

A másolási tevékenység során két érték források és mosdók hello típusától függően. Ha adatok egy helyi fájl rendszerből, beállítása hello forrástípus hello másolási tevékenység túl**FileSystemSource**. Hasonlóképpen, ha adatokat tooan helyszíni fájlrendszer, beállítása hello a fogadó típusa másolási tevékenység hello túl**FileSystemSink**. Ez a témakör FileSystemSource és FileSystemSink által támogatott tulajdonságokról.

**FileSystemSource** következő tulajdonságai hello támogatja:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| Rekurzív |Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello. |IGAZ, hamis (alapértelmezés) |Nem |

**FileSystemSink** következő tulajdonságai hello támogatja:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| copyBehavior |Hello másolási viselkedését határozza meg, ha hello adatforrás BlobSource vagy a fájlrendszer. |**PreserveHierarchy:** hello fájl hierarchia hello célmappában megőrzi. Ez azt jelenti, hogy van hello ugyanaz, mint a hello relatív elérési útja hello cél fájl toohello célmappa hello hello forrás fájl toohello forrásmappa relatív elérési út.<br/><br/>**FlattenHierarchy:** hello forrásmappából minden fájl első szintű hello célmappában jönnek létre. hello fájljaira jönnek létre automatikusan létrehozott névvel.<br/><br/>**Mergefiles típusú:** forrásfájlból hello mappa tooone minden fájl egyesíti. Ha hello fájl neve/blob neve meg van adva, a hello egyesített fájl neve az hello megadott név. Ellenkező esetben egy automatikusan létrehozott nevét. |Nem |

### <a name="recursive-and-copybehavior-examples"></a>rekurzív és copyBehavior példák
Ez a szakasz ismerteti a hello viselkedésről hello másolási műveletek kombinációk hello rekurzív és a copyBehavior tulajdonság értéktartománya.

| rekurzív érték | copyBehavior érték | Viselkedésről |
| --- | --- | --- |
| Igaz |preserveHierarchy |A forrásmappa mappa1 a struktúra, a következő hello<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello tároló mappa mappa1 azonos szerkezeti hello forrásaként hello jön létre:<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| Igaz |flattenHierarchy |A forrásmappa mappa1 a struktúra, a következő hello<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello cél mappa1 jön létre: <br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File5 |
| Igaz |mergefiles típusú |A forrásmappa mappa1 a struktúra, a következő hello<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello cél mappa1 jön létre: <br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + fájl3 + File4 + 5 fájl tartalmát egy fájlba, egy fájl automatikusan létrehozott névvel egyesítve lesznek. |
| hamis |preserveHierarchy |A forrásmappa mappa1 a struktúra, a következő hello<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello tároló mappa mappa1 jön létre:<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Fájl3, File4 és File5 Subfolder1 nem felvételre. |
| hamis |flattenHierarchy |A forrásmappa mappa1 a struktúra, a következő hello<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello tároló mappa mappa1 jön létre:<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2<br/><br/>Fájl3, File4 és File5 Subfolder1 nem felvételre. |
| hamis |mergefiles típusú |A forrásmappa mappa1 a struktúra, a következő hello<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello tároló mappa mappa1 jön létre:<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 tartalma egyesítődnek fájl automatikusan létrehozott névvel egy fájlba.<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatikusan létrehozott nevet a file1 kiszolgálón<br/><br/>Fájl3, File4 és File5 Subfolder1 nem felvételre. |

## <a name="supported-file-and-compression-formats"></a>Támogatott formátumú és tömörítés
Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk részletei.

## <a name="json-examples-for-copying-data-tooand-from-file-system"></a>Az adatok tooand Másolás a fájlrendszerből JSON példák
hello alábbi példák megadják minta JSON-definíciók használható toocreate adatcsatorna hello segítségével [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azok hogyan toocopy adatok tooand egy a helyszíni fájlrendszerben és az Azure Blob Storage tárolóban. Adatok másolása azonban *közvetlenül* bármelyik hello források tooany felsorolt hello nyelő [támogatott források és mosdók](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység az Azure Data Factory használatával.

### <a name="example-copy-data-from-an-on-premises-file-system-tooazure-blob-storage"></a>Példa: Adatok másolása egy helyi fájl rendszer tooAzure Blob-tároló
Ez a példa bemutatja, hogyan egy helyi fájl rendszer tooAzure Blob-tároló toocopy adatait. hello minta a következő adat-előállító entitások hello rendelkezik:

* A társított szolgáltatás típusa [OnPremisesFileServer](#linked-service-properties).
* A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bemeneti [dataset](data-factory-create-datasets.md) típusú [fájlmegosztási](#dataset-properties).
* Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [FileSystemSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

a következő minta hello átmásolja-idősoros adatok egy helyi fájl rendszer tooAzure Blob-tároló minden órában. Ezeket a mintákat a használt hello JSON-tulajdonságok hello minta után hello szakaszok ismertetik.

Első lépésként állítsa be az adatkezelési átjáró szerint hello utasításait [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró hello felhő között](data-factory-move-data-between-onprem-and-cloud.md).

**A helyi fájlkiszolgáló társított szolgáltatáshoz:**

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

Azt javasoljuk, hello **encryptedCredential** tulajdonság helyette hello **userid** és **jelszó** tulajdonságok. Lásd: [fájlkiszolgáló társított szolgáltatás](#linked-service-properties) részleteiről a társított szolgáltatás.

**Az Azure tárolás társított szolgáltatásának:**

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

**A helyi rendszer bemeneti adatkészlet fájl:**

Adatok van felvett egy új fájl óránként. hello folderPath, és a fájlnév tulajdonságok alapján hello szelet hello kezdési idejét határozza meg.  

Beállítás `"external": "true"` adat-előállító tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```JSON
{
  "name": "OnpremisesFileSystemInput",
  "properties": {
    "type": " FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
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

**Az Azure Blob storage kimeneti adatkészlet:**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útját használja hello év, hónap, nap és óra részei hello kezdési ideje.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**A másolási tevékenység során a fájlrendszer és a Blob fogadó folyamat:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek, és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**FileSystemSource**, és **fogadó** típusuk értéke túl**BlobSink**.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T19:00:00",
    "description":"Pipeline for copy activity",
    "activities":[  
      {
        "name": "OnpremisesFileSystemtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "OnpremisesFileSystemInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
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
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```

### <a name="example-copy-data-from-azure-sql-database-tooan-on-premises-file-system"></a>Példa: Adatok másolása az Azure SQL Database tooan helyszíni fájlrendszer
a következő példa azt mutatja be hello:

* A társított szolgáltatás típusa [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)
* A társított szolgáltatás típusa [OnPremisesFileServer](#linked-service-properties).
* Egy bemeneti adatkészlet típusú [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
* Egy kimeneti adatkészlet típusú [fájlmegosztási](#dataset-properties).
* A másolási tevékenység által használt az adatcsatorna [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) és [FileSystemSink](#copy-activity-properties).

hello minta másol idősorozat adatokat az Azure SQL táblázat tooan helyszíni fájlrendszer minden órában. Ezeket a mintákat a használt hello JSON-tulajdonságok hello minta után szakaszok ismertetik.

**A társított szolgáltatásnak Azure SQL Database:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```

**A helyi fájlkiszolgáló társított szolgáltatáshoz:**

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

Azt javasoljuk, hello **encryptedCredential** hello használata helyett tulajdonság **userid** és **jelszó** tulajdonságok. Lásd: [fájlrendszer társított szolgáltatás](#linked-service-properties) részleteiről a társított szolgáltatás.

**Az Azure SQL bemeneti adatkészlet:**

hello minta feltételezi, hogy létrehozott egy tábla "MyTable" Azure SQL-ben, és egy idősorozat adatok "timestampcolumn" nevű oszlopot tartalmaz.

Beállítás ``“external”: ”true”`` adat-előállító tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
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

**A helyszíni rendszer kimeneti adatkészlet fájl:**

Adata másolt tooa új fájl óránként. hello folderPath és hello blob fájlnevét határozza meg hello szelet hello kezdési ideje alapján.

```JSON
{
  "name": "OnpremisesFileSystemOutput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
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

**A másolási tevékenység során egy SQL-forrás és fogadó fájlrendszer folyamaton belül:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek, és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**SqlSource**, és hello **fogadó** típusuk értéke túl**FileSystemSink**. hello SQL-lekérdezésben megadott hello **SqlReaderQuery** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T20:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLtoOnPremisesFile",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "OnpremisesFileSystemOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "FileSystemSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 3,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```


Forrás adatkészlet toocolumns hello másolási tevékenységdefinícióban fogadó adatkészletből oszlopokat is leképezheti. További információkért lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Teljesítmény és finomhangolás
 toolearn kulccsal kapcsolatos tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás hello teljesítmény, lásd: hello [másolási tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md).
