---
title: "Az Azure Import/Export eszköz hibaelhárítása |} Microsoft Docs"
description: "További tudnivalók azzal kapcsolatban, az az Azure Import/Export eszköz, és hogyan kezelje őket használata során tapasztalt gyakori problémákat."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: b91ca5eb-c557-460a-9afc-0590b38471f9
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 7bfda602dbc0ea47828a7c9243b8b9b09ec78432
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshooting-the-azure-importexport-tool"></a><span data-ttu-id="059b6-103">Az Azure Import/Export eszköz hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="059b6-103">Troubleshooting the Azure Import/Export Tool</span></span>
<span data-ttu-id="059b6-104">A Microsoft Azure Import/Export eszköz hibaüzeneteket ad vissza, ha fut a problémákat.</span><span class="sxs-lookup"><span data-stu-id="059b6-104">The Microsoft Azure Import/Export Tool returns error messages if it runs into issues.</span></span> <span data-ttu-id="059b6-105">Ez a témakör néhány gyakori probléma, amely futtathatják azokat.</span><span class="sxs-lookup"><span data-stu-id="059b6-105">This topic lists some common issues that users may run into.</span></span>  
  
## <a name="a-copy-session-fails-what-i-should-do"></a><span data-ttu-id="059b6-106">A másolási munkamenet nem sikerül, mit kell tenni?</span><span class="sxs-lookup"><span data-stu-id="059b6-106">A copy session fails, what I should do?</span></span>  
 <span data-ttu-id="059b6-107">A másolási munkamenet sikertelensége esetén van két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="059b6-107">When a copy session fails, there are two options:</span></span>  
  
 <span data-ttu-id="059b6-108">Ha a hiba Újrapróbálkozást lehetővé tevő, például ha a hálózati megosztás rövid ideig offline állapotban volt, és most újra online állapotba kerül, a Másolás munkamenet folytathatja.</span><span class="sxs-lookup"><span data-stu-id="059b6-108">If the error is retryable, for example if the network share was offline for a short period and now is back online, you can resume the copy session.</span></span> <span data-ttu-id="059b6-109">Ha a hiba nem Újrapróbálkozást lehetővé tevő, például ha a fájl nem megfelelő forráskönyvtár szerepel a parancssori paraméterek, a Másolás munkamenet megszakítása szeretné.</span><span class="sxs-lookup"><span data-stu-id="059b6-109">If the error is not retryable, for example if you specified the wrong source file directory in the command line parameters, you need to abort the copy session.</span></span> <span data-ttu-id="059b6-110">Lásd: [előkészítése merevlemezek hogy az importálás](../storage-import-export-tool-preparing-hard-drives-import-v1.md) folytatása és másolási munkamenet megszakítása további információt.</span><span class="sxs-lookup"><span data-stu-id="059b6-110">See [Preparing Hard Drives for an Import Job](../storage-import-export-tool-preparing-hard-drives-import-v1.md) for more information about resuming and aborting copy sessions.</span></span>  
  
## <a name="i-cant-resume-or-abort-a-copy-session"></a><span data-ttu-id="059b6-111">Nem lehet folytatni vagy egy másolás munkamenet megszakítása.</span><span class="sxs-lookup"><span data-stu-id="059b6-111">I can't resume or abort a copy session.</span></span>  
 <span data-ttu-id="059b6-112">Ha a példány neve az első másolási munkamenet meghajtóhoz, akkor a hibaüzenet a következő kell nyújtaniuk: "az első másolás munkamenet nem folytatható vagy megszakadt."</span><span class="sxs-lookup"><span data-stu-id="059b6-112">If the copy session is the first copy session for a drive, then the error message should state: "The first copy session cannot be resumed or aborted."</span></span> <span data-ttu-id="059b6-113">Ebben az esetben törölje a régi napló fájlt, és futtassa újból a parancsot.</span><span class="sxs-lookup"><span data-stu-id="059b6-113">In this case, you can delete the old journal file and rerun the command.</span></span>  
  
 <span data-ttu-id="059b6-114">Ha a Másolás munkamenet nincs az elsőt a meghajtók, azt is mindig folytatódik vagy megszakadt.</span><span class="sxs-lookup"><span data-stu-id="059b6-114">If a copy session is not the first one for a drive, it can always be resumed or aborted.</span></span>  
  
## <a name="i-lost-the-journal-file-can-i-still-create-the-job"></a><span data-ttu-id="059b6-115">A naplófájl elvész, továbbra is létrehozhatók a feladatot?</span><span class="sxs-lookup"><span data-stu-id="059b6-115">I lost the journal file, can I still create the job?</span></span>  
 <span data-ttu-id="059b6-116">A naplófájl a meghajtók adat másolása az ezen a meghajtón a teljes információkat tartalmaz, és szükség esetén vegyen fel további fájlokat a meghajtóra és az importálási feladat létrehozására használható.</span><span class="sxs-lookup"><span data-stu-id="059b6-116">The journal file for a drive contains the complete information of copying data to this drive, and it is needed to add more files to the drive and will be used to create an import job.</span></span> <span data-ttu-id="059b6-117">Ha a naplófájl elveszik, akkor végezze el újra a meghajtó másolása munkamenetek.</span><span class="sxs-lookup"><span data-stu-id="059b6-117">If the journal file is lost, you will have to redo all the copy sessions for the drive.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="059b6-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="059b6-118">Next steps</span></span>
 
* [<span data-ttu-id="059b6-119">Az azure import/export eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="059b6-119">Setting up the azure import/export tool</span></span>](../storage-import-export-tool-setup-v1.md)   
* [<span data-ttu-id="059b6-120">Merevlemezek előkészítése importálási feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="059b6-120">Preparing hard drives for an import job</span></span>](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="059b6-121">Feladatok állapotának áttekintése a másolási naplófájlok segítségével</span><span class="sxs-lookup"><span data-stu-id="059b6-121">Reviewing job status with copy log files</span></span>](../storage-import-export-tool-reviewing-job-status-v1.md)   
* [<span data-ttu-id="059b6-122">Importálási feladat javítása</span><span class="sxs-lookup"><span data-stu-id="059b6-122">Repairing an import job</span></span>](../storage-import-export-tool-repairing-an-import-job-v1.md)   
* [<span data-ttu-id="059b6-123">Exportálási feladat javítása</span><span class="sxs-lookup"><span data-stu-id="059b6-123">Repairing an export job</span></span>](../storage-import-export-tool-repairing-an-export-job-v1.md)
