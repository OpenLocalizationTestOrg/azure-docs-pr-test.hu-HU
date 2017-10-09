---
title: "az Azure Import/Export eszköz importálási feladat parancsok aaaQuick referencia |} Microsoft Docs"
description: "Az Azure Import/Export eszköz parancsreferencia a gyakran használt importálási feladat parancsok."
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
ms.openlocfilehash: 35e46f24f764a5098ca465adb51badcab2d13e9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a><span data-ttu-id="1d021-103">Az importálási feladatokhoz használt gyakori parancsok rövid áttekintése</span><span class="sxs-lookup"><span data-stu-id="1d021-103">Quick reference for frequently used commands for import jobs</span></span>

<span data-ttu-id="1d021-104">Ez a cikk ismerteti, hogy egy rövid összefoglaló néhány gyakran használt parancsok.</span><span class="sxs-lookup"><span data-stu-id="1d021-104">This article provides a quick reference for some frequently used commands.</span></span> <span data-ttu-id="1d021-105">Tekintse meg a részletes használati [előkészítése merevlemezek hogy az importálás](../storage-import-export-tool-preparing-hard-drives-import.md).</span><span class="sxs-lookup"><span data-stu-id="1d021-105">For detailed usage, see [Preparing Hard Drives for an Import Job](../storage-import-export-tool-preparing-hard-drives-import.md).</span></span>

## <a name="first-session"></a><span data-ttu-id="1d021-106">Első munkamenet</span><span class="sxs-lookup"><span data-stu-id="1d021-106">First session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1 /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

## <a name="second-session"></a><span data-ttu-id="1d021-107">Második munkamenet</span><span class="sxs-lookup"><span data-stu-id="1d021-107">Second session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /DataSet:dataset-2.csv
```

## <a name="abort-latest-session"></a><span data-ttu-id="1d021-108">Legújabb munkamenet megszakítása</span><span class="sxs-lookup"><span data-stu-id="1d021-108">Abort latest session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /AbortSession
```

## <a name="resume-latest-interrupted-session"></a><span data-ttu-id="1d021-109">Legújabb megszakított munkamenet folytatása</span><span class="sxs-lookup"><span data-stu-id="1d021-109">Resume latest interrupted session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /ResumeSession
```

## <a name="add-drives-toolatest-session"></a><span data-ttu-id="1d021-110">Adja hozzá a meghajtók toolatest munkamenet</span><span class="sxs-lookup"><span data-stu-id="1d021-110">Add drives toolatest session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /AdditionalDriveSet:driveset-2.csv
```

## <a name="next-steps"></a><span data-ttu-id="1d021-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d021-111">Next steps</span></span>

* [<span data-ttu-id="1d021-112">Minta munkafolyamat tooprepare merevlemezeit, az importálási feladat</span><span class="sxs-lookup"><span data-stu-id="1d021-112">Sample workflow tooprepare hard drives for an import job</span></span>](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
