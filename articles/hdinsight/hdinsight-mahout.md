---
title: "PowerShell - Azure Mahout HDInsight segítségével aaaGenerate javaslatok |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Apache Mahout gépi tanulási a szalagtár toogenerate movie javaslatok és a HDInsight (Hadoop) együttes az ügyfélszámítógépen futó PowerShell parancsfájl."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 07b57208-32aa-4e59-900a-6c934fa1b7a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: 675a2cd8ecaf7fc797d6cd094e4e58f9aca7ed92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight-powershell"></a><span data-ttu-id="2abeb-103">Film javaslatok generálása Apache Mahout a hadooppal a Hdinsightban (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="2abeb-103">Generate movie recommendations by using Apache Mahout with Hadoop in HDInsight (PowerShell)</span></span>

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

<span data-ttu-id="2abeb-104">Megtudhatja, hogyan toouse hello [Apache Mahout](http://mahout.apache.org) machine learning függvénytár Azure HDInsight toogenerate movie javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="2abeb-104">Learn how toouse hello [Apache Mahout](http://mahout.apache.org) machine learning library with Azure HDInsight toogenerate movie recommendations.</span></span> <span data-ttu-id="2abeb-105">hello ebben a dokumentumban példa Azure PowerShell toorun Mahout feladatok.</span><span class="sxs-lookup"><span data-stu-id="2abeb-105">hello example in this document uses Azure PowerShell toorun Mahout jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2abeb-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2abeb-106">Prerequisites</span></span>

* <span data-ttu-id="2abeb-107">A Linux-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="2abeb-107">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="2abeb-108">Egy létrehozásával kapcsolatos további információkért lásd: [hdinsight Linux-alapú Hadoop használatának megkezdésében][getstarted].</span><span class="sxs-lookup"><span data-stu-id="2abeb-108">For information about creating one, see [Get started using Linux-based Hadoop in HDInsight][getstarted].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2abeb-109">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="2abeb-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="2abeb-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="2abeb-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* [<span data-ttu-id="2abeb-111">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="2abeb-111">Azure PowerShell</span></span>](/powershell/azure/overview)

## <span data-ttu-id="2abeb-112"><a name="recommendations"></a>Javaslatok generálása Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="2abeb-112"><a name="recommendations"></a>Generate recommendations by using Azure PowerShell</span></span>

> [!WARNING]
> <span data-ttu-id="2abeb-113">Ebben a szakaszban hello feladat Azure PowerShell használatával működik.</span><span class="sxs-lookup"><span data-stu-id="2abeb-113">hello job in this section works by using Azure PowerShell.</span></span> <span data-ttu-id="2abeb-114">Mahout megadott hello osztályok többsége nem jelenleg Azure PowerShell-lel dolgozni.</span><span class="sxs-lookup"><span data-stu-id="2abeb-114">Many of hello classes provided with Mahout do not currently work with Azure PowerShell.</span></span> <span data-ttu-id="2abeb-115">Nem működnek az Azure PowerShell osztályok listáját lásd: hello [hibaelhárítás](#troubleshooting) szakasz.</span><span class="sxs-lookup"><span data-stu-id="2abeb-115">For a list of classes that do not work with Azure PowerShell, see hello [Troubleshooting](#troubleshooting) section.</span></span>
>
> <span data-ttu-id="2abeb-116">Az SSH tooconnect tooHDInsight és futtatási Mahout példák segítségével közvetlenül hello fürtön példáért lásd: [Mahout és a HDInsight-(SSH) használatával movie javaslatok generálása](hdinsight-hadoop-mahout-linux-mac.md).</span><span class="sxs-lookup"><span data-stu-id="2abeb-116">For an example of using SSH tooconnect tooHDInsight and run Mahout examples directly on hello cluster, see [Generate movie recommendations using Mahout and HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).</span></span>

<span data-ttu-id="2abeb-117">Mahout által biztosított hello függvények egyike egy javaslat motor.</span><span class="sxs-lookup"><span data-stu-id="2abeb-117">One of hello functions that is provided by Mahout is a recommendation engine.</span></span> <span data-ttu-id="2abeb-118">Ez a motor hello formátumban adatokat fogad `userID`, `itemId`, és `prefValue` (hello hello elem felhasználók beállításait).</span><span class="sxs-lookup"><span data-stu-id="2abeb-118">This engine accepts data in hello format of `userID`, `itemId`, and `prefValue` (hello users preference for hello item).</span></span> <span data-ttu-id="2abeb-119">Mahout hello adatok toodetermine felhasználók hasonló-cikk beállítások, amelyek használt toomake javaslatok is használ.</span><span class="sxs-lookup"><span data-stu-id="2abeb-119">Mahout uses hello data toodetermine users with like-item preferences, which can be used toomake recommendations.</span></span>

<span data-ttu-id="2abeb-120">hello következő példája egy egyszerűsített segédlet hello javaslat folyamat működését:</span><span class="sxs-lookup"><span data-stu-id="2abeb-120">hello following example is a simplified walk-through of how hello recommendation process works:</span></span>

* <span data-ttu-id="2abeb-121">**közös előfordulási**: Joe, Ágnes és minden tetszését Bob *csillag ütközések*, *Empire sztrájkok vissza hello*, és *Return a hello Jedi*.</span><span class="sxs-lookup"><span data-stu-id="2abeb-121">**co-occurrence**: Joe, Alice, and Bob all liked *Star Wars*, *hello Empire Strikes Back*, and *Return of hello Jedi*.</span></span> <span data-ttu-id="2abeb-122">Mahout határozza meg, hogy a felhasználók, akik például ezek filmek egyikét sem is, például más két hello.</span><span class="sxs-lookup"><span data-stu-id="2abeb-122">Mahout determines that users who like any one of these movies also like hello other two.</span></span>

* <span data-ttu-id="2abeb-123">**közös előfordulási**: Bálint és Alice is tetszett *látszólagos támadása hello*, *hello klónja támadás*, és *megtorlás hello Sith a*.</span><span class="sxs-lookup"><span data-stu-id="2abeb-123">**co-occurrence**: Bob and Alice also liked *hello Phantom Menace*, *Attack of hello Clones*, and *Revenge of hello Sith*.</span></span> <span data-ttu-id="2abeb-124">Mahout határozza meg, hogy felhasználók, akik hello előző három filmek is tetszett, például a filmek.</span><span class="sxs-lookup"><span data-stu-id="2abeb-124">Mahout determines that users who liked hello previous three movies also like these movies.</span></span>

* <span data-ttu-id="2abeb-125">**Hasonlóság ajánlás**: mivel Joe tetszését hello első három filmek, Mahout ellenőrzi, hogy az filmek, hogy más, hasonló beállítások tetszett, de Joe rendelkezik nem figyelt (tetszését/névleges).</span><span class="sxs-lookup"><span data-stu-id="2abeb-125">**Similarity recommendation**: Because Joe liked hello first three movies, Mahout looks at movies that others with similar preferences liked, but Joe has not watched (liked/rated).</span></span> <span data-ttu-id="2abeb-126">Ebben az esetben Mahout javasolja *látszólagos támadása hello*, *támadás hello klónja*, és *a hello Sith megtorlás*.</span><span class="sxs-lookup"><span data-stu-id="2abeb-126">In this case, Mahout recommends *hello Phantom Menace*, *Attack of hello Clones*, and *Revenge of hello Sith*.</span></span>

### <a name="understanding-hello-data"></a><span data-ttu-id="2abeb-127">Hello adatok ismertetése</span><span class="sxs-lookup"><span data-stu-id="2abeb-127">Understanding hello data</span></span>

<span data-ttu-id="2abeb-128">[GroupLens kutatási] [ movielens] minősítés adatokat biztosít a filmek formátuma nem kompatibilis a Mahout.</span><span class="sxs-lookup"><span data-stu-id="2abeb-128">[GroupLens Research][movielens] provides rating data for movies in a format that is compatible with Mahout.</span></span> <span data-ttu-id="2abeb-129">Hello alapértelmezett tároló, a fürt számára érhetők el az adatok `/HdiSamples//HdiSamples/MahoutMovieData`.</span><span class="sxs-lookup"><span data-stu-id="2abeb-129">This data is available on hello default storage for your cluster at `/HdiSamples//HdiSamples/MahoutMovieData`.</span></span>

<span data-ttu-id="2abeb-130">Két fájlt `moviedb.txt` (hello filmek információ) és `user-ratings.txt`.</span><span class="sxs-lookup"><span data-stu-id="2abeb-130">There are two files, `moviedb.txt` (information about hello movies) and `user-ratings.txt`.</span></span> <span data-ttu-id="2abeb-131">Hello `user-ratings.txt` fájllal elemzése során.</span><span class="sxs-lookup"><span data-stu-id="2abeb-131">hello `user-ratings.txt` file is used during analysis.</span></span> <span data-ttu-id="2abeb-132">Hello `moviedb.txt` fájl használt tooprovide felhasználóbarát szöveg hello elemzés eredményeinek hello megjelenítésekor.</span><span class="sxs-lookup"><span data-stu-id="2abeb-132">hello `moviedb.txt` file is used tooprovide user-friendly text when displaying hello results of hello analysis.</span></span>

<span data-ttu-id="2abeb-133">hello felhasználói-ratings.txt szereplő adatok rendelkezik egy szerkezete `userID`, `movieID`, `userRating`, és `timestamp`, amely közli a gép magas hogyan minden felhasználó besorolású film.</span><span class="sxs-lookup"><span data-stu-id="2abeb-133">hello data contained in user-ratings.txt has a structure of `userID`, `movieID`, `userRating`, and `timestamp`, which tells us how highly each user rated a movie.</span></span> <span data-ttu-id="2abeb-134">Íme egy példa a hello adatok:</span><span class="sxs-lookup"><span data-stu-id="2abeb-134">Here is an example of hello data:</span></span>

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

### <a name="run-hello-job"></a><span data-ttu-id="2abeb-135">Hello feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="2abeb-135">Run hello job</span></span>

<span data-ttu-id="2abeb-136">A következő Windows PowerShell parancsfájl toorun egy feladatot, amely hello Mahout javaslat motort használja, hello movie adatokkal hello használata:</span><span class="sxs-lookup"><span data-stu-id="2abeb-136">Use hello following Windows PowerShell script toorun a job that uses hello Mahout recommendation engine with hello movie data:</span></span>

> [!NOTE]
> <span data-ttu-id="2abeb-137">Ez a fájl információkat kér, amely használt tooconnect tooyour HDInsight-fürtöt és futtatási feladatok.</span><span class="sxs-lookup"><span data-stu-id="2abeb-137">This file prompts you for information that is used tooconnect tooyour HDInsight cluster and run jobs.</span></span> <span data-ttu-id="2abeb-138">Ez a hello feladatok toocomplete néhány percet igénybe vehet, és hello kimenet.txt fájl letöltése.</span><span class="sxs-lookup"><span data-stu-id="2abeb-138">It may take several minutes for hello jobs toocomplete and download hello output.txt file.</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=5-98)]

> [!NOTE]
> <span data-ttu-id="2abeb-139">Mahout feladatok hello feladat feldolgozása közben létrehozott ideiglenes adatok nem távolítja el.</span><span class="sxs-lookup"><span data-stu-id="2abeb-139">Mahout jobs do not remove temporary data that is created while processing hello job.</span></span> <span data-ttu-id="2abeb-140">Hello `--tempDir` paraméter megadott hello példa tooisolate hello ideiglenes feladatfájlokhoz. egy adott könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="2abeb-140">hello `--tempDir` parameter is specified in hello example job tooisolate hello temporary files into a specific directory.</span></span>

<span data-ttu-id="2abeb-141">hello Mahout feladat hello kimeneti tooSTDOUT nem ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2abeb-141">hello Mahout job does not return hello output tooSTDOUT.</span></span> <span data-ttu-id="2abeb-142">Ehelyett azt tárolja hello megadott kimeneti könyvtár, **rész-r-00000**.</span><span class="sxs-lookup"><span data-stu-id="2abeb-142">Instead, it stores it in hello specified output directory as **part-r-00000**.</span></span> <span data-ttu-id="2abeb-143">hello parancsfájl letölti a fájl túl**kimenet.txt** hello aktuális könyvtárban a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="2abeb-143">hello script downloads this file too**output.txt** in hello current directory on your workstation.</span></span>

<span data-ttu-id="2abeb-144">hello következő szöveg látható egy példa a fájl tartalma hello:</span><span class="sxs-lookup"><span data-stu-id="2abeb-144">hello following text is an example of hello content of this file:</span></span>

    1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

<span data-ttu-id="2abeb-145">hello első oszlopa hello `userID`.</span><span class="sxs-lookup"><span data-stu-id="2abeb-145">hello first column is hello `userID`.</span></span> <span data-ttu-id="2abeb-146">hello található értékek ' ["és"] "vannak `movieId`:`recommendationScore`.</span><span class="sxs-lookup"><span data-stu-id="2abeb-146">hello values contained in '[' and ']' are `movieId`:`recommendationScore`.</span></span>

<span data-ttu-id="2abeb-147">hello parancsfájlt is letölti hello `moviedb.txt` és `user-ratings.txt` fájlokat, amelyek szükséges tooformat hello kimeneti toobe olvashatóbbá.</span><span class="sxs-lookup"><span data-stu-id="2abeb-147">hello script also downloads hello `moviedb.txt` and `user-ratings.txt` files, which are needed tooformat hello output toobe more readable.</span></span>

### <a name="view-hello-output"></a><span data-ttu-id="2abeb-148">Nézet hello kimeneti</span><span class="sxs-lookup"><span data-stu-id="2abeb-148">View hello output</span></span>

<span data-ttu-id="2abeb-149">Bár hello létrehozott kimeneti lehetnek OK alkalmazásban használható, nincs felhasználóbarát.</span><span class="sxs-lookup"><span data-stu-id="2abeb-149">Although hello generated output might be OK for use in an application, it's not user-friendly.</span></span> <span data-ttu-id="2abeb-150">Hello `moviedb.txt` hello kiszolgáló lehet használt tooresolve hello `movieId` tooa movie nevét.</span><span class="sxs-lookup"><span data-stu-id="2abeb-150">hello `moviedb.txt` from hello server can be used tooresolve hello `movieId` tooa movie name.</span></span> <span data-ttu-id="2abeb-151">A következő PowerShell parancsfájl toodisplay javaslatok movie nevű hello használata:</span><span class="sxs-lookup"><span data-stu-id="2abeb-151">Use hello following PowerShell script toodisplay recommendations with movie names:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=106-180)]

<span data-ttu-id="2abeb-152">A következő parancs toodisplay hello javaslatok felhasználóbarát formátumban hello használata:</span><span class="sxs-lookup"><span data-stu-id="2abeb-152">Use hello following command toodisplay hello recommendations in a user-friendly format:</span></span> 

```powershell
.\show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt
```

<span data-ttu-id="2abeb-153">a kimeneti hello hasonló toohello a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="2abeb-153">hello output is similar toohello following text:</span></span>

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, hello (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of hello Dove, hello (1997)            4.6666665
    People vs. Larry Flynt, hello (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237

## <span data-ttu-id="2abeb-154"><a name="troubleshooting"></a>Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="2abeb-154"><a name="troubleshooting"></a>Troubleshooting</span></span>

### <a name="cannot-overwrite-files"></a><span data-ttu-id="2abeb-155">Fájlok nem írható felül.</span><span class="sxs-lookup"><span data-stu-id="2abeb-155">Cannot overwrite files</span></span>

<span data-ttu-id="2abeb-156">Mahout feladatok adatformátuma nem karbantartása feldolgozás során létrehozott ideiglenes fájlokat.</span><span class="sxs-lookup"><span data-stu-id="2abeb-156">Mahout jobs do not clean up temporary files that were created during processing.</span></span> <span data-ttu-id="2abeb-157">Ezenkívül hello feladatok fájl felülírásának mellőzése meglévő kimeneti.</span><span class="sxs-lookup"><span data-stu-id="2abeb-157">In addition, hello jobs do not overwrite existing output file.</span></span>

<span data-ttu-id="2abeb-158">Mahout feladatok futtatásakor tooavoid hibák között futtatása ideiglenes és a kimeneti fájlok törlése.</span><span class="sxs-lookup"><span data-stu-id="2abeb-158">tooavoid errors when running Mahout jobs, delete temporary and output files between runs.</span></span> <span data-ttu-id="2abeb-159">létrehozott tooremove hello fájlok hello a jelen dokumentum korábbi parancsfájlok használja a következő PowerShell-parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="2abeb-159">tooremove hello files created by hello earlier scripts in this document, use hello following PowerShell script:</span></span>

```powershell
# Login tooyour Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter hello HDInsight cluster name"
$creds=Get-Credential -Message "Enter hello login for hello cluster"

#Get hello cluster info so we can get hello resource group, storage, etc.
$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
$container = $clusterInfo.DefaultStorageContainer
$storageAccountKey = (Get-AzureRmStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage context and upload hello file
$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

#Azure PowerShell can't delete blobs using wildcard,
#so have tooget a list and delete one at a time
# Start with hello output
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/out"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
# Next hello temp files
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/temp"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
```

### <span data-ttu-id="2abeb-160"><a name="nopowershell"></a>Olyan osztállyal, amelynek nem működnek az Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="2abeb-160"><a name="nopowershell"></a>Classes that do not work with Azure PowerShell</span></span>

<span data-ttu-id="2abeb-161">A következő osztályok hello használó mahout feladatok vissza különböző hibaüzenetek a Windows PowerShell használata esetén:</span><span class="sxs-lookup"><span data-stu-id="2abeb-161">Mahout jobs that use hello following classes return various error messages when used from Windows PowerShell:</span></span>

* <span data-ttu-id="2abeb-162">org.apache.mahout.utils.clustering.ClusterDumper</span><span class="sxs-lookup"><span data-stu-id="2abeb-162">org.apache.mahout.utils.clustering.ClusterDumper</span></span>
* <span data-ttu-id="2abeb-163">org.apache.mahout.utils.SequenceFileDumper</span><span class="sxs-lookup"><span data-stu-id="2abeb-163">org.apache.mahout.utils.SequenceFileDumper</span></span>
* <span data-ttu-id="2abeb-164">org.apache.mahout.utils.vectors.lucene.Driver</span><span class="sxs-lookup"><span data-stu-id="2abeb-164">org.apache.mahout.utils.vectors.lucene.Driver</span></span>
* <span data-ttu-id="2abeb-165">org.apache.mahout.utils.vectors.arff.Driver</span><span class="sxs-lookup"><span data-stu-id="2abeb-165">org.apache.mahout.utils.vectors.arff.Driver</span></span>
* <span data-ttu-id="2abeb-166">org.apache.mahout.text.WikipediaToSequenceFile</span><span class="sxs-lookup"><span data-stu-id="2abeb-166">org.apache.mahout.text.WikipediaToSequenceFile</span></span>
* <span data-ttu-id="2abeb-167">org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles</span><span class="sxs-lookup"><span data-stu-id="2abeb-167">org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles</span></span>
* <span data-ttu-id="2abeb-168">org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer</span><span class="sxs-lookup"><span data-stu-id="2abeb-168">org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer</span></span>
* <span data-ttu-id="2abeb-169">org.apache.mahout.classifier.sgd.TrainLogistic</span><span class="sxs-lookup"><span data-stu-id="2abeb-169">org.apache.mahout.classifier.sgd.TrainLogistic</span></span>
* <span data-ttu-id="2abeb-170">org.apache.mahout.classifier.sgd.RunLogistic</span><span class="sxs-lookup"><span data-stu-id="2abeb-170">org.apache.mahout.classifier.sgd.RunLogistic</span></span>
* <span data-ttu-id="2abeb-171">org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic</span><span class="sxs-lookup"><span data-stu-id="2abeb-171">org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic</span></span>
* <span data-ttu-id="2abeb-172">org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic</span><span class="sxs-lookup"><span data-stu-id="2abeb-172">org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic</span></span>
* <span data-ttu-id="2abeb-173">org.apache.mahout.classifier.sgd.RunAdaptiveLogistic</span><span class="sxs-lookup"><span data-stu-id="2abeb-173">org.apache.mahout.classifier.sgd.RunAdaptiveLogistic</span></span>
* <span data-ttu-id="2abeb-174">org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer</span><span class="sxs-lookup"><span data-stu-id="2abeb-174">org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer</span></span>
* <span data-ttu-id="2abeb-175">org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator</span><span class="sxs-lookup"><span data-stu-id="2abeb-175">org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator</span></span>
* <span data-ttu-id="2abeb-176">org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator</span><span class="sxs-lookup"><span data-stu-id="2abeb-176">org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator</span></span>
* <span data-ttu-id="2abeb-177">org.apache.mahout.classifier.df.tools.Describe</span><span class="sxs-lookup"><span data-stu-id="2abeb-177">org.apache.mahout.classifier.df.tools.Describe</span></span>

<span data-ttu-id="2abeb-178">Ezeket az osztályokat használó toorun feladatok toohello HDInsight-fürthöz SSH használatával csatlakozzon, és futtassa a hello feladatok hello parancssorból.</span><span class="sxs-lookup"><span data-stu-id="2abeb-178">toorun jobs that use these classes, connect toohello HDInsight cluster using SSH and run hello jobs from hello command line.</span></span> <span data-ttu-id="2abeb-179">Például egy SSH toorun Mahout feladatok használatával, lásd: [Mahout és a HDInsight-(SSH) használatával movie javaslatok generálása](hdinsight-hadoop-mahout-linux-mac.md).</span><span class="sxs-lookup"><span data-stu-id="2abeb-179">For an example of using SSH toorun Mahout jobs, see [Generate movie recommendations using Mahout and HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2abeb-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2abeb-180">Next steps</span></span>

<span data-ttu-id="2abeb-181">Most, hogy megtanulta, hogyan toouse Mahout, felderíteni a más módon a HDInsight-adatokkal:</span><span class="sxs-lookup"><span data-stu-id="2abeb-181">Now that you have learned how toouse Mahout, discover other ways of working with data on HDInsight:</span></span>

* [<span data-ttu-id="2abeb-182">A HDInsight Hive</span><span class="sxs-lookup"><span data-stu-id="2abeb-182">Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="2abeb-183">Pig with HDInsight</span><span class="sxs-lookup"><span data-stu-id="2abeb-183">Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="2abeb-184">MapReduce a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="2abeb-184">MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: /powershell/azureps-cmdlets-docs
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
