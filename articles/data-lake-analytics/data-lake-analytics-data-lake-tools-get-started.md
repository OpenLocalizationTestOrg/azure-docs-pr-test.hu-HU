---
title: "a Data Lake Tools for Visual Studio használatával aaaDevelop U-SQL-parancsfájlok |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall Data Lake Tools for Visual Studio, és hogyan toodevelop és tesztelési U-SQL-parancsfájlok."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/28/2017
ms.author: saveenr, yanacai
ms.openlocfilehash: 7a0c02c275b8cefecbe784ba63969cbf24a150d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a><span data-ttu-id="1d0c3-103">U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="1d0c3-103">Develop U-SQL scripts by using Data Lake Tools for Visual Studio</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


<span data-ttu-id="1d0c3-104">Megtudhatja, hogyan toouse Visual Studio toocreate Azure Data Lake Analytics fiókok-feladatok definiálásához [U-SQL](data-lake-analytics-u-sql-get-started.md), és küldje el a feladatok toohello Data Lake Analytics szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-104">Learn how toouse Visual Studio toocreate Azure Data Lake Analytics accounts, define jobs in [U-SQL](data-lake-analytics-u-sql-get-started.md), and submit jobs toohello Data Lake Analytics service.</span></span> <span data-ttu-id="1d0c3-105">További információk a Data Lake Analyticsről: [Azure Data Lake Analytics overview](data-lake-analytics-overview.md) (Az Azure Data Lake Analytics áttekintése).</span><span class="sxs-lookup"><span data-stu-id="1d0c3-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="1d0c3-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1d0c3-106">Prerequisites</span></span>

* <span data-ttu-id="1d0c3-107">**Visual Studio**: Az Express kivételével minden kiadás támogatott.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-107">**Visual Studio**: All editions except Express are supported.</span></span>
    * <span data-ttu-id="1d0c3-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1d0c3-108">Visual Studio 2017</span></span>
    * <span data-ttu-id="1d0c3-109">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="1d0c3-109">Visual Studio 2015</span></span>
    * <span data-ttu-id="1d0c3-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="1d0c3-110">Visual Studio 2013</span></span>
* <span data-ttu-id="1d0c3-111">**Microsoft Azure SDK for .NET** 2.7.1-es vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-111">**Microsoft Azure SDK for .NET** version 2.7.1 or later.</span></span>  <span data-ttu-id="1d0c3-112">Telepítheti azt a hello [webplatform-telepítő](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="1d0c3-112">Install it by using hello [Web platform installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="1d0c3-113">Egy **Data Lake Analytics**-fiók.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-113">A **Data Lake Analytics** account.</span></span> <span data-ttu-id="1d0c3-114">egy olyan fiók, toocreate lásd [Ismerkedés az Azure Data Lake Analytics az Azure portál használatával](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1d0c3-114">toocreate an account, see [Get Started with Azure Data Lake Analytics using Azure portal](data-lake-analytics-get-started-portal.md).</span></span>

## <a name="install-azure-data-lake-tools-for-visual-studio"></a><span data-ttu-id="1d0c3-115">Az Azure Data Lake Tools for Visual Studio telepítése</span><span class="sxs-lookup"><span data-stu-id="1d0c3-115">Install Azure Data Lake Tools for Visual Studio</span></span> 

<span data-ttu-id="1d0c3-116">Töltse le és telepítse az Azure Data Lake Tools for Visual Studio [a letöltőközpontból hello](http://aka.ms/adltoolsvs).</span><span class="sxs-lookup"><span data-stu-id="1d0c3-116">Download and install Azure Data Lake Tools for Visual Studio [from hello Download Center](http://aka.ms/adltoolsvs).</span></span> <span data-ttu-id="1d0c3-117">A telepítést követően vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="1d0c3-117">After installation, note that:</span></span>
* <span data-ttu-id="1d0c3-118">Hello **Server Explorer** > **Azure** csomópont tartalmaz egy **Data Lake Analytics** csomópont.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-118">hello **Server Explorer** > **Azure** node contains a **Data Lake Analytics** node.</span></span> 
* <span data-ttu-id="1d0c3-119">Hello **eszközök** menü tartalmaz egy **Data Lake** elemet.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-119">hello **Tools** menu has a **Data Lake** item.</span></span>

## <a name="connect-tooan-azure-data-lake-analytics-account"></a><span data-ttu-id="1d0c3-120">Csatlakozás tooan Azure Data Lake Analytics-fiók</span><span class="sxs-lookup"><span data-stu-id="1d0c3-120">Connect tooan Azure Data Lake Analytics account</span></span>

1. <span data-ttu-id="1d0c3-121">Nyissa meg a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-121">Open Visual Studio.</span></span>
2. <span data-ttu-id="1d0c3-122">Nyissa meg a Kiszolgálókezelőt a **Nézet** > **Kiszolgálókezelő** kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-122">Open Server Explorer by selecting **View** > **Server Explorer**.</span></span>
3. <span data-ttu-id="1d0c3-123">Kattintson a jobb gombbal az **Azure** elemre.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-123">Right-click **Azure**.</span></span> <span data-ttu-id="1d0c3-124">Válassza ki **csatlakozás Azure-előfizetés tooMicrosoft** és hello utasítások szerint.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-124">Then select **Connect tooMicrosoft Azure Subscription** and follow hello instructions.</span></span>
4. <span data-ttu-id="1d0c3-125">A Kiszolgálókezelőben válassza az **Azure** > **Data Lake Analytics** elemet.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-125">In Server Explorer, select **Azure** > **Data Lake Analytics**.</span></span> <span data-ttu-id="1d0c3-126">Ekkor megjelenik a Data Lake Analytics-fiókok listája.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-126">You see a list of your Data Lake Analytics accounts.</span></span>


## <a name="write-your-first-u-sql-script"></a><span data-ttu-id="1d0c3-127">Az első U-SQL parancsfájl megírása</span><span class="sxs-lookup"><span data-stu-id="1d0c3-127">Write your first U-SQL script</span></span>

<span data-ttu-id="1d0c3-128">a következő szöveg hello egy egyszerű U-SQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-128">hello following text is a simple U-SQL script.</span></span> <span data-ttu-id="1d0c3-129">Azt határozza meg, kisebb adatkészlet, mind az írás, amelyeket a dataset toohello alapértelmezett Data Lake Store-fájlként `/data.csv`.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-129">It defines a small dataset and writes that dataset toohello default Data Lake Store as a file called `/data.csv`.</span></span>

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

### <a name="submit-a-data-lake-analytics-job"></a><span data-ttu-id="1d0c3-130">Data Lake Analytics-feladat küldése</span><span class="sxs-lookup"><span data-stu-id="1d0c3-130">Submit a Data Lake Analytics job</span></span>

1. <span data-ttu-id="1d0c3-131">Válassza a **File** (Fájl) > **New** (Új) > **Project** (Projekt) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-131">Select **File** > **New** > **Project**.</span></span>

2. <span data-ttu-id="1d0c3-132">Jelölje be hello **U-SQL projekt** írja be, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-132">Select hello **U-SQL Project** type, and then click **OK**.</span></span> <span data-ttu-id="1d0c3-133">A Visual Studio létrehoz egy megoldást egy **Script.usql** fájllal.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-133">Visual Studio creates a solution with a **Script.usql** file.</span></span>

3. <span data-ttu-id="1d0c3-134">Hello előző parancsfájl beillesztése hello **Script.usql** ablak.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-134">Paste hello previous script into hello **Script.usql** window.</span></span>

4. <span data-ttu-id="1d0c3-135">Hello bal felső sarkában hello **Script.usql** ablakban adja meg a hello Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-135">In hello upper-left corner of hello **Script.usql** window, specify hello Data Lake Analytics account.</span></span>

    ![U-SQL Visual Studio-projekt elküldése](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. <span data-ttu-id="1d0c3-137">Hello bal felső sarkában hello **Script.usql** ablakban válassza ki **Submit**.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-137">In hello upper-left corner of hello **Script.usql** window, select **Submit**.</span></span>
6. <span data-ttu-id="1d0c3-138">Ellenőrizze a hello **Analytics-fiók**, majd válassza ki **Submit**.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-138">Verify hello **Analytics Account**, and then select **Submit**.</span></span> <span data-ttu-id="1d0c3-139">Után az eredmények érhetők el a Data Lake Tools for Visual Studio eredmények hello, hello küldésének befejezése után.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-139">Submission results are available in hello Data Lake Tools for Visual Studio Results after hello submission is complete.</span></span>

    ![U-SQL Visual Studio-projekt elküldése](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
7. <span data-ttu-id="1d0c3-141">toosee hello legújabb feladat állapotát és a frissítési üdvözlő képernyőt, kattintson a **frissítése**.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-141">toosee hello latest job status and refresh hello screen, click **Refresh**.</span></span> <span data-ttu-id="1d0c3-142">Hello feladat sikeres, mutat hello **Feladatgrafikon**, **metaadat-művelet**, **State History**, és **diagnosztika**:</span><span class="sxs-lookup"><span data-stu-id="1d0c3-142">When hello job succeeds, it shows hello **Job Graph**, **MetaData Operations**, **State History**, and **Diagnostics**:</span></span>

    ![U-SQL Visual Studio Data Lake Analytics-feladat teljesítménygrafikonja](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * <span data-ttu-id="1d0c3-144">**Feladat összegzése** mutat be hello hello feladat összegzése.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-144">**Job Summary** shows hello summary of hello job.</span></span>   
   * <span data-ttu-id="1d0c3-145">**Feladat részleteinek** hello feladat, többek között a hello parancsfájl, erőforrások és csúcsban konkrétabb információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-145">**Job Details** shows more specific information about hello job, including hello script, resources, and vertices.</span></span>
   * <span data-ttu-id="1d0c3-146">**A Feladatgrafikon** visualizes hello hello feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-146">**Job Graph** visualizes hello progress of hello job.</span></span>
   * <span data-ttu-id="1d0c3-147">**Metaadat-művelet** jeleníti meg a hello U-SQL catalog végrehajtott összes hello-műveleteket.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-147">**MetaData Operations** shows all hello actions that were taken on hello U-SQL catalog.</span></span>
   * <span data-ttu-id="1d0c3-148">**Adatok** összes hello bemenetekhez és kimenetekhez jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-148">**Data** shows all hello inputs and outputs.</span></span>
   * <span data-ttu-id="1d0c3-149">A **Diagnosztika** részletes elemzést nyújt a feladat végrehajtásához és a teljesítmény optimalizálásához.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-149">**Diagnostics** provides an advanced analysis for job execution and performance optimization.</span></span>

### <a name="toocheck-job-state"></a><span data-ttu-id="1d0c3-150">toocheck feladat állapota</span><span class="sxs-lookup"><span data-stu-id="1d0c3-150">toocheck job state</span></span>

1. <span data-ttu-id="1d0c3-151">A Kiszolgálókezelőben válassza az **Azure** > **Data Lake Analytics** elemet.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-151">In Server Explorer, select **Azure** > **Data Lake Analytics**.</span></span> 
2. <span data-ttu-id="1d0c3-152">Bontsa ki a hello Data Lake Analytics-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-152">Expand hello Data Lake Analytics account name.</span></span>
3. <span data-ttu-id="1d0c3-153">Kattintson duplán a **Feladatok** elemre.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-153">Double-click **Jobs**.</span></span>
4. <span data-ttu-id="1d0c3-154">Jelölje ki az előzőleg elküldött hello feladatot.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-154">Select hello job that you previously submitted.</span></span>

### <a name="toosee-hello-output-of-a-job"></a><span data-ttu-id="1d0c3-155">feladat kimenetének toosee hello</span><span class="sxs-lookup"><span data-stu-id="1d0c3-155">toosee hello output of a job</span></span>

1. <span data-ttu-id="1d0c3-156">A Server Explorer eszközben keresse meg az Ön által küldött toohello feladat.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-156">In Server Explorer, browse toohello job you submitted.</span></span>
2. <span data-ttu-id="1d0c3-157">Kattintson a hello **adatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-157">Click hello **Data** tab.</span></span>
3. <span data-ttu-id="1d0c3-158">A hello **feladatot kimenetek** lapra, jelölje be hello `"/data.csv"` fájlt.</span><span class="sxs-lookup"><span data-stu-id="1d0c3-158">In hello **Job Outputs** tab, select hello `"/data.csv"` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d0c3-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d0c3-159">Next steps</span></span>

* [<span data-ttu-id="1d0c3-160">U-SQL-szkript futtatása a munkaállomáson teszteléshez és hibakereséshez</span><span class="sxs-lookup"><span data-stu-id="1d0c3-160">Run U-SQL scripts on your own workstation for testing and debugging</span></span>](data-lake-analytics-data-lake-tools-local-run.md)
* [<span data-ttu-id="1d0c3-161">Hibakeresés a C#-kódban – U-SQL-feladatok</span><span class="sxs-lookup"><span data-stu-id="1d0c3-161">Debug C# code in U-SQL jobs</span></span>](data-lake-analytics-debug-u-sql-jobs.md)
* [<span data-ttu-id="1d0c3-162">A Visual Studio Code hello Azure Data Lake Tools használata</span><span class="sxs-lookup"><span data-stu-id="1d0c3-162">Use hello Azure Data Lake Tools for Visual Studio Code</span></span>](data-lake-analytics-data-lake-tools-for-vscode.md)
