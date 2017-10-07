---
title: "aaaMapReduce és a távoli asztal hadooppal a Hdinsightban - Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse távoli asztal tooconnect tooHadoop HDInsight és futtatási MapReduce-feladatok."
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
ms.openlocfilehash: bdbbcf59ca86dd1b170471aa9e12334a04c23667
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a><span data-ttu-id="a0b19-103">A távoli asztal hdinsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="a0b19-103">Use MapReduce in Hadoop on HDInsight with Remote Desktop</span></span>
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="a0b19-104">Ebből a cikkből megtudhatja, hogyan tooconnect tooa a HDInsight Hadoop cluster távoli asztal használatával, és futtassa a MapReduce-feladatok hello Hadoop parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="a0b19-104">In this article, you will learn how tooconnect tooa Hadoop on HDInsight cluster by using Remote Desktop and then run MapReduce jobs by using hello Hadoop command.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a0b19-105">Távoli asztali Windows-alapú HDInsight-fürtök csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="a0b19-105">Remote Desktop is only available on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="a0b19-106">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="a0b19-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a0b19-107">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="a0b19-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="a0b19-108">HDInsight 3.4-es vagy nagyobb, lásd: [az SSH használata MapReduce](hdinsight-hadoop-use-mapreduce-ssh.md) toohello HDInsight-fürthöz kapcsolódó, és a MapReduce-feladatok futtatása.</span><span class="sxs-lookup"><span data-stu-id="a0b19-108">For HDInsight 3.4 or greater, see [Use MapReduce with SSH](hdinsight-hadoop-use-mapreduce-ssh.md) for information on connecting toohello HDInsight cluster and running MapReduce jobs.</span></span>

## <span data-ttu-id="a0b19-109"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a0b19-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="a0b19-110">toocomplete hello cikkben leírt lépéseket, szüksége lesz a következő hello:</span><span class="sxs-lookup"><span data-stu-id="a0b19-110">toocomplete hello steps in this article, you will need hello following:</span></span>

* <span data-ttu-id="a0b19-111">(A HDInsight Hadoop) a Windows-alapú HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="a0b19-111">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="a0b19-112">Windows 10, Windows 8 vagy Windows 7 rendszert futtató ügyfélszámítógép</span><span class="sxs-lookup"><span data-stu-id="a0b19-112">A client computer running Windows 10, Windows 8, or Windows 7</span></span>

## <span data-ttu-id="a0b19-113"><a id="connect"></a>Csatlakozzon a távoli asztal</span><span class="sxs-lookup"><span data-stu-id="a0b19-113"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="a0b19-114">A távoli asztal engedélyezése a hello HDInsight-fürthöz, majd kösse a tooit: hello utasításokat követve [csatlakozás RDP tooHDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="a0b19-114">Enable Remote Desktop for hello HDInsight cluster, then connect tooit by following hello instructions at [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="a0b19-115"><a id="hadoop"></a>Hello Hadoop parancs használata</span><span class="sxs-lookup"><span data-stu-id="a0b19-115"><a id="hadoop"></a>Use hello Hadoop command</span></span>
<span data-ttu-id="a0b19-116">Ha csatlakoztatott toohello asztali hello HDInsight-fürthöz, használja a hello lépéseket toorun MapReduce feladatot hello Hadoop parancs használatával a következő:</span><span class="sxs-lookup"><span data-stu-id="a0b19-116">When you are connected toohello desktop for hello HDInsight cluster, use hello following steps toorun a MapReduce job by using hello Hadoop command:</span></span>

1. <span data-ttu-id="a0b19-117">Hello HDInsight asztalról start hello **Hadoop parancssori**.</span><span class="sxs-lookup"><span data-stu-id="a0b19-117">From hello HDInsight desktop, start hello **Hadoop Command Line**.</span></span> <span data-ttu-id="a0b19-118">Ekkor megnyílik egy új parancssort a hello **c:\apps\dist\hadoop-&lt;verziószám >** directory.</span><span class="sxs-lookup"><span data-stu-id="a0b19-118">This opens a new command prompt in hello **c:\apps\dist\hadoop-&lt;version number>** directory.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a0b19-119">hello verziószám változik Hadoop frissül.</span><span class="sxs-lookup"><span data-stu-id="a0b19-119">hello version number changes as Hadoop is updated.</span></span> <span data-ttu-id="a0b19-120">Hello **HADOOP_HOME** környezeti változó használt toofind hello elérési út is lehet.</span><span class="sxs-lookup"><span data-stu-id="a0b19-120">hello **HADOOP_HOME** environment variable can be used toofind hello path.</span></span> <span data-ttu-id="a0b19-121">Például `cd %HADOOP_HOME%` módosítások könyvtárak toohello Hadoop könyvtár, anélkül, hogy Ön tooknow hello verziószáma.</span><span class="sxs-lookup"><span data-stu-id="a0b19-121">For example, `cd %HADOOP_HOME%` changes directories toohello Hadoop directory, without requiring you tooknow hello version number.</span></span>
   >
   >
2. <span data-ttu-id="a0b19-122">toouse hello **Hadoop** parancs egy példa MapReduce feladatot toorun, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="a0b19-122">toouse hello **Hadoop** command toorun an example MapReduce job, use hello following command:</span></span>

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    <span data-ttu-id="a0b19-123">Ekkor elindul a hello **wordcount** osztályt, amelyet hello **hadoop-mapreduce-examples.jar** hello aktuális könyvtárban található fájl.</span><span class="sxs-lookup"><span data-stu-id="a0b19-123">This starts hello **wordcount** class, which is contained in hello **hadoop-mapreduce-examples.jar** file in hello current directory.</span></span> <span data-ttu-id="a0b19-124">Bemeneti, használja a hello **wasb://example/data/gutenberg/davinci.txt** dokumentum, és a kimeneti tárolódik: **wasb: / / / Példa/data/WordCountOutput**.</span><span class="sxs-lookup"><span data-stu-id="a0b19-124">As input, it uses hello **wasb://example/data/gutenberg/davinci.txt** document, and output is stored at: **wasb:///example/data/WordCountOutput**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a0b19-125">a MapReduce feladatot és hello példa adatokkal kapcsolatos további információkért lásd: <a href="hdinsight-use-mapreduce.md">MapReduce használata a HDInsight Hadoop a</a>.</span><span class="sxs-lookup"><span data-stu-id="a0b19-125">for more information about this MapReduce job and hello example data, see <a href="hdinsight-use-mapreduce.md">Use MapReduce in HDInsight Hadoop</a>.</span></span>
   >
   >
3. <span data-ttu-id="a0b19-126">hello feladat részletei bocsát ki, a feldolgozása, valamint hello feladatot ad vissza adatokat hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="a0b19-126">hello job emits details as it is processed, and it returns information similar toohello following when hello job is complete:</span></span>

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. <span data-ttu-id="a0b19-127">Ha hello folyamat befejeződik, használja a következő parancs toolist hello kimeneti fájl helye: hello **wasb://example/data/WordCountOutput**:</span><span class="sxs-lookup"><span data-stu-id="a0b19-127">When hello job is complete, use hello following command toolist hello output files stored at **wasb://example/data/WordCountOutput**:</span></span>

        hadoop fs -ls wasb:///example/data/WordCountOutput

    <span data-ttu-id="a0b19-128">Ez megjelenjen-e két fájlt **_SUCCESS** és **rész-r-00000**.</span><span class="sxs-lookup"><span data-stu-id="a0b19-128">This should display two files, **_SUCCESS** and **part-r-00000**.</span></span> <span data-ttu-id="a0b19-129">Hello **rész-r-00000** fájl tartalmazza a feladat hello kimenet.</span><span class="sxs-lookup"><span data-stu-id="a0b19-129">hello **part-r-00000** file contains hello output for this job.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a0b19-130">Bizonyos MapReduce-feladatok hello eredmények lehet, hogy e osztani több **rész-r-###** fájlokat.</span><span class="sxs-lookup"><span data-stu-id="a0b19-130">Some MapReduce jobs may split hello results across multiple **part-r-#####** files.</span></span> <span data-ttu-id="a0b19-131">Ha igen, használjon hello ### utótag hello fájlok tooindicate hello sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="a0b19-131">If so, use hello ##### suffix tooindicate hello order of hello files.</span></span>
   >
   >
5. <span data-ttu-id="a0b19-132">tooview hello kimeneti, használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="a0b19-132">tooview hello output, use hello following command:</span></span>

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    <span data-ttu-id="a0b19-133">Ez hello szereplő hello szavak listáját jeleníti meg **wasb://example/data/gutenberg/davinci.txt** fájl hello száma minden szó történt.</span><span class="sxs-lookup"><span data-stu-id="a0b19-133">This displays a list of hello words that are contained in hello **wasb://example/data/gutenberg/davinci.txt** file, along with hello number of times each word occured.</span></span> <span data-ttu-id="a0b19-134">hello következő az hello fájlban szereplő adatok hello:</span><span class="sxs-lookup"><span data-stu-id="a0b19-134">hello following is an example of hello data that will be contained in hello file:</span></span>

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <span data-ttu-id="a0b19-135"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="a0b19-135"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="a0b19-136">Ahogy látja, hello Hadoop parancs Ez egy egyszerű módot toorun MapReduce-feladatok egy HDInsight-fürtöt, és a nézet hello feladatkiemenetét.</span><span class="sxs-lookup"><span data-stu-id="a0b19-136">As you can see, hello Hadoop command provides an easy way toorun MapReduce jobs on an HDInsight cluster and then view hello job output.</span></span>

## <span data-ttu-id="a0b19-137"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0b19-137"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="a0b19-138">Általános információk a hdinsight MapReduce-feladatok:</span><span class="sxs-lookup"><span data-stu-id="a0b19-138">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="a0b19-139">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="a0b19-139">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="a0b19-140">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="a0b19-140">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="a0b19-141">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="a0b19-141">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="a0b19-142">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="a0b19-142">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
