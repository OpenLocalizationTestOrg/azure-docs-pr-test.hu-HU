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
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a><span data-ttu-id="5fb0a-104">Az importálási feladatokhoz használt gyakori parancsok rövid áttekintése</span><span class="sxs-lookup"><span data-stu-id="5fb0a-104">Quick reference for frequently used commands for import jobs</span></span>
<span data-ttu-id="5fb0a-105">Ez a témakör egy gyors hivatkozásokat az egyes gyakran használt parancsok.</span><span class="sxs-lookup"><span data-stu-id="5fb0a-105">This section provides a quick references for some frequently used commands.</span></span> <span data-ttu-id="5fb0a-106">Tekintse meg a részletes használati [előkészítése merevlemezek hogy az importálás](storage-import-export-tool-preparing-hard-drives-import-v1.md).</span><span class="sxs-lookup"><span data-stu-id="5fb0a-106">For detailed usage, see [Preparing Hard Drives for an Import Job](storage-import-export-tool-preparing-hard-drives-import-v1.md).</span></span>  

## <a name="prepare-the-disks-when-data-already-copied-to-the-disks"></a><span data-ttu-id="5fb0a-107">Készítse elő a lemezeket, amikor adatokat már másolja lemezei</span><span class="sxs-lookup"><span data-stu-id="5fb0a-107">Prepare the disks when data already copied to the disks</span></span>
 <span data-ttu-id="5fb0a-108">Íme egy példa a parancsra készítse elő a lemezeket, amikor adatokat már másolja a merevlemez-meghajtóról, amely még nem lett még nincs a Bitlockerrel titkosítani:</span><span class="sxs-lookup"><span data-stu-id="5fb0a-108">Here is a sample command to prepare a disks when data already copied to the hard drive that hasn't been yet been encrypted with BitLocker:</span></span>  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-to-a-hard-drive"></a><span data-ttu-id="5fb0a-109">Másolja a egyetlen könyvtárat a merevlemez-meghajtóra</span><span class="sxs-lookup"><span data-stu-id="5fb0a-109">Copy a single directory to a hard drive</span></span>  
 <span data-ttu-id="5fb0a-110">Íme egy minta parancs egyetlen forráskönyvtárat átmásolása egy merevlemezen, amely még nem lett még nincs a Bitlockerrel titkosítani:</span><span class="sxs-lookup"><span data-stu-id="5fb0a-110">Here is a sample command to copy a single source directory to a hard drive that hasn't been yet been encrypted with BitLocker:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-wwo-directories-to-a-hard-drive"></a><span data-ttu-id="5fb0a-111">A merevlemez-meghajtóról wwo könyvtárak másolása</span><span class="sxs-lookup"><span data-stu-id="5fb0a-111">Copy wwo directories to a hard drive</span></span>  
 <span data-ttu-id="5fb0a-112">Két forrás könyvtárak másolása meghajtóra, akkor két parancsot.</span><span class="sxs-lookup"><span data-stu-id="5fb0a-112">To copy two source directories to a drive, you will need two commands.</span></span>  
  
 <span data-ttu-id="5fb0a-113">Az első parancs meghatározza a naplókönyvtár, a tárfiók hívóbetűjét, a célként megadott meghajtóbetűjel és `format/encrypt` követelményeket, a következő általános paramétereket:</span><span class="sxs-lookup"><span data-stu-id="5fb0a-113">The first command specifies the log directory, storage account key, target drive letter and `format/encrypt` requirements, in addition to the common parameters:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 <span data-ttu-id="5fb0a-114">A második parancs határozza meg, a napló fájlt, egy új munkamenet-Azonosítót és a forrás és cél helyeken:</span><span class="sxs-lookup"><span data-stu-id="5fb0a-114">The second command specifies the journal file, a new session ID, and the source and destination locations:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-to-a-hard-drive-in-a-second-copy-session"></a><span data-ttu-id="5fb0a-115">A merevlemez-meghajtóról egy második példány munkamenetben nagy fájl másolása</span><span class="sxs-lookup"><span data-stu-id="5fb0a-115">Copy a large file to a hard drive in a second copy session</span></span>  
 <span data-ttu-id="5fb0a-116">Íme egy minta parancs, amely egy előző munkamenetben másolási előkészített meghajtóra másolja át a egyetlen nagy fájlok:</span><span class="sxs-lookup"><span data-stu-id="5fb0a-116">Here is a sample command that copies a single large file to a drive that has been prepared in a previous copy session:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a><span data-ttu-id="5fb0a-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5fb0a-117">Next steps</span></span>

* [<span data-ttu-id="5fb0a-118">Munkafolyamat-minta a merevlemezek importálási feladatokhoz való előkészítésére</span><span class="sxs-lookup"><span data-stu-id="5fb0a-118">Sample workflow to prepare hard drives for an import job</span></span>](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
