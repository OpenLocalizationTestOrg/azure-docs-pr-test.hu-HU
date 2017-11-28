---
title: "aaaReviewing Azure Import/Export feladatállapot - 1-es verzió |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse hello naplófájlokat, ha létre hello importálása vagy exportálása feladat volt futtatva toosee hello hello importálási/exportálási feladatok állapotát."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: c69d1d69-6403-4eee-9949-0185faeecfa1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: muralikk
ms.openlocfilehash: 378bc9a7ef6bfe65209413c8c4134f313a2c0d79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a><span data-ttu-id="c88dd-103">A másolási naplófájlok Azure Import/Export feladat állapota áttekintése</span><span class="sxs-lookup"><span data-stu-id="c88dd-103">Reviewing Azure Import/Export job status with copy log files</span></span>
<span data-ttu-id="c88dd-104">A Microsoft Azure Import/Export szolgáltatás hello egy importálása vagy exportálása feladattal társított meghajtók dolgozza fel, írja az másolási napló fájlok toohello tárolási fiók tooor, amelyből importálunk vagy bináris objektumok exportálása.</span><span class="sxs-lookup"><span data-stu-id="c88dd-104">When hello Microsoft Azure Import/Export service processes drives associated with an import or export job, it writes copy log files toohello storage account tooor from which you are importing or exporting blobs.</span></span> <span data-ttu-id="c88dd-105">hello naplófájl minden olyan fájlról, amely lett importálva, illetve nem exportálható részletes állapotát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c88dd-105">hello log file contains detailed status about each file that was imported or exported.</span></span> <span data-ttu-id="c88dd-106">Befejezett feladatok; hello állapotának lekérdezése hello URL-cím tooeach másolási naplófájl adott vissza. Lásd: [Get Job](/rest/api/storageservices/Get-Job3) további információt.</span><span class="sxs-lookup"><span data-stu-id="c88dd-106">hello URL tooeach copy log file is returned when you query hello status of a completed job; see [Get Job](/rest/api/storageservices/Get-Job3) for more information.</span></span>  

## <a name="example-urls"></a><span data-ttu-id="c88dd-107">Példa URL-címek</span><span class="sxs-lookup"><span data-stu-id="c88dd-107">Example URLs</span></span>

<span data-ttu-id="c88dd-108">Az alábbiakban hello példa URL-címek másolása naplófájlok hogy az importálás két meghajtók:</span><span class="sxs-lookup"><span data-stu-id="c88dd-108">hello following are example URLs for copy log files for an import job with two drives:</span></span>  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 <span data-ttu-id="c88dd-109">Lásd: [Import/Export szolgáltatás naplófájlformátum](storage-import-export-file-format-log.md) hello formátum másolási naplók és állapotkódok hello teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="c88dd-109">See [Import/Export service Log File Format](storage-import-export-file-format-log.md) for hello format of copy logs and hello full list of status codes.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="c88dd-110">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c88dd-110">Next steps</span></span>
 
 * [<span data-ttu-id="c88dd-111">Hello Azure Import/Export eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="c88dd-111">Setting Up hello Azure Import/Export Tool</span></span>](storage-import-export-tool-setup-v1.md)   
 * [<span data-ttu-id="c88dd-112">Merevlemezek előkészítése importálási feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="c88dd-112">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [<span data-ttu-id="c88dd-113">Importálási feladat javítása</span><span class="sxs-lookup"><span data-stu-id="c88dd-113">Repairing an import job</span></span>](storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [<span data-ttu-id="c88dd-114">Exportálási feladat javítása</span><span class="sxs-lookup"><span data-stu-id="c88dd-114">Repairing an export job</span></span>](storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [<span data-ttu-id="c88dd-115">Hibaelhárítási hello Azure Import/Export eszköz</span><span class="sxs-lookup"><span data-stu-id="c88dd-115">Troubleshooting hello Azure Import/Export Tool</span></span>](storage-import-export-tool-troubleshooting-v1.md)
