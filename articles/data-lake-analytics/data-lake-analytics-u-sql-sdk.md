---
title: "aaaScale U-SQL helyi futtatása, és tesztelje az Azure Data Lake U-SQL SDK-val |} Microsoft Docs"
description: "Ismerje meg, hogy miként toouse Azure Data Lake U-SQL SDK tooscale U-SQL feladatok helyi futtatása és a parancssor és a helyi munkaállomáson alkalmazásprogramozási felületek."
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
ms.openlocfilehash: 2b0a16229789268e333f723ff6fc2c3efdc29905
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-u-sql-local-run-and-test-with-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="9a833-103">Skála U-SQL helyi futtatása és a teszt Azure Data Lake U-SQL-SDK-val</span><span class="sxs-lookup"><span data-stu-id="9a833-103">Scale U-SQL local run and test with Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="9a833-104">U-SQL parancsfájl fejlesztésekor közös toorun és tesztelhet U-SQL parancsfájl helyi előtt küldje el toocloud.</span><span class="sxs-lookup"><span data-stu-id="9a833-104">When developing U-SQL script, it is common toorun and test U-SQL script locally before submit it toocloud.</span></span> <span data-ttu-id="9a833-105">Azure Data Lake biztosít az Azure Data Lake U-SQL SDK hívása ebben a forgatókönyvben a Nuget-csomagot, keresztül, amely könnyedén méretezhető U-SQL helyi futtatásával és a vizsgálat.</span><span class="sxs-lookup"><span data-stu-id="9a833-105">Azure Data Lake provides a Nuget package called Azure Data Lake U-SQL SDK for this scenario, through which you can easily scale U-SQL local run and test.</span></span> <span data-ttu-id="9a833-106">A U-SQL (folyamatos integrációt) CI rendszer tooautomate hello fordítási és tesztelési tesztelést lehetséges toointegrate egyben.</span><span class="sxs-lookup"><span data-stu-id="9a833-106">It is also possible toointegrate this U-SQL test with CI (Continuous Integration) system tooautomate hello compile and test.</span></span>

<span data-ttu-id="9a833-107">Ha az Ön számára legfontosabb hogyan toomanually helyi futtatásához, és hibakeresési grafikus felhasználói Felülettel tooling U-SQL parancsfájlt, majd használható Azure Data Lake Tools for Visual Studio, amely.</span><span class="sxs-lookup"><span data-stu-id="9a833-107">If you care about how toomanually local run and debug U-SQL script with GUI tooling, then you can use Azure Data Lake Tools for Visual Studio for that.</span></span> <span data-ttu-id="9a833-108">Többet is megtudhat [Itt](data-lake-analytics-data-lake-tools-local-run.md).</span><span class="sxs-lookup"><span data-stu-id="9a833-108">You can learn more from [here](data-lake-analytics-data-lake-tools-local-run.md).</span></span>

## <a name="install-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="9a833-109">Telepítés Azure Data Lake U-SQL-SDK</span><span class="sxs-lookup"><span data-stu-id="9a833-109">Install Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="9a833-110">Azure Data Lake U-SQL SDK hello kaphat [Itt](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) a Nuget.org. És annak használata előtt meg kell toomake, az alábbiak szerint függőségeinek rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="9a833-110">You can get hello Azure Data Lake U-SQL SDK [here](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) on Nuget.org. And before using it, you need toomake sure you have dependencies as follows.</span></span>

### <a name="dependencies"></a><span data-ttu-id="9a833-111">Függőségek</span><span class="sxs-lookup"><span data-stu-id="9a833-111">Dependencies</span></span>

<span data-ttu-id="9a833-112">Data Lake U-SQL SDK hello hello függőségek a következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="9a833-112">hello Data Lake U-SQL SDK requires hello following dependencies:</span></span>

- <span data-ttu-id="9a833-113">[A Microsoft .NET-keretrendszer 4.6-os vagy újabb](https://www.microsoft.com/download/details.aspx?id=17851).</span><span class="sxs-lookup"><span data-stu-id="9a833-113">[Microsoft .NET Framework 4.6 or newer](https://www.microsoft.com/download/details.aspx?id=17851).</span></span>
- <span data-ttu-id="9a833-114">Microsoft Visual C++ 14 és a Windows SDK 10.0.10240.0 vagy újabb (amely más néven CppSDK ebben a cikkben).</span><span class="sxs-lookup"><span data-stu-id="9a833-114">Microsoft Visual C++ 14 and Windows SDK 10.0.10240.0 or newer (which is called CppSDK in this article).</span></span> <span data-ttu-id="9a833-115">Két módon tooget CppSDK van:</span><span class="sxs-lookup"><span data-stu-id="9a833-115">There are two ways tooget CppSDK:</span></span>

    - <span data-ttu-id="9a833-116">Telepítés [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou).</span><span class="sxs-lookup"><span data-stu-id="9a833-116">Install [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou).</span></span> <span data-ttu-id="9a833-117">Hello Program Files mappában – például a C:\Program Files (x86) \Windows Kits\10\ kell egy \Windows Kits\10 mappát.</span><span class="sxs-lookup"><span data-stu-id="9a833-117">You'll have a \Windows Kits\10 folder under hello Program Files folder--for example, C:\Program Files (x86)\Windows Kits\10\.</span></span> <span data-ttu-id="9a833-118">Windows 10-es SDK verzió hello \Windows Kits\10\Lib is találhat.</span><span class="sxs-lookup"><span data-stu-id="9a833-118">You'll also find hello Windows 10 SDK version under \Windows Kits\10\Lib.</span></span> <span data-ttu-id="9a833-119">Ha nem látja ezeket a mappákat, telepítse újra a Visual Studio, és lehet, hogy tooselect hello Windows 10-es SDK hello telepítése során.</span><span class="sxs-lookup"><span data-stu-id="9a833-119">If you don’t see these folders, reinstall Visual Studio and be sure tooselect hello Windows 10 SDK during hello installation.</span></span> <span data-ttu-id="9a833-120">Ha ez a Visual Studio telepítve van, hello U-SQL helyi fordító megkeresi azt automatikusan.</span><span class="sxs-lookup"><span data-stu-id="9a833-120">If you have this installed with Visual Studio, hello U-SQL local compiler will find it automatically.</span></span>

    ![A Data Lake Tools for Visual Studio helyi futtatási Windows 10-es SDK](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

    - <span data-ttu-id="9a833-122">Telepítés [a Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs).</span><span class="sxs-lookup"><span data-stu-id="9a833-122">Install [Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs).</span></span> <span data-ttu-id="9a833-123">Hello előre csomagolt Visual C++ és a Windows SDK fájlokra a C:\Program Files (x86) \Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK találja.</span><span class="sxs-lookup"><span data-stu-id="9a833-123">You can find hello prepackaged Visual C++ and Windows SDK files at C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK.</span></span> <span data-ttu-id="9a833-124">Ebben az esetben hello U-SQL helyi fordító nem található hello függőségek automatikusan.</span><span class="sxs-lookup"><span data-stu-id="9a833-124">In this case, hello U-SQL local compiler cannot find hello dependencies automatically.</span></span> <span data-ttu-id="9a833-125">Az toospecify hello CppSDK elérési út van szüksége.</span><span class="sxs-lookup"><span data-stu-id="9a833-125">You need toospecify hello CppSDK path for it.</span></span> <span data-ttu-id="9a833-126">Hello fájlok tooanother helyre másolja, vagy használja a jelenlegi állapotában.</span><span class="sxs-lookup"><span data-stu-id="9a833-126">You can either copy hello files tooanother location or use it as is.</span></span>

## <a name="understand-basic-concepts"></a><span data-ttu-id="9a833-127">Alapvető fogalmak megismerése</span><span class="sxs-lookup"><span data-stu-id="9a833-127">Understand basic concepts</span></span>

### <a name="data-root"></a><span data-ttu-id="9a833-128">Adatgyökerében</span><span class="sxs-lookup"><span data-stu-id="9a833-128">Data root</span></span>

<span data-ttu-id="9a833-129">hello adatok-gyökérmappája "helyi tároló" hello helyi számítási fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="9a833-129">hello data-root folder is a "local store" for hello local compute account.</span></span> <span data-ttu-id="9a833-130">Egyenértékű toohello Azure Data Lake Store-fiók egy Data Lake Analytics-fiók is.</span><span class="sxs-lookup"><span data-stu-id="9a833-130">It's equivalent toohello Azure Data Lake Store account of a Data Lake Analytics account.</span></span> <span data-ttu-id="9a833-131">Váltás tooa különböző adatok-gyökérmappája hasonlóan tooa különböző store-fiók vált.</span><span class="sxs-lookup"><span data-stu-id="9a833-131">Switching tooa different data-root folder is just like switching tooa different store account.</span></span> <span data-ttu-id="9a833-132">Ha azt szeretné, hogy tooaccess hagyományosan megosztott adatokat különböző adatgyökerében mappákkal, a parancsfájlok abszolút elérési utakat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="9a833-132">If you want tooaccess commonly shared data with different data-root folders, you must use absolute paths in your scripts.</span></span> <span data-ttu-id="9a833-133">Vagy hozzon létre fájlt rendszer szimbolikus hivatkozásokat (például **mklink** NTFS) hello adatgyökerében mappa toopoint toohello a megosztott adatok.</span><span class="sxs-lookup"><span data-stu-id="9a833-133">Or, create file system symbolic links (for example, **mklink** on NTFS) under hello data-root folder toopoint toohello shared data.</span></span>

<span data-ttu-id="9a833-134">hello adatgyökerében mappa a következőkre használható:</span><span class="sxs-lookup"><span data-stu-id="9a833-134">hello data-root folder is used to:</span></span>

- <span data-ttu-id="9a833-135">Helyi metaadatainak, például adatbázisok, táblák, táblaértékű függvények (TVFs) és szerelvényeket tárolják.</span><span class="sxs-lookup"><span data-stu-id="9a833-135">Store local metadata, including databases, tables, table-valued functions (TVFs), and assemblies.</span></span>
- <span data-ttu-id="9a833-136">Hello bemeneti és kimeneti elérési utak relatív elérési utak a U-SQL definiált kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="9a833-136">Look up hello input and output paths that are defined as relative paths in U-SQL.</span></span> <span data-ttu-id="9a833-137">Relatív elérési utak révén könnyebben toodeploy a U-SQL-projektek tooAzure.</span><span class="sxs-lookup"><span data-stu-id="9a833-137">Using relative paths makes it easier toodeploy your U-SQL projects tooAzure.</span></span>

### <a name="file-path-in-u-sql"></a><span data-ttu-id="9a833-138">U-SQL-fájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="9a833-138">File path in U-SQL</span></span>

<span data-ttu-id="9a833-139">U-SQL-parancsfájlok használhatja relatív elérési út és a helyi abszolút elérési utat.</span><span class="sxs-lookup"><span data-stu-id="9a833-139">You can use both a relative path and a local absolute path in U-SQL scripts.</span></span> <span data-ttu-id="9a833-140">hello relatív elérési út relatív toohello megadott adatok-gyökérmappa elérési útja.</span><span class="sxs-lookup"><span data-stu-id="9a833-140">hello relative path is relative toohello specified data-root folder path.</span></span> <span data-ttu-id="9a833-141">Azt javasoljuk, hogy azt használja "/", hello elérési útját elválasztójel toomake hello kiszolgálóoldali kompatibilis a parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="9a833-141">We recommend that you use "/" as hello path separator toomake your scripts compatible with hello server side.</span></span> <span data-ttu-id="9a833-142">Az alábbiakban néhány olyan relatív elérési útja és az egyenértékű abszolút elérési utakat.</span><span class="sxs-lookup"><span data-stu-id="9a833-142">Here are some examples of relative paths and their equivalent absolute paths.</span></span> <span data-ttu-id="9a833-143">Ezekben a példákban C:\LocalRunDataRoot hello adatok-gyökérmappájába.</span><span class="sxs-lookup"><span data-stu-id="9a833-143">In these examples, C:\LocalRunDataRoot is hello data-root folder.</span></span>

|<span data-ttu-id="9a833-144">Relatív elérési útja</span><span class="sxs-lookup"><span data-stu-id="9a833-144">Relative path</span></span>|<span data-ttu-id="9a833-145">Abszolút elérési útja</span><span class="sxs-lookup"><span data-stu-id="9a833-145">Absolute path</span></span>|
|-------------|-------------|
|<span data-ttu-id="9a833-146">/ABC/DEF/input.csv</span><span class="sxs-lookup"><span data-stu-id="9a833-146">/abc/def/input.csv</span></span> |<span data-ttu-id="9a833-147">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="9a833-147">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="9a833-148">ABC/DEF/input.csv</span><span class="sxs-lookup"><span data-stu-id="9a833-148">abc/def/input.csv</span></span>  |<span data-ttu-id="9a833-149">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="9a833-149">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="9a833-150">D:/ABC/DEF/input.csv</span><span class="sxs-lookup"><span data-stu-id="9a833-150">D:/abc/def/input.csv</span></span> |<span data-ttu-id="9a833-151">D:\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="9a833-151">D:\abc\def\input.csv</span></span>|

### <a name="working-directory"></a><span data-ttu-id="9a833-152">Munkakönyvtár</span><span class="sxs-lookup"><span data-stu-id="9a833-152">Working directory</span></span>

<span data-ttu-id="9a833-153">Hello U-SQL parancsfájl helyben fut, amikor egy működő könyvtárba aktuális futtatási könyvtárának a fordítás során jön létre.</span><span class="sxs-lookup"><span data-stu-id="9a833-153">When running hello U-SQL script locally, a working directory is created during compilation under current running directory.</span></span> <span data-ttu-id="9a833-154">Ezenkívül toohello fordítási kimenete, hello szükséges futásidejű fájlok helyi végrehajtásra lesznek árnyékmásolat másolt toothis munkakönyvtár.</span><span class="sxs-lookup"><span data-stu-id="9a833-154">In addition toohello compilation outputs, hello needed runtime files for local execution will be shadow copied toothis working directory.</span></span> <span data-ttu-id="9a833-155">directory legfelső szintű munkamappa hello "ScopeWorkDir" nevezik, és a munkakönyvtár hello hello fájlok a következők:</span><span class="sxs-lookup"><span data-stu-id="9a833-155">hello working directory root folder is called "ScopeWorkDir" and hello files under hello working directory are as follows:</span></span>

|<span data-ttu-id="9a833-156">Könyvtár vagy fájl</span><span class="sxs-lookup"><span data-stu-id="9a833-156">Directory/file</span></span>|<span data-ttu-id="9a833-157">Könyvtár vagy fájl</span><span class="sxs-lookup"><span data-stu-id="9a833-157">Directory/file</span></span>|<span data-ttu-id="9a833-158">Könyvtár vagy fájl</span><span class="sxs-lookup"><span data-stu-id="9a833-158">Directory/file</span></span>|<span data-ttu-id="9a833-159">Meghatározás</span><span class="sxs-lookup"><span data-stu-id="9a833-159">Definition</span></span>|<span data-ttu-id="9a833-160">Leírás</span><span class="sxs-lookup"><span data-stu-id="9a833-160">Description</span></span>|
|--------------|--------------|--------------|----------|-----------|
|<span data-ttu-id="9a833-161">C6A101DDCB470506</span><span class="sxs-lookup"><span data-stu-id="9a833-161">C6A101DDCB470506</span></span>| | |<span data-ttu-id="9a833-162">Kivonat karakterláncot futtatókörnyezet-verzió</span><span class="sxs-lookup"><span data-stu-id="9a833-162">Hash string of runtime version</span></span>|<span data-ttu-id="9a833-163">A helyi végrehajtásához szükséges futásidejű fájlok árnyékmásolatát</span><span class="sxs-lookup"><span data-stu-id="9a833-163">Shadow copy of runtime files needed for local execution</span></span>|
| |<span data-ttu-id="9a833-164">Script_66AE4909AA0ED06C</span><span class="sxs-lookup"><span data-stu-id="9a833-164">Script_66AE4909AA0ED06C</span></span>| |<span data-ttu-id="9a833-165">Parancsfájl neve + kivonat karakterláncot a parancsprogram elérési útja</span><span class="sxs-lookup"><span data-stu-id="9a833-165">Script name + hash string of script path</span></span>|<span data-ttu-id="9a833-166">Fordítási kimenetek és végrehajtási lépés a naplózás</span><span class="sxs-lookup"><span data-stu-id="9a833-166">Compilation outputs and execution step logging</span></span>|
| | |<span data-ttu-id="9a833-167">\_parancsfájl\_.abr</span><span class="sxs-lookup"><span data-stu-id="9a833-167">\_script\_.abr</span></span>|<span data-ttu-id="9a833-168">A fordítóprogram kimenetének</span><span class="sxs-lookup"><span data-stu-id="9a833-168">Compiler output</span></span>|<span data-ttu-id="9a833-169">Algebra fájl</span><span class="sxs-lookup"><span data-stu-id="9a833-169">Algebra file</span></span>|
| | |<span data-ttu-id="9a833-170">\_ScopeCodeGen\_. *</span><span class="sxs-lookup"><span data-stu-id="9a833-170">\_ScopeCodeGen\_.*</span></span>|<span data-ttu-id="9a833-171">A fordítóprogram kimenetének</span><span class="sxs-lookup"><span data-stu-id="9a833-171">Compiler output</span></span>|<span data-ttu-id="9a833-172">A felügyelt kód jön létre</span><span class="sxs-lookup"><span data-stu-id="9a833-172">Generated managed code</span></span>|
| | |<span data-ttu-id="9a833-173">\_ScopeCodeGenEngine\_. *</span><span class="sxs-lookup"><span data-stu-id="9a833-173">\_ScopeCodeGenEngine\_.*</span></span>|<span data-ttu-id="9a833-174">A fordítóprogram kimenetének</span><span class="sxs-lookup"><span data-stu-id="9a833-174">Compiler output</span></span>|<span data-ttu-id="9a833-175">Létrehozott natív kód</span><span class="sxs-lookup"><span data-stu-id="9a833-175">Generated native code</span></span>|
| | |<span data-ttu-id="9a833-176">hivatkozott szerelvények</span><span class="sxs-lookup"><span data-stu-id="9a833-176">referenced assemblies</span></span>|<span data-ttu-id="9a833-177">Szerelvényre mutató hivatkozás</span><span class="sxs-lookup"><span data-stu-id="9a833-177">Assembly reference</span></span>|<span data-ttu-id="9a833-178">Hivatkozott szerelvény fájlok</span><span class="sxs-lookup"><span data-stu-id="9a833-178">Referenced assembly files</span></span>|
| | |<span data-ttu-id="9a833-179">deployed_resources</span><span class="sxs-lookup"><span data-stu-id="9a833-179">deployed_resources</span></span>|<span data-ttu-id="9a833-180">Erőforrások telepítése</span><span class="sxs-lookup"><span data-stu-id="9a833-180">Resource deployment</span></span>|<span data-ttu-id="9a833-181">Erőforrás-telepítési fájlok</span><span class="sxs-lookup"><span data-stu-id="9a833-181">Resource deployment files</span></span>|
| | |<span data-ttu-id="9a833-182">xxxxxxxx.xxx[1..n]\_\*. *</span><span class="sxs-lookup"><span data-stu-id="9a833-182">xxxxxxxx.xxx[1..n]\_\*.*</span></span>|<span data-ttu-id="9a833-183">Végrehajtási napló</span><span class="sxs-lookup"><span data-stu-id="9a833-183">Execution log</span></span>|<span data-ttu-id="9a833-184">A végrehajtási lépések napló</span><span class="sxs-lookup"><span data-stu-id="9a833-184">Log of execution steps</span></span>|


## <a name="use-hello-sdk-from-hello-command-line"></a><span data-ttu-id="9a833-185">Hello parancssorból SDK hello használata</span><span class="sxs-lookup"><span data-stu-id="9a833-185">Use hello SDK from hello command line</span></span>

### <a name="command-line-interface-of-hello-helper-application"></a><span data-ttu-id="9a833-186">Parancssori felületének hello segédalkalmazással.</span><span class="sxs-lookup"><span data-stu-id="9a833-186">Command-line interface of hello helper application</span></span>

<span data-ttu-id="9a833-187">Az SDK directory\build\runtime LocalRunHelper.exe hello parancssori segítő alkalmazással, amely felületek helyi futtatási általánosan használt hello toomost funkciókat biztosít.</span><span class="sxs-lookup"><span data-stu-id="9a833-187">Under SDK directory\build\runtime, LocalRunHelper.exe is hello command-line helper application that provides interfaces toomost of hello commonly used local-run functions.</span></span> <span data-ttu-id="9a833-188">Ne feledje, hogy mindkét hello parancs hello argumentum kapcsolók a következők kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="9a833-188">Note that both hello command and hello argument switches are case-sensitive.</span></span> <span data-ttu-id="9a833-189">tooinvoke azt:</span><span class="sxs-lookup"><span data-stu-id="9a833-189">tooinvoke it:</span></span>

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

<span data-ttu-id="9a833-190">Paraméterek nélkül, vagy a hello fusson a LocalRunHelper.exe **súgó** tooshow hello súgóinformációval váltani:</span><span class="sxs-lookup"><span data-stu-id="9a833-190">Run LocalRunHelper.exe without arguments or with hello **help** switch tooshow hello help information:</span></span>

    > LocalRunHelper.exe help

        Command 'help' :  Show usage information
        Command 'compile' :  Compile hello script
        Required Arguments :
            -Script param
                    Script File Path
        Optional Arguments :
            -Shallow [default value 'False']
                    Shallow compile

<span data-ttu-id="9a833-191">A hello segítő információk:</span><span class="sxs-lookup"><span data-stu-id="9a833-191">In hello help information:</span></span>

-  <span data-ttu-id="9a833-192">**A parancs** által biztosított hello parancs nevét.</span><span class="sxs-lookup"><span data-stu-id="9a833-192">**Command** gives hello command’s name.</span></span>  
-  <span data-ttu-id="9a833-193">**Kötelező argumentum** , meg kell adni argumentumait sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="9a833-193">**Required Argument** lists arguments that must be supplied.</span></span>  
-  <span data-ttu-id="9a833-194">**Nem kötelező argumentumában** sorolja fel, ez nem kötelező megadni, alapértelmezett értékekkel.</span><span class="sxs-lookup"><span data-stu-id="9a833-194">**Optional Argument** lists arguments that are optional, with default values.</span></span>  <span data-ttu-id="9a833-195">Nem kötelező, logikai típusú argumentumokat nem kell a paramétereket, és azok megjelenésének negatív tootheir alapértelmezett értéket jelenti.</span><span class="sxs-lookup"><span data-stu-id="9a833-195">Optional Boolean arguments don’t have parameters, and their appearances mean negative tootheir default value.</span></span>

### <a name="return-value-and-logging"></a><span data-ttu-id="9a833-196">Visszatérési érték és a naplózás</span><span class="sxs-lookup"><span data-stu-id="9a833-196">Return value and logging</span></span>

<span data-ttu-id="9a833-197">hello segítő alkalmazással adja vissza **0** a sikeres és **-1** sikertelen.</span><span class="sxs-lookup"><span data-stu-id="9a833-197">hello helper application returns **0** for success and **-1** for failure.</span></span> <span data-ttu-id="9a833-198">Alapértelmezés szerint a hello segítő küld minden üzenetek toohello jelenlegi konzolt.</span><span class="sxs-lookup"><span data-stu-id="9a833-198">By default, hello helper sends all messages toohello current console.</span></span> <span data-ttu-id="9a833-199">Azonban a legtöbb hello parancs támogatja hello **- MessageOut naplófájl_elérési_útja** nem kötelező argumentum, amely átirányítja a hello kimenete tooa naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="9a833-199">However, most of hello commands support hello **-MessageOut path_to_log_file** optional argument that redirects hello outputs tooa log file.</span></span>

### <a name="environment-variable-configuring"></a><span data-ttu-id="9a833-200">Környezeti változó beállítása</span><span class="sxs-lookup"><span data-stu-id="9a833-200">Environment variable configuring</span></span>

<span data-ttu-id="9a833-201">U-SQL helyi futnia kell a megadott adatok legfelső szintű helyi storage-fiók, valamint egy adott CppSDK függőségek elérési úton.</span><span class="sxs-lookup"><span data-stu-id="9a833-201">U-SQL local run needs a specified data root as local storage account, as well as a specified CppSDK path for dependencies.</span></span> <span data-ttu-id="9a833-202">Mindkét hello argumentum parancssori vagy beállított környezeti változóban végezhető el őket.</span><span class="sxs-lookup"><span data-stu-id="9a833-202">You can both set hello argument in command-line or set environment variable for them.</span></span>

- <span data-ttu-id="9a833-203">Set hello **SCOPE_CPP_SDK** környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="9a833-203">Set hello **SCOPE_CPP_SDK** environment variable.</span></span>

    <span data-ttu-id="9a833-204">Ha a Microsoft Visual C++ és hello Windows SDK-t úgy, hogy a Data Lake Tools for Visual Studio telepítése, győződjön meg arról, hogy rendelkezik-e a következő mappa hello:</span><span class="sxs-lookup"><span data-stu-id="9a833-204">If you get Microsoft Visual C++ and hello Windows SDK by installing Data Lake Tools for Visual Studio, verify that you have hello following folder:</span></span>

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    <span data-ttu-id="9a833-205">Adja meg egy új környezeti változó neve **SCOPE_CPP_SDK** toopoint toothis könyvtár.</span><span class="sxs-lookup"><span data-stu-id="9a833-205">Define a new environment variable called **SCOPE_CPP_SDK** toopoint toothis directory.</span></span> <span data-ttu-id="9a833-206">Vagy másolja hello mappa toohello más helyre, és adja meg **SCOPE_CPP_SDK** , mint a.</span><span class="sxs-lookup"><span data-stu-id="9a833-206">Or copy hello folder toohello other location and specify **SCOPE_CPP_SDK** as that.</span></span>

    <span data-ttu-id="9a833-207">Továbbá toosetting hello környezeti változóban, megadhatja a hello **- CppSDK** használatakor hello parancssori argumentum.</span><span class="sxs-lookup"><span data-stu-id="9a833-207">In addition toosetting hello environment variable, you can specify hello **-CppSDK** argument when you're using hello command line.</span></span> <span data-ttu-id="9a833-208">Ennek az argumentumnak felülírja az alapértelmezett CppSDK környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="9a833-208">This argument overwrites your default CppSDK environment variable.</span></span>

- <span data-ttu-id="9a833-209">Set hello **LOCALRUN_DATAROOT** környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="9a833-209">Set hello **LOCALRUN_DATAROOT** environment variable.</span></span>

    <span data-ttu-id="9a833-210">Adja meg egy új környezeti változó neve **LOCALRUN_DATAROOT** toohello adatgyökerében mutat.</span><span class="sxs-lookup"><span data-stu-id="9a833-210">Define a new environment variable called **LOCALRUN_DATAROOT** that points toohello data root.</span></span>

    <span data-ttu-id="9a833-211">Továbbá toosetting hello környezeti változóban, megadhatja a hello **- DataRoot** hello adatgyökerében elérési útja, ha a parancssor használata argumentumot.</span><span class="sxs-lookup"><span data-stu-id="9a833-211">In addition toosetting hello environment variable, you can specify hello **-DataRoot** argument with hello data-root path when you're using a command line.</span></span> <span data-ttu-id="9a833-212">Ennek az argumentumnak felülírja az alapértelmezett adatgyökerében környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="9a833-212">This argument overwrites your default data-root environment variable.</span></span> <span data-ttu-id="9a833-213">Tooadd kell ez argumentum tooevery a parancssor futtatja, hogy felülírhatja hello alapértelmezett adatgyökerében környezeti változó minden műveletnél.</span><span class="sxs-lookup"><span data-stu-id="9a833-213">You need tooadd this argument tooevery command line you're running so that you can overwrite hello default data-root environment variable for all operations.</span></span>

### <a name="sdk-command-line-usage-samples"></a><span data-ttu-id="9a833-214">SDK parancssori használati minták</span><span class="sxs-lookup"><span data-stu-id="9a833-214">SDK command line usage samples</span></span>

#### <a name="compile-and-run"></a><span data-ttu-id="9a833-215">Fordítás és futtatás</span><span class="sxs-lookup"><span data-stu-id="9a833-215">Compile and run</span></span>

<span data-ttu-id="9a833-216">Hello **futtatása** parancsnak toocompile hello parancsfájlt, és hajthat végre a lefordított eredmények.</span><span class="sxs-lookup"><span data-stu-id="9a833-216">hello **run** command is used toocompile hello script and then execute compiled results.</span></span> <span data-ttu-id="9a833-217">Parancssori argumentumok szerintiek kombinációja **fordítási** és **hajtható végre**.</span><span class="sxs-lookup"><span data-stu-id="9a833-217">Its command-line arguments are a combination of those from **compile** and **execute**.</span></span>

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

<span data-ttu-id="9a833-218">hello a következők nem kötelező argumentumainak **futtatása**:</span><span class="sxs-lookup"><span data-stu-id="9a833-218">hello following are optional arguments for **run**:</span></span>


|<span data-ttu-id="9a833-219">Argumentum</span><span class="sxs-lookup"><span data-stu-id="9a833-219">Argument</span></span>|<span data-ttu-id="9a833-220">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="9a833-220">Default value</span></span>|<span data-ttu-id="9a833-221">Leírás</span><span class="sxs-lookup"><span data-stu-id="9a833-221">Description</span></span>|
|--------|-------------|-----------|
|<span data-ttu-id="9a833-222">-Háttérkódban</span><span class="sxs-lookup"><span data-stu-id="9a833-222">-CodeBehind</span></span>|<span data-ttu-id="9a833-223">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="9a833-223">False</span></span>|<span data-ttu-id="9a833-224">hello parancsfájl .cs kód tartozik.</span><span class="sxs-lookup"><span data-stu-id="9a833-224">hello script has .cs code behind</span></span>|
|<span data-ttu-id="9a833-225">-CppSDK</span><span class="sxs-lookup"><span data-stu-id="9a833-225">-CppSDK</span></span>| |<span data-ttu-id="9a833-226">CppSDK könyvtár</span><span class="sxs-lookup"><span data-stu-id="9a833-226">CppSDK Directory</span></span>|
|<span data-ttu-id="9a833-227">-DataRoot</span><span class="sxs-lookup"><span data-stu-id="9a833-227">-DataRoot</span></span>| <span data-ttu-id="9a833-228">DataRoot környezeti változó</span><span class="sxs-lookup"><span data-stu-id="9a833-228">DataRoot environment variable</span></span>|<span data-ttu-id="9a833-229">A helyi futtató DataRoot alapértelmezett túl "LOCALRUN_DATAROOT" környezeti változó</span><span class="sxs-lookup"><span data-stu-id="9a833-229">DataRoot for local run, default too'LOCALRUN_DATAROOT' environment variable</span></span>|
|<span data-ttu-id="9a833-230">-MessageOut</span><span class="sxs-lookup"><span data-stu-id="9a833-230">-MessageOut</span></span>| |<span data-ttu-id="9a833-231">A konzol tooa fájl memóriakép üzenetek</span><span class="sxs-lookup"><span data-stu-id="9a833-231">Dump messages on console tooa file</span></span>|
|<span data-ttu-id="9a833-232">-Párhuzamos</span><span class="sxs-lookup"><span data-stu-id="9a833-232">-Parallel</span></span>|<span data-ttu-id="9a833-233">1</span><span class="sxs-lookup"><span data-stu-id="9a833-233">1</span></span>|<span data-ttu-id="9a833-234">Futtassa a hello hello terv megadott párhuzamossági</span><span class="sxs-lookup"><span data-stu-id="9a833-234">Run hello plan with hello specified parallelism</span></span>|
|<span data-ttu-id="9a833-235">-Hivatkozások</span><span class="sxs-lookup"><span data-stu-id="9a833-235">-References</span></span>| |<span data-ttu-id="9a833-236">Elérési utak tooextra referenciaszerelvények vagy listája adatforrás adatfájljainak tartozó kódot, szóközzel elválasztva ';'</span><span class="sxs-lookup"><span data-stu-id="9a833-236">List of paths tooextra reference assemblies or data files of code behind, separated by ';'</span></span>|
|<span data-ttu-id="9a833-237">-UdoRedirect</span><span class="sxs-lookup"><span data-stu-id="9a833-237">-UdoRedirect</span></span>|<span data-ttu-id="9a833-238">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="9a833-238">False</span></span>|<span data-ttu-id="9a833-239">Udo szerelvény átirányítási config készítése</span><span class="sxs-lookup"><span data-stu-id="9a833-239">Generate Udo assembly redirect config</span></span>|
|<span data-ttu-id="9a833-240">-UseDatabase</span><span class="sxs-lookup"><span data-stu-id="9a833-240">-UseDatabase</span></span>|<span data-ttu-id="9a833-241">master</span><span class="sxs-lookup"><span data-stu-id="9a833-241">master</span></span>|<span data-ttu-id="9a833-242">Az ideiglenes szerelvények regisztrálásakor háttérkódot adatbázis toouse</span><span class="sxs-lookup"><span data-stu-id="9a833-242">Database toouse for code behind temporary assembly registration</span></span>|
|<span data-ttu-id="9a833-243">-Verbose</span><span class="sxs-lookup"><span data-stu-id="9a833-243">-Verbose</span></span>|<span data-ttu-id="9a833-244">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="9a833-244">False</span></span>|<span data-ttu-id="9a833-245">A futásidejű részletes kimenetének megjelenítése</span><span class="sxs-lookup"><span data-stu-id="9a833-245">Show detailed outputs from runtime</span></span>|
|<span data-ttu-id="9a833-246">-WorkDir</span><span class="sxs-lookup"><span data-stu-id="9a833-246">-WorkDir</span></span>|<span data-ttu-id="9a833-247">Aktuális könyvtárhoz</span><span class="sxs-lookup"><span data-stu-id="9a833-247">Current Directory</span></span>|<span data-ttu-id="9a833-248">A fordító használati és a kimeneti könyvtár</span><span class="sxs-lookup"><span data-stu-id="9a833-248">Directory for compiler usage and outputs</span></span>|
|<span data-ttu-id="9a833-249">-RunScopeCEP</span><span class="sxs-lookup"><span data-stu-id="9a833-249">-RunScopeCEP</span></span>|<span data-ttu-id="9a833-250">0</span><span class="sxs-lookup"><span data-stu-id="9a833-250">0</span></span>|<span data-ttu-id="9a833-251">ScopeCEP mód toouse</span><span class="sxs-lookup"><span data-stu-id="9a833-251">ScopeCEP mode toouse</span></span>|
|<span data-ttu-id="9a833-252">-ScopeCEPTempPath</span><span class="sxs-lookup"><span data-stu-id="9a833-252">-ScopeCEPTempPath</span></span>|<span data-ttu-id="9a833-253">TEMP</span><span class="sxs-lookup"><span data-stu-id="9a833-253">temp</span></span>|<span data-ttu-id="9a833-254">A streamelési adatok ideiglenes elérési toouse</span><span class="sxs-lookup"><span data-stu-id="9a833-254">Temp path toouse for streaming data</span></span>|
|<span data-ttu-id="9a833-255">-OptFlags</span><span class="sxs-lookup"><span data-stu-id="9a833-255">-OptFlags</span></span>| |<span data-ttu-id="9a833-256">Optimalizáló jelzők vesszővel elválasztott listája</span><span class="sxs-lookup"><span data-stu-id="9a833-256">Comma-separated list of optimizer flags</span></span>|


<span data-ttu-id="9a833-257">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="9a833-257">Here's an example:</span></span>

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

<span data-ttu-id="9a833-258">Egyidejű mellett **fordítási** és **hajtható végre**, fordítása, és külön-külön végrehajtása fordítása hello végrehajtható fájlokat.</span><span class="sxs-lookup"><span data-stu-id="9a833-258">Besides combining **compile** and **execute**, you can compile and execute hello compiled executables separately.</span></span>

#### <a name="compile-a-u-sql-script"></a><span data-ttu-id="9a833-259">A U-SQL parancsfájl összeállítása</span><span class="sxs-lookup"><span data-stu-id="9a833-259">Compile a U-SQL script</span></span>

<span data-ttu-id="9a833-260">Hello **fordítási** parancs használt toocompile egy U-SQL parancsfájl tooexecutables.</span><span class="sxs-lookup"><span data-stu-id="9a833-260">hello **compile** command is used toocompile a U-SQL script tooexecutables.</span></span>

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

<span data-ttu-id="9a833-261">hello a következők nem kötelező argumentumainak **fordítási**:</span><span class="sxs-lookup"><span data-stu-id="9a833-261">hello following are optional arguments for **compile**:</span></span>


|<span data-ttu-id="9a833-262">Argumentum</span><span class="sxs-lookup"><span data-stu-id="9a833-262">Argument</span></span>|<span data-ttu-id="9a833-263">Leírás</span><span class="sxs-lookup"><span data-stu-id="9a833-263">Description</span></span>|
|--------|-----------|
| <span data-ttu-id="9a833-264">-Háttérkódban [alapértelmezett értéke "False"]</span><span class="sxs-lookup"><span data-stu-id="9a833-264">-CodeBehind [default value 'False']</span></span>|<span data-ttu-id="9a833-265">hello parancsfájl .cs kód tartozik.</span><span class="sxs-lookup"><span data-stu-id="9a833-265">hello script has .cs code behind</span></span>|
| <span data-ttu-id="9a833-266">-CppSDK [alapértelmezett érték "]</span><span class="sxs-lookup"><span data-stu-id="9a833-266">-CppSDK [default value '']</span></span>|<span data-ttu-id="9a833-267">CppSDK könyvtár</span><span class="sxs-lookup"><span data-stu-id="9a833-267">CppSDK Directory</span></span>|
| <span data-ttu-id="9a833-268">-DataRoot [alapértelmezett érték "DataRoot környezeti változó"]</span><span class="sxs-lookup"><span data-stu-id="9a833-268">-DataRoot [default value 'DataRoot environment variable']</span></span>|<span data-ttu-id="9a833-269">A helyi futtató DataRoot alapértelmezett túl "LOCALRUN_DATAROOT" környezeti változó</span><span class="sxs-lookup"><span data-stu-id="9a833-269">DataRoot for local run, default too'LOCALRUN_DATAROOT' environment variable</span></span>|
| <span data-ttu-id="9a833-270">-MessageOut [alapértelmezett érték "]</span><span class="sxs-lookup"><span data-stu-id="9a833-270">-MessageOut [default value '']</span></span>|<span data-ttu-id="9a833-271">A konzol tooa fájl memóriakép üzenetek</span><span class="sxs-lookup"><span data-stu-id="9a833-271">Dump messages on console tooa file</span></span>|
| <span data-ttu-id="9a833-272">-Hivatkozik [alapértelmezett érték "]</span><span class="sxs-lookup"><span data-stu-id="9a833-272">-References [default value '']</span></span>|<span data-ttu-id="9a833-273">Elérési utak tooextra referenciaszerelvények vagy listája adatforrás adatfájljainak tartozó kódot, szóközzel elválasztva ';'</span><span class="sxs-lookup"><span data-stu-id="9a833-273">List of paths tooextra reference assemblies or data files of code behind, separated by ';'</span></span>|
| <span data-ttu-id="9a833-274">-Sekély [alapértelmezett értéke "False"]</span><span class="sxs-lookup"><span data-stu-id="9a833-274">-Shallow [default value 'False']</span></span>|<span data-ttu-id="9a833-275">Sekély fordítási</span><span class="sxs-lookup"><span data-stu-id="9a833-275">Shallow compile</span></span>|
| <span data-ttu-id="9a833-276">-UdoRedirect [alapértelmezett értéke "False"]</span><span class="sxs-lookup"><span data-stu-id="9a833-276">-UdoRedirect [default value 'False']</span></span>|<span data-ttu-id="9a833-277">Udo szerelvény átirányítási config készítése</span><span class="sxs-lookup"><span data-stu-id="9a833-277">Generate Udo assembly redirect config</span></span>|
| <span data-ttu-id="9a833-278">-UseDatabase [alapértelmezett érték "master"]</span><span class="sxs-lookup"><span data-stu-id="9a833-278">-UseDatabase [default value 'master']</span></span>|<span data-ttu-id="9a833-279">Az ideiglenes szerelvények regisztrálásakor háttérkódot adatbázis toouse</span><span class="sxs-lookup"><span data-stu-id="9a833-279">Database toouse for code behind temporary assembly registration</span></span>|
| <span data-ttu-id="9a833-280">-WorkDir [alapértelmezett érték "Az aktuális könyvtár"]</span><span class="sxs-lookup"><span data-stu-id="9a833-280">-WorkDir [default value 'Current Directory']</span></span>|<span data-ttu-id="9a833-281">A fordító használati és a kimeneti könyvtár</span><span class="sxs-lookup"><span data-stu-id="9a833-281">Directory for compiler usage and outputs</span></span>|
| <span data-ttu-id="9a833-282">-RunScopeCEP [alapértelmezett érték "0"]</span><span class="sxs-lookup"><span data-stu-id="9a833-282">-RunScopeCEP [default value '0']</span></span>|<span data-ttu-id="9a833-283">ScopeCEP mód toouse</span><span class="sxs-lookup"><span data-stu-id="9a833-283">ScopeCEP mode toouse</span></span>|
| <span data-ttu-id="9a833-284">-ScopeCEPTempPath [alapértelmezett érték "temp"]</span><span class="sxs-lookup"><span data-stu-id="9a833-284">-ScopeCEPTempPath [default value 'temp']</span></span>|<span data-ttu-id="9a833-285">A streamelési adatok ideiglenes elérési toouse</span><span class="sxs-lookup"><span data-stu-id="9a833-285">Temp path toouse for streaming data</span></span>|
| <span data-ttu-id="9a833-286">-OptFlags [alapértelmezett érték "]</span><span class="sxs-lookup"><span data-stu-id="9a833-286">-OptFlags [default value '']</span></span>|<span data-ttu-id="9a833-287">Optimalizáló jelzők vesszővel elválasztott listája</span><span class="sxs-lookup"><span data-stu-id="9a833-287">Comma-separated list of optimizer flags</span></span>|


<span data-ttu-id="9a833-288">Az alábbiakban néhány használati példák.</span><span class="sxs-lookup"><span data-stu-id="9a833-288">Here are some usage examples.</span></span>

<span data-ttu-id="9a833-289">A U-SQL parancsfájl összeállítása:</span><span class="sxs-lookup"><span data-stu-id="9a833-289">Compile a U-SQL script:</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql

<span data-ttu-id="9a833-290">A U-SQL parancsfájl összeállításához, és állítsa be a hello adatok-gyökérmappájába.</span><span class="sxs-lookup"><span data-stu-id="9a833-290">Compile a U-SQL script and set hello data-root folder.</span></span> <span data-ttu-id="9a833-291">Vegye figyelembe, hogy ezzel a művelettel felülírom hello beállítása környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="9a833-291">Note that this will overwrite hello set environment variable.</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

<span data-ttu-id="9a833-292">U-SQL parancsfájl fordítása, és egy működő könyvtárba, a referencia szerelvény és az adatbázis beállítása:</span><span class="sxs-lookup"><span data-stu-id="9a833-292">Compile a U-SQL script and set a working directory, reference assembly, and database:</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a><span data-ttu-id="9a833-293">Lefordított eredmények végrehajtása</span><span class="sxs-lookup"><span data-stu-id="9a833-293">Execute compiled results</span></span>

<span data-ttu-id="9a833-294">Hello **hajtható végre** parancs lefordítva használt tooexecute eredmények.</span><span class="sxs-lookup"><span data-stu-id="9a833-294">hello **execute** command is used tooexecute compiled results.</span></span>   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

<span data-ttu-id="9a833-295">hello a következők nem kötelező argumentumainak **hajtható végre**:</span><span class="sxs-lookup"><span data-stu-id="9a833-295">hello following are optional arguments for **execute**:</span></span>

|<span data-ttu-id="9a833-296">Argumentum</span><span class="sxs-lookup"><span data-stu-id="9a833-296">Argument</span></span>|<span data-ttu-id="9a833-297">Leírás</span><span class="sxs-lookup"><span data-stu-id="9a833-297">Description</span></span>|
|--------|-----------|
|<span data-ttu-id="9a833-298">-DataRoot [alapértelmezett érték "]</span><span class="sxs-lookup"><span data-stu-id="9a833-298">-DataRoot [default value '']</span></span>|<span data-ttu-id="9a833-299">A metaadatok végrehajtás adatgyökerében.</span><span class="sxs-lookup"><span data-stu-id="9a833-299">Data root for metadata execution.</span></span> <span data-ttu-id="9a833-300">Alapértelmezés szerint toohello **LOCALRUN_DATAROOT** környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="9a833-300">It defaults toohello **LOCALRUN_DATAROOT** environment variable.</span></span>|
|<span data-ttu-id="9a833-301">-MessageOut [alapértelmezett érték "]</span><span class="sxs-lookup"><span data-stu-id="9a833-301">-MessageOut [default value '']</span></span>|<span data-ttu-id="9a833-302">Memóriakép hello tooa fájlban üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="9a833-302">Dump messages on hello console tooa file.</span></span>|
|<span data-ttu-id="9a833-303">-Párhuzamos [alapértelmezett értéke "1"]</span><span class="sxs-lookup"><span data-stu-id="9a833-303">-Parallel [default value '1']</span></span>|<span data-ttu-id="9a833-304">Kijelző toorun hello létrehozott helyi futtatási lépéseket hello megadott párhuzamossági szintjét.</span><span class="sxs-lookup"><span data-stu-id="9a833-304">Indicator toorun hello generated local-run steps with hello specified parallelism level.</span></span>|
|<span data-ttu-id="9a833-305">-Verbose [alapértelmezett érték "Hamis"]</span><span class="sxs-lookup"><span data-stu-id="9a833-305">-Verbose [default value 'False']</span></span>|<span data-ttu-id="9a833-306">Kijelző tooshow futásidejű kimeneteinek részletes.</span><span class="sxs-lookup"><span data-stu-id="9a833-306">Indicator tooshow detailed outputs from runtime.</span></span>|

<span data-ttu-id="9a833-307">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="9a833-307">Here's a usage example:</span></span>

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-hello-sdk-with-programming-interfaces"></a><span data-ttu-id="9a833-308">Alkalmazásprogramozási felületek hello SDK használata</span><span class="sxs-lookup"><span data-stu-id="9a833-308">Use hello SDK with programming interfaces</span></span>

<span data-ttu-id="9a833-309">hello alkalmazásprogramozási felületek a hello LocalRunHelper.exe találhatók.</span><span class="sxs-lookup"><span data-stu-id="9a833-309">hello programming interfaces are all located in hello LocalRunHelper.exe.</span></span> <span data-ttu-id="9a833-310">U-SQL SDK hello toointegrate hello funkcióit használhat, és C# teszt keretében tooscale hello a U-SQL parancsfájl helyi tesztelése.</span><span class="sxs-lookup"><span data-stu-id="9a833-310">You can use them toointegrate hello functionality of hello U-SQL SDK and hello C# test framework tooscale your U-SQL script local test.</span></span> <span data-ttu-id="9a833-311">Ebben a cikkben használom hello szabványos C# egység teszt projekt tooshow hogyan toouse ezek felületeihez tootest a U-SQL parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="9a833-311">In this article, I will use hello standard C# unit test project tooshow how toouse these interfaces tootest your U-SQL script.</span></span>

### <a name="step-1-create-c-unit-test-project-and-configuration"></a><span data-ttu-id="9a833-312">1. lépés: A C# egység teszt projekt és konfigurációs létrehozása</span><span class="sxs-lookup"><span data-stu-id="9a833-312">Step 1: Create C# unit test project and configuration</span></span>

- <span data-ttu-id="9a833-313">Hozzon létre egy C# egység teszt projektet fájl > Új > Projekt > Visual C# > tesztelése > egység tesztelése projekt.</span><span class="sxs-lookup"><span data-stu-id="9a833-313">Create a C# unit test project through File > New > Project > Visual C# > Test > Unit Test Project.</span></span>
- <span data-ttu-id="9a833-314">Adja hozzá a LocalRunHelper.exe hello projekt hivatkozásként.</span><span class="sxs-lookup"><span data-stu-id="9a833-314">Add LocalRunHelper.exe as a reference for hello project.</span></span> <span data-ttu-id="9a833-315">hello LocalRunHelper.exe itt található: \build\runtime\LocalRunHelper.exe a Nuget-csomagot.</span><span class="sxs-lookup"><span data-stu-id="9a833-315">hello LocalRunHelper.exe is located at \build\runtime\LocalRunHelper.exe in Nuget package.</span></span>

    ![Azure Data Lake U-SQL SDK hivatkozás hozzáadása](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- <span data-ttu-id="9a833-317">U-SQL SDK **csak** támogatási x64 környezetben, győződjön meg arról, hogy tooset build platform cél x64 szerint.</span><span class="sxs-lookup"><span data-stu-id="9a833-317">U-SQL SDK **only** support x64 environment, make sure tooset build platform target as x64.</span></span> <span data-ttu-id="9a833-318">Beállíthatja, hogy a projekt tulajdonságon keresztül > Build > Platform cél.</span><span class="sxs-lookup"><span data-stu-id="9a833-318">You can set that through Project Property > Build > Platform target.</span></span>

    ![Azure Data Lake U-SQL SDK konfigurálása x64 projekt](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- <span data-ttu-id="9a833-320">Győződjön meg arról, hogy tooset tesztelési környezetben, x64.</span><span class="sxs-lookup"><span data-stu-id="9a833-320">Make sure tooset your test environment as x64.</span></span> <span data-ttu-id="9a833-321">A Visual Studio állíthatja keresztül Test > vizsgálati beállítások > alapértelmezett processzorarchitektúra > x64.</span><span class="sxs-lookup"><span data-stu-id="9a833-321">In Visual Studio, you can set it through Test > Test Settings > Default Processor Architecture > x64.</span></span>

    ![Azure Data Lake U-SQL SDK konfigurálása x64 tesztelési környezetben](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- <span data-ttu-id="9a833-323">Győződjön meg arról, hogy toocopy NugetPackage\build\runtime\ tooproject munkakönyvtár általában ProjectFolder\bin\x64\Debug alatt álló összes függőség fájlra.</span><span class="sxs-lookup"><span data-stu-id="9a833-323">Make sure toocopy all dependency files under NugetPackage\build\runtime\ tooproject working directory which is usually under ProjectFolder\bin\x64\Debug.</span></span>

### <a name="step-2-create-u-sql-script-test-case"></a><span data-ttu-id="9a833-324">2. lépés: A U-SQL parancsfájl Teszteset létrehozása</span><span class="sxs-lookup"><span data-stu-id="9a833-324">Step 2: Create U-SQL script test case</span></span>

<span data-ttu-id="9a833-325">Az alábbiakban van hello mintakód a U-SQL parancsfájl teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9a833-325">Below is hello sample code for U-SQL script test.</span></span> <span data-ttu-id="9a833-326">Tesztelési, tooprepare parancsfájlok kell bemeneti fájlok és a várt kimeneti fájlokat.</span><span class="sxs-lookup"><span data-stu-id="9a833-326">For testing, you need tooprepare scripts, input files and expected output files.</span></span>

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
                //Specify hello local run message output path
                StreamWriter MessageOutput = new StreamWriter("../../../log.txt");

                LocalRunHelper localrun = new LocalRunHelper(MessageOutput);

                //Configure hello DateRoot path, Script Path and CPPSDK path
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

                //Don't forget tooclose MessageOutput tooget logs into file
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


### <a name="programming-interfaces-in-localrunhelperexe"></a><span data-ttu-id="9a833-327">A LocalRunHelper.exe felületek</span><span class="sxs-lookup"><span data-stu-id="9a833-327">Programming interfaces in LocalRunHelper.exe</span></span>

<span data-ttu-id="9a833-328">LocalRunHelper.exe biztosít programozási felületek U-SQL helyi fordítási, futtassa a hello, stb. hello felületek az alábbiak szerint vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="9a833-328">LocalRunHelper.exe provides hello programming interfaces for U-SQL local compile, run, etc. hello interfaces are listed as follows.</span></span>

<span data-ttu-id="9a833-329">**Konstruktor**</span><span class="sxs-lookup"><span data-stu-id="9a833-329">**Constructor**</span></span>

<span data-ttu-id="9a833-330">nyilvános LocalRunHelper ([System.IO.TextWriter messageOutput = null])</span><span class="sxs-lookup"><span data-stu-id="9a833-330">public LocalRunHelper([System.IO.TextWriter messageOutput = null])</span></span>

|<span data-ttu-id="9a833-331">Paraméter</span><span class="sxs-lookup"><span data-stu-id="9a833-331">Parameter</span></span>|<span data-ttu-id="9a833-332">Típus</span><span class="sxs-lookup"><span data-stu-id="9a833-332">Type</span></span>|<span data-ttu-id="9a833-333">Leírás</span><span class="sxs-lookup"><span data-stu-id="9a833-333">Description</span></span>|
|---------|----|-----------|
|<span data-ttu-id="9a833-334">messageOutput</span><span class="sxs-lookup"><span data-stu-id="9a833-334">messageOutput</span></span>|<span data-ttu-id="9a833-335">System.IO.TextWriter</span><span class="sxs-lookup"><span data-stu-id="9a833-335">System.IO.TextWriter</span></span>|<span data-ttu-id="9a833-336">a kimeneti üzenetek be toonull toouse konzol</span><span class="sxs-lookup"><span data-stu-id="9a833-336">for output messages, set toonull toouse Console</span></span>|

<span data-ttu-id="9a833-337">**Tulajdonságok**</span><span class="sxs-lookup"><span data-stu-id="9a833-337">**Properties**</span></span>

|<span data-ttu-id="9a833-338">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9a833-338">Property</span></span>|<span data-ttu-id="9a833-339">Típus</span><span class="sxs-lookup"><span data-stu-id="9a833-339">Type</span></span>|<span data-ttu-id="9a833-340">Leírás</span><span class="sxs-lookup"><span data-stu-id="9a833-340">Description</span></span>|
|--------|----|-----------|
|<span data-ttu-id="9a833-341">AlgebraPath</span><span class="sxs-lookup"><span data-stu-id="9a833-341">AlgebraPath</span></span>|<span data-ttu-id="9a833-342">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-342">string</span></span>|<span data-ttu-id="9a833-343">hello elérési tooalgebra fájl (algebra fájl egyike hello fordítási eredmények)</span><span class="sxs-lookup"><span data-stu-id="9a833-343">hello path tooalgebra file (algebra file is one of hello compilation results)</span></span>|
|<span data-ttu-id="9a833-344">CodeBehindReferences</span><span class="sxs-lookup"><span data-stu-id="9a833-344">CodeBehindReferences</span></span>|<span data-ttu-id="9a833-345">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-345">string</span></span>|<span data-ttu-id="9a833-346">Ha hello parancsfájl további háttérkódot hivatkozásokat, adja meg a hello elérési útjait határolt ";"</span><span class="sxs-lookup"><span data-stu-id="9a833-346">If hello script has additional code behind references, specify hello paths separated with ';'</span></span>|
|<span data-ttu-id="9a833-347">CppSdkDir</span><span class="sxs-lookup"><span data-stu-id="9a833-347">CppSdkDir</span></span>|<span data-ttu-id="9a833-348">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-348">string</span></span>|<span data-ttu-id="9a833-349">CppSDK könyvtár</span><span class="sxs-lookup"><span data-stu-id="9a833-349">CppSDK directory</span></span>|
|<span data-ttu-id="9a833-350">CurrentDir</span><span class="sxs-lookup"><span data-stu-id="9a833-350">CurrentDir</span></span>|<span data-ttu-id="9a833-351">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-351">string</span></span>|<span data-ttu-id="9a833-352">Aktuális könyvtárhoz</span><span class="sxs-lookup"><span data-stu-id="9a833-352">Current directory</span></span>|
|<span data-ttu-id="9a833-353">DataRoot</span><span class="sxs-lookup"><span data-stu-id="9a833-353">DataRoot</span></span>|<span data-ttu-id="9a833-354">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-354">string</span></span>|<span data-ttu-id="9a833-355">Adatok elérési útjának gyökeréhez</span><span class="sxs-lookup"><span data-stu-id="9a833-355">Data root path</span></span>|
|<span data-ttu-id="9a833-356">DebuggerMailPath</span><span class="sxs-lookup"><span data-stu-id="9a833-356">DebuggerMailPath</span></span>|<span data-ttu-id="9a833-357">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-357">string</span></span>|<span data-ttu-id="9a833-358">hello elérési toodebugger mailslot</span><span class="sxs-lookup"><span data-stu-id="9a833-358">hello path toodebugger mailslot</span></span>|
|<span data-ttu-id="9a833-359">GenerateUdoRedirect</span><span class="sxs-lookup"><span data-stu-id="9a833-359">GenerateUdoRedirect</span></span>|<span data-ttu-id="9a833-360">logikai érték</span><span class="sxs-lookup"><span data-stu-id="9a833-360">bool</span></span>|<span data-ttu-id="9a833-361">Ha azt szeretné, hogy toogenerate szerelvény betöltése a átirányítási config felülbírálása</span><span class="sxs-lookup"><span data-stu-id="9a833-361">If we want toogenerate assembly loading redirection override config</span></span>|
|<span data-ttu-id="9a833-362">HasCodeBehind</span><span class="sxs-lookup"><span data-stu-id="9a833-362">HasCodeBehind</span></span>|<span data-ttu-id="9a833-363">logikai érték</span><span class="sxs-lookup"><span data-stu-id="9a833-363">bool</span></span>|<span data-ttu-id="9a833-364">Ha hello parancsfájl kód tartozik.</span><span class="sxs-lookup"><span data-stu-id="9a833-364">If hello script has code behind</span></span>|
|<span data-ttu-id="9a833-365">InputDir</span><span class="sxs-lookup"><span data-stu-id="9a833-365">InputDir</span></span>|<span data-ttu-id="9a833-366">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-366">string</span></span>|<span data-ttu-id="9a833-367">A bemeneti adatok könyvtár</span><span class="sxs-lookup"><span data-stu-id="9a833-367">Directory for input data</span></span>|
|<span data-ttu-id="9a833-368">MessagePath</span><span class="sxs-lookup"><span data-stu-id="9a833-368">MessagePath</span></span>|<span data-ttu-id="9a833-369">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-369">string</span></span>|<span data-ttu-id="9a833-370">Üzenet memóriakép fájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="9a833-370">Message dump file path</span></span>|
|<span data-ttu-id="9a833-371">OutputDir</span><span class="sxs-lookup"><span data-stu-id="9a833-371">OutputDir</span></span>|<span data-ttu-id="9a833-372">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-372">string</span></span>|<span data-ttu-id="9a833-373">A kimeneti adatok könyvtár</span><span class="sxs-lookup"><span data-stu-id="9a833-373">Directory for output data</span></span>|
|<span data-ttu-id="9a833-374">Párhuzamos végrehajtás</span><span class="sxs-lookup"><span data-stu-id="9a833-374">Parallelism</span></span>|<span data-ttu-id="9a833-375">int</span><span class="sxs-lookup"><span data-stu-id="9a833-375">int</span></span>|<span data-ttu-id="9a833-376">Párhuzamossági toorun hello algebra</span><span class="sxs-lookup"><span data-stu-id="9a833-376">Parallelism toorun hello algebra</span></span>|
|<span data-ttu-id="9a833-377">ParentPid</span><span class="sxs-lookup"><span data-stu-id="9a833-377">ParentPid</span></span>|<span data-ttu-id="9a833-378">int</span><span class="sxs-lookup"><span data-stu-id="9a833-378">int</span></span>|<span data-ttu-id="9a833-379">A mely hello szolgáltatás figyeli tooexit, set too0 vagy negatív tooignore hello szülő azonosítója (PID)</span><span class="sxs-lookup"><span data-stu-id="9a833-379">PID of hello parent on which hello service monitors tooexit, set too0 or negative tooignore</span></span>|
|<span data-ttu-id="9a833-380">ResultPath</span><span class="sxs-lookup"><span data-stu-id="9a833-380">ResultPath</span></span>|<span data-ttu-id="9a833-381">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-381">string</span></span>|<span data-ttu-id="9a833-382">Eredmény memóriakép fájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="9a833-382">Result dump file path</span></span>|
|<span data-ttu-id="9a833-383">RuntimeDir</span><span class="sxs-lookup"><span data-stu-id="9a833-383">RuntimeDir</span></span>|<span data-ttu-id="9a833-384">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-384">string</span></span>|<span data-ttu-id="9a833-385">Futásidejű könyvtár</span><span class="sxs-lookup"><span data-stu-id="9a833-385">Runtime directory</span></span>|
|<span data-ttu-id="9a833-386">scriptPath</span><span class="sxs-lookup"><span data-stu-id="9a833-386">ScriptPath</span></span>|<span data-ttu-id="9a833-387">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-387">string</span></span>|<span data-ttu-id="9a833-388">Ha toofind hello parancsfájl</span><span class="sxs-lookup"><span data-stu-id="9a833-388">Where toofind hello script</span></span>|
|<span data-ttu-id="9a833-389">Sekély</span><span class="sxs-lookup"><span data-stu-id="9a833-389">Shallow</span></span>|<span data-ttu-id="9a833-390">logikai érték</span><span class="sxs-lookup"><span data-stu-id="9a833-390">bool</span></span>|<span data-ttu-id="9a833-391">Vagy nem a fordítási sekély</span><span class="sxs-lookup"><span data-stu-id="9a833-391">Shallow compile or not</span></span>|
|<span data-ttu-id="9a833-392">TempDir</span><span class="sxs-lookup"><span data-stu-id="9a833-392">TempDir</span></span>|<span data-ttu-id="9a833-393">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-393">string</span></span>|<span data-ttu-id="9a833-394">Ideiglenes könyvtár</span><span class="sxs-lookup"><span data-stu-id="9a833-394">Temp directory</span></span>|
|<span data-ttu-id="9a833-395">UseDataBase</span><span class="sxs-lookup"><span data-stu-id="9a833-395">UseDataBase</span></span>|<span data-ttu-id="9a833-396">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-396">string</span></span>|<span data-ttu-id="9a833-397">Adja meg az ideiglenes szerelvények regisztrálásakor, alapértelmezés szerint fő háttérkódot hello adatbázis toouse</span><span class="sxs-lookup"><span data-stu-id="9a833-397">Specify hello database toouse for code behind temporary assembly registration, master by default</span></span>|
|<span data-ttu-id="9a833-398">WorkDir</span><span class="sxs-lookup"><span data-stu-id="9a833-398">WorkDir</span></span>|<span data-ttu-id="9a833-399">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9a833-399">string</span></span>|<span data-ttu-id="9a833-400">Előnyben részesített munkakönyvtár</span><span class="sxs-lookup"><span data-stu-id="9a833-400">Preferred working directory</span></span>|


<span data-ttu-id="9a833-401">**Módszer**</span><span class="sxs-lookup"><span data-stu-id="9a833-401">**Method**</span></span>

|<span data-ttu-id="9a833-402">Módszer</span><span class="sxs-lookup"><span data-stu-id="9a833-402">Method</span></span>|<span data-ttu-id="9a833-403">Leírás</span><span class="sxs-lookup"><span data-stu-id="9a833-403">Description</span></span>|<span data-ttu-id="9a833-404">térjen vissza</span><span class="sxs-lookup"><span data-stu-id="9a833-404">Return</span></span>|<span data-ttu-id="9a833-405">Paraméter</span><span class="sxs-lookup"><span data-stu-id="9a833-405">Parameter</span></span>|
|------|-----------|------|---------|
|<span data-ttu-id="9a833-406">nyilvános bool DoCompile()</span><span class="sxs-lookup"><span data-stu-id="9a833-406">public bool DoCompile()</span></span>|<span data-ttu-id="9a833-407">Hello U-SQL parancsfájl összeállítása</span><span class="sxs-lookup"><span data-stu-id="9a833-407">Compile hello U-SQL script</span></span>|<span data-ttu-id="9a833-408">Sikeres művelet igaz</span><span class="sxs-lookup"><span data-stu-id="9a833-408">True on success</span></span>| |
|<span data-ttu-id="9a833-409">nyilvános bool DoExec()</span><span class="sxs-lookup"><span data-stu-id="9a833-409">public bool DoExec()</span></span>|<span data-ttu-id="9a833-410">Végrehajtása fordítása hello eredménye</span><span class="sxs-lookup"><span data-stu-id="9a833-410">Execute hello compiled result</span></span>|<span data-ttu-id="9a833-411">Sikeres művelet igaz</span><span class="sxs-lookup"><span data-stu-id="9a833-411">True on success</span></span>| |
|<span data-ttu-id="9a833-412">nyilvános bool DoRun()</span><span class="sxs-lookup"><span data-stu-id="9a833-412">public bool DoRun()</span></span>|<span data-ttu-id="9a833-413">(Fordítási + Execute) hello U-SQL-parancsfájl futtatása</span><span class="sxs-lookup"><span data-stu-id="9a833-413">Run hello U-SQL script (Compile + Execute)</span></span>|<span data-ttu-id="9a833-414">Sikeres művelet igaz</span><span class="sxs-lookup"><span data-stu-id="9a833-414">True on success</span></span>| |
|<span data-ttu-id="9a833-415">nyilvános bool IsValidRuntimeDir (karakterlánc elérési út)</span><span class="sxs-lookup"><span data-stu-id="9a833-415">public bool IsValidRuntimeDir(string path)</span></span>|<span data-ttu-id="9a833-416">Ellenőrizze, hogy a megadott elérési út hello érvényes futásidejű elérési útja</span><span class="sxs-lookup"><span data-stu-id="9a833-416">Check if hello given path is valid runtime path</span></span>|<span data-ttu-id="9a833-417">Igaz a érvényes</span><span class="sxs-lookup"><span data-stu-id="9a833-417">True for valid</span></span>|<span data-ttu-id="9a833-418">hello futásidejű könyvtár elérési útja</span><span class="sxs-lookup"><span data-stu-id="9a833-418">hello path of runtime directory</span></span>|


## <a name="faq-about-common-issue"></a><span data-ttu-id="9a833-419">Közös problémával kapcsolatos gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="9a833-419">FAQ about common issue</span></span>

### <a name="error-1"></a><span data-ttu-id="9a833-420">1. hiba:</span><span class="sxs-lookup"><span data-stu-id="9a833-420">Error 1:</span></span>
<span data-ttu-id="9a833-421">E_CSC_SYSTEM_INTERNAL: Belső hiba!</span><span class="sxs-lookup"><span data-stu-id="9a833-421">E_CSC_SYSTEM_INTERNAL: Internal error!</span></span> <span data-ttu-id="9a833-422">Nem sikerült betölteni a fájl vagy szerelvény "ScopeEngineManaged.dll" vagy annak valamelyik függősége.</span><span class="sxs-lookup"><span data-stu-id="9a833-422">Could not load file or assembly 'ScopeEngineManaged.dll' or one of its dependencies.</span></span> <span data-ttu-id="9a833-423">hello megadott modul nem található.</span><span class="sxs-lookup"><span data-stu-id="9a833-423">hello specified module could not be found.</span></span>

<span data-ttu-id="9a833-424">Ellenőrizze a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="9a833-424">Please check hello following:</span></span>

- <span data-ttu-id="9a833-425">Győződjön meg arról, hogy x64 környezetben.</span><span class="sxs-lookup"><span data-stu-id="9a833-425">Make sure you have x64 environment.</span></span> <span data-ttu-id="9a833-426">hello build célplatform és hello tesztkörnyezetben kell x64, tekintse meg a túl**1. lépés: egység létrehozása a C# projekt és konfigurációs teszteléséhez** felett.</span><span class="sxs-lookup"><span data-stu-id="9a833-426">hello build target platform and hello test environment should be x64, refer too**Step 1: Create C# unit test project and configuration** above.</span></span>
- <span data-ttu-id="9a833-427">Ellenőrizze, hogy az összes függőség fájlra NugetPackage\build\runtime\ tooproject munkakönyvtár másolta.</span><span class="sxs-lookup"><span data-stu-id="9a833-427">Make sure you have copied all dependency files under NugetPackage\build\runtime\ tooproject working directory.</span></span>


## <a name="next-steps"></a><span data-ttu-id="9a833-428">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9a833-428">Next steps</span></span>

* <span data-ttu-id="9a833-429">toolearn U-SQL, lásd: [Ismerkedés az Azure Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9a833-429">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="9a833-430">toolog diagnosztikai információkat, lásd: [diagnosztikai naplók elérése az Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="9a833-430">toolog diagnostics information, see [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).</span></span>
* <span data-ttu-id="9a833-431">egy összetettebb lekérdezés toosee lásd [Azure Data Lake Analytics használatával webhelyek naplóinak elemzése](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="9a833-431">toosee a more complex query, see [Analyze website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="9a833-432">feladat részletei tooview, lásd: [használata feladat böngésző és az Azure Data Lake Analytics-feladatok feladat megtekintése](data-lake-analytics-data-lake-tools-view-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="9a833-432">tooview job details, see [Use Job Browser and Job View for Azure Data Lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md).</span></span>
* <span data-ttu-id="9a833-433">toouse hello vertex végrehajtási nézetet, lásd: [használata hello Vertex végrehajtási nézetet a Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span><span class="sxs-lookup"><span data-stu-id="9a833-433">toouse hello vertex execution view, see [Use hello Vertex Execution View in Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span></span>
