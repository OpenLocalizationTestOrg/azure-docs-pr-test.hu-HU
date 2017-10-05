---
title: "Hadoop Sqoop használata a hdinsight - Azure Curl |} Microsoft Docs"
description: "Megtudhatja, hogyan távolról a Sqoop feladatok HDInsight használata Curl használatával való elküldéséhez."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 39798321-78ca-428c-bcfe-322e49af4059
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 0975aedf58c6e110726dd3308eae5f9ad3907cc7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a><span data-ttu-id="8ceea-103">Sqoop feladatok futtatása a hadooppal a Hdinsightban a Curl</span><span class="sxs-lookup"><span data-stu-id="8ceea-103">Run Sqoop jobs with Hadoop in HDInsight with Curl</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="8ceea-104">Megtudhatja, hogyan használja a Curl hdinsight Hadoop-fürthöz Sqoop feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="8ceea-104">Learn how to use Curl to run Sqoop jobs on a Hadoop cluster in HDInsight.</span></span>

<span data-ttu-id="8ceea-105">Curl használatával bemutatják, hogyan kezelheti a HDInsight nyers HTTP-kérelmek futtatásához, a figyelheti és a Sqoop feladatot az eredmények visszakeresésére használatával.</span><span class="sxs-lookup"><span data-stu-id="8ceea-105">Curl is used to demonstrate how you can interact with HDInsight by using raw HTTP requests to run, monitor, and retrieve the results of Sqoop jobs.</span></span> <span data-ttu-id="8ceea-106">Ez a módszer a WebHCat REST API használatával (korábbi nevén lépni a Templeton) a HDInsight-fürt által biztosított.</span><span class="sxs-lookup"><span data-stu-id="8ceea-106">This works by using the WebHCat REST API (formerly known as Templeton) provided by your HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="8ceea-107">Ha ismeri a Linux-alapú Hadoop-kiszolgálókat használ, de még nem használta a HDInsight, lásd: [információ a HDInsight használata Linux](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="8ceea-107">If you are already familiar with using Linux-based Hadoop servers, but are new to HDInsight, see [Information about using HDInsight on Linux](hdinsight-hadoop-linux-information.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="8ceea-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8ceea-108">Prerequisites</span></span>
<span data-ttu-id="8ceea-109">Ebben a cikkben szereplő lépések elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="8ceea-109">To complete the steps in this article, you will need the following:</span></span>

* <span data-ttu-id="8ceea-110">A Hadoop on HDInsight-fürt (Linux vagy a Windows-alapú)</span><span class="sxs-lookup"><span data-stu-id="8ceea-110">A Hadoop on HDInsight cluster (Linux or Windows-based)</span></span>
* [<span data-ttu-id="8ceea-111">Curl</span><span class="sxs-lookup"><span data-stu-id="8ceea-111">Curl</span></span>](http://curl.haxx.se/)
* [<span data-ttu-id="8ceea-112">jq</span><span class="sxs-lookup"><span data-stu-id="8ceea-112">jq</span></span>](http://stedolan.github.io/jq/)

## <a name="submit-sqoop-jobs-by-using-curl"></a><span data-ttu-id="8ceea-113">Sqoop feladatok elküldéséhez a Curl használatával</span><span class="sxs-lookup"><span data-stu-id="8ceea-113">Submit Sqoop jobs by using Curl</span></span>
> [!NOTE]
> <span data-ttu-id="8ceea-114">Amikor a Curl vagy más REST kommunikációt használ a WebHCattel, hitelesítenie kell a kéréseket a HDInsight fürt rendszergazdája felhasználónevének és jelszavának megadásával.</span><span class="sxs-lookup"><span data-stu-id="8ceea-114">When using Curl or any other REST communication with WebHCat, you must authenticate the requests by providing the user name and password for the HDInsight cluster administrator.</span></span> <span data-ttu-id="8ceea-115">A fürtnevet a kérések kiszolgálóhoz küldéséhez használt egységes erőforrás-azonosító (URI) részeként is használnia kell.</span><span class="sxs-lookup"><span data-stu-id="8ceea-115">You must also use the cluster name as part of the Uniform Resource Identifier (URI) used to send the requests to the server.</span></span>
> 
> <span data-ttu-id="8ceea-116">Ezen szakasz parancsaiban cserélje le a **USERNAME** elemet a fürthöz hitelesíteni kívánt felhasználóval, és a **PASSWORD** elemet pedig a felhasználói fiók jelszavával.</span><span class="sxs-lookup"><span data-stu-id="8ceea-116">For the commands in this section, replace **USERNAME** with the user to authenticate to the cluster, and replace **PASSWORD** with the password for the user account.</span></span> <span data-ttu-id="8ceea-117">Cserélje le a **CLUSTERNAME** elemet a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="8ceea-117">Replace **CLUSTERNAME** with the name of your cluster.</span></span>
> 
> <span data-ttu-id="8ceea-118">A REST API védelméről [alapszintű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication) gondoskodik.</span><span class="sxs-lookup"><span data-stu-id="8ceea-118">The REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="8ceea-119">Mindig biztonságos HTTP-n (HTTPS-en) keresztül kell kéréseket végeznie, hogy a hitelesítő adatait biztonságos módon küldje a kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="8ceea-119">You should always make requests by using Secure HTTP (HTTPS) to help ensure that your credentials are securely sent to the server.</span></span>
> 
> 

1. <span data-ttu-id="8ceea-120">Egy parancssorból a következő paranccsal ellenőrizze, hogy tud-e kapcsolódni a HDInsight-fürthöz:</span><span class="sxs-lookup"><span data-stu-id="8ceea-120">From a command line, use the following command to verify that you can connect to your HDInsight cluster:</span></span>
   
        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
   
    <span data-ttu-id="8ceea-121">A következőhöz hasonló választ kell kapnia:</span><span class="sxs-lookup"><span data-stu-id="8ceea-121">You should receive a response similar to the following:</span></span>
   
        {"status":"ok","version":"v1"}
   
    <span data-ttu-id="8ceea-122">Ezen parancs paraméterei a következők:</span><span class="sxs-lookup"><span data-stu-id="8ceea-122">The parameters used in this command are as follows:</span></span>
   
   * <span data-ttu-id="8ceea-123">**-u** - A kérés hitelesítéséhez használt felhasználónév és jelszó.</span><span class="sxs-lookup"><span data-stu-id="8ceea-123">**-u** - The user name and password used to authenticate the request.</span></span>
   * <span data-ttu-id="8ceea-124">**-G** - Jelzi, hogy ez egy GET kérés.</span><span class="sxs-lookup"><span data-stu-id="8ceea-124">**-G** - Indicates that this is a GET request.</span></span>
     
     <span data-ttu-id="8ceea-125">Az URL-cím, az elején **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, minden olyan kérelem esetében ugyanaz lesz.</span><span class="sxs-lookup"><span data-stu-id="8ceea-125">The beginning of the URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, will be the same for all requests.</span></span> <span data-ttu-id="8ceea-126">Az elérési út **/status**, azt jelzi, hogy a kérelem vissza WebHCat (más néven Templeton) állapota a kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="8ceea-126">The path, **/status**, indicates that the request is to return a status of WebHCat (also known as Templeton) for the server.</span></span> 
2. <span data-ttu-id="8ceea-127">Használja a következő sqoop feladat elküldése:</span><span class="sxs-lookup"><span data-stu-id="8ceea-127">Use the following to submit a sqoop job:</span></span>

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    <span data-ttu-id="8ceea-128">Ezen parancs paraméterei a következők:</span><span class="sxs-lookup"><span data-stu-id="8ceea-128">The parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="8ceea-129">**-d** - óta `-G` nem használ, akkor a kérelmet az alapértelmezett a POST metódussal.</span><span class="sxs-lookup"><span data-stu-id="8ceea-129">**-d** - Since `-G` is not used, the request defaults to the POST method.</span></span> <span data-ttu-id="8ceea-130">`-d`Megadja a küldött adatértékekkel mellékel a kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="8ceea-130">`-d` specifies the data values that are sent with the request.</span></span>

        * <span data-ttu-id="8ceea-131">**User.name** – a parancsot futtató felhasználónak.</span><span class="sxs-lookup"><span data-stu-id="8ceea-131">**user.name** - The user that is running the command.</span></span>

        * <span data-ttu-id="8ceea-132">**a parancs** -a Sqoop parancs végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="8ceea-132">**command** - The Sqoop command to execute.</span></span>

        * <span data-ttu-id="8ceea-133">**statusdir** -címtárban, ami a feladat állapotát a rendszer úgy tünteti fel.</span><span class="sxs-lookup"><span data-stu-id="8ceea-133">**statusdir** - The directory that the status for this job will be written to.</span></span>

    <span data-ttu-id="8ceea-134">Ez a parancs a Feladatazonosítót a feladat állapotának ellenőrzéséhez használható kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="8ceea-134">This command should return a job ID that can be used to check the status of the job.</span></span>

        {"id":"job_1415651640909_0026"}

1. <span data-ttu-id="8ceea-135">A feladat állapotának ellenőrzéséhez használja a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="8ceea-135">To check the status of the job, use the following command.</span></span> <span data-ttu-id="8ceea-136">Cserélje le **JOBID** az előző lépésben visszaadott értékkel.</span><span class="sxs-lookup"><span data-stu-id="8ceea-136">Replace **JOBID** with the value returned in the previous step.</span></span> <span data-ttu-id="8ceea-137">Például, ha a visszatérési érték volt `{"id":"job_1415651640909_0026"}`, majd **JOBID** lenne `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="8ceea-137">For example, if the return value was `{"id":"job_1415651640909_0026"}`, then **JOBID** would be `job_1415651640909_0026`.</span></span>
   
        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
   
    <span data-ttu-id="8ceea-138">Ha a feladat befejeződött, az állapot nem **sikeres**.</span><span class="sxs-lookup"><span data-stu-id="8ceea-138">If the job has finished, the state will be **SUCCEEDED**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="8ceea-139">A Curl kérelem egy JavaScript Object Notation (JSON) dokumentumot, a feladat; információkat ad vissza csak a állapotérték beolvasásához használt jq.</span><span class="sxs-lookup"><span data-stu-id="8ceea-139">This Curl request returns a JavaScript Object Notation (JSON) document with information about the job; jq is used to retrieve only the state value.</span></span>
   > 
   > 
2. <span data-ttu-id="8ceea-140">Ha a feladat állapota módosult **sikeres**, a feladat eredményeinek lekérése Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="8ceea-140">Once the state of the job has changed to **SUCCEEDED**, you can retrieve the results of the job from Azure Blob storage.</span></span> <span data-ttu-id="8ceea-141">A `statusdir` a lekérdezés átadott paraméter tartalmazza a hely a kimeneti fájl; ebben az esetben az **wasb: / / / Példa/curl**.</span><span class="sxs-lookup"><span data-stu-id="8ceea-141">The `statusdir` parameter passed with the query contains the location of the output file; in this case, **wasb:///example/curl**.</span></span> <span data-ttu-id="8ceea-142">Ez a cím tárolja a feladat kimenete a **példa/curl** könyvtárába a HDInsight-fürt által használt alapértelmezett tárolót.</span><span class="sxs-lookup"><span data-stu-id="8ceea-142">This address stores the output of the job in the **example/curl** directory on the default storage container used by your HDInsight cluster.</span></span>
   
    <span data-ttu-id="8ceea-143">Listáról, és ezek a fájlok letöltésére a [Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="8ceea-143">You can list and download these files by using the [Azure CLI](../cli-install-nodejs.md).</span></span> <span data-ttu-id="8ceea-144">Például, hogy a fájlok listázása **példa/curl**, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="8ceea-144">For example, to list files in **example/curl**, use the following command:</span></span>
   
        azure storage blob list <container-name> example/curl
   
    <span data-ttu-id="8ceea-145">A fájl letöltésére, az alábbi parancsokat használja:</span><span class="sxs-lookup"><span data-stu-id="8ceea-145">To download a file, use the following:</span></span>
   
        azure storage blob download <container-name> <blob-name> <destination-file>
   
   > [!NOTE]
   > <span data-ttu-id="8ceea-146">Meg kell adnia a tárfiók nevét, amely tartalmazza a blob használatával a `-a` és `-k` paramétereket, vagy állítsa a **AZURE\_tárolási\_fiók** és **AZURE \_Tárolási\_hozzáférés\_kulcs** környezeti változókat.</span><span class="sxs-lookup"><span data-stu-id="8ceea-146">You must either specify the storage account name that contains the blob by using the `-a` and `-k` parameters, or set the **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS\_KEY** environment variables.</span></span> <span data-ttu-id="8ceea-147">Lásd: < a href = "a hdinsight-feltöltési-data.md" target = "_blank" További információ.</span><span class="sxs-lookup"><span data-stu-id="8ceea-147">See <a href="hdinsight-upload-data.md" target="_blank" for more information.</span></span>
   > 
   > 

## <a name="limitations"></a><span data-ttu-id="8ceea-148">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="8ceea-148">Limitations</span></span>
* <span data-ttu-id="8ceea-149">Tömeges export - a Linux-alapú HDInsight, a Sqoop összekötő használt Microsoft SQL Server vagy az Azure SQL Database adatainak exportálása jelenleg nem támogatja a tömeges beszúrások.</span><span class="sxs-lookup"><span data-stu-id="8ceea-149">Bulk export - With Linux-based HDInsight, the Sqoop connector used to export data to Microsoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="8ceea-150">Kötegelés - és a Linux-alapú HDInsight együttes használata esetén a `-batch` beszúrása végrehajtásakor kapcsoló, a Sqoop több beszúrás helyett a beszúrási műveletek kötegelése hajt végre.</span><span class="sxs-lookup"><span data-stu-id="8ceea-150">Batching - With Linux-based HDInsight, When using the `-batch` switch when performing inserts, Sqoop will perform multiple inserts instead of batching the insert operations.</span></span>

## <a name="summary"></a><span data-ttu-id="8ceea-151">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="8ceea-151">Summary</span></span>
<span data-ttu-id="8ceea-152">Ahogyan az ebben a dokumentumban, használhatja a nyers HTTP-kérelmek futtatásához, a figyelheti és a Sqoop feladat eredményeinek megtekintéséhez a HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="8ceea-152">As demonstrated in this document, you can use a raw HTTP request to run, monitor, and view the results of Sqoop jobs on your HDInsight cluster.</span></span>

<span data-ttu-id="8ceea-153">A cikk ezt használja a REST-felület további információkért tekintse meg a <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API útmutató</a>.</span><span class="sxs-lookup"><span data-stu-id="8ceea-153">For more information on the REST interface used in this article, see the <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API guide</a>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8ceea-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8ceea-154">Next steps</span></span>
<span data-ttu-id="8ceea-155">Általános információk a hdinsight Hive:</span><span class="sxs-lookup"><span data-stu-id="8ceea-155">For general information on Hive with HDInsight:</span></span>

* [<span data-ttu-id="8ceea-156">Sqoop használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="8ceea-156">Use Sqoop with Hadoop on HDInsight</span></span>](hdinsight-use-sqoop.md)

<span data-ttu-id="8ceea-157">Az egyéb lehetőségeiről a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="8ceea-157">For information on other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="8ceea-158">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="8ceea-158">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="8ceea-159">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="8ceea-159">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="8ceea-160">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="8ceea-160">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

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




[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


