---
title: "aaaMapReduce és a hadooppal a Hdinsightban - Azure SSH-kapcsolat |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse SSH toorun MapReduce feladatokat, a HDInsight Hadoop használatával."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 844678ba-1e1f-4fda-b9ef-34df4035d547
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 9626577687fc5cc119a39d65a9c45298f57f81c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a><span data-ttu-id="01052-103">Az SSH hdinsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="01052-103">Use MapReduce with Hadoop on HDInsight with SSH</span></span>

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="01052-104">Ismerje meg, hogyan toosubmit MapReduce egy Secure Shell (SSH) kapcsolat tooHDInsight a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="01052-104">Learn how toosubmit MapReduce jobs from a Secure Shell (SSH) connection tooHDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="01052-105">Ha már ismeri a Linux-alapú Hadoop használatával kiszolgálók, de új tooHDInsight, lásd: [Linux-alapú HDInsight tippek](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="01052-105">If you are already familiar with using Linux-based Hadoop servers, but you are new tooHDInsight, see [Linux-based HDInsight tips](hdinsight-hadoop-linux-information.md).</span></span>

## <span data-ttu-id="01052-106"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="01052-106"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="01052-107">(A HDInsight Hadoop) a Linux-alapú HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="01052-107">A Linux-based HDInsight (Hadoop on HDInsight) cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="01052-108">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="01052-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="01052-109">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="01052-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="01052-110">Egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="01052-110">An SSH client.</span></span> <span data-ttu-id="01052-111">További információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="01052-111">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

## <span data-ttu-id="01052-112"><a id="ssh"></a>Csatlakozzon SSH</span><span class="sxs-lookup"><span data-stu-id="01052-112"><a id="ssh"></a>Connect with SSH</span></span>

<span data-ttu-id="01052-113">Csatlakozzon az SSH használatával toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="01052-113">Connect toohello cluster using SSH.</span></span> <span data-ttu-id="01052-114">Például a következő parancs hello csatlakozik nevű tooa fürt **myhdinsight**:</span><span class="sxs-lookup"><span data-stu-id="01052-114">For example, hello following command connects tooa cluster named **myhdinsight**:</span></span>

```bash
ssh admin@myhdinsight-ssh.azurehdinsight.net
```

<span data-ttu-id="01052-115">**Ha egy tanúsítvány-kulcsot használ SSH hitelesítés**, szükség lehet hello titkos kulcs toospecify hello helyét az ügyfél rendszerén, például:</span><span class="sxs-lookup"><span data-stu-id="01052-115">**If you use a certificate key for SSH authentication**, you may need toospecify hello location of hello private key on your client system, for example:</span></span>

```bash
ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net
```

<span data-ttu-id="01052-116">**Ha jelszót használhat SSH hitelesítés**, tooprovide hello jelszót van szüksége.</span><span class="sxs-lookup"><span data-stu-id="01052-116">**If you use a password for SSH authentication**, you need tooprovide hello password when prompted.</span></span>

<span data-ttu-id="01052-117">Az SSH és a HDInsight együttes használatával további információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="01052-117">For more information on using SSH with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="01052-118"><a id="hadoop"></a>Hadoop-parancsok használata</span><span class="sxs-lookup"><span data-stu-id="01052-118"><a id="hadoop"></a>Use Hadoop commands</span></span>

1. <span data-ttu-id="01052-119">Után csatlakoztatott toohello HDInsight-fürtre, használja a következő parancs toostart MapReduce feladatot hello:</span><span class="sxs-lookup"><span data-stu-id="01052-119">After you are connected toohello HDInsight cluster, use hello following command toostart a MapReduce job:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    <span data-ttu-id="01052-120">A paranccsal elindítja hello `wordcount` osztályt, amelyet hello `hadoop-mapreduce-examples.jar` fájlt.</span><span class="sxs-lookup"><span data-stu-id="01052-120">This command starts hello `wordcount` class, which is contained in hello `hadoop-mapreduce-examples.jar` file.</span></span> <span data-ttu-id="01052-121">Hello használ `/example/data/gutenberg/davinci.txt` bemeneti és kimeneti dokumentumot tárolódik `/example/data/WordCountOutput`.</span><span class="sxs-lookup"><span data-stu-id="01052-121">It uses hello `/example/data/gutenberg/davinci.txt` document as input, and output is stored at `/example/data/WordCountOutput`.</span></span>

    > [!NOTE]
    > <span data-ttu-id="01052-122">A MapReduce feladatot és hello példa adatokkal kapcsolatos további információkért lásd: [használata MapReduce a Hadoop on HDInsight](hdinsight-use-mapreduce.md).</span><span class="sxs-lookup"><span data-stu-id="01052-122">For more information about this MapReduce job and hello example data, see [Use MapReduce in Hadoop on HDInsight](hdinsight-use-mapreduce.md).</span></span>

2. <span data-ttu-id="01052-123">hello feladat részletei bocsát ki, akkor feldolgozza, és információt hasonló toohello hello feladat befejezésekor a következő szöveget adja vissza:</span><span class="sxs-lookup"><span data-stu-id="01052-123">hello job emits details as it processes, and it returns information similar toohello following text when hello job completes:</span></span>

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. <span data-ttu-id="01052-124">Hello feladat befejezése után használja a következő parancs toolist hello kimeneti fájlok hello:</span><span class="sxs-lookup"><span data-stu-id="01052-124">When hello job completes, use hello following command toolist hello output files:</span></span>

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    <span data-ttu-id="01052-125">Ez a parancs két fájlt megjelenítése `_SUCCESS` és `part-r-00000`.</span><span class="sxs-lookup"><span data-stu-id="01052-125">This command display two files, `_SUCCESS` and `part-r-00000`.</span></span> <span data-ttu-id="01052-126">Hello `part-r-00000` fájl tartalmazza a feladat hello kimenet.</span><span class="sxs-lookup"><span data-stu-id="01052-126">hello `part-r-00000` file contains hello output for this job.</span></span>

    > [!NOTE]
    > <span data-ttu-id="01052-127">Bizonyos MapReduce-feladatok hello eredmények lehet, hogy e osztani több **rész-r-###** fájlokat.</span><span class="sxs-lookup"><span data-stu-id="01052-127">Some MapReduce jobs may split hello results across multiple **part-r-#####** files.</span></span> <span data-ttu-id="01052-128">Ha igen, használjon hello ### utótag hello fájlok tooindicate hello sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="01052-128">If so, use hello ##### suffix tooindicate hello order of hello files.</span></span>

4. <span data-ttu-id="01052-129">tooview hello kimeneti, használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="01052-129">tooview hello output, use hello following command:</span></span>

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    <span data-ttu-id="01052-130">Ez a parancs szereplő hello szavak listáját jeleníti meg hello **wasb://example/data/gutenberg/davinci.txt** fájl- és hello száma minden szó történt.</span><span class="sxs-lookup"><span data-stu-id="01052-130">This command displays a list of hello words that are contained in hello **wasb://example/data/gutenberg/davinci.txt** file and hello number of times each word occurred.</span></span> <span data-ttu-id="01052-131">hello következő szövege hello tárolt adatokat használó hello fájlban egy példát:</span><span class="sxs-lookup"><span data-stu-id="01052-131">hello following text is an example of hello data that is contained in hello file:</span></span>

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <span data-ttu-id="01052-132"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="01052-132"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="01052-133">Ahogy látja, Hadoop parancsok egy egyszerűen toorun MapReduce-feladatok egy HDInsight-fürtöt, és a nézet hello feladatkiemenetét adja meg.</span><span class="sxs-lookup"><span data-stu-id="01052-133">As you can see, Hadoop commands provide an easy way toorun MapReduce jobs in an HDInsight cluster and then view hello job output.</span></span>

## <span data-ttu-id="01052-134"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="01052-134"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="01052-135">Általános információk a hdinsight MapReduce-feladatok:</span><span class="sxs-lookup"><span data-stu-id="01052-135">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="01052-136">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="01052-136">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="01052-137">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="01052-137">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="01052-138">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="01052-138">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="01052-139">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="01052-139">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
