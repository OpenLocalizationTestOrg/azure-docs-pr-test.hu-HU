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
ms.openlocfilehash: 945eb4f9eff28c92ec963585d27cba73a7eb59bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Az importálási feladatokhoz használt gyakori parancsok rövid áttekintése
Ez a témakör egy rövid összefoglaló néhány gyakran használt parancsok. Tekintse meg a részletes használati [előkészítése merevlemezek hogy az importálás](../storage-import-export-tool-preparing-hard-drives-import-v1.md).  

## <a name="prepare-a-hard-drive-when-data-has-already-been-copied-toohello-hard-drive"></a>Készítse elő egy merevlemezen, amikor az adatok már be lett másolva toohello merevlemez-meghajtó
 a következő parancs hello előkészíti a merevlemez-meghajtóról, adatok másolt tooit már megtörtént, de a BitLocker még nincs titkosítva:  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-tooa-hard-drive"></a>Egyetlen tooa merevlemez-meghajtóra másolni  
 a következő parancs hello másolja át egy egyetlen forrásai directory tooa merevlemezen a BitLocker még nem titkosított:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-two-directories-tooa-hard-drive"></a>Két könyvtárak tooa merevlemez másolása  
 a két toocopy forrás könyvtárak tooa meghajtó, a következő parancsok használata hello:  
  
 hello első parancs meghatározza hello naplókönyvtár, a tárfiók hívóbetűjét, a célként megadott meghajtóbetűjel, `format/encrypt` követelményeket és az általános paraméterek:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 hello második parancsot határozza meg a hello napló fájlt, egy új munkamenet-Azonosítót és hello forrás és cél helyeken:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-tooa-hard-drive-in-a-second-copy-session"></a>Nagy méretű fájlok tooa merevlemez-meghajtóra másolja a második példány munkamenetben  
 a következő parancs hello másolja át egy egyetlen nagy méretű fájlok tooa merevlemezen, amely egy előző munkamenetben másolási előkészített:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a>Következő lépések

* [Minta munkafolyamat tooprepare merevlemezeit, az importálási feladat](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
