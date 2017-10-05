---
title: "Hadoop a Pig használata a HDInsight - Azure távoli asztal |} Microsoft Docs"
description: "Ismerje meg, a Pig parancs használata a távoli asztali kapcsolat egy Windows-alapú Hadoop-fürt hdinsightban történő Pig Latin utasítás futtatását."
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
ms.openlocfilehash: 5e8d4fbd8afc54c8bbc1a9a71c66d7022a7d5986
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="run-pig-jobs-from-a-remote-desktop-connection"></a><span data-ttu-id="5a3b6-103">A távoli asztali kapcsolat-ről futtatva a Pig-feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="5a3b6-103">Run Pig jobs from a Remote Desktop connection</span></span>
[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="5a3b6-104">Ez a dokumentum egy forgatókönyv biztosít a Pig paranccsal futtassa a Pig Latin utasításokat egy távoli asztali kapcsolaton keresztül egy Windows-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-104">This document provides a walkthrough for using the Pig command to run Pig Latin statements from a Remote Desktop connection to a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="5a3b6-105">A Pig Latin adatátalakítást, azok MapReduce-alkalmazások létrehozása helyett hozzárendelését és funkciók teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-105">Pig Latin allows you to create MapReduce applications by describing data transformations, rather than map and reduce functions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5a3b6-106">A távoli asztal a Windows operációs rendszert használó HDInsight-fürtök csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-106">Remote Desktop is only available on HDInsight clusters that use Windows as the operating system.</span></span> <span data-ttu-id="5a3b6-107">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="5a3b6-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="5a3b6-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="5a3b6-109">HDInsight 3.4-es vagy nagyobb, lásd: [HDInsight és az SSH használata a Pig](hdinsight-hadoop-use-pig-ssh.md) információk interaktív Pig-feladatokhoz közvetlenül a fürtön futó parancsot a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-109">For HDInsight 3.4 or greater, see [Use Pig with HDInsight and SSH](hdinsight-hadoop-use-pig-ssh.md) for information on interactively running Pig jobs directly on the cluster from a command-line.</span></span>

## <span data-ttu-id="5a3b6-110"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5a3b6-110"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="5a3b6-111">Ebben a cikkben szereplő lépések elvégzéséhez a következőkre lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-111">To complete the steps in this article, you will need the following.</span></span>

* <span data-ttu-id="5a3b6-112">(A HDInsight Hadoop) a Windows-alapú HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="5a3b6-112">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="5a3b6-113">Windows 10, Windows 8 vagy Windows 7 rendszert futtató ügyfélszámítógép</span><span class="sxs-lookup"><span data-stu-id="5a3b6-113">A client computer running Windows 10, Windows 8, or Windows 7</span></span>

## <span data-ttu-id="5a3b6-114"><a id="connect"></a>Csatlakozzon a távoli asztal</span><span class="sxs-lookup"><span data-stu-id="5a3b6-114"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="5a3b6-115">A távoli asztal engedélyezése a HDInsight-fürthöz, majd csatlakozni a következő utasításokat követve [csatlakozás RDP Funkciót használnak a HDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="5a3b6-115">Enable Remote Desktop for the HDInsight cluster, then connect to it by following the instructions at [Connect to HDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="5a3b6-116"><a id="pig"></a>A Pig paranccsal</span><span class="sxs-lookup"><span data-stu-id="5a3b6-116"><a id="pig"></a>Use the Pig command</span></span>
1. <span data-ttu-id="5a3b6-117">A távoli asztali kapcsolattal rendelkezik, után indítsa el a **Hadoop parancssori** ikonnal az asztalon.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-117">After you have a Remote Desktop connection, start the **Hadoop Command Line** by using the icon on the desktop.</span></span>
2. <span data-ttu-id="5a3b6-118">A Pig parancs indítását. használja a következőket:</span><span class="sxs-lookup"><span data-stu-id="5a3b6-118">Use the following to start the Pig command:</span></span>

        %pig_home%\bin\pig

    <span data-ttu-id="5a3b6-119">Meg fog jelenni a `grunt>` kérdés.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-119">You will be presented with a `grunt>` prompt.</span></span>
3. <span data-ttu-id="5a3b6-120">Adja meg a következő utasítást:</span><span class="sxs-lookup"><span data-stu-id="5a3b6-120">Enter the following statement:</span></span>

        LOGS = LOAD 'wasb:///example/data/sample.log';

    <span data-ttu-id="5a3b6-121">Ez a parancs a sample.log fájl tartalmát betölti a naplók fájlba.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-121">This command loads the contents of the sample.log file into the LOGS file.</span></span> <span data-ttu-id="5a3b6-122">A fájl tartalmát a következő parancs használatával tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="5a3b6-122">You can view the contents of the file by using the following command:</span></span>

        DUMP LOGS;
4. <span data-ttu-id="5a3b6-123">Az adatok átalakítása úgy, hogy csak a naplózási szint kibontani rekordokban reguláris kifejezést alkalmazza:</span><span class="sxs-lookup"><span data-stu-id="5a3b6-123">Transform the data by applying a regular expression to extract only the logging level from each record:</span></span>

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    <span data-ttu-id="5a3b6-124">Használhat **DUMP** az átalakítás után az adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-124">You can use **DUMP** to view the data after the transformation.</span></span> <span data-ttu-id="5a3b6-125">Ebben az esetben `DUMP LEVELS;`.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-125">In this case, `DUMP LEVELS;`.</span></span>
5. <span data-ttu-id="5a3b6-126">További, átalakítások alkalmazása a következő utasítások használatával.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-126">Continue applying transformations by using the following statements.</span></span> <span data-ttu-id="5a3b6-127">Használjon `DUMP` az átalakítás lépések után az eredmények megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-127">Use `DUMP` to view the result of the transformation after each step.</span></span>

    <table>
    <tr>
    <th><span data-ttu-id="5a3b6-128">Utasítás</span><span class="sxs-lookup"><span data-stu-id="5a3b6-128">Statement</span></span></th><th><span data-ttu-id="5a3b6-129">Funkció</span><span class="sxs-lookup"><span data-stu-id="5a3b6-129">What it does</span></span></th>
    </tr>
    <tr>
    <td><span data-ttu-id="5a3b6-130">FILTEREDLEVELS = által LOGLEVEL SZINTJEINEK értéke nem null;</span><span class="sxs-lookup"><span data-stu-id="5a3b6-130">FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;</span></span></td><td><span data-ttu-id="5a3b6-131">A naplózási szint megadásához null értéket tartalmazó sorok eltávolítása és az eredményt FILTEREDLEVELS be.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-131">Removes rows that contain a null value for the log level and stores the results into FILTEREDLEVELS.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="5a3b6-132">GROUPEDLEVELS = csoport FILTEREDLEVELS által LOGLEVEL;</span><span class="sxs-lookup"><span data-stu-id="5a3b6-132">GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;</span></span></td><td><span data-ttu-id="5a3b6-133">A sorok naplózási szint szerint csoportosítja, és tárolja az eredményeket a GROUPEDLEVELS.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-133">Groups the rows by log level and stores the results into GROUPEDLEVELS.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="5a3b6-134">GYAKORISÁGOT = foreach GROUPEDLEVELS csoport létrehozás módja LOGLEVEL, COUNT (FILTEREDLEVELS. LOGLEVEL), COUNT;</span><span class="sxs-lookup"><span data-stu-id="5a3b6-134">FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;</span></span></td><td><span data-ttu-id="5a3b6-135">Egy új készletet hoz létre, amely tartalmazza az egyes egyedi napló adatok értéke szinten, és hány alkalommal következik be.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-135">Creates a new set of data that contains each unique log level value and how many times it occurs.</span></span> <span data-ttu-id="5a3b6-136">A GYAKORISÁGOT történő tárolás</span><span class="sxs-lookup"><span data-stu-id="5a3b6-136">This is stored into FREQUENCIES</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="5a3b6-137">EREDMÉNY = rendelés GYAKORISÁGOT által száma desc;</span><span class="sxs-lookup"><span data-stu-id="5a3b6-137">RESULT = order FREQUENCIES by COUNT desc;</span></span></td><td><span data-ttu-id="5a3b6-138">A naplózási szintek rendelések (csökkenő) száma szerint, és tárolja az eredmény</span><span class="sxs-lookup"><span data-stu-id="5a3b6-138">Orders the log levels by count (descending,) and stores into RESULT</span></span></td>
    </tr>
    </table><span data-ttu-id="5a3b6-139">
6.Az átalakítás eredménye használatával is mentheti a `STORE` utasítást.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-139">
6. You can also save the results of a transformation by using the `STORE` statement.</span></span> <span data-ttu-id="5a3b6-140">Például a következő parancsot a menti a `RESULT` számára a **/example/data/pigout** könyvtárban lévő az alapértelmezett tároló, a fürt:</span><span class="sxs-lookup"><span data-stu-id="5a3b6-140">For example, the following command saves the `RESULT` to the **/example/data/pigout** directory in the default storage container for your cluster:</span></span>

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]
   > <span data-ttu-id="5a3b6-141">Az adatok tárolási fájlban található a megadott könyvtár **rész-nnnnn**.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-141">The data is stored in the specified directory in files named **part-nnnnn**.</span></span> <span data-ttu-id="5a3b6-142">Ha a könyvtár már létezik, kapni fog egy hibaüzenetet.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-142">If the directory already exists, you will receive an error message.</span></span>
   >
   >
7. <span data-ttu-id="5a3b6-143">Lépjen ki a grunt parancssorába, írja be a következő utasítást.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-143">To exit the grunt prompt, enter the following statement.</span></span>

        QUIT;

### <a name="pig-latin-batch-files"></a><span data-ttu-id="5a3b6-144">A Pig Latin parancsfájlokat</span><span class="sxs-lookup"><span data-stu-id="5a3b6-144">Pig Latin batch files</span></span>
<span data-ttu-id="5a3b6-145">A Pig-parancs segítségével futtassa a Pig Latin, amely egy fájlban található.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-145">You can also use the Pig command to run Pig Latin that is contained in a file.</span></span>

1. <span data-ttu-id="5a3b6-146">Nyissa meg a grunt parancssorába kilépéskor **Jegyzettömb** , és hozzon létre egy új fájlt **pigbatch.pig** a a **PIG_HOME %** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-146">After exiting the grunt prompt, open **Notepad** and create a new file named **pigbatch.pig** in the **%PIG_HOME%** directory.</span></span>
2. <span data-ttu-id="5a3b6-147">Írja vagy illessze be a következő sorokat a **pigbatch.pig** fájlt, és mentse azt:</span><span class="sxs-lookup"><span data-stu-id="5a3b6-147">Type or paste the following lines into the **pigbatch.pig** file, and then save it:</span></span>

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. <span data-ttu-id="5a3b6-148">Használja a következő futtatásához a **pigbatch.pig** fájl, a pig-paranccsal.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-148">Use the following to run the **pigbatch.pig** file using the pig command.</span></span>

        pig %PIG_HOME%\pigbatch.pig

    <span data-ttu-id="5a3b6-149">A kötegelt befejezését követően megjelenik a következő kimenetet, és legyen ugyanaz, mint amikor használt `DUMP RESULT;` az előző lépésekben:</span><span class="sxs-lookup"><span data-stu-id="5a3b6-149">When the batch job completes, you should see the following output, which should be the same as when you used `DUMP RESULT;` in the previous steps:</span></span>

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <span data-ttu-id="5a3b6-150"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="5a3b6-150"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="5a3b6-151">Ahogy látja, a Pig-parancs segítségével interaktív módon futtassa MapReduce műveleteket, vagy egy kötegfájlt tárolt Pig Latin feladatok futtatása.</span><span class="sxs-lookup"><span data-stu-id="5a3b6-151">As you can see, the Pig command allows you to interactively run MapReduce operations, or run Pig Latin jobs that are stored in a batch file.</span></span>

## <span data-ttu-id="5a3b6-152"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a3b6-152"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="5a3b6-153">Általános információk a hdinsight Pig:</span><span class="sxs-lookup"><span data-stu-id="5a3b6-153">For general information about Pig in HDInsight:</span></span>

* [<span data-ttu-id="5a3b6-154">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="5a3b6-154">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="5a3b6-155">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="5a3b6-155">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="5a3b6-156">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="5a3b6-156">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="5a3b6-157">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="5a3b6-157">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
