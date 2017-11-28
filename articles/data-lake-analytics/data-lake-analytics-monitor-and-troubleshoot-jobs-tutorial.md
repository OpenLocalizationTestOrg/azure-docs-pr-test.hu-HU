---
title: "Azure portál használatával aaaTroubleshoot Azure Data Lake Analytics-feladatok |} Microsoft Docs"
description: 'Ismerje meg, hogyan toouse hello Azure Portal tootroubleshoot Data Lake Analytics-feladatok. '
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: e810d56bab8f1a8254721ec9906bb6a4508dc22a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-data-lake-analytics-jobs-using-azure-portal"></a><span data-ttu-id="7b826-103">Azure portál használata az Azure Data Lake Analytics-feladatok hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="7b826-103">Troubleshoot Azure Data Lake Analytics jobs using Azure Portal</span></span>
<span data-ttu-id="7b826-104">Ismerje meg, hogyan toouse hello Azure Portal tootroubleshoot Data Lake Analytics-feladatok.</span><span class="sxs-lookup"><span data-stu-id="7b826-104">Learn how toouse hello Azure Portal tootroubleshoot Data Lake Analytics jobs.</span></span>

<span data-ttu-id="7b826-105">Ebben az oktatóanyagban meg fog beállítani egy hiányzó forrás fájl probléma, valamint hello Azure Portal tootroubleshoot hello probléma használja.</span><span class="sxs-lookup"><span data-stu-id="7b826-105">In this tutorial, you will setup a missing source file problem, and use hello Azure Portal tootroubleshoot hello problem.</span></span>

## <a name="submit-a-data-lake-analytics-job"></a><span data-ttu-id="7b826-106">Data Lake Analytics-feladat küldése</span><span class="sxs-lookup"><span data-stu-id="7b826-106">Submit a Data Lake Analytics job</span></span>

<span data-ttu-id="7b826-107">Küldje el a U-SQL-feladatot a következő hello:</span><span class="sxs-lookup"><span data-stu-id="7b826-107">Submit hello following U-SQL job:</span></span>

```
@searchlog =
   EXTRACT UserId          int,
           Start           DateTime,
           Region          string,
           Query           string,
           Duration        int?,
           Urls            string,
           ClickedUrls     string
   FROM "/Samples/Data/SearchLog.tsv1"
   USING Extractors.Tsv();

OUTPUT @searchlog   
   too"/output/SearchLog-from-adls.csv"
   USING Outputters.Csv();
```
    
<span data-ttu-id="7b826-108">hello forrásfájl hello parancsfájl definiálva van **/Samples/Data/SearchLog.tsv1**, ahol meg kell **/Samples/Data/SearchLog.tsv**.</span><span class="sxs-lookup"><span data-stu-id="7b826-108">hello source file defined in hello script is **/Samples/Data/SearchLog.tsv1**, where it should be **/Samples/Data/SearchLog.tsv**.</span></span>


## <a name="troubleshoot-hello-job"></a><span data-ttu-id="7b826-109">Hello-feladat hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="7b826-109">Troubleshoot hello job</span></span>

<span data-ttu-id="7b826-110">**toosee összes hello feladatok**</span><span class="sxs-lookup"><span data-stu-id="7b826-110">**toosee all hello jobs**</span></span>

1. <span data-ttu-id="7b826-111">A hello Azure-portálon, kattintson az **Microsoft Azure** hello bal felső sarokban található.</span><span class="sxs-lookup"><span data-stu-id="7b826-111">From hello Azure portal, click **Microsoft Azure** in hello upper left corner.</span></span>
2. <span data-ttu-id="7b826-112">Kattintson a hello csempe, amely a Data Lake Analytics-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="7b826-112">Click hello tile with your Data Lake Analytics account name.</span></span>  <span data-ttu-id="7b826-113">összegző hello feladat jelenik meg hello **feladatkezelés** csempére.</span><span class="sxs-lookup"><span data-stu-id="7b826-113">hello job summary is shown on hello **Job Management** tile.</span></span>

    ![Azure Data Lake Analytics-feladatok kezelése](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    <span data-ttu-id="7b826-115">hello feladat felügyeleti lehetővé teszi egy pillanat alatt a hello feladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="7b826-115">hello job Management gives you a glance of hello job status.</span></span> <span data-ttu-id="7b826-116">Figyelje meg, nincs a sikertelen feladatokat.</span><span class="sxs-lookup"><span data-stu-id="7b826-116">Notice there is a failed job.</span></span>
3. <span data-ttu-id="7b826-117">Kattintson a hello **feladatkezelés** toosee hello feladatok csempére.</span><span class="sxs-lookup"><span data-stu-id="7b826-117">Click hello **Job Management** tile toosee hello jobs.</span></span> <span data-ttu-id="7b826-118">hello feladat szerint vannak kategóriába sorolva **futtató**, **várakozik**, és **befejezve**.</span><span class="sxs-lookup"><span data-stu-id="7b826-118">hello jobs are categorized in **Running**, **Queued**, and **Ended**.</span></span> <span data-ttu-id="7b826-119">Ekkor megjelenik a sikertelen feladat a hello **befejezve** szakasz.</span><span class="sxs-lookup"><span data-stu-id="7b826-119">You shall see your failed job in hello **Ended** section.</span></span> <span data-ttu-id="7b826-120">Hello lista első tanúsítványt kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7b826-120">It shall be first one in hello list.</span></span> <span data-ttu-id="7b826-121">Ha nagy mennyiségű feladatok, rákattinthat **szűrő** toohelp toolocate feladatok meg.</span><span class="sxs-lookup"><span data-stu-id="7b826-121">When you have a lot of jobs, you can click **Filter** toohelp you toolocate jobs.</span></span>

    ![Azure Data Lake Analytics szűrheti a feladatokat](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. <span data-ttu-id="7b826-123">Kattintson a sikertelen feladatot hello hello lista tooopen hello feladat részleteit egy új panelen:</span><span class="sxs-lookup"><span data-stu-id="7b826-123">Click hello failed job from hello list tooopen hello job details in a new blade:</span></span>

    ![Az Azure Data Lake Analytics feladat sikertelen volt.](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    <span data-ttu-id="7b826-125">Értesítés hello **küldje el újra** gombra.</span><span class="sxs-lookup"><span data-stu-id="7b826-125">Notice hello **Resubmit** button.</span></span> <span data-ttu-id="7b826-126">Hello probléma megoldása után újraküldése a hello feladat.</span><span class="sxs-lookup"><span data-stu-id="7b826-126">After you fix hello problem, you can resubmit hello job.</span></span>
5. <span data-ttu-id="7b826-127">Kattintson a kijelölt részben hello előző képernyőkép tooopen hello hiba részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="7b826-127">Click highlighted part from hello previous screenshot tooopen hello error details.</span></span>  <span data-ttu-id="7b826-128">Látni fogja hasonlót:</span><span class="sxs-lookup"><span data-stu-id="7b826-128">You shall see something like:</span></span>

    ![Az Azure Data Lake Analytics nem sikerült a feladat részletei](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    <span data-ttu-id="7b826-130">Jelzi, hogy hello forrásmappa nem található.</span><span class="sxs-lookup"><span data-stu-id="7b826-130">It tells you hello source folder is not found.</span></span>
6. <span data-ttu-id="7b826-131">Kattintson a **parancsfájl ismétlődő**.</span><span class="sxs-lookup"><span data-stu-id="7b826-131">Click **Duplicate Script**.</span></span>
7. <span data-ttu-id="7b826-132">Frissítés hello **FROM** toohello következő elérési út:</span><span class="sxs-lookup"><span data-stu-id="7b826-132">Update hello **FROM** path toohello following:</span></span>

    <span data-ttu-id="7b826-133">"/ Samples/Data/SearchLog.tsv"</span><span class="sxs-lookup"><span data-stu-id="7b826-133">"/Samples/Data/SearchLog.tsv"</span></span>
8. <span data-ttu-id="7b826-134">Kattintson a **Feladat elküldése** elemre.</span><span class="sxs-lookup"><span data-stu-id="7b826-134">Click **Submit Job**.</span></span>

## <a name="see-also"></a><span data-ttu-id="7b826-135">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="7b826-135">See also</span></span>
* [<span data-ttu-id="7b826-136">Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="7b826-136">Azure Data Lake Analytics overview</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="7b826-137">Ismerkedés az Azure Data Lake Analytics Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7b826-137">Get started with Azure Data Lake Analytics using Azure PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="7b826-138">Ismerkedés az Azure Data Lake Analytics és a U-SQL Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="7b826-138">Get started with Azure Data Lake Analytics and U-SQL using Visual Studio</span></span>](data-lake-analytics-u-sql-get-started.md)
* [<span data-ttu-id="7b826-139">Az Azure Data Lake Analytics kezelése az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="7b826-139">Manage Azure Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-manage-use-portal.md)
