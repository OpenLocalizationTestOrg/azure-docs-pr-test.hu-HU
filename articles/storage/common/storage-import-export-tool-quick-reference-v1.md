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
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a><span data-ttu-id="16287-104">Az importálási feladatokhoz használt gyakori parancsok rövid áttekintése</span><span class="sxs-lookup"><span data-stu-id="16287-104">Quick reference for frequently used commands for import jobs</span></span>
<span data-ttu-id="16287-105">Ez a témakör egy rövid összefoglaló néhány gyakran használt parancsok.</span><span class="sxs-lookup"><span data-stu-id="16287-105">This section provides a quick reference for some frequently used commands.</span></span> <span data-ttu-id="16287-106">Tekintse meg a részletes használati [előkészítése merevlemezek hogy az importálás](../storage-import-export-tool-preparing-hard-drives-import-v1.md).</span><span class="sxs-lookup"><span data-stu-id="16287-106">For detailed usage, see [Preparing Hard Drives for an Import Job](../storage-import-export-tool-preparing-hard-drives-import-v1.md).</span></span>  

## <a name="prepare-a-hard-drive-when-data-has-already-been-copied-toohello-hard-drive"></a><span data-ttu-id="16287-107">Készítse elő egy merevlemezen, amikor az adatok már be lett másolva toohello merevlemez-meghajtó</span><span class="sxs-lookup"><span data-stu-id="16287-107">Prepare a hard drive when data has already been copied toohello hard drive</span></span>
 <span data-ttu-id="16287-108">a következő parancs hello előkészíti a merevlemez-meghajtóról, adatok másolt tooit már megtörtént, de a BitLocker még nincs titkosítva:</span><span class="sxs-lookup"><span data-stu-id="16287-108">hello following command prepares a hard drive when data has already been copied tooit, but has not yet been encrypted with BitLocker:</span></span>  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-tooa-hard-drive"></a><span data-ttu-id="16287-109">Egyetlen tooa merevlemez-meghajtóra másolni</span><span class="sxs-lookup"><span data-stu-id="16287-109">Copy a single directory tooa hard drive</span></span>  
 <span data-ttu-id="16287-110">a következő parancs hello másolja át egy egyetlen forrásai directory tooa merevlemezen a BitLocker még nem titkosított:</span><span class="sxs-lookup"><span data-stu-id="16287-110">hello following command copies a single source directory tooa hard drive that has not yet been encrypted with BitLocker:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-two-directories-tooa-hard-drive"></a><span data-ttu-id="16287-111">Két könyvtárak tooa merevlemez másolása</span><span class="sxs-lookup"><span data-stu-id="16287-111">Copy two directories tooa hard drive</span></span>  
 <span data-ttu-id="16287-112">a két toocopy forrás könyvtárak tooa meghajtó, a következő parancsok használata hello:</span><span class="sxs-lookup"><span data-stu-id="16287-112">toocopy two source directories tooa drive, use hello following commands:</span></span>  
  
 <span data-ttu-id="16287-113">hello első parancs meghatározza hello naplókönyvtár, a tárfiók hívóbetűjét, a célként megadott meghajtóbetűjel, `format/encrypt` követelményeket és az általános paraméterek:</span><span class="sxs-lookup"><span data-stu-id="16287-113">hello first command specifies hello log directory, storage account key, target drive letter, `format/encrypt` requirements, and common parameters:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 <span data-ttu-id="16287-114">hello második parancsot határozza meg a hello napló fájlt, egy új munkamenet-Azonosítót és hello forrás és cél helyeken:</span><span class="sxs-lookup"><span data-stu-id="16287-114">hello second command specifies hello journal file, a new session ID, and hello source and destination locations:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-tooa-hard-drive-in-a-second-copy-session"></a><span data-ttu-id="16287-115">Nagy méretű fájlok tooa merevlemez-meghajtóra másolja a második példány munkamenetben</span><span class="sxs-lookup"><span data-stu-id="16287-115">Copy a large file tooa hard drive in a second copy session</span></span>  
 <span data-ttu-id="16287-116">a következő parancs hello másolja át egy egyetlen nagy méretű fájlok tooa merevlemezen, amely egy előző munkamenetben másolási előkészített:</span><span class="sxs-lookup"><span data-stu-id="16287-116">hello following command copies a single large file tooa hard drive that was prepared in a previous copy session:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a><span data-ttu-id="16287-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="16287-117">Next steps</span></span>

* [<span data-ttu-id="16287-118">Minta munkafolyamat tooprepare merevlemezeit, az importálási feladat</span><span class="sxs-lookup"><span data-stu-id="16287-118">Sample workflow tooprepare hard drives for an import job</span></span>](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
