---
title: "a Microsoft Azure Import/Export eszköz importálási feladat parancsok - v1 aaaQuick referencia |} Microsoft Docs"
description: "Az Azure Import/Export eszköz parancsreferencia a gyakran használt importálási feladat parancsok. Az Import/Export eszköz hello toov1 utal."
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
ms.openlocfilehash: e36f065e5d23268758cf6b6db9428fe8a8e1056d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Az importálási feladatokhoz használt gyakori parancsok rövid áttekintése
Ez a témakör egy gyors hivatkozásokat az egyes gyakran használt parancsok. Tekintse meg a részletes használati [előkészítése merevlemezek hogy az importálás](storage-import-export-tool-preparing-hard-drives-import-v1.md).  

## <a name="prepare-hello-disks-when-data-already-copied-toohello-disks"></a>Készítse elő a hello lemezek adatok már toohello lemezek másolása
 Íme egy minta parancs tooprepare a lemezek adatok már másolását toohello merevlemez-meghajtóról, amely még nem lett még nincs a Bitlockerrel titkosítani:  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-tooa-hard-drive"></a>Egyetlen tooa merevlemez-meghajtóra másolni  
 Íme egy minta parancs toocopy egyetlen adatforrás directory tooa merevlemez-meghajtóról, amely még nem lett még nincs a Bitlockerrel titkosítani:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-wwo-directories-tooa-hard-drive"></a>Wwo könyvtárak tooa merevlemez-meghajtó másolása  
 toocopy két forrás könyvtárak tooa meghajtó, szüksége lesz a két parancsot.  
  
 hello első parancs meghatározza hello naplókönyvtár, a tárfiók hívóbetűjét, a célként megadott meghajtóbetűjel és `format/encrypt` követelményeinek, továbbá toohello általános paraméterek:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 hello második parancsot határozza meg a hello napló fájlt, egy új munkamenet-Azonosítót és hello forrás és cél helyeken:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-tooa-hard-drive-in-a-second-copy-session"></a>Nagy méretű fájlok tooa merevlemez-meghajtóra másolja a második példány munkamenetben  
 Íme egy minta parancs, amely egy előző munkamenetben másolási előkészített egyetlen nagy méretű fájlok tooa meghajtót másolja:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a>Következő lépések

* [Minta munkafolyamat tooprepare merevlemezeit, az importálási feladat](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
