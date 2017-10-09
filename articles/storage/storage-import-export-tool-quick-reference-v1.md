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
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a><span data-ttu-id="222e7-104">Az importálási feladatokhoz használt gyakori parancsok rövid áttekintése</span><span class="sxs-lookup"><span data-stu-id="222e7-104">Quick reference for frequently used commands for import jobs</span></span>
<span data-ttu-id="222e7-105">Ez a témakör egy gyors hivatkozásokat az egyes gyakran használt parancsok.</span><span class="sxs-lookup"><span data-stu-id="222e7-105">This section provides a quick references for some frequently used commands.</span></span> <span data-ttu-id="222e7-106">Tekintse meg a részletes használati [előkészítése merevlemezek hogy az importálás](storage-import-export-tool-preparing-hard-drives-import-v1.md).</span><span class="sxs-lookup"><span data-stu-id="222e7-106">For detailed usage, see [Preparing Hard Drives for an Import Job](storage-import-export-tool-preparing-hard-drives-import-v1.md).</span></span>  

## <a name="prepare-hello-disks-when-data-already-copied-toohello-disks"></a><span data-ttu-id="222e7-107">Készítse elő a hello lemezek adatok már toohello lemezek másolása</span><span class="sxs-lookup"><span data-stu-id="222e7-107">Prepare hello disks when data already copied toohello disks</span></span>
 <span data-ttu-id="222e7-108">Íme egy minta parancs tooprepare a lemezek adatok már másolását toohello merevlemez-meghajtóról, amely még nem lett még nincs a Bitlockerrel titkosítani:</span><span class="sxs-lookup"><span data-stu-id="222e7-108">Here is a sample command tooprepare a disks when data already copied toohello hard drive that hasn't been yet been encrypted with BitLocker:</span></span>  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-tooa-hard-drive"></a><span data-ttu-id="222e7-109">Egyetlen tooa merevlemez-meghajtóra másolni</span><span class="sxs-lookup"><span data-stu-id="222e7-109">Copy a single directory tooa hard drive</span></span>  
 <span data-ttu-id="222e7-110">Íme egy minta parancs toocopy egyetlen adatforrás directory tooa merevlemez-meghajtóról, amely még nem lett még nincs a Bitlockerrel titkosítani:</span><span class="sxs-lookup"><span data-stu-id="222e7-110">Here is a sample command toocopy a single source directory tooa hard drive that hasn't been yet been encrypted with BitLocker:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-wwo-directories-tooa-hard-drive"></a><span data-ttu-id="222e7-111">Wwo könyvtárak tooa merevlemez-meghajtó másolása</span><span class="sxs-lookup"><span data-stu-id="222e7-111">Copy wwo directories tooa hard drive</span></span>  
 <span data-ttu-id="222e7-112">toocopy két forrás könyvtárak tooa meghajtó, szüksége lesz a két parancsot.</span><span class="sxs-lookup"><span data-stu-id="222e7-112">toocopy two source directories tooa drive, you will need two commands.</span></span>  
  
 <span data-ttu-id="222e7-113">hello első parancs meghatározza hello naplókönyvtár, a tárfiók hívóbetűjét, a célként megadott meghajtóbetűjel és `format/encrypt` követelményeinek, továbbá toohello általános paraméterek:</span><span class="sxs-lookup"><span data-stu-id="222e7-113">hello first command specifies hello log directory, storage account key, target drive letter and `format/encrypt` requirements, in addition toohello common parameters:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 <span data-ttu-id="222e7-114">hello második parancsot határozza meg a hello napló fájlt, egy új munkamenet-Azonosítót és hello forrás és cél helyeken:</span><span class="sxs-lookup"><span data-stu-id="222e7-114">hello second command specifies hello journal file, a new session ID, and hello source and destination locations:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-tooa-hard-drive-in-a-second-copy-session"></a><span data-ttu-id="222e7-115">Nagy méretű fájlok tooa merevlemez-meghajtóra másolja a második példány munkamenetben</span><span class="sxs-lookup"><span data-stu-id="222e7-115">Copy a large file tooa hard drive in a second copy session</span></span>  
 <span data-ttu-id="222e7-116">Íme egy minta parancs, amely egy előző munkamenetben másolási előkészített egyetlen nagy méretű fájlok tooa meghajtót másolja:</span><span class="sxs-lookup"><span data-stu-id="222e7-116">Here is a sample command that copies a single large file tooa drive that has been prepared in a previous copy session:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a><span data-ttu-id="222e7-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="222e7-117">Next steps</span></span>

* [<span data-ttu-id="222e7-118">Minta munkafolyamat tooprepare merevlemezeit, az importálási feladat</span><span class="sxs-lookup"><span data-stu-id="222e7-118">Sample workflow tooprepare hard drives for an import job</span></span>](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
