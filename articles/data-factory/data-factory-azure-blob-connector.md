---
title: az Azure Blob Storage aaaCopy adatok |} Microsoft Docs
description: "Ismerje meg, hogyan toocopy blob az Azure Data Factoryben az adatok. A minta: hogyan toocopy adatok tooand az Azure Blob Storage és az Azure SQL Database."
keywords: "Blobadatok, az azure blob másolása"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: bec8160f-5e07-47e4-8ee1-ebb14cfb805d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jingwang
ms.openlocfilehash: 8428c64e8e8b1084b3f2f680c4e1819559e4ffa3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooor-from-azure-blob-storage-using-azure-data-factory"></a>Adatok tooor másolása az Azure Blob Storage Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toocopy adatok tooand Azure Blob Storage-ból a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

## <a name="overview"></a>Áttekintés
Bármely támogatott forráshierarchiából adatokat tooAzure Blob Storage tárolóban tárolja, és az Azure Blob Storage támogatott tooany fogadó adatok tárolásához adatainak másolhatja. hello következő tábla adattárolókhoz támogatott adatforrások listáját tartalmazza vagy hello másolási tevékenység által fogadók esetében. Adatok áthelyezése például **a** egy SQL Server-adatbázist vagy egy Azure SQL-adatbázis **való** egy Azure blobtárolóba. És adatainak másolhatja **a** Azure blob storage **való** Azure SQL Data Warehouse vagy egy Azure Cosmos DB gyűjteményt. 

## <a name="supported-scenarios"></a>Támogatott helyzetek
Adatokat másolhat **az Azure Blob Storage** toohello a következő adatokat tárolja:

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

Adatok másolása a következő adatokat tárolja hello **tooAzure Blob Storage**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]
 
> [!IMPORTANT]
> Másolási tevékenység támogatja az adatok másolását / tooboth általános célú Azure Storage accounts és közbeni/Cool Blob storage. hello tevékenység támogatja **blokk, hozzáfűzése, vagy olvasása lapblobokat**, azonban a **tooonly blokkblobokat írása**. Prémium szintű Storage nem támogatott, a fogadó, mert a lapblobokat ezt támogatja.
> 
> Másolási tevékenység adatot nem töröl hello forrásból származó adatok sikeresen van hello toohello cél másolását követően. Ha sikeres másolatot után kell toodelete forrásadatok, hozzon létre egy [egyéni tevékenység](data-factory-use-custom-activities.md) toodelete adatok hello és hello tevékenység hello folyamat használja. Egy vonatkozó példáért lásd: hello [Delete blob vagy mappa mintát a Githubon](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity). 

## <a name="get-started"></a>Bevezetés
A másolási tevékenység, amely helyezi át az adatokat az Azure Blob Storage vagy a különböző eszközök/API-k használatával létrehozhat egy folyamatot.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Ennek a cikknek a [forgatókönyv](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) létrehozásának adatcsatorna toocopy az az Azure Blob-tároló helye tooanother Azure Blob-tároló helye. Egy folyamat toocopy adatok létrehozása az Azure Blob Storage tooAzure SQL Database oktatóanyag, lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md).

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre egy **adat-előállító**. Egy adat-előállító tartalmazhat egy vagy több folyamatok. 
2. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban. Adatok másolása az Azure blob storage tooan Azure SQL-adatbázis, akkor hozzon létre például két összekapcsolt szolgáltatások toolink az Azure storage-fiók és az Azure SQL adatbázis tooyour adat-előállítóban. Csatolt szolgáltatás tulajdonságait, amelyek adott tooAzure Blob-tároló, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz. 
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. Hello utolsó lépésében említett hello például létrehoz egy adatkészlet toospecify hello blobtárolót és hello bemeneti adatokat tartalmazó mappát. És egy másik dataset toospecify hello SQL táblázat hello blob-tároló átmásolva hello adatokat tartalmazó hello Azure SQL-adatbázis létrehozása. Tekintse meg, amelyek adott tooAzure Blob-tároló adatkészlet tulajdonságai, [adatkészlet tulajdonságai](#dataset-properties) szakasz.
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. A korábban említett hello példában BlobSource forrás-és SqlSink akár használhatja a fogadó hello másolási tevékenységhez. Hasonlóképpen a Blob Storage Azure SQL Database tooAzure másolása, használható SqlSource és BlobSink hello másolási tevékenység. A másolási tevékenység tulajdonságait, amelyek adott tooAzure Blob-tároló, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz. További információkért hogyan toouse egy adatok tárolót, mint a forrás- és a fogadó hivatkozásra hello az adattároló hello előző szakaszban.  

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek az Azure Blob-tároló felhasznált toocopy adatok JSON-definíciók minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-blob-storage  ) című szakaszát.

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooAzure Blob Storage részleteit tartalmazzák.

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
Két különböző összekapcsolt szolgáltatások toolink egy Azure Storage tooan az Azure data factory használatával. Ezek: **AzureStorage** társított szolgáltatás és **AzureStorageSas** társított szolgáltatás. hello Azure tárolás társított szolgáltatásának biztosít hello data Factory összetevőnek a globális hozzáférési toohello Azure Storage. Mivel hello Azure Storage SAS (közös hozzáférésű Jogosultságkód) kapcsolódó szolgáltatás biztosítja azt az Azure Storage korlátozott/időhöz kötött hozzáférés toohello hello adat-előállítóban. Nincsenek más különbségek a következő két összekapcsolt szolgáltatások között. Válassza ki az igényeinek megfelelő kapcsolódó hello szolgáltatást. hello következő szakaszokban további részleteket a következő két összekapcsolt szolgáltatások.

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
a dataset toorepresent toospecify hello adatkészlet hello type tulajdonsága bemeneti vagy kimeneti adatokat az Azure Blob Storage tárolóban, beállítása: **AzureBlob**. Set hello **linkedServiceName** hello dataset toohello nevének hello Azure Storage vagy az Azure Storage SAS tulajdonság társított szolgáltatás.  hello dataset hello típus tulajdonságainak megadása hello **blob tároló** és hello **mappa** hello blob Storage tárolóban.

JSON-szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

Adat-előállító támogatja alapú CLS-kompatibilis .NET típusú értékek biztosítani, például az Azure blob séma olvasható az adatforrásokhoz tartozó "structure" írja be az adatokat a következő hello: Int16, Int32, Int64, egyetlen, Double, Decimal, Byte [], Bool, String, Guid, Datetime, Datetimeoffset, Timespan. Adat-előállító automatikusan típuskonverziók hajt végre, a forrásadatok az adattároló tooa fogadó adattár áthelyezésekor.

Hello **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú, és információkat hello hely formátumra stb, hello adatok hello-tárolóban. hello typeProperties szakasz típusú adatkészlet **AzureBlob** dataset adatkészletben hello következő tulajdonságai:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| folderPath |Elérési út toohello tároló és mappa hello blob Storage tárolóban. Példa: myblobcontainer\myblobfolder\ |Igen |
| fileName |Hello blob neve. Fájlnév nem, kötelező, és a kis-és nagybetűket.<br/><br/>Ha meg kell adnia egy fájlnevet, hello (például a Másolás) tevékenység works hello adott Blob.<br/><br/>Ha nincs megadva fájlnév, másolása összes BLOB bemeneti adatkészlet hello folderPath tartalmazza.<br/><br/>Ha **Fájlnév** nincs megadva egy kimeneti adatkészlet és **preserveHierarchy** nincs megadva a tevékenység fogadó hello létrehozott fájl hello név lesz a következő formátumban hello: adatok.<Guid>. txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nem |
| partitionedBy |partitionedBy egy nem kötelező tulajdonság. Használhatja a dinamikus folderPath toospecify és a fájlnév idő adatsorozat adatok. Például folderPath is paraméteres adatok óránkénti. Lásd: hello [partitionedBy tulajdonság szakaszában](#using-partitionedBy-property) részletek és a példákat. |Nem |
| Formátumban | a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket. További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. <br><br> Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban. |Nem |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**. Támogatott szintek a következők: **Optimal** és **leggyorsabb**. További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem |

### <a name="using-partitionedby-property"></a>PartitionedBy tulajdonság használatával
Hello előző szakaszban említett, megadhatja a dinamikus folderPath és a sorozat időadatok fájlnevét hello **partitionedBy** tulajdonság, [adat-előállító funkciók és hello rendszerváltozók](data-factory-functions-variables.md).

Idő adatsorozat adatkészleteket, az ütemezés és a szeletek további információkért lásd: [létrehozása adatkészletek](data-factory-create-datasets.md) és [ütemezés & végrehajtási](data-factory-scheduling-and-execution.md) cikkeket.

#### <a name="sample-1"></a>1. példa

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

Ebben a példában {szelet} hello adat-előállító rendszer változó SliceStart hello formátumban (YYYYMMDDHH) megadott érték helyére. hello SliceStart toostart idő hello szelet hivatkozik. hello folderPath nem azonos az egyes szeletek. Például: wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104

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

Ebben a példában év, hónap, nap és SliceStart idején ki kell olvasni a külön változókat, amelyek folderPath és a fájlnév tulajdonság.

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti adatkészletek és házirendek érhetők el. Mivel tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően. Egy Azure Blob Storage-ból adatokat helyez át, ha beállítása hello forrás típusa másolási tevékenység hello túl**BlobSource**. Ehhez hasonlóan adatok tooan Azure Blob Storage helyez át, ha beállítása hello a fogadó típusa másolási tevékenység hello túl**BlobSink**. Ez a témakör BlobSource és BlobSink által támogatott tulajdonságokról.

**BlobSource** támogatja a következő tulajdonságai a hello hello **typeProperties** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| Rekurzív |Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello. |TRUE hamis (alapértelmezés) |Nem |

**BlobSink** támogatja a következő tulajdonságai hello **typeProperties** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| copyBehavior |Hello másolási viselkedését határozza meg, ha hello adatforrás BlobSource vagy a fájlrendszer. |<b>PreserveHierarchy</b>: megtartja hello fájl hierarchia hello célmappában. hello relatív fájl toosource forrásmappa elérési út azonos toohello fájl tootarget célmappa relatív elérési útját.<br/><br/><b>FlattenHierarchy</b>: összes fájl hello forrásmappából a hello első szintű tároló mappa. hello fájljaira automatikusan létrehozott nevet adni. <br/><br/><b>Mergefiles típusú</b>: összes fájl forrásfájlból hello mappa tooone egyesíti. Ha hello fájl/Blob neve meg van adva, egyesített hello neve legyen hello megadott név; Ellenkező esetben lenne automatikusan létrehozott fájl nevét. |Nem |

**BlobSource** is támogatja a visszamenőleges kompatibilitás érdekében a két tulajdonság.

* **treatEmptyAsNull**: Megadja, hogy tootreat null vagy üres karakterlánc null értékként.
* **skipHeaderLineCount** -határozza meg, hogy hány sort kell figyelmen kívül hagyja. Azt is alkalmazható csak amikor bemeneti adatkészlet által használt szöveges.

Hasonlóképpen **BlobSink** tulajdonság a visszamenőleges kompatibilitás érdekében a következő hello támogatja.

* **blobWriterAddHeader**: meghatározza, hogy tooadd tooan írásakor oszlopdefiníciók fejléc kimeneti adatkészlet.

Következő tulajdonságai, amelyek megvalósítják az adatkészletek most támogatási hello hello ugyanezt a funkcionalitást: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.

hello következő táblázat nyújt útmutatást hello új adatkészlet tulajdonságai helyett a blob-forrás/fogadó tulajdonságok használatával.

| Másolja az Activity tulajdonság | A DataSet tulajdonság |
|:--- |:--- |
| a BlobSource skipHeaderLineCount |skipLineCount és firstRowAsHeader. Sorok először kimarad, és majd hello első sorának fejlécként olvasható. |
| a BlobSource treatEmptyAsNull |a bemeneti adatkészlet treatEmptyAsNull |
| a BlobSink blobWriterAddHeader |a kimeneti adatkészlet firstRowAsHeader |

Lásd: [megadása szöveges](data-factory-supported-file-and-compression-formats.md#text-format) szakasz ezeket a tulajdonságokat részletes tájékoztatást.    

### <a name="recursive-and-copybehavior-examples"></a>rekurzív és copyBehavior példák
Ez a szakasz ismerteti a hello viselkedésről hello másolási műveletek kombinációk rekurzív és copybehavior tulajdonságot.

| Rekurzív | copyBehavior | Viselkedésről |
| --- | --- | --- |
| Igaz |preserveHierarchy |A forrásmappa mappa1 a struktúra a következő hello: <br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello tároló mappa mappa1 azonos szerkezeti hello forrásaként hello jön létre<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| Igaz |flattenHierarchy |A forrásmappa mappa1 a struktúra a következő hello: <br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello cél mappa1 jön létre: <br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File5 |
| Igaz |mergefiles típusú |A forrásmappa mappa1 a struktúra a következő hello: <br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello cél mappa1 jön létre: <br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + fájl3 + File4 + 5 fájl tartalmát egy fájl automatikusan létrehozott fájlnévvel egyesülnek |
| hamis |preserveHierarchy |A forrásmappa mappa1 a struktúra a következő hello: <br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello tároló mappa mappa1 jön létre<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/><br/>Fájl3, File4 és File5 Subfolder1 nem átveszik. |
| hamis |flattenHierarchy |A forrásmappa mappa1 a struktúra a következő hello:<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello tároló mappa mappa1 jön létre<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2<br/><br/><br/>Fájl3, File4 és File5 Subfolder1 nem átveszik. |
| hamis |mergefiles típusú |A forrásmappa mappa1 a struktúra a következő hello:<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello tároló mappa mappa1 jön létre<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Egy fájl automatikusan létrehozott fájlnévvel egyesített file1 + File2 tartalma. automatikusan létrehozott nevet a file1 kiszolgálón<br/><br/>Fájl3, File4 és File5 Subfolder1 nem átveszik. |

## <a name="walkthrough-use-copy-wizard-toocopy-data-tofrom-blob-storage"></a>Forgatókönyv: Másolása varázsló használata toocopy adatok Blob Storage onnan
Nézzük hogyan tooquickly adatok másolása az Azure és a blob-tároló. Ebben a bemutatóban forrás- és célkiszolgálón adatokat tárolja, típus: Azure Blob Storage tárolóban. hello ebben a bemutatóban csővezeték másol adatokat hello egy mappáját tooanother blob tárolóhoz. Ez a forgatókönyv nem szándékosan egyszerű tooshow beállítások vagy a Blob Storage használata a forrás vagy a fogadó tulajdonságait. 

### <a name="prerequisites"></a>Előfeltételek
1. Hozzon létre egy általános célú **Azure Storage-fiók** Ha Ön nem rendelkezik ilyennel. Hello a blob storage használata egyaránt **forrás** és **cél** adattároló ebben a forgatókönyvben. Ha egy Azure storage-fiók nem rendelkezik, tekintse meg a hello [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) lépéseket toocreate egyet a cikkben találhat.
2. Hozzon létre egy blob-tároló nevű **adfblobconnector** hello tárfiókban. 
4. Hozza létre a **bemeneti** a hello **adfblobconnector** tároló.
5. Hozzon létre egy fájlt **emp.txt** hello követően a tartalmat, és töltse fel toohello **bemeneti** mappa eszközökkel, mint [Azure Tártallózó](https://azurestorageexplorer.codeplex.com/)
    ```json
    John, Doe
    Jane, Doe
    ```
### <a name="create-hello-data-factory"></a>Hello adat-előállító létrehozása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **+ új** hello bal felső sarokban, kattintson **Eszközintelligencia + analitika**, és kattintson a **adat-előállító**.
3. A hello **új adat-előállító** panel:   
    1. Adja meg **ADFBlobConnectorDF** a hello **neve**. az Azure data factory hello hello nevének globálisan egyedi kell lennie. Ha hello hibaüzenetet kapja: `*Data factory name “ADFBlobConnectorDF” is not available`, hello adat-előállítóban (például yournameADFBlobConnectorDF) hello nevének módosítása, és próbálja meg újra létrehozni. A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.
    2. Jelölje ki az Azure-**előfizetést**.
    3. Az erőforráscsoport, válassza ki a **használata meglévő** tooselect egy meglévő erőforrás csoport (vagy) válassza **hozzon létre új** tooenter erőforráscsoport nevét.
    4. Válassza ki a **hely** hello adat-előállító esetében.
    5. Válassza ki **PIN-kód toodashboard** hello hello panel alsó részén jelölőnégyzetet.
    6. Kattintson a **Create** (Létrehozás) gombra.
3. Hello létrehozásának befejezése után megjelenik a hello **adat-előállító** panelen látható hello kép a következő módon: ![Data factory kezdőlap](./media/data-factory-azure-blob-connector/data-factory-home-page.png)

### <a name="copy-wizard"></a>Másolás varázsló
1. A hello adat-előállító kezdőlapján kattintson a hello **[előzetes verzió] adatok másolása** toolaunch csempe **másolása varázsló** egy külön lapján.    
    
    > [!NOTE]
    >    Ha azt látja, hogy hello webböngésző akadt-e a "Engedélyező...", tiltsa le vagy törölje a jelet **külső cookie-k blokkolását, és a helyadatok** beállítása (vagy) engedélyezve legyen, és hozzon létre egy kivételt **login.microsoftonline.com**, és ezután próbálja meg újból elindítani a hello varázsló.
2. A hello **tulajdonságok** lap:
    1. Adja meg **CopyPipeline** a **feladatnév**. hello feladatnév: hello hello kimenetátirányítási mechanizmusával a data factory neve.
    2. Adjon meg egy **leírás** hello feladathoz (választható).
    3. A **feladat ütemben történik vagy ütemezett feladat**, tartsa hello **futtatása rendszeres ütemezés szerint** lehetőséget. Ha ezt a feladatot csak egyszer ahelyett, hogy futhat ismétlődően egy ütemezés szerint toorun, jelölje be **futtassa egyszer most**. Választ ki, ha **futtassa egyszer most** beállítást, a [egyszeri folyamat](data-factory-create-pipelines.md#onetime-pipeline) jön létre. 
    4. Megtarthatja hello **ismétlődő mintát**. Ez a feladat futtatása naponta hello közötti kezdési és befejezési időpontokat, adja meg a következő lépésben hello.
    5. Változás hello **kezdő dátum/idő** túl**04/21/2017**. 
    6. Változás hello **záró dátum és idő** túl**04/25/2017**. Érdemes lehet tootype hello dátum keresse meg azt hello naptár helyett.     
    8. Kattintson a **Tovább** gombra.
      ![Másolja az eszköz - Tulajdonságok lap](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png) 
3. A hello **forrás adattár** kattintson **Azure Blob Storage** csempére. Az oldal toospecify hello forrás adattároló hello másolási feladathoz kell használnia. Használhatja egy meglévő adattár társított szolgáltatását, vagy megadhat egy új adattárat. egy meglévő toouse társított szolgáltatás, a kiválasztott **a meglévő összekapcsolt szolgáltatások** , és válassza ki a megfelelő társított szolgáltatás hello. 
    ![Másolja az eszköz - forrás egy adattárolási lap](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)
4. A hello **hello Azure Blob storage-fiók megadása** lap:
   1. Tartsa hello automatikusan létrehozott nevet **kapcsolatnév**. hello kapcsolat neve a következő típusú csatolt hello szolgáltatást hello neve: Azure Storage. 
   2. Győződjön meg arról, hogy az **Account selection method** (Fiókválasztási módszer) mezőben a **From Azure subscriptions** (Azure-előfizetésekből) lehetőség van kiválasztva.
   3. Válassza ki az Azure-előfizetéssel, vagy hagyja **válassza ki az összes** a **Azure-előfizetés**.   
   4. Válasszon egy **Azure storage-fiók** hello a hello kiválasztott előfizetésben elérhető fiókok az Azure storage listája. Másik lehetőségként tooenter tárolási fiók beállításait manuálisan kiválasztásával **adja meg manuálisan** hello beállítása **kijelöléséről fiók**.
   5. Kattintson a **Tovább** gombra. 
      ![Másolja az eszköz – hello Azure Blob storage-fiók megadása](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)
5. A **válasszon hello bemeneti fájl vagy mappa** lap:
   1. Kattintson duplán a **adfblobcontainer**.
   2. Válassza ki **bemeneti**, és kattintson a **válasszon**. Ebben a bemutatóban hello bemeneti mappa kiválasztása. Kiválaszthatja hello emp.txt fájl hello mappában helyette. 
      ![Másolja az eszköz - a hello bemeneti fájl vagy mappa](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)
6. A hello **válasszon hello bemeneti fájl vagy mappa** lap:
    1. Győződjön meg arról, hogy hello **fájl vagy mappa** értéke túl**adfblobconnector/bemeneti**. Ha hello fájlok runbookok, 2017/04/01, 2017/04/02, és így tovább, írja be például adfblobconnector/bemeneti / {year} / {month} / {day} fájlra vagy mappára. TAB billentyű megnyomásával kívül hello szövegmezőben, három legördülő listákból tooselect formátumok láthatja az év (éééé), a hónap (MM), és a nap (nn). 
    2. Ne adja meg az **fájl rekurzív módon másolja**. Válassza ezt a beállítást toorecursively bejárás fájlok toobe másolt toohello cél mappákban. 
    3. Nem hello **bináris másolási** lehetőséget. Válassza ezt a beállítást tooperform forrás fájl toohello cél bináris másolatát. Nem jelölt ki ez a forgatókönyv, hogy a további beállítások a következő oldalain hello láthatja. 
    4. Győződjön meg arról, hogy hello **tömörítési típus** értéke túl**nincs**. Válassza az ehhez a beállításhoz tartozó értéket, ha a forrásfájlok tömörített hello támogatott formátumok egyikében. 
    5. Kattintson a **Tovább** gombra.
    ![Másolja az eszköz - a hello bemeneti fájl vagy mappa](./media/data-factory-azure-blob-connector/chose-input-file-folder.png) 
7. A hello **fájl formázási beállítások** lapon látni hello határolójelek és hello sémát, amelyhez hello varázsló automatikusan észleli hello-fájl elemzése. 
    1. Erősítse meg az alábbi beállítások hello: egy. Hello **fájlformátum** értéke túl**szövegformátum**. Minden hello támogatott formátumok hello legördülő listában tekintheti meg. Például: JSON, Avro, ORC, Parquet.
        b. Hello **oszlop elválasztó** értéke túl`Comma (,)`. Látható hello más hello legördülő listából válassza ki a Data Factory által támogatott oszlop elválasztó karaktert. Egy egyéni elválasztó karakter is megadható.
        c. Hello **sor elválasztó** értéke túl`Carriage Return + Line feed (\r\n)`. Látható hello más hello legördülő listából válassza ki a Data Factory által támogatott sor elválasztó karaktert. Egy egyéni elválasztó karakter is megadható.
        d. Hello **sorszám kihagyása** értéke túl**0**. Ha azt szeretné, hogy néhány sorok toobe hello felső hello fájl kihagyva, adja meg itt a hello számát.
        e.  Hello **adatok első sora oszlopneveket tartalmaz** nincs beállítva. Ha hello forrásfájlok hello első sorában oszlopneveket tartalmaz, akkor válassza ezt a lehetőséget.
        f. Hello **üres oszlopérték tekinti null** beállítás.
    2. Bontsa ki a **speciális beállítások** toosee speciális beállítás érhető el.
    3. Hello hello lap alsó részén, lásd: hello **előzetes** adatok hello emp.txt fájlból.
    4. Kattintson a **SÉMA** hello forrásfájlban hello adatok megtekintésével következtetni hello másolása varázsló hello alsó toosee hello séma lapot.
    5. Kattintson a **következő** után tekintse át a hello elválasztó karaktert, és tekintse meg adatok.
    ![Eszköz - fájl formátuma beállítások másolása](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)  
8. A hello **cél adattároló lap**, jelölje be **Azure Blob Storage**, és kattintson a **következő**. Hello Azure Blob Storage mindkét hello forrás és cél adatokat tárolja a forgatókönyv használ.    
    ![Eszköz - célkiszolgáló kijelölése adattár másolása](media/data-factory-azure-blob-connector/select-destination-data-store.png)
9. A **hello Azure Blob storage-fiók megadása** lap:
   1. Adja meg **AzureStorageLinkedService** a hello **kapcsolatnév** mező.
   2. Győződjön meg arról, hogy az **Account selection method** (Fiókválasztási módszer) mezőben a **From Azure subscriptions** (Azure-előfizetésekből) lehetőség van kiválasztva.
   3. Jelölje ki az Azure-**előfizetést**.  
   4. Válassza ki az Azure storage-fiók. 
   5. Kattintson a **Tovább** gombra.     
10. A hello **válasszon hello kimeneti fájl vagy mappa** lap: 
    6. Adja meg **mappa elérési útja** , **adfblobconnector/kimeneti / {year} / {month} / {day}**. Adja meg **lapon**.
    7. A hello **év**, jelölje be **éééé**.
    8. A hello **hónap**, győződjön meg arról, hogy túl van-e állítva**MM**.
    9. A hello **nap**, győződjön meg arról, hogy túl van-e állítva**nn**.
    10. Győződjön meg arról, hogy hello **tömörítési típus** értéke túl**nincs**.
    11. Győződjön meg arról, hogy hello **viselkedés másolása** értéke túl**fájlok egyesítése**. Ha hello kimeneti hello ugyanazzal a névvel már létezik a fájl, hello új tartalma hozzáadott toohello ugyanazon fájl hello végén.
    12. Kattintson a **Tovább** gombra.
    ![Másolja az eszköz - a kimeneti fájl vagy mappa](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)
11. A hello **fájl formázási beállítások** lapon tekintse át a hello-beállítások, és kattintson a **következő**. Hello Itt a további beállítások egyike tooadd egy fejléc toohello kimeneti fájl. Ha ezt a beállítást választja, a fejlécsor hozzá rendelkező hello oszlopainak hello séma hello forrás. Átnevezheti hello alapértelmezett oszlopnevek hello séma hello forrás megtekintésekor. Megváltoztathatja például a hello első oszlop tooFirst nevét és a második oszlopban tooLast nevét. Ezt követően hello kimeneti fájl jön létre az ezekkel a nevekkel fejléc oszlopnevekként. 
    ![Eszköz - cél formátum beállításainak fájl másolása](media/data-factory-azure-blob-connector/file-format-destination.png)
12. A hello **Teljesítménybeállítások** lapján ellenőrizze, hogy **egységek cloud** és **másolatok párhuzamos** túl van beállítva**automatikus**, és kattintson a Tovább gombra. Ezek a beállítások kapcsolatos részletekért lásd: [másolása tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md#parallel-copy).
    ![Eszköz - teljesítmény beállítások másolása](media/data-factory-azure-blob-connector/copy-performance-settings.png) 
14. A hello **összegzés** lapon tekintse át az összes beállítást (a feladat tulajdonságai, a beállításokat a forrás és cél és a beállítások), és kattintson a **következő**.
    ![Másolja az eszköz - összegző lapja](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)
15. Tekintse át a hello **összegzés** lapon, majd kattintson **Befejezés**. hello varázsló létrehoz két társított szolgáltatások, két adatkészletet (bemeneti és kimeneti) és egy folyamat (amelyen Ön indítani hello másolása varázsló) az adat-előállító hello.
    ![Másolja az eszköz - központi telepítés lap](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)

### <a name="monitor-hello-pipeline-copy-task"></a>A figyelő hello csővezeték (feladatok)

1. Hello hivatkozásra `Click here toomonitor copy pipeline` a hello **telepítési** lap. 
2. Megtekintheti az hello **felügyeletéhez és kezeléséhez az alkalmazás** egy külön lapján.  ![Megfigyelés és kezelés alkalmazás](media/data-factory-azure-blob-connector/monitor-manage-app.png)
3. Változás hello **start** idő hello felső túl`04/19/2017` és **end** idő túl`04/27/2017`, és kattintson a **alkalmaz**. 
4. Megtekintheti az öt tevékenység a windows hello **tevékenység WINDOWS** listája. Hello **WindowStart** alkalommal közé tartozik a csővezeték indítási toopipeline befejezési idejének minden nap. 
5. Kattintson a **frissítése** hello gombra **tevékenység WINDOWS** lista néhány alkalommal amíg minden hello tevékenység windows hello állapotának tooReady van beállítva. 
6. Most győződjön meg arról, hogy a kimeneti fájlok hello adfblobconnector tároló hello kimeneti mappában legyenek létrehozva. A következő mappaszerkezet hello kimeneti mappában hello kell megjelennie: 
    ```
    2017/04/21
    2017/04/22
    2017/04/23
    2017/04/24
    2017/04/25    
    ```
Figyelése és kezelése az adat-előállítók kapcsolatos részletes információkért lásd: [figyelése és kezelése a Data Factory-folyamathoz](data-factory-monitor-manage-app.md) cikk. 
 
### <a name="data-factory-entities"></a>Data Factory entitások
Most váltson vissza toohello hello adat-előállító Kezdőlap lap. Figyelje meg, hogy van két összekapcsolt szolgáltatások, két adatkészletet, és a data factory egyik adatcsatornáinak most. 

![Data Factory kezdőlapján entitások](media/data-factory-azure-blob-connector/data-factory-home-page-with-numbers.png)

Kattintson a **Szerző és központi telepítése** toolaunch Data Factory Editor. 

![A Data Factory szerkesztője](media/data-factory-azure-blob-connector/data-factory-editor.png)

A következő adat-előállító bejegyzései szerepelnek a data factory hello kell megjelennie: 

 - Két társított szolgáltatások. Egy a hello forrás és cél hello egy másikra hello. Mindkét kapcsolódó hello szolgáltatás toohello tekintse meg a forgatókönyv azonos Azure Storage-fiók. 
 - Két adatkészletet. Egy bemeneti adatkészlet és egy kimeneti adatkészletet. Ez a forgatókönyv mindkettő használja a hello azonos blob-tároló, de toodifferent mappák (bemeneti és kimeneti) hivatkozik.
 - Egy folyamat. hello feldolgozási sor tartalmazza a másolási tevékenység, amely egy blob-forrás- és egy blob fogadó toocopy adatokat az Azure blob hely tooanother Azure blob helyére használja. 

hello következő szakaszok további információval szolgálnak ezeket az entitásokat. 

#### <a name="linked-services"></a>Társított szolgáltatások
Két összekapcsolt szolgáltatások kell megjelennie. Egy a hello forrás és cél hello egy másikra hello. Mindkét definíciók tekintse meg a forgatókönyv hello hello nevek kivételével azonos. Hello **típus** hello a társított szolgáltatás túl van beállítva**AzureStorage**. Hello társított szolgáltatás definíciójának legfontosabb tulajdonsága hello **connectionString**, amely adat-előállító tooconnect tooyour futásidőben Azure Storage-fiókot használja. Hagyja figyelmen kívül hello hubName tulajdonság hello definícióban. 

##### <a name="source-blob-storage-linked-service"></a>Forrás blob storage társított szolgáltatás
```json
{
    "name": "Source-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

##### <a name="destination-blob-storage-linked-service"></a>Cél blob storage társított szolgáltatás

```json
{
    "name": "Destination-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

Az Azure tárolás társított szolgáltatásának kapcsolatos további információkért lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz. 

#### <a name="datasets"></a>Adathalmazok
Két adatkészletet van: egy bemeneti adatkészlet és egy kimeneti adatkészletet. hello típusú hello dataset értéke túl**AzureBlob** is. 

hello bemeneti adatkészlet mutat toohello **bemeneti** mappában található hello **adfblobconnector** blob tároló. Hello **külső** tulajdonsága túl**igaz** ehhez az adatkészlethez, hello adatok nem hozzák hello csővezeték hello másolási tevékenységhez, amely ehhez az adatkészlethez bemenetként. 

kimeneti adatkészlet pontok toohello hello **kimeneti** mappában található hello blob tárolóhoz. hello kimeneti adatkészletet is használ hello év, hónap és hello napján **SliceStart** rendszer változó toodynamically kiértékelése hello hello kimeneti fájl elérési útját. Függvények és a Data Factory által támogatott rendszerváltozók listáját lásd: [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md). Hello **külső** tulajdonsága túl**hamis** (alapértelmezett érték), mert ez az adatkészlet hozzák hello folyamat. 

Azure Blob-adathalmazra által támogatott tulajdonságokról kapcsolatos további információkért lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.

##### <a name="input-dataset"></a>Bemeneti adatkészlet

```json
{
    "name": "InputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Source-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/input/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

##### <a name="output-dataset"></a>Kimeneti adatkészlet

```json
{
    "name": "OutputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Destination-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/output/{year}/{month}/{day}",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
            "partitionedBy": [
                { "name": "year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }
            ]
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

#### <a name="pipeline"></a>Folyamat
hello folyamat csak egy tevékenység rendelkezik. Hello **típus** hello tevékenység van állítva túl**másolási**.  A típus tulajdonságai hello hello tevékenység szakaszok is vannak két, egy a forrás- és a fogadó egy másikra hello. hello adatforrás típusának értéke túl**BlobSource** , hello tevékenység van az adatok másolása egy blob Storage tárolóban. hello a fogadó típusa értéke túl**BlobSink** hello tevékenységként adattárolás tooa blob másolása. hello másolási tevékenység InputDataset-z4y hello bemeneti és OutputDataset-z4y hello kimenetként vesz igénybe. 

BlobSource és BlobSink által támogatott tulajdonságokról kapcsolatos további információkért lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz. 

```json
{
    "name": "CopyPipeline",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "MergeFiles",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-z4y"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-z4y"
                    }
                ],
                "policy": {
                    "timeout": "1.00:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 3,
                    "longRetry": 0,
                    "longRetryInterval": "00:00:00"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "Activity-0-Blob path_ adfblobconnector_input_->OutputDataset-z4y"
            }
        ],
        "start": "2017-04-21T22:34:00Z",
        "end": "2017-04-25T05:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
```

## <a name="json-examples-for-copying-data-tooand-from-blob-storage"></a>Az adatok tooand másolását a Blob Storage JSON példák  
hello alábbi példák megadják minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azok hogyan toocopy adatok tooand az Azure Blob Storage és az Azure SQL Database. Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott hello nyelő források tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.

### <a name="json-example-copy-data-from-blob-storage-toosql-database"></a>JSON-NÁ. példa: Adatok másolása a Blob Storage tooSQL adatbázis
a következő példa azt mutatja be hello:

1. A társított szolgáltatás típusa [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [BlobSource](#copy-activity-properties) és [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).

hello minta idősorozat adatainak másolása az Azure blob tooan Azure SQL-táblából óránként. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

**Az Azure SQL társított szolgáltatásnak:**

```json
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
**Az Azure tárolás társított szolgáltatásának:**

```json
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
Az Azure Data Factory két típusú Azure Storage társított szolgáltatásokat támogat: **AzureStorage** és **AzureStorageSas**. Az első hello, hello kapcsolati karakterlánc, amely tartalmazza az hello fiókkulcs ad meg, és hello újabb egy, a közös hozzáférésű Jogosultságkód (SAS) Uri hello meg. Lásd: [összekapcsolt szolgáltatások](#linked-service-properties) című szakaszban talál információt.  

**Az Azure Blob bemeneti adatkészletet:**

Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján. hello mappa elérési útját használja év, hónap és nap részét hello kezdési ideje, valamint fájlnév hello kezdő időpontja óra részét hello. "external": "true" beállítás arról értesíti az hello tábla külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák adat-Előállítóban.

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
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
**Az Azure SQL kimeneti adatkészlet:**

hello minta Azure SQL adatbázis "MyTable" nevű tooa adattábla másolja. Hello tábla létrehozása az Azure SQL adatbázis azonos számú oszlopot hello hello Blob CSV-fájl toocontain várt. Új sorok hozzáadásakor toohello tábla óránként.

```json
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**A másolási tevékenység során a Blob-forrás és fogadó SQL-feldolgozási folyamat:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**BlobSource** és **fogadó** típusuk értéke túl**SqlSink**.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
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
### <a name="json-example-copy-data-from-azure-sql-tooazure-blob"></a>JSON-NÁ. példa: Adatok másolása az Azure SQL tooAzure Blob
a következő példa azt mutatja be hello:

1. A társított szolgáltatás típusa [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) és [BlobSink](#copy-activity-properties).

hello minta másol idősorozat adatokat az Azure SQL-tábla tooan Azure blob óránként. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

**Az Azure SQL társított szolgáltatásnak:**

```json
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
**Az Azure tárolás társított szolgáltatásának:**

```json
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
Az Azure Data Factory két típusú Azure Storage társított szolgáltatásokat támogat: **AzureStorage** és **AzureStorageSas**. Az első hello, hello kapcsolati karakterlánc, amely tartalmazza az hello fiókkulcs ad meg, és hello újabb egy, a közös hozzáférésű Jogosultságkód (SAS) Uri hello meg. Lásd: [összekapcsolt szolgáltatások](#linked-service-properties) című szakaszban talál információt.  

**Az Azure SQL bemeneti adatkészlet:**

hello minta azt feltételezi, hogy létrehozott egy tábla "MyTable" Azure SQL, egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.

"External" beállítása: "true" tájékoztatja Data Factory szolgáltatásnak, hogy hello tábla külső toohello adat-előállító, ezért nem hozzák hello adat-előállítóban tevékenységgel.

```json
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

**Az Azure Blob kimeneti adatkészlet:**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
      "partitionedBy": [
        {
          "name": "Year",
          "value": { "type": "DateTime",  "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
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

**A másolási tevékenység során az SQL-forrás és fogadó Blob egy folyamaton belül:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**SqlSource** és **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott hello **SqlReaderQuery** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureSQLInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
