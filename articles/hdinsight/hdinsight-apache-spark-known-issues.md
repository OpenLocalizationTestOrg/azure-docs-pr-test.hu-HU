---
title: "Az Azure HDInsight az Apache Spark-fürt elhárítása |} Microsoft Docs"
description: "További tudnivalók az Apache Spark on Azure HDInsight és azok megkerülő fürtökkel kapcsolatos problémák."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 610c4103-ffc8-4ec0-ad06-fdaf3c4d7c10
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 3a493a2c35a6cdd31bb1e4ff66113a8f8d97d4f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a><span data-ttu-id="b5b6d-103">A HDInsight az Apache Spark-fürt kapcsolatos ismert problémák</span><span class="sxs-lookup"><span data-stu-id="b5b6d-103">Known issues for Apache Spark cluster on HDInsight</span></span>

<span data-ttu-id="b5b6d-104">Ez a dokumentum nyomon követi az összes ismert problémák a HDInsight Spark nyilvános előzetes verzióhoz.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-104">This document keeps track of all the known issues for the HDInsight Spark public preview.</span></span>  

## <a name="livy-leaks-interactive-session"></a><span data-ttu-id="b5b6d-105">Livy szivárgást interaktív munkamenet</span><span class="sxs-lookup"><span data-stu-id="b5b6d-105">Livy leaks interactive session</span></span>
<span data-ttu-id="b5b6d-106">Újraindításakor Livy (az Ambari, illetve a virtuális gép újraindítás headnode 0) egy interaktív munkamenet-életben egy interaktív feladat munkamenet szivárgását okozhatja.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-106">When Livy is restarted (from Ambari or due to headnode 0 virtual machine reboot) with an interactive session still alive, an interactive job session will be leaked.</span></span> <span data-ttu-id="b5b6d-107">Emiatt az új feladatok elfogadott állapotban ragadt is, és nem lehet elindítani.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-107">Because of this, new jobs can stuck in the Accepted state, and cannot be started.</span></span>

<span data-ttu-id="b5b6d-108">**Megoldás:**</span><span class="sxs-lookup"><span data-stu-id="b5b6d-108">**Mitigation:**</span></span>

<span data-ttu-id="b5b6d-109">Az alábbi eljárással a probléma megoldása:</span><span class="sxs-lookup"><span data-stu-id="b5b6d-109">Use the following procedure to workaround the issue:</span></span>

1. <span data-ttu-id="b5b6d-110">Ssh headnode be.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-110">Ssh into headnode.</span></span> <span data-ttu-id="b5b6d-111">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="b5b6d-111">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="b5b6d-112">A következő parancsot a Livy keresztül elindított interaktív feladatokat az alkalmazás azonosítóit kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-112">Run the following command to find the application IDs of the interactive jobs started through Livy.</span></span> 
   
        yarn application –list
   
    <span data-ttu-id="b5b6d-113">Alapértelmezett kezdjen a feladat nevét kell-e a Livy, ha a feladat lett elindítva a Livy interaktív munkamenethez nincs explicit névvel megadva, a Livy munkamenet Jupyter notebook indította, a feladat nevére a remotesparkmagics_ *.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-113">The default job names will be Livy if the jobs were started with a Livy interactive session with no explicit names specified, For the Livy session started by Jupyter notebook, the job name will start with remotesparkmagics_*.</span></span> 
3. <span data-ttu-id="b5b6d-114">A következő parancsot a kill ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-114">Run the following command to kill those jobs.</span></span> 
   
        yarn application –kill <Application ID>

<span data-ttu-id="b5b6d-115">Új feladat futtatása elindul.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-115">New jobs will start running.</span></span> 

## <a name="spark-history-server-not-started"></a><span data-ttu-id="b5b6d-116">Spark előzmények kiszolgáló nem indult el</span><span class="sxs-lookup"><span data-stu-id="b5b6d-116">Spark History Server not started</span></span>
<span data-ttu-id="b5b6d-117">Spark előzmények kiszolgáló nem automatikusan elindul a fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-117">Spark History Server is not started automatically after a cluster is created.</span></span>  

<span data-ttu-id="b5b6d-118">**Megoldás:**</span><span class="sxs-lookup"><span data-stu-id="b5b6d-118">**Mitigation:**</span></span> 

<span data-ttu-id="b5b6d-119">Manuálisan indítsa el az előzmények server Ambari.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-119">Manually start the history server from Ambari.</span></span>

## <a name="permission-issue-in-spark-log-directory"></a><span data-ttu-id="b5b6d-120">A Spark naplókönyvtár engedély probléma</span><span class="sxs-lookup"><span data-stu-id="b5b6d-120">Permission issue in Spark log directory</span></span>
<span data-ttu-id="b5b6d-121">Amikor hdiuser spark-submit egy feladatot ad meg, nincs-e egy hiba java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (engedély megtagadva) és az illesztőprogram-napló nem készül.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-121">When hdiuser submits a job with spark-submit, there is an error java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (Permission denied) and the driver log is not written.</span></span> 

<span data-ttu-id="b5b6d-122">**Megoldás:**</span><span class="sxs-lookup"><span data-stu-id="b5b6d-122">**Mitigation:**</span></span>

1. <span data-ttu-id="b5b6d-123">Hdiuser hozzáadása a Hadoop-csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-123">Add hdiuser to the Hadoop group.</span></span> 
2. <span data-ttu-id="b5b6d-124">Adja meg a 777 engedélyek /var/log/spark a fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-124">Provide 777 permissions on /var/log/spark after cluster creation.</span></span> 
3. <span data-ttu-id="b5b6d-125">Frissítse az Ambari segítségével lehet 777 engedélyekkel könyvtár spark napló helyét.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-125">Update the spark log location using Ambari to be a directory with 777 permissions.</span></span>  
4. <span data-ttu-id="b5b6d-126">Futtassa a spark-nyújt, sudo.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-126">Run spark-submit as sudo.</span></span>  

## <a name="spark-phoenix-connector-is-not-supported"></a><span data-ttu-id="b5b6d-127">A Spark-Phoenix összekötő nem támogatott</span><span class="sxs-lookup"><span data-stu-id="b5b6d-127">Spark-Phoenix connector is not supported</span></span>

<span data-ttu-id="b5b6d-128">A Spark-Phoenix összekötő egy HDInsight Spark-fürt nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-128">Currently, the Spark-Phoenix connector is not supported with an HDInsight Spark cluster.</span></span>

<span data-ttu-id="b5b6d-129">**Megoldás:**</span><span class="sxs-lookup"><span data-stu-id="b5b6d-129">**Mitigation:**</span></span>

<span data-ttu-id="b5b6d-130">A Spark-HBase-összekötő kell helyette használni.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-130">You must use the Spark-HBase connector instead.</span></span> <span data-ttu-id="b5b6d-131">Útmutatásért lásd: [használata Spark-HBase-összekötő](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).</span><span class="sxs-lookup"><span data-stu-id="b5b6d-131">For instructions see [How to use Spark-HBase connector](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).</span></span>

## <a name="issues-related-to-jupyter-notebooks"></a><span data-ttu-id="b5b6d-132">Jupyter notebookok kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="b5b6d-132">Issues related to Jupyter notebooks</span></span>
<span data-ttu-id="b5b6d-133">Az alábbiakban néhány Jupyter notebookok kapcsolatos ismert problémák.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-133">Following are some known issues related to Jupyter notebooks.</span></span>

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a><span data-ttu-id="b5b6d-134">A fájlnevek nem ASCII-karaktereket notebookokban</span><span class="sxs-lookup"><span data-stu-id="b5b6d-134">Notebooks with non-ASCII characters in filenames</span></span>
<span data-ttu-id="b5b6d-135">A Spark HDInsight-fürtökkel használt Jupyter notebookok fájlnevekben nem rendelkezhet nem ASCII-karaktereket.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-135">Jupyter notebooks that can be used in Spark HDInsight clusters should not have non-ASCII characters in filenames.</span></span> <span data-ttu-id="b5b6d-136">Ha megpróbálja feltölteni a fájlt a Jupyter felhasználói felületen, amelynek a nem ASCII-fájl nevét, meghiúsul csendes (Ez azt jelenti, hogy Jupyter nem teszi lehetővé, hogy a fájl feltöltése, de a vagy azt nem throw látható hiba).</span><span class="sxs-lookup"><span data-stu-id="b5b6d-136">If you try to upload a file through the Jupyter UI which has a non-ASCII filename, it will fail silently (that is, Jupyter won’t let you upload the file, but it won’t throw a visible error either).</span></span> 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a><span data-ttu-id="b5b6d-137">Nagyobb méretű notebookok betöltése közben hiba</span><span class="sxs-lookup"><span data-stu-id="b5b6d-137">Error while loading notebooks of larger sizes</span></span>
<span data-ttu-id="b5b6d-138">Láthatja, hogy hiba  **`Error loading notebook`**  Ha nagyobb méretű notebookok tölthető be.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-138">You might see an error **`Error loading notebook`** when you load notebooks that are larger in size.</span></span>  

<span data-ttu-id="b5b6d-139">**Megoldás:**</span><span class="sxs-lookup"><span data-stu-id="b5b6d-139">**Mitigation:**</span></span>

<span data-ttu-id="b5b6d-140">Ha ez a hibaüzenet azt jelenti az adatok elveszett vagy sérült.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-140">If you get this error, it does not mean your data is corrupt or lost.</span></span>  <span data-ttu-id="b5b6d-141">Továbbra is a lemezen vannak jegyzetfüzetek `/var/lib/jupyter`, és azok eléréséhez a fürthöz SSH is.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-141">Your notebooks are still on disk in `/var/lib/jupyter`, and you can SSH into the cluster to access them.</span></span> <span data-ttu-id="b5b6d-142">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="b5b6d-142">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="b5b6d-143">Miután csatlakozott az SSH-fürtjéhez, átmásolhatja a jegyzetfüzetek a fürt a helyi számítógépen (SCP vagy WinSCP használatával) biztonsági mentéséhez a fontos adatokról a notebook az adatvesztés elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-143">Once you have connected to the cluster using SSH, you can copy your notebooks from your cluster to your local machine (using SCP or WinSCP) as a backup to prevent the loss of any important data in the notebook.</span></span> <span data-ttu-id="b5b6d-144">Ezek közül SSH-alagút azokat a headnode porton 8001 Jupyter eléréséhez az átjárón keresztül nélkül.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-144">You can then SSH tunnel into your headnode at port 8001 to access Jupyter without going through the gateway.</span></span>  <span data-ttu-id="b5b6d-145">Ott törölje a notebook kimenetét, és mentse újra a notebook méretének minimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-145">From there, you can clear the output of your notebook and re-save it to minimize the notebook’s size.</span></span>

<span data-ttu-id="b5b6d-146">Ez a hiba megakadályozza a jövőben történik, kövesse az néhány ajánlott eljárás:</span><span class="sxs-lookup"><span data-stu-id="b5b6d-146">To prevent this error from happening in the future, you must follow some best practices:</span></span>

* <span data-ttu-id="b5b6d-147">Fontos, hogy maradjon kicsi a notebook méretét.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-147">It is important to keep the notebook size small.</span></span> <span data-ttu-id="b5b6d-148">A Spark feladatok bármely olyan kimenete, amely küld vissza a Jupyter van őrzi meg a notebook.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-148">Any output from your Spark jobs that is sent back to Jupyter is persisted in the notebook.</span></span>  <span data-ttu-id="b5b6d-149">A legjobb Jupyter általában futó elkerülése érdekében `.collect()` a nagy RDD vagy dataframes; helyette, ha szeretné bepillanthat, hogy egy RDD tartalmát, fontolja meg a futó `.take()` vagy `.sample()` , hogy a kimenet nem get túl nagy.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-149">It is a best practice with Jupyter in general to avoid running `.collect()` on large RDD’s or dataframes; instead, if you want to peek at an RDD’s contents, consider running `.take()` or `.sample()` so that your output doesn’t get too big.</span></span>
* <span data-ttu-id="b5b6d-150">Is a notebook mentésekor törölje az összes kimeneti cellák méretének csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-150">Also, when you save a notebook, clear all output cells to reduce the size.</span></span>

### <a name="notebook-initial-startup-takes-longer-than-expected"></a><span data-ttu-id="b5b6d-151">Kezdeti indítási notebook vártnál hosszabb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-151">Notebook initial startup takes longer than expected</span></span>
<span data-ttu-id="b5b6d-152">Jupyter notebook használatával Spark magic utasításnak elsőként kód több mint egy percbe is beletelhet.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-152">First code statement in Jupyter notebook using Spark magic could take more than a minute.</span></span>  

<span data-ttu-id="b5b6d-153">**Magyarázat:**</span><span class="sxs-lookup"><span data-stu-id="b5b6d-153">**Explanation:**</span></span>

<span data-ttu-id="b5b6d-154">Ez akkor fordul elő, mert az első kódcella futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-154">This happens because when the first code cell is run.</span></span> <span data-ttu-id="b5b6d-155">A háttérben kezdeményez a munkamenet-konfigurációhoz és Spark, SQL, és a Hive-környezeteket.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-155">In the background this initiates session configuration and Spark, SQL, and Hive contexts are set.</span></span> <span data-ttu-id="b5b6d-156">Miután ezek a környezetek vannak beállítva, az első utasításban fut, és ezáltal a benyomást, amely az utasítás hosszú időt vett igénybe befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-156">After these contexts are set, the first statement is run and this gives the impression that the statement took a long time to complete.</span></span>

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a><span data-ttu-id="b5b6d-157">Jupyter notebook időkorlátot adja meg a munkamenet létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5b6d-157">Jupyter notebook timeout in creating the session</span></span>
<span data-ttu-id="b5b6d-158">Amikor Spark-fürt kifogyott az erőforrásokból, a Jupyter notebook a Spark- és Pyspark kernelek fog időtúllépés történt a munkamenet létrehozása közben.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-158">When Spark cluster is out of resources, the Spark and Pyspark kernels in the Jupyter notebook will timeout trying to create the session.</span></span> 

<span data-ttu-id="b5b6d-159">**Megoldást:**</span><span class="sxs-lookup"><span data-stu-id="b5b6d-159">**Mitigations:**</span></span> 

1. <span data-ttu-id="b5b6d-160">Szabadítson fel a Spark-fürt egyes erőforrások:</span><span class="sxs-lookup"><span data-stu-id="b5b6d-160">Free up some resources in your Spark cluster by:</span></span>
   
   * <span data-ttu-id="b5b6d-161">Egyéb külső notebookok leállítása a zárja be és Halt menü címen, vagy kattintson a leállítás a notebook Explorer.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-161">Stopping other Spark notebooks by going to the Close and Halt menu or clicking Shutdown in the notebook explorer.</span></span>
   * <span data-ttu-id="b5b6d-162">A YARN más Spark-alkalmazások leállítása.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-162">Stopping other Spark applications from YARN.</span></span>
2. <span data-ttu-id="b5b6d-163">Indítsa újra a notebook kívánt elindításához.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-163">Restart the notebook you were trying to start up.</span></span> <span data-ttu-id="b5b6d-164">Elegendő erőforrást ahhoz, hogy hozzon létre most egy munkamenet elérhetőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b5b6d-164">Enough resources should be available for you to create a session now.</span></span>

## <a name="see-also"></a><span data-ttu-id="b5b6d-165">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="b5b6d-165">See also</span></span>
* [<span data-ttu-id="b5b6d-166">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="b5b6d-166">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="b5b6d-167">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="b5b6d-167">Scenarios</span></span>
* [<span data-ttu-id="b5b6d-168">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="b5b6d-168">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="b5b6d-169">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="b5b6d-169">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="b5b6d-170">Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére</span><span class="sxs-lookup"><span data-stu-id="b5b6d-170">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="b5b6d-171">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="b5b6d-171">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="b5b6d-172">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="b5b6d-172">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="b5b6d-173">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="b5b6d-173">Create and run applications</span></span>
* [<span data-ttu-id="b5b6d-174">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="b5b6d-174">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="b5b6d-175">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="b5b6d-175">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="b5b6d-176">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="b5b6d-176">Tools and extensions</span></span>
* [<span data-ttu-id="b5b6d-177">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark Scala-alkalmazások létrehozásához és elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="b5b6d-177">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="b5b6d-178">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark-alkalmazások távoli hibaelhárításához</span><span class="sxs-lookup"><span data-stu-id="b5b6d-178">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="b5b6d-179">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="b5b6d-179">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="b5b6d-180">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="b5b6d-180">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="b5b6d-181">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="b5b6d-181">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="b5b6d-182">A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz</span><span class="sxs-lookup"><span data-stu-id="b5b6d-182">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="b5b6d-183">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="b5b6d-183">Manage resources</span></span>
* [<span data-ttu-id="b5b6d-184">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="b5b6d-184">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="b5b6d-185">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="b5b6d-185">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

