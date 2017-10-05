---
title: "Gyors referenciaként a Microsoft Azure Import/Export eszköz importálási feladat parancsok - 1-es verzió |} Microsoft Docs"
description: "Az Azure Import/Export eszköz parancsreferencia a gyakran használt importálási feladat parancsok. A v1 az Import/Export eszköz vonatkozik."
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
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 47f450ee87dac3db2ccf7659928d52a6330a5697
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Az importálási feladatokhoz használt gyakori parancsok rövid áttekintése
Ez a témakör egy gyors hivatkozásokat az egyes gyakran használt parancsok. Tekintse meg a részletes használati [előkészítése merevlemezek hogy az importálás](storage-import-export-tool-preparing-hard-drives-import-v1.md).  

## <a name="prepare-the-disks-when-data-already-copied-to-the-disks"></a>Készítse elő a lemezeket, amikor adatokat már másolja lemezei
 Íme egy példa a parancsra készítse elő a lemezeket, amikor adatokat már másolja a merevlemez-meghajtóról, amely még nem lett még nincs a Bitlockerrel titkosítani:  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-to-a-hard-drive"></a>Másolja a egyetlen könyvtárat a merevlemez-meghajtóra  
 Íme egy minta parancs egyetlen forráskönyvtárat átmásolása egy merevlemezen, amely még nem lett még nincs a Bitlockerrel titkosítani:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-wwo-directories-to-a-hard-drive"></a>A merevlemez-meghajtóról wwo könyvtárak másolása  
 Két forrás könyvtárak másolása meghajtóra, akkor két parancsot.  
  
 Az első parancs meghatározza a naplókönyvtár, a tárfiók hívóbetűjét, a célként megadott meghajtóbetűjel és `format/encrypt` követelményeket, a következő általános paramétereket:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 A második parancs határozza meg, a napló fájlt, egy új munkamenet-Azonosítót és a forrás és cél helyeken:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-to-a-hard-drive-in-a-second-copy-session"></a>A merevlemez-meghajtóról egy második példány munkamenetben nagy fájl másolása  
 Íme egy minta parancs, amely egy előző munkamenetben másolási előkészített meghajtóra másolja át a egyetlen nagy fájlok:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a>Következő lépések

* [Munkafolyamat-minta a merevlemezek importálási feladatokhoz való előkészítésére](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
