---
title: "aaaTroubleshooting hello Azure Import/Export eszköz |} Microsoft Docs"
description: "Megismerhet néhány hello tapasztalt gyakori problémákat mutatjuk hello Azure Import/Export eszköz használatakor, és hogyan toohandle őket."
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
ms.openlocfilehash: 5445cefe2703edf4d9d285f761433b7b66d8cb6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-azure-importexport-tool"></a><span data-ttu-id="f711f-103">Hibaelhárítási hello Azure Import/Export eszköz</span><span class="sxs-lookup"><span data-stu-id="f711f-103">Troubleshooting hello Azure Import/Export Tool</span></span>
<span data-ttu-id="f711f-104">Microsoft Azure Import/Export eszköz hello hibaüzenetek adja vissza, ha fut a problémákat.</span><span class="sxs-lookup"><span data-stu-id="f711f-104">hello Microsoft Azure Import/Export Tool returns error messages if it runs into issues.</span></span> <span data-ttu-id="f711f-105">Ez a témakör néhány gyakori probléma, amely futtathatják azokat.</span><span class="sxs-lookup"><span data-stu-id="f711f-105">This topic lists some common issues that users may run into.</span></span>  
  
## <a name="a-copy-session-fails-what-i-should-do"></a><span data-ttu-id="f711f-106">A másolási munkamenet nem sikerül, mit kell tenni?</span><span class="sxs-lookup"><span data-stu-id="f711f-106">A copy session fails, what I should do?</span></span>  
 <span data-ttu-id="f711f-107">A másolási munkamenet sikertelensége esetén van két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="f711f-107">When a copy session fails, there are two options:</span></span>  
  
 <span data-ttu-id="f711f-108">Ha hello hiba Újrapróbálkozást lehetővé tevő, például ha hálózati megosztásra hello offline állapotban volt, a rövid időszak, és most újra online állapotba kerül, folytathatja a hello másolási munkamenet.</span><span class="sxs-lookup"><span data-stu-id="f711f-108">If hello error is retryable, for example if hello network share was offline for a short period and now is back online, you can resume hello copy session.</span></span> <span data-ttu-id="f711f-109">Ha hello hiba nem Újrapróbálkozást lehetővé tevő, például ha a megadott hello hibás fájl forráskönyvtár hello parancssori paraméterek vannak, akkor tooabort hello másolási munkamenet.</span><span class="sxs-lookup"><span data-stu-id="f711f-109">If hello error is not retryable, for example if you specified hello wrong source file directory in hello command line parameters, you need tooabort hello copy session.</span></span> <span data-ttu-id="f711f-110">Lásd: [előkészítése merevlemezek hogy az importálás](storage-import-export-tool-preparing-hard-drives-import-v1.md) folytatása és másolási munkamenet megszakítása további információt.</span><span class="sxs-lookup"><span data-stu-id="f711f-110">See [Preparing Hard Drives for an Import Job](storage-import-export-tool-preparing-hard-drives-import-v1.md) for more information about resuming and aborting copy sessions.</span></span>  
  
## <a name="i-cant-resume-or-abort-a-copy-session"></a><span data-ttu-id="f711f-111">Nem lehet folytatni vagy egy másolás munkamenet megszakítása.</span><span class="sxs-lookup"><span data-stu-id="f711f-111">I can't resume or abort a copy session.</span></span>  
 <span data-ttu-id="f711f-112">Ha hello példány neve hello első másolás munkamenet meghajtóhoz, majd hello hibaüzenet kell nyújtaniuk: "hello első másolás munkamenet nem folytatható vagy megszakadt."</span><span class="sxs-lookup"><span data-stu-id="f711f-112">If hello copy session is hello first copy session for a drive, then hello error message should state: "hello first copy session cannot be resumed or aborted."</span></span> <span data-ttu-id="f711f-113">Ebben az esetben törölje hello régi napló fájlt, és futtassa újra hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="f711f-113">In this case, you can delete hello old journal file and rerun hello command.</span></span>  
  
 <span data-ttu-id="f711f-114">Ha a Másolás munkamenet nem van hello elsőt meghajtóhoz, azt is mindig folytatódik, vagy megszakadt.</span><span class="sxs-lookup"><span data-stu-id="f711f-114">If a copy session is not hello first one for a drive, it can always be resumed or aborted.</span></span>  
  
## <a name="i-lost-hello-journal-file-can-i-still-create-hello-job"></a><span data-ttu-id="f711f-115">Hello napló fájl elvesztése, továbbra is létrehozhatók hello feladatot?</span><span class="sxs-lookup"><span data-stu-id="f711f-115">I lost hello journal file, can I still create hello job?</span></span>  
 <span data-ttu-id="f711f-116">hello napló fájl meghajtóhoz hello teljes körű információkat toothis adatmeghajtó másolásának tartalmaz, és azt szükséges tooadd további fájlokat toohello meghajtót, azaz használt toocreate az importálási feladat.</span><span class="sxs-lookup"><span data-stu-id="f711f-116">hello journal file for a drive contains hello complete information of copying data toothis drive, and it is needed tooadd more files toohello drive and will be used toocreate an import job.</span></span> <span data-ttu-id="f711f-117">Ha hello naplófájl elveszik, akkor minden hello másolási munkamenet hello meghajtó tooredo.</span><span class="sxs-lookup"><span data-stu-id="f711f-117">If hello journal file is lost, you will have tooredo all hello copy sessions for hello drive.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="f711f-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f711f-118">Next steps</span></span>
 
* [<span data-ttu-id="f711f-119">Hello azure import/export eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="f711f-119">Setting up hello azure import/export tool</span></span>](storage-import-export-tool-setup-v1.md)   
* [<span data-ttu-id="f711f-120">Merevlemezek előkészítése importálási feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="f711f-120">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="f711f-121">Feladatok állapotának áttekintése a másolási naplófájlok segítségével</span><span class="sxs-lookup"><span data-stu-id="f711f-121">Reviewing job status with copy log files</span></span>](storage-import-export-tool-reviewing-job-status-v1.md)   
* [<span data-ttu-id="f711f-122">Importálási feladat javítása</span><span class="sxs-lookup"><span data-stu-id="f711f-122">Repairing an import job</span></span>](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [<span data-ttu-id="f711f-123">Exportálási feladat javítása</span><span class="sxs-lookup"><span data-stu-id="f711f-123">Repairing an export job</span></span>](storage-import-export-tool-repairing-an-export-job-v1.md)
