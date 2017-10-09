---
title: "a HDInsight - Azure Ambari Tez nézet aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Ambari Tez megtekintése a HDInsight toodebug Tez feladatokhoz."
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
ms.openlocfilehash: 5d61bd0403c98284c86982073af91468ae62ac60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-ambari-views-toodebug-tez-jobs-on-hdinsight"></a><span data-ttu-id="91ec1-103">Az Ambari nézetek toodebug Tez feladatokhoz a HDInsight használata</span><span class="sxs-lookup"><span data-stu-id="91ec1-103">Use Ambari Views toodebug Tez Jobs on HDInsight</span></span>

<span data-ttu-id="91ec1-104">hello Ambari webes felhasználói felületén a HDInsight a Tez nézetet tartalmaz, amely lehet Tez használó használt toounderstand és hibakeresési feladatokat.</span><span class="sxs-lookup"><span data-stu-id="91ec1-104">hello Ambari Web UI for HDInsight contains a Tez view that can be used toounderstand and debug jobs that use Tez.</span></span> <span data-ttu-id="91ec1-105">hello Tez nézet lehetővé teszi toovisualize hello feladat csatlakoztatott elemek grafikon elemezze az egyes elemek, valamint statisztikák és naplózási információk beolvasása.</span><span class="sxs-lookup"><span data-stu-id="91ec1-105">hello Tez view allows you toovisualize hello job as a graph of connected items, drill into each item, and retrieve statistics and logging information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="91ec1-106">hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="91ec1-106">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="91ec1-107">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="91ec1-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="91ec1-108">További információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="91ec1-108">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="91ec1-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="91ec1-109">Prerequisites</span></span>

* <span data-ttu-id="91ec1-110">A Linux-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="91ec1-110">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="91ec1-111">A fürt létrehozásának lépései: [Ismerkedés a Linux-alapú HDInsight eszközzel](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="91ec1-111">For steps on creating a cluster, see [Get started using Linux-based HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
* <span data-ttu-id="91ec1-112">Egy HTML5-támogatással rendelkező modern webböngésző.</span><span class="sxs-lookup"><span data-stu-id="91ec1-112">A modern web browser that supports HTML5.</span></span>

## <a name="understanding-tez"></a><span data-ttu-id="91ec1-113">Tez ismertetése</span><span class="sxs-lookup"><span data-stu-id="91ec1-113">Understanding Tez</span></span>

<span data-ttu-id="91ec1-114">Tez egy bővíthető keretrendszer, amely a hagyományos MapReduce feldolgozási-nál nagyobb sebesség biztosítja az adatok feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="91ec1-114">Tez is an extensible framework for data processing in Hadoop that provides greater speeds than traditional MapReduce processing.</span></span> <span data-ttu-id="91ec1-115">Linux-alapú HDInsight-fürtök a Hive hello alapértelmezett motor esetén.</span><span class="sxs-lookup"><span data-stu-id="91ec1-115">For Linux-based HDInsight clusters, it is hello default engine for Hive.</span></span>

<span data-ttu-id="91ec1-116">Tez egy irányított aciklikus diagramhoz (DAG), amely leírja a feladatok által szükséges műveletek sorrendjét hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="91ec1-116">Tez creates a Directed Acyclic Graph (DAG) that describes hello order of actions required by jobs.</span></span> <span data-ttu-id="91ec1-117">Egyes műveletek csúcsban nevezik, és hajtható végre egy hello adat általános feladat.</span><span class="sxs-lookup"><span data-stu-id="91ec1-117">Individual actions are called vertices, and execute a piece of hello overall job.</span></span> <span data-ttu-id="91ec1-118">hello tényleges csúcspont szerint hello munkaelem végrehajtásának feladat neve és hello fürt több csomópontja között szétoszthatók.</span><span class="sxs-lookup"><span data-stu-id="91ec1-118">hello actual execution of hello work described by a vertex is called a task, and may be distributed across multiple nodes in hello cluster.</span></span>

### <a name="understanding-hello-tez-view"></a><span data-ttu-id="91ec1-119">Understanding hello Tez megtekintése</span><span class="sxs-lookup"><span data-stu-id="91ec1-119">Understanding hello Tez view</span></span>

<span data-ttu-id="91ec1-120">hello Tez nézet futó folyamatok előzményadatok és a információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="91ec1-120">hello Tez view provides both historical information and information on processes that are running.</span></span> <span data-ttu-id="91ec1-121">Ezeket az információkat jeleníti meg, hogyan oszlik meg a feladat más fürtökre.</span><span class="sxs-lookup"><span data-stu-id="91ec1-121">This information shows how a job is distributed across clusters.</span></span> <span data-ttu-id="91ec1-122">Feladatok és a csúcsban által használt számlálókat is megjeleníti, és a hibainformációk kapcsolódó toohello feladat.</span><span class="sxs-lookup"><span data-stu-id="91ec1-122">It also displays counters used by tasks and vertices, and error information related toohello job.</span></span> <span data-ttu-id="91ec1-123">Felajánlhatja, hogy a következő forgatókönyvek hello hasznos információkat:</span><span class="sxs-lookup"><span data-stu-id="91ec1-123">It may offer useful information in hello following scenarios:</span></span>

* <span data-ttu-id="91ec1-124">Hosszan futó folyamatok figyelése, megtekintése hello térkép előrehaladását, és csökkentheti a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="91ec1-124">Monitoring long-running processes, viewing hello progress of map and reduce tasks.</span></span>
* <span data-ttu-id="91ec1-125">Sikeres vagy sikertelen folyamatok toolearn előzményadatainak elemzése, hogyan lehet javítani, feldolgozás, vagy okát.</span><span class="sxs-lookup"><span data-stu-id="91ec1-125">Analyzing historical data for successful or failed processes toolearn how processing could be improved or why it failed.</span></span>

## <a name="generate-a-dag"></a><span data-ttu-id="91ec1-126">Egy dag-csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="91ec1-126">Generate a DAG</span></span>

<span data-ttu-id="91ec1-127">hello nézet csak olyan adatokat, ha egy feladat által használt hello Tez motor Tez jelenleg is fut, vagy a korábban lefutott.</span><span class="sxs-lookup"><span data-stu-id="91ec1-127">hello Tez view only contains data if a job that uses hello Tez engine is currently running, or has been ran previously.</span></span> <span data-ttu-id="91ec1-128">Egyszerű Hive-lekérdezéseket is feloldható Tez használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="91ec1-128">Simple Hive queries can be resolved without using Tez.</span></span> <span data-ttu-id="91ec1-129">Összetettebb lekérdezi, hogy tegye szűrés csoportosítás, rendezés, illesztéseket, stb.</span><span class="sxs-lookup"><span data-stu-id="91ec1-129">More complex queries that do filtering, grouping, ordering, joins, etc.</span></span> <span data-ttu-id="91ec1-130">hello Tez motort használja.</span><span class="sxs-lookup"><span data-stu-id="91ec1-130">use hello Tez engine.</span></span>

<span data-ttu-id="91ec1-131">A következő lépéseket toorun által használt Tez Hive-lekérdezések hello használata:</span><span class="sxs-lookup"><span data-stu-id="91ec1-131">Use hello following steps toorun a Hive query that uses Tez:</span></span>

1. <span data-ttu-id="91ec1-132">Egy böngészőben nyissa meg a toohttps://CLUSTERNAME.azurehdinsight.net, ahol **CLUSTERNAME** hello neve, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="91ec1-132">In a web browser, navigate toohttps://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is hello name of your HDInsight cluster.</span></span>

2. <span data-ttu-id="91ec1-133">Hello oldal hello tetején hello menüben válassza ki a hello **nézetek** ikonra.</span><span class="sxs-lookup"><span data-stu-id="91ec1-133">From hello menu at hello top of hello page, select hello **Views** icon.</span></span> <span data-ttu-id="91ec1-134">Ez az ikon négyzetes több tűnik.</span><span class="sxs-lookup"><span data-stu-id="91ec1-134">This icon looks like a series of squares.</span></span> <span data-ttu-id="91ec1-135">A megjelenő hello legördülő menüben válassza ki **Hive view**.</span><span class="sxs-lookup"><span data-stu-id="91ec1-135">In hello dropdown that appears, select **Hive view**.</span></span>

    ![Hive-nézetek kijelölése](./media/hdinsight-debug-ambari-tez-view/selecthive.png)

3. <span data-ttu-id="91ec1-137">Ha hello Hive nézete betölti, Beillesztés hello következő hello Lekérdezésszerkesztő történő lekérdezése, és kattintson **hajtható végre**.</span><span class="sxs-lookup"><span data-stu-id="91ec1-137">When hello Hive view loads, paste hello following query into hello Query Editor, and then click **execute**.</span></span>

        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;

    <span data-ttu-id="91ec1-138">Hello feladat befejezése után jelennek meg hello hello kimenetet kell látnia **lekérdezési folyamat eredményei** szakasz.</span><span class="sxs-lookup"><span data-stu-id="91ec1-138">Once hello job has completed, you should see hello output displayed in hello **Query Process Results** section.</span></span> <span data-ttu-id="91ec1-139">hello eredmények hasonló toohello szöveg a következő legyen:</span><span class="sxs-lookup"><span data-stu-id="91ec1-139">hello results should be similar toohello following text:</span></span>

        market  state       country
        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica

4. <span data-ttu-id="91ec1-140">Jelölje be hello **napló** fülre. Információk a következő szöveg hasonló toohello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="91ec1-140">Select hello **Log** tab. You see information similar toohello following text:</span></span>

        INFO : Session is already open
        INFO :

        INFO : Status: Running (Executing on YARN cluster with App id application_1454546500517_0063)

    <span data-ttu-id="91ec1-141">Mentse a hello **alkalmazásazonosító** érték, mivel ezt az értéket használja a következő szakaszban hello.</span><span class="sxs-lookup"><span data-stu-id="91ec1-141">Save hello **App id** value, as this value is used in hello next section.</span></span>

## <a name="use-hello-tez-view"></a><span data-ttu-id="91ec1-142">Tez nézet hello használata</span><span class="sxs-lookup"><span data-stu-id="91ec1-142">Use hello Tez View</span></span>

1. <span data-ttu-id="91ec1-143">Hello oldal hello tetején hello menüben válassza ki a hello **nézetek** ikonra.</span><span class="sxs-lookup"><span data-stu-id="91ec1-143">From hello menu at hello top of hello page, select hello **Views** icon.</span></span> <span data-ttu-id="91ec1-144">A megjelenő hello legördülő menüben válassza ki **Tez nézet**.</span><span class="sxs-lookup"><span data-stu-id="91ec1-144">In hello dropdown that appears, select **Tez view**.</span></span>

    ![Tez nézet kijelölése](./media/hdinsight-debug-ambari-tez-view/selecttez.png)

2. <span data-ttu-id="91ec1-146">Hello Tez nézet betöltésekor hello fürt futott, amely jelenleg fut, vagy hive-lekérdezések listáját láthatja.</span><span class="sxs-lookup"><span data-stu-id="91ec1-146">When hello Tez view loads, you see a list of hive queries that are currently running, or have been ran on hello cluster.</span></span>

    ![Minden DAG](./media/hdinsight-debug-ambari-tez-view/tez-view-home.png)

3. <span data-ttu-id="91ec1-148">Ha csak egy bejegyzést, az előző szakaszban hello futtatott hello lekérdezés is.</span><span class="sxs-lookup"><span data-stu-id="91ec1-148">If you have only one entry, it is for hello query that you ran in hello previous section.</span></span> <span data-ttu-id="91ec1-149">Ha több bejegyzés, kereshet hello mezőkkel hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="91ec1-149">If you have multiple entries, you can search by using hello fields at hello top of hello page.</span></span>

4. <span data-ttu-id="91ec1-150">Jelölje be hello **Lekérdezésazonosítóval** a Hive-lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="91ec1-150">Select hello **Query ID** for a Hive query.</span></span> <span data-ttu-id="91ec1-151">Hello lekérdezés információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="91ec1-151">Information about hello query is displayed.</span></span>

    ![DAG-részletek](./media/hdinsight-debug-ambari-tez-view/query-details.png)

5. <span data-ttu-id="91ec1-153">hello lapok ezen a lapon a következő információk tooview hello engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="91ec1-153">hello tabs on this page allow you tooview hello following information:</span></span>

    * <span data-ttu-id="91ec1-154">**Részletei lekérdezni**: hello Hive-lekérdezések adatait.</span><span class="sxs-lookup"><span data-stu-id="91ec1-154">**Query Details**: Details about hello Hive query.</span></span>
    * <span data-ttu-id="91ec1-155">**Az idősor**: mennyi ideig tartott minden szakaszhoz feldolgozási információt.</span><span class="sxs-lookup"><span data-stu-id="91ec1-155">**Timeline**: Information about how long each stage of processing took.</span></span>
    * <span data-ttu-id="91ec1-156">**Konfigurációk**: hello konfigurálása ehhez a lekérdezéshez használt.</span><span class="sxs-lookup"><span data-stu-id="91ec1-156">**Configurations**: hello configuration used for this query.</span></span>

    <span data-ttu-id="91ec1-157">A __lekérdezés részletei__ használhatja hello hivatkozások toofind információ hello __alkalmazás__ vagy hello __DAG__ ehhez a lekérdezéshez.</span><span class="sxs-lookup"><span data-stu-id="91ec1-157">From __Query Details__ you can use hello links toofind information about hello __Application__ or hello __DAG__ for this query.</span></span>
    
    * <span data-ttu-id="91ec1-158">Hello __alkalmazás__ hivatkozás hello YARN alkalmazás ehhez a lekérdezéshez információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="91ec1-158">hello __Application__ link displays information about hello YARN application for this query.</span></span> <span data-ttu-id="91ec1-159">Itt hello YARN alkalmazásnaplók végezheti el.</span><span class="sxs-lookup"><span data-stu-id="91ec1-159">From here you can access hello YARN application logs.</span></span>
    * <span data-ttu-id="91ec1-160">Hello __DAG__ hivatkozás hello irányított aciklikus diagramhoz ehhez a lekérdezéshez információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="91ec1-160">hello __DAG__ link displays information about hello directed acyclic graph for this query.</span></span> <span data-ttu-id="91ec1-161">Itt megtekintheti a hello DAG grafikus ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="91ec1-161">From here you can view a graphical representation of hello DAG.</span></span> <span data-ttu-id="91ec1-162">Hello csúcsban belül hello DAG információk is megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="91ec1-162">You can also find information on hello vertices within hello DAG.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91ec1-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="91ec1-163">Next Steps</span></span>

<span data-ttu-id="91ec1-164">Most, hogy megtanulta, hogyan toouse hello Tez meg, további információ [használata a HDInsight Hive](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="91ec1-164">Now that you have learned how toouse hello Tez view, learn more about [Using Hive on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="91ec1-165">Részletesebb műszaki információkat Tez, lásd: hello [Hortonworks Tez lapon](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="91ec1-165">For more detailed technical information on Tez, see hello [Tez page at Hortonworks](http://hortonworks.com/hadoop/tez/).</span></span>

<span data-ttu-id="91ec1-166">Az Ambari és a HDInsight együttes használatával további információkért lásd: [hello Ambari webes felhasználói felület használatával kezelheti a HDInsight-fürtök](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="91ec1-166">For more information on using Ambari with HDInsight, see [Manage HDInsight clusters using hello Ambari Web UI](hdinsight-hadoop-manage-ambari.md)</span></span>
