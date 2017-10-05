---
title: "Adatfolyam-MapReduce-feladatok a HDInsight - Azure Python kidolgozása |} Microsoft Docs"
description: "Ismerje meg, hogyan használható a Python az adatfolyam-MapReduce-feladatok. Hadoop streamelési API biztosít a Java nyelven írt a MapReduce."
services: hdinsight
keyword: mapreduce python,python map reduce,python mapreduce
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7631d8d9-98ae-42ec-b9ec-ee3cf7e57fb3
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: b86605c49291a99f49c4b2841d46324cfd0db56d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="develop-python-streaming-mapreduce-programs-for-hdinsight"></a><span data-ttu-id="d9442-104">A HDInsight MapReduce programok streaming Python fejlesztése</span><span class="sxs-lookup"><span data-stu-id="d9442-104">Develop Python streaming MapReduce programs for HDInsight</span></span>

<span data-ttu-id="d9442-105">Megtudhatja, hogyan használja a Python MapReduce műveletek adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="d9442-105">Learn how to use Python in streaming MapReduce operations.</span></span> <span data-ttu-id="d9442-106">Hadoop streamelési API biztosít, amely lehetővé teszi, hogy a térkép írása, és csökkentheti a funkciók nem Java nyelven MapReduce.</span><span class="sxs-lookup"><span data-stu-id="d9442-106">Hadoop provides a streaming API for MapReduce that enables you to write map and reduce functions in languages other than Java.</span></span> <span data-ttu-id="d9442-107">A jelen dokumentumban leírt lépések végrehajtása a térkép, és csökkentse az összetevők a Python.</span><span class="sxs-lookup"><span data-stu-id="d9442-107">The steps in this document implement the Map and Reduce components in Python.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9442-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d9442-108">Prerequisites</span></span>

* <span data-ttu-id="d9442-109">A Linux-based Hadoop on HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="d9442-109">A Linux-based Hadoop on HDInsight cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="d9442-110">A jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="d9442-110">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="d9442-111">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="d9442-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d9442-112">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d9442-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="d9442-113">Egy szövegszerkesztőben</span><span class="sxs-lookup"><span data-stu-id="d9442-113">A text editor</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="d9442-114">A szövegszerkesztőben kell LF használják a sor befejezése.</span><span class="sxs-lookup"><span data-stu-id="d9442-114">The text editor must use LF as the line ending.</span></span> <span data-ttu-id="d9442-115">Egy sor vége CRLF használatával hatására a hibák, a MapReduce feladatot Linux-alapú HDInsight-fürtök futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="d9442-115">Using a line ending of CRLF causes errors when running the MapReduce job on Linux-based HDInsight clusters.</span></span>

* <span data-ttu-id="d9442-116">A `ssh` és `scp` parancsok, vagy [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)</span><span class="sxs-lookup"><span data-stu-id="d9442-116">The `ssh` and `scp` commands, or [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)</span></span>

## <a name="word-count"></a><span data-ttu-id="d9442-117">Word száma</span><span class="sxs-lookup"><span data-stu-id="d9442-117">Word count</span></span>

<span data-ttu-id="d9442-118">Ebben a példában az alapvető szószámot python megvalósítva, a hozzárendelést és nyomáscsökkentő.</span><span class="sxs-lookup"><span data-stu-id="d9442-118">This example is a basic word count implemented in a python a mapper and reducer.</span></span> <span data-ttu-id="d9442-119">A leképező mondat bontja egyedi szót, és a nyomáscsökkentő összesíti a szavakat, és megjeleníti a kimenet előállításához.</span><span class="sxs-lookup"><span data-stu-id="d9442-119">The mapper breaks sentences into individual words, and the reducer aggregates the words and counts to produce the output.</span></span>

<span data-ttu-id="d9442-120">Az alábbi folyamatábra bemutatja, mi történik a leképezés során, és csökkentheti a fázisok.</span><span class="sxs-lookup"><span data-stu-id="d9442-120">The following flowchart illustrates what happens during the map and reduce phases.</span></span>

![a mapreduce folyamat ábrája](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="streaming-mapreduce"></a><span data-ttu-id="d9442-122">Adatfolyam-továbbítási MapReduce</span><span class="sxs-lookup"><span data-stu-id="d9442-122">Streaming MapReduce</span></span>

<span data-ttu-id="d9442-123">Hadoop lehetővé teszi a térkép tartalmazó fájlt ad meg, és csökkenti a feladat által használt logikai.</span><span class="sxs-lookup"><span data-stu-id="d9442-123">Hadoop allows you to specify a file that contains the map and reduce logic that is used by a job.</span></span> <span data-ttu-id="d9442-124">A térkép a konkrét követelmények, ami csökkenti a logika vannak:</span><span class="sxs-lookup"><span data-stu-id="d9442-124">The specific requirements for the map and reduce logic are:</span></span>

* <span data-ttu-id="d9442-125">**Bemeneti**: A térkép és összetevők bemeneti adatokat STDIN kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="d9442-125">**Input**: The map and reduce components must read input data from STDIN.</span></span>
* <span data-ttu-id="d9442-126">**Kimeneti**: A térkép és összetevők kimeneti adatokat STDOUT kell írni.</span><span class="sxs-lookup"><span data-stu-id="d9442-126">**Output**: The map and reduce components must write output data to STDOUT.</span></span>
* <span data-ttu-id="d9442-127">**Az adatformátum**: felhasznált és előállított adatokat egy kulcs/érték pár, karakterrel elválasztó karakterláncként lapon kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d9442-127">**Data format**: The data consumed and produced must be a key/value pair, separated by a tab character.</span></span>

<span data-ttu-id="d9442-128">Python használatával könnyen képes kezelni ezeket a követelményeket a `sys` STDIN és használatával olvasni modul `print` STDOUT nyomtatás.</span><span class="sxs-lookup"><span data-stu-id="d9442-128">Python can easily handle these requirements by using the `sys` module to read from STDIN and using `print` to print to STDOUT.</span></span> <span data-ttu-id="d9442-129">A fennmaradó feladat egyszerűen formázza az adatokat a lap (`\t`) karaktert a kulcs és az érték között.</span><span class="sxs-lookup"><span data-stu-id="d9442-129">The remaining task is simply formatting the data with a tab (`\t`) character between the key and value.</span></span>

## <a name="create-the-mapper-and-reducer"></a><span data-ttu-id="d9442-130">A hozzárendelést és nyomáscsökkentő létrehozása</span><span class="sxs-lookup"><span data-stu-id="d9442-130">Create the mapper and reducer</span></span>

1. <span data-ttu-id="d9442-131">Hozzon létre egy fájlt `mapper.py` , és használja a tartalom a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="d9442-131">Create a file named `mapper.py` and use the following code as the content:</span></span>

   ```python
   #!/usr/bin/env python

   # Use the sys module
   import sys

   # 'file' in this case is STDIN
   def read_input(file):
       # Split each line into words
       for line in file:
           yield line.split()

   def main(separator='\t'):
       # Read the data using read_input
       data = read_input(sys.stdin)
       # Process each word returned from read_input
       for words in data:
           # Process each word
           for word in words:
               # Write to STDOUT
               print '%s%s%d' % (word, separator, 1)

   if __name__ == "__main__":
       main()
   ```

2. <span data-ttu-id="d9442-132">Hozzon létre egy fájlt **reducer.py** , és használja a tartalom a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="d9442-132">Create a file named **reducer.py** and use the following code as the content:</span></span>

   ```python
   #!/usr/bin/env python

   # import modules
   from itertools import groupby
   from operator import itemgetter
   import sys

   # 'file' in this case is STDIN
   def read_mapper_output(file, separator='\t'):
       # Go through each line
       for line in file:
           # Strip out the separator character
           yield line.rstrip().split(separator, 1)

   def main(separator='\t'):
       # Read the data using read_mapper_output
       data = read_mapper_output(sys.stdin, separator=separator)
       # Group words and counts into 'group'
       #   Since MapReduce is a distributed process, each word
       #   may have multiple counts. 'group' will have all counts
       #   which can be retrieved using the word as the key.
       for current_word, group in groupby(data, itemgetter(0)):
           try:
               # For each word, pull the count(s) for the word
               #   from 'group' and create a total count
               total_count = sum(int(count) for current_word, count in group)
               # Write to stdout
               print "%s%s%d" % (current_word, separator, total_count)
           except ValueError:
               # Count was not a number, so do nothing
               pass

   if __name__ == "__main__":
       main()
   ```

## <a name="run-using-powershell"></a><span data-ttu-id="d9442-133">Futtatás a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="d9442-133">Run using PowerShell</span></span>

<span data-ttu-id="d9442-134">Győződjön meg arról, hogy a fájlok rendelkeznek-e a megfelelő sorvégződések, használja a következő PowerShell-parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="d9442-134">To ensure that your files have the right line endings, use the following PowerShell script:</span></span>

<span data-ttu-id="d9442-135">[!code-powershell[fő](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]</span><span class="sxs-lookup"><span data-stu-id="d9442-135">[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]</span></span>

<span data-ttu-id="d9442-136">Töltse fel a fájlokat, a feladat futtatása és eredményének megtekintéséhez használja a következő PowerShell-parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="d9442-136">Use the following PowerShell script to upload the files, run the job, and view the output:</span></span>

<span data-ttu-id="d9442-137">[!code-powershell[fő](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]</span><span class="sxs-lookup"><span data-stu-id="d9442-137">[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]</span></span>

## <a name="run-from-an-ssh-session"></a><span data-ttu-id="d9442-138">Az SSH-munkamenetet futtatása</span><span class="sxs-lookup"><span data-stu-id="d9442-138">Run from an SSH session</span></span>

1. <span data-ttu-id="d9442-139">A fejlesztési környezetet ugyanabban a könyvtárban a `mapper.py` és `reducer.py` fájlok, a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="d9442-139">From your development environment, in the same directory as `mapper.py` and `reducer.py` files, use the following command:</span></span>

    ```bash
    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:
    ```

    <span data-ttu-id="d9442-140">Cserélje le `username` rendelkező a fürthöz az SSH-felhasználónév és `clustername` a fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="d9442-140">Replace `username` with the SSH user name for your cluster, and `clustername` with the name of your cluster.</span></span>

    <span data-ttu-id="d9442-141">Ez a parancs másolja át a fájlokat a helyi rendszer az átjárócsomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="d9442-141">This command copies the files from the local system to the head node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d9442-142">Ha a jelszó SSH fiókja biztonsága érdekében, a rendszer kéri a jelszót.</span><span class="sxs-lookup"><span data-stu-id="d9442-142">If you used a password to secure your SSH account, you are prompted for the password.</span></span> <span data-ttu-id="d9442-143">Ha SSH-kulcsot használt, előfordulhat, hogy használatára a `-i` paraméter és a titkos kulcs elérési útját.</span><span class="sxs-lookup"><span data-stu-id="d9442-143">If you used an SSH key, you may have to use the `-i` parameter and the path to the private key.</span></span> <span data-ttu-id="d9442-144">Például: `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.</span><span class="sxs-lookup"><span data-stu-id="d9442-144">For example, `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.</span></span>

2. <span data-ttu-id="d9442-145">Csatlakozzon a fürthöz SSH segítségével:</span><span class="sxs-lookup"><span data-stu-id="d9442-145">Connect to the cluster by using SSH:</span></span>

    ```bash
    ssh username@clustername-ssh.azurehdinsight.net`
    ```

    <span data-ttu-id="d9442-146">További információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="d9442-146">For more information on, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="d9442-147">Győződjön meg arról a mapper.py, illetve reducer.py rendelkezik a megfelelő sorvégeket, az alábbi parancsokat használja:</span><span class="sxs-lookup"><span data-stu-id="d9442-147">To ensure the mapper.py and reducer.py have the correct line endings, use the following commands:</span></span>

    ```bash
    perl -pi -e 's/\r\n/\n/g' mapper.py
    perl -pi -e 's/\r\n/\n/g' reducer.py
    ```

4. <span data-ttu-id="d9442-148">Az alábbi parancs segítségével indítsa el a MapReduce feladatot.</span><span class="sxs-lookup"><span data-stu-id="d9442-148">Use the following command to start the MapReduce job.</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
    ```

    <span data-ttu-id="d9442-149">Ez a parancs a következő részekből áll:</span><span class="sxs-lookup"><span data-stu-id="d9442-149">This command has the following parts:</span></span>

   * <span data-ttu-id="d9442-150">**hadoop-streaming.jar**: használt adatfolyam-továbbítási MapReduce műveletek végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="d9442-150">**hadoop-streaming.jar**: Used when performing streaming MapReduce operations.</span></span> <span data-ttu-id="d9442-151">A külső MapReduce kódot megadnia a Hadoop az illesztők.</span><span class="sxs-lookup"><span data-stu-id="d9442-151">It interfaces Hadoop with the external MapReduce code you provide.</span></span>

   * <span data-ttu-id="d9442-152">**-fájlok**: a megadott fájlokat ad hozzá a MapReduce feladatot.</span><span class="sxs-lookup"><span data-stu-id="d9442-152">**-files**: Adds the specified files to the MapReduce job.</span></span>

   * <span data-ttu-id="d9442-153">**-leképező**: közli a Hadoop melyik fájlt kívánja használni, mint a leképező.</span><span class="sxs-lookup"><span data-stu-id="d9442-153">**-mapper**: Tells Hadoop which file to use as the mapper.</span></span>

   * <span data-ttu-id="d9442-154">**-Nyomáscsökkentő**: közli a Hadoop kívánja használni, mint a nyomáscsökkentő fájlt.</span><span class="sxs-lookup"><span data-stu-id="d9442-154">**-reducer**: Tells Hadoop which file to use as the reducer.</span></span>

   * <span data-ttu-id="d9442-155">**-bemeneti**: A bemeneti fájl, amely azt számolja a jelenti.</span><span class="sxs-lookup"><span data-stu-id="d9442-155">**-input**: The input file that we should count words from.</span></span>

   * <span data-ttu-id="d9442-156">**-kimeneti**: az írt a kimeneti könyvtár.</span><span class="sxs-lookup"><span data-stu-id="d9442-156">**-output**: The directory that the output is written to.</span></span>

    <span data-ttu-id="d9442-157">A MapReduce feladatot működik, mert a folyamat százalékként jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d9442-157">As the MapReduce job works, the process is displayed as percentages.</span></span>

        <span data-ttu-id="d9442-158">15-02-05 19:01:04 információ mapreduce. Feladat: a térkép 0 % csökkentheti a 0 % 15-02-05 19:01:16 információ mapreduce. Feladat: a térkép 100 %-os csökkentése 0 % 15-02-05 19:01:27 információ mapreduce. Feladat: a térkép 100 %-os csökkentheti a 100 %-os</span><span class="sxs-lookup"><span data-stu-id="d9442-158">15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%</span></span>


5. <span data-ttu-id="d9442-159">A kimenet megtekintéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d9442-159">To view the output, use the following command:</span></span>

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    <span data-ttu-id="d9442-160">Ez a parancs megjeleníti szavak és hányszor a word történt.</span><span class="sxs-lookup"><span data-stu-id="d9442-160">This command displays a list of words and how many times the word occurred.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9442-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d9442-161">Next steps</span></span>

<span data-ttu-id="d9442-162">Most, hogy megismerte a MapRedcue folyamatos átviteli feladat használata a hdinsight eszközzel rendelkezik, az alábbi hivatkozások segítségével más módjai Azure HDInsight használata.</span><span class="sxs-lookup"><span data-stu-id="d9442-162">Now that you have learned how to use streaming MapRedcue jobs with HDInsight, use the following links to explore other ways to work with Azure HDInsight.</span></span>

* [<span data-ttu-id="d9442-163">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="d9442-163">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="d9442-164">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="d9442-164">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="d9442-165">MapReduce-feladatok használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="d9442-165">Use MapReduce jobs with HDInsight</span></span>](hdinsight-use-mapreduce.md)
