---
title: "a HDInsight - Azure többi Hadoop Pig aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse REST toorun Pig Latin feladatok egy hadoop Azure hdinsight fürt."
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
ms.openlocfilehash: 760139e3caad9103d8c9d34e7f548d476014b5ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-rest"></a><span data-ttu-id="dac60-103">Pig-feladatokhoz a Hadoop on HDInsight használatával futtassa REST</span><span class="sxs-lookup"><span data-stu-id="dac60-103">Run Pig jobs with Hadoop on HDInsight by using REST</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="dac60-104">Ismerje meg, hogyan toorun Pig Latin feladatok azáltal, hogy a többi kérelmek tooan Azure HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="dac60-104">Learn how toorun Pig Latin jobs by making REST requests tooan Azure HDInsight cluster.</span></span> <span data-ttu-id="dac60-105">Curl használt toodemonstrate, és együttműködését HDInsight hello WebHCat REST API-t a rendszer.</span><span class="sxs-lookup"><span data-stu-id="dac60-105">Curl is used toodemonstrate how you can interact with HDInsight using hello WebHCat REST API.</span></span>

> [!NOTE]
> <span data-ttu-id="dac60-106">Ha ismeri a Linux-alapú Hadoop-kiszolgálókat használ, de új tooHDInsight, tekintse meg a [Linux-alapú HDInsight tippek](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="dac60-106">If you are already familiar with using Linux-based Hadoop servers, but are new tooHDInsight, see [Linux-based HDInsight Tips](hdinsight-hadoop-linux-information.md).</span></span>

## <span data-ttu-id="dac60-107"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dac60-107"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="dac60-108">(A HDInsight Hadoop) Azure HDInsight-fürtök (Linux- vagy Windows-alapú)</span><span class="sxs-lookup"><span data-stu-id="dac60-108">An Azure HDInsight (Hadoop on HDInsight) cluster (Linux-based or Windows-based)</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="dac60-109">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="dac60-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="dac60-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="dac60-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* [<span data-ttu-id="dac60-111">Curl</span><span class="sxs-lookup"><span data-stu-id="dac60-111">Curl</span></span>](http://curl.haxx.se/)

* [<span data-ttu-id="dac60-112">jq</span><span class="sxs-lookup"><span data-stu-id="dac60-112">jq</span></span>](http://stedolan.github.io/jq/)

## <span data-ttu-id="dac60-113"><a id="curl"></a>Pig-feladatokhoz Curl használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="dac60-113"><a id="curl"></a>Run Pig jobs by using Curl</span></span>

> [!NOTE]
> <span data-ttu-id="dac60-114">hello REST API védelméről [alapszintű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="dac60-114">hello REST API is secured via [basic access authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="dac60-115">Mindig végezzen kérelmeket, hogy a hitelesítő adatait biztonságos módon küldje toohello kiszolgáló biztonságos HTTP (HTTPS) tooensure.</span><span class="sxs-lookup"><span data-stu-id="dac60-115">Always make requests by using Secure HTTP (HTTPS) tooensure that your credentials are securely sent toohello server.</span></span>
>
> <span data-ttu-id="dac60-116">Ha ez a szakasz hello parancsokkal, cserélje le `USERNAME` hello felhasználói tooauthenticate toohello fürt, és cserélje ki a `PASSWORD` hello jelszóval hello felhasználói fiók.</span><span class="sxs-lookup"><span data-stu-id="dac60-116">When using hello commands in this section, replace `USERNAME` with hello user tooauthenticate toohello cluster, and replace `PASSWORD` with hello password for hello user account.</span></span> <span data-ttu-id="dac60-117">Cserélje le `CLUSTERNAME` hello néven a fürt.</span><span class="sxs-lookup"><span data-stu-id="dac60-117">Replace `CLUSTERNAME` with hello name of your cluster.</span></span>
>


1. <span data-ttu-id="dac60-118">A parancssorból a következő parancs tooverify, hogy csatlakozhasson tooyour HDInsight-fürt hello használata:</span><span class="sxs-lookup"><span data-stu-id="dac60-118">From a command line, use hello following command tooverify that you can connect tooyour HDInsight cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    <span data-ttu-id="dac60-119">A következő JSON-válasz hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="dac60-119">You should receive hello following JSON response:</span></span>

        {"status":"ok","version":"v1"}

    <span data-ttu-id="dac60-120">Ebben a parancsban használt hello paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="dac60-120">hello parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="dac60-121">**-u**: hello felhasználónevet és jelszót használt tooauthenticate hello kérelem</span><span class="sxs-lookup"><span data-stu-id="dac60-121">**-u**: hello user name and password used tooauthenticate hello request</span></span>
    * <span data-ttu-id="dac60-122">**-G**: azt jelzi, hogy a kérelem a GET kérés</span><span class="sxs-lookup"><span data-stu-id="dac60-122">**-G**: Indicates that this request is a GET request</span></span>

     <span data-ttu-id="dac60-123">hello hello URL-címe, eleje **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, van hello azonos minden olyan kérelem esetében.</span><span class="sxs-lookup"><span data-stu-id="dac60-123">hello beginning of hello URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hello same for all requests.</span></span> <span data-ttu-id="dac60-124">hello elérési **/status**, mutatja, hogy hello kérés WebHCat (más néven Templeton) állapotának tooreturn hello hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="dac60-124">hello path, **/status**, indicates that hello request is tooreturn hello status of WebHCat (also known as Templeton) for hello server.</span></span>

2. <span data-ttu-id="dac60-125">A következő kód toosubmit Pig Latin feladat toohello fürt hello használata:</span><span class="sxs-lookup"><span data-stu-id="dac60-125">Use hello following code toosubmit a Pig Latin job toohello cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'/example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="/example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig
    ```

    <span data-ttu-id="dac60-126">Ebben a parancsban használt hello paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="dac60-126">hello parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="dac60-127">**-d**: mivel `-G` nem használ, akkor hello kérelem alapértelmezés szerint használt érték toohello POST metódussal.</span><span class="sxs-lookup"><span data-stu-id="dac60-127">**-d**: Because `-G` is not used, hello request defaults toohello POST method.</span></span> <span data-ttu-id="dac60-128">`-d`Megadja a küldött hello adatértékek hello kérelemmel.</span><span class="sxs-lookup"><span data-stu-id="dac60-128">`-d` specifies hello data values that are sent with hello request.</span></span>

    * <span data-ttu-id="dac60-129">**User.name**: hello hello parancsot futtató felhasználó</span><span class="sxs-lookup"><span data-stu-id="dac60-129">**user.name**: hello user who is running hello command</span></span>
    * <span data-ttu-id="dac60-130">**végrehajtás**: hello Pig Latin utasítások tooexecute</span><span class="sxs-lookup"><span data-stu-id="dac60-130">**execute**: hello Pig Latin statements tooexecute</span></span>
    * <span data-ttu-id="dac60-131">**statusdir**: Ez a feladat állapotát hello hello könyvtár nevével</span><span class="sxs-lookup"><span data-stu-id="dac60-131">**statusdir**: hello directory that hello status for this job is written to</span></span>

    > [!NOTE]
    > <span data-ttu-id="dac60-132">Figyelje meg, hogy a Pig Latin utasításokban hello szóközöket helyébe hello `+` karakter Curl használata esetén.</span><span class="sxs-lookup"><span data-stu-id="dac60-132">Notice that hello spaces in Pig Latin statements are replaced by hello `+` character when used with Curl.</span></span>

    <span data-ttu-id="dac60-133">Ez a parancs visszaadja-e a feladat azonosítója, amely lehet például hello feladat, használt toocheck hello állapota:</span><span class="sxs-lookup"><span data-stu-id="dac60-133">This command should return a job ID that can be used toocheck hello status of hello job, for example:</span></span>

        {"id":"job_1415651640909_0026"}

3. <span data-ttu-id="dac60-134">hello feladat, a következő parancs használata hello toocheck hello állapota</span><span class="sxs-lookup"><span data-stu-id="dac60-134">toocheck hello status of hello job, use hello following command</span></span>

     ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

     <span data-ttu-id="dac60-135">Cserélje le `JOBID` hello előző lépés eredményeképpen visszakapott hello értékkel.</span><span class="sxs-lookup"><span data-stu-id="dac60-135">Replace `JOBID` with hello value returned in hello previous step.</span></span> <span data-ttu-id="dac60-136">Például ha hello visszatérési érték volt `{"id":"job_1415651640909_0026"}`, majd `JOBID` van `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="dac60-136">For example, if hello return value was `{"id":"job_1415651640909_0026"}`, then `JOBID` is `job_1415651640909_0026`.</span></span>

    <span data-ttu-id="dac60-137">Ha hello feladat befejeződött, hello állapota **sikeres**.</span><span class="sxs-lookup"><span data-stu-id="dac60-137">If hello job has finished, hello state is **SUCCEEDED**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dac60-138">A Curl kérelmeket értéket ad vissza a JavaScript Object Notation (JSON)-dokumentum található információkat hello feladat, illetve jq használt tooretrieve csak hello állapotérték.</span><span class="sxs-lookup"><span data-stu-id="dac60-138">This Curl request returns a JavaScript Object Notation (JSON) document with information about hello job, and jq is used tooretrieve only hello state value.</span></span>

## <span data-ttu-id="dac60-139"><a id="results"></a>Az eredmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="dac60-139"><a id="results"></a>View results</span></span>

<span data-ttu-id="dac60-140">Ha hello állapota hello feladat túl megváltozott,**sikeres**, hello feladat eredményeinek hello kérheti le.</span><span class="sxs-lookup"><span data-stu-id="dac60-140">When hello state of hello job has changed too**SUCCEEDED**, you can retrieve hello results of hello job.</span></span> <span data-ttu-id="dac60-141">Hello `statusdir` hello lekérdezéssel átadott paraméter tartalmaz hello hely hello kimeneti fájl; ebben az esetben az `/example/pigcurl`.</span><span class="sxs-lookup"><span data-stu-id="dac60-141">hello `statusdir` parameter passed with hello query contains hello location of hello output file; in this case, `/example/pigcurl`.</span></span>

<span data-ttu-id="dac60-142">HDInsight a hello alapértelmezett adatokat tároló Azure Storage vagy az Azure Data Lake Store is használhatja.</span><span class="sxs-lookup"><span data-stu-id="dac60-142">HDInsight can use either Azure Storage or Azure Data Lake Store as hello default data store.</span></span> <span data-ttu-id="dac60-143">Nincsenek különböző módokon tooget hello adatait attól függően, melyiket használja.</span><span class="sxs-lookup"><span data-stu-id="dac60-143">There are various ways tooget at hello data depending on which one you use.</span></span> <span data-ttu-id="dac60-144">További információ a hello hello tárolási című szakaszában talál [Linux-alapú HDInsight-információk](hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="dac60-144">For more information, see hello storage section of hello [Linux-based HDInsight information](hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) document.</span></span>

## <span data-ttu-id="dac60-145"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="dac60-145"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="dac60-146">Ahogyan az a dokumentum, egy nyers HTTP-kérelem toorun, a figyelő és hello eredmények megtekintése a Pig feladatot használhatja a HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="dac60-146">As demonstrated in this document, you can use a raw HTTP request toorun, monitor, and view hello results of Pig jobs on your HDInsight cluster.</span></span>

<span data-ttu-id="dac60-147">A cikk ezt használja a hello REST-felület kapcsolatos további információkért lásd: hello [WebHCat hivatkozás](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span><span class="sxs-lookup"><span data-stu-id="dac60-147">For more information about hello REST interface used in this article, see hello [WebHCat Reference](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span></span>

## <span data-ttu-id="dac60-148"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dac60-148"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="dac60-149">A Pig általános információk a HDInsight:</span><span class="sxs-lookup"><span data-stu-id="dac60-149">For general information about Pig on HDInsight:</span></span>

* [<span data-ttu-id="dac60-150">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="dac60-150">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="dac60-151">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="dac60-151">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="dac60-152">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="dac60-152">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="dac60-153">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="dac60-153">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
