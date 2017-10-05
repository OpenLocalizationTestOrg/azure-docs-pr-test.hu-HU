---
title: "Futtassa a Hadoop-MapReduce példák a HDInsight - Azure-on |} Microsoft Docs"
description: "MapReduce-minták a HDInsight szereplő jar-fájlok az első lépéseiben. Csatlakozzon a fürthöz az SSH segítségével, és a Hadoop paranccsal minta feladatok futtatásához."
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
ms.openlocfilehash: 447c07f869ff9a2a2a00089248be98e6729d6dc4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="run-the-mapreduce-examples-included-in-hdinsight"></a><span data-ttu-id="c70f7-105">Futtassa a HDInsight szereplő MapReduce példák</span><span class="sxs-lookup"><span data-stu-id="c70f7-105">Run the MapReduce examples included in HDInsight</span></span>

[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

<span data-ttu-id="c70f7-106">Útmutató a mellékelt a HDInsight Hadoop MapReduce példák futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c70f7-106">Learn how to run the MapReduce examples included with Hadoop on HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c70f7-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c70f7-107">Prerequisites</span></span>

* <span data-ttu-id="c70f7-108">**HDInsight-fürtök**: lásd: [Hadoop használatának megkezdésében a Hive HDInsight Linux rendszeren](hdinsight-hadoop-linux-tutorial-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="c70f7-108">**An HDInsight cluster**: See [Get started using Hadoop with Hive in HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c70f7-109">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="c70f7-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="c70f7-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="c70f7-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="c70f7-111">**Egy SSH-ügyfél**: további információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="c70f7-111">**An SSH client**: For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="the-mapreduce-examples"></a><span data-ttu-id="c70f7-112">A MapReduce példák</span><span class="sxs-lookup"><span data-stu-id="c70f7-112">The MapReduce examples</span></span>

<span data-ttu-id="c70f7-113">**Hely**: A minták a HDInsight-fürthöz, a lévő `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.</span><span class="sxs-lookup"><span data-stu-id="c70f7-113">**Location**: The samples are located on the HDInsight cluster at `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.</span></span>

<span data-ttu-id="c70f7-114">**Tartalom**: A következő minták ebből az archívumból tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="c70f7-114">**Contents**: The following samples are contained in this archive:</span></span>

* <span data-ttu-id="c70f7-115">`aggregatewordcount`: Egy összesítés mapreduce program, amely a bemeneti fájlok szavakat száma alapján.</span><span class="sxs-lookup"><span data-stu-id="c70f7-115">`aggregatewordcount`: An Aggregate based mapreduce program that counts the words in the input files.</span></span>
* <span data-ttu-id="c70f7-116">`aggregatewordhist`: Egy összesítés mapreduce program, amely kiszámítja a szövegrész szerepel a bemeneti fájlok hisztogram alapján.</span><span class="sxs-lookup"><span data-stu-id="c70f7-116">`aggregatewordhist`: An Aggregate based mapreduce program that computes the histogram of the words in the input files.</span></span>
* <span data-ttu-id="c70f7-117">`bbp`: Mapreduce használó program Bailey-Borwein-Plouffe pi számjegyeket kiszámításához.</span><span class="sxs-lookup"><span data-stu-id="c70f7-117">`bbp`: A mapreduce program that uses Bailey-Borwein-Plouffe to compute exact digits of Pi.</span></span>
* <span data-ttu-id="c70f7-118">`dbcount`: Egy példa a feladat, amely megjeleníti az adatbázisban tárolt pageview naplókat.</span><span class="sxs-lookup"><span data-stu-id="c70f7-118">`dbcount`: An example job that counts the pageview logs stored in a database.</span></span>
* <span data-ttu-id="c70f7-119">`distbbp`: Mapreduce program pi pontos bits számítási BBP típusú képlet használatával.</span><span class="sxs-lookup"><span data-stu-id="c70f7-119">`distbbp`: A mapreduce program that uses a BBP-type formula to compute exact bits of Pi.</span></span>
* <span data-ttu-id="c70f7-120">`grep`: A mapreduce program, amely egy reguláris kifejezéssel, a bemeneti adatok egyezések száma.</span><span class="sxs-lookup"><span data-stu-id="c70f7-120">`grep`: A mapreduce program that counts the matches of a regex in the input.</span></span>
* <span data-ttu-id="c70f7-121">`join`: Egy feladatot, amely végrehajtja a való csatlakozást keresztül rendezve, adatkészletek egyaránt particionálva.</span><span class="sxs-lookup"><span data-stu-id="c70f7-121">`join`: A job that performs a join over sorted, equally partitioned datasets.</span></span>
* <span data-ttu-id="c70f7-122">`multifilewc`: Egy feladatot, amely a szavakat több fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="c70f7-122">`multifilewc`: A job that counts words from several files.</span></span>
* <span data-ttu-id="c70f7-123">`pentomino`: A mapreduce csempe szóló program pentomino problémák megoldása.</span><span class="sxs-lookup"><span data-stu-id="c70f7-123">`pentomino`: A mapreduce tile laying program to find solutions to pentomino problems.</span></span>
* <span data-ttu-id="c70f7-124">`pi`: A mapreduce program, amely segítségével látszólagos Monte Pi becslése Carlo metódust.</span><span class="sxs-lookup"><span data-stu-id="c70f7-124">`pi`: A mapreduce program that estimates Pi using a quasi-Monte Carlo method.</span></span>
* <span data-ttu-id="c70f7-125">`randomtextwriter`: A mapreduce program 10 GB-nyi véletlenszerű szöveges adatok csomópontonként írja.</span><span class="sxs-lookup"><span data-stu-id="c70f7-125">`randomtextwriter`: A mapreduce program that writes 10 GB of random textual data per node.</span></span>
* <span data-ttu-id="c70f7-126">`randomwriter`: A mapreduce program 10 GB-nyi véletlenszerű adatokat csomópontonként írja.</span><span class="sxs-lookup"><span data-stu-id="c70f7-126">`randomwriter`: A mapreduce program that writes 10 GB of random data per node.</span></span>
* <span data-ttu-id="c70f7-127">`secondarysort`: Példa egy másodlagos rendezési meghatározása a reduce szakaszhoz ugranak.</span><span class="sxs-lookup"><span data-stu-id="c70f7-127">`secondarysort`: An example defining a secondary sort to the reduce phase.</span></span>
* <span data-ttu-id="c70f7-128">`sort`: Mapreduce program a véletlenszerű író írt adatok rendezi.</span><span class="sxs-lookup"><span data-stu-id="c70f7-128">`sort`: A mapreduce program that sorts the data written by the random writer.</span></span>
* <span data-ttu-id="c70f7-129">`sudoku`: A sudoku solver.</span><span class="sxs-lookup"><span data-stu-id="c70f7-129">`sudoku`: A sudoku solver.</span></span>
* <span data-ttu-id="c70f7-130">`teragen`: A terasort adatainak létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c70f7-130">`teragen`: Generate data for the terasort.</span></span>
* <span data-ttu-id="c70f7-131">`terasort`: A terasort futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c70f7-131">`terasort`: Run the terasort.</span></span>
* <span data-ttu-id="c70f7-132">`teravalidate`: Terasort eredményeinek ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="c70f7-132">`teravalidate`: Checking results of terasort.</span></span>
* <span data-ttu-id="c70f7-133">`wordcount`: A mapreduce program, amely a szavakat a bemeneti fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="c70f7-133">`wordcount`: A mapreduce program that counts the words in the input files.</span></span>
* <span data-ttu-id="c70f7-134">`wordmean`: A mapreduce program, amely a bemeneti fájlok szavakat átlagos hosszának száma.</span><span class="sxs-lookup"><span data-stu-id="c70f7-134">`wordmean`: A mapreduce program that counts the average length of the words in the input files.</span></span>
* <span data-ttu-id="c70f7-135">`wordmedian`: Mapreduce program a közepes hossza a szövegrész szerepel a bemeneti fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="c70f7-135">`wordmedian`: A mapreduce program that counts the median length of the words in the input files.</span></span>
* <span data-ttu-id="c70f7-136">`wordstandarddeviation`: A mapreduce program, amely a bemeneti fájlok szavakat hosszát szórását száma.</span><span class="sxs-lookup"><span data-stu-id="c70f7-136">`wordstandarddeviation`: A mapreduce program that counts the standard deviation of the length of the words in the input files.</span></span>

<span data-ttu-id="c70f7-137">**Forráskód**: ezeket a mintákat forráskódja szerepel-e a HDInsight-fürt `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.</span><span class="sxs-lookup"><span data-stu-id="c70f7-137">**Source code**: Source code for these samples is included on the HDInsight cluster at `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.</span></span>

> [!NOTE]
> <span data-ttu-id="c70f7-138">A `2.2.4.9-1` az elérési út a Hortonworks Data Platform a HDInsight-fürthöz verziója van telepítve, és a fürt eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="c70f7-138">The `2.2.4.9-1` in the path is the version of the Hortonworks Data Platform for the HDInsight cluster, and may be different for your cluster.</span></span>

## <a name="run-the-wordcount-example"></a><span data-ttu-id="c70f7-139">Futtassa a wordcount-példa</span><span class="sxs-lookup"><span data-stu-id="c70f7-139">Run the wordcount example</span></span>

1. <span data-ttu-id="c70f7-140">Csatlakozzon a HDInsight SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="c70f7-140">Connect to HDInsight using SSH.</span></span> <span data-ttu-id="c70f7-141">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="c70f7-141">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="c70f7-142">Az a `username@#######:~$` kérni, használja a következő parancsot a minták listázásához:</span><span class="sxs-lookup"><span data-stu-id="c70f7-142">From the `username@#######:~$` prompt, use the following command to list the samples:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    <span data-ttu-id="c70f7-143">Ez a parancs létrehozza az összetevőlistát a minta a jelen dokumentum korábbi szakaszában.</span><span class="sxs-lookup"><span data-stu-id="c70f7-143">This command generates the list of sample from the previous section of this document.</span></span>

3. <span data-ttu-id="c70f7-144">A következő paranccsal egy adott minta segítség.</span><span class="sxs-lookup"><span data-stu-id="c70f7-144">Use the following command to get help on a specific sample.</span></span> <span data-ttu-id="c70f7-145">Ebben az esetben a **wordcount** minta:</span><span class="sxs-lookup"><span data-stu-id="c70f7-145">In this case, the **wordcount** sample:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    <span data-ttu-id="c70f7-146">A következő hibaüzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="c70f7-146">You receive the following message:</span></span>

        Usage: wordcount <in> [<in>...] <out>

    <span data-ttu-id="c70f7-147">Ez az üzenet azt jelzi, hogy több bemeneti elérési utak biztosíthat a forrás-dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="c70f7-147">This message indicates that you can provide several input paths for the source documents.</span></span> <span data-ttu-id="c70f7-148">A végső elérési út, a kimenet (a forrás-dokumentumokban szavak száma) tárolására.</span><span class="sxs-lookup"><span data-stu-id="c70f7-148">The final path is where the output (count of words in the source documents) is stored.</span></span>

4. <span data-ttu-id="c70f7-149">A megszámlálandó a notebookok a Leonardo Da Vinci, amely vannak megadva, a mintaadatok a fürthöz az összes szó használja a következőket:</span><span class="sxs-lookup"><span data-stu-id="c70f7-149">Use the following to count all words in the Notebooks of Leonardo Da Vinci, which are provided as sample data with your cluster:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    <span data-ttu-id="c70f7-150">Ez a feladat olvasása az adatokat `/example/data/gutenberg/davinci.txt`.</span><span class="sxs-lookup"><span data-stu-id="c70f7-150">Input for this job is read from `/example/data/gutenberg/davinci.txt`.</span></span> <span data-ttu-id="c70f7-151">Az ebben a példában tárolódik kimeneti `/example/data/davinciwordcount`.</span><span class="sxs-lookup"><span data-stu-id="c70f7-151">Output for this example is stored in `/example/data/davinciwordcount`.</span></span> <span data-ttu-id="c70f7-152">Mindkét elérési utak a fürt, nem a helyi fájlrendszer alapértelmezett tároló található.</span><span class="sxs-lookup"><span data-stu-id="c70f7-152">Both paths are located on default storage for the cluster, not the local file system.</span></span>

   > [!NOTE]
   > <span data-ttu-id="c70f7-153">Amint a Súgó gombra a wordcount-példa, több bemeneti fájl is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="c70f7-153">As noted in the help for the wordcount sample, you could also specify multiple input files.</span></span> <span data-ttu-id="c70f7-154">Például `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` számolja davinci.txt és ulysses.txt szavakat.</span><span class="sxs-lookup"><span data-stu-id="c70f7-154">For example, `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` would count words in both davinci.txt and ulysses.txt.</span></span>

5. <span data-ttu-id="c70f7-155">Ha a feladat befejeződik, a következő paranccsal eredményének megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="c70f7-155">Once the job completes, use the following command to view the output:</span></span>

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    <span data-ttu-id="c70f7-156">Ez a parancs a feladat által létrehozott kimeneti fájlok fűzi össze.</span><span class="sxs-lookup"><span data-stu-id="c70f7-156">This command concatenates all the output files produced by the job.</span></span> <span data-ttu-id="c70f7-157">A kimenet megjeleníti a konzolhoz.</span><span class="sxs-lookup"><span data-stu-id="c70f7-157">It displays the output to the console.</span></span> <span data-ttu-id="c70f7-158">A kimenet az alábbi szöveghez hasonló:</span><span class="sxs-lookup"><span data-stu-id="c70f7-158">The output is similar to the following text:</span></span>

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    <span data-ttu-id="c70f7-159">Soronként egy szót, és hány alkalommal azt történt a bemeneti adatok jelöli.</span><span class="sxs-lookup"><span data-stu-id="c70f7-159">Each line represents a word and how many times it occurred in the input data.</span></span>

## <a name="the-sudoku-example"></a><span data-ttu-id="c70f7-160">A Sudoku – példa</span><span class="sxs-lookup"><span data-stu-id="c70f7-160">The Sudoku example</span></span>

<span data-ttu-id="c70f7-161">[Sudoku](https://en.wikipedia.org/wiki/Sudoku) egy logikai kirakós kilenc 3 x 3 rácsok áll.</span><span class="sxs-lookup"><span data-stu-id="c70f7-161">[Sudoku](https://en.wikipedia.org/wiki/Sudoku) is a logic puzzle made up of nine 3x3 grids.</span></span> <span data-ttu-id="c70f7-162">Néhány a rács celláinak számok, üres, és a cél az, hogy az üres cellák megoldásában.</span><span class="sxs-lookup"><span data-stu-id="c70f7-162">Some cells in the grid have numbers, while others are blank, and the goal is to solve for the blank cells.</span></span> <span data-ttu-id="c70f7-163">A korábbi hivatkozás a további információk a Kirakós rendelkezik, de ez a minta az a célja, hogy oldja meg az üres cellák.</span><span class="sxs-lookup"><span data-stu-id="c70f7-163">The previous link has more information on the puzzle, but the purpose of this sample is to solve for the blank cells.</span></span> <span data-ttu-id="c70f7-164">A bemenet, a következő formátumú fájlnak kell lennie:</span><span class="sxs-lookup"><span data-stu-id="c70f7-164">So our input should be a file that is in the following format:</span></span>

* <span data-ttu-id="c70f7-165">Kilenc oszlopok kilenc sorok</span><span class="sxs-lookup"><span data-stu-id="c70f7-165">Nine rows of nine columns</span></span>
* <span data-ttu-id="c70f7-166">Minden oszlop tartalmazhat vagy egy szám vagy `?` (amely azt jelzi, egy üres cella)</span><span class="sxs-lookup"><span data-stu-id="c70f7-166">Each column can contain either a number or `?` (which indicates a blank cell)</span></span>
* <span data-ttu-id="c70f7-167">Cellák egymástól elválasztva</span><span class="sxs-lookup"><span data-stu-id="c70f7-167">Cells are separated by a space</span></span>

<span data-ttu-id="c70f7-168">Egy bizonyos módon összeállítani Sudoku rejtvények; egy szám oszlop vagy sor nem ismétlődhet.</span><span class="sxs-lookup"><span data-stu-id="c70f7-168">There is a certain way to construct Sudoku puzzles; you can't repeat a number in a column or row.</span></span> <span data-ttu-id="c70f7-169">A HDInsight-fürt megfelelően összeállított például nincs.</span><span class="sxs-lookup"><span data-stu-id="c70f7-169">There's an example on the HDInsight cluster that is properly constructed.</span></span> <span data-ttu-id="c70f7-170">Az itt található: `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` és tartalmazza a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="c70f7-170">It is located at `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` and contains the following text:</span></span>

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

<span data-ttu-id="c70f7-171">Példa a probléma keresztül a Sudoku példa futtatásához a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="c70f7-171">To run this example problem through the Sudoku example, use the following command:</span></span>

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

<span data-ttu-id="c70f7-172">Az eredmény az alábbihoz hasonló jelennek meg:</span><span class="sxs-lookup"><span data-stu-id="c70f7-172">The results appear similar to the following text:</span></span>

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi--example"></a><span data-ttu-id="c70f7-173">A pi (π) – Példa</span><span class="sxs-lookup"><span data-stu-id="c70f7-173">Pi (π) example</span></span>

<span data-ttu-id="c70f7-174">A pi mintát használ a statisztikai (látszólagos Monte Carlo) módszer a pi értékét.</span><span class="sxs-lookup"><span data-stu-id="c70f7-174">The pi sample uses a statistical (quasi-Monte Carlo) method to estimate the value of pi.</span></span> <span data-ttu-id="c70f7-175">Pontok egység négyzet véletlenszerűen kerülnek.</span><span class="sxs-lookup"><span data-stu-id="c70f7-175">Points are placed at random in a unit square.</span></span> <span data-ttu-id="c70f7-176">A négyzet kör is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c70f7-176">The square also contains a circle.</span></span> <span data-ttu-id="c70f7-177">Annak a valószínűsége, hogy a pontok esik-e a kör megegyeznek a terület a kör pi/4.</span><span class="sxs-lookup"><span data-stu-id="c70f7-177">The probability that the points fall within the circle are equal to the area of the circle, pi/4.</span></span> <span data-ttu-id="c70f7-178">A pi értékét megbecsülhető 4R értékét.</span><span class="sxs-lookup"><span data-stu-id="c70f7-178">The value of pi can be estimated from the value of 4R.</span></span> <span data-ttu-id="c70f7-179">R a pontok, amelyek a teljes számú négyzetét belüli pontra körben aránya.</span><span class="sxs-lookup"><span data-stu-id="c70f7-179">R is the ratio of the number of points that are inside the circle to the total number of points that are within the square.</span></span> <span data-ttu-id="c70f7-180">Minél nagyobb a mintában használt pontok, annál pontosabb becslést van.</span><span class="sxs-lookup"><span data-stu-id="c70f7-180">The larger the sample of points used, the better the estimate is.</span></span>

<span data-ttu-id="c70f7-181">A minta futtatásához használja a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="c70f7-181">Use the following command to run this sample.</span></span> <span data-ttu-id="c70f7-182">Ez a parancs használ 16 maps 10,000,000 minták becsléséhez a pi értékét:</span><span class="sxs-lookup"><span data-stu-id="c70f7-182">This command uses 16 maps with 10,000,000 samples each to estimate the value of pi:</span></span>

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

<span data-ttu-id="c70f7-183">Ez a parancs által visszaadott érték hasonlít **3.14159155000000000000**.</span><span class="sxs-lookup"><span data-stu-id="c70f7-183">The value returned by this command is similar to **3.14159155000000000000**.</span></span> <span data-ttu-id="c70f7-184">Hivatkozások az első 10 tizedesjegyek pi 3.1415926535 vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="c70f7-184">For references, the first 10 decimal places of pi are 3.1415926535.</span></span>

## <a name="10-gb-greysort-example"></a><span data-ttu-id="c70f7-185">10 GB-os Greysort – példa</span><span class="sxs-lookup"><span data-stu-id="c70f7-185">10 GB Greysort example</span></span>

<span data-ttu-id="c70f7-186">GraySort teljesítményteszt rendezés.</span><span class="sxs-lookup"><span data-stu-id="c70f7-186">GraySort is a benchmark sort.</span></span> <span data-ttu-id="c70f7-187">A metrika az a rendezési gyakoriság (TB percenként), amelyek során nagy mennyiségű adat, általában egy 100 TB-os minimális rendezés érhető el.</span><span class="sxs-lookup"><span data-stu-id="c70f7-187">The metric is the sort rate (TB/minute) that is achieved while sorting large amounts of data, usually a 100 TB minimum.</span></span>

<span data-ttu-id="c70f7-188">Ez a minta egy mérsékelt 10 GB adatot használ, így viszonylag gyorsan futtatása.</span><span class="sxs-lookup"><span data-stu-id="c70f7-188">This sample uses a modest 10 GB of data so that it can be run relatively quickly.</span></span> <span data-ttu-id="c70f7-189">A MapReduce alkalmazások Owen O'Malley és Arun Murthy által fejlesztett használ.</span><span class="sxs-lookup"><span data-stu-id="c70f7-189">It uses the MapReduce applications developed by Owen O'Malley and Arun Murthy.</span></span> <span data-ttu-id="c70f7-190">Ezeket az alkalmazásokat az éves általános célú ("daytona") terabájt rendezési teljesítményteszt nyert 2009, sebességet 0.578 TB/perc (173 percben 100 TB).</span><span class="sxs-lookup"><span data-stu-id="c70f7-190">These applications won the annual general-purpose ("daytona") terabyte sort benchmark in 2009, with a rate of 0.578 TB/min (100 TB in 173 minutes).</span></span> <span data-ttu-id="c70f7-191">Erről és más rendezési referenciaalapok további információkért lásd: a [Sortbenchmark](http://sortbenchmark.org/) hely.</span><span class="sxs-lookup"><span data-stu-id="c70f7-191">For more information on this and other sorting benchmarks, see the [Sortbenchmark](http://sortbenchmark.org/) site.</span></span>

<span data-ttu-id="c70f7-192">A példa a MapReduce programok három különböző használja:</span><span class="sxs-lookup"><span data-stu-id="c70f7-192">This sample uses three sets of MapReduce programs:</span></span>

* <span data-ttu-id="c70f7-193">**TeraGen**: A MapReduce program, amely hoz létre sorok az adatok rendezése</span><span class="sxs-lookup"><span data-stu-id="c70f7-193">**TeraGen**: A MapReduce program that generates rows of data to sort</span></span>

* <span data-ttu-id="c70f7-194">**TeraSort**: a bemeneti adatok minták és MapReduce segítségével rendezze az adatokat egy teljes rendelés</span><span class="sxs-lookup"><span data-stu-id="c70f7-194">**TeraSort**: Samples the input data and uses MapReduce to sort the data into a total order</span></span>

    <span data-ttu-id="c70f7-195">TeraSort szabványos MapReduce rendezési, kivéve egy egyéni particionáló.</span><span class="sxs-lookup"><span data-stu-id="c70f7-195">TeraSort is a standard MapReduce sort, except for a custom partitioner.</span></span> <span data-ttu-id="c70f7-196">A particionáló mintát N-1 kulcsok minden egyes csökkentse kulcs tartományának megadása rendezett listáját használja.</span><span class="sxs-lookup"><span data-stu-id="c70f7-196">The partitioner uses a sorted list of N-1 sampled keys that define the key range for each reduce.</span></span> <span data-ttu-id="c70f7-197">Ebben az esetben, minden kulcsok ilyen mintát [i-1] < kulcs = < minta [i] kerülnek i csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="c70f7-197">In particular, all keys such that sample[i-1] <= key < sample[i] are sent to reduce i.</span></span> <span data-ttu-id="c70f7-198">Ez a particionáló garantálja, hogy a kimenetének csökkentése i segédanyagokra-nál kisebb kimenete csökkentése i + 1.</span><span class="sxs-lookup"><span data-stu-id="c70f7-198">This partitioner guarantees that the outputs of reduce i are all less than the output of reduce i+1.</span></span>

* <span data-ttu-id="c70f7-199">**TeraValidate**: A MapReduce program, amely ellenőrzi, hogy a kimeneti globálisan rendezett</span><span class="sxs-lookup"><span data-stu-id="c70f7-199">**TeraValidate**: A MapReduce program that validates that the output is globally sorted</span></span>

    <span data-ttu-id="c70f7-200">Fájlonként egy leképezést a kimeneti könyvtár hozna létre, és minden leképezés biztosítja, hogy minden kulcs kisebb vagy egyenlő, mint az előzőt.</span><span class="sxs-lookup"><span data-stu-id="c70f7-200">It creates one map per file in the output directory, and each map ensures that each key is less than or equal to the previous one.</span></span> <span data-ttu-id="c70f7-201">A térkép függvény állít elő, az első és utolsó kulcsok minden egyes fájl rögzíti.</span><span class="sxs-lookup"><span data-stu-id="c70f7-201">The map function generates records of the first and last keys of each file.</span></span> <span data-ttu-id="c70f7-202">Csökkentse a függvény biztosítja, hogy az első kulcsát fájl i nagyobb, mint az utolsó fájl i-1 kulcsa.</span><span class="sxs-lookup"><span data-stu-id="c70f7-202">The reduce function ensures that the first key of file i is greater than the last key of file i-1.</span></span> <span data-ttu-id="c70f7-203">Problémák a kulcsokkal, amelyek nem megfelelő sorrendben csökkentse fázis kimenetként jelenti.</span><span class="sxs-lookup"><span data-stu-id="c70f7-203">Any problems are reported as an output of the reduce phase, with the keys that are out of order.</span></span>

<span data-ttu-id="c70f7-204">Az alábbi lépések végrehajtásával hozhat létre adatokat, rendezés, és a kimeneti ellenőrzéséhez:</span><span class="sxs-lookup"><span data-stu-id="c70f7-204">Use the following steps to generate data, sort, and then validate the output:</span></span>

1. <span data-ttu-id="c70f7-205">10 GB-os adatait, amely a HDInsight-fürt alapértelmezett tárhelyre, létre `/example/data/10GB-sort-input`:</span><span class="sxs-lookup"><span data-stu-id="c70f7-205">Generate 10 GB of data, which is stored to the HDInsight cluster's default storage at `/example/data/10GB-sort-input`:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    <span data-ttu-id="c70f7-206">A `-Dmapred.map.tasks` Hadoop közli az feladathoz használandó térkép feladatok.</span><span class="sxs-lookup"><span data-stu-id="c70f7-206">The `-Dmapred.map.tasks` tells Hadoop how many map tasks to use for this job.</span></span> <span data-ttu-id="c70f7-207">Az utolsó két paramétert kérje meg a feladat létrehozása a 10 GB adatot és tárolja a `/example/data/10GB-sort-input`.</span><span class="sxs-lookup"><span data-stu-id="c70f7-207">The final two parameters instruct the job to create 10 GB of data and to store it at `/example/data/10GB-sort-input`.</span></span>

2. <span data-ttu-id="c70f7-208">Az alábbi parancs segítségével rendezze az adatokat:</span><span class="sxs-lookup"><span data-stu-id="c70f7-208">Use the following command to sort the data:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    <span data-ttu-id="c70f7-209">A `-Dmapred.reduce.tasks` közli a Hadoop számának csökkentése a feladathoz használandó feladatok.</span><span class="sxs-lookup"><span data-stu-id="c70f7-209">The `-Dmapred.reduce.tasks` tells Hadoop how many reduce tasks to use for the job.</span></span> <span data-ttu-id="c70f7-210">Az utolsó két paraméterei csak a bemeneti és kimeneti helyek az adatok.</span><span class="sxs-lookup"><span data-stu-id="c70f7-210">The final two parameters are just the input and output locations for data.</span></span>

3. <span data-ttu-id="c70f7-211">A rendezés által létrehozott adatok érvényesítéséhez használja a következőket:</span><span class="sxs-lookup"><span data-stu-id="c70f7-211">Use the following to validate the data generated by the sort:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a><span data-ttu-id="c70f7-212">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c70f7-212">Next steps</span></span>

<span data-ttu-id="c70f7-213">Az ebben a cikkben megtanulta, a Linux-alapú HDInsight-fürtökkel minták futtatása.</span><span class="sxs-lookup"><span data-stu-id="c70f7-213">From this article, you learned how to run the samples included with the Linux-based HDInsight clusters.</span></span> <span data-ttu-id="c70f7-214">Oktatóanyagok Pig, a Hive és a MapReduce használata a hdinsight eszközzel a következő témakörökben található:</span><span class="sxs-lookup"><span data-stu-id="c70f7-214">For tutorials about using Pig, Hive, and MapReduce with HDInsight, see the following topics:</span></span>

* <span data-ttu-id="c70f7-215">[A Pig használata a HDInsight Hadoop][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="c70f7-215">[Use Pig with Hadoop on HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="c70f7-216">[A Hive használata a hdinsight Hadoop][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="c70f7-216">[Use Hive with Hadoop on HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="c70f7-217">[A HDInsight Hadoop MapReduce használata][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="c70f7-217">[Use MapReduce with Hadoop on HDInsight][hdinsight-use-mapreduce]</span></span>

[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
