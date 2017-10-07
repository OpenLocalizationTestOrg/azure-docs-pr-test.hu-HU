---
title: "aaaPreviewing meghajtó használata az Azure Import/Export exportálási feladat - 1-es verzió |} Microsoft Docs"
description: "Ismerje meg, hogyan toopreview hello listája blobok kiválasztott hello Azure Import/Export szolgáltatás exportálási feladat."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 7707d744-7ec7-4de8-ac9b-93a18608dc9a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 88495f921371458c0451da6878fd7cc9a45d20cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="previewing-drive-usage-for-an-export-job"></a>Meghajtóhasználat előnézete exportálási feladatokhoz
Az exportálási feladatot hoz létre, meg kell toochoose blobok toobe készlete exportált. hello Microsoft Azure Import/Export szolgáltatás lehetővé teszi a blob elérési utak listájának toouse vagy blob előtagok toorepresent hello blobok választott.  
  
A következő lépésben toodetermine hány meghajtók toosend van szüksége. hello Import/Export eszköz biztosít hello `PreviewExport` hello blobok parancs toopreview meghajtóinak használatát választotta, toouse fog hello meghajtók hello mérete alapján.

## <a name="command-line-parameters"></a>Parancssori paraméterek

Használhatja a hello használata esetén a következő paraméterek hello `PreviewExport` az Import/Export eszköz hello parancsot.

|Parancssori paraméter|Leírás|  
|--------------------------|-----------------|  
|**/ logdir:**< LogDirectory\>|Választható. hello naplókönyvtár. Részletes naplófájlok toothis directory lesz írva. Ha nincs naplókönyvtár meg van adva, hello aktuális könyvtárhoz hello naplókönyvtár lesz.|  
|**/sn:**< StorageAccountName\>|Kötelező. hello hello hello storage-fiók nevét a feladat exportálja.|  
|**/SK:**< StorageAccountKey\>|Csak ha nincs megadva a tároló SAS szükséges. hello fiók kulcsának hello hello a feladat exportálja.|  
|**/csas:**< ContainerSas\>|Szükséges, csak ha nincs megadva a tárfiók kulcsára. hello tárolója SAS listaelem hello blobok toobe hello exportálási feladat exportálni.|  
|**/ ExportBlobListFile:**< ExportBlobListFile\>|Kötelező. Elérési út toohello XML blob elérési utak listája tartalmazó fájlt, vagy blob elérési út előtagokat hello blobok toobe exportált. hello fájlformátum, amelyet a hello `BlobListBlobPath` hello elemének [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) hello Import/Export szolgáltatás REST API működését.|  
|**/ DriveSize:**< DriveSize\>|Kötelező. az exportálási feladat, a meghajtók toouse mérete hello *pl.*, 500 GB, 1,5 TB.|  

## <a name="command-line-example"></a>Parancssori – példa

hello következő példa bemutatja, hogyan hello `PreviewExport` parancs:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
hello blob lista exportfájl blob nevének és tartalmazhat blob-előtagok, ahogy az itt látható:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

hello Azure Import/Export eszköz exportált összes BLOB toobe sorolja fel, és számítja ki, hogyan őket hello meghajtóit megadott mérete, bármilyen szükséges terhelést, figyelembe véve majd meghajtók hello számának becslése toopack toohold hello blobokat és a meghajtó használata szükséges információ.  
  
Itt látható egy példa hello kimeneti, és nincs megadva tájékoztató naplófájlokat:  
  
```  
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  
  
Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB    
```  
  
## <a name="next-steps"></a>Következő lépések

* [Az Azure Import/Export eszköz referencia](storage-import-export-tool-how-to-v1.md)
