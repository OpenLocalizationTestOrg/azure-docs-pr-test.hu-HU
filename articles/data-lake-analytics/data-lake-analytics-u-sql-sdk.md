---
title: "Skála U-SQL helyi futtatása és a teszt Azure Data Lake U-SQL-SDK-val |} Microsoft Docs"
description: "Ismerje meg, hogyan használható az Azure Data Lake U-SQL SDK méretezési U-SQL feladatok helyi futtatása és a vizsgálat a parancssor és a helyi munkaállomáson alkalmazásprogramozási felületek."
services: data-lake-analytics
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: yanacai
ms.openlocfilehash: 55242bcf644ca0e7f30cfe7eada2130451c36e64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="scale-u-sql-local-run-and-test-with-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="c3c01-103">Skála U-SQL helyi futtatása és a teszt Azure Data Lake U-SQL-SDK-val</span><span class="sxs-lookup"><span data-stu-id="c3c01-103">Scale U-SQL local run and test with Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="c3c01-104">U-SQL parancsfájl fejlesztésekor közös futtatásához és tesztelhet U-SQL parancsfájl helyi előtt küldje el a felhőben.</span><span class="sxs-lookup"><span data-stu-id="c3c01-104">When developing U-SQL script, it is common to run and test U-SQL script locally before submit it to cloud.</span></span> <span data-ttu-id="c3c01-105">Azure Data Lake biztosít az Azure Data Lake U-SQL SDK hívása ebben a forgatókönyvben a Nuget-csomagot, keresztül, amely könnyedén méretezhető U-SQL helyi futtatásával és a vizsgálat.</span><span class="sxs-lookup"><span data-stu-id="c3c01-105">Azure Data Lake provides a Nuget package called Azure Data Lake U-SQL SDK for this scenario, through which you can easily scale U-SQL local run and test.</span></span> <span data-ttu-id="c3c01-106">Akkor is a U-SQL teszt integrálása CI (folyamatos integrációt) rendszer automatizálhatja a fordítási és teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c3c01-106">It is also possible to integrate this U-SQL test with CI (Continuous Integration) system to automate the compile and test.</span></span>

<span data-ttu-id="c3c01-107">Ha az Ön számára legfontosabb hogyan manuálisan a helyi Futtatás és hibakeresési grafikus felhasználói Felülettel tooling U-SQL parancsfájlt, majd használható Azure Data Lake Tools for Visual Studio, amely.</span><span class="sxs-lookup"><span data-stu-id="c3c01-107">If you care about how to manually local run and debug U-SQL script with GUI tooling, then you can use Azure Data Lake Tools for Visual Studio for that.</span></span> <span data-ttu-id="c3c01-108">Többet is megtudhat [Itt](data-lake-analytics-data-lake-tools-local-run.md).</span><span class="sxs-lookup"><span data-stu-id="c3c01-108">You can learn more from [here](data-lake-analytics-data-lake-tools-local-run.md).</span></span>

## <a name="install-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="c3c01-109">Telepítés Azure Data Lake U-SQL-SDK</span><span class="sxs-lookup"><span data-stu-id="c3c01-109">Install Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="c3c01-110">Az Azure Data Lake U-SQL SDK kaphat [Itt](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) a Nuget.org.</span><span class="sxs-lookup"><span data-stu-id="c3c01-110">You can get the Azure Data Lake U-SQL SDK [here](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) on Nuget.org.</span></span> <span data-ttu-id="c3c01-111">És annak használata előtt győződjön meg arról, hogy a függőségek az alábbiak szerint van szükség.</span><span class="sxs-lookup"><span data-stu-id="c3c01-111">And before using it, you need to make sure you have dependencies as follows.</span></span>

### <a name="dependencies"></a><span data-ttu-id="c3c01-112">Függőségek</span><span class="sxs-lookup"><span data-stu-id="c3c01-112">Dependencies</span></span>

<span data-ttu-id="c3c01-113">A Data Lake U-SQL SDK számára szükséges a következő függőségek:</span><span class="sxs-lookup"><span data-stu-id="c3c01-113">The Data Lake U-SQL SDK requires the following dependencies:</span></span>

- <span data-ttu-id="c3c01-114">[A Microsoft .NET-keretrendszer 4.6-os vagy újabb](https://www.microsoft.com/download/details.aspx?id=17851).</span><span class="sxs-lookup"><span data-stu-id="c3c01-114">[Microsoft .NET Framework 4.6 or newer](https://www.microsoft.com/download/details.aspx?id=17851).</span></span>
- <span data-ttu-id="c3c01-115">Microsoft Visual C++ 14 és a Windows SDK 10.0.10240.0 vagy újabb (amely más néven CppSDK ebben a cikkben).</span><span class="sxs-lookup"><span data-stu-id="c3c01-115">Microsoft Visual C++ 14 and Windows SDK 10.0.10240.0 or newer (which is called CppSDK in this article).</span></span> <span data-ttu-id="c3c01-116">Kétféleképpen CppSDK eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="c3c01-116">There are two ways to get CppSDK:</span></span>

    - <span data-ttu-id="c3c01-117">Telepítés [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou).</span><span class="sxs-lookup"><span data-stu-id="c3c01-117">Install [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou).</span></span> <span data-ttu-id="c3c01-118">A Program Files mappában – például a C:\Program Files (x86) \Windows Kits\10\ kell egy \Windows Kits\10 mappát.</span><span class="sxs-lookup"><span data-stu-id="c3c01-118">You'll have a \Windows Kits\10 folder under the Program Files folder--for example, C:\Program Files (x86)\Windows Kits\10\.</span></span> <span data-ttu-id="c3c01-119">A Windows 10-es SDK verzió \Windows Kits\10\Lib is találhat.</span><span class="sxs-lookup"><span data-stu-id="c3c01-119">You'll also find the Windows 10 SDK version under \Windows Kits\10\Lib.</span></span> <span data-ttu-id="c3c01-120">Ha nem látja ezeket a mappákat, telepítse újra a Visual Studio, és ügyeljen arra, hogy a telepítés során válassza ki a Windows 10-es SDK-t.</span><span class="sxs-lookup"><span data-stu-id="c3c01-120">If you don’t see these folders, reinstall Visual Studio and be sure to select the Windows 10 SDK during the installation.</span></span> <span data-ttu-id="c3c01-121">Ha ez a Visual Studio telepítve van, a U-SQL helyi fordító megkeresi azt automatikusan.</span><span class="sxs-lookup"><span data-stu-id="c3c01-121">If you have this installed with Visual Studio, the U-SQL local compiler will find it automatically.</span></span>

    ![A Data Lake Tools for Visual Studio helyi futtatási Windows 10-es SDK](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

    - <span data-ttu-id="c3c01-123">Telepítés [a Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs).</span><span class="sxs-lookup"><span data-stu-id="c3c01-123">Install [Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs).</span></span> <span data-ttu-id="c3c01-124">Az előre csomagolt Visual C++ és a Windows SDK-fájlokat a C:\Program Files (x86) \Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK találja.</span><span class="sxs-lookup"><span data-stu-id="c3c01-124">You can find the prepackaged Visual C++ and Windows SDK files at C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK.</span></span> <span data-ttu-id="c3c01-125">Ebben az esetben a U-SQL helyi fordító nem található a függőségek automatikusan.</span><span class="sxs-lookup"><span data-stu-id="c3c01-125">In this case, the U-SQL local compiler cannot find the dependencies automatically.</span></span> <span data-ttu-id="c3c01-126">Meg kell adnia a CppSDK elérési út azt.</span><span class="sxs-lookup"><span data-stu-id="c3c01-126">You need to specify the CppSDK path for it.</span></span> <span data-ttu-id="c3c01-127">Másolja a fájlokat egy másik helyre, vagy használja a jelenlegi állapotában.</span><span class="sxs-lookup"><span data-stu-id="c3c01-127">You can either copy the files to another location or use it as is.</span></span>

## <a name="understand-basic-concepts"></a><span data-ttu-id="c3c01-128">Alapvető fogalmak megismerése</span><span class="sxs-lookup"><span data-stu-id="c3c01-128">Understand basic concepts</span></span>

### <a name="data-root"></a><span data-ttu-id="c3c01-129">Adatgyökerében</span><span class="sxs-lookup"><span data-stu-id="c3c01-129">Data root</span></span>

<span data-ttu-id="c3c01-130">Az adatok-gyökérmappája "helyi tároló" a helyi számítási fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="c3c01-130">The data-root folder is a "local store" for the local compute account.</span></span> <span data-ttu-id="c3c01-131">Megegyezik a Data Lake Analytics-fiók az Azure Data Lake Store-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="c3c01-131">It's equivalent to the Azure Data Lake Store account of a Data Lake Analytics account.</span></span> <span data-ttu-id="c3c01-132">Váltás másik adatok-gyökérmappának az csakúgy, mint egy másik store-fiók vált.</span><span class="sxs-lookup"><span data-stu-id="c3c01-132">Switching to a different data-root folder is just like switching to a different store account.</span></span> <span data-ttu-id="c3c01-133">Ha azt szeretné, más adatok legfelső szintű mappák általában megosztott adatokhoz való hozzáférést, abszolút elérési utakat kell használnia a parancsfájlban.</span><span class="sxs-lookup"><span data-stu-id="c3c01-133">If you want to access commonly shared data with different data-root folders, you must use absolute paths in your scripts.</span></span> <span data-ttu-id="c3c01-134">Vagy hozzon létre fájlt rendszer szimbolikus hivatkozásokat (például **mklink** NTFS) a megosztott adatok az adatok-gyökere alatt.</span><span class="sxs-lookup"><span data-stu-id="c3c01-134">Or, create file system symbolic links (for example, **mklink** on NTFS) under the data-root folder to point to the shared data.</span></span>

<span data-ttu-id="c3c01-135">Az adatok gyökérmappa a következőkre használható:</span><span class="sxs-lookup"><span data-stu-id="c3c01-135">The data-root folder is used to:</span></span>

- <span data-ttu-id="c3c01-136">Helyi metaadatainak, például adatbázisok, táblák, táblaértékű függvények (TVFs) és szerelvényeket tárolják.</span><span class="sxs-lookup"><span data-stu-id="c3c01-136">Store local metadata, including databases, tables, table-valued functions (TVFs), and assemblies.</span></span>
- <span data-ttu-id="c3c01-137">Keresse meg a bemeneti és kimeneti elérési utak relatív elérési utak a U-SQL is meg van adva.</span><span class="sxs-lookup"><span data-stu-id="c3c01-137">Look up the input and output paths that are defined as relative paths in U-SQL.</span></span> <span data-ttu-id="c3c01-138">Relatív elérési utak használata megkönnyíti a U-SQL projekt telepítése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="c3c01-138">Using relative paths makes it easier to deploy your U-SQL projects to Azure.</span></span>

### <a name="file-path-in-u-sql"></a><span data-ttu-id="c3c01-139">U-SQL-fájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="c3c01-139">File path in U-SQL</span></span>

<span data-ttu-id="c3c01-140">U-SQL-parancsfájlok használhatja relatív elérési út és a helyi abszolút elérési utat.</span><span class="sxs-lookup"><span data-stu-id="c3c01-140">You can use both a relative path and a local absolute path in U-SQL scripts.</span></span> <span data-ttu-id="c3c01-141">A relatív elérési út a megadott adatok-gyökérmappa elérési útja viszonyítva.</span><span class="sxs-lookup"><span data-stu-id="c3c01-141">The relative path is relative to the specified data-root folder path.</span></span> <span data-ttu-id="c3c01-142">Azt javasoljuk, hogy használjon "/" az elérési út elválasztó annak a parancsfájlokat a kiszolgálóoldali kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="c3c01-142">We recommend that you use "/" as the path separator to make your scripts compatible with the server side.</span></span> <span data-ttu-id="c3c01-143">Az alábbiakban néhány olyan relatív elérési útja és az egyenértékű abszolút elérési utakat.</span><span class="sxs-lookup"><span data-stu-id="c3c01-143">Here are some examples of relative paths and their equivalent absolute paths.</span></span> <span data-ttu-id="c3c01-144">Ezekben a példákban C:\LocalRunDataRoot az adatok-gyökérmappájába.</span><span class="sxs-lookup"><span data-stu-id="c3c01-144">In these examples, C:\LocalRunDataRoot is the data-root folder.</span></span>

|<span data-ttu-id="c3c01-145">Relatív elérési útja</span><span class="sxs-lookup"><span data-stu-id="c3c01-145">Relative path</span></span>|<span data-ttu-id="c3c01-146">Abszolút elérési útja</span><span class="sxs-lookup"><span data-stu-id="c3c01-146">Absolute path</span></span>|
|-------------|-------------|
|<span data-ttu-id="c3c01-147">/ABC/DEF/input.csv</span><span class="sxs-lookup"><span data-stu-id="c3c01-147">/abc/def/input.csv</span></span> |<span data-ttu-id="c3c01-148">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="c3c01-148">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="c3c01-149">ABC/DEF/input.csv</span><span class="sxs-lookup"><span data-stu-id="c3c01-149">abc/def/input.csv</span></span>  |<span data-ttu-id="c3c01-150">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="c3c01-150">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="c3c01-151">D:/ABC/DEF/input.csv</span><span class="sxs-lookup"><span data-stu-id="c3c01-151">D:/abc/def/input.csv</span></span> |<span data-ttu-id="c3c01-152">D:\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="c3c01-152">D:\abc\def\input.csv</span></span>|

### <a name="working-directory"></a><span data-ttu-id="c3c01-153">Munkakönyvtár</span><span class="sxs-lookup"><span data-stu-id="c3c01-153">Working directory</span></span>

<span data-ttu-id="c3c01-154">A U-SQL parancsfájl helyben fut, amikor egy működő könyvtárba aktuális futtatási könyvtárának a fordítás során jön létre.</span><span class="sxs-lookup"><span data-stu-id="c3c01-154">When running the U-SQL script locally, a working directory is created during compilation under current running directory.</span></span> <span data-ttu-id="c3c01-155">A fordítás kimenetek mellett a szükséges futásidejű fájlok helyi végrehajtásra lesz a munkakönyvtárba árnyékmásolat.</span><span class="sxs-lookup"><span data-stu-id="c3c01-155">In addition to the compilation outputs, the needed runtime files for local execution will be shadow copied to this working directory.</span></span> <span data-ttu-id="c3c01-156">Working directory gyökérmappájába "ScopeWorkDir" nevezik, és a munkakönyvtárat a fájlok a következők:</span><span class="sxs-lookup"><span data-stu-id="c3c01-156">The working directory root folder is called "ScopeWorkDir" and the files under the working directory are as follows:</span></span>

|<span data-ttu-id="c3c01-157">Könyvtár vagy fájl</span><span class="sxs-lookup"><span data-stu-id="c3c01-157">Directory/file</span></span>|<span data-ttu-id="c3c01-158">Könyvtár vagy fájl</span><span class="sxs-lookup"><span data-stu-id="c3c01-158">Directory/file</span></span>|<span data-ttu-id="c3c01-159">Könyvtár vagy fájl</span><span class="sxs-lookup"><span data-stu-id="c3c01-159">Directory/file</span></span>|<span data-ttu-id="c3c01-160">Meghatározás</span><span class="sxs-lookup"><span data-stu-id="c3c01-160">Definition</span></span>|<span data-ttu-id="c3c01-161">Leírás</span><span class="sxs-lookup"><span data-stu-id="c3c01-161">Description</span></span>|
|--------------|--------------|--------------|----------|-----------|
|<span data-ttu-id="c3c01-162">C6A101DDCB470506</span><span class="sxs-lookup"><span data-stu-id="c3c01-162">C6A101DDCB470506</span></span>| | |<span data-ttu-id="c3c01-163">Kivonat karakterláncot futtatókörnyezet-verzió</span><span class="sxs-lookup"><span data-stu-id="c3c01-163">Hash string of runtime version</span></span>|<span data-ttu-id="c3c01-164">A helyi végrehajtásához szükséges futásidejű fájlok árnyékmásolatát</span><span class="sxs-lookup"><span data-stu-id="c3c01-164">Shadow copy of runtime files needed for local execution</span></span>|
| |<span data-ttu-id="c3c01-165">Script_66AE4909AA0ED06C</span><span class="sxs-lookup"><span data-stu-id="c3c01-165">Script_66AE4909AA0ED06C</span></span>| |<span data-ttu-id="c3c01-166">Parancsfájl neve + kivonat karakterláncot a parancsprogram elérési útja</span><span class="sxs-lookup"><span data-stu-id="c3c01-166">Script name + hash string of script path</span></span>|<span data-ttu-id="c3c01-167">Fordítási kimenetek és végrehajtási lépés a naplózás</span><span class="sxs-lookup"><span data-stu-id="c3c01-167">Compilation outputs and execution step logging</span></span>|
| | |<span data-ttu-id="c3c01-168">\_parancsfájl\_.abr</span><span class="sxs-lookup"><span data-stu-id="c3c01-168">\_script\_.abr</span></span>|<span data-ttu-id="c3c01-169">A fordítóprogram kimenetének</span><span class="sxs-lookup"><span data-stu-id="c3c01-169">Compiler output</span></span>|<span data-ttu-id="c3c01-170">Algebra fájl</span><span class="sxs-lookup"><span data-stu-id="c3c01-170">Algebra file</span></span>|
| | |<span data-ttu-id="c3c01-171">\_ScopeCodeGen\_. *</span><span class="sxs-lookup"><span data-stu-id="c3c01-171">\_ScopeCodeGen\_.*</span></span>|<span data-ttu-id="c3c01-172">A fordítóprogram kimenetének</span><span class="sxs-lookup"><span data-stu-id="c3c01-172">Compiler output</span></span>|<span data-ttu-id="c3c01-173">A felügyelt kód jön létre</span><span class="sxs-lookup"><span data-stu-id="c3c01-173">Generated managed code</span></span>|
| | |<span data-ttu-id="c3c01-174">\_ScopeCodeGenEngine\_. *</span><span class="sxs-lookup"><span data-stu-id="c3c01-174">\_ScopeCodeGenEngine\_.*</span></span>|<span data-ttu-id="c3c01-175">A fordítóprogram kimenetének</span><span class="sxs-lookup"><span data-stu-id="c3c01-175">Compiler output</span></span>|<span data-ttu-id="c3c01-176">Létrehozott natív kód</span><span class="sxs-lookup"><span data-stu-id="c3c01-176">Generated native code</span></span>|
| | |<span data-ttu-id="c3c01-177">hivatkozott szerelvények</span><span class="sxs-lookup"><span data-stu-id="c3c01-177">referenced assemblies</span></span>|<span data-ttu-id="c3c01-178">Szerelvényre mutató hivatkozás</span><span class="sxs-lookup"><span data-stu-id="c3c01-178">Assembly reference</span></span>|<span data-ttu-id="c3c01-179">Hivatkozott szerelvény fájlok</span><span class="sxs-lookup"><span data-stu-id="c3c01-179">Referenced assembly files</span></span>|
| | |<span data-ttu-id="c3c01-180">deployed_resources</span><span class="sxs-lookup"><span data-stu-id="c3c01-180">deployed_resources</span></span>|<span data-ttu-id="c3c01-181">Erőforrások telepítése</span><span class="sxs-lookup"><span data-stu-id="c3c01-181">Resource deployment</span></span>|<span data-ttu-id="c3c01-182">Erőforrás-telepítési fájlok</span><span class="sxs-lookup"><span data-stu-id="c3c01-182">Resource deployment files</span></span>|
| | |<span data-ttu-id="c3c01-183">xxxxxxxx.xxx[1..n]\_\*. *</span><span class="sxs-lookup"><span data-stu-id="c3c01-183">xxxxxxxx.xxx[1..n]\_\*.*</span></span>|<span data-ttu-id="c3c01-184">Végrehajtási napló</span><span class="sxs-lookup"><span data-stu-id="c3c01-184">Execution log</span></span>|<span data-ttu-id="c3c01-185">A végrehajtási lépések napló</span><span class="sxs-lookup"><span data-stu-id="c3c01-185">Log of execution steps</span></span>|


## <a name="use-the-sdk-from-the-command-line"></a><span data-ttu-id="c3c01-186">Az SDK-val a parancssorból</span><span class="sxs-lookup"><span data-stu-id="c3c01-186">Use the SDK from the command line</span></span>

### <a name="command-line-interface-of-the-helper-application"></a><span data-ttu-id="c3c01-187">Parancssori felületének a segédalkalmazással.</span><span class="sxs-lookup"><span data-stu-id="c3c01-187">Command-line interface of the helper application</span></span>

<span data-ttu-id="c3c01-188">Az SDK directory\build\runtime LocalRunHelper.exe kifejezés a parancssori segédprogram, amely a gyakran használt helyi futtatási funkciók a legtöbb felületet biztosít.</span><span class="sxs-lookup"><span data-stu-id="c3c01-188">Under SDK directory\build\runtime, LocalRunHelper.exe is the command-line helper application that provides interfaces to most of the commonly used local-run functions.</span></span> <span data-ttu-id="c3c01-189">Vegye figyelembe, hogy a parancs, mind a argumentum kapcsolók kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="c3c01-189">Note that both the command and the argument switches are case-sensitive.</span></span> <span data-ttu-id="c3c01-190">Indítsa el:</span><span class="sxs-lookup"><span data-stu-id="c3c01-190">To invoke it:</span></span>

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

<span data-ttu-id="c3c01-191">Paraméterek nélkül, vagy a LocalRunHelper.exe fusson a **súgó** kapcsolót, hogy az információk megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="c3c01-191">Run LocalRunHelper.exe without arguments or with the **help** switch to show the help information:</span></span>

    > LocalRunHelper.exe help

        Command 'help' :  Show usage information
        Command 'compile' :  Compile the script
        Required Arguments :
            -Script param
                    Script File Path
        Optional Arguments :
            -Shallow [default value 'False']
                    Shallow compile

<span data-ttu-id="c3c01-192">A Súgó adatokat:</span><span class="sxs-lookup"><span data-stu-id="c3c01-192">In the help information:</span></span>

-  <span data-ttu-id="c3c01-193">**A parancs** elnevezi a parancsot.</span><span class="sxs-lookup"><span data-stu-id="c3c01-193">**Command** gives the command’s name.</span></span>  
-  <span data-ttu-id="c3c01-194">**Kötelező argumentum** , meg kell adni argumentumait sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="c3c01-194">**Required Argument** lists arguments that must be supplied.</span></span>  
-  <span data-ttu-id="c3c01-195">**Nem kötelező argumentumában** sorolja fel, ez nem kötelező megadni, alapértelmezett értékekkel.</span><span class="sxs-lookup"><span data-stu-id="c3c01-195">**Optional Argument** lists arguments that are optional, with default values.</span></span>  <span data-ttu-id="c3c01-196">Nem kötelező, logikai típusú argumentumokat nem kell a paramétereket, és azok megjelenésének jelenti negatív alapértelmezett értékét.</span><span class="sxs-lookup"><span data-stu-id="c3c01-196">Optional Boolean arguments don’t have parameters, and their appearances mean negative to their default value.</span></span>

### <a name="return-value-and-logging"></a><span data-ttu-id="c3c01-197">Visszatérési érték és a naplózás</span><span class="sxs-lookup"><span data-stu-id="c3c01-197">Return value and logging</span></span>

<span data-ttu-id="c3c01-198">Visszaadja a segédalkalmazással **0** a sikeres és **-1** sikertelen.</span><span class="sxs-lookup"><span data-stu-id="c3c01-198">The helper application returns **0** for success and **-1** for failure.</span></span> <span data-ttu-id="c3c01-199">Alapértelmezés szerint az segítő minden üzenetet küld a jelenlegi konzolt.</span><span class="sxs-lookup"><span data-stu-id="c3c01-199">By default, the helper sends all messages to the current console.</span></span> <span data-ttu-id="c3c01-200">Azonban a legtöbb parancs támogatja a **- MessageOut naplófájl_elérési_útja** nem kötelező argumentum, amely átirányítja a kimenetek naplófájlba.</span><span class="sxs-lookup"><span data-stu-id="c3c01-200">However, most of the commands support the **-MessageOut path_to_log_file** optional argument that redirects the outputs to a log file.</span></span>

### <a name="environment-variable-configuring"></a><span data-ttu-id="c3c01-201">Környezeti változó beállítása</span><span class="sxs-lookup"><span data-stu-id="c3c01-201">Environment variable configuring</span></span>

<span data-ttu-id="c3c01-202">U-SQL helyi futnia kell a megadott adatok legfelső szintű helyi storage-fiók, valamint egy adott CppSDK függőségek elérési úton.</span><span class="sxs-lookup"><span data-stu-id="c3c01-202">U-SQL local run needs a specified data root as local storage account, as well as a specified CppSDK path for dependencies.</span></span> <span data-ttu-id="c3c01-203">Is beállíthat a argumentum parancssori vagy beállított környezeti változóban számukra.</span><span class="sxs-lookup"><span data-stu-id="c3c01-203">You can both set the argument in command-line or set environment variable for them.</span></span>

- <span data-ttu-id="c3c01-204">Állítsa be a **SCOPE_CPP_SDK** környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="c3c01-204">Set the **SCOPE_CPP_SDK** environment variable.</span></span>

    <span data-ttu-id="c3c01-205">Ha a Microsoft Visual C++ és a Windows SDK-t úgy, hogy a Data Lake Tools for Visual Studio telepítése, győződjön meg arról, hogy rendelkezik-e a következő mappát:</span><span class="sxs-lookup"><span data-stu-id="c3c01-205">If you get Microsoft Visual C++ and the Windows SDK by installing Data Lake Tools for Visual Studio, verify that you have the following folder:</span></span>

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    <span data-ttu-id="c3c01-206">Adja meg egy új környezeti változó neve **SCOPE_CPP_SDK** úgy, hogy ez a könyvtár mutasson.</span><span class="sxs-lookup"><span data-stu-id="c3c01-206">Define a new environment variable called **SCOPE_CPP_SDK** to point to this directory.</span></span> <span data-ttu-id="c3c01-207">Vagy a más helyre másolja a mappát, és adja meg **SCOPE_CPP_SDK** , mint a.</span><span class="sxs-lookup"><span data-stu-id="c3c01-207">Or copy the folder to the other location and specify **SCOPE_CPP_SDK** as that.</span></span>

    <span data-ttu-id="c3c01-208">A környezeti változó mellett, megadhatja a **- CppSDK** argumentum a parancssorban használatakor.</span><span class="sxs-lookup"><span data-stu-id="c3c01-208">In addition to setting the environment variable, you can specify the **-CppSDK** argument when you're using the command line.</span></span> <span data-ttu-id="c3c01-209">Ennek az argumentumnak felülírja az alapértelmezett CppSDK környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="c3c01-209">This argument overwrites your default CppSDK environment variable.</span></span>

- <span data-ttu-id="c3c01-210">Állítsa be a **LOCALRUN_DATAROOT** környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="c3c01-210">Set the **LOCALRUN_DATAROOT** environment variable.</span></span>

    <span data-ttu-id="c3c01-211">Adja meg egy új környezeti változó neve **LOCALRUN_DATAROOT** a adatgyökerében mutat.</span><span class="sxs-lookup"><span data-stu-id="c3c01-211">Define a new environment variable called **LOCALRUN_DATAROOT** that points to the data root.</span></span>

    <span data-ttu-id="c3c01-212">A környezeti változó mellett, megadhatja a **- DataRoot** adatok-gyökérútvonalát parancssor használatakor argumentumot.</span><span class="sxs-lookup"><span data-stu-id="c3c01-212">In addition to setting the environment variable, you can specify the **-DataRoot** argument with the data-root path when you're using a command line.</span></span> <span data-ttu-id="c3c01-213">Ennek az argumentumnak felülírja az alapértelmezett adatgyökerében környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="c3c01-213">This argument overwrites your default data-root environment variable.</span></span> <span data-ttu-id="c3c01-214">Ennek az argumentumnak parancssorhoz adhat hozzá minden futtatja, hogy az alapértelmezett adatgyökerében környezeti változó minden műveletnél felülírhatja kell.</span><span class="sxs-lookup"><span data-stu-id="c3c01-214">You need to add this argument to every command line you're running so that you can overwrite the default data-root environment variable for all operations.</span></span>

### <a name="sdk-command-line-usage-samples"></a><span data-ttu-id="c3c01-215">SDK parancssori használati minták</span><span class="sxs-lookup"><span data-stu-id="c3c01-215">SDK command line usage samples</span></span>

#### <a name="compile-and-run"></a><span data-ttu-id="c3c01-216">Fordítás és futtatás</span><span class="sxs-lookup"><span data-stu-id="c3c01-216">Compile and run</span></span>

<span data-ttu-id="c3c01-217">A **futtatása** parancs segítségével a parancsfájl összeállításához és lefordított eredmények hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="c3c01-217">The **run** command is used to compile the script and then execute compiled results.</span></span> <span data-ttu-id="c3c01-218">Parancssori argumentumok szerintiek kombinációja **fordítási** és **hajtható végre**.</span><span class="sxs-lookup"><span data-stu-id="c3c01-218">Its command-line arguments are a combination of those from **compile** and **execute**.</span></span>

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

<span data-ttu-id="c3c01-219">A következők: nem kötelező argumentumainak **futtatása**:</span><span class="sxs-lookup"><span data-stu-id="c3c01-219">The following are optional arguments for **run**:</span></span>


|<span data-ttu-id="c3c01-220">Argumentum</span><span class="sxs-lookup"><span data-stu-id="c3c01-220">Argument</span></span>|<span data-ttu-id="c3c01-221">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c3c01-221">Default value</span></span>|<span data-ttu-id="c3c01-222">Leírás</span><span class="sxs-lookup"><span data-stu-id="c3c01-222">Description</span></span>|
|--------|-------------|-----------|
|<span data-ttu-id="c3c01-223">-Háttérkódban</span><span class="sxs-lookup"><span data-stu-id="c3c01-223">-CodeBehind</span></span>|<span data-ttu-id="c3c01-224">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="c3c01-224">False</span></span>|<span data-ttu-id="c3c01-225">A parancsfájl .cs kód tartozik.</span><span class="sxs-lookup"><span data-stu-id="c3c01-225">The script has .cs code behind</span></span>|
|<span data-ttu-id="c3c01-226">-CppSDK</span><span class="sxs-lookup"><span data-stu-id="c3c01-226">-CppSDK</span></span>| |<span data-ttu-id="c3c01-227">CppSDK könyvtár</span><span class="sxs-lookup"><span data-stu-id="c3c01-227">CppSDK Directory</span></span>|
|<span data-ttu-id="c3c01-228">-DataRoot</span><span class="sxs-lookup"><span data-stu-id="c3c01-228">-DataRoot</span></span>| <span data-ttu-id="c3c01-229">DataRoot környezeti változó</span><span class="sxs-lookup"><span data-stu-id="c3c01-229">DataRoot environment variable</span></span>|<span data-ttu-id="c3c01-230">A helyi futtatáskor, "LOCALRUN_DATAROOT" környezeti változó alapértelmezett DataRoot</span><span class="sxs-lookup"><span data-stu-id="c3c01-230">DataRoot for local run, default to 'LOCALRUN_DATAROOT' environment variable</span></span>|
|<span data-ttu-id="c3c01-231">-MessageOut</span><span class="sxs-lookup"><span data-stu-id="c3c01-231">-MessageOut</span></span>| |<span data-ttu-id="c3c01-232">A konzol fájlba memóriakép üzenetek</span><span class="sxs-lookup"><span data-stu-id="c3c01-232">Dump messages on console to a file</span></span>|
|<span data-ttu-id="c3c01-233">-Párhuzamos</span><span class="sxs-lookup"><span data-stu-id="c3c01-233">-Parallel</span></span>|<span data-ttu-id="c3c01-234">1</span><span class="sxs-lookup"><span data-stu-id="c3c01-234">1</span></span>|<span data-ttu-id="c3c01-235">A terv a megadott párhuzamos Futtatás</span><span class="sxs-lookup"><span data-stu-id="c3c01-235">Run the plan with the specified parallelism</span></span>|
|<span data-ttu-id="c3c01-236">-Hivatkozások</span><span class="sxs-lookup"><span data-stu-id="c3c01-236">-References</span></span>| |<span data-ttu-id="c3c01-237">Az elérési utak listája további hivatkozási szerelvények vagy adatforrás adatfájljainak tartozó kódot, szóközzel elválasztva ';'</span><span class="sxs-lookup"><span data-stu-id="c3c01-237">List of paths to extra reference assemblies or data files of code behind, separated by ';'</span></span>|
|<span data-ttu-id="c3c01-238">-UdoRedirect</span><span class="sxs-lookup"><span data-stu-id="c3c01-238">-UdoRedirect</span></span>|<span data-ttu-id="c3c01-239">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="c3c01-239">False</span></span>|<span data-ttu-id="c3c01-240">Udo szerelvény átirányítási config készítése</span><span class="sxs-lookup"><span data-stu-id="c3c01-240">Generate Udo assembly redirect config</span></span>|
|<span data-ttu-id="c3c01-241">-UseDatabase</span><span class="sxs-lookup"><span data-stu-id="c3c01-241">-UseDatabase</span></span>|<span data-ttu-id="c3c01-242">master</span><span class="sxs-lookup"><span data-stu-id="c3c01-242">master</span></span>|<span data-ttu-id="c3c01-243">Az ideiglenes szerelvények regisztrálásakor háttérkódot használandó adatbázis</span><span class="sxs-lookup"><span data-stu-id="c3c01-243">Database to use for code behind temporary assembly registration</span></span>|
|<span data-ttu-id="c3c01-244">-Verbose</span><span class="sxs-lookup"><span data-stu-id="c3c01-244">-Verbose</span></span>|<span data-ttu-id="c3c01-245">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="c3c01-245">False</span></span>|<span data-ttu-id="c3c01-246">A futásidejű részletes kimenetének megjelenítése</span><span class="sxs-lookup"><span data-stu-id="c3c01-246">Show detailed outputs from runtime</span></span>|
|<span data-ttu-id="c3c01-247">-WorkDir</span><span class="sxs-lookup"><span data-stu-id="c3c01-247">-WorkDir</span></span>|<span data-ttu-id="c3c01-248">Aktuális könyvtárhoz</span><span class="sxs-lookup"><span data-stu-id="c3c01-248">Current Directory</span></span>|<span data-ttu-id="c3c01-249">A fordító használati és a kimeneti könyvtár</span><span class="sxs-lookup"><span data-stu-id="c3c01-249">Directory for compiler usage and outputs</span></span>|
|<span data-ttu-id="c3c01-250">-RunScopeCEP</span><span class="sxs-lookup"><span data-stu-id="c3c01-250">-RunScopeCEP</span></span>|<span data-ttu-id="c3c01-251">0</span><span class="sxs-lookup"><span data-stu-id="c3c01-251">0</span></span>|<span data-ttu-id="c3c01-252">ScopeCEP módot használja</span><span class="sxs-lookup"><span data-stu-id="c3c01-252">ScopeCEP mode to use</span></span>|
|<span data-ttu-id="c3c01-253">-ScopeCEPTempPath</span><span class="sxs-lookup"><span data-stu-id="c3c01-253">-ScopeCEPTempPath</span></span>|<span data-ttu-id="c3c01-254">TEMP</span><span class="sxs-lookup"><span data-stu-id="c3c01-254">temp</span></span>|<span data-ttu-id="c3c01-255">Ideiglenes útvonalat a streamelési adatok</span><span class="sxs-lookup"><span data-stu-id="c3c01-255">Temp path to use for streaming data</span></span>|
|<span data-ttu-id="c3c01-256">-OptFlags</span><span class="sxs-lookup"><span data-stu-id="c3c01-256">-OptFlags</span></span>| |<span data-ttu-id="c3c01-257">Optimalizáló jelzők vesszővel elválasztott listája</span><span class="sxs-lookup"><span data-stu-id="c3c01-257">Comma-separated list of optimizer flags</span></span>|


<span data-ttu-id="c3c01-258">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="c3c01-258">Here's an example:</span></span>

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

<span data-ttu-id="c3c01-259">Egyidejű mellett **fordítási** és **hajtható végre**, fordítása, és a lefordított végrehajtható fájlok külön-külön végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="c3c01-259">Besides combining **compile** and **execute**, you can compile and execute the compiled executables separately.</span></span>

#### <a name="compile-a-u-sql-script"></a><span data-ttu-id="c3c01-260">A U-SQL parancsfájl összeállítása</span><span class="sxs-lookup"><span data-stu-id="c3c01-260">Compile a U-SQL script</span></span>

<span data-ttu-id="c3c01-261">A **fordítási** parancs segítségével végrehajtható fájlokat egy U-SQL parancsfájl összeállításához.</span><span class="sxs-lookup"><span data-stu-id="c3c01-261">The **compile** command is used to compile a U-SQL script to executables.</span></span>

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

<span data-ttu-id="c3c01-262">A következők: nem kötelező argumentumainak **fordítási**:</span><span class="sxs-lookup"><span data-stu-id="c3c01-262">The following are optional arguments for **compile**:</span></span>


|<span data-ttu-id="c3c01-263">Argumentum</span><span class="sxs-lookup"><span data-stu-id="c3c01-263">Argument</span></span>|<span data-ttu-id="c3c01-264">Leírás</span><span class="sxs-lookup"><span data-stu-id="c3c01-264">Description</span></span>|
|--------|-----------|
| <span data-ttu-id="c3c01-265">-Háttérkódban [alapértelmezett értéke "False"]</span><span class="sxs-lookup"><span data-stu-id="c3c01-265">-CodeBehind [default value 'False']</span></span>|<span data-ttu-id="c3c01-266">A parancsfájl .cs kód tartozik.</span><span class="sxs-lookup"><span data-stu-id="c3c01-266">The script has .cs code behind</span></span>|
| <span data-ttu-id="c3c01-267">-CppSDK [alapértelmezett érték "]</span><span class="sxs-lookup"><span data-stu-id="c3c01-267">-CppSDK [default value '']</span></span>|<span data-ttu-id="c3c01-268">CppSDK könyvtár</span><span class="sxs-lookup"><span data-stu-id="c3c01-268">CppSDK Directory</span></span>|
| <span data-ttu-id="c3c01-269">-DataRoot [alapértelmezett érték "DataRoot környezeti változó"]</span><span class="sxs-lookup"><span data-stu-id="c3c01-269">-DataRoot [default value 'DataRoot environment variable']</span></span>|<span data-ttu-id="c3c01-270">A helyi futtatáskor, "LOCALRUN_DATAROOT" környezeti változó alapértelmezett DataRoot</span><span class="sxs-lookup"><span data-stu-id="c3c01-270">DataRoot for local run, default to 'LOCALRUN_DATAROOT' environment variable</span></span>|
| <span data-ttu-id="c3c01-271">-MessageOut [alapértelmezett érték "]</span><span class="sxs-lookup"><span data-stu-id="c3c01-271">-MessageOut [default value '']</span></span>|<span data-ttu-id="c3c01-272">A konzol fájlba memóriakép üzenetek</span><span class="sxs-lookup"><span data-stu-id="c3c01-272">Dump messages on console to a file</span></span>|
| <span data-ttu-id="c3c01-273">-Hivatkozik [alapértelmezett érték "]</span><span class="sxs-lookup"><span data-stu-id="c3c01-273">-References [default value '']</span></span>|<span data-ttu-id="c3c01-274">Az elérési utak listája további hivatkozási szerelvények vagy adatforrás adatfájljainak tartozó kódot, szóközzel elválasztva ';'</span><span class="sxs-lookup"><span data-stu-id="c3c01-274">List of paths to extra reference assemblies or data files of code behind, separated by ';'</span></span>|
| <span data-ttu-id="c3c01-275">-Sekély [alapértelmezett értéke "False"]</span><span class="sxs-lookup"><span data-stu-id="c3c01-275">-Shallow [default value 'False']</span></span>|<span data-ttu-id="c3c01-276">Sekély fordítási</span><span class="sxs-lookup"><span data-stu-id="c3c01-276">Shallow compile</span></span>|
| <span data-ttu-id="c3c01-277">-UdoRedirect [alapértelmezett értéke "False"]</span><span class="sxs-lookup"><span data-stu-id="c3c01-277">-UdoRedirect [default value 'False']</span></span>|<span data-ttu-id="c3c01-278">Udo szerelvény átirányítási config készítése</span><span class="sxs-lookup"><span data-stu-id="c3c01-278">Generate Udo assembly redirect config</span></span>|
| <span data-ttu-id="c3c01-279">-UseDatabase [alapértelmezett érték "master"]</span><span class="sxs-lookup"><span data-stu-id="c3c01-279">-UseDatabase [default value 'master']</span></span>|<span data-ttu-id="c3c01-280">Az ideiglenes szerelvények regisztrálásakor háttérkódot használandó adatbázis</span><span class="sxs-lookup"><span data-stu-id="c3c01-280">Database to use for code behind temporary assembly registration</span></span>|
| <span data-ttu-id="c3c01-281">-WorkDir [alapértelmezett érték "Az aktuális könyvtár"]</span><span class="sxs-lookup"><span data-stu-id="c3c01-281">-WorkDir [default value 'Current Directory']</span></span>|<span data-ttu-id="c3c01-282">A fordító használati és a kimeneti könyvtár</span><span class="sxs-lookup"><span data-stu-id="c3c01-282">Directory for compiler usage and outputs</span></span>|
| <span data-ttu-id="c3c01-283">-RunScopeCEP [alapértelmezett érték "0"]</span><span class="sxs-lookup"><span data-stu-id="c3c01-283">-RunScopeCEP [default value '0']</span></span>|<span data-ttu-id="c3c01-284">ScopeCEP módot használja</span><span class="sxs-lookup"><span data-stu-id="c3c01-284">ScopeCEP mode to use</span></span>|
| <span data-ttu-id="c3c01-285">-ScopeCEPTempPath [alapértelmezett érték "temp"]</span><span class="sxs-lookup"><span data-stu-id="c3c01-285">-ScopeCEPTempPath [default value 'temp']</span></span>|<span data-ttu-id="c3c01-286">Ideiglenes útvonalat a streamelési adatok</span><span class="sxs-lookup"><span data-stu-id="c3c01-286">Temp path to use for streaming data</span></span>|
| <span data-ttu-id="c3c01-287">-OptFlags [alapértelmezett érték "]</span><span class="sxs-lookup"><span data-stu-id="c3c01-287">-OptFlags [default value '']</span></span>|<span data-ttu-id="c3c01-288">Optimalizáló jelzők vesszővel elválasztott listája</span><span class="sxs-lookup"><span data-stu-id="c3c01-288">Comma-separated list of optimizer flags</span></span>|


<span data-ttu-id="c3c01-289">Az alábbiakban néhány használati példák.</span><span class="sxs-lookup"><span data-stu-id="c3c01-289">Here are some usage examples.</span></span>

<span data-ttu-id="c3c01-290">A U-SQL parancsfájl összeállítása:</span><span class="sxs-lookup"><span data-stu-id="c3c01-290">Compile a U-SQL script:</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql

<span data-ttu-id="c3c01-291">U-SQL parancsfájl összeállítása, és állítsa be az adatok gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="c3c01-291">Compile a U-SQL script and set the data-root folder.</span></span> <span data-ttu-id="c3c01-292">Vegye figyelembe, hogy ezzel a művelettel felülírja a készlet környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="c3c01-292">Note that this will overwrite the set environment variable.</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

<span data-ttu-id="c3c01-293">U-SQL parancsfájl fordítása, és egy működő könyvtárba, a referencia szerelvény és az adatbázis beállítása:</span><span class="sxs-lookup"><span data-stu-id="c3c01-293">Compile a U-SQL script and set a working directory, reference assembly, and database:</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a><span data-ttu-id="c3c01-294">Lefordított eredmények végrehajtása</span><span class="sxs-lookup"><span data-stu-id="c3c01-294">Execute compiled results</span></span>

<span data-ttu-id="c3c01-295">A **hajtható végre** parancs hajthatók végre a lefordított eredmények.</span><span class="sxs-lookup"><span data-stu-id="c3c01-295">The **execute** command is used to execute compiled results.</span></span>   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

<span data-ttu-id="c3c01-296">A következők: nem kötelező argumentumainak **hajtható végre**:</span><span class="sxs-lookup"><span data-stu-id="c3c01-296">The following are optional arguments for **execute**:</span></span>

|<span data-ttu-id="c3c01-297">Argumentum</span><span class="sxs-lookup"><span data-stu-id="c3c01-297">Argument</span></span>|<span data-ttu-id="c3c01-298">Leírás</span><span class="sxs-lookup"><span data-stu-id="c3c01-298">Description</span></span>|
|--------|-----------|
|<span data-ttu-id="c3c01-299">-DataRoot [alapértelmezett érték "]</span><span class="sxs-lookup"><span data-stu-id="c3c01-299">-DataRoot [default value '']</span></span>|<span data-ttu-id="c3c01-300">A metaadatok végrehajtás adatgyökerében.</span><span class="sxs-lookup"><span data-stu-id="c3c01-300">Data root for metadata execution.</span></span> <span data-ttu-id="c3c01-301">Alapértelmezés szerint az a **LOCALRUN_DATAROOT** környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="c3c01-301">It defaults to the **LOCALRUN_DATAROOT** environment variable.</span></span>|
|<span data-ttu-id="c3c01-302">-MessageOut [alapértelmezett érték "]</span><span class="sxs-lookup"><span data-stu-id="c3c01-302">-MessageOut [default value '']</span></span>|<span data-ttu-id="c3c01-303">A konzol egy fájlba az üzenetek dump.</span><span class="sxs-lookup"><span data-stu-id="c3c01-303">Dump messages on the console to a file.</span></span>|
|<span data-ttu-id="c3c01-304">-Párhuzamos [alapértelmezett értéke "1"]</span><span class="sxs-lookup"><span data-stu-id="c3c01-304">-Parallel [default value '1']</span></span>|<span data-ttu-id="c3c01-305">A létrehozott helyi futtatási lépéseket futtatni a megadott párhuzamossági szintű mutató.</span><span class="sxs-lookup"><span data-stu-id="c3c01-305">Indicator to run the generated local-run steps with the specified parallelism level.</span></span>|
|<span data-ttu-id="c3c01-306">-Verbose [alapértelmezett érték "Hamis"]</span><span class="sxs-lookup"><span data-stu-id="c3c01-306">-Verbose [default value 'False']</span></span>|<span data-ttu-id="c3c01-307">A futásidejű részletes kimenetének megjelenítése mutató.</span><span class="sxs-lookup"><span data-stu-id="c3c01-307">Indicator to show detailed outputs from runtime.</span></span>|

<span data-ttu-id="c3c01-308">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="c3c01-308">Here's a usage example:</span></span>

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-the-sdk-with-programming-interfaces"></a><span data-ttu-id="c3c01-309">Az SDK használatához programozási felületek</span><span class="sxs-lookup"><span data-stu-id="c3c01-309">Use the SDK with programming interfaces</span></span>

<span data-ttu-id="c3c01-310">Az alkalmazásprogramozási felületek a LocalRunHelper.exe a találhatók.</span><span class="sxs-lookup"><span data-stu-id="c3c01-310">The programming interfaces are all located in the LocalRunHelper.exe.</span></span> <span data-ttu-id="c3c01-311">A U-SQL SDK és a C# teszt keretében a U-SQL parancsfájl helyi tesztelése méretezési funkcióinak integrálását használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="c3c01-311">You can use them to integrate the functionality of the U-SQL SDK and the C# test framework to scale your U-SQL script local test.</span></span> <span data-ttu-id="c3c01-312">Ebben a cikkben használom bemutatja ezek felületek segítségével tesztelje a U-SQL parancsfájlt a szabványos C# egység teszt-projektet.</span><span class="sxs-lookup"><span data-stu-id="c3c01-312">In this article, I will use the standard C# unit test project to show how to use these interfaces to test your U-SQL script.</span></span>

### <a name="step-1-create-c-unit-test-project-and-configuration"></a><span data-ttu-id="c3c01-313">1. lépés: A C# egység teszt projekt és konfigurációs létrehozása</span><span class="sxs-lookup"><span data-stu-id="c3c01-313">Step 1: Create C# unit test project and configuration</span></span>

- <span data-ttu-id="c3c01-314">Hozzon létre egy C# egység teszt projektet fájl > Új > Projekt > Visual C# > tesztelése > egység tesztelése projekt.</span><span class="sxs-lookup"><span data-stu-id="c3c01-314">Create a C# unit test project through File > New > Project > Visual C# > Test > Unit Test Project.</span></span>
- <span data-ttu-id="c3c01-315">Adja hozzá egy hivatkozást a projekt LocalRunHelper.exe.</span><span class="sxs-lookup"><span data-stu-id="c3c01-315">Add LocalRunHelper.exe as a reference for the project.</span></span> <span data-ttu-id="c3c01-316">A LocalRunHelper.exe itt található: \build\runtime\LocalRunHelper.exe a Nuget-csomagot.</span><span class="sxs-lookup"><span data-stu-id="c3c01-316">The LocalRunHelper.exe is located at \build\runtime\LocalRunHelper.exe in Nuget package.</span></span>

    ![Azure Data Lake U-SQL SDK hivatkozás hozzáadása](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- <span data-ttu-id="c3c01-318">U-SQL SDK **csak** támogatási x64 környezet, ügyeljen arra, hogy x64 build platform célként beállítva.</span><span class="sxs-lookup"><span data-stu-id="c3c01-318">U-SQL SDK **only** support x64 environment, make sure to set build platform target as x64.</span></span> <span data-ttu-id="c3c01-319">Beállíthatja, hogy a projekt tulajdonságon keresztül > Build > Platform cél.</span><span class="sxs-lookup"><span data-stu-id="c3c01-319">You can set that through Project Property > Build > Platform target.</span></span>

    ![Azure Data Lake U-SQL SDK konfigurálása x64 projekt](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- <span data-ttu-id="c3c01-321">Ügyeljen arra, hogy a tesztkörnyezet beállítása x64.</span><span class="sxs-lookup"><span data-stu-id="c3c01-321">Make sure to set your test environment as x64.</span></span> <span data-ttu-id="c3c01-322">A Visual Studio állíthatja keresztül Test > vizsgálati beállítások > alapértelmezett processzorarchitektúra > x64.</span><span class="sxs-lookup"><span data-stu-id="c3c01-322">In Visual Studio, you can set it through Test > Test Settings > Default Processor Architecture > x64.</span></span>

    ![Azure Data Lake U-SQL SDK konfigurálása x64 tesztelési környezetben](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- <span data-ttu-id="c3c01-324">Ügyeljen arra, hogy másolja az összes függőség fájlt a NugetPackage\build\runtime\ munkakönyvtár, amely általában a ProjectFolder\bin\x64\Debug projekthez.</span><span class="sxs-lookup"><span data-stu-id="c3c01-324">Make sure to copy all dependency files under NugetPackage\build\runtime\ to project working directory which is usually under ProjectFolder\bin\x64\Debug.</span></span>

### <a name="step-2-create-u-sql-script-test-case"></a><span data-ttu-id="c3c01-325">2. lépés: A U-SQL parancsfájl Teszteset létrehozása</span><span class="sxs-lookup"><span data-stu-id="c3c01-325">Step 2: Create U-SQL script test case</span></span>

<span data-ttu-id="c3c01-326">Az alábbiakban van a mintakódot a U-SQL parancsfájl teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c3c01-326">Below is the sample code for U-SQL script test.</span></span> <span data-ttu-id="c3c01-327">Tesztelési, elő kell készíteni a parancsfájlok, a bemeneti fájlok és a várt kimeneti fájlokat.</span><span class="sxs-lookup"><span data-stu-id="c3c01-327">For testing, you need to prepare scripts, input files and expected output files.</span></span>

    using System;
    using Microsoft.VisualStudio.TestTools.UnitTesting;
    using System.IO;
    using System.Text;
    using System.Security.Cryptography;
    using Microsoft.Analytics.LocalRun;

    namespace UnitTestProject1
    {
        [TestClass]
        public class USQLUnitTest
        {
            [TestMethod]
            public void TestUSQLScript()
            {
                //Specify the local run message output path
                StreamWriter MessageOutput = new StreamWriter("../../../log.txt");

                LocalRunHelper localrun = new LocalRunHelper(MessageOutput);

                //Configure the DateRoot path, Script Path and CPPSDK path
                localrun.DataRoot = "../../../";
                localrun.ScriptPath = "../../../Script/Script.usql";
                localrun.CppSdkDir = "../../../CppSDK";

                //Run U-SQL script
                localrun.DoRun();

                //Script output 
                string Result = Path.Combine(localrun.DataRoot, "Output/result.csv");

                //Expected script output
                string ExpectedResult = "../../../ExpectedOutput/result.csv";

                Test.Helpers.FileAssert.AreEqual(Result, ExpectedResult);

                //Don't forget to close MessageOutput to get logs into file
                MessageOutput.Close();
            }
        }
    }

    namespace Test.Helpers
    {
        public static class FileAssert
        {
            static string GetFileHash(string filename)
            {
                Assert.IsTrue(File.Exists(filename));

                using (var hash = new SHA1Managed())
                {
                    var clearBytes = File.ReadAllBytes(filename);
                    var hashedBytes = hash.ComputeHash(clearBytes);
                    return ConvertBytesToHex(hashedBytes);
                }
            }

            static string ConvertBytesToHex(byte[] bytes)
            {
                var sb = new StringBuilder();

                for (var i = 0; i < bytes.Length; i++)
                {
                    sb.Append(bytes[i].ToString("x"));
                }
                return sb.ToString();
            }

            public static void AreEqual(string filename1, string filename2)
            {
                string hash1 = GetFileHash(filename1);
                string hash2 = GetFileHash(filename2);

                Assert.AreEqual(hash1, hash2);
            }
        }
    }


### <a name="programming-interfaces-in-localrunhelperexe"></a><span data-ttu-id="c3c01-328">A LocalRunHelper.exe felületek</span><span class="sxs-lookup"><span data-stu-id="c3c01-328">Programming interfaces in LocalRunHelper.exe</span></span>

<span data-ttu-id="c3c01-329">LocalRunHelper.exe az alkalmazásprogramozási felületek biztosít a U-SQL helyi fordítási, futtassa, stb. Azok a felületek a következőképpen vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="c3c01-329">LocalRunHelper.exe provides the programming interfaces for U-SQL local compile, run, etc. The interfaces are listed as follows.</span></span>

<span data-ttu-id="c3c01-330">**Konstruktor**</span><span class="sxs-lookup"><span data-stu-id="c3c01-330">**Constructor**</span></span>

<span data-ttu-id="c3c01-331">nyilvános LocalRunHelper ([System.IO.TextWriter messageOutput = null])</span><span class="sxs-lookup"><span data-stu-id="c3c01-331">public LocalRunHelper([System.IO.TextWriter messageOutput = null])</span></span>

|<span data-ttu-id="c3c01-332">Paraméter</span><span class="sxs-lookup"><span data-stu-id="c3c01-332">Parameter</span></span>|<span data-ttu-id="c3c01-333">Típus</span><span class="sxs-lookup"><span data-stu-id="c3c01-333">Type</span></span>|<span data-ttu-id="c3c01-334">Leírás</span><span class="sxs-lookup"><span data-stu-id="c3c01-334">Description</span></span>|
|---------|----|-----------|
|<span data-ttu-id="c3c01-335">messageOutput</span><span class="sxs-lookup"><span data-stu-id="c3c01-335">messageOutput</span></span>|<span data-ttu-id="c3c01-336">System.IO.TextWriter</span><span class="sxs-lookup"><span data-stu-id="c3c01-336">System.IO.TextWriter</span></span>|<span data-ttu-id="c3c01-337">a kimeneti üzenetek be null konzol használata</span><span class="sxs-lookup"><span data-stu-id="c3c01-337">for output messages, set to null to use Console</span></span>|

<span data-ttu-id="c3c01-338">**Tulajdonságok**</span><span class="sxs-lookup"><span data-stu-id="c3c01-338">**Properties**</span></span>

|<span data-ttu-id="c3c01-339">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c3c01-339">Property</span></span>|<span data-ttu-id="c3c01-340">Típus</span><span class="sxs-lookup"><span data-stu-id="c3c01-340">Type</span></span>|<span data-ttu-id="c3c01-341">Leírás</span><span class="sxs-lookup"><span data-stu-id="c3c01-341">Description</span></span>|
|--------|----|-----------|
|<span data-ttu-id="c3c01-342">AlgebraPath</span><span class="sxs-lookup"><span data-stu-id="c3c01-342">AlgebraPath</span></span>|<span data-ttu-id="c3c01-343">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-343">string</span></span>|<span data-ttu-id="c3c01-344">A fájl elérési útját algebra (algebra fájl egyike a fordítási eredmények)</span><span class="sxs-lookup"><span data-stu-id="c3c01-344">The path to algebra file (algebra file is one of the compilation results)</span></span>|
|<span data-ttu-id="c3c01-345">CodeBehindReferences</span><span class="sxs-lookup"><span data-stu-id="c3c01-345">CodeBehindReferences</span></span>|<span data-ttu-id="c3c01-346">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-346">string</span></span>|<span data-ttu-id="c3c01-347">Ha a parancsfájl hivatkozások további kód tartozik, adja meg az elérési utak határolt ";"</span><span class="sxs-lookup"><span data-stu-id="c3c01-347">If the script has additional code behind references, specify the paths separated with ';'</span></span>|
|<span data-ttu-id="c3c01-348">CppSdkDir</span><span class="sxs-lookup"><span data-stu-id="c3c01-348">CppSdkDir</span></span>|<span data-ttu-id="c3c01-349">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-349">string</span></span>|<span data-ttu-id="c3c01-350">CppSDK könyvtár</span><span class="sxs-lookup"><span data-stu-id="c3c01-350">CppSDK directory</span></span>|
|<span data-ttu-id="c3c01-351">CurrentDir</span><span class="sxs-lookup"><span data-stu-id="c3c01-351">CurrentDir</span></span>|<span data-ttu-id="c3c01-352">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-352">string</span></span>|<span data-ttu-id="c3c01-353">Aktuális könyvtárhoz</span><span class="sxs-lookup"><span data-stu-id="c3c01-353">Current directory</span></span>|
|<span data-ttu-id="c3c01-354">DataRoot</span><span class="sxs-lookup"><span data-stu-id="c3c01-354">DataRoot</span></span>|<span data-ttu-id="c3c01-355">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-355">string</span></span>|<span data-ttu-id="c3c01-356">Adatok elérési útjának gyökeréhez</span><span class="sxs-lookup"><span data-stu-id="c3c01-356">Data root path</span></span>|
|<span data-ttu-id="c3c01-357">DebuggerMailPath</span><span class="sxs-lookup"><span data-stu-id="c3c01-357">DebuggerMailPath</span></span>|<span data-ttu-id="c3c01-358">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-358">string</span></span>|<span data-ttu-id="c3c01-359">A hibakereső mailslot elérési útja</span><span class="sxs-lookup"><span data-stu-id="c3c01-359">The path to debugger mailslot</span></span>|
|<span data-ttu-id="c3c01-360">GenerateUdoRedirect</span><span class="sxs-lookup"><span data-stu-id="c3c01-360">GenerateUdoRedirect</span></span>|<span data-ttu-id="c3c01-361">logikai érték</span><span class="sxs-lookup"><span data-stu-id="c3c01-361">bool</span></span>|<span data-ttu-id="c3c01-362">Ha azt szeretné, hogy készítése a szerelvény betöltése átirányítási felülbírálása config</span><span class="sxs-lookup"><span data-stu-id="c3c01-362">If we want to generate assembly loading redirection override config</span></span>|
|<span data-ttu-id="c3c01-363">HasCodeBehind</span><span class="sxs-lookup"><span data-stu-id="c3c01-363">HasCodeBehind</span></span>|<span data-ttu-id="c3c01-364">logikai érték</span><span class="sxs-lookup"><span data-stu-id="c3c01-364">bool</span></span>|<span data-ttu-id="c3c01-365">Ha a parancsfájl kód tartozik.</span><span class="sxs-lookup"><span data-stu-id="c3c01-365">If the script has code behind</span></span>|
|<span data-ttu-id="c3c01-366">InputDir</span><span class="sxs-lookup"><span data-stu-id="c3c01-366">InputDir</span></span>|<span data-ttu-id="c3c01-367">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-367">string</span></span>|<span data-ttu-id="c3c01-368">A bemeneti adatok könyvtár</span><span class="sxs-lookup"><span data-stu-id="c3c01-368">Directory for input data</span></span>|
|<span data-ttu-id="c3c01-369">MessagePath</span><span class="sxs-lookup"><span data-stu-id="c3c01-369">MessagePath</span></span>|<span data-ttu-id="c3c01-370">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-370">string</span></span>|<span data-ttu-id="c3c01-371">Üzenet memóriakép fájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="c3c01-371">Message dump file path</span></span>|
|<span data-ttu-id="c3c01-372">OutputDir</span><span class="sxs-lookup"><span data-stu-id="c3c01-372">OutputDir</span></span>|<span data-ttu-id="c3c01-373">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-373">string</span></span>|<span data-ttu-id="c3c01-374">A kimeneti adatok könyvtár</span><span class="sxs-lookup"><span data-stu-id="c3c01-374">Directory for output data</span></span>|
|<span data-ttu-id="c3c01-375">Párhuzamos végrehajtás</span><span class="sxs-lookup"><span data-stu-id="c3c01-375">Parallelism</span></span>|<span data-ttu-id="c3c01-376">int</span><span class="sxs-lookup"><span data-stu-id="c3c01-376">int</span></span>|<span data-ttu-id="c3c01-377">Párhuzamossági a algebra futtatásához</span><span class="sxs-lookup"><span data-stu-id="c3c01-377">Parallelism to run the algebra</span></span>|
|<span data-ttu-id="c3c01-378">ParentPid</span><span class="sxs-lookup"><span data-stu-id="c3c01-378">ParentPid</span></span>|<span data-ttu-id="c3c01-379">int</span><span class="sxs-lookup"><span data-stu-id="c3c01-379">int</span></span>|<span data-ttu-id="c3c01-380">Azonosítója (PID), amelyen a szolgáltatás való kilépéshez, figyeli a szülő értéke 0, vagy negatív figyelmen kívül hagyása</span><span class="sxs-lookup"><span data-stu-id="c3c01-380">PID of the parent on which the service monitors to exit, set to 0 or negative to ignore</span></span>|
|<span data-ttu-id="c3c01-381">ResultPath</span><span class="sxs-lookup"><span data-stu-id="c3c01-381">ResultPath</span></span>|<span data-ttu-id="c3c01-382">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-382">string</span></span>|<span data-ttu-id="c3c01-383">Eredmény memóriakép fájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="c3c01-383">Result dump file path</span></span>|
|<span data-ttu-id="c3c01-384">RuntimeDir</span><span class="sxs-lookup"><span data-stu-id="c3c01-384">RuntimeDir</span></span>|<span data-ttu-id="c3c01-385">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-385">string</span></span>|<span data-ttu-id="c3c01-386">Futásidejű könyvtár</span><span class="sxs-lookup"><span data-stu-id="c3c01-386">Runtime directory</span></span>|
|<span data-ttu-id="c3c01-387">scriptPath</span><span class="sxs-lookup"><span data-stu-id="c3c01-387">ScriptPath</span></span>|<span data-ttu-id="c3c01-388">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-388">string</span></span>|<span data-ttu-id="c3c01-389">Hol találhatók a parancsprogram</span><span class="sxs-lookup"><span data-stu-id="c3c01-389">Where to find the script</span></span>|
|<span data-ttu-id="c3c01-390">Sekély</span><span class="sxs-lookup"><span data-stu-id="c3c01-390">Shallow</span></span>|<span data-ttu-id="c3c01-391">logikai érték</span><span class="sxs-lookup"><span data-stu-id="c3c01-391">bool</span></span>|<span data-ttu-id="c3c01-392">Vagy nem a fordítási sekély</span><span class="sxs-lookup"><span data-stu-id="c3c01-392">Shallow compile or not</span></span>|
|<span data-ttu-id="c3c01-393">TempDir</span><span class="sxs-lookup"><span data-stu-id="c3c01-393">TempDir</span></span>|<span data-ttu-id="c3c01-394">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-394">string</span></span>|<span data-ttu-id="c3c01-395">Ideiglenes könyvtár</span><span class="sxs-lookup"><span data-stu-id="c3c01-395">Temp directory</span></span>|
|<span data-ttu-id="c3c01-396">UseDataBase</span><span class="sxs-lookup"><span data-stu-id="c3c01-396">UseDataBase</span></span>|<span data-ttu-id="c3c01-397">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-397">string</span></span>|<span data-ttu-id="c3c01-398">Adja meg az adatbázishoz való használatra ideiglenes szerelvények regisztrálásakor, alapértelmezés szerint fő mögötti kódban</span><span class="sxs-lookup"><span data-stu-id="c3c01-398">Specify the database to use for code behind temporary assembly registration, master by default</span></span>|
|<span data-ttu-id="c3c01-399">WorkDir</span><span class="sxs-lookup"><span data-stu-id="c3c01-399">WorkDir</span></span>|<span data-ttu-id="c3c01-400">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c3c01-400">string</span></span>|<span data-ttu-id="c3c01-401">Előnyben részesített munkakönyvtár</span><span class="sxs-lookup"><span data-stu-id="c3c01-401">Preferred working directory</span></span>|


<span data-ttu-id="c3c01-402">**Módszer**</span><span class="sxs-lookup"><span data-stu-id="c3c01-402">**Method**</span></span>

|<span data-ttu-id="c3c01-403">Módszer</span><span class="sxs-lookup"><span data-stu-id="c3c01-403">Method</span></span>|<span data-ttu-id="c3c01-404">Leírás</span><span class="sxs-lookup"><span data-stu-id="c3c01-404">Description</span></span>|<span data-ttu-id="c3c01-405">térjen vissza</span><span class="sxs-lookup"><span data-stu-id="c3c01-405">Return</span></span>|<span data-ttu-id="c3c01-406">Paraméter</span><span class="sxs-lookup"><span data-stu-id="c3c01-406">Parameter</span></span>|
|------|-----------|------|---------|
|<span data-ttu-id="c3c01-407">nyilvános bool DoCompile()</span><span class="sxs-lookup"><span data-stu-id="c3c01-407">public bool DoCompile()</span></span>|<span data-ttu-id="c3c01-408">A U-SQL parancsfájl összeállítása</span><span class="sxs-lookup"><span data-stu-id="c3c01-408">Compile the U-SQL script</span></span>|<span data-ttu-id="c3c01-409">Sikeres művelet igaz</span><span class="sxs-lookup"><span data-stu-id="c3c01-409">True on success</span></span>| |
|<span data-ttu-id="c3c01-410">nyilvános bool DoExec()</span><span class="sxs-lookup"><span data-stu-id="c3c01-410">public bool DoExec()</span></span>|<span data-ttu-id="c3c01-411">A lefordított eredmény végrehajtása</span><span class="sxs-lookup"><span data-stu-id="c3c01-411">Execute the compiled result</span></span>|<span data-ttu-id="c3c01-412">Sikeres művelet igaz</span><span class="sxs-lookup"><span data-stu-id="c3c01-412">True on success</span></span>| |
|<span data-ttu-id="c3c01-413">nyilvános bool DoRun()</span><span class="sxs-lookup"><span data-stu-id="c3c01-413">public bool DoRun()</span></span>|<span data-ttu-id="c3c01-414">Futtassa a U-SQL parancsfájlt (fordítási + végrehajtás)</span><span class="sxs-lookup"><span data-stu-id="c3c01-414">Run the U-SQL script (Compile + Execute)</span></span>|<span data-ttu-id="c3c01-415">Sikeres művelet igaz</span><span class="sxs-lookup"><span data-stu-id="c3c01-415">True on success</span></span>| |
|<span data-ttu-id="c3c01-416">nyilvános bool IsValidRuntimeDir (karakterlánc elérési út)</span><span class="sxs-lookup"><span data-stu-id="c3c01-416">public bool IsValidRuntimeDir(string path)</span></span>|<span data-ttu-id="c3c01-417">Annak ellenőrzése, hogy a megadott elérési út érvényes futásidejű elérési útja</span><span class="sxs-lookup"><span data-stu-id="c3c01-417">Check if the given path is valid runtime path</span></span>|<span data-ttu-id="c3c01-418">Igaz a érvényes</span><span class="sxs-lookup"><span data-stu-id="c3c01-418">True for valid</span></span>|<span data-ttu-id="c3c01-419">Futásidejű könyvtár elérési útja</span><span class="sxs-lookup"><span data-stu-id="c3c01-419">The path of runtime directory</span></span>|


## <a name="faq-about-common-issue"></a><span data-ttu-id="c3c01-420">Közös problémával kapcsolatos gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="c3c01-420">FAQ about common issue</span></span>

### <a name="error-1"></a><span data-ttu-id="c3c01-421">1. hiba:</span><span class="sxs-lookup"><span data-stu-id="c3c01-421">Error 1:</span></span>
<span data-ttu-id="c3c01-422">E_CSC_SYSTEM_INTERNAL: Belső hiba!</span><span class="sxs-lookup"><span data-stu-id="c3c01-422">E_CSC_SYSTEM_INTERNAL: Internal error!</span></span> <span data-ttu-id="c3c01-423">Nem sikerült betölteni a fájl vagy szerelvény "ScopeEngineManaged.dll" vagy annak valamelyik függősége.</span><span class="sxs-lookup"><span data-stu-id="c3c01-423">Could not load file or assembly 'ScopeEngineManaged.dll' or one of its dependencies.</span></span> <span data-ttu-id="c3c01-424">A megadott modul nem található.</span><span class="sxs-lookup"><span data-stu-id="c3c01-424">The specified module could not be found.</span></span>

<span data-ttu-id="c3c01-425">Ellenőrizze a következőket:</span><span class="sxs-lookup"><span data-stu-id="c3c01-425">Please check the following:</span></span>

- <span data-ttu-id="c3c01-426">Győződjön meg arról, hogy x64 környezetben.</span><span class="sxs-lookup"><span data-stu-id="c3c01-426">Make sure you have x64 environment.</span></span> <span data-ttu-id="c3c01-427">A build célplatform és a tesztkörnyezet kell x64, tekintse meg **1. lépés: egység létrehozása a C# projekt és konfigurációs teszteléséhez** felett.</span><span class="sxs-lookup"><span data-stu-id="c3c01-427">The build target platform and the test environment should be x64, refer to **Step 1: Create C# unit test project and configuration** above.</span></span>
- <span data-ttu-id="c3c01-428">Ellenőrizze, hogy NugetPackage\build\runtime\ található összes függőség fájl munkakönyvtár projekthez másolta.</span><span class="sxs-lookup"><span data-stu-id="c3c01-428">Make sure you have copied all dependency files under NugetPackage\build\runtime\ to project working directory.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c3c01-429">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c3c01-429">Next steps</span></span>

* <span data-ttu-id="c3c01-430">A U-SQL nyelv megismerése: [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md) (Ismerkedés az Azure Data Lake Analytics U-SQL nyelvével).</span><span class="sxs-lookup"><span data-stu-id="c3c01-430">To learn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="c3c01-431">Diagnosztikai adatok naplózására, lásd: [diagnosztikai naplók elérése az Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="c3c01-431">To log diagnostics information, see [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).</span></span>
* <span data-ttu-id="c3c01-432">Egy összetettebb lekérdezés megtekintéséhez lásd: [Azure Data Lake Analytics használatával webhelyek naplóinak elemzése](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="c3c01-432">To see a more complex query, see [Analyze website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="c3c01-433">Feladat részleteinek megtekintése: [használata feladat böngésző és az Azure Data Lake Analytics-feladatok feladat megtekintése](data-lake-analytics-data-lake-tools-view-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="c3c01-433">To view job details, see [Use Job Browser and Job View for Azure Data Lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md).</span></span>
* <span data-ttu-id="c3c01-434">Ha szeretné használni a vertex végrehajtási nézetet, lásd [a Vertex végrehajtási nézetet használja a Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span><span class="sxs-lookup"><span data-stu-id="c3c01-434">To use the vertex execution view, see [Use the Vertex Execution View in Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span></span>
