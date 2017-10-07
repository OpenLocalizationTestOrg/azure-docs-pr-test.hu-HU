---
title: "aaaTroubleshoot Spark on Azure HDInsight használatával |} Microsoft Docs"
description: "Válaszok az Apache Spark és az Azure HDInsight toocommon kérdésekre."
keywords: "Az Azure HDInsight Spark, gyakran ismételt kérdések, hibaelhárítási útmutató, gyakori problémákat, Alkalmazáskonfiguráció, Ambari"
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: 25D89586-DE5B-4268-B5D5-CC2CE12207ED
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: arijitt
ms.openlocfilehash: c9f910daf295462238a3143ae2589db6d383097f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-spark-by-using-azure-hdinsight"></a><span data-ttu-id="de398-104">Hibaelhárítás Spark on Azure HDInsight segítségével</span><span class="sxs-lookup"><span data-stu-id="de398-104">Troubleshoot Spark by using Azure HDInsight</span></span>

<span data-ttu-id="de398-105">Hello legfőbb problémákat és azok megoldásait ismerje meg az Apache Ambari az Apache Spark Payload van jelen használatakor.</span><span class="sxs-lookup"><span data-stu-id="de398-105">Learn about hello top issues and their resolutions when working with Apache Spark payloads in Apache Ambari.</span></span>

## <a name="how-do-i-configure-a-spark-application-by-using-ambari-on-clusters"></a><span data-ttu-id="de398-106">Hogyan konfigurálhatom a Spark-alkalmazások fürtök az Ambari használatával</span><span class="sxs-lookup"><span data-stu-id="de398-106">How do I configure a Spark application by using Ambari on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="de398-107">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="de398-107">Resolution steps</span></span>

<span data-ttu-id="de398-108">hello konfigurációs értékeket az eljárás végrehajtásához a Hdinsightban korábban beállított.</span><span class="sxs-lookup"><span data-stu-id="de398-108">hello configuration values for this procedure were previously set in HDInsight.</span></span> <span data-ttu-id="de398-109">toodetermine mely Spark konfigurációk kell toobe és a toowhat értékek, lásd: [mi okozza a Spark OutofMemoryError Alkalmazáskivétel](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="de398-109">toodetermine which Spark configurations need toobe set and toowhat values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span> 

1. <span data-ttu-id="de398-110">A fürtök hello listában válassza ki a **Spark2**.</span><span class="sxs-lookup"><span data-stu-id="de398-110">In hello list of clusters, select **Spark2**.</span></span>

    ![Válassza ki a fürtöt a listából](media/hdinsight-troubleshoot-spark/update-config-1.png)

2. <span data-ttu-id="de398-112">Jelölje be hello **Configs** fülre.</span><span class="sxs-lookup"><span data-stu-id="de398-112">Select hello **Configs** tab.</span></span>

    ![Válassza ki a hello Configs lap](media/hdinsight-troubleshoot-spark/update-config-2.png)

3. <span data-ttu-id="de398-114">A konfigurációk hello listában válassza ki a **egyéni-spark2-alapértelmezett**.</span><span class="sxs-lookup"><span data-stu-id="de398-114">In hello list of configurations, select **Custom-spark2-defaults**.</span></span>

    ![Válassza ki az egyéni-spark-alapértelmezései](media/hdinsight-troubleshoot-spark/update-config-3.png)

4. <span data-ttu-id="de398-116">Hello érték beállítása tooadjust, például a kell keresni **spark.executor.memory**.</span><span class="sxs-lookup"><span data-stu-id="de398-116">Look for hello value setting that you need tooadjust, such as **spark.executor.memory**.</span></span> <span data-ttu-id="de398-117">Ebben az esetben a értékének hello **4608m** túl nagy.</span><span class="sxs-lookup"><span data-stu-id="de398-117">In this case, hello value of **4608m** is too high.</span></span>

    ![Válassza ki a hello spark.executor.memory mező](media/hdinsight-troubleshoot-spark/update-config-4.png)

5. <span data-ttu-id="de398-119">Set hello érték toohello ajánlott beállítás.</span><span class="sxs-lookup"><span data-stu-id="de398-119">Set hello value toohello recommended setting.</span></span> <span data-ttu-id="de398-120">érték hello **2048m** ezt a beállítást ajánlott.</span><span class="sxs-lookup"><span data-stu-id="de398-120">hello value **2048m** is recommended for this setting.</span></span>

    ![Változás érték too2048m](media/hdinsight-troubleshoot-spark/update-config-5.png)

6. <span data-ttu-id="de398-122">Mentse hello értéket, és mentse a hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="de398-122">Save hello value, and then save hello configuration.</span></span> <span data-ttu-id="de398-123">Hello eszköztáron válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="de398-123">On hello toolbar, select **Save**.</span></span>

    ![Hello beállítás és konfiguráció mentése](media/hdinsight-troubleshoot-spark/update-config-6a.png)

    <span data-ttu-id="de398-125">Ha a konfigurációkat kezeléséről értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="de398-125">You are notified if any configurations need attention.</span></span> <span data-ttu-id="de398-126">Vegye figyelembe a hello elemeket, majd válassza ki **mégis folytatni**.</span><span class="sxs-lookup"><span data-stu-id="de398-126">Note hello items, and then select **Proceed Anyway**.</span></span> 

    ![Válassza ki mégis folytatni](media/hdinsight-troubleshoot-spark/update-config-6b.png)

    <span data-ttu-id="de398-128">Írjon megjegyzést hello konfigurációs módosításokat, majd válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="de398-128">Write a note about hello configuration changes, and then select **Save**.</span></span>

    ![Hello módosításokkal kapcsolatos megjegyzés](media/hdinsight-troubleshoot-spark/update-config-6c.png)

7. <span data-ttu-id="de398-130">Amikor egy konfigurációs mentésekor kéri toorestart hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="de398-130">Whenever a configuration is saved, you are prompted toorestart hello service.</span></span> <span data-ttu-id="de398-131">Válassza ki **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="de398-131">Select **Restart**.</span></span>

    ![Válassza ki az újraindítást](media/hdinsight-troubleshoot-spark/update-config-7a.png)

    <span data-ttu-id="de398-133">Erősítse meg a hello újraindítás.</span><span class="sxs-lookup"><span data-stu-id="de398-133">Confirm hello restart.</span></span>

    ![Válassza ki erősítse meg, indítsa újra az összes](media/hdinsight-troubleshoot-spark/update-config-7b.png)

    <span data-ttu-id="de398-135">Futó folyamatok hello tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="de398-135">You can review hello processes that are running.</span></span>

    ![Tekintse át a futó folyamatok](media/hdinsight-troubleshoot-spark/update-config-7c.png)

8. <span data-ttu-id="de398-137">Konfigurációk is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="de398-137">You can add configurations.</span></span> <span data-ttu-id="de398-138">A konfigurációk hello listában válassza ki a **egyéni-spark2-alapértelmezett**, majd válassza ki **tulajdonság hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="de398-138">In hello list of configurations, select **Custom-spark2-defaults**, and then select **Add Property**.</span></span>

    ![Válassza ki a tulajdonság hozzáadása](media/hdinsight-troubleshoot-spark/update-config-8.png)

9. <span data-ttu-id="de398-140">Adja meg az új tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="de398-140">Define a new property.</span></span> <span data-ttu-id="de398-141">Egy-egy tulajdonság az egyes beállítások, például hello adatok típusát egy párbeszédpanel segítségével adhat meg.</span><span class="sxs-lookup"><span data-stu-id="de398-141">You can define a single property by using a dialog box for specific settings such as hello data type.</span></span> <span data-ttu-id="de398-142">Vagy több tulajdonságok adhatók soronként egy definíciót használatával.</span><span class="sxs-lookup"><span data-stu-id="de398-142">Or, you can define multiple properties by using one definition per line.</span></span> 

    <span data-ttu-id="de398-143">Ebben a példában hello **spark.driver.memory** tulajdonságot definiálták, értékű **4g**.</span><span class="sxs-lookup"><span data-stu-id="de398-143">In this example, hello **spark.driver.memory** property is defined with a value of **4g**.</span></span>

    ![Új tulajdonság megadása](media/hdinsight-troubleshoot-spark/update-config-9.png)

10. <span data-ttu-id="de398-145">Hello konfiguráció mentéséhez, majd indítsa újra hello szolgáltatást, 6 és 7 lépésben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="de398-145">Save hello configuration, and then restart hello service as described in steps 6 and 7.</span></span>

<span data-ttu-id="de398-146">Ezek a változások fürt kiterjedő, de hello Spark feladat elküldése felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="de398-146">These changes are cluster-wide but can be overridden when you submit hello Spark job.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="de398-147">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="de398-147">Additional reading</span></span>

[<span data-ttu-id="de398-148">Spark feladat elküldése a HDInsight-fürtökön</span><span class="sxs-lookup"><span data-stu-id="de398-148">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters"></a><span data-ttu-id="de398-149">Hogyan konfigurálhatom a Spark-alkalmazások fürtökön Jupyter notebook használatával</span><span class="sxs-lookup"><span data-stu-id="de398-149">How do I configure a Spark application by using a Jupyter notebook on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="de398-150">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="de398-150">Resolution steps</span></span>

1. <span data-ttu-id="de398-151">toodetermine mely Spark konfigurációk kell toobe és a toowhat értékek, lásd: [mi okozza a Spark OutofMemoryError Alkalmazáskivétel](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="de398-151">toodetermine which Spark configurations need toobe set and toowhat values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span>

2. <span data-ttu-id="de398-152">Hello első cellájában hello Jupyter notebook hello után **%% konfigurálása** irányelv, adja meg a hello Spark konfigurációk érvényes JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="de398-152">In hello first cell of hello Jupyter notebook, after hello **%%configure** directive, specify hello Spark configurations in valid JSON format.</span></span> <span data-ttu-id="de398-153">Hello tényleges értékek módosítása szükséges:</span><span class="sxs-lookup"><span data-stu-id="de398-153">Change hello actual values as necessary:</span></span>

    ![A konfiguráció hozzáadása](media/hdinsight-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a><span data-ttu-id="de398-155">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="de398-155">Additional reading</span></span>

[<span data-ttu-id="de398-156">Spark feladat elküldése a HDInsight-fürtökön</span><span class="sxs-lookup"><span data-stu-id="de398-156">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-livy-on-clusters"></a><span data-ttu-id="de398-157">Hogyan konfigurálhatom a Spark-alkalmazások fürtökön Livy használatával</span><span class="sxs-lookup"><span data-stu-id="de398-157">How do I configure a Spark application by using Livy on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="de398-158">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="de398-158">Resolution steps</span></span>

1. <span data-ttu-id="de398-159">toodetermine mely Spark konfigurációk kell toobe és a toowhat értékek, lásd: [mi okozza a Spark OutofMemoryError Alkalmazáskivétel](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="de398-159">toodetermine which Spark configurations need toobe set and toowhat values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span> 

2. <span data-ttu-id="de398-160">Hello Spark alkalmazás tooLivy nyújt a REST-ügyfelek, például cURL használatával.</span><span class="sxs-lookup"><span data-stu-id="de398-160">Submit hello Spark application tooLivy by using a REST client like cURL.</span></span> <span data-ttu-id="de398-161">Használja a parancs hasonló toohello következő.</span><span class="sxs-lookup"><span data-stu-id="de398-161">Use a command similar toohello following.</span></span> <span data-ttu-id="de398-162">Hello tényleges értékek módosítása szükséges:</span><span class="sxs-lookup"><span data-stu-id="de398-162">Change hello actual values as necessary:</span></span>

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a><span data-ttu-id="de398-163">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="de398-163">Additional reading</span></span>

[<span data-ttu-id="de398-164">Spark feladat elküldése a HDInsight-fürtökön</span><span class="sxs-lookup"><span data-stu-id="de398-164">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters"></a><span data-ttu-id="de398-165">Hogyan konfigurálhatók a alkalmazás használatával spark-elküldeni egy Spark-fürtökön</span><span class="sxs-lookup"><span data-stu-id="de398-165">How do I configure a Spark application by using spark-submit on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="de398-166">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="de398-166">Resolution steps</span></span>

1. <span data-ttu-id="de398-167">toodetermine mely Spark konfigurációk kell toobe és a toowhat értékek, lásd: [mi okozza a Spark OutofMemoryError Alkalmazáskivétel](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="de398-167">toodetermine which Spark configurations need toobe set and toowhat values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span>

2. <span data-ttu-id="de398-168">Egy hasonló toohello következő parancs használatával indítsa el a spark-rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="de398-168">Launch spark-shell by using a command similar toohello following.</span></span> <span data-ttu-id="de398-169">Hello konfigurációk hello aktuális értékét módosítsa szükség szerint:</span><span class="sxs-lookup"><span data-stu-id="de398-169">Change hello actual value of hello configurations as necessary:</span></span> 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a><span data-ttu-id="de398-170">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="de398-170">Additional reading</span></span>

[<span data-ttu-id="de398-171">Spark feladat elküldése a HDInsight-fürtökön</span><span class="sxs-lookup"><span data-stu-id="de398-171">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="what-causes-a-spark-application-outofmemoryerror-exception"></a><span data-ttu-id="de398-172">Mi okozza a Spark OutofMemoryError Alkalmazáskivétel</span><span class="sxs-lookup"><span data-stu-id="de398-172">What causes a Spark application OutofMemoryError exception</span></span>

### <a name="detailed-description"></a><span data-ttu-id="de398-173">Részletes leírás</span><span class="sxs-lookup"><span data-stu-id="de398-173">Detailed description</span></span>

<span data-ttu-id="de398-174">hello Spark alkalmazás nem sikerül, a következő típusú nem kezelt kivételek hello:</span><span class="sxs-lookup"><span data-stu-id="de398-174">hello Spark application fails, with hello following types of uncaught exceptions:</span></span>

```apache
ERROR Executor: Exception in task 7.0 in stage 6.0 (TID 439) 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

```apache
ERROR SparkUncaughtExceptionHandler: Uncaught exception in thread Thread[Executor task launch worker-0,5,main] 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

### <a name="probable-cause"></a><span data-ttu-id="de398-175">Lehetséges ok</span><span class="sxs-lookup"><span data-stu-id="de398-175">Probable cause</span></span>

<span data-ttu-id="de398-176">Ez a kivétel hello legvalószínűbb oka, hogy nem elég halommemória lefoglalja a memóriát toohello Java virtuális gépek (JVMs).</span><span class="sxs-lookup"><span data-stu-id="de398-176">hello most likely cause of this exception is that not enough heap memory is allocated toohello Java virtual machines (JVMs).</span></span> <span data-ttu-id="de398-177">Ezek JVMs hello Spark alkalmazás részeként végrehajtója vagy illesztőprogramok működését.</span><span class="sxs-lookup"><span data-stu-id="de398-177">These JVMs are launched as executors or drivers as part of hello Spark application.</span></span> 

### <a name="resolution-steps"></a><span data-ttu-id="de398-178">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="de398-178">Resolution steps</span></span>

1. <span data-ttu-id="de398-179">Határozza meg a maximális méretét hello hello adatok hello Spark alkalmazás kezeli.</span><span class="sxs-lookup"><span data-stu-id="de398-179">Determine hello maximum size of hello data hello Spark application handles.</span></span> <span data-ttu-id="de398-180">Egy becslés hello bemeneti adatok hello köztes hello bemeneti adatokat, és átalakításakor hello alkalmazás van további hello köztes adatok előállított hello kimeneti adatok átalakítása által létrehozott hello maximális mérete alapján végezheti el.</span><span class="sxs-lookup"><span data-stu-id="de398-180">You can make a guess based on hello maximum size of hello input data, hello intermediate data that's produced by transforming hello input data, and hello output data that's produced when hello application is further transforming hello intermediate data.</span></span> <span data-ttu-id="de398-181">Ez a folyamat az ismétlődő lehet, ha egy kezdeti formális becslés nem hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="de398-181">This process can be an iterative if you can't make an initial formal guess.</span></span> 

2. <span data-ttu-id="de398-182">Győződjön meg arról, hogy, hogy fog toouse rendelkezik elegendő memóriával és -magok tooaccommodate hello Spark alkalmazás erőforrások hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="de398-182">Make sure that hello HDInsight cluster that you're going toouse has enough resources in terms of memory and cores tooaccommodate hello Spark application.</span></span> <span data-ttu-id="de398-183">Ez hello fürt metrikák szakasza hello YARN felhasználói felületen hello értékek megtekintésével meghatározhatja a **használt memória** vs. **Memória összesen**, és **VCores használt** vs. **VCores összesen**.</span><span class="sxs-lookup"><span data-stu-id="de398-183">You can determine this by viewing hello cluster metrics section of hello YARN UI for hello values of **Memory Used** vs. **Memory Total**, and **VCores Used** vs. **VCores Total**.</span></span>

3. <span data-ttu-id="de398-184">Állítsa be a következő Spark hello konfigurációk tooappropriate értékek, amelyek nem haladhatja meg a rendelkezésre álló memória hello és -magok 90 %-át.</span><span class="sxs-lookup"><span data-stu-id="de398-184">Set hello following Spark configurations tooappropriate values, which should not exceed 90% of hello available memory and cores.</span></span> <span data-ttu-id="de398-185">hello értékeket is hello memóriaigényének hello Spark alkalmazás belül kell lennie:</span><span class="sxs-lookup"><span data-stu-id="de398-185">hello values should be well within hello memory requirements of hello Spark application:</span></span> 

    ```apache
    spark.executor.instances (Example: 8 for 8 executor count) 
    spark.executor.memory (Example: 4g for 4 GB) 
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB) 
    spark.executor.cores (Example: 2 for 2 cores per executor) 
    spark.driver.memory (Example: 8g for 8GB) 
    spark.driver.cores (Example: 4 for 4 cores)   
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB) 
    ```

    <span data-ttu-id="de398-186">tooget hello teljes memóriahasználatát összes végrehajtója, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="de398-186">tooget hello total memory used by all executors, run hello following command:</span></span> 
    
    ```apache
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```
    <span data-ttu-id="de398-187">tooget hello teljes memóriahasználatát hello illesztőprogram, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="de398-187">tooget hello total memory used by hello driver, run hello following command:</span></span>
    
    ```apache
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

### <a name="additional-reading"></a><span data-ttu-id="de398-188">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="de398-188">Additional reading</span></span>

- [<span data-ttu-id="de398-189">Spark memória – áttekintés</span><span class="sxs-lookup"><span data-stu-id="de398-189">Spark memory management overview</span></span>](http://spark.apache.org/docs/latest/tuning.html#memory-management-overview)
- [<span data-ttu-id="de398-190">A Spark on HDInsight-fürt alkalmazások hibakeresése</span><span class="sxs-lookup"><span data-stu-id="de398-190">Debug a Spark application on an HDInsight cluster</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)

