---
title: "aaaUse Hadoop Hive és a távoli asztal a HDInsight - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconnect tooHadoop fürt a Hdinsightban távoli asztal használatával, és futtassa a Hive-lekérdezések hello Hive parancssori felület használatával."
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
ms.openlocfilehash: f86ffc1be33a8b0b2346d1a1388e5dfa6d0f8777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a><span data-ttu-id="31999-103">A Hive használata a HDInsight a távoli asztal hadooppal</span><span class="sxs-lookup"><span data-stu-id="31999-103">Use Hive with Hadoop on HDInsight with Remote Desktop</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="31999-104">Ebből a cikkből megtudhatja, hogyan tooconnect tooan HDInsight fürt távoli asztal használatával, és futtassa a Hive lekérdezi hello Hive parancssori felület (CLI) használatával.</span><span class="sxs-lookup"><span data-stu-id="31999-104">In this article, you will learn how tooconnect tooan HDInsight cluster by using Remote Desktop, and then run Hive queries by using hello Hive Command-Line Interface (CLI).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="31999-105">A távoli asztal a Windows hello operációs rendszert használó HDInsight-fürtök csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="31999-105">Remote Desktop is only available on HDInsight clusters that use Windows as hello operating system.</span></span> <span data-ttu-id="31999-106">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="31999-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="31999-107">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="31999-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="31999-108">HDInsight 3.4-es vagy nagyobb, lásd: [használata a HDInsight és Beeline Hive](hdinsight-hadoop-use-hive-beeline.md) információk Hive-lekérdezéseket közvetlenül hello fürtben futó parancsot a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="31999-108">For HDInsight 3.4 or greater, see [Use Hive with HDInsight and Beeline](hdinsight-hadoop-use-hive-beeline.md) for information on running Hive queries directly on hello cluster from a command-line.</span></span>

## <span data-ttu-id="31999-109"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="31999-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="31999-110">toocomplete hello cikkben leírt lépéseket, szüksége lesz a következő hello:</span><span class="sxs-lookup"><span data-stu-id="31999-110">toocomplete hello steps in this article, you will need hello following:</span></span>

* <span data-ttu-id="31999-111">(A HDInsight Hadoop) a Windows-alapú HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="31999-111">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="31999-112">Windows 10, Windows 8 vagy Windows 7 rendszert futtató ügyfélszámítógép</span><span class="sxs-lookup"><span data-stu-id="31999-112">A client computer running Windows 10, Window 8, or Windows 7</span></span>

## <span data-ttu-id="31999-113"><a id="connect"></a>Csatlakozzon a távoli asztal</span><span class="sxs-lookup"><span data-stu-id="31999-113"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="31999-114">A távoli asztal engedélyezése a hello HDInsight-fürthöz, majd kösse a tooit: hello utasításokat követve [csatlakozás RDP tooHDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="31999-114">Enable Remote Desktop for hello HDInsight cluster, then connect tooit by following hello instructions at [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="31999-115"><a id="hive"></a>Hello Hive parancs használata</span><span class="sxs-lookup"><span data-stu-id="31999-115"><a id="hive"></a>Use hello Hive command</span></span>
<span data-ttu-id="31999-116">Hello HDInsight-fürthöz toohello asztali csatlakozáskor használja a következő lépéseket toowork a Hive hello:</span><span class="sxs-lookup"><span data-stu-id="31999-116">When you have connected toohello desktop for hello HDInsight cluster, use hello following steps toowork with Hive:</span></span>

1. <span data-ttu-id="31999-117">Hello HDInsight asztalról start hello **Hadoop parancssori**.</span><span class="sxs-lookup"><span data-stu-id="31999-117">From hello HDInsight desktop, start hello **Hadoop Command Line**.</span></span>
2. <span data-ttu-id="31999-118">Adja meg a következő parancs toostart hello Hive CLI hello:</span><span class="sxs-lookup"><span data-stu-id="31999-118">Enter hello following command toostart hello Hive CLI:</span></span>

        %hive_home%\bin\hive

    <span data-ttu-id="31999-119">Hello CLI indításakor megjelenik hello CLI struktúra kérdés: `hive>`.</span><span class="sxs-lookup"><span data-stu-id="31999-119">When hello CLI has started, you will see hello Hive CLI prompt: `hive>`.</span></span>
3. <span data-ttu-id="31999-120">Hello CLI, adja meg a következő utasítások toocreate nevű új tábla hello **log4jLogs** mintaadatok használatával:</span><span class="sxs-lookup"><span data-stu-id="31999-120">Using hello CLI, enter hello following statements toocreate a new table named **log4jLogs** using sample data:</span></span>

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="31999-121">Ezekre az utasításokra hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="31999-121">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="31999-122">**DROP TABLE**: hello tábla és hello adatfájl törlése, ha hello tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="31999-122">**DROP TABLE**: Deletes hello table and hello data file if hello table already exists.</span></span>
   * <span data-ttu-id="31999-123">**A külső tábla létrehozása**: táblát hoz létre egy új "külső" struktúra.</span><span class="sxs-lookup"><span data-stu-id="31999-123">**CREATE EXTERNAL TABLE**: Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="31999-124">Külső táblák csak hello tábladefiníció struktúra (hello adatok marad az eredeti helyükre hello) tárolja.</span><span class="sxs-lookup"><span data-stu-id="31999-124">External tables store only hello table definition in Hive (hello data is left in hello original location).</span></span>

     > [!NOTE]
     > <span data-ttu-id="31999-125">Külső táblák kell használni, amikor hello alapjául szolgáló adatok toobe frissíteni az külső forrásból (például egy automatizált adatok feltöltési folyamat) vagy egy másik MapReduce művelet által várt, de azt szeretné, hogy Hive toouse hello legfrissebb adatokat lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="31999-125">External tables should be used when you expect hello underlying data toobe updated by an external source (such as an automated data upload process) or by another MapReduce operation, but you always want Hive queries toouse hello latest data.</span></span>
     >
     > <span data-ttu-id="31999-126">A külső tábla eldobása does **nem** törölni hello adatokat, csak a hello tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="31999-126">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>
     >
     >
   * <span data-ttu-id="31999-127">**SOR formátum**: közli Hive hello adatok formázását.</span><span class="sxs-lookup"><span data-stu-id="31999-127">**ROW FORMAT**: Tells Hive how hello data is formatted.</span></span> <span data-ttu-id="31999-128">Ebben az esetben az egyes naplókon hello mezők szóközzel elválasztva.</span><span class="sxs-lookup"><span data-stu-id="31999-128">In this case, hello fields in each log are separated by a space.</span></span>
   * <span data-ttu-id="31999-129">**AS TEXTFILE helyen tárolt**: közli Hive hello adatok esetén tárolt (hello példaadatokat/könyvtár), és szövegként tárolt.</span><span class="sxs-lookup"><span data-stu-id="31999-129">**STORED AS TEXTFILE LOCATION**: Tells Hive where hello data is stored (hello example/data directory) and that it is stored as text.</span></span>
   * <span data-ttu-id="31999-130">**Válassza ki**: választja ki az összes sorok számát ahol oszlop **t4** hello értéket tartalmaz **[hiba]**.</span><span class="sxs-lookup"><span data-stu-id="31999-130">**SELECT**: Selects a count of all rows where column **t4** contains hello value **[ERROR]**.</span></span> <span data-ttu-id="31999-131">Ez visszaadja-e érték **3** mert három ezt az értéket tartalmazó sorok.</span><span class="sxs-lookup"><span data-stu-id="31999-131">This should return a value of **3** because there are three rows that contain this value.</span></span>
   * <span data-ttu-id="31999-132">**INPUT__FILE__NAME PÉLDÁUL "%.log"** -struktúra azt ismerteti, amely jelenleg csak a befejezési fájlok kell vissza adatokat. napló.</span><span class="sxs-lookup"><span data-stu-id="31999-132">**INPUT__FILE__NAME LIKE '%.log'** - Tells Hive that we should only return data from files ending in .log.</span></span> <span data-ttu-id="31999-133">Ez korlátozza a hello keresési toohello sample.log fájlt, amely hello adatokat tartalmaz, és a nem ad vissza adatot más példa, amely nem egyezik a meghatározott hello séma adatfájlok tartja azt.</span><span class="sxs-lookup"><span data-stu-id="31999-133">This restricts hello search toohello sample.log file that contains hello data, and keeps it from returning data from other example data files that do not match hello schema we defined.</span></span>
4. <span data-ttu-id="31999-134">A következő utasítások toocreate nevű új "belső" tábla használata hello **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="31999-134">Use hello following statements toocreate a new 'internal' table named **errorLogs**:</span></span>

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    <span data-ttu-id="31999-135">Ezekre az utasításokra hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="31999-135">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="31999-136">**Hozzon létre Ha nem létezik táblázat**: egy táblát hoz létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="31999-136">**CREATE TABLE IF NOT EXISTS**: Creates a table if it does not already exist.</span></span> <span data-ttu-id="31999-137">Mivel hello **külső** kulcsszó nem használható, ez egy belső tábla, amely hello Hive-adatraktárban tárolja, és Hive teljesen kezeli.</span><span class="sxs-lookup"><span data-stu-id="31999-137">Because hello **EXTERNAL** keyword is not used, this is an internal table, which is stored in hello Hive data warehouse and is managed completely by Hive.</span></span>

     > [!NOTE]
     > <span data-ttu-id="31999-138">Eltérően **külső** táblák, egy belső tábla is eldobása hello alapjául szolgáló adatok törlése.</span><span class="sxs-lookup"><span data-stu-id="31999-138">Unlike **EXTERNAL** tables, dropping an internal table also deletes hello underlying data.</span></span>
     >
     >
   * <span data-ttu-id="31999-139">**TÁROLT AS ORC**: hello eltárolja optimalizált sor oszlopos (ORC) formátumban.</span><span class="sxs-lookup"><span data-stu-id="31999-139">**STORED AS ORC**: Stores hello data in optimized row columnar (ORC) format.</span></span> <span data-ttu-id="31999-140">Ez az egy magas optimalizált és hatékony formátumú Hive adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="31999-140">This is a highly optimized and efficient format for storing Hive data.</span></span>
   * <span data-ttu-id="31999-141">**ÍRJA FELÜL AZ INSERT... Válassza ki**: hello sorok kiválaszt **log4jLogs** tartalmazó tábla **[hiba]**, majd Beszúrások adatok hello be hello **errorLogs** tábla.</span><span class="sxs-lookup"><span data-stu-id="31999-141">**INSERT OVERWRITE ... SELECT**: Selects rows from hello **log4jLogs** table that contain **[ERROR]**, then inserts hello data into hello **errorLogs** table.</span></span>

     <span data-ttu-id="31999-142">tartalmaz, amely csak a sorok, amely tooverify **[hiba]** oszlop t4 voltak tárolt toohello **errorLogs** table, használja a következő utasítás tooreturn összes sorát hello hello **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="31999-142">tooverify that only rows that contain **[ERROR]** in column t4 were stored toohello **errorLogs** table, use hello following statement tooreturn all hello rows from **errorLogs**:</span></span>

       <span data-ttu-id="31999-143">Válassza ki * a errorLogs;</span><span class="sxs-lookup"><span data-stu-id="31999-143">SELECT * from errorLogs;</span></span>

     <span data-ttu-id="31999-144">Három sor adat vissza kell adni, az összes tartalmazó **[hiba]** az oszlop t4.</span><span class="sxs-lookup"><span data-stu-id="31999-144">Three rows of data should be returned, all containing **[ERROR]** in column t4.</span></span>

## <span data-ttu-id="31999-145"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="31999-145"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="31999-146">Ahogy látja, hello hello Hive parancs biztosít egy egyszerűen toointeractively futtathat Hive-lekérdezéseket a HDInsight-fürtöt, a figyelő hello feladat állapota, hello kimeneti beolvasása.</span><span class="sxs-lookup"><span data-stu-id="31999-146">As you can see, hello hello Hive command provides an easy way toointeractively run Hive queries on an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

## <span data-ttu-id="31999-147"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="31999-147"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="31999-148">Általános információk a hdinsight Hive:</span><span class="sxs-lookup"><span data-stu-id="31999-148">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="31999-149">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="31999-149">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="31999-150">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="31999-150">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="31999-151">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="31999-151">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="31999-152">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="31999-152">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="31999-153">Ha a Hive Tez használ, tekintse meg a hello dokumentumok hibakeresési információkat a következő:</span><span class="sxs-lookup"><span data-stu-id="31999-153">If you are using Tez with Hive, see hello following documents for debugging information:</span></span>

* [<span data-ttu-id="31999-154">A Windows-alapú HDInsight hello Tez felhasználói felület használata</span><span class="sxs-lookup"><span data-stu-id="31999-154">Use hello Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="31999-155">A Linux-alapú HDInsight Ambari Tez nézet hello használata</span><span class="sxs-lookup"><span data-stu-id="31999-155">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

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
