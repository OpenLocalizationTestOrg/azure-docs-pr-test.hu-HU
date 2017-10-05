---
title: "Mahout és-(SSH) HDInsight - Azure javaslatok generálása |} Microsoft Docs"
description: "Megtudhatja, hogyan használja az Apache Mahout machine learning-könyvtárral movie javaslatok és a HDInsight (Hadoop) együttes létrehozásához."
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
ms.openlocfilehash: 28450d72f19a5467d88bc787d11f6c37c5afbf9a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight-ssh"></a><span data-ttu-id="f3417-103">Film javaslatok generálása Apache Mahout Linux-alapú hadooppal a HDInsight-(SSH) használatával</span><span class="sxs-lookup"><span data-stu-id="f3417-103">Generate movie recommendations by using Apache Mahout with Linux-based Hadoop in HDInsight (SSH)</span></span>

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

<span data-ttu-id="f3417-104">Ismerje meg, hogyan használható a [Apache Mahout](http://mahout.apache.org) machine learning függvénytár, amely Azure HDInsight movie javaslatok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f3417-104">Learn how to use the [Apache Mahout](http://mahout.apache.org) machine learning library with Azure HDInsight to generate movie recommendations.</span></span>

<span data-ttu-id="f3417-105">Mahout van egy [gépi tanulás] [ ml] könyvtára Apache Hadoop.</span><span class="sxs-lookup"><span data-stu-id="f3417-105">Mahout is a [machine learning][ml] library for Apache Hadoop.</span></span> <span data-ttu-id="f3417-106">Mahout adatok, például a szűrést, besorolást, és a fürtszolgáltatás feldolgozására algoritmusok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f3417-106">Mahout contains algorithms for processing data, such as filtering, classification, and clustering.</span></span> <span data-ttu-id="f3417-107">Ebben a cikkben egy javaslat motor movie javaslatok az ismerősök láthatta filmek alapuló létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="f3417-107">In this article, you use a recommendation engine to generate movie recommendations that are based on movies your friends have seen.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f3417-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f3417-108">Prerequisites</span></span>

* <span data-ttu-id="f3417-109">A Linux-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="f3417-109">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="f3417-110">Egy létrehozásával kapcsolatos további információkért lásd: [hdinsight Linux-alapú Hadoop használatának megkezdésében][getstarted].</span><span class="sxs-lookup"><span data-stu-id="f3417-110">For information about creating one, see [Get started using Linux-based Hadoop in HDInsight][getstarted].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f3417-111">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="f3417-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f3417-112">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f3417-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="f3417-113">Egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="f3417-113">An SSH client.</span></span> <span data-ttu-id="f3417-114">További információ: [SSH használata a HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="f3417-114">For more information, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

## <a name="mahout-versioning"></a><span data-ttu-id="f3417-115">Mahout versioning</span><span class="sxs-lookup"><span data-stu-id="f3417-115">Mahout versioning</span></span>

<span data-ttu-id="f3417-116">A hdinsight Mahout verziójával kapcsolatos további információkért lásd: [HDInsight verziója és a Hadoop-összetevők](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="f3417-116">For more information about the version of Mahout in HDInsight, see [HDInsight versions and Hadoop components](hdinsight-component-versioning.md).</span></span>

## <span data-ttu-id="f3417-117"><a name="recommendations"></a>Understanding javaslatok</span><span class="sxs-lookup"><span data-stu-id="f3417-117"><a name="recommendations"></a>Understanding recommendations</span></span>

<span data-ttu-id="f3417-118">A funkciók Mahout által biztosított egyike egy javaslat motor.</span><span class="sxs-lookup"><span data-stu-id="f3417-118">One of the functions that is provided by Mahout is a recommendation engine.</span></span> <span data-ttu-id="f3417-119">Ez a motor elfogadja a kell adatokat `userID`, `itemId`, és `prefValue` (a beállítások a cikkhez).</span><span class="sxs-lookup"><span data-stu-id="f3417-119">This engine accepts data in the format of `userID`, `itemId`, and `prefValue` (the preference for the item).</span></span> <span data-ttu-id="f3417-120">Mahout végezhet el, közös elemként elemzés meghatározásához: *egy elem előnyben rendelkező felhasználók is hozzáférhetnek ezeket a beállításokat szabályozó egyéb elemek*.</span><span class="sxs-lookup"><span data-stu-id="f3417-120">Mahout can then perform co-occurance analysis to determine: *users who have a preference for an item also have a preference for these other items*.</span></span> <span data-ttu-id="f3417-121">Mahout majd határozza meg a felhasználók hasonló-cikk beállítások, amely ajánlásokat is használható.</span><span class="sxs-lookup"><span data-stu-id="f3417-121">Mahout then determines users with like-item preferences, which can be used to make recommendations.</span></span>

<span data-ttu-id="f3417-122">Az alábbi munkafolyamat egy egyszerűsített példa movie adatait használó:</span><span class="sxs-lookup"><span data-stu-id="f3417-122">The following workflow is a simplified example that uses movie data:</span></span>

* <span data-ttu-id="f3417-123">**Közös elemként**: Joe, Ágnes és minden tetszését Bob *csillag ütközések*, *vissza sztrájkok a Empire*, és *a Jedi visszaküldése*.</span><span class="sxs-lookup"><span data-stu-id="f3417-123">**Co-occurance**: Joe, Alice, and Bob all liked *Star Wars*, *The Empire Strikes Back*, and *Return of the Jedi*.</span></span> <span data-ttu-id="f3417-124">Mahout határozza meg, hogy a felhasználók számára is, például a filmek egyikét sem például a másik kettőt.</span><span class="sxs-lookup"><span data-stu-id="f3417-124">Mahout determines that users who like any one of these movies also like the other two.</span></span>

* <span data-ttu-id="f3417-125">**Közös elemként**: Bálint és Alice is tetszését *a látszólagos támadása*, *támadás a klónja*, és *a Sith a megtorlás*.</span><span class="sxs-lookup"><span data-stu-id="f3417-125">**Co-occurance**: Bob and Alice also liked *The Phantom Menace*, *Attack of the Clones*, and *Revenge of the Sith*.</span></span> <span data-ttu-id="f3417-126">Mahout határozza meg, hogy felhasználók, akik az előző három filmek is tetszését hasonlóan ezen három filmek.</span><span class="sxs-lookup"><span data-stu-id="f3417-126">Mahout determines that users who liked the previous three movies also like these three movies.</span></span>

* <span data-ttu-id="f3417-127">**Hasonlóság ajánlás**: mivel Joe tetszését az első három filmek, Mahout ellenőrzi, hogy az filmek, hogy más, hasonló beállítások tetszett, de Joe rendelkezik nem figyelt (tetszését/névleges).</span><span class="sxs-lookup"><span data-stu-id="f3417-127">**Similarity recommendation**: Because Joe liked the first three movies, Mahout looks at movies that others with similar preferences liked, but Joe has not watched (liked/rated).</span></span> <span data-ttu-id="f3417-128">Ebben az esetben Mahout javasolja *a látszólagos támadása*, *támadás a klónja*, és *a Sith a megtorlás*.</span><span class="sxs-lookup"><span data-stu-id="f3417-128">In this case, Mahout recommends *The Phantom Menace*, *Attack of the Clones*, and *Revenge of the Sith*.</span></span>

### <a name="understanding-the-data"></a><span data-ttu-id="f3417-129">Az adatok ismertetése</span><span class="sxs-lookup"><span data-stu-id="f3417-129">Understanding the data</span></span>

<span data-ttu-id="f3417-130">Kényelmesen [GroupLens kutatási] [ movielens] minősítés adatokat biztosít a filmek formátuma nem kompatibilis a Mahout.</span><span class="sxs-lookup"><span data-stu-id="f3417-130">Conveniently, [GroupLens Research][movielens] provides rating data for movies in a format that is compatible with Mahout.</span></span> <span data-ttu-id="f3417-131">Ezek az adatok érhető el a fürt alapértelmezett tárolás `/HdiSamples/HdiSamples/MahoutMovieData`.</span><span class="sxs-lookup"><span data-stu-id="f3417-131">This data is available on your cluster's default storage at `/HdiSamples/HdiSamples/MahoutMovieData`.</span></span>

<span data-ttu-id="f3417-132">Két fájlt `moviedb.txt` és `user-ratings.txt`.</span><span class="sxs-lookup"><span data-stu-id="f3417-132">There are two files, `moviedb.txt` and `user-ratings.txt`.</span></span> <span data-ttu-id="f3417-133">A felhasználó-ratings.txt fájllal elemzés, míg az moviedb.txt felhasználóbarát szöveg információk megadására, amikor a vizsgálat eredményeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f3417-133">The user-ratings.txt file is used during analysis, while moviedb.txt is used to provide user-friendly text information when displaying the results of the analysis.</span></span>

<span data-ttu-id="f3417-134">A felhasználó-ratings.txt szereplő adatok struktúrája `userID`, `movieID`, `userRating`, és `timestamp`, amely közli a gép magas hogyan minden felhasználó besorolású film.</span><span class="sxs-lookup"><span data-stu-id="f3417-134">The data contained in user-ratings.txt has a structure of `userID`, `movieID`, `userRating`, and `timestamp`, which tells us how highly each user rated a movie.</span></span> <span data-ttu-id="f3417-135">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="f3417-135">Here is an example of the data:</span></span>

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

## <a name="run-the-analysis"></a><span data-ttu-id="f3417-136">Az elemzés futtatása</span><span class="sxs-lookup"><span data-stu-id="f3417-136">Run the analysis</span></span>

<span data-ttu-id="f3417-137">Az SSH-kapcsolatot a fürt alkalmazás a javaslat feladat futtatásához a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="f3417-137">From an SSH connection to the cluster, use the following command to run the recommendation job:</span></span>

```bash
mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp
```

> [!NOTE]
> <span data-ttu-id="f3417-138">A feladat is igénybe vehet néhány percet, és előfordulhat, hogy több MapReduce-feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="f3417-138">The job may take several minutes to complete, and may run multiple MapReduce jobs.</span></span>

## <a name="view-the-output"></a><span data-ttu-id="f3417-139">A kimeneti megtekintése</span><span class="sxs-lookup"><span data-stu-id="f3417-139">View the output</span></span>

1. <span data-ttu-id="f3417-140">Ha a feladat befejeződik, a következő paranccsal létrehozott eredményének megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="f3417-140">Once the job completes, use the following command to view the generated output:</span></span>

    ```bash
    hdfs dfs -text /example/data/mahoutout/part-r-00000
    ```

    <span data-ttu-id="f3417-141">A kimenet a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="f3417-141">The output appears as follows:</span></span>

        1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    <span data-ttu-id="f3417-142">Az első oszlop a `userID`.</span><span class="sxs-lookup"><span data-stu-id="f3417-142">The first column is the `userID`.</span></span> <span data-ttu-id="f3417-143">Szereplő értékeket ' ["és"] "vannak `movieId`:`recommendationScore`.</span><span class="sxs-lookup"><span data-stu-id="f3417-143">The values contained in '[' and ']' are `movieId`:`recommendationScore`.</span></span>

2. <span data-ttu-id="f3417-144">A kimeneti együtt a moviedb.txt segítségével nyújtanak további információt a ajánlása.</span><span class="sxs-lookup"><span data-stu-id="f3417-144">You can use the output, along with the moviedb.txt, to provide more information on the recommendations.</span></span> <span data-ttu-id="f3417-145">Először igazolnia kell átmásolni a fájlokat helyileg az alábbi parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="f3417-145">First, we need to copy the files locally using the following commands:</span></span>

    ```bash
    hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
    hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .
    ```

    <span data-ttu-id="f3417-146">Ez a parancs nevű fájlt másolja a kimeneti adatokat **recommendations.txt** az aktuális könyvtárban található, a movie adatfájlok együtt.</span><span class="sxs-lookup"><span data-stu-id="f3417-146">This command copies the output data to a file named **recommendations.txt** in the current directory, along with the movie data files.</span></span>

3. <span data-ttu-id="f3417-147">Az alábbi parancs segítségével hozzon létre egy Python-parancsfájl, amely megkeresi a javaslatok kimenet movie neve:</span><span class="sxs-lookup"><span data-stu-id="f3417-147">Use the following command to create a Python script that looks up movie names for the data in the recommendations output:</span></span>

    ```bash
    nano show_recommendations.py
    ```

    <span data-ttu-id="f3417-148">A szerkesztő megnyitása után használata a fájl tartalmát a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="f3417-148">When the editor opens, use the following text as the contents of the file:</span></span>

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

    <span data-ttu-id="f3417-149">Nyomja le az **Ctrl-X**, **Y**, és végül **Enter** az adatok mentése.</span><span class="sxs-lookup"><span data-stu-id="f3417-149">Press **Ctrl-X**, **Y**, and finally **Enter** to save the data.</span></span>

4. <span data-ttu-id="f3417-150">A Python-parancsfájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="f3417-150">Run the Python script.</span></span> <span data-ttu-id="f3417-151">Az alábbi parancs feltételezi, hogy a könyvtárban, ahol az összes fájl letöltése:</span><span class="sxs-lookup"><span data-stu-id="f3417-151">The following command assumes you are in the directory where all the files were downloaded:</span></span>

    ```bash
    python show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt
    ```

    <span data-ttu-id="f3417-152">Ez a parancs ellenőrzi, hogy az a felhasználói azonosító 4 előállított javaslatok.</span><span class="sxs-lookup"><span data-stu-id="f3417-152">This command looks at the recommendations generated for user ID 4.</span></span>

    * <span data-ttu-id="f3417-153">A **felhasználói-ratings.txt** fájl értékelése filmek beolvasásához használt.</span><span class="sxs-lookup"><span data-stu-id="f3417-153">The **user-ratings.txt** file is used to retrieve movies that have been rated.</span></span>

    * <span data-ttu-id="f3417-154">A **moviedb.txt** fájl segítségével olvassa be a filmek.</span><span class="sxs-lookup"><span data-stu-id="f3417-154">The **moviedb.txt** file is used to retrieve the names of the movies.</span></span>

    * <span data-ttu-id="f3417-155">A **recommendations.txt** a felhasználó movie ajánlások beolvasásához használt.</span><span class="sxs-lookup"><span data-stu-id="f3417-155">The **recommendations.txt** is used to retrieve the movie recommendations for this user.</span></span>

     <span data-ttu-id="f3417-156">A parancs kimenete az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="f3417-156">The output from this command is similar to the following text:</span></span>

        <span data-ttu-id="f3417-157">Tibet (1997), a hét éven pontozása = 5.0 pontozása Indiana János és az utolsó Crusade (1989), = 5.0 Jaws (1975) pontozása = 5.0 logika és Sensibility (1995) pontszám = 5.0 függetlenség napja (ID4) (1996) pontszám = 5.0 a legjobb barátjának Esküvői (1997), pontozása = 5.0 Jerry Maguire (1996) pontszám = 5.0 Scream 2 (1997) pontszám = 5.0 pontozása Kill, az idő A (1996), = 5.0</span><span class="sxs-lookup"><span data-stu-id="f3417-157">Seven Years in Tibet (1997), score=5.0   Indiana Jones and the Last Crusade (1989), score=5.0   Jaws (1975), score=5.0   Sense and Sensibility (1995), score=5.0   Independence Day (ID4) (1996), score=5.0   My Best Friend's Wedding (1997), score=5.0   Jerry Maguire (1996), score=5.0   Scream 2 (1997), score=5.0   Time to Kill, A (1996), score=5.0</span></span>

## <a name="delete-temporary-data"></a><span data-ttu-id="f3417-158">Ideiglenes adatok törlése</span><span class="sxs-lookup"><span data-stu-id="f3417-158">Delete temporary data</span></span>

<span data-ttu-id="f3417-159">Mahout feladatok ne távolítsa el a feldolgozás során létrehozott ideiglenes adatok.</span><span class="sxs-lookup"><span data-stu-id="f3417-159">Mahout jobs do not remove temporary data that is created while processing the job.</span></span> <span data-ttu-id="f3417-160">A `--tempDir` paraméter van megadva a példa feladat különítheti el az ideiglenes fájlokat egyszerűen a törlésre megadott elérési útra.</span><span class="sxs-lookup"><span data-stu-id="f3417-160">The `--tempDir` parameter is specified in the example job to isolate the temporary files into a specific path for easy deletion.</span></span> <span data-ttu-id="f3417-161">Az ideiglenes fájlok eltávolításához használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="f3417-161">To remove the temp files, use the following command:</span></span>

```bash
hdfs dfs -rm -f -r /temp/mahouttemp
```

> [!WARNING]
> <span data-ttu-id="f3417-162">Ha azt szeretné, hogy futtassa újra a parancsot, a kimeneti könyvtár is törölnie kell.</span><span class="sxs-lookup"><span data-stu-id="f3417-162">If you want to run the command again, you must also delete the output directory.</span></span> <span data-ttu-id="f3417-163">A címtár törléséhez használja a következőket:</span><span class="sxs-lookup"><span data-stu-id="f3417-163">Use the following to delete this directory:</span></span>
>
> `hdfs dfs -rm -f -r /example/data/mahoutout`


## <a name="next-steps"></a><span data-ttu-id="f3417-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f3417-164">Next steps</span></span>

<span data-ttu-id="f3417-165">Most, hogy rendelkezik megismerte Mahout használata, felderítése egyéb módjait a HDInsight-adatokkal végzett munka:</span><span class="sxs-lookup"><span data-stu-id="f3417-165">Now that you have learned how to use Mahout, discover other ways of working with data on HDInsight:</span></span>

* [<span data-ttu-id="f3417-166">A HDInsight Hive</span><span class="sxs-lookup"><span data-stu-id="f3417-166">Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="f3417-167">Pig with HDInsight</span><span class="sxs-lookup"><span data-stu-id="f3417-167">Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="f3417-168">MapReduce a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="f3417-168">MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

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
