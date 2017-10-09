---
title: "aaaSample munkafolyamat tooprep merevlemez-meghajtók az Azure Import/Export importálási feladat |} Microsoft Docs"
description: "Tekintse meg a teljes folyamat hello meghajtók előkészítése az importálási feladat hello Azure Import/Export szolgáltatás egy bemutató."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: muralikk
ms.openlocfilehash: fa7f36300b35b81757523de4960fd583bd4bf305
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sample-workflow-tooprepare-hard-drives-for-an-import-job"></a>Minta munkafolyamat tooprepare merevlemezeit, az importálási feladat

Ez a cikk bemutatja, hogyan hello teljes folyamata meghajtók előkészítése az importálási feladat.

## <a name="sample-data"></a>Mintaadatok

Ez a példa importál adatokat az Azure-tárfiók neve a következő hello `mystorageaccount`:

|Hely|Leírás|Adatok mérete|
|--------------|-----------------|-----|
|H:\Video\ |Videók gyűjteménye|12 TB|
|H:\Photo\ |Fotók|30 GB|
|K:\Temp\FavoriteMovie.ISO|A Blu-ray™ lemezképe|25 GB|
|\\\bigshare\john\music\|Letilthatja a zenei fájlok egy hálózati megosztáson|10 GB|

## <a name="storage-account-destinations"></a>Tárolási fiók célok

hello importálási feladat hello adatok importálása a következő célhoz hello tárfiók hello:

|Forrás|Cél virtuális könyvtárat vagy blob|
|------------|-------------------------------------------|
|H:\Video\ |videó /|
|H:\Photo\ |fénykép vagy|
|K:\Temp\FavoriteMovie.ISO|favorite/FavoriteMovies.ISO|
|\\\bigshare\john\music\ |Zene|

Ez a leképezés a hello fájl `H:\Video\Drama\GreatMovie.mov` importált toohello blob lesz `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.

## <a name="determine-hard-drive-requirements"></a>Merevlemez-meghajtóról követelmények meghatározása

Ezt követően toodetermine hány merevlemezek szükségesek, számítási hello hello adatok mérete:

`12TB + 30GB + 25GB + 10GB = 12TB + 65GB`

Az ebben a példában két 8 TB-os merevlemezeket elegendőnek kell lennie. Hello forráskönyvtár mivel azonban `H:\Video` 12TB adatot, és az egyetlen merevlemez-területtel csak 8TB, fogja tudni toospecify ezt a következő módon hello hello **driveset.csv** fájlt:

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
X,Format,SilentMode,Encrypt,
Y,Format,SilentMode,Encrypt,
```
hello eszköz lesz el az adatokat két merevlemez-meghajtók optimalizált módon.

## <a name="attach-drives-and-configure-hello-job"></a>Meghajtó csatlakoztatása és hello feladat konfigurálása
A rendszer Csatlakoztassa mindkét lemezek toohello gép, és hozzon létre köteteket. Majd szerzői **dataset.csv** fájlt:
```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

Ezenkívül a következő összes fájlok metaadatait hello állíthatja be:

* **UploadMethod:** Windows Azure Import/Export szolgáltatás
* **DataSetName:** SampleData
* **CreationDate:** 10/1/2013

importált hello fájlok metaadatait tooset hozzon létre egy szövegfájlt, `c:\WAImportExport\SampleMetadata.txt`, a tartalom a következő hello:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

Hello bizonyos tulajdonságait is beállíthat `FavoriteMovie.ISO` blob:

* **Content-Type:** application/octet-stream
* **Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==
* **A Cache-Control:** no-cache

tooset ezeket a tulajdonságokat, hozzon létre egy szövegfájlt `c:\WAImportExport\SampleProperties.txt`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

## <a name="run-hello-azure-importexport-tool-waimportexportexe"></a>Futtatási hello Azure Import/Export eszköz (WAImportExport.exe)

Most már készen áll a toorun hello Azure Import/Export eszköz tooprepare hello két merevlemez-meghajtók áll.

**Az első munkamenet hello:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

Ha több adat hozzáadott toobe, hozzon létre egy másik dataset fájlt (Initialdataset megegyező formátumban).

**A második munkamenet hello:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

Hello másolási munkamenetek befejezése után hello két meghajtók leválasztása hello másolási számítógépről, és toohello megfelelő Azure-adatközpont küldje el. Hello két Adatbázisnapló-fájlok feltöltése lesz `<FirstDriveSerialNumber>.xml` és `<SecondDriveSerialNumber>.xml`, hello Azure-portálon hello importálási feladat létrehozásakor.

## <a name="next-steps"></a>Következő lépések

* [Merevlemezek előkészítése importálási feladatokhoz](../storage-import-export-tool-preparing-hard-drives-import.md)
* [A gyakran használt parancsok rövid összefoglalása](../storage-import-export-tool-quick-reference.md)
