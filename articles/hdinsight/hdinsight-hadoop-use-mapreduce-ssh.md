---
title: "A HDInsight - Azure Hadoop MapReduce és SSH kapcsolatot |} Microsoft Docs"
description: "Útmutató az SSH használata a HDInsight Hadoop használatával MapReduce-feladatok futtatásához."
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
ms.openlocfilehash: eaf6278f97cd5ddd7e049ff4745181f39d7949a0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a><span data-ttu-id="bdcfe-103">Az SSH hdinsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="bdcfe-103">Use MapReduce with Hadoop on HDInsight with SSH</span></span>

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="bdcfe-104">Útmutató a HDInsight a Secure Shell (SSH) kapcsolatról MapReduce-feladatok elküldése.</span><span class="sxs-lookup"><span data-stu-id="bdcfe-104">Learn how to submit MapReduce jobs from a Secure Shell (SSH) connection to HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="bdcfe-105">Ha már ismeri a Linux-alapú Hadoop-kiszolgálókat használ, de még nem ismeri a HDInsight, [Linux-alapú HDInsight tippek](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="bdcfe-105">If you are already familiar with using Linux-based Hadoop servers, but you are new to HDInsight, see [Linux-based HDInsight tips](hdinsight-hadoop-linux-information.md).</span></span>

## <span data-ttu-id="bdcfe-106"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bdcfe-106"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="bdcfe-107">(A HDInsight Hadoop) a Linux-alapú HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="bdcfe-107">A Linux-based HDInsight (Hadoop on HDInsight) cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="bdcfe-108">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="bdcfe-108">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="bdcfe-109">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="bdcfe-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="bdcfe-110">Egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="bdcfe-110">An SSH client.</span></span> <span data-ttu-id="bdcfe-111">További információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="bdcfe-111">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

## <span data-ttu-id="bdcfe-112"><a id="ssh"></a>Csatlakozzon SSH</span><span class="sxs-lookup"><span data-stu-id="bdcfe-112"><a id="ssh"></a>Connect with SSH</span></span>

<span data-ttu-id="bdcfe-113">Csatlakozzon a fürthöz SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="bdcfe-113">Connect to the cluster using SSH.</span></span> <span data-ttu-id="bdcfe-114">A következő parancs például nevű fürthöz csatlakozó **myhdinsight**:</span><span class="sxs-lookup"><span data-stu-id="bdcfe-114">For example, the following command connects to a cluster named **myhdinsight**:</span></span>

```bash
ssh admin@myhdinsight-ssh.azurehdinsight.net
```

<span data-ttu-id="bdcfe-115">**Ha egy tanúsítvány-kulcsot használ SSH hitelesítés**, adja meg a titkos kulcs helyét az ügyfélrendszeren szeretne:</span><span class="sxs-lookup"><span data-stu-id="bdcfe-115">**If you use a certificate key for SSH authentication**, you may need to specify the location of the private key on your client system, for example:</span></span>

```bash
ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net
```

<span data-ttu-id="bdcfe-116">**Ha jelszót használhat SSH hitelesítés**, meg kell adnia a jelszót.</span><span class="sxs-lookup"><span data-stu-id="bdcfe-116">**If you use a password for SSH authentication**, you need to provide the password when prompted.</span></span>

<span data-ttu-id="bdcfe-117">Az SSH és a HDInsight együttes használatával további információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="bdcfe-117">For more information on using SSH with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="bdcfe-118"><a id="hadoop"></a>Hadoop-parancsok használata</span><span class="sxs-lookup"><span data-stu-id="bdcfe-118"><a id="hadoop"></a>Use Hadoop commands</span></span>

1. <span data-ttu-id="bdcfe-119">Miután csatlakozott a HDInsight-fürthöz, a következő parancs segítségével indítsa el a MapReduce feladatot:</span><span class="sxs-lookup"><span data-stu-id="bdcfe-119">After you are connected to the HDInsight cluster, use the following command to start a MapReduce job:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    <span data-ttu-id="bdcfe-120">A paranccsal elindítja a `wordcount` osztályt, amely tartalmazza a `hadoop-mapreduce-examples.jar` fájlt.</span><span class="sxs-lookup"><span data-stu-id="bdcfe-120">This command starts the `wordcount` class, which is contained in the `hadoop-mapreduce-examples.jar` file.</span></span> <span data-ttu-id="bdcfe-121">Használja a `/example/data/gutenberg/davinci.txt` bemeneti és kimeneti dokumentumot tárolódik `/example/data/WordCountOutput`.</span><span class="sxs-lookup"><span data-stu-id="bdcfe-121">It uses the `/example/data/gutenberg/davinci.txt` document as input, and output is stored at `/example/data/WordCountOutput`.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bdcfe-122">A MapReduce feladatot és a példaadatokat kapcsolatos további információkért lásd: [használata MapReduce a Hadoop on HDInsight](hdinsight-use-mapreduce.md).</span><span class="sxs-lookup"><span data-stu-id="bdcfe-122">For more information about this MapReduce job and the example data, see [Use MapReduce in Hadoop on HDInsight](hdinsight-use-mapreduce.md).</span></span>

2. <span data-ttu-id="bdcfe-123">A feladat részletei bocsát ki, akkor feldolgozza, és olyan információkat ad vissza az alábbihoz hasonló a feladat befejezése után:</span><span class="sxs-lookup"><span data-stu-id="bdcfe-123">The job emits details as it processes, and it returns information similar to the following text when the job completes:</span></span>

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. <span data-ttu-id="bdcfe-124">A feladat befejezése után, használja a következő parancsot a kimeneti fájlok listázásához:</span><span class="sxs-lookup"><span data-stu-id="bdcfe-124">When the job completes, use the following command to list the output files:</span></span>

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    <span data-ttu-id="bdcfe-125">Ez a parancs két fájlt megjelenítése `_SUCCESS` és `part-r-00000`.</span><span class="sxs-lookup"><span data-stu-id="bdcfe-125">This command display two files, `_SUCCESS` and `part-r-00000`.</span></span> <span data-ttu-id="bdcfe-126">A `part-r-00000` fájl tartalmazza a kimenet ehhez a feladathoz.</span><span class="sxs-lookup"><span data-stu-id="bdcfe-126">The `part-r-00000` file contains the output for this job.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bdcfe-127">Bizonyos MapReduce-feladatok az eredmények lehet, hogy e osztani több **rész-r-###** fájlokat.</span><span class="sxs-lookup"><span data-stu-id="bdcfe-127">Some MapReduce jobs may split the results across multiple **part-r-#####** files.</span></span> <span data-ttu-id="bdcfe-128">Ha igen, használja a ### utótag fájlok sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="bdcfe-128">If so, use the ##### suffix to indicate the order of the files.</span></span>

4. <span data-ttu-id="bdcfe-129">A kimenet megtekintéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="bdcfe-129">To view the output, use the following command:</span></span>

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    <span data-ttu-id="bdcfe-130">Ez a parancs megjeleníti a szavakat, amelyek szerepelnek a **wasb://example/data/gutenberg/davinci.txt** fájl- és a szám, ahányszor minden szó történt.</span><span class="sxs-lookup"><span data-stu-id="bdcfe-130">This command displays a list of the words that are contained in the **wasb://example/data/gutenberg/davinci.txt** file and the number of times each word occurred.</span></span> <span data-ttu-id="bdcfe-131">A következő szöveget a fájlban található adatok példája:</span><span class="sxs-lookup"><span data-stu-id="bdcfe-131">The following text is an example of the data that is contained in the file:</span></span>

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <span data-ttu-id="bdcfe-132"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="bdcfe-132"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="bdcfe-133">Ahogy látja, a Hadoop parancsok MapReduce-feladatok futtatása a HDInsight-fürtöt, és nézze meg a feladat kimenetére egyszerűen adja meg.</span><span class="sxs-lookup"><span data-stu-id="bdcfe-133">As you can see, Hadoop commands provide an easy way to run MapReduce jobs in an HDInsight cluster and then view the job output.</span></span>

## <span data-ttu-id="bdcfe-134"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bdcfe-134"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="bdcfe-135">Általános információk a hdinsight MapReduce-feladatok:</span><span class="sxs-lookup"><span data-stu-id="bdcfe-135">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="bdcfe-136">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="bdcfe-136">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="bdcfe-137">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="bdcfe-137">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="bdcfe-138">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="bdcfe-138">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="bdcfe-139">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="bdcfe-139">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
