---
title: "Ambari Tez nézet használata a HDInsight - Azure |} Microsoft Docs"
description: "Útmutató az Ambari Tez nézet használata a HDInsight-on Tez feladatokhoz."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 9c39ea56-670b-4699-aba0-0f64c261e411
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 65d89309b9eea8544b85d16687baa90d49688d77
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-ambari-views-to-debug-tez-jobs-on-hdinsight"></a><span data-ttu-id="b18a7-103">Az Ambari nézetek használata a HDInsight-on Tez feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="b18a7-103">Use Ambari Views to debug Tez Jobs on HDInsight</span></span>

<span data-ttu-id="b18a7-104">Az Ambari webes felhasználói felületén, a HDInsight a Tez nézetet tartalmaz, amely megértéséhez, valamint a Tez használó feladatok debug használható.</span><span class="sxs-lookup"><span data-stu-id="b18a7-104">The Ambari Web UI for HDInsight contains a Tez view that can be used to understand and debug jobs that use Tez.</span></span> <span data-ttu-id="b18a7-105">A Tez nézet lehetővé teszi a feladathoz egy grafikonon csatlakoztatott elemek megjelenítése, egyes elemek elemezze és statisztika és naplózási információk lekérdezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b18a7-105">The Tez view allows you to visualize the job as a graph of connected items, drill into each item, and retrieve statistics and logging information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b18a7-106">A jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="b18a7-106">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="b18a7-107">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="b18a7-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="b18a7-108">További információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="b18a7-108">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b18a7-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b18a7-109">Prerequisites</span></span>

* <span data-ttu-id="b18a7-110">A Linux-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="b18a7-110">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="b18a7-111">A fürt létrehozásának lépései: [Ismerkedés a Linux-alapú HDInsight eszközzel](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b18a7-111">For steps on creating a cluster, see [Get started using Linux-based HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
* <span data-ttu-id="b18a7-112">Egy HTML5-támogatással rendelkező modern webböngésző.</span><span class="sxs-lookup"><span data-stu-id="b18a7-112">A modern web browser that supports HTML5.</span></span>

## <a name="understanding-tez"></a><span data-ttu-id="b18a7-113">Tez ismertetése</span><span class="sxs-lookup"><span data-stu-id="b18a7-113">Understanding Tez</span></span>

<span data-ttu-id="b18a7-114">Tez egy bővíthető keretrendszer, amely a hagyományos MapReduce feldolgozási-nál nagyobb sebesség biztosítja az adatok feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="b18a7-114">Tez is an extensible framework for data processing in Hadoop that provides greater speeds than traditional MapReduce processing.</span></span> <span data-ttu-id="b18a7-115">Linux-alapú HDInsight-fürtök az alapértelmezett szolgáltatás a Hive esetén.</span><span class="sxs-lookup"><span data-stu-id="b18a7-115">For Linux-based HDInsight clusters, it is the default engine for Hive.</span></span>

<span data-ttu-id="b18a7-116">Tez létrehoz egy irányított aciklikus diagramhoz (DAG), amely leírja a feladatok által szükséges műveletek sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="b18a7-116">Tez creates a Directed Acyclic Graph (DAG) that describes the order of actions required by jobs.</span></span> <span data-ttu-id="b18a7-117">Egyes műveletek csúcsban hívják, és egy adat, a teljes feladat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="b18a7-117">Individual actions are called vertices, and execute a piece of the overall job.</span></span> <span data-ttu-id="b18a7-118">A munkahelyi csúcspont szerint tényleges végrehajtása feladat neve, és előfordulhat, hogy a fürt több csomópontja legyen elosztva.</span><span class="sxs-lookup"><span data-stu-id="b18a7-118">The actual execution of the work described by a vertex is called a task, and may be distributed across multiple nodes in the cluster.</span></span>

### <a name="understanding-the-tez-view"></a><span data-ttu-id="b18a7-119">A Tez nézet ismertetése</span><span class="sxs-lookup"><span data-stu-id="b18a7-119">Understanding the Tez view</span></span>

<span data-ttu-id="b18a7-120">A Tez nézet futó folyamatok előzményadatok és információkat is biztosít.</span><span class="sxs-lookup"><span data-stu-id="b18a7-120">The Tez view provides both historical information and information on processes that are running.</span></span> <span data-ttu-id="b18a7-121">Ezeket az információkat jeleníti meg, hogyan oszlik meg a feladat más fürtökre.</span><span class="sxs-lookup"><span data-stu-id="b18a7-121">This information shows how a job is distributed across clusters.</span></span> <span data-ttu-id="b18a7-122">Feladatok és a csúcsban által használt számlálókat, és a feladathoz kapcsolódó hibainformációk is megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="b18a7-122">It also displays counters used by tasks and vertices, and error information related to the job.</span></span> <span data-ttu-id="b18a7-123">Felajánlhatja, hogy a következő esetekben hasznos információkat:</span><span class="sxs-lookup"><span data-stu-id="b18a7-123">It may offer useful information in the following scenarios:</span></span>

* <span data-ttu-id="b18a7-124">Figyelési hosszan futó dolgozza fel a térkép állapotának megtekintése, és a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="b18a7-124">Monitoring long-running processes, viewing the progress of map and reduce tasks.</span></span>
* <span data-ttu-id="b18a7-125">Megtudhatja, hogyan lehet javítani, feldolgozás, vagy sikertelenségének sikeres vagy sikertelen folyamatok előzményadatainak elemzése.</span><span class="sxs-lookup"><span data-stu-id="b18a7-125">Analyzing historical data for successful or failed processes to learn how processing could be improved or why it failed.</span></span>

## <a name="generate-a-dag"></a><span data-ttu-id="b18a7-126">Egy dag-csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="b18a7-126">Generate a DAG</span></span>

<span data-ttu-id="b18a7-127">A Tez nézet csak adatokat tartalmaz, ha egy feladatot, amely használja a Tez motor éppen fut, vagy már korábban lefutott.</span><span class="sxs-lookup"><span data-stu-id="b18a7-127">The Tez view only contains data if a job that uses the Tez engine is currently running, or has been ran previously.</span></span> <span data-ttu-id="b18a7-128">Egyszerű Hive-lekérdezéseket is feloldható Tez használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="b18a7-128">Simple Hive queries can be resolved without using Tez.</span></span> <span data-ttu-id="b18a7-129">Összetettebb lekérdezi, hogy tegye szűrés csoportosítás, rendezés, illesztéseket, stb.</span><span class="sxs-lookup"><span data-stu-id="b18a7-129">More complex queries that do filtering, grouping, ordering, joins, etc.</span></span> <span data-ttu-id="b18a7-130">a Tez motort használja.</span><span class="sxs-lookup"><span data-stu-id="b18a7-130">use the Tez engine.</span></span>

<span data-ttu-id="b18a7-131">Az alábbi lépések segítségével által használt Tez Hive-lekérdezések futtatása:</span><span class="sxs-lookup"><span data-stu-id="b18a7-131">Use the following steps to run a Hive query that uses Tez:</span></span>

1. <span data-ttu-id="b18a7-132">Egy böngészőben navigáljon a https://CLUSTERNAME.azurehdinsight.net, ahol **CLUSTERNAME** a HDInsight-fürt neve.</span><span class="sxs-lookup"><span data-stu-id="b18a7-132">In a web browser, navigate to https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is the name of your HDInsight cluster.</span></span>

2. <span data-ttu-id="b18a7-133">Az oldal tetején a menüből válassza ki a **nézetek** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b18a7-133">From the menu at the top of the page, select the **Views** icon.</span></span> <span data-ttu-id="b18a7-134">Ez az ikon négyzetes több tűnik.</span><span class="sxs-lookup"><span data-stu-id="b18a7-134">This icon looks like a series of squares.</span></span> <span data-ttu-id="b18a7-135">Válassza ki a legördülő listában megjelenő, **Hive view**.</span><span class="sxs-lookup"><span data-stu-id="b18a7-135">In the dropdown that appears, select **Hive view**.</span></span>

    ![Hive-nézetek kijelölése](./media/hdinsight-debug-ambari-tez-view/selecthive.png)

3. <span data-ttu-id="b18a7-137">Ha a Hive nézete betölti, illessze be a következő lekérdezés a lekérdezés-szerkesztő be, és kattintson **hajtható végre**.</span><span class="sxs-lookup"><span data-stu-id="b18a7-137">When the Hive view loads, paste the following query into the Query Editor, and then click **execute**.</span></span>

        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;

    <span data-ttu-id="b18a7-138">A feladat befejezése után a megjelenő kimenetnek kell megjelennie a **lekérdezési folyamat eredményei** szakasz.</span><span class="sxs-lookup"><span data-stu-id="b18a7-138">Once the job has completed, you should see the output displayed in the **Query Process Results** section.</span></span> <span data-ttu-id="b18a7-139">Az eredmény az alábbihoz hasonló legyen:</span><span class="sxs-lookup"><span data-stu-id="b18a7-139">The results should be similar to the following text:</span></span>

        market  state       country
        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica

4. <span data-ttu-id="b18a7-140">Válassza ki a **napló** fülre.</span><span class="sxs-lookup"><span data-stu-id="b18a7-140">Select the **Log** tab.</span></span> <span data-ttu-id="b18a7-141">Információ az alábbihoz hasonló jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="b18a7-141">You see information similar to the following text:</span></span>

        INFO : Session is already open
        INFO :

        INFO : Status: Running (Executing on YARN cluster with App id application_1454546500517_0063)

    <span data-ttu-id="b18a7-142">Mentse a **alkalmazásazonosító** érték, mivel ez az érték a következő szakaszban használható.</span><span class="sxs-lookup"><span data-stu-id="b18a7-142">Save the **App id** value, as this value is used in the next section.</span></span>

## <a name="use-the-tez-view"></a><span data-ttu-id="b18a7-143">A Tez nézetben</span><span class="sxs-lookup"><span data-stu-id="b18a7-143">Use the Tez View</span></span>

1. <span data-ttu-id="b18a7-144">Az oldal tetején a menüből válassza ki a **nézetek** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b18a7-144">From the menu at the top of the page, select the **Views** icon.</span></span> <span data-ttu-id="b18a7-145">Válassza ki a legördülő listában megjelenő, **Tez nézet**.</span><span class="sxs-lookup"><span data-stu-id="b18a7-145">In the dropdown that appears, select **Tez view**.</span></span>

    ![Tez nézet kijelölése](./media/hdinsight-debug-ambari-tez-view/selecttez.png)

2. <span data-ttu-id="b18a7-147">A Tez nézet betöltésekor megjelenik, amely jelenleg fut, vagy hive-lekérdezések listáját a fürtön futottak.</span><span class="sxs-lookup"><span data-stu-id="b18a7-147">When the Tez view loads, you see a list of hive queries that are currently running, or have been ran on the cluster.</span></span>

    ![Minden DAG](./media/hdinsight-debug-ambari-tez-view/tez-view-home.png)

3. <span data-ttu-id="b18a7-149">Ha csak egy bejegyzés, a lekérdezés, amely futtatta az előző szakaszban is.</span><span class="sxs-lookup"><span data-stu-id="b18a7-149">If you have only one entry, it is for the query that you ran in the previous section.</span></span> <span data-ttu-id="b18a7-150">Ha több bejegyzést, az oldal tetején a mezők használatával kereshet.</span><span class="sxs-lookup"><span data-stu-id="b18a7-150">If you have multiple entries, you can search by using the fields at the top of the page.</span></span>

4. <span data-ttu-id="b18a7-151">Válassza ki a **Lekérdezésazonosítóval** a Hive-lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="b18a7-151">Select the **Query ID** for a Hive query.</span></span> <span data-ttu-id="b18a7-152">A lekérdezés információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="b18a7-152">Information about the query is displayed.</span></span>

    ![DAG-részletek](./media/hdinsight-debug-ambari-tez-view/query-details.png)

5. <span data-ttu-id="b18a7-154">Ezen a lapon a lapok engedélyezi, hogy a következő információk megtekintése:</span><span class="sxs-lookup"><span data-stu-id="b18a7-154">The tabs on this page allow you to view the following information:</span></span>

    * <span data-ttu-id="b18a7-155">**Részletei lekérdezni**: a Hive-lekérdezés részleteiről.</span><span class="sxs-lookup"><span data-stu-id="b18a7-155">**Query Details**: Details about the Hive query.</span></span>
    * <span data-ttu-id="b18a7-156">**Az idősor**: mennyi ideig tartott minden szakaszhoz feldolgozási információt.</span><span class="sxs-lookup"><span data-stu-id="b18a7-156">**Timeline**: Information about how long each stage of processing took.</span></span>
    * <span data-ttu-id="b18a7-157">**Konfigurációk**: ehhez a lekérdezéshez használt konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="b18a7-157">**Configurations**: The configuration used for this query.</span></span>

    <span data-ttu-id="b18a7-158">A __lekérdezés részletei__ hivatkozások segítségével talál információkat a __alkalmazás__ vagy a __DAG__ ehhez a lekérdezéshez.</span><span class="sxs-lookup"><span data-stu-id="b18a7-158">From __Query Details__ you can use the links to find information about the __Application__ or the __DAG__ for this query.</span></span>
    
    * <span data-ttu-id="b18a7-159">A __alkalmazás__ hivatkozás ehhez a lekérdezéshez a YARN alkalmazással kapcsolatos információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b18a7-159">The __Application__ link displays information about the YARN application for this query.</span></span> <span data-ttu-id="b18a7-160">Itt a YARN alkalmazásnaplók végezheti el.</span><span class="sxs-lookup"><span data-stu-id="b18a7-160">From here you can access the YARN application logs.</span></span>
    * <span data-ttu-id="b18a7-161">A __DAG__ hivatkozás ehhez a lekérdezéshez irányított aciklikus diagramhoz információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b18a7-161">The __DAG__ link displays information about the directed acyclic graph for this query.</span></span> <span data-ttu-id="b18a7-162">Itt megtekintheti a DAG grafikus ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="b18a7-162">From here you can view a graphical representation of the DAG.</span></span> <span data-ttu-id="b18a7-163">A DAG belül a csúcsban információk is megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="b18a7-163">You can also find information on the vertices within the DAG.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b18a7-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b18a7-164">Next Steps</span></span>

<span data-ttu-id="b18a7-165">Most, hogy rendelkezik megtudta, hogyan használja a Tez, további információ [használata a HDInsight Hive](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="b18a7-165">Now that you have learned how to use the Tez view, learn more about [Using Hive on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="b18a7-166">Részletesebb műszaki információkat Tez, lásd: a [Hortonworks Tez lapon](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="b18a7-166">For more detailed technical information on Tez, see the [Tez page at Hortonworks](http://hortonworks.com/hadoop/tez/).</span></span>

<span data-ttu-id="b18a7-167">Az Ambari és a HDInsight együttes használatával további információkért lásd: [kezelése HDInsight-fürtök az Ambari webes felhasználói felület használatával](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="b18a7-167">For more information on using Ambari with HDInsight, see [Manage HDInsight clusters using the Ambari Web UI](hdinsight-hadoop-manage-ambari.md)</span></span>
