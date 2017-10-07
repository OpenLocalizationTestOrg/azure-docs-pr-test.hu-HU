---
title: "a HDInsight - Azure aaaRun Hadoop MapReduce példák |} Microsoft Docs"
description: "MapReduce-minták a HDInsight szereplő jar-fájlok az első lépéseiben. SSH tooconnect toohello fürtöt használ, és aztán hello Hadoop parancs toorun mintafeladatok."
keywords: "hadoop példa jar, a hadoop példák jar, a hadoop-mapreduce példák, a mapreduce példák"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e1d2a0b9-1659-4fab-921e-4a8990cbb30a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 7a16bbd51eb17570fcaa3b1e0f5990fa889c106a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-hello-mapreduce-examples-included-in-hdinsight"></a><span data-ttu-id="39352-105">Futtassa a HDInsight szereplő hello MapReduce példák</span><span class="sxs-lookup"><span data-stu-id="39352-105">Run hello MapReduce examples included in HDInsight</span></span>

[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

<span data-ttu-id="39352-106">Ismerje meg, hogyan toorun hello MapReduce példák a HDInsight Hadoop része.</span><span class="sxs-lookup"><span data-stu-id="39352-106">Learn how toorun hello MapReduce examples included with Hadoop on HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="39352-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="39352-107">Prerequisites</span></span>

* <span data-ttu-id="39352-108">**HDInsight-fürtök**: lásd: [Hadoop használatának megkezdésében a Hive HDInsight Linux rendszeren](hdinsight-hadoop-linux-tutorial-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="39352-108">**An HDInsight cluster**: See [Get started using Hadoop with Hive in HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="39352-109">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="39352-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="39352-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="39352-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="39352-111">**Egy SSH-ügyfél**: további információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="39352-111">**An SSH client**: For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="hello-mapreduce-examples"></a><span data-ttu-id="39352-112">hello MapReduce példák</span><span class="sxs-lookup"><span data-stu-id="39352-112">hello MapReduce examples</span></span>

<span data-ttu-id="39352-113">**Hely**: hello minták a HDInsight-fürtöt: hello lévő `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.</span><span class="sxs-lookup"><span data-stu-id="39352-113">**Location**: hello samples are located on hello HDInsight cluster at `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.</span></span>

<span data-ttu-id="39352-114">**Tartalom**: a következő minták hello ebből az archívumból tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="39352-114">**Contents**: hello following samples are contained in this archive:</span></span>

* <span data-ttu-id="39352-115">`aggregatewordcount`: Egy összesítés mapreduce program, amely a bemeneti fájlok hello hello szavak száma alapján.</span><span class="sxs-lookup"><span data-stu-id="39352-115">`aggregatewordcount`: An Aggregate based mapreduce program that counts hello words in hello input files.</span></span>
* <span data-ttu-id="39352-116">`aggregatewordhist`: Egy összesítés alapján kiszámítja a bemeneti fájlok hello hello szavak hello hisztogram mapreduce program.</span><span class="sxs-lookup"><span data-stu-id="39352-116">`aggregatewordhist`: An Aggregate based mapreduce program that computes hello histogram of hello words in hello input files.</span></span>
* <span data-ttu-id="39352-117">`bbp`: A Pi Bailey-Borwein-Plouffe toocompute számjegyeket használó a mapreduce programot.</span><span class="sxs-lookup"><span data-stu-id="39352-117">`bbp`: A mapreduce program that uses Bailey-Borwein-Plouffe toocompute exact digits of Pi.</span></span>
* <span data-ttu-id="39352-118">`dbcount`: Egy példa a feladat, amely megjeleníti az adatbázisban tárolt hello pageview naplók.</span><span class="sxs-lookup"><span data-stu-id="39352-118">`dbcount`: An example job that counts hello pageview logs stored in a database.</span></span>
* <span data-ttu-id="39352-119">`distbbp`: A Pi bit pontos BBP típusú képlet toocompute használó a mapreduce programot.</span><span class="sxs-lookup"><span data-stu-id="39352-119">`distbbp`: A mapreduce program that uses a BBP-type formula toocompute exact bits of Pi.</span></span>
* <span data-ttu-id="39352-120">`grep`: A mapreduce program hello megadó megegyezik a regex hello bemeneti a.</span><span class="sxs-lookup"><span data-stu-id="39352-120">`grep`: A mapreduce program that counts hello matches of a regex in hello input.</span></span>
* <span data-ttu-id="39352-121">`join`: Egy feladatot, amely végrehajtja a való csatlakozást keresztül rendezve, adatkészletek egyaránt particionálva.</span><span class="sxs-lookup"><span data-stu-id="39352-121">`join`: A job that performs a join over sorted, equally partitioned datasets.</span></span>
* <span data-ttu-id="39352-122">`multifilewc`: Egy feladatot, amely a szavakat több fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="39352-122">`multifilewc`: A job that counts words from several files.</span></span>
* <span data-ttu-id="39352-123">`pentomino`: A mapreduce csempe elrendezése program toofind megoldások toopentomino problémákat.</span><span class="sxs-lookup"><span data-stu-id="39352-123">`pentomino`: A mapreduce tile laying program toofind solutions toopentomino problems.</span></span>
* <span data-ttu-id="39352-124">`pi`: A mapreduce program, amely segítségével látszólagos Monte Pi becslése Carlo metódust.</span><span class="sxs-lookup"><span data-stu-id="39352-124">`pi`: A mapreduce program that estimates Pi using a quasi-Monte Carlo method.</span></span>
* <span data-ttu-id="39352-125">`randomtextwriter`: A mapreduce program 10 GB-nyi véletlenszerű szöveges adatok csomópontonként írja.</span><span class="sxs-lookup"><span data-stu-id="39352-125">`randomtextwriter`: A mapreduce program that writes 10 GB of random textual data per node.</span></span>
* <span data-ttu-id="39352-126">`randomwriter`: A mapreduce program 10 GB-nyi véletlenszerű adatokat csomópontonként írja.</span><span class="sxs-lookup"><span data-stu-id="39352-126">`randomwriter`: A mapreduce program that writes 10 GB of random data per node.</span></span>
* <span data-ttu-id="39352-127">`secondarysort`: Példa egy másodlagos rendezési toohello meghatározása csökkenti fázisban.</span><span class="sxs-lookup"><span data-stu-id="39352-127">`secondarysort`: An example defining a secondary sort toohello reduce phase.</span></span>
* <span data-ttu-id="39352-128">`sort`: Mapreduce program hello adatok hello véletlenszerű írási szerint rendezi.</span><span class="sxs-lookup"><span data-stu-id="39352-128">`sort`: A mapreduce program that sorts hello data written by hello random writer.</span></span>
* <span data-ttu-id="39352-129">`sudoku`: A sudoku solver.</span><span class="sxs-lookup"><span data-stu-id="39352-129">`sudoku`: A sudoku solver.</span></span>
* <span data-ttu-id="39352-130">`teragen`: Hello terasort adatainak létrehozása.</span><span class="sxs-lookup"><span data-stu-id="39352-130">`teragen`: Generate data for hello terasort.</span></span>
* <span data-ttu-id="39352-131">`terasort`: Hello terasort futtatásához.</span><span class="sxs-lookup"><span data-stu-id="39352-131">`terasort`: Run hello terasort.</span></span>
* <span data-ttu-id="39352-132">`teravalidate`: Terasort eredményeinek ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="39352-132">`teravalidate`: Checking results of terasort.</span></span>
* <span data-ttu-id="39352-133">`wordcount`: Mapreduce program hello szavak hello bemeneti fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="39352-133">`wordcount`: A mapreduce program that counts hello words in hello input files.</span></span>
* <span data-ttu-id="39352-134">`wordmean`: Mapreduce program hello átlagos hossza hello szavak hello bemeneti fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="39352-134">`wordmean`: A mapreduce program that counts hello average length of hello words in hello input files.</span></span>
* <span data-ttu-id="39352-135">`wordmedian`: Mapreduce program hello közepes hossza hello szavak hello bemeneti fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="39352-135">`wordmedian`: A mapreduce program that counts hello median length of hello words in hello input files.</span></span>
* <span data-ttu-id="39352-136">`wordstandarddeviation`: Mapreduce program hello szórása hello hosszát hello szavak hello bemeneti fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="39352-136">`wordstandarddeviation`: A mapreduce program that counts hello standard deviation of hello length of hello words in hello input files.</span></span>

<span data-ttu-id="39352-137">**Forráskód**: HDInsight-fürtöt: hello szerepel-e ezeket a mintákat forráskódja `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.</span><span class="sxs-lookup"><span data-stu-id="39352-137">**Source code**: Source code for these samples is included on hello HDInsight cluster at `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.</span></span>

> [!NOTE]
> <span data-ttu-id="39352-138">Hello `2.2.4.9-1` hello elérési hello verziója hello Hortonworks Data Platform hello HDInsight-fürthöz, és lehet, hogy a fürt másik.</span><span class="sxs-lookup"><span data-stu-id="39352-138">hello `2.2.4.9-1` in hello path is hello version of hello Hortonworks Data Platform for hello HDInsight cluster, and may be different for your cluster.</span></span>

## <a name="run-hello-wordcount-example"></a><span data-ttu-id="39352-139">Futtassa a hello wordcount-példa</span><span class="sxs-lookup"><span data-stu-id="39352-139">Run hello wordcount example</span></span>

1. <span data-ttu-id="39352-140">Csatlakozzon az SSH használatával tooHDInsight.</span><span class="sxs-lookup"><span data-stu-id="39352-140">Connect tooHDInsight using SSH.</span></span> <span data-ttu-id="39352-141">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="39352-141">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="39352-142">A hello `username@#######:~$` kérni, használja a következő Példaparancsok toolist hello hello:</span><span class="sxs-lookup"><span data-stu-id="39352-142">From hello `username@#######:~$` prompt, use hello following command toolist hello samples:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    <span data-ttu-id="39352-143">Ez a parancs a dokumentum korábbi szakaszában hello minta hello listát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="39352-143">This command generates hello list of sample from hello previous section of this document.</span></span>

3. <span data-ttu-id="39352-144">Használjon hello következő parancsot a tooget súgó adott mintán.</span><span class="sxs-lookup"><span data-stu-id="39352-144">Use hello following command tooget help on a specific sample.</span></span> <span data-ttu-id="39352-145">Ebben az esetben hello **wordcount** minta:</span><span class="sxs-lookup"><span data-stu-id="39352-145">In this case, hello **wordcount** sample:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    <span data-ttu-id="39352-146">Hello a következő üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="39352-146">You receive hello following message:</span></span>

        Usage: wordcount <in> [<in>...] <out>

    <span data-ttu-id="39352-147">Ez az üzenet azt jelzi, hogy több bemeneti elérési utak biztosíthat hello forrás dokumentumokhoz.</span><span class="sxs-lookup"><span data-stu-id="39352-147">This message indicates that you can provide several input paths for hello source documents.</span></span> <span data-ttu-id="39352-148">hello végső elérési út hello kimeneti (szavak hello forrás dokumentumok száma) tárolására.</span><span class="sxs-lookup"><span data-stu-id="39352-148">hello final path is where hello output (count of words in hello source documents) is stored.</span></span>

4. <span data-ttu-id="39352-149">A következő toocount hello minden szó használjon hello notebookok a Leonardo Da Vinci, amely vannak megadva, a mintaadatok a fürthöz:</span><span class="sxs-lookup"><span data-stu-id="39352-149">Use hello following toocount all words in hello Notebooks of Leonardo Da Vinci, which are provided as sample data with your cluster:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    <span data-ttu-id="39352-150">Ez a feladat olvasása az adatokat `/example/data/gutenberg/davinci.txt`.</span><span class="sxs-lookup"><span data-stu-id="39352-150">Input for this job is read from `/example/data/gutenberg/davinci.txt`.</span></span> <span data-ttu-id="39352-151">Az ebben a példában tárolódik kimeneti `/example/data/davinciwordcount`.</span><span class="sxs-lookup"><span data-stu-id="39352-151">Output for this example is stored in `/example/data/davinciwordcount`.</span></span> <span data-ttu-id="39352-152">Mindkét elérési utak alapértelmezett tárolási hello fürt esetén nem hello helyi fájlrendszerben található.</span><span class="sxs-lookup"><span data-stu-id="39352-152">Both paths are located on default storage for hello cluster, not hello local file system.</span></span>

   > [!NOTE]
   > <span data-ttu-id="39352-153">Amint hello wordcount minta hello súgóját, több bemeneti fájl is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="39352-153">As noted in hello help for hello wordcount sample, you could also specify multiple input files.</span></span> <span data-ttu-id="39352-154">Például `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` számolja davinci.txt és ulysses.txt szavakat.</span><span class="sxs-lookup"><span data-stu-id="39352-154">For example, `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` would count words in both davinci.txt and ulysses.txt.</span></span>

5. <span data-ttu-id="39352-155">Amikor hello feladat befejeződik, használja a következő tooview hello parancskimenet hello:</span><span class="sxs-lookup"><span data-stu-id="39352-155">Once hello job completes, use hello following command tooview hello output:</span></span>

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    <span data-ttu-id="39352-156">Ez a parancs hello feladat által létrehozott összes hello kimeneti fájlok fűzi össze.</span><span class="sxs-lookup"><span data-stu-id="39352-156">This command concatenates all hello output files produced by hello job.</span></span> <span data-ttu-id="39352-157">Hello kimeneti toohello konzol jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="39352-157">It displays hello output toohello console.</span></span> <span data-ttu-id="39352-158">a kimeneti hello hasonló toohello a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="39352-158">hello output is similar toohello following text:</span></span>

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    <span data-ttu-id="39352-159">Soronként egy word és hányszor történt a hello a bemeneti adatok jelöli.</span><span class="sxs-lookup"><span data-stu-id="39352-159">Each line represents a word and how many times it occurred in hello input data.</span></span>

## <a name="hello-sudoku-example"></a><span data-ttu-id="39352-160">hello Sudoku – példa</span><span class="sxs-lookup"><span data-stu-id="39352-160">hello Sudoku example</span></span>

<span data-ttu-id="39352-161">[Sudoku](https://en.wikipedia.org/wiki/Sudoku) egy logikai kirakós kilenc 3 x 3 rácsok áll.</span><span class="sxs-lookup"><span data-stu-id="39352-161">[Sudoku](https://en.wikipedia.org/wiki/Sudoku) is a logic puzzle made up of nine 3x3 grids.</span></span> <span data-ttu-id="39352-162">Néhány hello rács celláinak számok, amíg üres, és hello célja az üres cellák hello toosolve.</span><span class="sxs-lookup"><span data-stu-id="39352-162">Some cells in hello grid have numbers, while others are blank, and hello goal is toosolve for hello blank cells.</span></span> <span data-ttu-id="39352-163">hello előző hivatkozás hello Kirakós további információkat, de ez a minta hello célját toosolve hello üres cellák.</span><span class="sxs-lookup"><span data-stu-id="39352-163">hello previous link has more information on hello puzzle, but hello purpose of this sample is toosolve for hello blank cells.</span></span> <span data-ttu-id="39352-164">A bemeneti úgy, hogy megtalálható-e a következő formátumban hello fájlnak kell lennie:</span><span class="sxs-lookup"><span data-stu-id="39352-164">So our input should be a file that is in hello following format:</span></span>

* <span data-ttu-id="39352-165">Kilenc oszlopok kilenc sorok</span><span class="sxs-lookup"><span data-stu-id="39352-165">Nine rows of nine columns</span></span>
* <span data-ttu-id="39352-166">Minden oszlop tartalmazhat vagy egy szám vagy `?` (amely azt jelzi, egy üres cella)</span><span class="sxs-lookup"><span data-stu-id="39352-166">Each column can contain either a number or `?` (which indicates a blank cell)</span></span>
* <span data-ttu-id="39352-167">Cellák egymástól elválasztva</span><span class="sxs-lookup"><span data-stu-id="39352-167">Cells are separated by a space</span></span>

<span data-ttu-id="39352-168">Van egy bizonyos módon tooconstruct Sudoku rejtvények; egy szám oszlop vagy sor nem ismétlődhet.</span><span class="sxs-lookup"><span data-stu-id="39352-168">There is a certain way tooconstruct Sudoku puzzles; you can't repeat a number in a column or row.</span></span> <span data-ttu-id="39352-169">Például nincs megfelelően összeállított hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="39352-169">There's an example on hello HDInsight cluster that is properly constructed.</span></span> <span data-ttu-id="39352-170">Az itt található: `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` és a következő szöveg hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="39352-170">It is located at `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` and contains hello following text:</span></span>

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

<span data-ttu-id="39352-171">toorun hello Sudoku a példában a példa probléma használja hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="39352-171">toorun this example problem through hello Sudoku example, use hello following command:</span></span>

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

<span data-ttu-id="39352-172">hello eredményei jelennek meg a következő szöveg hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="39352-172">hello results appear similar toohello following text:</span></span>

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi--example"></a><span data-ttu-id="39352-173">A pi (π) – Példa</span><span class="sxs-lookup"><span data-stu-id="39352-173">Pi (π) example</span></span>

<span data-ttu-id="39352-174">hello pi mintát használ a statisztikai (látszólagos Monte Carlo) metódus tooestimate hello a pi értékét.</span><span class="sxs-lookup"><span data-stu-id="39352-174">hello pi sample uses a statistical (quasi-Monte Carlo) method tooestimate hello value of pi.</span></span> <span data-ttu-id="39352-175">Pontok egység négyzet véletlenszerűen kerülnek.</span><span class="sxs-lookup"><span data-stu-id="39352-175">Points are placed at random in a unit square.</span></span> <span data-ttu-id="39352-176">hello szögletes kör is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="39352-176">hello square also contains a circle.</span></span> <span data-ttu-id="39352-177">hello valószínűsége, hogy hello pontok tartoznak hello kör hello egyenlő toohello területén kör, pi/4.</span><span class="sxs-lookup"><span data-stu-id="39352-177">hello probability that hello points fall within hello circle are equal toohello area of hello circle, pi/4.</span></span> <span data-ttu-id="39352-178">a pi értékét hello megbecsülhető 4R hello értékét.</span><span class="sxs-lookup"><span data-stu-id="39352-178">hello value of pi can be estimated from hello value of 4R.</span></span> <span data-ttu-id="39352-179">R hello aránya hello száma pontok belüli hello kör toohello pontok számát, amelyek hello szögletes belül.</span><span class="sxs-lookup"><span data-stu-id="39352-179">R is hello ratio of hello number of points that are inside hello circle toohello total number of points that are within hello square.</span></span> <span data-ttu-id="39352-180">hello hello kódtöredéket használt pontok, hello jobb hello becsült értéke.</span><span class="sxs-lookup"><span data-stu-id="39352-180">hello larger hello sample of points used, hello better hello estimate is.</span></span>

<span data-ttu-id="39352-181">A minta a következő parancs toorun hello használata.</span><span class="sxs-lookup"><span data-stu-id="39352-181">Use hello following command toorun this sample.</span></span> <span data-ttu-id="39352-182">Ez a parancs 16 maps 10,000,000 minták tooestimate hello a pi értékét használja:</span><span class="sxs-lookup"><span data-stu-id="39352-182">This command uses 16 maps with 10,000,000 samples each tooestimate hello value of pi:</span></span>

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

<span data-ttu-id="39352-183">hello Ez a parancs által visszaadott érték hasonló túl**3.14159155000000000000**.</span><span class="sxs-lookup"><span data-stu-id="39352-183">hello value returned by this command is similar too**3.14159155000000000000**.</span></span> <span data-ttu-id="39352-184">A hivatkozásokat hello pi első 10 tizedesjegyre 3.1415926535.</span><span class="sxs-lookup"><span data-stu-id="39352-184">For references, hello first 10 decimal places of pi are 3.1415926535.</span></span>

## <a name="10-gb-greysort-example"></a><span data-ttu-id="39352-185">10 GB-os Greysort – példa</span><span class="sxs-lookup"><span data-stu-id="39352-185">10 GB Greysort example</span></span>

<span data-ttu-id="39352-186">GraySort teljesítményteszt rendezés.</span><span class="sxs-lookup"><span data-stu-id="39352-186">GraySort is a benchmark sort.</span></span> <span data-ttu-id="39352-187">hello metrika az hello rendezési gyakoriság (TB percenként), amelyek során nagy mennyiségű adat, általában egy 100 TB-os minimális rendezés érhető el.</span><span class="sxs-lookup"><span data-stu-id="39352-187">hello metric is hello sort rate (TB/minute) that is achieved while sorting large amounts of data, usually a 100 TB minimum.</span></span>

<span data-ttu-id="39352-188">Ez a minta egy mérsékelt 10 GB adatot használ, így viszonylag gyorsan futtatása.</span><span class="sxs-lookup"><span data-stu-id="39352-188">This sample uses a modest 10 GB of data so that it can be run relatively quickly.</span></span> <span data-ttu-id="39352-189">Hello MapReduce alkalmazások Owen O'Malley és Arun Murthy által fejlesztett használ.</span><span class="sxs-lookup"><span data-stu-id="39352-189">It uses hello MapReduce applications developed by Owen O'Malley and Arun Murthy.</span></span> <span data-ttu-id="39352-190">Ezek az alkalmazások hello éves általános célú ("daytona") terabájt rendezési teljesítményteszt nyert 2009, sebességet 0.578 TB/perc (173 percben 100 TB).</span><span class="sxs-lookup"><span data-stu-id="39352-190">These applications won hello annual general-purpose ("daytona") terabyte sort benchmark in 2009, with a rate of 0.578 TB/min (100 TB in 173 minutes).</span></span> <span data-ttu-id="39352-191">Erről és más rendezési referenciaalapok további információkért lásd: hello [Sortbenchmark](http://sortbenchmark.org/) hely.</span><span class="sxs-lookup"><span data-stu-id="39352-191">For more information on this and other sorting benchmarks, see hello [Sortbenchmark](http://sortbenchmark.org/) site.</span></span>

<span data-ttu-id="39352-192">A példa a MapReduce programok három különböző használja:</span><span class="sxs-lookup"><span data-stu-id="39352-192">This sample uses three sets of MapReduce programs:</span></span>

* <span data-ttu-id="39352-193">**TeraGen**: A MapReduce program által generált adatok toosort sorát</span><span class="sxs-lookup"><span data-stu-id="39352-193">**TeraGen**: A MapReduce program that generates rows of data toosort</span></span>

* <span data-ttu-id="39352-194">**TeraSort**: hello bemeneti adatokat, és használja a MapReduce toosort hello adatok egy teljes rendelés</span><span class="sxs-lookup"><span data-stu-id="39352-194">**TeraSort**: Samples hello input data and uses MapReduce toosort hello data into a total order</span></span>

    <span data-ttu-id="39352-195">TeraSort szabványos MapReduce rendezési, kivéve egy egyéni particionáló.</span><span class="sxs-lookup"><span data-stu-id="39352-195">TeraSort is a standard MapReduce sort, except for a custom partitioner.</span></span> <span data-ttu-id="39352-196">hello particionáló hello kulcs tartomány minden csökkentse definiálása mintát N-1 kulcsok rendezett listáját használja.</span><span class="sxs-lookup"><span data-stu-id="39352-196">hello partitioner uses a sorted list of N-1 sampled keys that define hello key range for each reduce.</span></span> <span data-ttu-id="39352-197">Ebben az esetben, minden kulcsok ilyen mintát [i-1] < kulcs = < minta [i] küldött tooreduce i.</span><span class="sxs-lookup"><span data-stu-id="39352-197">In particular, all keys such that sample[i-1] <= key < sample[i] are sent tooreduce i.</span></span> <span data-ttu-id="39352-198">Ez a particionáló garantálja, hogy hello kimenetének csökkentése i segédanyagokra-nál kisebb hello kimenete csökkentése i + 1.</span><span class="sxs-lookup"><span data-stu-id="39352-198">This partitioner guarantees that hello outputs of reduce i are all less than hello output of reduce i+1.</span></span>

* <span data-ttu-id="39352-199">**TeraValidate**: A MapReduce program, amely ellenőrzi, hogy hello kimeneti globálisan rendezése</span><span class="sxs-lookup"><span data-stu-id="39352-199">**TeraValidate**: A MapReduce program that validates that hello output is globally sorted</span></span>

    <span data-ttu-id="39352-200">Fájlonként egy leképezést hello kimeneti könyvtár hozna létre, és minden leképezés biztosítja, hogy minden kulcs kisebb vagy egyenlő, mint toohello előzőre.</span><span class="sxs-lookup"><span data-stu-id="39352-200">It creates one map per file in hello output directory, and each map ensures that each key is less than or equal toohello previous one.</span></span> <span data-ttu-id="39352-201">hello leképezés funkció hello rekordjának először hoz létre, és a kulcsok az egyes fájlok utolsó.</span><span class="sxs-lookup"><span data-stu-id="39352-201">hello map function generates records of hello first and last keys of each file.</span></span> <span data-ttu-id="39352-202">hello csökkentése függvény biztosítja, hogy hello fájl i első kulcsát érték nagyobb, mint a fájl i-1 hello utolsó kulcsa.</span><span class="sxs-lookup"><span data-stu-id="39352-202">hello reduce function ensures that hello first key of file i is greater than hello last key of file i-1.</span></span> <span data-ttu-id="39352-203">Problémákat jelentett, a hello kimenete csökkentése fázisban, amelyek nem megfelelő sorrendben hello kulcsokkal.</span><span class="sxs-lookup"><span data-stu-id="39352-203">Any problems are reported as an output of hello reduce phase, with hello keys that are out of order.</span></span>

<span data-ttu-id="39352-204">Használjon hello alábbi lépéseit toogenerate adatok rendezéséhez, és ezután ellenőrizze a hello kimeneti:</span><span class="sxs-lookup"><span data-stu-id="39352-204">Use hello following steps toogenerate data, sort, and then validate hello output:</span></span>

1. <span data-ttu-id="39352-205">10 GB adatot, amely tárolt toohello HDInsight fürt alapértelmezett tárhely létrehozása a `/example/data/10GB-sort-input`:</span><span class="sxs-lookup"><span data-stu-id="39352-205">Generate 10 GB of data, which is stored toohello HDInsight cluster's default storage at `/example/data/10GB-sort-input`:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    <span data-ttu-id="39352-206">Hello `-Dmapred.map.tasks` közli Hadoop hány térkép feladatok toouse ehhez a feladathoz.</span><span class="sxs-lookup"><span data-stu-id="39352-206">hello `-Dmapred.map.tasks` tells Hadoop how many map tasks toouse for this job.</span></span> <span data-ttu-id="39352-207">utolsó két hello paraméterek utasítani hello feladat toocreate 10 GB adat és toostore a `/example/data/10GB-sort-input`.</span><span class="sxs-lookup"><span data-stu-id="39352-207">hello final two parameters instruct hello job toocreate 10 GB of data and toostore it at `/example/data/10GB-sort-input`.</span></span>

2. <span data-ttu-id="39352-208">A következő parancs toosort hello adatok hello használata:</span><span class="sxs-lookup"><span data-stu-id="39352-208">Use hello following command toosort hello data:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    <span data-ttu-id="39352-209">Hello `-Dmapred.reduce.tasks` közli a Hadoop számának csökkentése feladatok toouse hello feladat.</span><span class="sxs-lookup"><span data-stu-id="39352-209">hello `-Dmapred.reduce.tasks` tells Hadoop how many reduce tasks toouse for hello job.</span></span> <span data-ttu-id="39352-210">utolsó két hello paraméterek csak hello bemeneti és kimeneti adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="39352-210">hello final two parameters are just hello input and output locations for data.</span></span>

3. <span data-ttu-id="39352-211">A következő hello rendezési által létrehozott toovalidate hello adatok hello használata:</span><span class="sxs-lookup"><span data-stu-id="39352-211">Use hello following toovalidate hello data generated by hello sort:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a><span data-ttu-id="39352-212">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="39352-212">Next steps</span></span>

<span data-ttu-id="39352-213">Az ebben a cikkben megtanulta, hogyan toorun hello minták mellékelt hello Linux-alapú HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="39352-213">From this article, you learned how toorun hello samples included with hello Linux-based HDInsight clusters.</span></span> <span data-ttu-id="39352-214">A Pig, a Hive és a MapReduce és a HDInsight együttes használatával kapcsolatos oktatóanyagok tekintse meg a következő témakörök hello:</span><span class="sxs-lookup"><span data-stu-id="39352-214">For tutorials about using Pig, Hive, and MapReduce with HDInsight, see hello following topics:</span></span>

* <span data-ttu-id="39352-215">[A Pig használata a HDInsight Hadoop][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="39352-215">[Use Pig with Hadoop on HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="39352-216">[A Hive használata a hdinsight Hadoop][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="39352-216">[Use Hive with Hadoop on HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="39352-217">[A HDInsight Hadoop MapReduce használata][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="39352-217">[Use MapReduce with Hadoop on HDInsight][hdinsight-use-mapreduce]</span></span>

[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
