---
title: "MapReduce és a távoli asztal hadooppal a Hdinsightban - Azure |} Microsoft Docs"
description: "Útmutató: a távoli asztal használatával csatlakozhat a HDInsight Hadoop, és a MapReduce-feladatok futtatása."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9d3a7b34-7def-4c2e-bb6c-52682d30dee8
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: b56674857b013f9bb3d4dd4b6e97b34e0a97b1b2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a><span data-ttu-id="24c54-103">A távoli asztal hdinsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="24c54-103">Use MapReduce in Hadoop on HDInsight with Remote Desktop</span></span>
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="24c54-104">Ebből a cikkből megtudhatja a távoli asztal használatával kapcsolódik a Hadoop on HDInsight-fürt és a Hadoop-paranccsal futtassa a MapReduce-feladatok.</span><span class="sxs-lookup"><span data-stu-id="24c54-104">In this article, you will learn how to connect to a Hadoop on HDInsight cluster by using Remote Desktop and then run MapReduce jobs by using the Hadoop command.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="24c54-105">Távoli asztali Windows-alapú HDInsight-fürtök csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="24c54-105">Remote Desktop is only available on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="24c54-106">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="24c54-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="24c54-107">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="24c54-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="24c54-108">HDInsight 3.4-es vagy nagyobb, lásd: [az SSH használata MapReduce](hdinsight-hadoop-use-mapreduce-ssh.md) információk a HDInsight-fürthöz csatlakozik, és a MapReduce-feladatok futtatása.</span><span class="sxs-lookup"><span data-stu-id="24c54-108">For HDInsight 3.4 or greater, see [Use MapReduce with SSH](hdinsight-hadoop-use-mapreduce-ssh.md) for information on connecting to the HDInsight cluster and running MapReduce jobs.</span></span>

## <span data-ttu-id="24c54-109"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="24c54-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="24c54-110">Ebben a cikkben szereplő lépések elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="24c54-110">To complete the steps in this article, you will need the following:</span></span>

* <span data-ttu-id="24c54-111">(A HDInsight Hadoop) a Windows-alapú HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="24c54-111">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="24c54-112">Windows 10, Windows 8 vagy Windows 7 rendszert futtató ügyfélszámítógép</span><span class="sxs-lookup"><span data-stu-id="24c54-112">A client computer running Windows 10, Windows 8, or Windows 7</span></span>

## <span data-ttu-id="24c54-113"><a id="connect"></a>Csatlakozzon a távoli asztal</span><span class="sxs-lookup"><span data-stu-id="24c54-113"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="24c54-114">A távoli asztal engedélyezése a HDInsight-fürthöz, majd csatlakozni a következő utasításokat követve [csatlakozás RDP Funkciót használnak a HDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="24c54-114">Enable Remote Desktop for the HDInsight cluster, then connect to it by following the instructions at [Connect to HDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="24c54-115"><a id="hadoop"></a>A Hadoop paranccsal</span><span class="sxs-lookup"><span data-stu-id="24c54-115"><a id="hadoop"></a>Use the Hadoop command</span></span>
<span data-ttu-id="24c54-116">Az asztalon a HDInsight-fürthöz csatlakozik, az az alábbi lépések segítségével a Hadoop-paranccsal futtassa a MapReduce feladatot:</span><span class="sxs-lookup"><span data-stu-id="24c54-116">When you are connected to the desktop for the HDInsight cluster, use the following steps to run a MapReduce job by using the Hadoop command:</span></span>

1. <span data-ttu-id="24c54-117">A HDInsight asztalról indítsa el a **Hadoop parancssori**.</span><span class="sxs-lookup"><span data-stu-id="24c54-117">From the HDInsight desktop, start the **Hadoop Command Line**.</span></span> <span data-ttu-id="24c54-118">Ekkor megnyílik egy új, a parancssort a **c:\apps\dist\hadoop-&lt;verziószám >** directory.</span><span class="sxs-lookup"><span data-stu-id="24c54-118">This opens a new command prompt in the **c:\apps\dist\hadoop-&lt;version number>** directory.</span></span>

   > [!NOTE]
   > <span data-ttu-id="24c54-119">A verziószám változik Hadoop frissül.</span><span class="sxs-lookup"><span data-stu-id="24c54-119">The version number changes as Hadoop is updated.</span></span> <span data-ttu-id="24c54-120">A **HADOOP_HOME** környezeti változó segítségével található az elérési út.</span><span class="sxs-lookup"><span data-stu-id="24c54-120">The **HADOOP_HOME** environment variable can be used to find the path.</span></span> <span data-ttu-id="24c54-121">Például `cd %HADOOP_HOME%` könyvtárak módosítja a Hadoop-címtárban, anélkül, hogy ismeri a verziószámot.</span><span class="sxs-lookup"><span data-stu-id="24c54-121">For example, `cd %HADOOP_HOME%` changes directories to the Hadoop directory, without requiring you to know the version number.</span></span>
   >
   >
2. <span data-ttu-id="24c54-122">Használatához a **Hadoop** parancs egy példa MapReduce feladatot futtatni, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="24c54-122">To use the **Hadoop** command to run an example MapReduce job, use the following command:</span></span>

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    <span data-ttu-id="24c54-123">Ekkor elindul a **wordcount** osztályt, amely tartalmazza a **hadoop-mapreduce-examples.jar** fájl az aktuális könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="24c54-123">This starts the **wordcount** class, which is contained in the **hadoop-mapreduce-examples.jar** file in the current directory.</span></span> <span data-ttu-id="24c54-124">Bemeneti, használja a **wasb://example/data/gutenberg/davinci.txt** dokumentum, és a kimeneti tárolódik: **wasb: / / / Példa/data/WordCountOutput**.</span><span class="sxs-lookup"><span data-stu-id="24c54-124">As input, it uses the **wasb://example/data/gutenberg/davinci.txt** document, and output is stored at: **wasb:///example/data/WordCountOutput**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="24c54-125">a MapReduce feladatot és a példaadatokat kapcsolatos további információkért lásd: <a href="hdinsight-use-mapreduce.md">MapReduce használata a HDInsight Hadoop a</a>.</span><span class="sxs-lookup"><span data-stu-id="24c54-125">for more information about this MapReduce job and the example data, see <a href="hdinsight-use-mapreduce.md">Use MapReduce in HDInsight Hadoop</a>.</span></span>
   >
   >
3. <span data-ttu-id="24c54-126">A feladat részletei bocsát ki, a feldolgozása, és olyan információkat ad vissza a következőhöz hasonló a feladatot:</span><span class="sxs-lookup"><span data-stu-id="24c54-126">The job emits details as it is processed, and it returns information similar to the following when the job is complete:</span></span>

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. <span data-ttu-id="24c54-127">Ha a folyamat befejeződik, az alábbi parancs segítségével a kimeneti fájl helye: **wasb://example/data/WordCountOutput**:</span><span class="sxs-lookup"><span data-stu-id="24c54-127">When the job is complete, use the following command to list the output files stored at **wasb://example/data/WordCountOutput**:</span></span>

        hadoop fs -ls wasb:///example/data/WordCountOutput

    <span data-ttu-id="24c54-128">Ez megjelenjen-e két fájlt **_SUCCESS** és **rész-r-00000**.</span><span class="sxs-lookup"><span data-stu-id="24c54-128">This should display two files, **_SUCCESS** and **part-r-00000**.</span></span> <span data-ttu-id="24c54-129">A **rész-r-00000** fájl tartalmazza a kimenet ehhez a feladathoz.</span><span class="sxs-lookup"><span data-stu-id="24c54-129">The **part-r-00000** file contains the output for this job.</span></span>

   > [!NOTE]
   > <span data-ttu-id="24c54-130">Bizonyos MapReduce-feladatok az eredmények lehet, hogy e osztani több **rész-r-###** fájlokat.</span><span class="sxs-lookup"><span data-stu-id="24c54-130">Some MapReduce jobs may split the results across multiple **part-r-#####** files.</span></span> <span data-ttu-id="24c54-131">Ha igen, használja a ### utótag fájlok sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="24c54-131">If so, use the ##### suffix to indicate the order of the files.</span></span>
   >
   >
5. <span data-ttu-id="24c54-132">A kimenet megtekintéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="24c54-132">To view the output, use the following command:</span></span>

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    <span data-ttu-id="24c54-133">Ez megjeleníti a szavakat, amelyek szerepelnek a **wasb://example/data/gutenberg/davinci.txt** fájl hányszor minden szó történt.</span><span class="sxs-lookup"><span data-stu-id="24c54-133">This displays a list of the words that are contained in the **wasb://example/data/gutenberg/davinci.txt** file, along with the number of times each word occured.</span></span> <span data-ttu-id="24c54-134">A következő egy példa a fájlban szereplő adatok:</span><span class="sxs-lookup"><span data-stu-id="24c54-134">The following is an example of the data that will be contained in the file:</span></span>

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <span data-ttu-id="24c54-135"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="24c54-135"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="24c54-136">Látható, a Hadoop parancs MapReduce-feladatok futtatása a HDInsight-fürtöt, és nézze meg a feladat kimenetére egyszerű módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="24c54-136">As you can see, the Hadoop command provides an easy way to run MapReduce jobs on an HDInsight cluster and then view the job output.</span></span>

## <span data-ttu-id="24c54-137"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="24c54-137"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="24c54-138">Általános információk a hdinsight MapReduce-feladatok:</span><span class="sxs-lookup"><span data-stu-id="24c54-138">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="24c54-139">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="24c54-139">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="24c54-140">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="24c54-140">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="24c54-141">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="24c54-141">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="24c54-142">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="24c54-142">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
