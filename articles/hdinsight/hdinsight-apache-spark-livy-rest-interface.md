---
title: "aaaUse Livy Spark toosubmit feladatok tooSpark fürt Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Apache Spark REST API toosubmit Spark feladatok távolról tooan Azure HDInsight-fürthöz."
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
ms.openlocfilehash: 417213b5f1db1522373188002fe05117faea5243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-spark-rest-api-toosubmit-remote-jobs-tooan-hdinsight-spark-cluster"></a><span data-ttu-id="4662c-104">Apache Spark REST API toosubmit távoli feladatok tooan HDInsight Spark-fürt használatára</span><span class="sxs-lookup"><span data-stu-id="4662c-104">Use Apache Spark REST API toosubmit remote jobs tooan HDInsight Spark cluster</span></span>

<span data-ttu-id="4662c-105">Ismerje meg, hogyan toouse Livy, Apache Spark REST API, amely távoli használt toosubmit hello feladatok tooan Azure HDInsight Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="4662c-105">Learn how toouse Livy, hello Apache Spark REST API, which is used toosubmit remote jobs tooan Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="4662c-106">További részletes dokumentációt: [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).</span><span class="sxs-lookup"><span data-stu-id="4662c-106">For detailed documentation, see [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).</span></span>

<span data-ttu-id="4662c-107">Livy toorun interaktív Spark ismertetése használhatja, vagy küldje el a kötegelt feladatok toobe Spark futtatnak.</span><span class="sxs-lookup"><span data-stu-id="4662c-107">You can use Livy toorun interactive Spark shells or submit batch jobs toobe run on Spark.</span></span> <span data-ttu-id="4662c-108">Ez a cikk beszél Livy toosubmit kötegelt feladatok használatával.</span><span class="sxs-lookup"><span data-stu-id="4662c-108">This article talks about using Livy toosubmit batch jobs.</span></span> <span data-ttu-id="4662c-109">Ebben a cikkben hello kódtöredékek használata cURL toomake REST API-hívások toohello Livy Spark végpont.</span><span class="sxs-lookup"><span data-stu-id="4662c-109">hello snippets in this article use cURL toomake REST API calls toohello Livy Spark endpoint.</span></span>

<span data-ttu-id="4662c-110">**Előfeltételek:**</span><span class="sxs-lookup"><span data-stu-id="4662c-110">**Prerequisites:**</span></span>

* <span data-ttu-id="4662c-111">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="4662c-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="4662c-112">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="4662c-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

* <span data-ttu-id="4662c-113">[cURL](http://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="4662c-113">[cURL](http://curl.haxx.se/).</span></span> <span data-ttu-id="4662c-114">Ez a cikk a cURL toodemonstrate hogyan toomake REST API meghívja a HDInsight Spark-fürt elleni használja.</span><span class="sxs-lookup"><span data-stu-id="4662c-114">This article uses cURL toodemonstrate how toomake REST API calls against an HDInsight Spark cluster.</span></span>

## <a name="submit-a-livy-spark-batch-job"></a><span data-ttu-id="4662c-115">Livy Spark kötegelt feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="4662c-115">Submit a Livy Spark batch job</span></span>
<span data-ttu-id="4662c-116">Kötegelt mentése előtt hello alkalmazás jar a hello-fürthöz tartozó hello fürttároló kell feltöltenie.</span><span class="sxs-lookup"><span data-stu-id="4662c-116">Before you submit a batch job, you must upload hello application jar on hello cluster storage associated with hello cluster.</span></span> <span data-ttu-id="4662c-117">Használhat [ **AzCopy**](../storage/common/storage-use-azcopy.md), a parancssori segédprogram, toodo így.</span><span class="sxs-lookup"><span data-stu-id="4662c-117">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command-line utility, toodo so.</span></span> <span data-ttu-id="4662c-118">Nincsenek más ügyfelek különböző tooupload adatokat használhatja.</span><span class="sxs-lookup"><span data-stu-id="4662c-118">There are various other clients you can use tooupload data.</span></span> <span data-ttu-id="4662c-119">A rájuk vonatkozó további található [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="4662c-119">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path tooapplication jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

<span data-ttu-id="4662c-120">**Példák**:</span><span class="sxs-lookup"><span data-stu-id="4662c-120">**Examples**:</span></span>

* <span data-ttu-id="4662c-121">Ha hello jar-fájlra hello fürt storage (WASB)</span><span class="sxs-lookup"><span data-stu-id="4662c-121">If hello jar file is on hello cluster storage (WASB)</span></span>
  
        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"
* <span data-ttu-id="4662c-122">Ha azt szeretné, hogy toopass hello jar fájlnév és a bemeneti fájl részeként osztálynév hello (ebben a példában input.txt)</span><span class="sxs-lookup"><span data-stu-id="4662c-122">If you want toopass hello jar filename and hello classname as part of an input file (in this example, input.txt)</span></span>
  
        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-livy-spark-batches-running-on-hello-cluster"></a><span data-ttu-id="4662c-123">Információ a hello fürtben futó Livy Spark kötegek</span><span class="sxs-lookup"><span data-stu-id="4662c-123">Get information on Livy Spark batches running on hello cluster</span></span>
    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

<span data-ttu-id="4662c-124">**Példák**:</span><span class="sxs-lookup"><span data-stu-id="4662c-124">**Examples**:</span></span>

* <span data-ttu-id="4662c-125">Ha azt szeretné, tooretrieve hello fürtben futó összes hello Livy Spark kötegek:</span><span class="sxs-lookup"><span data-stu-id="4662c-125">If you want tooretrieve all hello Livy Spark batches running on hello cluster:</span></span>
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
* <span data-ttu-id="4662c-126">Ha azt szeretné, hogy egy adott kötegazonosító adott kötegben tooretrieve</span><span class="sxs-lookup"><span data-stu-id="4662c-126">If you want tooretrieve a specific batch with a given batchId</span></span>
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="delete-a-livy-spark-batch-job"></a><span data-ttu-id="4662c-127">A Livy Spark kötegelt törlése</span><span class="sxs-lookup"><span data-stu-id="4662c-127">Delete a Livy Spark batch job</span></span>
    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

<span data-ttu-id="4662c-128">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="4662c-128">**Example**:</span></span>

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-spark-and-high-availability"></a><span data-ttu-id="4662c-129">Livy Spark és magas rendelkezésre állású</span><span class="sxs-lookup"><span data-stu-id="4662c-129">Livy Spark and high-availability</span></span>
<span data-ttu-id="4662c-130">Livy hello fürtben futó Spark feladatok biztosít a magas rendelkezésre állású.</span><span class="sxs-lookup"><span data-stu-id="4662c-130">Livy provides high-availability for Spark jobs running on hello cluster.</span></span> <span data-ttu-id="4662c-131">Íme néhány példa.</span><span class="sxs-lookup"><span data-stu-id="4662c-131">Here is a couple of examples.</span></span>

* <span data-ttu-id="4662c-132">Ha hello Livy szolgáltatás nem működik, miután egy feladat távolról elküldte tooa Spark fürtben hello feladat toorun hello háttérben folytatódik.</span><span class="sxs-lookup"><span data-stu-id="4662c-132">If hello Livy service goes down after you have submitted a job remotely tooa Spark cluster, hello job continues toorun in hello background.</span></span> <span data-ttu-id="4662c-133">Ha Livy készítsen biztonsági másolatot, hello feladat és a jelentések vissza hello állapotát állítja vissza.</span><span class="sxs-lookup"><span data-stu-id="4662c-133">When Livy is back up, it restores hello status of hello job and reports it back.</span></span>
* <span data-ttu-id="4662c-134">Jupyter notebookok a HDInsight a hello háttérbeli Livy szerint vannak kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="4662c-134">Jupyter notebooks for HDInsight are powered by Livy in hello backend.</span></span> <span data-ttu-id="4662c-135">Ha a notebook egy Spark feladat fut, és hello Livy szolgáltatás újraindítása lekérdezi, hello notebook toorun hello kód cellák továbbra is.</span><span class="sxs-lookup"><span data-stu-id="4662c-135">If a notebook is running a Spark job and hello Livy service gets restarted, hello notebook continues toorun hello code cells.</span></span> 

## <a name="show-me-an-example"></a><span data-ttu-id="4662c-136">Példa megjelenítése</span><span class="sxs-lookup"><span data-stu-id="4662c-136">Show me an example</span></span>
<span data-ttu-id="4662c-137">Ebben a részben azt példák toouse Livy Spark toosubmit kötegelt tekintse meg, hello hello feladat előrehaladásának figyeléséhez, és törölje azt.</span><span class="sxs-lookup"><span data-stu-id="4662c-137">In this section, we look at examples toouse Livy Spark toosubmit batch job, monitor hello progress of hello job, and then delete it.</span></span> <span data-ttu-id="4662c-138">hello ebben a példában használjuk alkalmazás egy fejlesztett hello cikkben hello [egy önálló Scala alkalmazás és toorun létrehozása a HDInsight Spark-fürt](hdinsight-apache-spark-create-standalone-application.md).</span><span class="sxs-lookup"><span data-stu-id="4662c-138">hello application we use in this example is hello one developed in hello article [Create a standalone Scala application and toorun on HDInsight Spark cluster](hdinsight-apache-spark-create-standalone-application.md).</span></span> <span data-ttu-id="4662c-139">Itt a hello lépések azt feltételezik, hogy:</span><span class="sxs-lookup"><span data-stu-id="4662c-139">hello steps here assume that:</span></span>

* <span data-ttu-id="4662c-140">Már felülírt hello alkalmazás jar toohello tárfiók hello-fürthöz tartozó.</span><span class="sxs-lookup"><span data-stu-id="4662c-140">You have already copied over hello application jar toohello storage account associated with hello cluster.</span></span>
* <span data-ttu-id="4662c-141">A CuRL ezeket a lépéseket megkísérlése hello számítógépen telepítve van.</span><span class="sxs-lookup"><span data-stu-id="4662c-141">You have CuRL installed on hello computer where you are trying these steps.</span></span>

<span data-ttu-id="4662c-142">Hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4662c-142">Perform hello following steps:</span></span>

1. <span data-ttu-id="4662c-143">Ossza meg velünk először győződjön meg arról, hogy fut-e Livy Spark hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="4662c-143">Let us first verify that Livy Spark is running on hello cluster.</span></span> <span data-ttu-id="4662c-144">Olvasson be kötegek futó azt is megteheti.</span><span class="sxs-lookup"><span data-stu-id="4662c-144">We can do so by getting a list of running batches.</span></span> <span data-ttu-id="4662c-145">Ha egy feladat Livy használatával hello az első alkalommal futtatja, hello kimeneti nulla kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="4662c-145">If you are running a job using Livy for hello first time, hello output should return zero.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    <span data-ttu-id="4662c-146">Az alábbi részlet kimeneti hasonló toohello szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="4662c-146">You should get an output similar toohello following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="4662c-147">Figyelje meg, hogyan hello utolsó hello kimenet sorban a **összesen: 0**, amely nem futó kötegek javasol.</span><span class="sxs-lookup"><span data-stu-id="4662c-147">Notice how hello last line in hello output says **total:0**, which suggests no running batches.</span></span>

2. <span data-ttu-id="4662c-148">Ossza meg velünk most kötegelt feladat elküldése.</span><span class="sxs-lookup"><span data-stu-id="4662c-148">Let us now submit a batch job.</span></span> <span data-ttu-id="4662c-149">a következő kódrészletet hello paraméterként egy bemeneti fájl (input.txt) toopass hello jar és hello osztály nevét használja.</span><span class="sxs-lookup"><span data-stu-id="4662c-149">hello following snippet uses an input file (input.txt) toopass hello jar name and hello class name as parameters.</span></span> <span data-ttu-id="4662c-150">Ha ezeket a lépéseket egy Windows-számítógépen futtatja, az ajánlott megközelítést alkalmazva hello bemeneti fájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="4662c-150">If you are running these steps from a Windows computer, using an input file is hello recommended approach.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    <span data-ttu-id="4662c-151">hello fájlban paraméterek hello **input.txt** az alábbiak szerint definiáltuk:</span><span class="sxs-lookup"><span data-stu-id="4662c-151">hello parameters in hello file **input.txt** are defined as follows:</span></span>
   
        { "file":"wasb:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }
   
    <span data-ttu-id="4662c-152">Az alábbi részlet kimeneti hasonló toohello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4662c-152">You should see an output similar toohello  following snippet:</span></span>
   
        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="4662c-153">Figyelje meg, hogyan hello hello kimenet utolsó sora szerint **állapota: indítása**.</span><span class="sxs-lookup"><span data-stu-id="4662c-153">Notice how hello last line of hello output says **state:starting**.</span></span> <span data-ttu-id="4662c-154">Azt is jelzi, **azonosító: 0**.</span><span class="sxs-lookup"><span data-stu-id="4662c-154">It also says, **id:0**.</span></span> <span data-ttu-id="4662c-155">Itt **0** hello kötegelt azonosító.</span><span class="sxs-lookup"><span data-stu-id="4662c-155">Here, **0** is hello batch ID.</span></span>

3. <span data-ttu-id="4662c-156">Most le hello állapota az adott kötegelt hello kötegelt azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="4662c-156">You can now retrieve hello status of this specific batch using hello batch ID.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    <span data-ttu-id="4662c-157">Az alábbi részlet kimeneti hasonló toohello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4662c-157">You should see an output similar toohello following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="4662c-158">hello most látható kimeneti **állapota: sikeres**, amely javasol hello feladat sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="4662c-158">hello output now shows **state:success**, which suggests that hello job was successfully completed.</span></span>

4. <span data-ttu-id="4662c-159">Ha azt szeretné, most törölheti hello kötegelt.</span><span class="sxs-lookup"><span data-stu-id="4662c-159">If you want, you can now delete hello batch.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    <span data-ttu-id="4662c-160">Az alábbi részlet kimeneti hasonló toohello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4662c-160">You should see an output similar toohello following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="4662c-161">hello hello kimenet utolsó sora jeleníti meg, hogy hello kötegelt törlése sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="4662c-161">hello last line of hello output shows that hello batch was successfully deleted.</span></span> <span data-ttu-id="4662c-162">Egy feladat törlése futtatása, is hello feladat használhatatlanná teszi.</span><span class="sxs-lookup"><span data-stu-id="4662c-162">Deleting a job, while it is running, also kills hello job.</span></span> <span data-ttu-id="4662c-163">Ha törli egy feladatot, amely már befejeződött, sikeres, vagy ellenkező esetben teljesen törli hello feladatok adatait.</span><span class="sxs-lookup"><span data-stu-id="4662c-163">If you delete a job that has completed, successfully or otherwise, it deletes hello job information completely.</span></span>

## <a name="using-livy-spark-on-hdinsight-35-clusters"></a><span data-ttu-id="4662c-164">Livy Spark használatával HDInsight 3.5-fürtökön</span><span class="sxs-lookup"><span data-stu-id="4662c-164">Using Livy Spark on HDInsight 3.5 clusters</span></span>

<span data-ttu-id="4662c-165">HDInsight 3.5 fürt, alapértelmezés szerint letiltja a helyi fájl elérési utak tooaccess mintaadatfájlok vagy a JAR-fájlok kivételével.</span><span class="sxs-lookup"><span data-stu-id="4662c-165">HDInsight 3.5 cluster, by default, disables use of local file paths tooaccess sample data files or jars.</span></span> <span data-ttu-id="4662c-166">Javasoljuk, toouse hello `wasb://` elérési út helyett tooaccess JAR-fájlok kivételével vagy mintaadatok fájlok hello fürtből.</span><span class="sxs-lookup"><span data-stu-id="4662c-166">We encourage you toouse hello `wasb://` path instead tooaccess jars or sample data files from hello cluster.</span></span> <span data-ttu-id="4662c-167">Ha szeretné, hogy toouse helyi elérési utat, hello Ambari konfigurációját ennek megfelelően kell frissíteni.</span><span class="sxs-lookup"><span data-stu-id="4662c-167">If you do want toouse local path, you must update hello Ambari configuration accordingly.</span></span> <span data-ttu-id="4662c-168">toodo így:</span><span class="sxs-lookup"><span data-stu-id="4662c-168">toodo so:</span></span>

1. <span data-ttu-id="4662c-169">Nyissa meg toohello Ambari portal hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="4662c-169">Go toohello Ambari portal for hello cluster.</span></span> <span data-ttu-id="4662c-170">hello Ambari webes felhasználói felület érhető el a HDInsight-fürthöz: https://**CLUSTERNAME**. azurehdidnsight.net, ahol CLUSTERNAME-e a fürt hello neve.</span><span class="sxs-lookup"><span data-stu-id="4662c-170">hello Ambari Web UI is available on your HDInsight cluster at https://**CLUSTERNAME**.azurehdidnsight.net, where CLUSTERNAME is hello name of your cluster.</span></span>

2. <span data-ttu-id="4662c-171">A bal oldali navigációs hello, kattintson az **Livy**, és kattintson a **Configs**.</span><span class="sxs-lookup"><span data-stu-id="4662c-171">From hello left navigation, click **Livy**, and then click **Configs**.</span></span>

3. <span data-ttu-id="4662c-172">A **livy alapértelmezett** hello tulajdonságnév hozzáadása `livy.file.local-dir-whitelist` és állítsa be az értéket túl**"/"** tooallow teljes körű hozzáférési toofile rendszer.</span><span class="sxs-lookup"><span data-stu-id="4662c-172">Under **livy-default** add hello property name `livy.file.local-dir-whitelist` and set it's value too**"/"** if you want tooallow full access toofile system.</span></span> <span data-ttu-id="4662c-173">Ha azt szeretné, hogy tooallow hozzáférés csak tooa adott directory, ad hello elérési toothat directory hello értékként.</span><span class="sxs-lookup"><span data-stu-id="4662c-173">If you want tooallow access only tooa specific directory, provide hello path toothat directory as hello value.</span></span>

## <a name="submitting-livy-jobs-for-a-cluster-within-an-azure-virtual-network"></a><span data-ttu-id="4662c-174">Egy Azure virtuális hálózaton belül fürt Livy feladatok elküldése</span><span class="sxs-lookup"><span data-stu-id="4662c-174">Submitting Livy jobs for a cluster within an Azure virtual network</span></span>

<span data-ttu-id="4662c-175">Ha egy Azure virtuális hálózaton belül a HDInsight Spark-fürt tooan, közvetlenül kapcsolódhatnak tooLivy hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="4662c-175">If you connect tooan HDInsight Spark cluster from within an Azure Virtual Network, you can directly connect tooLivy on hello cluster.</span></span> <span data-ttu-id="4662c-176">Ebben az esetben hello URL-címet a Livy végpont `http://<IP address of hello headnode>:8998/batches`.</span><span class="sxs-lookup"><span data-stu-id="4662c-176">In such a case, hello URL for Livy endpoint is `http://<IP address of hello headnode>:8998/batches`.</span></span> <span data-ttu-id="4662c-177">Itt **8998** hello port, amelyen Livy futó hello fürt headnode.</span><span class="sxs-lookup"><span data-stu-id="4662c-177">Here, **8998** is hello port on which Livy runs on hello cluster headnode.</span></span> <span data-ttu-id="4662c-178">A nem nyilvános portokon szolgáltatások eléréséhez szükséges további információkért lásd: [a HDInsight Hadoop-szolgáltatás által használt portok](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="4662c-178">For more information on accessing services on non-public ports, see [Ports used by Hadoop services on HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="4662c-179">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="4662c-179">Troubleshooting</span></span>

<span data-ttu-id="4662c-180">Az alábbiakban néhány problémát, távoli feladat elküldése tooSpark fürtök Livy használatakor mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="4662c-180">Here are some issues that you might run into while using Livy for remote job submission tooSpark clusters.</span></span>

### <a name="using-an-external-jar-from-hello-additional-storage-is-not-supported"></a><span data-ttu-id="4662c-181">Egy külső jar a további tárhely hello használata nem támogatott</span><span class="sxs-lookup"><span data-stu-id="4662c-181">Using an external jar from hello additional storage is not supported</span></span>

<span data-ttu-id="4662c-182">**Probléma:** Ha a Livy Spark feladat hivatkozik egy külső jar tárfiókból hello további hello-fürthöz tartozó, hello feladat sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="4662c-182">**Problem:** If your Livy Spark job references an external jar from hello additional storage account associated with hello cluster, hello job fails.</span></span>

<span data-ttu-id="4662c-183">**Megoldás:** győződjön meg arról, hogy azt szeretné, hogy a toouse érhető el hello alapértelmezett tárolási hello HDInsight-fürthöz társított hello jar.</span><span class="sxs-lookup"><span data-stu-id="4662c-183">**Resolution:** Make sure that hello jar you want toouse is available in hello default storage associated with hello HDInsight cluster.</span></span>





## <a name="next-step"></a><span data-ttu-id="4662c-184">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="4662c-184">Next step</span></span>

* [<span data-ttu-id="4662c-185">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="4662c-185">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="4662c-186">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="4662c-186">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

