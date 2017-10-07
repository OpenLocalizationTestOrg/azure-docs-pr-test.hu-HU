---
title: "aaaTest és hibakeresési U-SQL feladatok használatával helyi futtatása és hello Azure Data Lake U-SQL SDK |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Data Lake Tools for Visual Studio és hello Azure Data Lake U-SQL SDK tootest és hibakeresési U-SQL feladatok a helyi munkaállomáson."
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
ms.openlocfilehash: be04558a504acf6a088e207608ee2d4a011d3ffc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="test-and-debug-u-sql-jobs-by-using-local-run-and-hello-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="549a1-103">Tesztelése és hibakeresése a U-SQL feladatok segítségével helyi futtatásához, hello Azure Data Lake U-SQL-SDK</span><span class="sxs-lookup"><span data-stu-id="549a1-103">Test and debug U-SQL jobs by using local run and hello Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="549a1-104">Használhatja Azure Data Lake Tools for Visual Studio és hello Azure Data Lake U-SQL SDK toorun U-SQL feladatok a munkaállomáson, ugyanúgy, mint a hello Azure Data Lake szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="549a1-104">You can use Azure Data Lake Tools for Visual Studio and hello Azure Data Lake U-SQL SDK toorun U-SQL jobs on your workstation, just as you can in hello Azure Data Lake service.</span></span> <span data-ttu-id="549a1-105">Ez a két, helyi futtatású szolgáltatás időt takarít meg a U-SQL feladatok tesztelése és hibakeresése során.</span><span class="sxs-lookup"><span data-stu-id="549a1-105">These two local-run features save you time in testing and debugging your U-SQL jobs.</span></span>

## <a name="understand-hello-data-root-folder-and-hello-file-path"></a><span data-ttu-id="549a1-106">Hello adatok-gyökérmappa és hello fájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="549a1-106">Understand hello data-root folder and hello file path</span></span>

<span data-ttu-id="549a1-107">Helyi Futtatás és a U-SQL SDK hello szükséges adatok gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="549a1-107">Both local run and hello U-SQL SDK require a data-root folder.</span></span> <span data-ttu-id="549a1-108">hello adatok-gyökérmappája "helyi tároló" hello helyi számítási fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="549a1-108">hello data-root folder is a "local store" for hello local compute account.</span></span> <span data-ttu-id="549a1-109">Egyenértékű toohello Azure Data Lake Store-fiók egy Data Lake Analytics-fiók is.</span><span class="sxs-lookup"><span data-stu-id="549a1-109">It's equivalent toohello Azure Data Lake Store account of a Data Lake Analytics account.</span></span> <span data-ttu-id="549a1-110">Váltás tooa különböző adatok-gyökérmappája hasonlóan tooa különböző store-fiók vált.</span><span class="sxs-lookup"><span data-stu-id="549a1-110">Switching tooa different data-root folder is just like switching tooa different store account.</span></span> <span data-ttu-id="549a1-111">Ha azt szeretné, hogy tooaccess hagyományosan megosztott adatokat különböző adatgyökerében mappákkal, a parancsfájlok abszolút elérési utakat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="549a1-111">If you want tooaccess commonly shared data with different data-root folders, you must use absolute paths in your scripts.</span></span> <span data-ttu-id="549a1-112">Vagy hozzon létre fájlt rendszer szimbolikus hivatkozásokat (például **mklink** NTFS) hello adatgyökerében mappa toopoint toohello a megosztott adatok.</span><span class="sxs-lookup"><span data-stu-id="549a1-112">Or, create file system symbolic links (for example, **mklink** on NTFS) under hello data-root folder toopoint toohello shared data.</span></span>

<span data-ttu-id="549a1-113">hello adatgyökerében mappa a következőkre használható:</span><span class="sxs-lookup"><span data-stu-id="549a1-113">hello data-root folder is used to:</span></span>

- <span data-ttu-id="549a1-114">Tárolni a metaadatokat, például adatbázisok, táblák, táblaértékű függvények (TVFs) és szerelvényeket.</span><span class="sxs-lookup"><span data-stu-id="549a1-114">Store metadata, including databases, tables, table-valued functions (TVFs), and assemblies.</span></span>
- <span data-ttu-id="549a1-115">Hello bemeneti és kimeneti elérési utak relatív elérési utak a U-SQL definiált kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="549a1-115">Look up hello input and output paths that are defined as relative paths in U-SQL.</span></span> <span data-ttu-id="549a1-116">Relatív elérési utak révén könnyebben toodeploy a U-SQL-projektek tooAzure.</span><span class="sxs-lookup"><span data-stu-id="549a1-116">Using relative paths makes it easier toodeploy your U-SQL projects tooAzure.</span></span>

<span data-ttu-id="549a1-117">U-SQL-parancsfájlok használhatja relatív elérési út és a helyi abszolút elérési utat.</span><span class="sxs-lookup"><span data-stu-id="549a1-117">You can use both a relative path and a local absolute path in U-SQL scripts.</span></span> <span data-ttu-id="549a1-118">hello relatív elérési út relatív toohello megadott adatok-gyökérmappa elérési útja.</span><span class="sxs-lookup"><span data-stu-id="549a1-118">hello relative path is relative toohello specified data-root folder path.</span></span> <span data-ttu-id="549a1-119">Azt javasoljuk, hogy azt használja "/", hello elérési útját elválasztójel toomake hello kiszolgálóoldali kompatibilis a parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="549a1-119">We recommend that you use "/" as hello path separator toomake your scripts compatible with hello server side.</span></span> <span data-ttu-id="549a1-120">Az alábbiakban néhány olyan relatív elérési útja és az egyenértékű abszolút elérési utakat.</span><span class="sxs-lookup"><span data-stu-id="549a1-120">Here are some examples of relative paths and their equivalent absolute paths.</span></span> <span data-ttu-id="549a1-121">Ezekben a példákban C:\LocalRunDataRoot hello adatok-gyökérmappájába.</span><span class="sxs-lookup"><span data-stu-id="549a1-121">In these examples, C:\LocalRunDataRoot is hello data-root folder.</span></span>

|<span data-ttu-id="549a1-122">Relatív elérési útja</span><span class="sxs-lookup"><span data-stu-id="549a1-122">Relative path</span></span>|<span data-ttu-id="549a1-123">Abszolút elérési útja</span><span class="sxs-lookup"><span data-stu-id="549a1-123">Absolute path</span></span>|
|-------------|-------------|
|<span data-ttu-id="549a1-124">/ABC/DEF/input.csv</span><span class="sxs-lookup"><span data-stu-id="549a1-124">/abc/def/input.csv</span></span> |<span data-ttu-id="549a1-125">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="549a1-125">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="549a1-126">ABC/DEF/input.csv</span><span class="sxs-lookup"><span data-stu-id="549a1-126">abc/def/input.csv</span></span>  |<span data-ttu-id="549a1-127">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="549a1-127">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="549a1-128">D:/ABC/DEF/input.csv</span><span class="sxs-lookup"><span data-stu-id="549a1-128">D:/abc/def/input.csv</span></span> |<span data-ttu-id="549a1-129">D:\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="549a1-129">D:\abc\def\input.csv</span></span>|

## <a name="use-local-run-from-visual-studio"></a><span data-ttu-id="549a1-130">Futtassa a Visual Studio helyi használata</span><span class="sxs-lookup"><span data-stu-id="549a1-130">Use local run from Visual Studio</span></span>

<span data-ttu-id="549a1-131">A Data Lake Tools for Visual Studio a Visual Studio U-SQL helyi futtatási élményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="549a1-131">Data Lake Tools for Visual Studio provides a U-SQL local-run experience in Visual Studio.</span></span> <span data-ttu-id="549a1-132">Ez a funkció használatával a következő műveletek végezhetők el:</span><span class="sxs-lookup"><span data-stu-id="549a1-132">By using this feature, you can:</span></span>

- <span data-ttu-id="549a1-133">Helyileg, egy U-SQL-parancsfájl futtatása C#-szerelvények együtt.</span><span class="sxs-lookup"><span data-stu-id="549a1-133">Run a U-SQL script locally, along with C# assemblies.</span></span>
- <span data-ttu-id="549a1-134">A hibakeresési egy C# szerelvény helyileg.</span><span class="sxs-lookup"><span data-stu-id="549a1-134">Debug a C# assembly locally.</span></span>
- <span data-ttu-id="549a1-135">Létrehozása, megtekintése és a Server Explorer U-SQL katalógusok (helyi adatbázisokat, szerelvényeket, sémákat és táblák) törlése.</span><span class="sxs-lookup"><span data-stu-id="549a1-135">Create, view, and delete U-SQL catalogs (local databases, assemblies, schemas, and tables) from Server Explorer.</span></span> <span data-ttu-id="549a1-136">Hello helyi katalógus is is tájékozódhat a Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="549a1-136">You can also find hello local catalog also from Server Explorer.</span></span>

    ![A Data Lake Tools for Visual Studio helyi futtatási helyi katalógus](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-local-catalog.png)

<span data-ttu-id="549a1-138">hello Data Lake Tools installer egy C:\LocalRunRoot mappa toobe használja hello alapértelmezett adatok hoz.</span><span class="sxs-lookup"><span data-stu-id="549a1-138">hello Data Lake Tools installer creates a C:\LocalRunRoot folder toobe used as hello default data-root folder.</span></span> <span data-ttu-id="549a1-139">hello alapértelmezett helyi futtatási párhuzamossági: 1.</span><span class="sxs-lookup"><span data-stu-id="549a1-139">hello default local-run parallelism is 1.</span></span>

### <a name="tooconfigure-local-run-in-visual-studio"></a><span data-ttu-id="549a1-140">Futtassa a Visual Studio helyi tooconfigure</span><span class="sxs-lookup"><span data-stu-id="549a1-140">tooconfigure local run in Visual Studio</span></span>

1. <span data-ttu-id="549a1-141">Nyissa meg a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="549a1-141">Open Visual Studio.</span></span>
2. <span data-ttu-id="549a1-142">Nyissa meg **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="549a1-142">Open **Server Explorer**.</span></span>
3. <span data-ttu-id="549a1-143">Bontsa ki a **Azure** > **a Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="549a1-143">Expand **Azure** > **Data Lake Analytics**.</span></span>
4. <span data-ttu-id="549a1-144">Kattintson a hello **Data Lake** menüben, majd kattintson **lehetőségek és beállítások**.</span><span class="sxs-lookup"><span data-stu-id="549a1-144">Click hello **Data Lake** menu, and then click **Options and Settings**.</span></span>
5. <span data-ttu-id="549a1-145">A hello bal konzolfáján bontsa ki a **Azure Data Lake**, majd bontsa ki a **általános**.</span><span class="sxs-lookup"><span data-stu-id="549a1-145">In hello left tree, expand **Azure Data Lake**, and then expand **General**.</span></span>

    ![A Data Lake Tools for Visual Studio helyi futtatási beállítások konfigurálása](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-configure.png)

<span data-ttu-id="549a1-147">A Visual Studio U-SQL projekt végrehajtása helyi futtatásához szükség.</span><span class="sxs-lookup"><span data-stu-id="549a1-147">A Visual Studio U-SQL project is required for performing local run.</span></span> <span data-ttu-id="549a1-148">Ez a kijelző nem azonos a U-SQL-parancsfájlok futtatásakor, az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="549a1-148">This part is different from running U-SQL scripts from Azure.</span></span>

### <a name="toorun-a-u-sql-script-locally"></a><span data-ttu-id="549a1-149">helyileg toorun egy U-SQL-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="549a1-149">toorun a U-SQL script locally</span></span>
1. <span data-ttu-id="549a1-150">A Visual Studio eszközből a U-SQL projekt megnyitása.</span><span class="sxs-lookup"><span data-stu-id="549a1-150">From Visual Studio, open your U-SQL project.</span></span>   
2. <span data-ttu-id="549a1-151">Kattintson a jobb gombbal a Solution Explorer U-SQL parancsfájl, és kattintson **parancsfájl elküldése**.</span><span class="sxs-lookup"><span data-stu-id="549a1-151">Right-click a U-SQL script in Solution Explorer, and then click **Submit Script**.</span></span>
3. <span data-ttu-id="549a1-152">Válassza ki **(helyi)** hello Analytics fiók toorun helyileg a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="549a1-152">Select **(Local)** as hello Analytics account toorun your script locally.</span></span>
<span data-ttu-id="549a1-153">Kattintson a hello **(helyi)** parancsfájl ablak tetején hello fiókot, és kattintson a **küldje el a következőt** (vagy hello Ctrl + F5 billentyűkombináció).</span><span class="sxs-lookup"><span data-stu-id="549a1-153">You can also click hello **(Local)** account on hello top of script window, and then click **Submit** (or use hello Ctrl + F5 keyboard shortcut).</span></span>

    ![Data Lake Tools for Visual Studio helyi futtatási küldés feladatok](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-submit-job.png)

### <a name="debug-scripts-and-c-assemblies-locally"></a><span data-ttu-id="549a1-155">Parancsfájlok és C#-szerelvények helyi hibakeresése</span><span class="sxs-lookup"><span data-stu-id="549a1-155">Debug scripts and C# assemblies locally</span></span>

<span data-ttu-id="549a1-156">A hibakeresési C#-szerelvények elküldené és regisztrálná őket tooAzure Data Lake Analytics szolgáltatás nélkül.</span><span class="sxs-lookup"><span data-stu-id="549a1-156">You can debug C# assemblies without submitting and registering it tooAzure Data Lake Analytics Service.</span></span> <span data-ttu-id="549a1-157">Töréspontokat állíthat mindkét hello fájl mögötti kódban és a hivatkozott C#-projektben.</span><span class="sxs-lookup"><span data-stu-id="549a1-157">You can set breakpoints in both hello code behind file and in a referenced C# project.</span></span>

#### <a name="toodebug-local-code-in-code-behind-file"></a><span data-ttu-id="549a1-158">helyi kódok toodebug fájl mögötti kódban</span><span class="sxs-lookup"><span data-stu-id="549a1-158">toodebug local code in code behind file</span></span>

1. <span data-ttu-id="549a1-159">Állítson be töréspontokat a fájl mögötti kódban hello.</span><span class="sxs-lookup"><span data-stu-id="549a1-159">Set breakpoints in hello code behind file.</span></span>
2. <span data-ttu-id="549a1-160">Nyomja le az F5 toodebug hello parancsfájl helyi.</span><span class="sxs-lookup"><span data-stu-id="549a1-160">Press F5 toodebug hello script locally.</span></span>

> [!NOTE]
   > <span data-ttu-id="549a1-161">a következő eljárást csak akkor működik a Visual Studio 2015 hello.</span><span class="sxs-lookup"><span data-stu-id="549a1-161">hello following procedure only works in Visual Studio 2015.</span></span> <span data-ttu-id="549a1-162">A régebbi kiadásokban esetleg toomanually hozzáadása hello pdb-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="549a1-162">In older Visual Studio you may need toomanually add hello pdb files.</span></span>  
   >
   >

#### <a name="toodebug-local-code-in-a-referenced-c-project"></a><span data-ttu-id="549a1-163">a hivatkozott C# projekt helyi kód toodebug</span><span class="sxs-lookup"><span data-stu-id="549a1-163">toodebug local code in a referenced C# project</span></span>

1. <span data-ttu-id="549a1-164">Hozzon létre egy C#-szerelvényprojektet, és állítsa be úgy toogenerate hello kimeneti dll-fájlt.</span><span class="sxs-lookup"><span data-stu-id="549a1-164">Create a C# Assembly project, and build it toogenerate hello output dll.</span></span>
2. <span data-ttu-id="549a1-165">Regisztrálja a hello dll-fájlt egy U-SQL utasítás használatával:</span><span class="sxs-lookup"><span data-stu-id="549a1-165">Register hello dll using a U-SQL statement:</span></span>

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. <span data-ttu-id="549a1-166">Állítson be töréspontokat a C#-kódban hello.</span><span class="sxs-lookup"><span data-stu-id="549a1-166">Set breakpoints in hello C# code.</span></span>
4. <span data-ttu-id="549a1-167">Nyomja le az F5 toodebug hello parancsprogram helyileg hello C#-dll hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="549a1-167">Press F5 toodebug hello script with referencing hello C# dll locally.</span></span>

## <a name="use-local-run-from-hello-data-lake-u-sql-sdk"></a><span data-ttu-id="549a1-168">Használjon helyi hello Data Lake U-SQL-SDK-ről futtatva</span><span class="sxs-lookup"><span data-stu-id="549a1-168">Use local run from hello Data Lake U-SQL SDK</span></span>

<span data-ttu-id="549a1-169">Továbbá a toorunning U-SQL Visual Studio használatával helyileg parancsfájlok, használja hello Azure Data Lake U-SQL SDK toorun U-SQL-parancsfájlok helyi a parancssori és alkalmazásprogramozási felülethez.</span><span class="sxs-lookup"><span data-stu-id="549a1-169">In addition toorunning U-SQL scripts locally by using Visual Studio, you can use hello Azure Data Lake U-SQL SDK toorun U-SQL scripts locally with command-line and programming interfaces.</span></span> <span data-ttu-id="549a1-170">A fenti méretezheti a U-SQL helyi tesztelése.</span><span class="sxs-lookup"><span data-stu-id="549a1-170">Through these, you can scale your U-SQL local test.</span></span>

<span data-ttu-id="549a1-171">További információ [Azure Data Lake U-SQL SDK](data-lake-analytics-u-sql-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="549a1-171">Learn more about [Azure Data Lake U-SQL SDK](data-lake-analytics-u-sql-sdk.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="549a1-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="549a1-172">Next steps</span></span>

* <span data-ttu-id="549a1-173">egy összetettebb lekérdezés toosee lásd [Azure Data Lake Analytics használatával webhelyek naplóinak elemzése](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="549a1-173">toosee a more complex query, see [Analyze website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="549a1-174">feladat részletei tooview, lásd: [használata feladat böngésző és az Azure Data Lake Analytics-feladatok feladat megtekintése](data-lake-analytics-data-lake-tools-view-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="549a1-174">tooview job details, see [Use Job Browser and Job View for Azure Data Lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md).</span></span>
* <span data-ttu-id="549a1-175">toouse hello vertex végrehajtási nézetet, lásd: [használata hello Vertex végrehajtási nézetet a Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span><span class="sxs-lookup"><span data-stu-id="549a1-175">toouse hello vertex execution view, see [Use hello Vertex Execution View in Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span></span>
