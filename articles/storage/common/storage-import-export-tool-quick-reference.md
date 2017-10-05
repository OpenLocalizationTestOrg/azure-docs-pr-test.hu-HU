---
title: "Azure Import/Export eszköz importálási feladat parancsok rövid összefoglalása |} Microsoft Docs"
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
ms.openlocfilehash: d51ae35ead0e7d8289de663e5b7b48d28271e810
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a><span data-ttu-id="5f08b-103">Az importálási feladatokhoz használt gyakori parancsok rövid áttekintése</span><span class="sxs-lookup"><span data-stu-id="5f08b-103">Quick reference for frequently used commands for import jobs</span></span>

<span data-ttu-id="5f08b-104">Ez a cikk ismerteti, hogy egy rövid összefoglaló néhány gyakran használt parancsok.</span><span class="sxs-lookup"><span data-stu-id="5f08b-104">This article provides a quick reference for some frequently used commands.</span></span> <span data-ttu-id="5f08b-105">Tekintse meg a részletes használati [előkészítése merevlemezek hogy az importálás](../storage-import-export-tool-preparing-hard-drives-import.md).</span><span class="sxs-lookup"><span data-stu-id="5f08b-105">For detailed usage, see [Preparing Hard Drives for an Import Job](../storage-import-export-tool-preparing-hard-drives-import.md).</span></span>

## <a name="first-session"></a><span data-ttu-id="5f08b-106">Első munkamenet</span><span class="sxs-lookup"><span data-stu-id="5f08b-106">First session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1 /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

## <a name="second-session"></a><span data-ttu-id="5f08b-107">Második munkamenet</span><span class="sxs-lookup"><span data-stu-id="5f08b-107">Second session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /DataSet:dataset-2.csv
```

## <a name="abort-latest-session"></a><span data-ttu-id="5f08b-108">Legújabb munkamenet megszakítása</span><span class="sxs-lookup"><span data-stu-id="5f08b-108">Abort latest session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /AbortSession
```

## <a name="resume-latest-interrupted-session"></a><span data-ttu-id="5f08b-109">Legújabb megszakított munkamenet folytatása</span><span class="sxs-lookup"><span data-stu-id="5f08b-109">Resume latest interrupted session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /ResumeSession
```

## <a name="add-drives-to-latest-session"></a><span data-ttu-id="5f08b-110">Adja hozzá meghajtók legújabb-munkamenethez</span><span class="sxs-lookup"><span data-stu-id="5f08b-110">Add drives to latest session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /AdditionalDriveSet:driveset-2.csv
```

## <a name="next-steps"></a><span data-ttu-id="5f08b-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5f08b-111">Next steps</span></span>

* [<span data-ttu-id="5f08b-112">Munkafolyamat-minta a merevlemezek importálási feladatokhoz való előkészítésére</span><span class="sxs-lookup"><span data-stu-id="5f08b-112">Sample workflow to prepare hard drives for an import job</span></span>](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
