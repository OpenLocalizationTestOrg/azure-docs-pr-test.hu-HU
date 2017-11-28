---
title: "Hadoop Hive és a távoli asztal használata a HDInsight - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan csatlakozhat a HDInsight Hadoop-fürt távoli asztal használatával, és futtassa a Hive-lekérdezéseket a Hive parancssori felület használatával."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c228e35-d58a-4f22-917a-1d20c9da89b4
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 187c7cb413b3707e58eea387857375053d267189
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a><span data-ttu-id="ecf10-103">A Hive használata a HDInsight a távoli asztal hadooppal</span><span class="sxs-lookup"><span data-stu-id="ecf10-103">Use Hive with Hadoop on HDInsight with Remote Desktop</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="ecf10-104">Ebben a cikkben meg fog megtudhatja, hogyan távoli asztal használatával kapcsolódni HDInsight-fürtöt, és futtassa Hive-lekérdezéseket a Hive parancssori felület (CLI) használatával.</span><span class="sxs-lookup"><span data-stu-id="ecf10-104">In this article, you will learn how to connect to an HDInsight cluster by using Remote Desktop, and then run Hive queries by using the Hive Command-Line Interface (CLI).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ecf10-105">A távoli asztal a Windows operációs rendszert használó HDInsight-fürtök csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="ecf10-105">Remote Desktop is only available on HDInsight clusters that use Windows as the operating system.</span></span> <span data-ttu-id="ecf10-106">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="ecf10-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ecf10-107">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ecf10-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="ecf10-108">HDInsight 3.4-es vagy nagyobb, lásd: [használata a HDInsight és Beeline Hive](hdinsight-hadoop-use-hive-beeline.md) információk Hive-lekérdezéseket közvetlenül a fürtben futó parancsot a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="ecf10-108">For HDInsight 3.4 or greater, see [Use Hive with HDInsight and Beeline](hdinsight-hadoop-use-hive-beeline.md) for information on running Hive queries directly on the cluster from a command-line.</span></span>

## <span data-ttu-id="ecf10-109"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ecf10-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="ecf10-110">Ebben a cikkben szereplő lépések elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="ecf10-110">To complete the steps in this article, you will need the following:</span></span>

* <span data-ttu-id="ecf10-111">(A HDInsight Hadoop) a Windows-alapú HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="ecf10-111">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="ecf10-112">Windows 10, Windows 8 vagy Windows 7 rendszert futtató ügyfélszámítógép</span><span class="sxs-lookup"><span data-stu-id="ecf10-112">A client computer running Windows 10, Window 8, or Windows 7</span></span>

## <span data-ttu-id="ecf10-113"><a id="connect"></a>Csatlakozzon a távoli asztal</span><span class="sxs-lookup"><span data-stu-id="ecf10-113"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="ecf10-114">A távoli asztal engedélyezése a HDInsight-fürthöz, majd csatlakozni a következő utasításokat követve [csatlakozás RDP Funkciót használnak a HDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="ecf10-114">Enable Remote Desktop for the HDInsight cluster, then connect to it by following the instructions at [Connect to HDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="ecf10-115"><a id="hive"></a>A Hive paranccsal</span><span class="sxs-lookup"><span data-stu-id="ecf10-115"><a id="hive"></a>Use the Hive command</span></span>
<span data-ttu-id="ecf10-116">Amikor az asztalon a HDInsight-fürthöz csatlakozik, tegye a következőket Hive használható:</span><span class="sxs-lookup"><span data-stu-id="ecf10-116">When you have connected to the desktop for the HDInsight cluster, use the following steps to work with Hive:</span></span>

1. <span data-ttu-id="ecf10-117">A HDInsight asztalról indítsa el a **Hadoop parancssori**.</span><span class="sxs-lookup"><span data-stu-id="ecf10-117">From the HDInsight desktop, start the **Hadoop Command Line**.</span></span>
2. <span data-ttu-id="ecf10-118">Adja meg a következő parancsot a Hive CLI elindításához:</span><span class="sxs-lookup"><span data-stu-id="ecf10-118">Enter the following command to start the Hive CLI:</span></span>

        %hive_home%\bin\hive

    <span data-ttu-id="ecf10-119">Ha a parancssori felület elindult, jelennek meg a parancssori felület struktúra kérdés: `hive>`.</span><span class="sxs-lookup"><span data-stu-id="ecf10-119">When the CLI has started, you will see the Hive CLI prompt: `hive>`.</span></span>
3. <span data-ttu-id="ecf10-120">A parancssori felület használatával adja meg az alábbi állításokat annak nevű új tábla létrehozása **log4jLogs** mintaadatok használatával:</span><span class="sxs-lookup"><span data-stu-id="ecf10-120">Using the CLI, enter the following statements to create a new table named **log4jLogs** using sample data:</span></span>

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="ecf10-121">Ezekre az utasításokra hajtsa végre a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="ecf10-121">These statements perform the following actions:</span></span>

   * <span data-ttu-id="ecf10-122">**DROP TABLE**: törli a táblázat és az adatfájlban, ha a tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="ecf10-122">**DROP TABLE**: Deletes the table and the data file if the table already exists.</span></span>
   * <span data-ttu-id="ecf10-123">**A külső tábla létrehozása**: táblát hoz létre egy új "külső" struktúra.</span><span class="sxs-lookup"><span data-stu-id="ecf10-123">**CREATE EXTERNAL TABLE**: Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="ecf10-124">Külső táblák csak a táblázat megadása a Hive (az adatok marad az eredeti helyen) tárolja.</span><span class="sxs-lookup"><span data-stu-id="ecf10-124">External tables store only the table definition in Hive (the data is left in the original location).</span></span>

     > [!NOTE]
     > <span data-ttu-id="ecf10-125">Külső táblák kell használni, ha az alapul szolgáló adatokat frissítenie kell a külső forrásból (például egy automatizált adatok feltöltési folyamat) vagy egy másik MapReduce művelet által várt, de azt szeretné, hogy a legfrissebb adatok használata a Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="ecf10-125">External tables should be used when you expect the underlying data to be updated by an external source (such as an automated data upload process) or by another MapReduce operation, but you always want Hive queries to use the latest data.</span></span>
     >
     > <span data-ttu-id="ecf10-126">A külső tábla eldobása does **nem** törli az adatokat, csak a tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="ecf10-126">Dropping an external table does **not** delete the data, only the table definition.</span></span>
     >
     >
   * <span data-ttu-id="ecf10-127">**SOR formátum**: Hive közli az adatok formázását.</span><span class="sxs-lookup"><span data-stu-id="ecf10-127">**ROW FORMAT**: Tells Hive how the data is formatted.</span></span> <span data-ttu-id="ecf10-128">Ebben az esetben a mezőket az egyes naplókon szóközzel elválasztva.</span><span class="sxs-lookup"><span data-stu-id="ecf10-128">In this case, the fields in each log are separated by a space.</span></span>
   * <span data-ttu-id="ecf10-129">**AS TEXTFILE helyen tárolt**: (a példa/adatkönyvtár) Hive közli az adatokat tárolja, és szövegként tárolt.</span><span class="sxs-lookup"><span data-stu-id="ecf10-129">**STORED AS TEXTFILE LOCATION**: Tells Hive where the data is stored (the example/data directory) and that it is stored as text.</span></span>
   * <span data-ttu-id="ecf10-130">**Válassza ki**: választja ki az összes sorok számát ahol oszlop **t4** értéke **[hiba]**.</span><span class="sxs-lookup"><span data-stu-id="ecf10-130">**SELECT**: Selects a count of all rows where column **t4** contains the value **[ERROR]**.</span></span> <span data-ttu-id="ecf10-131">Ez visszaadja-e érték **3** mert három ezt az értéket tartalmazó sorok.</span><span class="sxs-lookup"><span data-stu-id="ecf10-131">This should return a value of **3** because there are three rows that contain this value.</span></span>
   * <span data-ttu-id="ecf10-132">**INPUT__FILE__NAME PÉLDÁUL "%.log"** -struktúra azt ismerteti, amely jelenleg csak a befejezési fájlok kell vissza adatokat. napló.</span><span class="sxs-lookup"><span data-stu-id="ecf10-132">**INPUT__FILE__NAME LIKE '%.log'** - Tells Hive that we should only return data from files ending in .log.</span></span> <span data-ttu-id="ecf10-133">Ez korlátozza a keresési a sample.log fájlt, amely tartalmazza az adatokat, és a nem ad vissza adatot más példa, amelyek nem felelnek meg a sémában meghatározott adatfájlok tartja azt.</span><span class="sxs-lookup"><span data-stu-id="ecf10-133">This restricts the search to the sample.log file that contains the data, and keeps it from returning data from other example data files that do not match the schema we defined.</span></span>
4. <span data-ttu-id="ecf10-134">A következő utasítások használatával hozzon létre egy új "belső" táblát nevű **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="ecf10-134">Use the following statements to create a new 'internal' table named **errorLogs**:</span></span>

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    <span data-ttu-id="ecf10-135">Ezekre az utasításokra hajtsa végre a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="ecf10-135">These statements perform the following actions:</span></span>

   * <span data-ttu-id="ecf10-136">**Hozzon létre Ha nem létezik táblázat**: egy táblát hoz létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="ecf10-136">**CREATE TABLE IF NOT EXISTS**: Creates a table if it does not already exist.</span></span> <span data-ttu-id="ecf10-137">Mivel a **külső** kulcsszó nem használható, ez egy belső tábla, amely a Hive-adatraktárban tárolja, és teljesen kezeli a Hive.</span><span class="sxs-lookup"><span data-stu-id="ecf10-137">Because the **EXTERNAL** keyword is not used, this is an internal table, which is stored in the Hive data warehouse and is managed completely by Hive.</span></span>

     > [!NOTE]
     > <span data-ttu-id="ecf10-138">Eltérően **külső** táblák, egy belső tábla eldobása is törli az alapul szolgáló adatokat.</span><span class="sxs-lookup"><span data-stu-id="ecf10-138">Unlike **EXTERNAL** tables, dropping an internal table also deletes the underlying data.</span></span>
     >
     >
   * <span data-ttu-id="ecf10-139">**TÁROLT AS ORC**: az adatok optimalizált sor oszlopos (ORC) formátumban tárolja.</span><span class="sxs-lookup"><span data-stu-id="ecf10-139">**STORED AS ORC**: Stores the data in optimized row columnar (ORC) format.</span></span> <span data-ttu-id="ecf10-140">Ez az egy magas optimalizált és hatékony formátumú Hive adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="ecf10-140">This is a highly optimized and efficient format for storing Hive data.</span></span>
   * <span data-ttu-id="ecf10-141">**ÍRJA FELÜL AZ INSERT... Válassza ki**: sorát kiválasztja a **log4jLogs** tartalmazó **[hiba]**, majd szúrja be az adatokat a **errorLogs** tábla.</span><span class="sxs-lookup"><span data-stu-id="ecf10-141">**INSERT OVERWRITE ... SELECT**: Selects rows from the **log4jLogs** table that contain **[ERROR]**, then inserts the data into the **errorLogs** table.</span></span>

     <span data-ttu-id="ecf10-142">Ellenőrizheti, hogy csak olyan sort tartalmazó **[hiba]** az oszlop t4 volt tároló a **errorLogs** table, használja a következő utasítás visszaadja az összes sort **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="ecf10-142">To verify that only rows that contain **[ERROR]** in column t4 were stored to the **errorLogs** table, use the following statement to return all the rows from **errorLogs**:</span></span>

       <span data-ttu-id="ecf10-143">Válassza ki * a errorLogs;</span><span class="sxs-lookup"><span data-stu-id="ecf10-143">SELECT * from errorLogs;</span></span>

     <span data-ttu-id="ecf10-144">Három sor adat vissza kell adni, az összes tartalmazó **[hiba]** az oszlop t4.</span><span class="sxs-lookup"><span data-stu-id="ecf10-144">Three rows of data should be returned, all containing **[ERROR]** in column t4.</span></span>

## <span data-ttu-id="ecf10-145"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="ecf10-145"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="ecf10-146">Ahogy látja, a a Hive-parancs segítségével egyszerűen interaktív futtathat Hive-lekérdezéseket a HDInsight-fürtöt, figyelheti a feladat állapotát és a kimeneti beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ecf10-146">As you can see, the the Hive command provides an easy way to interactively run Hive queries on an HDInsight cluster, monitor the job status, and retrieve the output.</span></span>

## <span data-ttu-id="ecf10-147"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ecf10-147"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="ecf10-148">Általános információk a hdinsight Hive:</span><span class="sxs-lookup"><span data-stu-id="ecf10-148">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="ecf10-149">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="ecf10-149">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="ecf10-150">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="ecf10-150">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="ecf10-151">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="ecf10-151">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="ecf10-152">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="ecf10-152">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="ecf10-153">Ha Tez Hive használ, tekintse meg a hibakeresési információk a következő dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="ecf10-153">If you are using Tez with Hive, see the following documents for debugging information:</span></span>

* [<span data-ttu-id="ecf10-154">A Tez felhasználói felület használata a Windows-alapú HDInsight-on</span><span class="sxs-lookup"><span data-stu-id="ecf10-154">Use the Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="ecf10-155">Az Ambari Tez nézetben a Linux-alapú HDInsight-on</span><span class="sxs-lookup"><span data-stu-id="ecf10-155">Use the Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
