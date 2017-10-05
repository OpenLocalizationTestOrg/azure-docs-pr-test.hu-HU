---
title: "Hadoop a Pig használata a HDInsight - Azure REST |} Microsoft Docs"
description: "Megtudhatja, hogyan használja a többi Azure hdinsight Hadoop-fürthöz Pig Latin feladatok futtatásához."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ed5e10d1-4f47-459c-a0d6-7ff967b468c4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: a86864a779b0de1c6d5669cfbba0f3e1a27f1ff1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-rest"></a><span data-ttu-id="860a7-103">Pig-feladatokhoz a Hadoop on HDInsight használatával futtassa REST</span><span class="sxs-lookup"><span data-stu-id="860a7-103">Run Pig jobs with Hadoop on HDInsight by using REST</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="860a7-104">Útmutató: Azure HDInsight-fürtök REST-kérelmek, így a Pig Latin feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="860a7-104">Learn how to run Pig Latin jobs by making REST requests to an Azure HDInsight cluster.</span></span> <span data-ttu-id="860a7-105">Curl használatával bemutatják, hogyan kezelheti a HDInsight a WebHCat REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="860a7-105">Curl is used to demonstrate how you can interact with HDInsight using the WebHCat REST API.</span></span>

> [!NOTE]
> <span data-ttu-id="860a7-106">Ha ismeri a Linux-alapú Hadoop-kiszolgálókat használ, de még nem használta a HDInsight, lásd: [Linux-alapú HDInsight tippek](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="860a7-106">If you are already familiar with using Linux-based Hadoop servers, but are new to HDInsight, see [Linux-based HDInsight Tips](hdinsight-hadoop-linux-information.md).</span></span>

## <span data-ttu-id="860a7-107"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="860a7-107"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="860a7-108">(A HDInsight Hadoop) Azure HDInsight-fürtök (Linux- vagy Windows-alapú)</span><span class="sxs-lookup"><span data-stu-id="860a7-108">An Azure HDInsight (Hadoop on HDInsight) cluster (Linux-based or Windows-based)</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="860a7-109">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="860a7-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="860a7-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="860a7-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* [<span data-ttu-id="860a7-111">Curl</span><span class="sxs-lookup"><span data-stu-id="860a7-111">Curl</span></span>](http://curl.haxx.se/)

* [<span data-ttu-id="860a7-112">jq</span><span class="sxs-lookup"><span data-stu-id="860a7-112">jq</span></span>](http://stedolan.github.io/jq/)

## <span data-ttu-id="860a7-113"><a id="curl"></a>Pig-feladatokhoz Curl használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="860a7-113"><a id="curl"></a>Run Pig jobs by using Curl</span></span>

> [!NOTE]
> <span data-ttu-id="860a7-114">A REST API védelméről [alapszintű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="860a7-114">The REST API is secured via [basic access authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="860a7-115">Mindig végezzen kérelmek HTTP Secure (HTTPS) annak érdekében, hogy a hitelesítő adatait biztonságos módon küldje a kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="860a7-115">Always make requests by using Secure HTTP (HTTPS) to ensure that your credentials are securely sent to the server.</span></span>
>
> <span data-ttu-id="860a7-116">A parancsok használata ebben a szakaszban, cserélje le a `USERNAME` a fürtön, és cserélje le a felhasználó `PASSWORD` a felhasználói fiók jelszavával.</span><span class="sxs-lookup"><span data-stu-id="860a7-116">When using the commands in this section, replace `USERNAME` with the user to authenticate to the cluster, and replace `PASSWORD` with the password for the user account.</span></span> <span data-ttu-id="860a7-117">Cserélje le a `CLUSTERNAME` elemet a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="860a7-117">Replace `CLUSTERNAME` with the name of your cluster.</span></span>
>


1. <span data-ttu-id="860a7-118">Egy parancssorból a következő paranccsal ellenőrizze, hogy tud-e kapcsolódni a HDInsight-fürthöz:</span><span class="sxs-lookup"><span data-stu-id="860a7-118">From a command line, use the following command to verify that you can connect to your HDInsight cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    <span data-ttu-id="860a7-119">A következő JSON-választ kell kapnia:</span><span class="sxs-lookup"><span data-stu-id="860a7-119">You should receive the following JSON response:</span></span>

        {"status":"ok","version":"v1"}

    <span data-ttu-id="860a7-120">Ezen parancs paraméterei a következők:</span><span class="sxs-lookup"><span data-stu-id="860a7-120">The parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="860a7-121">**-u**: A felhasználói nevet és a kérés hitelesítéséhez használt jelszó</span><span class="sxs-lookup"><span data-stu-id="860a7-121">**-u**: The user name and password used to authenticate the request</span></span>
    * <span data-ttu-id="860a7-122">**-G**: azt jelzi, hogy a kérelem a GET kérés</span><span class="sxs-lookup"><span data-stu-id="860a7-122">**-G**: Indicates that this request is a GET request</span></span>

     <span data-ttu-id="860a7-123">Az URL-cím, az elején **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, minden olyan kérelem esetében megegyezik.</span><span class="sxs-lookup"><span data-stu-id="860a7-123">The beginning of the URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is the same for all requests.</span></span> <span data-ttu-id="860a7-124">Az elérési út **/status**, azt jelzi, hogy a kérelem vissza WebHCat (más néven Templeton) állapotának a kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="860a7-124">The path, **/status**, indicates that the request is to return the status of WebHCat (also known as Templeton) for the server.</span></span>

2. <span data-ttu-id="860a7-125">A következő kód segítségével elküldeni a Pig Latin feladatot a fürt:</span><span class="sxs-lookup"><span data-stu-id="860a7-125">Use the following code to submit a Pig Latin job to the cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'/example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="/example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig
    ```

    <span data-ttu-id="860a7-126">Ezen parancs paraméterei a következők:</span><span class="sxs-lookup"><span data-stu-id="860a7-126">The parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="860a7-127">**-d**: mivel `-G` nem használ, akkor a kérelmet az alapértelmezett a POST metódussal.</span><span class="sxs-lookup"><span data-stu-id="860a7-127">**-d**: Because `-G` is not used, the request defaults to the POST method.</span></span> <span data-ttu-id="860a7-128">`-d`Megadja a küldött adatértékekkel mellékel a kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="860a7-128">`-d` specifies the data values that are sent with the request.</span></span>

    * <span data-ttu-id="860a7-129">**User.name**: a parancsot futtató felhasználó</span><span class="sxs-lookup"><span data-stu-id="860a7-129">**user.name**: The user who is running the command</span></span>
    * <span data-ttu-id="860a7-130">**végrehajtás**: A Pig Latin utasítás végrehajtása</span><span class="sxs-lookup"><span data-stu-id="860a7-130">**execute**: The Pig Latin statements to execute</span></span>
    * <span data-ttu-id="860a7-131">**statusdir**: A könyvtár írt meg a feladat állapota</span><span class="sxs-lookup"><span data-stu-id="860a7-131">**statusdir**: The directory that the status for this job is written to</span></span>

    > [!NOTE]
    > <span data-ttu-id="860a7-132">Figyelje meg, hogy a tárolóhelyek a Pig Latin utasításokban helyébe a `+` karakter Curl használata esetén.</span><span class="sxs-lookup"><span data-stu-id="860a7-132">Notice that the spaces in Pig Latin statements are replaced by the `+` character when used with Curl.</span></span>

    <span data-ttu-id="860a7-133">Ez a parancs visszaadja-e a Feladatazonosítót a feladat állapotának ellenőrzéséhez például használható:</span><span class="sxs-lookup"><span data-stu-id="860a7-133">This command should return a job ID that can be used to check the status of the job, for example:</span></span>

        {"id":"job_1415651640909_0026"}

3. <span data-ttu-id="860a7-134">A feladat állapotának ellenőrzéséhez használja a következő parancs</span><span class="sxs-lookup"><span data-stu-id="860a7-134">To check the status of the job, use the following command</span></span>

     ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

     <span data-ttu-id="860a7-135">Cserélje le `JOBID` az előző lépésben visszaadott értékkel.</span><span class="sxs-lookup"><span data-stu-id="860a7-135">Replace `JOBID` with the value returned in the previous step.</span></span> <span data-ttu-id="860a7-136">Például, ha a visszatérési érték volt `{"id":"job_1415651640909_0026"}`, majd `JOBID` van `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="860a7-136">For example, if the return value was `{"id":"job_1415651640909_0026"}`, then `JOBID` is `job_1415651640909_0026`.</span></span>

    <span data-ttu-id="860a7-137">Ha a feladat befejeződött, az állapota **sikeres**.</span><span class="sxs-lookup"><span data-stu-id="860a7-137">If the job has finished, the state is **SUCCEEDED**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="860a7-138">A Curl kérelem egy JavaScript Object Notation (JSON) dokumentumot, a feladat információkat ad vissza, és csak a állapotérték beolvasásához használt jq.</span><span class="sxs-lookup"><span data-stu-id="860a7-138">This Curl request returns a JavaScript Object Notation (JSON) document with information about the job, and jq is used to retrieve only the state value.</span></span>

## <span data-ttu-id="860a7-139"><a id="results"></a>Az eredmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="860a7-139"><a id="results"></a>View results</span></span>

<span data-ttu-id="860a7-140">Ha a feladat állapota módosult **sikeres**, a feladat eredményeinek kérheti le.</span><span class="sxs-lookup"><span data-stu-id="860a7-140">When the state of the job has changed to **SUCCEEDED**, you can retrieve the results of the job.</span></span> <span data-ttu-id="860a7-141">A `statusdir` a lekérdezés átadott paraméter tartalmazza a hely a kimeneti fájl; ebben az esetben az `/example/pigcurl`.</span><span class="sxs-lookup"><span data-stu-id="860a7-141">The `statusdir` parameter passed with the query contains the location of the output file; in this case, `/example/pigcurl`.</span></span>

<span data-ttu-id="860a7-142">HDInsight a alapértelmezett adattárként Azure Storage vagy az Azure Data Lake Store is használhatja.</span><span class="sxs-lookup"><span data-stu-id="860a7-142">HDInsight can use either Azure Storage or Azure Data Lake Store as the default data store.</span></span> <span data-ttu-id="860a7-143">Különböző módon lehet beolvasni az adatokat, attól függően, melyiket használja.</span><span class="sxs-lookup"><span data-stu-id="860a7-143">There are various ways to get at the data depending on which one you use.</span></span> <span data-ttu-id="860a7-144">További információkért lásd: a tárolási szakasza a [Linux-alapú HDInsight-információk](hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="860a7-144">For more information, see the storage section of the [Linux-based HDInsight information](hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) document.</span></span>

## <span data-ttu-id="860a7-145"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="860a7-145"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="860a7-146">Ahogyan az ebben a dokumentumban, használhatja a nyers HTTP-kérelmek futtatásához, a figyelheti és a Pig-feladatokhoz eredményeinek megtekintéséhez a HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="860a7-146">As demonstrated in this document, you can use a raw HTTP request to run, monitor, and view the results of Pig jobs on your HDInsight cluster.</span></span>

<span data-ttu-id="860a7-147">A cikk ezt használja a REST-felület kapcsolatos további információkért tekintse meg a [WebHCat hivatkozás](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span><span class="sxs-lookup"><span data-stu-id="860a7-147">For more information about the REST interface used in this article, see the [WebHCat Reference](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span></span>

## <span data-ttu-id="860a7-148"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="860a7-148"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="860a7-149">A Pig általános információk a HDInsight:</span><span class="sxs-lookup"><span data-stu-id="860a7-149">For general information about Pig on HDInsight:</span></span>

* [<span data-ttu-id="860a7-150">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="860a7-150">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="860a7-151">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="860a7-151">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="860a7-152">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="860a7-152">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="860a7-153">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="860a7-153">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
