---
title: "aaaTroubleshoot problémákat az Apache Spark on Azure hdinsight fürt |} Microsoft Docs"
description: "Az Azure HDInsight Spark-fürtjei kapcsolódó tooApache problémák megismerése és hogyan toowork körül azokat."
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
ms.openlocfilehash: 7373b90524ae5dbb10ab8ded593aa38d12c14b55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a><span data-ttu-id="c1a6e-103">A HDInsight az Apache Spark-fürt kapcsolatos ismert problémák</span><span class="sxs-lookup"><span data-stu-id="c1a6e-103">Known issues for Apache Spark cluster on HDInsight</span></span>

<span data-ttu-id="c1a6e-104">Ez a dokumentum nyomon követi az összes hello ismert problémái hello HDInsight Spark nyilvános előzetes verziójához.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-104">This document keeps track of all hello known issues for hello HDInsight Spark public preview.</span></span>  

## <a name="livy-leaks-interactive-session"></a><span data-ttu-id="c1a6e-105">Livy szivárgást interaktív munkamenet</span><span class="sxs-lookup"><span data-stu-id="c1a6e-105">Livy leaks interactive session</span></span>
<span data-ttu-id="c1a6e-106">Újraindításakor Livy (az Ambari vagy tooheadnode 0 virtuális gép újraindítása miatt) egy interaktív munkamenet-életben egy interaktív feladat munkamenet szivárgását okozhatja.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-106">When Livy is restarted (from Ambari or due tooheadnode 0 virtual machine reboot) with an interactive session still alive, an interactive job session will be leaked.</span></span> <span data-ttu-id="c1a6e-107">Emiatt az új feladatok is hello elfogadott állapotban ragadt, és nem indítható el.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-107">Because of this, new jobs can stuck in hello Accepted state, and cannot be started.</span></span>

<span data-ttu-id="c1a6e-108">**Megoldás:**</span><span class="sxs-lookup"><span data-stu-id="c1a6e-108">**Mitigation:**</span></span>

<span data-ttu-id="c1a6e-109">A következő eljárás tooworkaround hello probléma hello használata:</span><span class="sxs-lookup"><span data-stu-id="c1a6e-109">Use hello following procedure tooworkaround hello issue:</span></span>

1. <span data-ttu-id="c1a6e-110">Ssh headnode be.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-110">Ssh into headnode.</span></span> <span data-ttu-id="c1a6e-111">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="c1a6e-111">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="c1a6e-112">Futtassa a következő parancs toofind hello alkalmazás hello azonosítók hello interaktív feladatok elindítása Livy használatával.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-112">Run hello following command toofind hello application IDs of hello interactive jobs started through Livy.</span></span> 
   
        yarn application –list
   
    <span data-ttu-id="c1a6e-113">hello alapértelmezett feladat neve lesz Livy, ha nincs megadva, a hello explicit nevekkel hello feladatok indított egy Livy interaktív munkamenet Livy munkamenet Jupyter notebook indította, hello feladatnév indul rendelkező remotesparkmagics_ *.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-113">hello default job names will be Livy if hello jobs were started with a Livy interactive session with no explicit names specified, For hello Livy session started by Jupyter notebook, hello job name will start with remotesparkmagics_*.</span></span> 
3. <span data-ttu-id="c1a6e-114">Futtassa a következő parancs tookill hello ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-114">Run hello following command tookill those jobs.</span></span> 
   
        yarn application –kill <Application ID>

<span data-ttu-id="c1a6e-115">Új feladat futtatása elindul.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-115">New jobs will start running.</span></span> 

## <a name="spark-history-server-not-started"></a><span data-ttu-id="c1a6e-116">Spark előzmények kiszolgáló nem indult el</span><span class="sxs-lookup"><span data-stu-id="c1a6e-116">Spark History Server not started</span></span>
<span data-ttu-id="c1a6e-117">Spark előzmények kiszolgáló nem automatikusan elindul a fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-117">Spark History Server is not started automatically after a cluster is created.</span></span>  

<span data-ttu-id="c1a6e-118">**Megoldás:**</span><span class="sxs-lookup"><span data-stu-id="c1a6e-118">**Mitigation:**</span></span> 

<span data-ttu-id="c1a6e-119">Manuálisan indítsa el az Ambari hello előzmények kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-119">Manually start hello history server from Ambari.</span></span>

## <a name="permission-issue-in-spark-log-directory"></a><span data-ttu-id="c1a6e-120">A Spark naplókönyvtár engedély probléma</span><span class="sxs-lookup"><span data-stu-id="c1a6e-120">Permission issue in Spark log directory</span></span>
<span data-ttu-id="c1a6e-121">Amikor hdiuser spark-submit egy feladatot ad meg, nincs-e egy hiba java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (engedély megtagadva) és hello illesztőprogram napló nem készül.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-121">When hdiuser submits a job with spark-submit, there is an error java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (Permission denied) and hello driver log is not written.</span></span> 

<span data-ttu-id="c1a6e-122">**Megoldás:**</span><span class="sxs-lookup"><span data-stu-id="c1a6e-122">**Mitigation:**</span></span>

1. <span data-ttu-id="c1a6e-123">Hdiuser toohello Hadoop csoport hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-123">Add hdiuser toohello Hadoop group.</span></span> 
2. <span data-ttu-id="c1a6e-124">Adja meg a 777 engedélyek /var/log/spark a fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-124">Provide 777 permissions on /var/log/spark after cluster creation.</span></span> 
3. <span data-ttu-id="c1a6e-125">Frissítési hello spark napló helyét Ambari toobe könyvtár használatával 777 engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-125">Update hello spark log location using Ambari toobe a directory with 777 permissions.</span></span>  
4. <span data-ttu-id="c1a6e-126">Futtassa a spark-nyújt, sudo.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-126">Run spark-submit as sudo.</span></span>  

## <a name="spark-phoenix-connector-is-not-supported"></a><span data-ttu-id="c1a6e-127">A Spark-Phoenix összekötő nem támogatott</span><span class="sxs-lookup"><span data-stu-id="c1a6e-127">Spark-Phoenix connector is not supported</span></span>

<span data-ttu-id="c1a6e-128">Hello Spark-Phoenix összekötő egy HDInsight Spark-fürt nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-128">Currently, hello Spark-Phoenix connector is not supported with an HDInsight Spark cluster.</span></span>

<span data-ttu-id="c1a6e-129">**Megoldás:**</span><span class="sxs-lookup"><span data-stu-id="c1a6e-129">**Mitigation:**</span></span>

<span data-ttu-id="c1a6e-130">Hello Spark-HBase összekötő kell helyette használni.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-130">You must use hello Spark-HBase connector instead.</span></span> <span data-ttu-id="c1a6e-131">Útmutatásért lásd: [hogyan toouse Spark-HBase-összekötő](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).</span><span class="sxs-lookup"><span data-stu-id="c1a6e-131">For instructions see [How toouse Spark-HBase connector](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).</span></span>

## <a name="issues-related-toojupyter-notebooks"></a><span data-ttu-id="c1a6e-132">Kapcsolódó problémák tooJupyter notebookok</span><span class="sxs-lookup"><span data-stu-id="c1a6e-132">Issues related tooJupyter notebooks</span></span>
<span data-ttu-id="c1a6e-133">Az alábbiakban néhány ismert problémák kapcsolódó tooJupyter notebookok.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-133">Following are some known issues related tooJupyter notebooks.</span></span>

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a><span data-ttu-id="c1a6e-134">A fájlnevek nem ASCII-karaktereket notebookokban</span><span class="sxs-lookup"><span data-stu-id="c1a6e-134">Notebooks with non-ASCII characters in filenames</span></span>
<span data-ttu-id="c1a6e-135">A Spark HDInsight-fürtökkel használt Jupyter notebookok fájlnevekben nem rendelkezhet nem ASCII-karaktereket.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-135">Jupyter notebooks that can be used in Spark HDInsight clusters should not have non-ASCII characters in filenames.</span></span> <span data-ttu-id="c1a6e-136">Ha tooupload hello Jupyter felhasználói felület, amely a nem ASCII-fájl nevét a fájlban, akkor sikertelen lesz csendes (Ez azt jelenti, hogy Jupyter nem engedi hello fájl feltöltéséhez, de a vagy azt nem throw látható hiba).</span><span class="sxs-lookup"><span data-stu-id="c1a6e-136">If you try tooupload a file through hello Jupyter UI which has a non-ASCII filename, it will fail silently (that is, Jupyter won’t let you upload hello file, but it won’t throw a visible error either).</span></span> 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a><span data-ttu-id="c1a6e-137">Nagyobb méretű notebookok betöltése közben hiba</span><span class="sxs-lookup"><span data-stu-id="c1a6e-137">Error while loading notebooks of larger sizes</span></span>
<span data-ttu-id="c1a6e-138">Láthatja, hogy hiba ** `Error loading notebook` ** Ha nagyobb méretű notebookok tölthető be.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-138">You might see an error **`Error loading notebook`** when you load notebooks that are larger in size.</span></span>  

<span data-ttu-id="c1a6e-139">**Megoldás:**</span><span class="sxs-lookup"><span data-stu-id="c1a6e-139">**Mitigation:**</span></span>

<span data-ttu-id="c1a6e-140">Ha ez a hibaüzenet azt jelenti az adatok elveszett vagy sérült.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-140">If you get this error, it does not mean your data is corrupt or lost.</span></span>  <span data-ttu-id="c1a6e-141">Továbbra is a lemezen vannak jegyzetfüzetek `/var/lib/jupyter`, ráadásul SSH hello fürt tooaccess be őket.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-141">Your notebooks are still on disk in `/var/lib/jupyter`, and you can SSH into hello cluster tooaccess them.</span></span> <span data-ttu-id="c1a6e-142">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="c1a6e-142">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="c1a6e-143">Ha SSH használatával toohello fürt csatlakozott, átmásolhatja a jegyzetfüzetek a fürt tooyour helyi számítógép (SCP vagy WinSCP használatával) hello notebook a fontos adatok biztonsági mentés tooprevent hello veszteségként.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-143">Once you have connected toohello cluster using SSH, you can copy your notebooks from your cluster tooyour local machine (using SCP or WinSCP) as a backup tooprevent hello loss of any important data in hello notebook.</span></span> <span data-ttu-id="c1a6e-144">Ezek közül SSH-alagút be a következő port 8001 tooaccess Jupyter headnode hello átjáró áthaladás nélkül.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-144">You can then SSH tunnel into your headnode at port 8001 tooaccess Jupyter without going through hello gateway.</span></span>  <span data-ttu-id="c1a6e-145">Ezekből a notebook hello kimenete törölje és mentse újra toominimize hello jegyzetfüzet méretét.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-145">From there, you can clear hello output of your notebook and re-save it toominimize hello notebook’s size.</span></span>

<span data-ttu-id="c1a6e-146">tooprevent Ez a hiba a jövőbeli, hajtsa végre az tanácsokat hello le:</span><span class="sxs-lookup"><span data-stu-id="c1a6e-146">tooprevent this error from happening in hello future, you must follow some best practices:</span></span>

* <span data-ttu-id="c1a6e-147">Fontos tookeep hello notebook kis méretű legyen.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-147">It is important tookeep hello notebook size small.</span></span> <span data-ttu-id="c1a6e-148">Bármely olyan kimenete a Spark feladatok küldött vissza tooJupyter hello jegyzetfüzet megőrződjenek.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-148">Any output from your Spark jobs that is sent back tooJupyter is persisted in hello notebook.</span></span>  <span data-ttu-id="c1a6e-149">Az ajánlott eljárás a Jupyter futtató általános tooavoid `.collect()` a nagy RDD vagy dataframes; helyette, ha azt szeretné, hogy egy RDD tartalmát, toopeek, fontolja meg a futó `.take()` vagy `.sample()` , hogy a kimenet nem get túl nagy.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-149">It is a best practice with Jupyter in general tooavoid running `.collect()` on large RDD’s or dataframes; instead, if you want toopeek at an RDD’s contents, consider running `.take()` or `.sample()` so that your output doesn’t get too big.</span></span>
* <span data-ttu-id="c1a6e-150">Is a notebook mentésekor törölje az összes kimeneti cellák tooreduce hello méretét.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-150">Also, when you save a notebook, clear all output cells tooreduce hello size.</span></span>

### <a name="notebook-initial-startup-takes-longer-than-expected"></a><span data-ttu-id="c1a6e-151">Kezdeti indítási notebook vártnál hosszabb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-151">Notebook initial startup takes longer than expected</span></span>
<span data-ttu-id="c1a6e-152">Jupyter notebook használatával Spark magic utasításnak elsőként kód több mint egy percbe is beletelhet.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-152">First code statement in Jupyter notebook using Spark magic could take more than a minute.</span></span>  

<span data-ttu-id="c1a6e-153">**Magyarázat:**</span><span class="sxs-lookup"><span data-stu-id="c1a6e-153">**Explanation:**</span></span>

<span data-ttu-id="c1a6e-154">Ez akkor fordul elő, mert hello első kódcella futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-154">This happens because when hello first code cell is run.</span></span> <span data-ttu-id="c1a6e-155">Hello háttérben kezdeményez a munkamenet-konfigurációhoz és Spark, SQL, és a Hive-környezeteket.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-155">In hello background this initiates session configuration and Spark, SQL, and Hive contexts are set.</span></span> <span data-ttu-id="c1a6e-156">Miután ezek a környezetek van beállítva, hello első utasítása fut, és így az, hogy hello utasítás tartott egy hosszú ideig toocomplete hello benyomást.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-156">After these contexts are set, hello first statement is run and this gives hello impression that hello statement took a long time toocomplete.</span></span>

### <a name="jupyter-notebook-timeout-in-creating-hello-session"></a><span data-ttu-id="c1a6e-157">Jupyter notebook időtúllépés hello munkamenet létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1a6e-157">Jupyter notebook timeout in creating hello session</span></span>
<span data-ttu-id="c1a6e-158">Spark-fürt kifogyott az erőforrásokból, hello Spark és a a hello Jupyter notebook Pyspark kernel toocreate hello munkamenet közben időtúllépés lesz.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-158">When Spark cluster is out of resources, hello Spark and Pyspark kernels in hello Jupyter notebook will timeout trying toocreate hello session.</span></span> 

<span data-ttu-id="c1a6e-159">**Megoldást:**</span><span class="sxs-lookup"><span data-stu-id="c1a6e-159">**Mitigations:**</span></span> 

1. <span data-ttu-id="c1a6e-160">Szabadítson fel a Spark-fürt egyes erőforrások:</span><span class="sxs-lookup"><span data-stu-id="c1a6e-160">Free up some resources in your Spark cluster by:</span></span>
   
   * <span data-ttu-id="c1a6e-161">Egyéb külső notebookok toohello is zárja be és a Halt menüből, vagy kattintson a Leállítás hello notebook Explorer leállítása.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-161">Stopping other Spark notebooks by going toohello Close and Halt menu or clicking Shutdown in hello notebook explorer.</span></span>
   * <span data-ttu-id="c1a6e-162">A YARN más Spark-alkalmazások leállítása.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-162">Stopping other Spark applications from YARN.</span></span>
2. <span data-ttu-id="c1a6e-163">Indítsa újra a hello notebook toostart próbált fel.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-163">Restart hello notebook you were trying toostart up.</span></span> <span data-ttu-id="c1a6e-164">Erőforrásokkal elérhetőknek kell lenniük a akkor toocreate most egy munkamenet.</span><span class="sxs-lookup"><span data-stu-id="c1a6e-164">Enough resources should be available for you toocreate a session now.</span></span>

## <a name="see-also"></a><span data-ttu-id="c1a6e-165">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="c1a6e-165">See also</span></span>
* [<span data-ttu-id="c1a6e-166">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="c1a6e-166">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="c1a6e-167">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="c1a6e-167">Scenarios</span></span>
* [<span data-ttu-id="c1a6e-168">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="c1a6e-168">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="c1a6e-169">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="c1a6e-169">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="c1a6e-170">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="c1a6e-170">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="c1a6e-171">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="c1a6e-171">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="c1a6e-172">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="c1a6e-172">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="c1a6e-173">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="c1a6e-173">Create and run applications</span></span>
* [<span data-ttu-id="c1a6e-174">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="c1a6e-174">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="c1a6e-175">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="c1a6e-175">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="c1a6e-176">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="c1a6e-176">Tools and extensions</span></span>
* [<span data-ttu-id="c1a6e-177">Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala applicatons</span><span class="sxs-lookup"><span data-stu-id="c1a6e-177">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="c1a6e-178">IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni</span><span class="sxs-lookup"><span data-stu-id="c1a6e-178">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="c1a6e-179">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="c1a6e-179">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="c1a6e-180">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="c1a6e-180">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="c1a6e-181">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="c1a6e-181">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="c1a6e-182">Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="c1a6e-182">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="c1a6e-183">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="c1a6e-183">Manage resources</span></span>
* [<span data-ttu-id="c1a6e-184">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="c1a6e-184">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="c1a6e-185">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="c1a6e-185">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

