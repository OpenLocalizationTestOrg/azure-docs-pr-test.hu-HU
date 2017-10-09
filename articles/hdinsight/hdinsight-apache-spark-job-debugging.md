---
title: "aaaDebug Apache Spark on Azure HDInsight futó feladatok |} Microsoft Docs"
description: "YARN felhasználói felületen, a Spark felhasználói felület és a Spark-előzmények server tootrack és hibakeresési futó feladatok az Azure HDInsight Spark-fürt használatára"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 59af05a7-2bd9-44b0-b55f-2438d294198b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 33d352a5773920735aa4e5e8532b78122f381377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a><span data-ttu-id="a7fa3-103">Azure hdinsighton futó Apache Spark feladatok hibakeresése</span><span class="sxs-lookup"><span data-stu-id="a7fa3-103">Debug Apache Spark jobs running on Azure HDInsight</span></span>

<span data-ttu-id="a7fa3-104">Ebben a cikkben megtudhatja, hogyan tootrack és hibakeresési hello YARN felhasználói felületen, Spark felhasználói felületén, HDInsight-fürtök futó feladatok Spark és Spark előzmények Server hello.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-104">In this article you will learn how tootrack and debug Spark jobs running on HDInsight clusters using hello YARN UI, Spark UI, and hello Spark History Server.</span></span> <span data-ttu-id="a7fa3-105">Ebben a cikkben azt feladatot indít el a Spark elérhető Jegyzetfüzet használata Spark-fürt hello **gépi tanulás: prediktív elemzési étele ellenőrző adatokat, és MLLib a**.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-105">For this article, we will start a Spark job using a notebook available with hello Spark cluster, **Machine learning: Predictive analysis on food inspection data using MLLib**.</span></span> <span data-ttu-id="a7fa3-106">Hello lépések végrehajtásával tootrack beküldött bármely más megközelítéssel is, például egy alkalmazás alábbi **spark-nyújt**.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-106">You can use hello steps below tootrack an application that you submitted using any other approach as well, for example, **spark-submit**.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a7fa3-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a7fa3-107">Prerequisites</span></span>
<span data-ttu-id="a7fa3-108">Hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="a7fa3-108">You must have hello following:</span></span>

* <span data-ttu-id="a7fa3-109">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-109">An Azure subscription.</span></span> <span data-ttu-id="a7fa3-110">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="a7fa3-110">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="a7fa3-111">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="a7fa3-112">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="a7fa3-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="a7fa3-113">Meg kell kezdeni hello notebook futtató  **[gépi tanulás: prediktív elemzési a étele ellenőrző adatokat, és MLLib](hdinsight-apache-spark-machine-learning-mllib-ipython.md)**.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-113">You should have started running hello notebook, **[Machine learning: Predictive analysis on food inspection data using MLLib](hdinsight-apache-spark-machine-learning-mllib-ipython.md)**.</span></span> <span data-ttu-id="a7fa3-114">Útmutatást toorun jegyzetfüzet, hajtsa végre hello hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-114">For instructions on how toorun this notebook, follow hello link.</span></span>  

## <a name="track-an-application-in-hello-yarn-ui"></a><span data-ttu-id="a7fa3-115">Egy alkalmazás a YARN felhasználói felületen hello nyomon követése</span><span class="sxs-lookup"><span data-stu-id="a7fa3-115">Track an application in hello YARN UI</span></span>
1. <span data-ttu-id="a7fa3-116">Indítsa el a hello YARN felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-116">Launch hello YARN UI.</span></span> <span data-ttu-id="a7fa3-117">Hello-fürt panelén kattintson **fürt irányítópult**, és kattintson a **YARN**.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-117">From hello cluster blade, click **Cluster Dashboard**, and then click **YARN**.</span></span>
   
    ![Indítsa el a YARN felhasználói felületen](./media/hdinsight-apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]
   > <span data-ttu-id="a7fa3-119">Másik lehetőségként is elindíthatja a hello YARN felhasználói felületen a hello Ambari felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-119">Alternatively, you can also launch hello YARN UI from hello Ambari UI.</span></span> <span data-ttu-id="a7fa3-120">toolaunch hello Ambari felhasználói felületén, a hello-fürt panelén kattintson **fürt irányítópult**, és kattintson a **HDInsight fürt irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-120">toolaunch hello Ambari UI, from hello cluster blade, click **Cluster Dashboard**, and then click **HDInsight Cluster Dashboard**.</span></span> <span data-ttu-id="a7fa3-121">A hello Ambari felhasználói felület, kattintson az **YARN**, kattintson a **Gyorshivatkozások**, kattintson hello aktív erőforrás-kezelő, majd **erőforrás-kezelő felhasználói felületén**.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-121">From hello Ambari UI, click **YARN**, click **Quick Links**, click hello active resource manager, and then click **ResourceManager UI**.</span></span>    
   > 
   > 
2. <span data-ttu-id="a7fa3-122">Hello Spark feladat Jupyter notebookok használatával indította, mert hello alkalmazásnak hello neve **remotesparkmagics** (Ez az minden olyan alkalmazásnál, amely hello notebookok való indítása hello név).</span><span class="sxs-lookup"><span data-stu-id="a7fa3-122">Because you started hello Spark job using Jupyter notebooks, hello application has hello name **remotesparkmagics** (this is hello name for all applications that are started from hello notebooks).</span></span> <span data-ttu-id="a7fa3-123">Kattintson a hello Alkalmazásazonosító elleni hello alkalmazás neve tooget hello feladat további információt.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-123">Click hello application ID against hello application name tooget more information about hello job.</span></span> <span data-ttu-id="a7fa3-124">Ekkor elindul a hello alkalmazás megtekintése.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-124">This launches hello application view.</span></span>
   
    ![Keresés a külső alkalmazás azonosítója](./media/hdinsight-apache-spark-job-debugging/find-application-id.png)
   
    <span data-ttu-id="a7fa3-126">Az ilyen alkalmazások esetében a hello Jupyter notebookokból származó működését, hello állapota mindig **futtató** amíg hello notebook kilép.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-126">For such applications that are launched from hello Jupyter notebooks, hello status is always **RUNNING** until you exit hello notebook.</span></span>
3. <span data-ttu-id="a7fa3-127">Hello alkalmazás nézetből részletezve további toofind hello alkalmazás és hello naplók (stdout/stderr) társított hello tárolók ki.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-127">From hello application view, you can drill down further toofind out hello containers associated with hello application and hello logs (stdout/stderr).</span></span> <span data-ttu-id="a7fa3-128">Külső felhasználói felület hello indítsa el megfelelő toohello linking hello kattintva **követési URL-cím**lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-128">You can also launch hello Spark UI by clicking hello linking corresponding toohello **Tracking URL**, as shown below.</span></span> 
   
    ![Tároló naplók letöltése](./media/hdinsight-apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-hello-spark-ui"></a><span data-ttu-id="a7fa3-130">Nyomon követheti az alkalmazás hello Spark felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="a7fa3-130">Track an application in hello Spark UI</span></span>
<span data-ttu-id="a7fa3-131">Hello Spark felhasználói felület, a részletezhető le, hogy a korábbi lépések hello alkalmazás által indított vannak hello Spark feladatok.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-131">In hello Spark UI, you can drill down into hello Spark jobs that are spawned by hello application you started earlier.</span></span>

1. <span data-ttu-id="a7fa3-132">toolaunch hello Spark UI hello alkalmazás nézetből elleni hello hello hivatkozásra **követési URL-cím**, a fenti hello képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-132">toolaunch hello Spark UI, from hello application view, click hello link against hello **Tracking URL**, as shown in hello screen capture above.</span></span> <span data-ttu-id="a7fa3-133">Az összes futó hello Jupyter notebook hello alkalmazás működését hello Spark feladatok tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-133">You can see all hello Spark jobs that are launched by hello application running in hello Jupyter notebook.</span></span>
   
    ![A Spark-feladatok megtekintése](./media/hdinsight-apache-spark-job-debugging/view-spark-jobs.png)
2. <span data-ttu-id="a7fa3-135">Kattintson a hello **végrehajtója** lapon minden egyes végrehajtó toosee adatfeldolgozás és -tárolás adatait.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-135">Click hello **Executors** tab toosee processing and storage information for each executor.</span></span> <span data-ttu-id="a7fa3-136">Hello hívási verem hello kattintva is lekérhet **szál Dump** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-136">You can also retrieve hello call stack by clicking on hello **Thread Dump** link.</span></span>
   
    ![Spark végrehajtója megtekintése](./media/hdinsight-apache-spark-job-debugging/view-spark-executors.png)
3. <span data-ttu-id="a7fa3-138">Kattintson a hello **szakaszból** toosee hello szakaszból hello alkalmazáshoz kapcsolódó lapon.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-138">Click hello **Stages** tab toosee hello stages associated with hello application.</span></span>
   
    ![A Spark-állapotok megjelenítése](./media/hdinsight-apache-spark-job-debugging/view-spark-stages.png)
   
    <span data-ttu-id="a7fa3-140">Minden szakaszhoz rendelkezhet több feladatot, amely találhatja meg a végrehajtási statisztika, például alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-140">Each stage can have multiple tasks for which you can view execution statistics, like shown below.</span></span>
   
    ![A Spark-állapotok megjelenítése](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-details.png) 
4. <span data-ttu-id="a7fa3-142">Hello szakasz részleteit megjelenítő oldalon DAG képi megjelenítés is elindíthatja.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-142">From hello stage details page, you can launch DAG Visualization.</span></span> <span data-ttu-id="a7fa3-143">Bontsa ki a hello **DAG képi megjelenítés** hivatkozás hello lapon hello tetején alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-143">Expand hello **DAG Visualization** link at hello top of hello page, as shown below.</span></span>
   
    ![Spark szakaszból DAG képi megjelenítés megtekintése](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    <span data-ttu-id="a7fa3-145">DAG vagy közvetlen Aclyic Graph jelöli hello különböző szakaszaiban hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-145">DAG or Direct Aclyic Graph represents hello different stages in hello application.</span></span> <span data-ttu-id="a7fa3-146">Minden egyes kék mező hello grafikonon egy Spark művelet meghívása hello alkalmazásból jelöli.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-146">Each blue box in hello graph represents a Spark operation invoked from hello application.</span></span>
5. <span data-ttu-id="a7fa3-147">Hello szakasz részleteit megjelenítő oldalon is elindíthatja a hello alkalmazás idősor nézetének megnyitására.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-147">From hello stage details page, you can also launch hello application timeline view.</span></span> <span data-ttu-id="a7fa3-148">Bontsa ki a hello **esemény ütemterv** hivatkozás hello lapon hello tetején alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-148">Expand hello **Event Timeline** link at hello top of hello page, as shown below.</span></span>
   
    ![Spark szakaszból esemény ütemterv megtekintése](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    <span data-ttu-id="a7fa3-150">Ez hello Spark eseményeket ütemterv hello formában jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-150">This displays hello Spark events in hello form of a timeline.</span></span> <span data-ttu-id="a7fa3-151">hello Ütemterv nézet áll rendelkezésre három szinten különböző feladatokat, a feladatok és egy szakaszon belül.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-151">hello timeline view is available at three levels, across jobs, within a job, and within a stage.</span></span> <span data-ttu-id="a7fa3-152">a fenti kép hello hello idősor nézetének megnyitására adott szakaszában rögzíti.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-152">hello image above captures hello timeline view for a given stage.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="a7fa3-153">Ha hello **engedélyezése a Nagyítás** jelölőnégyzetet, görgetve bal és jobb között hello idősor nézetének megnyitására.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-153">If you select hello **Enable zooming** check box, you can scroll left and right across hello timeline view.</span></span>
   > 
   > 
6. <span data-ttu-id="a7fa3-154">A hello Spark felhasználói felületének más lapjaira hello Spark-példányban is hasznos információt nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-154">Other tabs in hello Spark UI provide useful information about hello Spark instance as well.</span></span>
   
   * <span data-ttu-id="a7fa3-155">Tárolási lap – Ha az alkalmazás létrehoz egy RDDs, akkor további információt talál hello tárolás fülön lévőket.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-155">Storage tab - If your application creates an RDDs, you can find information about those in hello Storage tab.</span></span>
   * <span data-ttu-id="a7fa3-156">Környezet lap – ezen a lapon számos hasznos információt a Spark-példányban, például a hello</span><span class="sxs-lookup"><span data-stu-id="a7fa3-156">Environment tab - This tab provides a lot of useful information about your Spark instance such as hello</span></span> 
     * <span data-ttu-id="a7fa3-157">Scala verzió</span><span class="sxs-lookup"><span data-stu-id="a7fa3-157">Scala version</span></span>
     * <span data-ttu-id="a7fa3-158">Hello fürt társított Eseménynapló könyvtár</span><span class="sxs-lookup"><span data-stu-id="a7fa3-158">Event log directory associated with hello cluster</span></span>
     * <span data-ttu-id="a7fa3-159">Hello alkalmazás végrehajtó magok száma</span><span class="sxs-lookup"><span data-stu-id="a7fa3-159">Number of executor cores for hello application</span></span>
     * <span data-ttu-id="a7fa3-160">Stb.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-160">Etc.</span></span>

## <a name="find-information-about-completed-jobs-using-hello-spark-history-server"></a><span data-ttu-id="a7fa3-161">Befejezett feladatokhoz hello Spark előzmények Server használatával kapcsolatos információk megkereséséhez</span><span class="sxs-lookup"><span data-stu-id="a7fa3-161">Find information about completed jobs using hello Spark History Server</span></span>
<span data-ttu-id="a7fa3-162">Ha egy feladat befejeződött, hello feladat hello információ hello Spark előzmények Server őrzi meg van.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-162">Once a job is completed, hello information about hello job is persisted in hello Spark History Server.</span></span>

1. <span data-ttu-id="a7fa3-163">toolaunch hello Spark előzmények Server, a hello-fürt panelén kattintson **fürt irányítópult**, és kattintson a **Spark előzmények Server**.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-163">toolaunch hello Spark History Server, from hello cluster blade, click **Cluster Dashboard**, and then click **Spark History Server**.</span></span>
   
    ![Indítsa el a Spark előzmények kiszolgáló](./media/hdinsight-apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]
   > <span data-ttu-id="a7fa3-165">Másik lehetőségként is elindíthatja a hello Spark előzmények kiszolgáló felhasználói Felületét a hello Ambari felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-165">Alternatively, you can also launch hello Spark History Server UI from hello Ambari UI.</span></span> <span data-ttu-id="a7fa3-166">toolaunch hello Ambari felhasználói felületén, a hello-fürt panelén kattintson **fürt irányítópult**, és kattintson a **HDInsight fürt irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-166">toolaunch hello Ambari UI, from hello cluster blade, click **Cluster Dashboard**, and then click **HDInsight Cluster Dashboard**.</span></span> <span data-ttu-id="a7fa3-167">A hello Ambari felhasználói felület, kattintson az **Spark**, kattintson a **Gyorshivatkozások**, és kattintson a **Spark előzmények kiszolgáló felhasználói felülete**.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-167">From hello Ambari UI, click **Spark**, click **Quick Links**, and then click **Spark History Server UI**.</span></span>
   > 
   > 
2. <span data-ttu-id="a7fa3-168">Minden felsorolt hello befejeződött alkalmazásokkal jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-168">You will see all hello completed applications listed.</span></span> <span data-ttu-id="a7fa3-169">Kattintson az alkalmazás azonosítója toodrill le egy alkalmazásba, további információért.</span><span class="sxs-lookup"><span data-stu-id="a7fa3-169">Click an application ID toodrill down into an application for more info.</span></span>
   
    ![Indítsa el a Spark előzmények kiszolgáló](./media/hdinsight-apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a><span data-ttu-id="a7fa3-171">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a7fa3-171">See also</span></span>
*  [<span data-ttu-id="a7fa3-172">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="a7fa3-172">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)

### <a name="for-data-analysts"></a><span data-ttu-id="a7fa3-173">Az adatok elemző</span><span class="sxs-lookup"><span data-stu-id="a7fa3-173">For data analysts</span></span>

* [<span data-ttu-id="a7fa3-174">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="a7fa3-174">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="a7fa3-175">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="a7fa3-175">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="a7fa3-176">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="a7fa3-176">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [<span data-ttu-id="a7fa3-177">Az Application Insights telemetriai adatainak elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="a7fa3-177">Application Insight telemetry data analysis using Spark in HDInsight</span></span>](hdinsight-spark-analyze-application-insight-logs.md)
* [<span data-ttu-id="a7fa3-178">Az Azure HDInsight Spark Caffe elosztott mély tanulási használata</span><span class="sxs-lookup"><span data-stu-id="a7fa3-178">Use Caffe on Azure HDInsight Spark for distributed deep learning</span></span>](hdinsight-deep-learning-caffe-spark.md)

### <a name="for-spark-developers"></a><span data-ttu-id="a7fa3-179">A Spark-fejlesztőknek</span><span class="sxs-lookup"><span data-stu-id="a7fa3-179">For Spark developers</span></span>

* [<span data-ttu-id="a7fa3-180">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="a7fa3-180">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="a7fa3-181">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="a7fa3-181">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)
* [<span data-ttu-id="a7fa3-182">Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="a7fa3-182">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="a7fa3-183">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="a7fa3-183">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="a7fa3-184">IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni</span><span class="sxs-lookup"><span data-stu-id="a7fa3-184">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="a7fa3-185">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="a7fa3-185">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="a7fa3-186">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="a7fa3-186">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="a7fa3-187">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="a7fa3-187">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="a7fa3-188">Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="a7fa3-188">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)


