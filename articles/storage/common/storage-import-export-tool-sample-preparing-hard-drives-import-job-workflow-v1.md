---
title: "aaaSample munkafolyamat tooprep merevlemez-meghajtók az Azure Import/Export importálási feladat - 1-es verzió |} Microsoft Docs"
description: "Tekintse meg a teljes folyamat hello meghajtók előkészítése az importálási feladat hello Azure Import/Export szolgáltatás egy bemutató."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 6eb1b1b7-c69f-4365-b5ef-3cd5e05eb72a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: eb77831a88c16c14838179a6432ddb06503067dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sample-workflow-tooprepare-hard-drives-for-an-import-job"></a>Minta munkafolyamat tooprepare merevlemezeit, az importálási feladat
Ez a témakör bemutatja, hogyan hello teljes folyamata meghajtók előkészítése az importálási feladat.  
  
Ebben a példában importálja a következő adatok azokat a Windows Azure storage-fiók nevű hello `mystorageaccount`:  
  
|Hely|Leírás|  
|--------------|-----------------|  
|H:\Video|Gyűjteménye videók, összesen 5 TB.|  
|H:\Photo|Egy gyűjtemény fényképek, összesen 30 GB.|  
|K:\Temp\FavoriteMovie.ISO|A Blu-ray™ lemezképét, 25 GB.|  
|\\\bigshare\john\music|Letilthatja a zenei fájlok egy hálózati megosztáson, 10 GB-os teljes gyűjteményét.|  
  
hello importálási feladat ezek az adatok importálása a következő célhoz hello tárfiók hello:  
  
|Forrás|Cél virtuális könyvtárat vagy blob|  
|------------|-------------------------------------------|  
|H:\Video|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Photo|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|  
|K:\Temp\FavoriteMovie.ISO|https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO|  
|\\\bigshare\john\music|https://mystorageaccount.BLOB.Core.Windows.NET/Music|  
  
Ez a leképezés a hello fájl `H:\Video\Drama\GreatMovie.mov` importált toohello BLOB `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.  
  
Ezt követően toodetermine hány merevlemezek szükségesek, számítási hello hello adatok mérete:  
  
`5TB + 30GB + 25GB + 10GB = 5TB + 65GB`  
  
Az ebben a példában két 3 TB merevlemezek elegendőnek kell lennie. Mivel hello forráskönyvtár azonban `H:\Video` 5 TB adatot, és az egyetlen merevlemez-területtel csak 3 TB, szükséges toobreak `H:\Video` be két kisebb könyvtárak: `H:\Video1` és `H:\Video2`, mielőtt fut a Microsoft hello Az Azure Import/Export eszköz. Ebben a lépésben adja eredményül a következő forrás könyvtárak hello:  
  
|Hely|Méret|Cél virtuális könyvtárat vagy blob|  
|--------------|----------|-------------------------------------------|  
|H:\Video1|2,5 TB|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Video2|2,5 TB|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Photo|30 GB|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|  
|K:\Temp\FavoriteMovies.ISO|25 GB|https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO|  
|\\\bigshare\john\music|10 GB|https://mystorageaccount.BLOB.Core.Windows.NET/Music|  
  
 Akkor is, ha hello `H:\Video`directory felosztott tootwo könyvtárak, toohello pontok azonos cél virtuális könyvtár hello tárfiókban. Így minden videofájlok megmaradnak az egyetlen `video` hello tárfiókban lévő tároló.  
  
 A következő hello előző forrás címtárai egyenletesen toohello két merevlemez-meghajtóval:  
  
||||  
|-|-|-|  
|Merevlemez-meghajtó|Forrás könyvtárak|Teljes méret|  
|Első meghajtó|H:\Video1|2.5 TB + 30 GB|  
||H:\Photo||  
|Második meghajtó|H:\Video2|2.5 TB + 35 GB|  
||K:\Temp\BlueRay.ISO||  
||\\\bigshare\john\music||  
  
Ezenkívül a következő összes fájlok metaadatait hello állíthatja be:  
  
-   **UploadMethod:** Windows Azure Import/Export szolgáltatás  
  
-   **DataSetName:** SampleData  
  
-   **CreationDate:** 10/1/2013  
  
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
  
-   **Content-Type:** application/octet-stream  
  
-   **Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==  
  
-   **A Cache-Control:** no-cache  
  
tooset ezeket a tulajdonságokat, hozzon létre egy szövegfájlt `c:\WAImportExport\SampleProperties.txt`:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
Most már készen áll a toorun hello Azure Import/Export eszköz tooprepare hello két merevlemez-meghajtók áll. Vegye figyelembe:  
  
-   az első meghajtó hello X meghajtóként lett-e csatlakoztatva.  
  
-   hello második meghajtó Y meghajtóként lett-e csatlakoztatva.  
  
-   hello hello kulcsának `mystorageaccount` van `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.  

## <a name="preparing-disk-for-import-when-data-is-pre-loaded"></a>Lemez előkészítése importálása, amikor az adatok előzetes betöltése
 
 Ha hello adatok toobe importált már hello lemezen, akkor használja a hello jelző /skipwrite. /t és /srcdir hello értékének kell mindkét pont toohello lemez való importálás. Ha az összes hello adatok toobe importált nem lesz toohello azonos a cél virtuális könyvtár vagy a főtanúsítvány hello tárfiók, azonos parancsot minden célkönyvtáron külön-külön futtassa hello /id hello értékének tartása hello azonos keresztül minden futtatásakor.

>[!NOTE] 
>Ne adjon meg Format, azt fogja hello lemezen hello adatainak törlése. Adja meg, / titkosítása vagy /bk attól függően, hogy hello lemez már titkosítva van-e. 
>

```
    When data is already present on hello disk for each drive run hello following command.
    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:x:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt /skipwrite
```

## <a name="copy-sessions---first-drive"></a>Másolja a munkamenet - először meghajtó

Hello első meghajtót futtassa hello Azure Import/Export eszköz kétszer két toocopy hello forrás-könyvtárak:  

**Először másolja az munkamenet**
  
```
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```

**Második példány munkamenet**

```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt
```

## <a name="copy-sessions---second-drive"></a>Másolja a munkamenet - meghajtó másodpercenként
 
A hello második meghajtó, futtassa hello Azure Import/Export eszköz háromszor, amennyiben az egyes hello a forrás-könyvtárak, és egyszer az hello önálló Blu-Ray™ fájl kép):  
  
**Először másolja az munkamenet** 

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Video2 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:y /format /encrypt /srcdir:H:\Video2 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```
  
**Második példány munkamenet**

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Music /srcdir:\\bigshare\john\music /dstdir:music/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```  
  
**Harmadik másolatot munkamenet**  

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```

## <a name="copy-session-completion"></a>Másolja a munkamenet befejezése

Hello másolási munkamenetek befejezése után hello két meghajtók leválasztása hello másolási számítógépről, és toohello megfelelő Windows Azure-adatközpont küldje el. A két hello Adatbázisnapló-fájlok feltöltése `FirstDrive.jrn` és `SecondDrive.jrn`, hello hello importálási feladat létrehozásakor [Windows Azure portálon](https://manage.windowsazure.com/).  
  
## <a name="next-steps"></a>Következő lépések

* [Merevlemezek előkészítése importálási feladatokhoz](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [A gyakran használt parancsok rövid összefoglalása](../storage-import-export-tool-quick-reference-v1.md) 
