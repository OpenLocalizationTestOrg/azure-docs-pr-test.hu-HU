---
title: "Mahout és-(SSH) HDInsight - Azure aaaGenerate javaslatok |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Apache Mahout gépi tanulási a szalagtár toogenerate movie ajánlások a hdinsight (Hadoop) eszközzel."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c78ec37c-9a8c-4bb6-9e38-0bdb9e89fbd7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: fedac9ceb4268f8421bce4623a5ad271041b8b3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight-ssh"></a><span data-ttu-id="41d90-103">Film javaslatok generálása Apache Mahout Linux-alapú hadooppal a HDInsight-(SSH) használatával</span><span class="sxs-lookup"><span data-stu-id="41d90-103">Generate movie recommendations by using Apache Mahout with Linux-based Hadoop in HDInsight (SSH)</span></span>

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

<span data-ttu-id="41d90-104">Megtudhatja, hogyan toouse hello [Apache Mahout](http://mahout.apache.org) machine learning függvénytár Azure HDInsight toogenerate movie javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="41d90-104">Learn how toouse hello [Apache Mahout](http://mahout.apache.org) machine learning library with Azure HDInsight toogenerate movie recommendations.</span></span>

<span data-ttu-id="41d90-105">Mahout van egy [gépi tanulás] [ ml] könyvtára Apache Hadoop.</span><span class="sxs-lookup"><span data-stu-id="41d90-105">Mahout is a [machine learning][ml] library for Apache Hadoop.</span></span> <span data-ttu-id="41d90-106">Mahout adatok, például a szűrést, besorolást, és a fürtszolgáltatás feldolgozására algoritmusok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="41d90-106">Mahout contains algorithms for processing data, such as filtering, classification, and clustering.</span></span> <span data-ttu-id="41d90-107">Ebben a cikkben egy javaslat motor toogenerate movie javaslatok az ismerősök láthatta filmek alapuló használja.</span><span class="sxs-lookup"><span data-stu-id="41d90-107">In this article, you use a recommendation engine toogenerate movie recommendations that are based on movies your friends have seen.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="41d90-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="41d90-108">Prerequisites</span></span>

* <span data-ttu-id="41d90-109">A Linux-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="41d90-109">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="41d90-110">Egy létrehozásával kapcsolatos további információkért lásd: [hdinsight Linux-alapú Hadoop használatának megkezdésében][getstarted].</span><span class="sxs-lookup"><span data-stu-id="41d90-110">For information about creating one, see [Get started using Linux-based Hadoop in HDInsight][getstarted].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="41d90-111">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="41d90-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="41d90-112">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="41d90-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="41d90-113">Egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="41d90-113">An SSH client.</span></span> <span data-ttu-id="41d90-114">További információkért lásd: hello [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="41d90-114">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

## <a name="mahout-versioning"></a><span data-ttu-id="41d90-115">Mahout versioning</span><span class="sxs-lookup"><span data-stu-id="41d90-115">Mahout versioning</span></span>

<span data-ttu-id="41d90-116">Hdinsight Mahout hello verziójával kapcsolatos további információkért lásd: [HDInsight verziója és a Hadoop-összetevők](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="41d90-116">For more information about hello version of Mahout in HDInsight, see [HDInsight versions and Hadoop components](hdinsight-component-versioning.md).</span></span>

## <span data-ttu-id="41d90-117"><a name="recommendations"></a>Understanding javaslatok</span><span class="sxs-lookup"><span data-stu-id="41d90-117"><a name="recommendations"></a>Understanding recommendations</span></span>

<span data-ttu-id="41d90-118">Mahout által biztosított hello függvények egyike egy javaslat motor.</span><span class="sxs-lookup"><span data-stu-id="41d90-118">One of hello functions that is provided by Mahout is a recommendation engine.</span></span> <span data-ttu-id="41d90-119">Ez a motor hello formátumban adatokat fogad `userID`, `itemId`, és `prefValue` (hello hello elem preferencia).</span><span class="sxs-lookup"><span data-stu-id="41d90-119">This engine accepts data in hello format of `userID`, `itemId`, and `prefValue` (hello preference for hello item).</span></span> <span data-ttu-id="41d90-120">Mahout végezhet el, közös elemként elemzés toodetermine: *egy elem előnyben rendelkező felhasználók is hozzáférhetnek ezeket a beállításokat szabályozó egyéb elemek*.</span><span class="sxs-lookup"><span data-stu-id="41d90-120">Mahout can then perform co-occurance analysis toodetermine: *users who have a preference for an item also have a preference for these other items*.</span></span> <span data-ttu-id="41d90-121">Mahout majd határozza meg a felhasználók hasonló-cikk beállítások, amelyek használt toomake javaslatok is.</span><span class="sxs-lookup"><span data-stu-id="41d90-121">Mahout then determines users with like-item preferences, which can be used toomake recommendations.</span></span>

<span data-ttu-id="41d90-122">hello következő munkafolyamat egy egyszerűsített példában látható, amely movie adatait használja:</span><span class="sxs-lookup"><span data-stu-id="41d90-122">hello following workflow is a simplified example that uses movie data:</span></span>

* <span data-ttu-id="41d90-123">**Közös elemként**: Joe, Ágnes és minden tetszését Bob *csillag ütközések*, *Empire sztrájkok vissza hello*, és *Return a hello Jedi*.</span><span class="sxs-lookup"><span data-stu-id="41d90-123">**Co-occurance**: Joe, Alice, and Bob all liked *Star Wars*, *hello Empire Strikes Back*, and *Return of hello Jedi*.</span></span> <span data-ttu-id="41d90-124">Mahout határozza meg, hogy a felhasználók, akik például ezek filmek egyikét sem is, például más két hello.</span><span class="sxs-lookup"><span data-stu-id="41d90-124">Mahout determines that users who like any one of these movies also like hello other two.</span></span>

* <span data-ttu-id="41d90-125">**Közös elemként**: Bálint és Alice is tetszett *látszólagos támadása hello*, *támadás hello klónja*, és *a hello Sith megtorlás*.</span><span class="sxs-lookup"><span data-stu-id="41d90-125">**Co-occurance**: Bob and Alice also liked *hello Phantom Menace*, *Attack of hello Clones*, and *Revenge of hello Sith*.</span></span> <span data-ttu-id="41d90-126">Mahout határozza meg, hogy felhasználók, akik hello előző három filmek is tetszett, például a három filmek.</span><span class="sxs-lookup"><span data-stu-id="41d90-126">Mahout determines that users who liked hello previous three movies also like these three movies.</span></span>

* <span data-ttu-id="41d90-127">**Hasonlóság ajánlás**: mivel Joe tetszését hello első három filmek, Mahout ellenőrzi, hogy az filmek, hogy más, hasonló beállítások tetszett, de Joe rendelkezik nem figyelt (tetszését/névleges).</span><span class="sxs-lookup"><span data-stu-id="41d90-127">**Similarity recommendation**: Because Joe liked hello first three movies, Mahout looks at movies that others with similar preferences liked, but Joe has not watched (liked/rated).</span></span> <span data-ttu-id="41d90-128">Ebben az esetben Mahout javasolja *látszólagos támadása hello*, *támadás hello klónja*, és *a hello Sith megtorlás*.</span><span class="sxs-lookup"><span data-stu-id="41d90-128">In this case, Mahout recommends *hello Phantom Menace*, *Attack of hello Clones*, and *Revenge of hello Sith*.</span></span>

### <a name="understanding-hello-data"></a><span data-ttu-id="41d90-129">Hello adatok ismertetése</span><span class="sxs-lookup"><span data-stu-id="41d90-129">Understanding hello data</span></span>

<span data-ttu-id="41d90-130">Kényelmesen [GroupLens kutatási] [ movielens] minősítés adatokat biztosít a filmek formátuma nem kompatibilis a Mahout.</span><span class="sxs-lookup"><span data-stu-id="41d90-130">Conveniently, [GroupLens Research][movielens] provides rating data for movies in a format that is compatible with Mahout.</span></span> <span data-ttu-id="41d90-131">Ezek az adatok érhető el a fürt alapértelmezett tárolás `/HdiSamples/HdiSamples/MahoutMovieData`.</span><span class="sxs-lookup"><span data-stu-id="41d90-131">This data is available on your cluster's default storage at `/HdiSamples/HdiSamples/MahoutMovieData`.</span></span>

<span data-ttu-id="41d90-132">Két fájlt `moviedb.txt` és `user-ratings.txt`.</span><span class="sxs-lookup"><span data-stu-id="41d90-132">There are two files, `moviedb.txt` and `user-ratings.txt`.</span></span> <span data-ttu-id="41d90-133">hello felhasználói-ratings.txt fájllal elemzés, amíg moviedb.txt használt tooprovide felhasználóbarát szöveges információ hello elemzés eredményeinek hello megjelenítésekor.</span><span class="sxs-lookup"><span data-stu-id="41d90-133">hello user-ratings.txt file is used during analysis, while moviedb.txt is used tooprovide user-friendly text information when displaying hello results of hello analysis.</span></span>

<span data-ttu-id="41d90-134">hello felhasználói-ratings.txt szereplő adatok rendelkezik egy szerkezete `userID`, `movieID`, `userRating`, és `timestamp`, amely közli a gép magas hogyan minden felhasználó besorolású film.</span><span class="sxs-lookup"><span data-stu-id="41d90-134">hello data contained in user-ratings.txt has a structure of `userID`, `movieID`, `userRating`, and `timestamp`, which tells us how highly each user rated a movie.</span></span> <span data-ttu-id="41d90-135">Íme egy példa a hello adatok:</span><span class="sxs-lookup"><span data-stu-id="41d90-135">Here is an example of hello data:</span></span>

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

## <a name="run-hello-analysis"></a><span data-ttu-id="41d90-136">Hello futtatása</span><span class="sxs-lookup"><span data-stu-id="41d90-136">Run hello analysis</span></span>

<span data-ttu-id="41d90-137">SSH kapcsolat toohello fürtök a következő parancs toorun hello javaslat feladat hello használata:</span><span class="sxs-lookup"><span data-stu-id="41d90-137">From an SSH connection toohello cluster, use hello following command toorun hello recommendation job:</span></span>

```bash
mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp
```

> [!NOTE]
> <span data-ttu-id="41d90-138">hello feladat eltarthat néhány percig toocomplete, és előfordulhat, hogy több MapReduce-feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="41d90-138">hello job may take several minutes toocomplete, and may run multiple MapReduce jobs.</span></span>

## <a name="view-hello-output"></a><span data-ttu-id="41d90-139">Nézet hello kimeneti</span><span class="sxs-lookup"><span data-stu-id="41d90-139">View hello output</span></span>

1. <span data-ttu-id="41d90-140">Amikor hello feladat befejeződik, használja a következő tooview generált hello parancskimenet hello:</span><span class="sxs-lookup"><span data-stu-id="41d90-140">Once hello job completes, use hello following command tooview hello generated output:</span></span>

    ```bash
    hdfs dfs -text /example/data/mahoutout/part-r-00000
    ```

    <span data-ttu-id="41d90-141">hello kimenete a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="41d90-141">hello output appears as follows:</span></span>

        1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    <span data-ttu-id="41d90-142">hello első oszlopa hello `userID`.</span><span class="sxs-lookup"><span data-stu-id="41d90-142">hello first column is hello `userID`.</span></span> <span data-ttu-id="41d90-143">hello található értékek ' ["és"] "vannak `movieId`:`recommendationScore`.</span><span class="sxs-lookup"><span data-stu-id="41d90-143">hello values contained in '[' and ']' are `movieId`:`recommendationScore`.</span></span>

2. <span data-ttu-id="41d90-144">Hello kimeneti hello moviedb.txt, tooprovide együtt további információt használhatja hello ajánlása.</span><span class="sxs-lookup"><span data-stu-id="41d90-144">You can use hello output, along with hello moviedb.txt, tooprovide more information on hello recommendations.</span></span> <span data-ttu-id="41d90-145">Először létre kell toocopy hello fájlokat a helyileg hello a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="41d90-145">First, we need toocopy hello files locally using hello following commands:</span></span>

    ```bash
    hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
    hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .
    ```

    <span data-ttu-id="41d90-146">Ez a parancs másolatok hello kimeneti tooa adatfájlt nevű **recommendations.txt** hello aktuális könyvtárban, hello movie adatfájlok együtt.</span><span class="sxs-lookup"><span data-stu-id="41d90-146">This command copies hello output data tooa file named **recommendations.txt** in hello current directory, along with hello movie data files.</span></span>

3. <span data-ttu-id="41d90-147">A következő parancs toocreate egy Python-parancsfájl, amely megkeresi az movie nevek hello javaslatok kimeneti hello adatainak hello használata:</span><span class="sxs-lookup"><span data-stu-id="41d90-147">Use hello following command toocreate a Python script that looks up movie names for hello data in hello recommendations output:</span></span>

    ```bash
    nano show_recommendations.py
    ```

    <span data-ttu-id="41d90-148">Hello szerkesztő megnyitása után szöveg hello hello fájl tartalmát, a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="41d90-148">When hello editor opens, use hello following text as hello contents of hello file:</span></span>

   ```python
   #!/usr/bin/env python

   import sys

   if len(sys.argv) != 5:
        print "Arguments: userId userDataFilename movieFilename recommendationFilename"
        sys.exit(1)

   userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

   print "Reading Movies Descriptions"
   movieFile = open(movieFilename)
   movieById = {}
   for line in movieFile:
       tokens = line.split("|")
       movieById[tokens[0]] = tokens[1:]
   movieFile.close()

   print "Reading Rated Movies"
   userDataFile = open(userDataFilename)
   ratedMovieIds = []
   for line in userDataFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           ratedMovieIds.append((tokens[1],tokens[2]))
   userDataFile.close()

   print "Reading Recommendations"
   recommendationFile = open(recommendationFilename)
   recommendations = []
   for line in recommendationFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           movieIdAndScores = tokens[1].strip("[]\n").split(",")
           recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
           break
   recommendationFile.close()

   print "Rated Movies"
   print "------------------------"
   for movieId, rating in ratedMovieIds:
       print "%s, rating=%s" % (movieById[movieId][0], rating)
   print "------------------------"

   print "Recommended Movies"
   print "------------------------"
   for movieId, score in recommendations:
       print "%s, score=%s" % (movieById[movieId][0], score)
   print "------------------------"
   ```

    <span data-ttu-id="41d90-149">Nyomja le az **Ctrl-X**, **Y**, és végül **Enter** toosave hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="41d90-149">Press **Ctrl-X**, **Y**, and finally **Enter** toosave hello data.</span></span>

4. <span data-ttu-id="41d90-150">Hello Python-parancsfájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="41d90-150">Run hello Python script.</span></span> <span data-ttu-id="41d90-151">hello alábbi parancs feltételezi, hogy hello könyvtárban, ahol az összes hello fájl letöltése:</span><span class="sxs-lookup"><span data-stu-id="41d90-151">hello following command assumes you are in hello directory where all hello files were downloaded:</span></span>

    ```bash
    python show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt
    ```

    <span data-ttu-id="41d90-152">Ez a parancs ellenőrzi, hogy az hello javaslatok a felhasználói azonosító 4 jön létre.</span><span class="sxs-lookup"><span data-stu-id="41d90-152">This command looks at hello recommendations generated for user ID 4.</span></span>

    * <span data-ttu-id="41d90-153">Hello **felhasználói-ratings.txt** fájl értékelése használt tooretrieve filmek.</span><span class="sxs-lookup"><span data-stu-id="41d90-153">hello **user-ratings.txt** file is used tooretrieve movies that have been rated.</span></span>

    * <span data-ttu-id="41d90-154">Hello **moviedb.txt** fájl hello filmek használt tooretrieve hello nevét.</span><span class="sxs-lookup"><span data-stu-id="41d90-154">hello **moviedb.txt** file is used tooretrieve hello names of hello movies.</span></span>

    * <span data-ttu-id="41d90-155">Hello **recommendations.txt** használt tooretrieve hello movie javaslatok a felhasználó van.</span><span class="sxs-lookup"><span data-stu-id="41d90-155">hello **recommendations.txt** is used tooretrieve hello movie recommendations for this user.</span></span>

     <span data-ttu-id="41d90-156">a kimeneti hello ezt a parancsot a rendszer hasonló toohello a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="41d90-156">hello output from this command is similar toohello following text:</span></span>

        <span data-ttu-id="41d90-157">Tibet (1997), a hét éven pontozása = 5.0 Indiana János és hello utolsó Crusade (1989) pontozása = 5.0 Jaws (1975) pontszám = 5.0 logika és Sensibility (1995) pontszám = 5.0 függetlenség napja (ID4) (1996) pontszám = 5.0 a legjobb barátjának Esküvői (1997), pontozása = 5.0 Jerry Maguire (1996 ), pontszám = 5.0 Scream 2 (1997) pontszám = 5.0 idő tooKill, egy (1996), pontozása = 5.0</span><span class="sxs-lookup"><span data-stu-id="41d90-157">Seven Years in Tibet (1997), score=5.0   Indiana Jones and hello Last Crusade (1989), score=5.0   Jaws (1975), score=5.0   Sense and Sensibility (1995), score=5.0   Independence Day (ID4) (1996), score=5.0   My Best Friend's Wedding (1997), score=5.0   Jerry Maguire (1996), score=5.0   Scream 2 (1997), score=5.0   Time tooKill, A (1996), score=5.0</span></span>

## <a name="delete-temporary-data"></a><span data-ttu-id="41d90-158">Ideiglenes adatok törlése</span><span class="sxs-lookup"><span data-stu-id="41d90-158">Delete temporary data</span></span>

<span data-ttu-id="41d90-159">Mahout feladatok hello feladat feldolgozása közben létrehozott ideiglenes adatok nem távolítja el.</span><span class="sxs-lookup"><span data-stu-id="41d90-159">Mahout jobs do not remove temporary data that is created while processing hello job.</span></span> <span data-ttu-id="41d90-160">Hello `--tempDir` paraméter megadott hello példa tooisolate hello ideiglenes feladatfájlokhoz könnyen törlésre megadott elérési útra.</span><span class="sxs-lookup"><span data-stu-id="41d90-160">hello `--tempDir` parameter is specified in hello example job tooisolate hello temporary files into a specific path for easy deletion.</span></span> <span data-ttu-id="41d90-161">tooremove hello ideiglenes fájlok, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="41d90-161">tooremove hello temp files, use hello following command:</span></span>

```bash
hdfs dfs -rm -f -r /temp/mahouttemp
```

> [!WARNING]
> <span data-ttu-id="41d90-162">Ha újra toorun hello parancs, hello kimeneti könyvtár is törölnie kell.</span><span class="sxs-lookup"><span data-stu-id="41d90-162">If you want toorun hello command again, you must also delete hello output directory.</span></span> <span data-ttu-id="41d90-163">Használja a következő toodelete hello ebben a könyvtárban:</span><span class="sxs-lookup"><span data-stu-id="41d90-163">Use hello following toodelete this directory:</span></span>
>
> `hdfs dfs -rm -f -r /example/data/mahoutout`


## <a name="next-steps"></a><span data-ttu-id="41d90-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41d90-164">Next steps</span></span>

<span data-ttu-id="41d90-165">Most, hogy megtanulta, hogyan toouse Mahout, felderíteni a más módon a HDInsight-adatokkal:</span><span class="sxs-lookup"><span data-stu-id="41d90-165">Now that you have learned how toouse Mahout, discover other ways of working with data on HDInsight:</span></span>

* [<span data-ttu-id="41d90-166">A HDInsight Hive</span><span class="sxs-lookup"><span data-stu-id="41d90-166">Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="41d90-167">Pig with HDInsight</span><span class="sxs-lookup"><span data-stu-id="41d90-167">Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="41d90-168">MapReduce a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="41d90-168">MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
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
