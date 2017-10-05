---
title: "Válassza a Livy Spark elküldeni a feladatok a Spark on Azure HDInsight fürt |} Microsoft Docs"
description: "Útmutató Apache Spark REST API használatával távolról a feladatok Spark on Azure HDInsight-fürtök elküldéséhez."
keywords: az Apache Spark on rest API-t livy spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2817b779-1594-486b-8759-489379ca907d
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: nitinme
ms.openlocfilehash: e1a28d69bbf40ea3134a7899a0d2fe70e5fc9e71
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-apache-spark-rest-api-to-submit-remote-jobs-to-an-hdinsight-spark-cluster"></a><span data-ttu-id="5198a-104">Egy HDInsight Spark-fürt távoli feladatok elküldéséhez az Apache Spark REST API használatával</span><span class="sxs-lookup"><span data-stu-id="5198a-104">Use Apache Spark REST API to submit remote jobs to an HDInsight Spark cluster</span></span>

<span data-ttu-id="5198a-105">Ismerje meg, hogyan Livy, az Apache Spark REST API, amely egy Azure HDInsight Spark-fürt távoli feladatok elküldéséhez használhatja.</span><span class="sxs-lookup"><span data-stu-id="5198a-105">Learn how to use Livy, the Apache Spark REST API, which is used to submit remote jobs to an Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="5198a-106">További részletes dokumentációt: [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).</span><span class="sxs-lookup"><span data-stu-id="5198a-106">For detailed documentation, see [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).</span></span>

<span data-ttu-id="5198a-107">Livy használatával futtassa az interaktív Spark ismertetése vagy kötegelt feladatok futni a Spark on való elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="5198a-107">You can use Livy to run interactive Spark shells or submit batch jobs to be run on Spark.</span></span> <span data-ttu-id="5198a-108">Ez a cikk beszél Livy használatával kötegelt feladatok küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="5198a-108">This article talks about using Livy to submit batch jobs.</span></span> <span data-ttu-id="5198a-109">Ebben a cikkben kódtöredékek cURL használatával REST API-hívások a Livy Spark-végponthoz.</span><span class="sxs-lookup"><span data-stu-id="5198a-109">The snippets in this article use cURL to make REST API calls to the Livy Spark endpoint.</span></span>

<span data-ttu-id="5198a-110">**Előfeltételek:**</span><span class="sxs-lookup"><span data-stu-id="5198a-110">**Prerequisites:**</span></span>

* <span data-ttu-id="5198a-111">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="5198a-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="5198a-112">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="5198a-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

* <span data-ttu-id="5198a-113">[cURL](http://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="5198a-113">[cURL](http://curl.haxx.se/).</span></span> <span data-ttu-id="5198a-114">Ez a cikk a curl használatával bemutatják, hogyan lehet REST API-hívások egy HDInsight Spark-fürt ellen.</span><span class="sxs-lookup"><span data-stu-id="5198a-114">This article uses cURL to demonstrate how to make REST API calls against an HDInsight Spark cluster.</span></span>

## <a name="submit-a-livy-spark-batch-job"></a><span data-ttu-id="5198a-115">Livy Spark kötegelt feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="5198a-115">Submit a Livy Spark batch job</span></span>
<span data-ttu-id="5198a-116">A kötegelt mentése előtt a fürthöz tartozó fürt tárhelyen az alkalmazás jar kell feltöltenie.</span><span class="sxs-lookup"><span data-stu-id="5198a-116">Before you submit a batch job, you must upload the application jar on the cluster storage associated with the cluster.</span></span> <span data-ttu-id="5198a-117">Használhat [ **AzCopy**](../storage/common/storage-use-azcopy.md), parancssori segédprogram, ehhez.</span><span class="sxs-lookup"><span data-stu-id="5198a-117">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command-line utility, to do so.</span></span> <span data-ttu-id="5198a-118">Nincsenek más ügyfelek különböző segítségével feltölteni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="5198a-118">There are various other clients you can use to upload data.</span></span> <span data-ttu-id="5198a-119">A rájuk vonatkozó további található [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="5198a-119">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

<span data-ttu-id="5198a-120">**Példák**:</span><span class="sxs-lookup"><span data-stu-id="5198a-120">**Examples**:</span></span>

* <span data-ttu-id="5198a-121">Ha a jar-fájlra a fürt storage (WASB)</span><span class="sxs-lookup"><span data-stu-id="5198a-121">If the jar file is on the cluster storage (WASB)</span></span>
  
        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"
* <span data-ttu-id="5198a-122">Ha a jar-fájl és az osztálynév bemeneti fájl részeként továbbítja szeretne (ebben a példában input.txt)</span><span class="sxs-lookup"><span data-stu-id="5198a-122">If you want to pass the jar filename and the classname as part of an input file (in this example, input.txt)</span></span>
  
        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-livy-spark-batches-running-on-the-cluster"></a><span data-ttu-id="5198a-123">Ismerje meg a fürtön a Livy Spark kötegek információit</span><span class="sxs-lookup"><span data-stu-id="5198a-123">Get information on Livy Spark batches running on the cluster</span></span>
    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

<span data-ttu-id="5198a-124">**Példák**:</span><span class="sxs-lookup"><span data-stu-id="5198a-124">**Examples**:</span></span>

* <span data-ttu-id="5198a-125">Ha szeretné beolvasni az összes, a fürtben futó Livy Spark kötegek:</span><span class="sxs-lookup"><span data-stu-id="5198a-125">If you want to retrieve all the Livy Spark batches running on the cluster:</span></span>
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
* <span data-ttu-id="5198a-126">Ha azt szeretné, hogy egy adott kötegazonosító adott kötegben beolvasása</span><span class="sxs-lookup"><span data-stu-id="5198a-126">If you want to retrieve a specific batch with a given batchId</span></span>
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="delete-a-livy-spark-batch-job"></a><span data-ttu-id="5198a-127">A Livy Spark kötegelt törlése</span><span class="sxs-lookup"><span data-stu-id="5198a-127">Delete a Livy Spark batch job</span></span>
    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

<span data-ttu-id="5198a-128">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="5198a-128">**Example**:</span></span>

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-spark-and-high-availability"></a><span data-ttu-id="5198a-129">Livy Spark és magas rendelkezésre állású</span><span class="sxs-lookup"><span data-stu-id="5198a-129">Livy Spark and high-availability</span></span>
<span data-ttu-id="5198a-130">Livy biztosít a magas rendelkezésre állású Spark a fürtön futó feladatok.</span><span class="sxs-lookup"><span data-stu-id="5198a-130">Livy provides high-availability for Spark jobs running on the cluster.</span></span> <span data-ttu-id="5198a-131">Íme néhány példa.</span><span class="sxs-lookup"><span data-stu-id="5198a-131">Here is a couple of examples.</span></span>

* <span data-ttu-id="5198a-132">Ha a Livy szolgáltatás leáll, miután elküldte a feladat távolról Spark-fürt, a feladat továbbra is fut a háttérben.</span><span class="sxs-lookup"><span data-stu-id="5198a-132">If the Livy service goes down after you have submitted a job remotely to a Spark cluster, the job continues to run in the background.</span></span> <span data-ttu-id="5198a-133">Készítsen biztonsági másolatot Livy esetén vissza jelentéseket, valamint a feladat állapotát állítja vissza.</span><span class="sxs-lookup"><span data-stu-id="5198a-133">When Livy is back up, it restores the status of the job and reports it back.</span></span>
* <span data-ttu-id="5198a-134">A HDInsight Jupyter notebookok Livy szerint vannak kapcsolva, a háttérben.</span><span class="sxs-lookup"><span data-stu-id="5198a-134">Jupyter notebooks for HDInsight are powered by Livy in the backend.</span></span> <span data-ttu-id="5198a-135">Ha a notebook egy Spark feladat fut, és a Livy szolgáltatás újraindítása lekérdezi, a notebook a kód cellák fut tovább.</span><span class="sxs-lookup"><span data-stu-id="5198a-135">If a notebook is running a Spark job and the Livy service gets restarted, the notebook continues to run the code cells.</span></span> 

## <a name="show-me-an-example"></a><span data-ttu-id="5198a-136">Példa megjelenítése</span><span class="sxs-lookup"><span data-stu-id="5198a-136">Show me an example</span></span>
<span data-ttu-id="5198a-137">Ebben a szakaszban úgy tekintünk Livy Spark használandó kötegelt elküldése, figyelheti a feladat állapotát, és törölje azt példákat is.</span><span class="sxs-lookup"><span data-stu-id="5198a-137">In this section, we look at examples to use Livy Spark to submit batch job, monitor the progress of the job, and then delete it.</span></span> <span data-ttu-id="5198a-138">Az alkalmazás ebben a példában használjuk, a cikkben található fejlesztett egy [hozzon létre egy önálló Scala alkalmazás, és futtassa a HDInsight Spark-fürt](hdinsight-apache-spark-create-standalone-application.md).</span><span class="sxs-lookup"><span data-stu-id="5198a-138">The application we use in this example is the one developed in the article [Create a standalone Scala application and to run on HDInsight Spark cluster](hdinsight-apache-spark-create-standalone-application.md).</span></span> <span data-ttu-id="5198a-139">Itt azt feltételezi, hogy:</span><span class="sxs-lookup"><span data-stu-id="5198a-139">The steps here assume that:</span></span>

* <span data-ttu-id="5198a-140">Már másolta az alkalmazás jar keresztül a fürthöz tartozó tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="5198a-140">You have already copied over the application jar to the storage account associated with the cluster.</span></span>
* <span data-ttu-id="5198a-141">Lehetősége van telepítve a számítógépen, amelyre ezeket a lépéseket próbálja CuRL.</span><span class="sxs-lookup"><span data-stu-id="5198a-141">You have CuRL installed on the computer where you are trying these steps.</span></span>

<span data-ttu-id="5198a-142">Hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5198a-142">Perform the following steps:</span></span>

1. <span data-ttu-id="5198a-143">Ossza meg velünk először győződjön meg arról, hogy Livy Spark fut a fürtön.</span><span class="sxs-lookup"><span data-stu-id="5198a-143">Let us first verify that Livy Spark is running on the cluster.</span></span> <span data-ttu-id="5198a-144">Olvasson be kötegek futó azt is megteheti.</span><span class="sxs-lookup"><span data-stu-id="5198a-144">We can do so by getting a list of running batches.</span></span> <span data-ttu-id="5198a-145">Ha egy feladat Livy használatával az első alkalommal futtatja, a kimeneti nulla kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="5198a-145">If you are running a job using Livy for the first time, the output should return zero.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    <span data-ttu-id="5198a-146">Kimenettel kell kapnia hasonló a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="5198a-146">You should get an output similar to the following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="5198a-147">Figyelje meg, hogyan kimenet utolsó sora szerint **összesen: 0**, amely nem futó kötegek javasol.</span><span class="sxs-lookup"><span data-stu-id="5198a-147">Notice how the last line in the output says **total:0**, which suggests no running batches.</span></span>

2. <span data-ttu-id="5198a-148">Ossza meg velünk most kötegelt feladat elküldése.</span><span class="sxs-lookup"><span data-stu-id="5198a-148">Let us now submit a batch job.</span></span> <span data-ttu-id="5198a-149">Az alábbi részlet egy bemeneti fájl (input.txt) használatával paraméterként átadni a jar és az osztály nevét.</span><span class="sxs-lookup"><span data-stu-id="5198a-149">The following snippet uses an input file (input.txt) to pass the jar name and the class name as parameters.</span></span> <span data-ttu-id="5198a-150">Windows rendszerű számítógépről futtatja ezeket a lépéseket, ha egy bemeneti fájl használata javasolt.</span><span class="sxs-lookup"><span data-stu-id="5198a-150">If you are running these steps from a Windows computer, using an input file is the recommended approach.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    <span data-ttu-id="5198a-151">A paraméterek, a fájl **input.txt** az alábbiak szerint definiáltuk:</span><span class="sxs-lookup"><span data-stu-id="5198a-151">The parameters in the file **input.txt** are defined as follows:</span></span>
   
        { "file":"wasb:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }
   
    <span data-ttu-id="5198a-152">Az alábbi kódrészletben hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="5198a-152">You should see an output similar to the  following snippet:</span></span>
   
        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="5198a-153">Figyelje meg, hogy a kimenet utolsó sora szerint **állapota: indítása**.</span><span class="sxs-lookup"><span data-stu-id="5198a-153">Notice how the last line of the output says **state:starting**.</span></span> <span data-ttu-id="5198a-154">Azt is jelzi, **azonosító: 0**.</span><span class="sxs-lookup"><span data-stu-id="5198a-154">It also says, **id:0**.</span></span> <span data-ttu-id="5198a-155">Itt **0** a batch-azonosító.</span><span class="sxs-lookup"><span data-stu-id="5198a-155">Here, **0** is the batch ID.</span></span>

3. <span data-ttu-id="5198a-156">Most le a kötegelt azonosítójával a megadott köteg állapota</span><span class="sxs-lookup"><span data-stu-id="5198a-156">You can now retrieve the status of this specific batch using the batch ID.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    <span data-ttu-id="5198a-157">Az alábbi kódrészletben hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="5198a-157">You should see an output similar to the following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="5198a-158">A kimenet most mutatja **állapota: sikeres**, amelyek arra utalnak, hogy a feladat sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="5198a-158">The output now shows **state:success**, which suggests that the job was successfully completed.</span></span>

4. <span data-ttu-id="5198a-159">Ha azt szeretné, most törölheti a kötegben.</span><span class="sxs-lookup"><span data-stu-id="5198a-159">If you want, you can now delete the batch.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    <span data-ttu-id="5198a-160">Az alábbi kódrészletben hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="5198a-160">You should see an output similar to the following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="5198a-161">A kimenet utolsó sora jeleníti meg, hogy a köteg sikeresen törölve lett.</span><span class="sxs-lookup"><span data-stu-id="5198a-161">The last line of the output shows that the batch was successfully deleted.</span></span> <span data-ttu-id="5198a-162">Is törlése egy feladat futtatása, használhatatlanná teszi a feladatot.</span><span class="sxs-lookup"><span data-stu-id="5198a-162">Deleting a job, while it is running, also kills the job.</span></span> <span data-ttu-id="5198a-163">Ha törli egy feladatot, amely már befejeződött, sikeres, vagy ellenkező esetben teljesen törli az adatok.</span><span class="sxs-lookup"><span data-stu-id="5198a-163">If you delete a job that has completed, successfully or otherwise, it deletes the job information completely.</span></span>

## <a name="using-livy-spark-on-hdinsight-35-clusters"></a><span data-ttu-id="5198a-164">Livy Spark használatával HDInsight 3.5-fürtökön</span><span class="sxs-lookup"><span data-stu-id="5198a-164">Using Livy Spark on HDInsight 3.5 clusters</span></span>

<span data-ttu-id="5198a-165">HDInsight 3.5 fürt, alapértelmezés szerint letiltja a hozzáférés mintaadatfájlok vagy JAR-fájlok kivételével helyi Fájlelérési utak használatát.</span><span class="sxs-lookup"><span data-stu-id="5198a-165">HDInsight 3.5 cluster, by default, disables use of local file paths to access sample data files or jars.</span></span> <span data-ttu-id="5198a-166">Javasoljuk, hogy használja a `wasb://` elérési út helyett JAR-fájlok kivételével eléréséhez, vagy az adatokat fájlok a fürtből.</span><span class="sxs-lookup"><span data-stu-id="5198a-166">We encourage you to use the `wasb://` path instead to access jars or sample data files from the cluster.</span></span> <span data-ttu-id="5198a-167">Ha használni kívánt helyi elérési utat, Ambari konfigurációját ennek megfelelően kell frissíteni.</span><span class="sxs-lookup"><span data-stu-id="5198a-167">If you do want to use local path, you must update the Ambari configuration accordingly.</span></span> <span data-ttu-id="5198a-168">Ehhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="5198a-168">To do so:</span></span>

1. <span data-ttu-id="5198a-169">Nyissa meg a fürt az Ambari portálra.</span><span class="sxs-lookup"><span data-stu-id="5198a-169">Go to the Ambari portal for the cluster.</span></span> <span data-ttu-id="5198a-170">Az Ambari webes felhasználói felület érhető el a HDInsight-fürthöz: https://**CLUSTERNAME**. azurehdidnsight.net, ahol CLUSTERNAME-e a fürt neve.</span><span class="sxs-lookup"><span data-stu-id="5198a-170">The Ambari Web UI is available on your HDInsight cluster at https://**CLUSTERNAME**.azurehdidnsight.net, where CLUSTERNAME is the name of your cluster.</span></span>

2. <span data-ttu-id="5198a-171">A bal oldali navigációs sávon kattintson **Livy**, és kattintson a **Configs**.</span><span class="sxs-lookup"><span data-stu-id="5198a-171">From the left navigation, click **Livy**, and then click **Configs**.</span></span>

3. <span data-ttu-id="5198a-172">A **livy alapértelmezett** adja hozzá a tulajdonság nevét `livy.file.local-dir-whitelist` majd az értékét állítsa **"/"** Ha azt szeretné, hogy a fájlrendszer teljes hozzáférést tesz lehetővé.</span><span class="sxs-lookup"><span data-stu-id="5198a-172">Under **livy-default** add the property name `livy.file.local-dir-whitelist` and set it's value to **"/"** if you want to allow full access to file system.</span></span> <span data-ttu-id="5198a-173">Ha szeretné engedélyezni a hozzáférést csak egy meghatározott könyvtár, adja meg, hogy a könyvtár elérési útjának értékeként.</span><span class="sxs-lookup"><span data-stu-id="5198a-173">If you want to allow access only to a specific directory, provide the path to that directory as the value.</span></span>

## <a name="submitting-livy-jobs-for-a-cluster-within-an-azure-virtual-network"></a><span data-ttu-id="5198a-174">Egy Azure virtuális hálózaton belül fürt Livy feladatok elküldése</span><span class="sxs-lookup"><span data-stu-id="5198a-174">Submitting Livy jobs for a cluster within an Azure virtual network</span></span>

<span data-ttu-id="5198a-175">Ha egy Azure virtuális hálózaton belül egy HDInsight Spark-fürt csatlakozni, akkor közvetlenül kapcsolódhatnak Livy a fürtön.</span><span class="sxs-lookup"><span data-stu-id="5198a-175">If you connect to an HDInsight Spark cluster from within an Azure Virtual Network, you can directly connect to Livy on the cluster.</span></span> <span data-ttu-id="5198a-176">Ebben az esetben Livy végpont URL-je `http://<IP address of the headnode>:8998/batches`.</span><span class="sxs-lookup"><span data-stu-id="5198a-176">In such a case, the URL for Livy endpoint is `http://<IP address of the headnode>:8998/batches`.</span></span> <span data-ttu-id="5198a-177">Itt **8998** a portot, amelyen Livy fut a fürtön headnode.</span><span class="sxs-lookup"><span data-stu-id="5198a-177">Here, **8998** is the port on which Livy runs on the cluster headnode.</span></span> <span data-ttu-id="5198a-178">A nem nyilvános portokon szolgáltatások eléréséhez szükséges további információkért lásd: [a HDInsight Hadoop-szolgáltatás által használt portok](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="5198a-178">For more information on accessing services on non-public ports, see [Ports used by Hadoop services on HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="5198a-179">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="5198a-179">Troubleshooting</span></span>

<span data-ttu-id="5198a-180">Az alábbiakban néhány problémát, előfordulhat, hogy futtatja a Livy használatával a Spark-fürtök távoli feladat elküldése közben.</span><span class="sxs-lookup"><span data-stu-id="5198a-180">Here are some issues that you might run into while using Livy for remote job submission to Spark clusters.</span></span>

### <a name="using-an-external-jar-from-the-additional-storage-is-not-supported"></a><span data-ttu-id="5198a-181">A további tárhely az egy külső jar használata nem támogatott</span><span class="sxs-lookup"><span data-stu-id="5198a-181">Using an external jar from the additional storage is not supported</span></span>

<span data-ttu-id="5198a-182">**Probléma:** a Livy Spark feladat a fürthöz tartozó további tárhely-fiókból egy külső jar hivatkozik, ha a feladat sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="5198a-182">**Problem:** If your Livy Spark job references an external jar from the additional storage account associated with the cluster, the job fails.</span></span>

<span data-ttu-id="5198a-183">**Megoldás:** győződjön meg arról, hogy a használni kívánt jar érhető el a HDInsight-fürthöz tartozó alapértelmezett tárolására.</span><span class="sxs-lookup"><span data-stu-id="5198a-183">**Resolution:** Make sure that the jar you want to use is available in the default storage associated with the HDInsight cluster.</span></span>





## <a name="next-step"></a><span data-ttu-id="5198a-184">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="5198a-184">Next step</span></span>

* [<span data-ttu-id="5198a-185">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="5198a-185">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="5198a-186">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="5198a-186">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

