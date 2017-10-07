---
title: "a távoli asztal a HDInsight - Azure Hadoop Pig aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Pig parancs toorun Pig Latin nyilatkozatai egy távoli asztali kapcsolat tooa Windows-alapú Hadoop-fürt hdinsightban."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e034a286-de0f-465f-8bf1-3d085ca6abed
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 2a4565fa827cd45fdbe6194b0486df93a6561084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-from-a-remote-desktop-connection"></a><span data-ttu-id="54494-103">A távoli asztali kapcsolat-ről futtatva a Pig-feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="54494-103">Run Pig jobs from a Remote Desktop connection</span></span>
[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="54494-104">Ez a dokumentum egy forgatókönyv biztosít hello Pig parancs toorun Pig Latin utasítások fürtről a távoli asztali kapcsolat tooa Windows-alapú HDInsight segítségével.</span><span class="sxs-lookup"><span data-stu-id="54494-104">This document provides a walkthrough for using hello Pig command toorun Pig Latin statements from a Remote Desktop connection tooa Windows-based HDInsight cluster.</span></span> <span data-ttu-id="54494-105">A Pig Latin lehetővé teszi azok adatátalakítást, toocreate MapReduce alkalmazások helyett képezi le, és csökkentheti a funkciók.</span><span class="sxs-lookup"><span data-stu-id="54494-105">Pig Latin allows you toocreate MapReduce applications by describing data transformations, rather than map and reduce functions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="54494-106">A távoli asztal a Windows hello operációs rendszert használó HDInsight-fürtök csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="54494-106">Remote Desktop is only available on HDInsight clusters that use Windows as hello operating system.</span></span> <span data-ttu-id="54494-107">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="54494-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="54494-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="54494-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="54494-109">HDInsight 3.4-es vagy nagyobb, lásd: [HDInsight és az SSH használata a Pig](hdinsight-hadoop-use-pig-ssh.md) információ interaktív Pig közvetlenül egy feladat hello fürt parancsot a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="54494-109">For HDInsight 3.4 or greater, see [Use Pig with HDInsight and SSH](hdinsight-hadoop-use-pig-ssh.md) for information on interactively running Pig jobs directly on hello cluster from a command-line.</span></span>

## <span data-ttu-id="54494-110"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="54494-110"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="54494-111">toocomplete hello cikkben leírt lépéseket, szüksége lesz a hello következő.</span><span class="sxs-lookup"><span data-stu-id="54494-111">toocomplete hello steps in this article, you will need hello following.</span></span>

* <span data-ttu-id="54494-112">(A HDInsight Hadoop) a Windows-alapú HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="54494-112">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="54494-113">Windows 10, Windows 8 vagy Windows 7 rendszert futtató ügyfélszámítógép</span><span class="sxs-lookup"><span data-stu-id="54494-113">A client computer running Windows 10, Windows 8, or Windows 7</span></span>

## <span data-ttu-id="54494-114"><a id="connect"></a>Csatlakozzon a távoli asztal</span><span class="sxs-lookup"><span data-stu-id="54494-114"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="54494-115">A távoli asztal engedélyezése a hello HDInsight-fürthöz, majd kösse a tooit: hello utasításokat követve [csatlakozás RDP tooHDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="54494-115">Enable Remote Desktop for hello HDInsight cluster, then connect tooit by following hello instructions at [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="54494-116"><a id="pig"></a>Hello Pig parancs használata</span><span class="sxs-lookup"><span data-stu-id="54494-116"><a id="pig"></a>Use hello Pig command</span></span>
1. <span data-ttu-id="54494-117">Miután egy távoli asztali kapcsolat, indítsa el a hello **Hadoop parancssori** hello ikonnal hello asztalon.</span><span class="sxs-lookup"><span data-stu-id="54494-117">After you have a Remote Desktop connection, start hello **Hadoop Command Line** by using hello icon on hello desktop.</span></span>
2. <span data-ttu-id="54494-118">A következő toostart hello Pig parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="54494-118">Use hello following toostart hello Pig command:</span></span>

        %pig_home%\bin\pig

    <span data-ttu-id="54494-119">Meg fog jelenni a `grunt>` kérdés.</span><span class="sxs-lookup"><span data-stu-id="54494-119">You will be presented with a `grunt>` prompt.</span></span>
3. <span data-ttu-id="54494-120">Adja meg a következő utasítás hello:</span><span class="sxs-lookup"><span data-stu-id="54494-120">Enter hello following statement:</span></span>

        LOGS = LOAD 'wasb:///example/data/sample.log';

    <span data-ttu-id="54494-121">Ez a parancs hello hello sample.log fájl tartalmát betölti a hello naplók fájlt.</span><span class="sxs-lookup"><span data-stu-id="54494-121">This command loads hello contents of hello sample.log file into hello LOGS file.</span></span> <span data-ttu-id="54494-122">Hello fájl tartalmának hello hello a következő parancs használatával tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="54494-122">You can view hello contents of hello file by using hello following command:</span></span>

        DUMP LOGS;
4. <span data-ttu-id="54494-123">Hello adatok átalakítása minden rekordból egy reguláris kifejezést tooextract csak hello naplózási szintek alkalmazásával:</span><span class="sxs-lookup"><span data-stu-id="54494-123">Transform hello data by applying a regular expression tooextract only hello logging level from each record:</span></span>

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    <span data-ttu-id="54494-124">Használhat **DUMP** tooview hello adatok hello átalakítás után.</span><span class="sxs-lookup"><span data-stu-id="54494-124">You can use **DUMP** tooview hello data after hello transformation.</span></span> <span data-ttu-id="54494-125">Ebben az esetben `DUMP LEVELS;`.</span><span class="sxs-lookup"><span data-stu-id="54494-125">In this case, `DUMP LEVELS;`.</span></span>
5. <span data-ttu-id="54494-126">További, átalakítások alkalmazása a következő utasítások hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="54494-126">Continue applying transformations by using hello following statements.</span></span> <span data-ttu-id="54494-127">Használjon `DUMP` tooview hello eredménye hello átalakítása lépések után.</span><span class="sxs-lookup"><span data-stu-id="54494-127">Use `DUMP` tooview hello result of hello transformation after each step.</span></span>

    <table>
    <tr>
    <th><span data-ttu-id="54494-128">Utasítás</span><span class="sxs-lookup"><span data-stu-id="54494-128">Statement</span></span></th><th><span data-ttu-id="54494-129">Funkció</span><span class="sxs-lookup"><span data-stu-id="54494-129">What it does</span></span></th>
    </tr>
    <tr>
    <td><span data-ttu-id="54494-130">FILTEREDLEVELS = által LOGLEVEL SZINTJEINEK értéke nem null;</span><span class="sxs-lookup"><span data-stu-id="54494-130">FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;</span></span></td><td><span data-ttu-id="54494-131">Hello naplózási szint null értéket tartalmazó sorok eltávolítása, és a FILTEREDLEVELS hello eredmények tárolja.</span><span class="sxs-lookup"><span data-stu-id="54494-131">Removes rows that contain a null value for hello log level and stores hello results into FILTEREDLEVELS.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="54494-132">GROUPEDLEVELS = csoport FILTEREDLEVELS által LOGLEVEL;</span><span class="sxs-lookup"><span data-stu-id="54494-132">GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;</span></span></td><td><span data-ttu-id="54494-133">Csoportok hello sort a naplózási szint szerint, és tárolja a hello eredmények GROUPEDLEVELS be.</span><span class="sxs-lookup"><span data-stu-id="54494-133">Groups hello rows by log level and stores hello results into GROUPEDLEVELS.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="54494-134">GYAKORISÁGOT = foreach GROUPEDLEVELS csoport létrehozás módja LOGLEVEL, COUNT (FILTEREDLEVELS. LOGLEVEL), COUNT;</span><span class="sxs-lookup"><span data-stu-id="54494-134">FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;</span></span></td><td><span data-ttu-id="54494-135">Egy új készletet hoz létre, amely tartalmazza az egyes egyedi napló adatok értéke szinten, és hány alkalommal következik be.</span><span class="sxs-lookup"><span data-stu-id="54494-135">Creates a new set of data that contains each unique log level value and how many times it occurs.</span></span> <span data-ttu-id="54494-136">A GYAKORISÁGOT történő tárolás</span><span class="sxs-lookup"><span data-stu-id="54494-136">This is stored into FREQUENCIES</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="54494-137">EREDMÉNY = rendelés GYAKORISÁGOT által száma desc;</span><span class="sxs-lookup"><span data-stu-id="54494-137">RESULT = order FREQUENCIES by COUNT desc;</span></span></td><td><span data-ttu-id="54494-138">Az eredmény hello naplózási szintek száma (csökkenő), és tárolja rendeléseket</span><span class="sxs-lookup"><span data-stu-id="54494-138">Orders hello log levels by count (descending,) and stores into RESULT</span></span></td>
    </tr>
    </table><span data-ttu-id="54494-139">
6.Az átalakítás hello eredménye hello segítségével is mentheti `STORE` utasítást.</span><span class="sxs-lookup"><span data-stu-id="54494-139">
6. You can also save hello results of a transformation by using hello `STORE` statement.</span></span> <span data-ttu-id="54494-140">Például a következő parancs hello menti hello `RESULT` toohello **/example/data/pigout** könyvtárban lévő hello alapértelmezett tároló a fürt:</span><span class="sxs-lookup"><span data-stu-id="54494-140">For example, hello following command saves hello `RESULT` toohello **/example/data/pigout** directory in hello default storage container for your cluster:</span></span>

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]
   > <span data-ttu-id="54494-141">hello adatok hello megadott könyvtár nevű fájlban tárolódnak **rész-nnnnn**.</span><span class="sxs-lookup"><span data-stu-id="54494-141">hello data is stored in hello specified directory in files named **part-nnnnn**.</span></span> <span data-ttu-id="54494-142">Ha hello könyvtár már létezik, kapni fog egy hibaüzenetet.</span><span class="sxs-lookup"><span data-stu-id="54494-142">If hello directory already exists, you will receive an error message.</span></span>
   >
   >
7. <span data-ttu-id="54494-143">tooexit hello grunt parancssorból, írja be a következő utasítás hello.</span><span class="sxs-lookup"><span data-stu-id="54494-143">tooexit hello grunt prompt, enter hello following statement.</span></span>

        QUIT;

### <a name="pig-latin-batch-files"></a><span data-ttu-id="54494-144">A Pig Latin parancsfájlokat</span><span class="sxs-lookup"><span data-stu-id="54494-144">Pig Latin batch files</span></span>
<span data-ttu-id="54494-145">Hello Pig parancs toorun Pig Latin található egy fájlban is használható.</span><span class="sxs-lookup"><span data-stu-id="54494-145">You can also use hello Pig command toorun Pig Latin that is contained in a file.</span></span>

1. <span data-ttu-id="54494-146">Hello grunt kérdés kilépéskor nyissa meg a **Jegyzettömb** , és hozzon létre egy új fájlt **pigbatch.pig** a hello **PIG_HOME %** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="54494-146">After exiting hello grunt prompt, open **Notepad** and create a new file named **pigbatch.pig** in hello **%PIG_HOME%** directory.</span></span>
2. <span data-ttu-id="54494-147">Típus vagy Beillesztés hello következő sorok be hello **pigbatch.pig** fájlt, és mentse azt:</span><span class="sxs-lookup"><span data-stu-id="54494-147">Type or paste hello following lines into hello **pigbatch.pig** file, and then save it:</span></span>

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. <span data-ttu-id="54494-148">A következő toorun hello használata hello **pigbatch.pig** fájl hello pig parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="54494-148">Use hello following toorun hello **pigbatch.pig** file using hello pig command.</span></span>

        pig %PIG_HOME%\pigbatch.pig

    <span data-ttu-id="54494-149">Hello kötegelt befejezését követően megjelenik kimenete, amely kell kell hello ugyanaz, mint ha használja a következő hello `DUMP RESULT;` hello előző lépésekben:</span><span class="sxs-lookup"><span data-stu-id="54494-149">When hello batch job completes, you should see hello following output, which should be hello same as when you used `DUMP RESULT;` in hello previous steps:</span></span>

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <span data-ttu-id="54494-150"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="54494-150"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="54494-151">Ahogy látja, a Pig parancs hello lehetővé teszi toointeractively MapReduce műveletek futtatása, vagy egy parancsfájlba tárolt Pig Latin feladatok futtatása.</span><span class="sxs-lookup"><span data-stu-id="54494-151">As you can see, hello Pig command allows you toointeractively run MapReduce operations, or run Pig Latin jobs that are stored in a batch file.</span></span>

## <span data-ttu-id="54494-152"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="54494-152"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="54494-153">Általános információk a hdinsight Pig:</span><span class="sxs-lookup"><span data-stu-id="54494-153">For general information about Pig in HDInsight:</span></span>

* [<span data-ttu-id="54494-154">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="54494-154">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="54494-155">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="54494-155">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="54494-156">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="54494-156">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="54494-157">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="54494-157">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
