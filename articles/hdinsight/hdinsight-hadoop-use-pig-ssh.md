---
title: "a HDInsight-fürtök - Azure Hadoop Pig with SSH aaaUse |} Microsoft Docs"
description: "Megtudhatja, hogyan csatlakozzon SSH tooa Linux-alapú Hadoop-fürtöt, és majd használata hello Pig parancs toorun Pig Latin utasítások interaktív módon, vagy egy kötegelt feladat."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b646a93b-4c51-4ba4-84da-3275d9124ebe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 1da303e239b537e6b331b1d33010058582718c90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-hello-pig-command-ssh"></a><span data-ttu-id="64e98-103">Futtassa a Pig-feladatokhoz hello Pig parancs (SSH) rendelkező Linux-alapú fürtön</span><span class="sxs-lookup"><span data-stu-id="64e98-103">Run Pig jobs on a Linux-based cluster with hello Pig command (SSH)</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="64e98-104">Ismerje meg, hogyan toointeractively futtassa egy SSH-kapcsolat tooyour HDInsight-fürt a Pig-feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="64e98-104">Learn how toointeractively run Pig jobs from an SSH connection tooyour HDInsight cluster.</span></span> <span data-ttu-id="64e98-105">hello Pig Latin programozási nyelv, amelyeket alkalmazott toohello bemeneti adatok tooproduce szükséges hello toodescribe átalakítások lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="64e98-105">hello Pig Latin programming language allows you toodescribe transformations that are applied toohello input data tooproduce hello desired output.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="64e98-106">hello jelen dokumentumban leírt lépések szükséges egy Linux-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="64e98-106">hello steps in this document require a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="64e98-107">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="64e98-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="64e98-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="64e98-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="64e98-109"><a id="ssh"></a>Csatlakozzon SSH</span><span class="sxs-lookup"><span data-stu-id="64e98-109"><a id="ssh"></a>Connect with SSH</span></span>

<span data-ttu-id="64e98-110">SSH tooconnect tooyour HDInsight-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="64e98-110">Use SSH tooconnect tooyour HDInsight cluster.</span></span> <span data-ttu-id="64e98-111">hello alábbi példa csatlakozik nevű tooa fürt **myhdinsight** nevű hello fiókként **sshuser**:</span><span class="sxs-lookup"><span data-stu-id="64e98-111">hello following example connects tooa cluster named **myhdinsight** as hello account named **sshuser**:</span></span>

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net

<span data-ttu-id="64e98-112">**Ha megadott egy SSH-hitelesítési tanúsítvány kulcs** hello HDInsight-fürt létrehozása után, szükség lehet hello titkos kulcs toospecify hello helyét az ügyfélrendszeren.</span><span class="sxs-lookup"><span data-stu-id="64e98-112">**If you provided a certificate key for SSH authentication** when you created hello HDInsight cluster, you may need toospecify hello location of hello private key on your client system.</span></span>

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

<span data-ttu-id="64e98-113">**Ha a megadott jelszó SSH hitelesítés** hello HDInsight-fürt létrehozásakor adja meg a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="64e98-113">**If you provided a password for SSH authentication** when you created hello HDInsight cluster, provide hello password when prompted.</span></span>

<span data-ttu-id="64e98-114">Az SSH és a HDInsight együttes használatával további információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="64e98-114">For more information on using SSH with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="64e98-115"><a id="pig"></a>Hello Pig parancs használata</span><span class="sxs-lookup"><span data-stu-id="64e98-115"><a id="pig"></a>Use hello Pig command</span></span>

1. <span data-ttu-id="64e98-116">A csatlakozás után a következő parancs hello használatával indítsa el az hello Pig parancssori felület (CLI):</span><span class="sxs-lookup"><span data-stu-id="64e98-116">Once connected, start hello Pig command-line interface (CLI) by using hello following command:</span></span>

        pig

    <span data-ttu-id="64e98-117">Néhány percet, megtekintheti a `grunt>` kérdés.</span><span class="sxs-lookup"><span data-stu-id="64e98-117">After a moment, you should see a `grunt>` prompt.</span></span>

2. <span data-ttu-id="64e98-118">Adja meg a következő utasítás hello:</span><span class="sxs-lookup"><span data-stu-id="64e98-118">Enter hello following statement:</span></span>

        LOGS = LOAD '/example/data/sample.log';

    <span data-ttu-id="64e98-119">Ez a parancs hello hello sample.log fájl tartalmát betölti a NAPLÓKAT.</span><span class="sxs-lookup"><span data-stu-id="64e98-119">This command loads hello contents of hello sample.log file into LOGS.</span></span> <span data-ttu-id="64e98-120">Hello hello fájl tartalmát a következő utasítás hello segítségével tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="64e98-120">You can view hello contents of hello file by using hello following statement:</span></span>

        DUMP LOGS;

3. <span data-ttu-id="64e98-121">A következő átalakító hello adatokat úgy, hogy egy reguláris kifejezést tooextract csak hello naplózási szint alkalmazza minden rekordból hello utasítás a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="64e98-121">Next, transform hello data by applying a regular expression tooextract only hello logging level from each record by using hello following statement:</span></span>

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    <span data-ttu-id="64e98-122">Használhat **DUMP** tooview hello adatok hello átalakítás után.</span><span class="sxs-lookup"><span data-stu-id="64e98-122">You can use **DUMP** tooview hello data after hello transformation.</span></span> <span data-ttu-id="64e98-123">Ebben az esetben használjon `DUMP LEVELS;`.</span><span class="sxs-lookup"><span data-stu-id="64e98-123">In this case, use `DUMP LEVELS;`.</span></span>

4. <span data-ttu-id="64e98-124">További, átalakítások alkalmazása a következő táblázat hello hello utasítások segítségével:</span><span class="sxs-lookup"><span data-stu-id="64e98-124">Continue applying transformations by using hello statements in hello following table:</span></span>

    | <span data-ttu-id="64e98-125">A Pig Latin utasítás</span><span class="sxs-lookup"><span data-stu-id="64e98-125">Pig Latin statement</span></span> | <span data-ttu-id="64e98-126">Milyen hello utasítás does</span><span class="sxs-lookup"><span data-stu-id="64e98-126">What hello statement does</span></span> |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | <span data-ttu-id="64e98-127">Hello naplózási szint null értéket tartalmazó sorok eltávolítása, és tárolja a hello eredmények `FILTEREDLEVELS`.</span><span class="sxs-lookup"><span data-stu-id="64e98-127">Removes rows that contain a null value for hello log level and stores hello results into `FILTEREDLEVELS`.</span></span> |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | <span data-ttu-id="64e98-128">Csoportok hello sort a naplózási szint szerint, és tárolja a hello eredmények `GROUPEDLEVELS`.</span><span class="sxs-lookup"><span data-stu-id="64e98-128">Groups hello rows by log level and stores hello results into `GROUPEDLEVELS`.</span></span> |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | <span data-ttu-id="64e98-129">Hoz létre, amely tartalmazza az egyes egyedi napló adatok értéke szinten, és hány alkalommal következik be.</span><span class="sxs-lookup"><span data-stu-id="64e98-129">Creates a set of data that contains each unique log level value and how many times it occurs.</span></span> <span data-ttu-id="64e98-130">hello adatkészlet tárolja azokat `FREQUENCIES`.</span><span class="sxs-lookup"><span data-stu-id="64e98-130">hello data set is stored into `FREQUENCIES`.</span></span> |
    | `RESULT = order FREQUENCIES by COUNT desc;` | <span data-ttu-id="64e98-131">Rendelések hello naplózási szintek száma (csökkenő) szerint, és tárolja a `RESULT`.</span><span class="sxs-lookup"><span data-stu-id="64e98-131">Orders hello log levels by count (descending) and stores into `RESULT`.</span></span> |

    > [!TIP]
    > <span data-ttu-id="64e98-132">Használjon `DUMP` tooview hello eredménye hello átalakítása lépések után.</span><span class="sxs-lookup"><span data-stu-id="64e98-132">Use `DUMP` tooview hello result of hello transformation after each step.</span></span>

5. <span data-ttu-id="64e98-133">Az átalakítás hello eredménye hello segítségével is mentheti `STORE` utasítást.</span><span class="sxs-lookup"><span data-stu-id="64e98-133">You can also save hello results of a transformation by using hello `STORE` statement.</span></span> <span data-ttu-id="64e98-134">Például a következő utasítás hello menti hello `RESULT` toohello `/example/data/pigout` a fürt hello alapértelmezett tároló könyvtár:</span><span class="sxs-lookup"><span data-stu-id="64e98-134">For example, hello following statement saves hello `RESULT` toohello `/example/data/pigout` directory on hello default storage for your cluster:</span></span>

        STORE RESULT into '/example/data/pigout';

   > [!NOTE]
   > <span data-ttu-id="64e98-135">hello adatok hello megadott könyvtár nevű fájlban tárolódnak `part-nnnnn`.</span><span class="sxs-lookup"><span data-stu-id="64e98-135">hello data is stored in hello specified directory in files named `part-nnnnn`.</span></span> <span data-ttu-id="64e98-136">Ha hello könyvtár már létezik, akkor egy hibaüzenet.</span><span class="sxs-lookup"><span data-stu-id="64e98-136">If hello directory already exists, you receive an error.</span></span>

6. <span data-ttu-id="64e98-137">tooexit hello grunt parancssorból, írja be a következő utasítás hello:</span><span class="sxs-lookup"><span data-stu-id="64e98-137">tooexit hello grunt prompt, enter hello following statement:</span></span>

        QUIT;

### <a name="pig-latin-batch-files"></a><span data-ttu-id="64e98-138">A Pig Latin parancsfájlokat</span><span class="sxs-lookup"><span data-stu-id="64e98-138">Pig Latin batch files</span></span>

<span data-ttu-id="64e98-139">Hello Pig parancs toorun fájlban található Pig Latin is használható.</span><span class="sxs-lookup"><span data-stu-id="64e98-139">You can also use hello Pig command toorun Pig Latin contained in a file.</span></span>

1. <span data-ttu-id="64e98-140">Hello grunt kérdés kilépéskor használata hello következő parancsot a toopipe STDIN fájlba `pigbatch.pig`.</span><span class="sxs-lookup"><span data-stu-id="64e98-140">After exiting hello grunt prompt, use hello following command toopipe STDIN into a file named `pigbatch.pig`.</span></span> <span data-ttu-id="64e98-141">A fájl hello kezdőkönyvtárban hello SSH felhasználói fiók jön létre.</span><span class="sxs-lookup"><span data-stu-id="64e98-141">This file is created in hello home directory for hello SSH user account.</span></span>

        cat > ~/pigbatch.pig

2. <span data-ttu-id="64e98-142">Írja be vagy illessze be az alábbi hello, és használja a Ctrl + D, amikor befejeződött az.</span><span class="sxs-lookup"><span data-stu-id="64e98-142">Type or paste hello following lines, and then use Ctrl+D when finished.</span></span>

        LOGS = LOAD '/example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

3. <span data-ttu-id="64e98-143">Használjon hello következő parancsot a toorun hello `pigbatch.pig` fájl hello Pig parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="64e98-143">Use hello following command toorun hello `pigbatch.pig` file by using hello Pig command.</span></span>

        pig ~/pigbatch.pig

    <span data-ttu-id="64e98-144">Miután hello kötegelt feladat befejeződik, a következő kimeneti hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="64e98-144">Once hello batch job finishes, you see hello following output:</span></span>

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <span data-ttu-id="64e98-145"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="64e98-145"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="64e98-146">Általános információk a hdinsight a Pig tekintse meg a következő dokumentum hello:</span><span class="sxs-lookup"><span data-stu-id="64e98-146">For general information on Pig in HDInsight, see hello following document:</span></span>

* [<span data-ttu-id="64e98-147">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="64e98-147">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="64e98-148">A HDInsight hadooppal a más módon toowork további információkért lásd: a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="64e98-148">For more information on other ways toowork with Hadoop on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="64e98-149">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="64e98-149">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="64e98-150">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="64e98-150">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
