---
title: "Tesztelése és hibakeresése a U-SQL feladatok helyi futtatáskor és az Azure Data Lake U-SQL SDK segítségével |} Microsoft Docs"
description: "Ismerje meg, hogyan használható az Azure Data Lake Tools for Visual Studio és az Azure Data Lake U-SQL SDK tesztelése és hibakeresése a U-SQL feladatok a helyi munkaállomáson."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/15/2016
ms.author: yanacai
ms.openlocfilehash: 771a96df5cc66bac46e7144785be8cc072b57b31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="test-and-debug-u-sql-jobs-by-using-local-run-and-the-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="aef04-103">Tesztelése és hibakeresése a U-SQL feladatok használatával helyi futtatásához és az Azure Data Lake U-SQL-SDK</span><span class="sxs-lookup"><span data-stu-id="aef04-103">Test and debug U-SQL jobs by using local run and the Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="aef04-104">Használhatja az Azure Data Lake Tools for Visual Studio és az Azure Data Lake U-SQL SDK szolgáltatásokat a U-SQL-feladatok munkaállomáson való futtatására, pontosan úgy, ahogyan azt az Azure Data Lake szolgáltatásban is megteheti.</span><span class="sxs-lookup"><span data-stu-id="aef04-104">You can use Azure Data Lake Tools for Visual Studio and the Azure Data Lake U-SQL SDK to run U-SQL jobs on your workstation, just as you can in the Azure Data Lake service.</span></span> <span data-ttu-id="aef04-105">Ez a két, helyi futtatású szolgáltatás időt takarít meg a U-SQL feladatok tesztelése és hibakeresése során.</span><span class="sxs-lookup"><span data-stu-id="aef04-105">These two local-run features save you time in testing and debugging your U-SQL jobs.</span></span>

## <a name="understand-the-data-root-folder-and-the-file-path"></a><span data-ttu-id="aef04-106">Az adatok gyökérmappa és a fájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="aef04-106">Understand the data-root folder and the file path</span></span>

<span data-ttu-id="aef04-107">Helyi futtatás, mind a U-SQL SDK szükséges adatok gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="aef04-107">Both local run and the U-SQL SDK require a data-root folder.</span></span> <span data-ttu-id="aef04-108">Az adatok-gyökérmappája "helyi tároló" a helyi számítási fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="aef04-108">The data-root folder is a "local store" for the local compute account.</span></span> <span data-ttu-id="aef04-109">Megegyezik a Data Lake Analytics-fiók az Azure Data Lake Store-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="aef04-109">It's equivalent to the Azure Data Lake Store account of a Data Lake Analytics account.</span></span> <span data-ttu-id="aef04-110">Váltás másik adatok-gyökérmappának az csakúgy, mint egy másik store-fiók vált.</span><span class="sxs-lookup"><span data-stu-id="aef04-110">Switching to a different data-root folder is just like switching to a different store account.</span></span> <span data-ttu-id="aef04-111">Ha azt szeretné, más adatok legfelső szintű mappák általában megosztott adatokhoz való hozzáférést, abszolút elérési utakat kell használnia a parancsfájlban.</span><span class="sxs-lookup"><span data-stu-id="aef04-111">If you want to access commonly shared data with different data-root folders, you must use absolute paths in your scripts.</span></span> <span data-ttu-id="aef04-112">Vagy hozzon létre fájlt rendszer szimbolikus hivatkozásokat (például **mklink** NTFS) a megosztott adatok az adatok-gyökere alatt.</span><span class="sxs-lookup"><span data-stu-id="aef04-112">Or, create file system symbolic links (for example, **mklink** on NTFS) under the data-root folder to point to the shared data.</span></span>

<span data-ttu-id="aef04-113">Az adatok gyökérmappa a következőkre használható:</span><span class="sxs-lookup"><span data-stu-id="aef04-113">The data-root folder is used to:</span></span>

- <span data-ttu-id="aef04-114">Tárolni a metaadatokat, például adatbázisok, táblák, táblaértékű függvények (TVFs) és szerelvényeket.</span><span class="sxs-lookup"><span data-stu-id="aef04-114">Store metadata, including databases, tables, table-valued functions (TVFs), and assemblies.</span></span>
- <span data-ttu-id="aef04-115">Keresse meg a bemeneti és kimeneti elérési utak relatív elérési utak a U-SQL is meg van adva.</span><span class="sxs-lookup"><span data-stu-id="aef04-115">Look up the input and output paths that are defined as relative paths in U-SQL.</span></span> <span data-ttu-id="aef04-116">Relatív elérési utak használata megkönnyíti a U-SQL projekt telepítése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="aef04-116">Using relative paths makes it easier to deploy your U-SQL projects to Azure.</span></span>

<span data-ttu-id="aef04-117">U-SQL-parancsfájlok használhatja relatív elérési út és a helyi abszolút elérési utat.</span><span class="sxs-lookup"><span data-stu-id="aef04-117">You can use both a relative path and a local absolute path in U-SQL scripts.</span></span> <span data-ttu-id="aef04-118">A relatív elérési út a megadott adatok-gyökérmappa elérési útja viszonyítva.</span><span class="sxs-lookup"><span data-stu-id="aef04-118">The relative path is relative to the specified data-root folder path.</span></span> <span data-ttu-id="aef04-119">Azt javasoljuk, hogy használjon "/" az elérési út elválasztó annak a parancsfájlokat a kiszolgálóoldali kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="aef04-119">We recommend that you use "/" as the path separator to make your scripts compatible with the server side.</span></span> <span data-ttu-id="aef04-120">Az alábbiakban néhány olyan relatív elérési útja és az egyenértékű abszolút elérési utakat.</span><span class="sxs-lookup"><span data-stu-id="aef04-120">Here are some examples of relative paths and their equivalent absolute paths.</span></span> <span data-ttu-id="aef04-121">Ezekben a példákban C:\LocalRunDataRoot az adatok-gyökérmappájába.</span><span class="sxs-lookup"><span data-stu-id="aef04-121">In these examples, C:\LocalRunDataRoot is the data-root folder.</span></span>

|<span data-ttu-id="aef04-122">Relatív elérési útja</span><span class="sxs-lookup"><span data-stu-id="aef04-122">Relative path</span></span>|<span data-ttu-id="aef04-123">Abszolút elérési útja</span><span class="sxs-lookup"><span data-stu-id="aef04-123">Absolute path</span></span>|
|-------------|-------------|
|<span data-ttu-id="aef04-124">/ABC/DEF/input.csv</span><span class="sxs-lookup"><span data-stu-id="aef04-124">/abc/def/input.csv</span></span> |<span data-ttu-id="aef04-125">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="aef04-125">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="aef04-126">ABC/DEF/input.csv</span><span class="sxs-lookup"><span data-stu-id="aef04-126">abc/def/input.csv</span></span>  |<span data-ttu-id="aef04-127">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="aef04-127">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="aef04-128">D:/ABC/DEF/input.csv</span><span class="sxs-lookup"><span data-stu-id="aef04-128">D:/abc/def/input.csv</span></span> |<span data-ttu-id="aef04-129">D:\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="aef04-129">D:\abc\def\input.csv</span></span>|

## <a name="use-local-run-from-visual-studio"></a><span data-ttu-id="aef04-130">Futtassa a Visual Studio helyi használata</span><span class="sxs-lookup"><span data-stu-id="aef04-130">Use local run from Visual Studio</span></span>

<span data-ttu-id="aef04-131">A Data Lake Tools for Visual Studio a Visual Studio U-SQL helyi futtatási élményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="aef04-131">Data Lake Tools for Visual Studio provides a U-SQL local-run experience in Visual Studio.</span></span> <span data-ttu-id="aef04-132">Ez a funkció használatával a következő műveletek végezhetők el:</span><span class="sxs-lookup"><span data-stu-id="aef04-132">By using this feature, you can:</span></span>

- <span data-ttu-id="aef04-133">Helyileg, egy U-SQL-parancsfájl futtatása C#-szerelvények együtt.</span><span class="sxs-lookup"><span data-stu-id="aef04-133">Run a U-SQL script locally, along with C# assemblies.</span></span>
- <span data-ttu-id="aef04-134">A hibakeresési egy C# szerelvény helyileg.</span><span class="sxs-lookup"><span data-stu-id="aef04-134">Debug a C# assembly locally.</span></span>
- <span data-ttu-id="aef04-135">Létrehozása, megtekintése és a Server Explorer U-SQL katalógusok (helyi adatbázisokat, szerelvényeket, sémákat és táblák) törlése.</span><span class="sxs-lookup"><span data-stu-id="aef04-135">Create, view, and delete U-SQL catalogs (local databases, assemblies, schemas, and tables) from Server Explorer.</span></span> <span data-ttu-id="aef04-136">A helyi katalógus is is tájékozódhat a Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="aef04-136">You can also find the local catalog also from Server Explorer.</span></span>

    ![A Data Lake Tools for Visual Studio helyi futtatási helyi katalógus](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-local-catalog.png)

<span data-ttu-id="aef04-138">A Data Lake Tools telepítő mappát hoz létre C:\LocalRunRoot használandó alapértelmezett adatok-gyökérmappájába.</span><span class="sxs-lookup"><span data-stu-id="aef04-138">The Data Lake Tools installer creates a C:\LocalRunRoot folder to be used as the default data-root folder.</span></span> <span data-ttu-id="aef04-139">Az alapértelmezett helyi futtatási párhuzamossági: 1.</span><span class="sxs-lookup"><span data-stu-id="aef04-139">The default local-run parallelism is 1.</span></span>

### <a name="to-configure-local-run-in-visual-studio"></a><span data-ttu-id="aef04-140">A Visual Studio helyi Futtatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="aef04-140">To configure local run in Visual Studio</span></span>

1. <span data-ttu-id="aef04-141">Nyissa meg a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="aef04-141">Open Visual Studio.</span></span>
2. <span data-ttu-id="aef04-142">Nyissa meg **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="aef04-142">Open **Server Explorer**.</span></span>
3. <span data-ttu-id="aef04-143">Bontsa ki a **Azure** > **a Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="aef04-143">Expand **Azure** > **Data Lake Analytics**.</span></span>
4. <span data-ttu-id="aef04-144">Kattintson a **Data Lake** menüben, majd kattintson **lehetőségek és beállítások**.</span><span class="sxs-lookup"><span data-stu-id="aef04-144">Click the **Data Lake** menu, and then click **Options and Settings**.</span></span>
5. <span data-ttu-id="aef04-145">A bal oldali fában bontsa ki a **Azure Data Lake**, majd bontsa ki a **általános**.</span><span class="sxs-lookup"><span data-stu-id="aef04-145">In the left tree, expand **Azure Data Lake**, and then expand **General**.</span></span>

    ![A Data Lake Tools for Visual Studio helyi futtatási beállítások konfigurálása](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-configure.png)

<span data-ttu-id="aef04-147">A Visual Studio U-SQL projekt végrehajtása helyi futtatásához szükség.</span><span class="sxs-lookup"><span data-stu-id="aef04-147">A Visual Studio U-SQL project is required for performing local run.</span></span> <span data-ttu-id="aef04-148">Ez a kijelző nem azonos a U-SQL-parancsfájlok futtatásakor, az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="aef04-148">This part is different from running U-SQL scripts from Azure.</span></span>

### <a name="to-run-a-u-sql-script-locally"></a><span data-ttu-id="aef04-149">U-SQL parancsfájl futtatása helyben</span><span class="sxs-lookup"><span data-stu-id="aef04-149">To run a U-SQL script locally</span></span>
1. <span data-ttu-id="aef04-150">A Visual Studio eszközből a U-SQL projekt megnyitása.</span><span class="sxs-lookup"><span data-stu-id="aef04-150">From Visual Studio, open your U-SQL project.</span></span>   
2. <span data-ttu-id="aef04-151">Kattintson a jobb gombbal a Solution Explorer U-SQL parancsfájl, és kattintson **parancsfájl elküldése**.</span><span class="sxs-lookup"><span data-stu-id="aef04-151">Right-click a U-SQL script in Solution Explorer, and then click **Submit Script**.</span></span>
3. <span data-ttu-id="aef04-152">Válassza ki **(helyi)** a Analytics-fiók helyileg futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="aef04-152">Select **(Local)** as the Analytics account to run your script locally.</span></span>
<span data-ttu-id="aef04-153">Is kattinthat a **(helyi)** parancsfájl ablak tetején fiókra, majd **Submit** (vagy használja a Ctrl + F5 billentyűparancsot).</span><span class="sxs-lookup"><span data-stu-id="aef04-153">You can also click the **(Local)** account on the top of script window, and then click **Submit** (or use the Ctrl + F5 keyboard shortcut).</span></span>

    ![Data Lake Tools for Visual Studio helyi futtatási küldés feladatok](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-submit-job.png)

### <a name="debug-scripts-and-c-assemblies-locally"></a><span data-ttu-id="aef04-155">Parancsfájlok és C#-szerelvények helyi hibakeresése</span><span class="sxs-lookup"><span data-stu-id="aef04-155">Debug scripts and C# assemblies locally</span></span>

<span data-ttu-id="aef04-156">A hibakeresési C#-szerelvények elküldené és regisztrálná őket az Azure Data Lake Analytics szolgáltatás nélkül.</span><span class="sxs-lookup"><span data-stu-id="aef04-156">You can debug C# assemblies without submitting and registering it to Azure Data Lake Analytics Service.</span></span> <span data-ttu-id="aef04-157">Töréspontokat állíthat be a fájl mögötti kódban és a hivatkozott C#-projektben is.</span><span class="sxs-lookup"><span data-stu-id="aef04-157">You can set breakpoints in both the code behind file and in a referenced C# project.</span></span>

#### <a name="to-debug-local-code-in-code-behind-file"></a><span data-ttu-id="aef04-158">Helyi kódok hibakeresése fájl mögötti kódban</span><span class="sxs-lookup"><span data-stu-id="aef04-158">To debug local code in code behind file</span></span>

1. <span data-ttu-id="aef04-159">Állítson be töréspontokat a fájl mögötti kódban.</span><span class="sxs-lookup"><span data-stu-id="aef04-159">Set breakpoints in the code behind file.</span></span>
2. <span data-ttu-id="aef04-160">Nyomja le az F5 billentyűt a parancsfájl helyi hibakereséséhez.</span><span class="sxs-lookup"><span data-stu-id="aef04-160">Press F5 to debug the script locally.</span></span>

> [!NOTE]
   > <span data-ttu-id="aef04-161">A következő eljárás csak a Visual Studio 2015 esetében működik.</span><span class="sxs-lookup"><span data-stu-id="aef04-161">The following procedure only works in Visual Studio 2015.</span></span> <span data-ttu-id="aef04-162">A régebbi kiadásokban lehetséges, hogy kézzel kell megadnia a pdb-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="aef04-162">In older Visual Studio you may need to manually add the pdb files.</span></span>  
   >
   >

#### <a name="to-debug-local-code-in-a-referenced-c-project"></a><span data-ttu-id="aef04-163">Helyi kódok hibakeresése egy hivatkozott C#-projektben</span><span class="sxs-lookup"><span data-stu-id="aef04-163">To debug local code in a referenced C# project</span></span>

1. <span data-ttu-id="aef04-164">Hozzon létre egy C#-szerelvényprojektet, és állítsa be úgy, hogy hozza létre a kimeneti dll-fájlt.</span><span class="sxs-lookup"><span data-stu-id="aef04-164">Create a C# Assembly project, and build it to generate the output dll.</span></span>
2. <span data-ttu-id="aef04-165">Regisztrálja a dll-fájlt egy U-SQL-kivonat használatával:</span><span class="sxs-lookup"><span data-stu-id="aef04-165">Register the dll using a U-SQL statement:</span></span>

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. <span data-ttu-id="aef04-166">Állítson be töréspontokat a C#-kódban.</span><span class="sxs-lookup"><span data-stu-id="aef04-166">Set breakpoints in the C# code.</span></span>
4. <span data-ttu-id="aef04-167">Nyomja le az F5 billentyűt a C#-DLL-helyileg fájlra hivatkozó parancsfájl hibakereséséhez.</span><span class="sxs-lookup"><span data-stu-id="aef04-167">Press F5 to debug the script with referencing the C# dll locally.</span></span>

## <a name="use-local-run-from-the-data-lake-u-sql-sdk"></a><span data-ttu-id="aef04-168">Használata helyi futtatáskor a Data Lake U-SQL-SDK-ból</span><span class="sxs-lookup"><span data-stu-id="aef04-168">Use local run from the Data Lake U-SQL SDK</span></span>

<span data-ttu-id="aef04-169">U-SQL-parancsfájlok futtatása helyben a Visual Studio használatával, valamint az Azure Data Lake U-SQL SDK használhatja U-SQL-parancsfájlok helyi futtatásához a parancssori és alkalmazásprogramozási felülethez.</span><span class="sxs-lookup"><span data-stu-id="aef04-169">In addition to running U-SQL scripts locally by using Visual Studio, you can use the Azure Data Lake U-SQL SDK to run U-SQL scripts locally with command-line and programming interfaces.</span></span> <span data-ttu-id="aef04-170">A fenti méretezheti a U-SQL helyi tesztelése.</span><span class="sxs-lookup"><span data-stu-id="aef04-170">Through these, you can scale your U-SQL local test.</span></span>

<span data-ttu-id="aef04-171">További információ [Azure Data Lake U-SQL SDK](data-lake-analytics-u-sql-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="aef04-171">Learn more about [Azure Data Lake U-SQL SDK](data-lake-analytics-u-sql-sdk.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="aef04-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aef04-172">Next steps</span></span>

* <span data-ttu-id="aef04-173">Egy összetettebb lekérdezés megtekintéséhez lásd: [Azure Data Lake Analytics használatával webhelyek naplóinak elemzése](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="aef04-173">To see a more complex query, see [Analyze website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="aef04-174">Feladat részleteinek megtekintése: [használata feladat böngésző és az Azure Data Lake Analytics-feladatok feladat megtekintése](data-lake-analytics-data-lake-tools-view-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="aef04-174">To view job details, see [Use Job Browser and Job View for Azure Data Lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md).</span></span>
* <span data-ttu-id="aef04-175">Ha szeretné használni a vertex végrehajtási nézetet, lásd [a Vertex végrehajtási nézetet használja a Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span><span class="sxs-lookup"><span data-stu-id="aef04-175">To use the vertex execution view, see [Use the Vertex Execution View in Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span></span>
