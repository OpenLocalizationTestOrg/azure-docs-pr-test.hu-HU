---
title: a HDInsight - Azure feladatok streaming MapReduce Python aaaDevelop |} Microsoft Docs
description: "Megtudhatja, hogyan toouse Python MapReduce-feladatok adatfolyam. Hadoop streamelési API biztosít a Java nyelven írt a MapReduce."
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
ms.openlocfilehash: a6ae3ba650b665ecc5839a4ddf5282f8ccfb6bd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-python-streaming-mapreduce-programs-for-hdinsight"></a><span data-ttu-id="73aba-104">A HDInsight MapReduce programok streaming Python fejlesztése</span><span class="sxs-lookup"><span data-stu-id="73aba-104">Develop Python streaming MapReduce programs for HDInsight</span></span>

<span data-ttu-id="73aba-105">Megtudhatja, hogyan toouse Python MapReduce műveletek adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="73aba-105">Learn how toouse Python in streaming MapReduce operations.</span></span> <span data-ttu-id="73aba-106">Hadoop streamelési API biztosít, amely lehetővé teszi, hogy a toowrite térkép, és csökkentheti a funkciók nem Java nyelven MapReduce.</span><span class="sxs-lookup"><span data-stu-id="73aba-106">Hadoop provides a streaming API for MapReduce that enables you toowrite map and reduce functions in languages other than Java.</span></span> <span data-ttu-id="73aba-107">hello jelen dokumentumban leírt lépések végrehajtása hello térkép és csökkentse az összetevők a Python.</span><span class="sxs-lookup"><span data-stu-id="73aba-107">hello steps in this document implement hello Map and Reduce components in Python.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73aba-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="73aba-108">Prerequisites</span></span>

* <span data-ttu-id="73aba-109">A Linux-based Hadoop on HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="73aba-109">A Linux-based Hadoop on HDInsight cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="73aba-110">hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="73aba-110">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="73aba-111">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="73aba-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="73aba-112">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="73aba-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="73aba-113">Egy szövegszerkesztőben</span><span class="sxs-lookup"><span data-stu-id="73aba-113">A text editor</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="73aba-114">hello szövegszerkesztőben LF hello sor befejezési karakterként használjon.</span><span class="sxs-lookup"><span data-stu-id="73aba-114">hello text editor must use LF as hello line ending.</span></span> <span data-ttu-id="73aba-115">Egy sor vége CRLF használata okoz hibák hello MapReduce feladatot futó Linux-alapú HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="73aba-115">Using a line ending of CRLF causes errors when running hello MapReduce job on Linux-based HDInsight clusters.</span></span>

* <span data-ttu-id="73aba-116">Hello `ssh` és `scp` parancsok, vagy [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)</span><span class="sxs-lookup"><span data-stu-id="73aba-116">hello `ssh` and `scp` commands, or [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)</span></span>

## <a name="word-count"></a><span data-ttu-id="73aba-117">Word száma</span><span class="sxs-lookup"><span data-stu-id="73aba-117">Word count</span></span>

<span data-ttu-id="73aba-118">Ebben a példában az alapvető szószámot python megvalósítva, a hozzárendelést és nyomáscsökkentő.</span><span class="sxs-lookup"><span data-stu-id="73aba-118">This example is a basic word count implemented in a python a mapper and reducer.</span></span> <span data-ttu-id="73aba-119">hello leképező mondat bontja egyes szavak és hello nyomáscsökkentő hello szavak összesíti és tooproduce hello kimeneti száma.</span><span class="sxs-lookup"><span data-stu-id="73aba-119">hello mapper breaks sentences into individual words, and hello reducer aggregates hello words and counts tooproduce hello output.</span></span>

<span data-ttu-id="73aba-120">hello a következő folyamatábra bemutatja, mi történik a hello leképezés során, és csökkentheti a fázisok.</span><span class="sxs-lookup"><span data-stu-id="73aba-120">hello following flowchart illustrates what happens during hello map and reduce phases.</span></span>

![hello mapreduce folyamat ábrája](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="streaming-mapreduce"></a><span data-ttu-id="73aba-122">Adatfolyam-továbbítási MapReduce</span><span class="sxs-lookup"><span data-stu-id="73aba-122">Streaming MapReduce</span></span>

<span data-ttu-id="73aba-123">Hadoop lehetővé teszi egy fájlt, amely hello térkép tartalmaz, és csökkenti a feladat által használt logikai toospecify.</span><span class="sxs-lookup"><span data-stu-id="73aba-123">Hadoop allows you toospecify a file that contains hello map and reduce logic that is used by a job.</span></span> <span data-ttu-id="73aba-124">konkrét követelmények hello hello rendelve, és csökkentse a logikai vannak:</span><span class="sxs-lookup"><span data-stu-id="73aba-124">hello specific requirements for hello map and reduce logic are:</span></span>

* <span data-ttu-id="73aba-125">**Bemeneti**: hello térkép, így csökkentheti összetevők bemeneti adatokat STDIN kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="73aba-125">**Input**: hello map and reduce components must read input data from STDIN.</span></span>
* <span data-ttu-id="73aba-126">**Kimeneti**: hello térkép, így csökkentheti összetevők kell írnia a kimeneti adatok tooSTDOUT.</span><span class="sxs-lookup"><span data-stu-id="73aba-126">**Output**: hello map and reduce components must write output data tooSTDOUT.</span></span>
* <span data-ttu-id="73aba-127">**Az adatformátum**: hello adatok felhasznált és előállított egy kulcs/érték pár, karakterrel elválasztó karakterláncként lapon kell lennie.</span><span class="sxs-lookup"><span data-stu-id="73aba-127">**Data format**: hello data consumed and produced must be a key/value pair, separated by a tab character.</span></span>

<span data-ttu-id="73aba-128">Python könnyen kezelhető követelménynek hello segítségével `sys` modul tooread STDIN és használatával `print` tooprint tooSTDOUT.</span><span class="sxs-lookup"><span data-stu-id="73aba-128">Python can easily handle these requirements by using hello `sys` module tooread from STDIN and using `print` tooprint tooSTDOUT.</span></span> <span data-ttu-id="73aba-129">hello fennmaradó feladat van egyszerűen hello adatok formázása a lap (`\t`) karakter közötti hello kulcs-érték.</span><span class="sxs-lookup"><span data-stu-id="73aba-129">hello remaining task is simply formatting hello data with a tab (`\t`) character between hello key and value.</span></span>

## <a name="create-hello-mapper-and-reducer"></a><span data-ttu-id="73aba-130">Hello leképező és nyomáscsökkentő létrehozása</span><span class="sxs-lookup"><span data-stu-id="73aba-130">Create hello mapper and reducer</span></span>

1. <span data-ttu-id="73aba-131">Hozzon létre egy fájlt `mapper.py` és hello használja a következő kód hello tartalmat:</span><span class="sxs-lookup"><span data-stu-id="73aba-131">Create a file named `mapper.py` and use hello following code as hello content:</span></span>

   ```python
   #!/usr/bin/env python

   # Use hello sys module
   import sys

   # 'file' in this case is STDIN
   def read_input(file):
       # Split each line into words
       for line in file:
           yield line.split()

   def main(separator='\t'):
       # Read hello data using read_input
       data = read_input(sys.stdin)
       # Process each word returned from read_input
       for words in data:
           # Process each word
           for word in words:
               # Write tooSTDOUT
               print '%s%s%d' % (word, separator, 1)

   if __name__ == "__main__":
       main()
   ```

2. <span data-ttu-id="73aba-132">Hozzon létre egy fájlt **reducer.py** és hello használja a következő kód hello tartalmat:</span><span class="sxs-lookup"><span data-stu-id="73aba-132">Create a file named **reducer.py** and use hello following code as hello content:</span></span>

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
           # Strip out hello separator character
           yield line.rstrip().split(separator, 1)

   def main(separator='\t'):
       # Read hello data using read_mapper_output
       data = read_mapper_output(sys.stdin, separator=separator)
       # Group words and counts into 'group'
       #   Since MapReduce is a distributed process, each word
       #   may have multiple counts. 'group' will have all counts
       #   which can be retrieved using hello word as hello key.
       for current_word, group in groupby(data, itemgetter(0)):
           try:
               # For each word, pull hello count(s) for hello word
               #   from 'group' and create a total count
               total_count = sum(int(count) for current_word, count in group)
               # Write toostdout
               print "%s%s%d" % (current_word, separator, total_count)
           except ValueError:
               # Count was not a number, so do nothing
               pass

   if __name__ == "__main__":
       main()
   ```

## <a name="run-using-powershell"></a><span data-ttu-id="73aba-133">Futtatás a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="73aba-133">Run using PowerShell</span></span>

<span data-ttu-id="73aba-134">tooensure, hogy az megfelelő hello jobb sorvégeket, a következő PowerShell-parancsfájl használata hello:</span><span class="sxs-lookup"><span data-stu-id="73aba-134">tooensure that your files have hello right line endings, use hello following PowerShell script:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]

<span data-ttu-id="73aba-135">Használja a következő PowerShell parancsfájl tooupload hello fájlok hello hello feladat futtatása és hello kimeneti megtekintése:</span><span class="sxs-lookup"><span data-stu-id="73aba-135">Use hello following PowerShell script tooupload hello files, run hello job, and view hello output:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]

## <a name="run-from-an-ssh-session"></a><span data-ttu-id="73aba-136">Az SSH-munkamenetet futtatása</span><span class="sxs-lookup"><span data-stu-id="73aba-136">Run from an SSH session</span></span>

1. <span data-ttu-id="73aba-137">A fejlesztési környezetet az a hello azonos könyvtárhoz, mint a `mapper.py` és `reducer.py` fájlok, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="73aba-137">From your development environment, in hello same directory as `mapper.py` and `reducer.py` files, use hello following command:</span></span>

    ```bash
    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:
    ```

    <span data-ttu-id="73aba-138">Cserélje le `username` a hello SSH-felhasználónév a fürtöt, és `clustername` hello néven a fürt.</span><span class="sxs-lookup"><span data-stu-id="73aba-138">Replace `username` with hello SSH user name for your cluster, and `clustername` with hello name of your cluster.</span></span>

    <span data-ttu-id="73aba-139">Ez a parancs hello helyi rendszer toohello átjárócsomópont hello fájlokat másolja át.</span><span class="sxs-lookup"><span data-stu-id="73aba-139">This command copies hello files from hello local system toohello head node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="73aba-140">Ha a jelszó toosecure SSH-fiókját, hello jelszót kéri.</span><span class="sxs-lookup"><span data-stu-id="73aba-140">If you used a password toosecure your SSH account, you are prompted for hello password.</span></span> <span data-ttu-id="73aba-141">Ha SSH-kulcsot használt, előfordulhat, hogy toouse hello `-i` paraméter és hello elérési személyes toohello-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="73aba-141">If you used an SSH key, you may have toouse hello `-i` parameter and hello path toohello private key.</span></span> <span data-ttu-id="73aba-142">Például: `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.</span><span class="sxs-lookup"><span data-stu-id="73aba-142">For example, `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.</span></span>

2. <span data-ttu-id="73aba-143">Csatlakozás toohello fürt SSH segítségével:</span><span class="sxs-lookup"><span data-stu-id="73aba-143">Connect toohello cluster by using SSH:</span></span>

    ```bash
    ssh username@clustername-ssh.azurehdinsight.net`
    ```

    <span data-ttu-id="73aba-144">További információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="73aba-144">For more information on, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="73aba-145">tooensure hello mapper.py és reducer.py rendelkezik hello sorvégződések javítsa ki, a következő parancsok hello használata:</span><span class="sxs-lookup"><span data-stu-id="73aba-145">tooensure hello mapper.py and reducer.py have hello correct line endings, use hello following commands:</span></span>

    ```bash
    perl -pi -e 's/\r\n/\n/g' mapper.py
    perl -pi -e 's/\r\n/\n/g' reducer.py
    ```

4. <span data-ttu-id="73aba-146">A következő parancs toostart hello MapReduce feladatot hello használata.</span><span class="sxs-lookup"><span data-stu-id="73aba-146">Use hello following command toostart hello MapReduce job.</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
    ```

    <span data-ttu-id="73aba-147">Ez a parancs a következő részek hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="73aba-147">This command has hello following parts:</span></span>

   * <span data-ttu-id="73aba-148">**hadoop-streaming.jar**: használt adatfolyam-továbbítási MapReduce műveletek végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="73aba-148">**hadoop-streaming.jar**: Used when performing streaming MapReduce operations.</span></span> <span data-ttu-id="73aba-149">Az illesztők Hadoop kóddal hello külső MapReduce megadnia.</span><span class="sxs-lookup"><span data-stu-id="73aba-149">It interfaces Hadoop with hello external MapReduce code you provide.</span></span>

   * <span data-ttu-id="73aba-150">**-fájlok**: hozzáadja a megadott hello fájlok toohello MapReduce feladatot.</span><span class="sxs-lookup"><span data-stu-id="73aba-150">**-files**: Adds hello specified files toohello MapReduce job.</span></span>

   * <span data-ttu-id="73aba-151">**-leképező**: toouse fájl leképező hello, amely közli Hadoop.</span><span class="sxs-lookup"><span data-stu-id="73aba-151">**-mapper**: Tells Hadoop which file toouse as hello mapper.</span></span>

   * <span data-ttu-id="73aba-152">**-Nyomáscsökkentő**: közli Hadoop, amely toouse fájlt, mert nyomáscsökkentő hello.</span><span class="sxs-lookup"><span data-stu-id="73aba-152">**-reducer**: Tells Hadoop which file toouse as hello reducer.</span></span>

   * <span data-ttu-id="73aba-153">**-bemeneti**: hello bemeneti fájl, amely azt számolja a jelenti.</span><span class="sxs-lookup"><span data-stu-id="73aba-153">**-input**: hello input file that we should count words from.</span></span>

   * <span data-ttu-id="73aba-154">**-kimeneti**: hello hello kimeneti könyvtár nevével.</span><span class="sxs-lookup"><span data-stu-id="73aba-154">**-output**: hello directory that hello output is written to.</span></span>

    <span data-ttu-id="73aba-155">Hello MapReduce feladatot működik, mint hello folyamat százalékként jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="73aba-155">As hello MapReduce job works, hello process is displayed as percentages.</span></span>

        <span data-ttu-id="73aba-156">15-02-05 19:01:04 információ mapreduce. Feladat: a térkép 0 % csökkentheti a 0 % 15-02-05 19:01:16 információ mapreduce. Feladat: a térkép 100 %-os csökkentése 0 % 15-02-05 19:01:27 információ mapreduce. Feladat: a térkép 100 %-os csökkentheti a 100 %-os</span><span class="sxs-lookup"><span data-stu-id="73aba-156">15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%</span></span>


5. <span data-ttu-id="73aba-157">tooview hello kimeneti, használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="73aba-157">tooview hello output, use hello following command:</span></span>

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    <span data-ttu-id="73aba-158">Ez a parancs megjeleníti szavak és hányszor hello word történt.</span><span class="sxs-lookup"><span data-stu-id="73aba-158">This command displays a list of words and how many times hello word occurred.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73aba-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="73aba-159">Next steps</span></span>

<span data-ttu-id="73aba-160">Most, hogy megtanulta, hogyan streaming MapRedcue toouse feladatok a hdinsight eszközzel, használja a következő hivatkozások tooexplore hello más módokon toowork Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="73aba-160">Now that you have learned how toouse streaming MapRedcue jobs with HDInsight, use hello following links tooexplore other ways toowork with Azure HDInsight.</span></span>

* [<span data-ttu-id="73aba-161">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="73aba-161">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="73aba-162">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="73aba-162">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="73aba-163">MapReduce-feladatok használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="73aba-163">Use MapReduce jobs with HDInsight</span></span>](hdinsight-use-mapreduce.md)
