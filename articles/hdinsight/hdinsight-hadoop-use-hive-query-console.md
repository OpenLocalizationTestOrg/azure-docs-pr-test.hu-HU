---
title: "a hdinsight - Azure lekérdezés konzol hello Hadoop Hive aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello webalapú konzol lekérdezés toorun Hive-lekérdezések egy HDInsight hadoop cluster a böngészőből."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5ae074b0-f55e-472d-94a7-005b0e79f779
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 621882082c9a07655d34b8dc980b8e47dd04b745
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hello-query-console"></a><span data-ttu-id="c471e-103">Hello lekérdezés konzol használatával Hive-lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="c471e-103">Run Hive queries using hello Query Console</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="c471e-104">Ebből a cikkből megtudhatja, hogyan toouse hello HDInsight lekérdezés konzol toorun Hive-lekérdezések egy HDInsight hadoop cluster a böngészőből.</span><span class="sxs-lookup"><span data-stu-id="c471e-104">In this article, you will learn how toouse hello HDInsight Query Console toorun Hive queries on an HDInsight Hadoop cluster from your browser.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c471e-105">hello HDInsight lekérdezés konzol a Windows-alapú HDInsight-fürtök csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="c471e-105">hello HDInsight Query Console is only available on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="c471e-106">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="c471e-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="c471e-107">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="c471e-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="c471e-108">HDInsight 3.4-es vagy nagyobb, lásd: [lekérdezések futtatása Hive Ambari Hive nézetben](hdinsight-hadoop-use-hive-ambari-view.md) információ Hive-lekérdezések egy webböngészőből.</span><span class="sxs-lookup"><span data-stu-id="c471e-108">For HDInsight 3.4 or greater, see [Run Hive queries in Ambari Hive View](hdinsight-hadoop-use-hive-ambari-view.md) for information on running Hive queries from a web browser.</span></span>

## <span data-ttu-id="c471e-109"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c471e-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="c471e-110">toocomplete hello cikkben leírt lépéseket, szüksége lesz a hello következő.</span><span class="sxs-lookup"><span data-stu-id="c471e-110">toocomplete hello steps in this article, you will need hello following.</span></span>

* <span data-ttu-id="c471e-111">A Windows-alapú HDInsight Hadoop-fürt</span><span class="sxs-lookup"><span data-stu-id="c471e-111">A Windows-based HDInsight Hadoop cluster</span></span>
* <span data-ttu-id="c471e-112">Modern webböngésző</span><span class="sxs-lookup"><span data-stu-id="c471e-112">A modern web browser</span></span>

## <span data-ttu-id="c471e-113"><a id="run"></a>Hello lekérdezés konzol használatával Hive-lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="c471e-113"><a id="run"></a> Run Hive queries using hello Query Console</span></span>
1. <span data-ttu-id="c471e-114">Nyisson meg egy webböngészőt, és keresse meg a túl**https://CLUSTERNAME.azurehdinsight.net**, ahol **CLUSTERNAME** hello neve, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="c471e-114">Open a web browser and navigate too**https://CLUSTERNAME.azurehdinsight.net**, where **CLUSTERNAME** is hello name of your HDInsight cluster.</span></span> <span data-ttu-id="c471e-115">Ha a rendszer kéri, adja meg a hello felhasználónevet és a hello fürt létrehozásakor használt jelszót.</span><span class="sxs-lookup"><span data-stu-id="c471e-115">If prompted, enter hello user name and password that you used when you created hello cluster.</span></span>
2. <span data-ttu-id="c471e-116">Hello hivatkozások hello oldal hello tetején, válassza ki **Hive szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="c471e-116">From hello links at hello top of hello page, select **Hive Editor**.</span></span> <span data-ttu-id="c471e-117">Lehet, hogy szeretné-e a HDInsight-fürt hello toorun használt tooenter hello HiveQL utasítások űrlap megjelenik.</span><span class="sxs-lookup"><span data-stu-id="c471e-117">This displays a form that can be used tooenter hello HiveQL statements that you want toorun in hello HDInsight cluster.</span></span>

    ![hello hive szerkesztő](./media/hdinsight-hadoop-use-hive-query-console/queryconsole.png)

    <span data-ttu-id="c471e-119">Cserélje le a hello szöveg `Select * from hivesampletable` a következő HiveQL utasítások hello:</span><span class="sxs-lookup"><span data-stu-id="c471e-119">Replace hello text `Select * from hivesampletable` with hello following HiveQL statements:</span></span>

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="c471e-120">Ezekre az utasításokra hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="c471e-120">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="c471e-121">**DROP TABLE**: hello tábla és hello adatfájl törlése, ha hello tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="c471e-121">**DROP TABLE**: Deletes hello table and hello data file if hello table already exists.</span></span>
   * <span data-ttu-id="c471e-122">**A külső tábla létrehozása**: táblát hoz létre egy új "külső" struktúra.</span><span class="sxs-lookup"><span data-stu-id="c471e-122">**CREATE EXTERNAL TABLE**: Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="c471e-123">Külső táblák csak hello tábladefiníció tárolása Hive; hello adatok hello eredeti helyen marad.</span><span class="sxs-lookup"><span data-stu-id="c471e-123">External tables store only hello table definition in Hive; hello data is left in hello original location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="c471e-124">Külső táblák kell használni, amikor hello alapjául szolgáló adatok toobe frissíteni az külső forrásból (például egy automatizált adatok feltöltési folyamat) vagy egy másik MapReduce művelet által várt, de azt szeretné, hogy Hive toouse hello legfrissebb adatokat lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="c471e-124">External tables should be used when you expect hello underlying data toobe updated by an external source (such as an automated data upload process) or by another MapReduce operation, but you always want Hive queries toouse hello latest data.</span></span>
     >
     > <span data-ttu-id="c471e-125">A külső tábla eldobása does **nem** törölni hello adatokat, csak a hello tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="c471e-125">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>
     >
     >
   * <span data-ttu-id="c471e-126">**SOR formátum**: közli Hive hello adatok formázását.</span><span class="sxs-lookup"><span data-stu-id="c471e-126">**ROW FORMAT**: Tells Hive how hello data is formatted.</span></span> <span data-ttu-id="c471e-127">Ebben az esetben az egyes naplókon hello mezők szóközzel elválasztva.</span><span class="sxs-lookup"><span data-stu-id="c471e-127">In this case, hello fields in each log are separated by a space.</span></span>
   * <span data-ttu-id="c471e-128">**AS TEXTFILE helyen tárolt**: közli Hive hello adatok esetén tárolt (hello példaadatokat/könyvtár), és szövegként tárolt</span><span class="sxs-lookup"><span data-stu-id="c471e-128">**STORED AS TEXTFILE LOCATION**: Tells Hive where hello data is stored (hello example/data directory) and that it is stored as text</span></span>
   * <span data-ttu-id="c471e-129">**Válassza ki**: válassza ki az összes sorok számát ahol oszlop **t4** tartalmaz hello értéket **[hiba]**.</span><span class="sxs-lookup"><span data-stu-id="c471e-129">**SELECT**: Select a count of all rows where column **t4** contain hello value **[ERROR]**.</span></span> <span data-ttu-id="c471e-130">Ez visszaadja-e érték **3** mert három ezt az értéket tartalmazó sorok.</span><span class="sxs-lookup"><span data-stu-id="c471e-130">This should return a value of **3** because there are three rows that contain this value.</span></span>
   * <span data-ttu-id="c471e-131">**INPUT__FILE__NAME PÉLDÁUL "%.log"** -struktúra azt ismerteti, amely jelenleg csak a befejezési fájlok kell vissza adatokat. napló.</span><span class="sxs-lookup"><span data-stu-id="c471e-131">**INPUT__FILE__NAME LIKE '%.log'** - Tells Hive that we should only return data from files ending in .log.</span></span> <span data-ttu-id="c471e-132">Ez korlátozza a hello keresési toohello sample.log fájlt, amely hello adatokat tartalmaz, és a nem ad vissza adatot más példa, amely nem egyezik a meghatározott hello séma adatfájlok tartja azt.</span><span class="sxs-lookup"><span data-stu-id="c471e-132">This restricts hello search toohello sample.log file that contains hello data, and keeps it from returning data from other example data files that do not match hello schema we defined.</span></span>
3. <span data-ttu-id="c471e-133">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="c471e-133">Click **Submit**.</span></span> <span data-ttu-id="c471e-134">Hello **feladat munkamenet** hello: hello lap alján megjelenjen-e meg hello feladat részleteit.</span><span class="sxs-lookup"><span data-stu-id="c471e-134">hello **Job Session** at hello bottom of hello page should display details for hello job.</span></span>
4. <span data-ttu-id="c471e-135">Ha hello **állapot** mező túl megváltozik**befejezve**, jelölje be **részleteinek megtekintése** hello feladat.</span><span class="sxs-lookup"><span data-stu-id="c471e-135">When hello **Status** field changes too**Completed**, select **View Details** for hello job.</span></span> <span data-ttu-id="c471e-136">Hello Részletek lapján hello **Job Output** tartalmaz `[ERROR]    3`.</span><span class="sxs-lookup"><span data-stu-id="c471e-136">On hello details page, hello **Job Output** contains `[ERROR]    3`.</span></span> <span data-ttu-id="c471e-137">Használhatja a hello **letöltése** hello feladat eredményének hello tartalmazó fájl alapján ez a mező toodownload gombra.</span><span class="sxs-lookup"><span data-stu-id="c471e-137">You can use hello **Download** button under this field toodownload a file that contains hello output of hello job.</span></span>

## <span data-ttu-id="c471e-138"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="c471e-138"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="c471e-139">Ahogy látja, lekérdezés konzol segítségével egy egyszerű módot toorun hello Hive lekérdezi a HDInsight-fürtöt, hello feladat állapotának figyelése és hello kimeneti beolvasása.</span><span class="sxs-lookup"><span data-stu-id="c471e-139">As you can see, hello Query Console provides an easy way toorun Hive queries in an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

<span data-ttu-id="c471e-140">Hive lekérdezés konzol toorun Hive-feladatok használatáról további toolearn válasszon **bevezetés** tetején hello hello lekérdezés konzolt, majd használja az hello minták által biztosított.</span><span class="sxs-lookup"><span data-stu-id="c471e-140">toolearn more about using Hive Query Console toorun Hive jobs, select **Getting Started** at hello top of hello Query Console, then use hello samples that are provided.</span></span> <span data-ttu-id="c471e-141">Minden minta bemutatja, hogyan hello folyamatot, amely Hive tooanalyze adatok, beleértve a magyarázatot hello HiveQL utasításokról hello mintában használt használatával.</span><span class="sxs-lookup"><span data-stu-id="c471e-141">Each sample walks through hello process of using Hive tooanalyze data, including explanations about hello HiveQL statements used in hello sample.</span></span>

## <span data-ttu-id="c471e-142"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c471e-142"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="c471e-143">Általános információk a hdinsight Hive:</span><span class="sxs-lookup"><span data-stu-id="c471e-143">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="c471e-144">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="c471e-144">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="c471e-145">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="c471e-145">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="c471e-146">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="c471e-146">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="c471e-147">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="c471e-147">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="c471e-148">Ha a Hive Tez használ, tekintse meg a hello dokumentumok hibakeresési információkat a következő:</span><span class="sxs-lookup"><span data-stu-id="c471e-148">If you are using Tez with Hive, see hello following documents for debugging information:</span></span>

* [<span data-ttu-id="c471e-149">A Windows-alapú HDInsight hello Tez felhasználói felület használata</span><span class="sxs-lookup"><span data-stu-id="c471e-149">Use hello Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="c471e-150">A Linux-alapú HDInsight Ambari Tez nézet hello használata</span><span class="sxs-lookup"><span data-stu-id="c471e-150">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
