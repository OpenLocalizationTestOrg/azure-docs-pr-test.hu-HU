---
title: a hdinsight - Azure Curl Hadoop Sqoop aaaUse |} Microsoft Docs
description: "Ismerje meg, hogyan nyújt az tooremotely a Sqoop feladatok tooHDInsight használata Curl használatával."
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
ms.openlocfilehash: d9c09a6704ab6c5f48be50ed6d6314ec406df8ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a><span data-ttu-id="787cf-103">Sqoop feladatok futtatása a hadooppal a Hdinsightban a Curl</span><span class="sxs-lookup"><span data-stu-id="787cf-103">Run Sqoop jobs with Hadoop in HDInsight with Curl</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="787cf-104">Ismerje meg, hogyan toouse Curl toorun Sqoop feladatok egy hadoop a Hdinsightban fürt.</span><span class="sxs-lookup"><span data-stu-id="787cf-104">Learn how toouse Curl toorun Sqoop jobs on a Hadoop cluster in HDInsight.</span></span>

<span data-ttu-id="787cf-105">Curl, és együttműködését HDInsight nyers HTTP-kérelmek toorun, a figyelő és a Sqoop feladatok eredményeinek lekérése hello segítségével használt toodemonstrate.</span><span class="sxs-lookup"><span data-stu-id="787cf-105">Curl is used toodemonstrate how you can interact with HDInsight by using raw HTTP requests toorun, monitor, and retrieve hello results of Sqoop jobs.</span></span> <span data-ttu-id="787cf-106">Ez a módszer hello WebHCat REST API-t (korábbi nevén lépni a Templeton) a HDInsight-fürt által biztosított használatával.</span><span class="sxs-lookup"><span data-stu-id="787cf-106">This works by using hello WebHCat REST API (formerly known as Templeton) provided by your HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="787cf-107">Ha ismeri a Linux-alapú Hadoop-kiszolgálókat használ, de új tooHDInsight, tekintse meg a [információ a HDInsight használata Linux](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="787cf-107">If you are already familiar with using Linux-based Hadoop servers, but are new tooHDInsight, see [Information about using HDInsight on Linux](hdinsight-hadoop-linux-information.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="787cf-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="787cf-108">Prerequisites</span></span>
<span data-ttu-id="787cf-109">toocomplete hello cikkben leírt lépéseket, szüksége lesz a következő hello:</span><span class="sxs-lookup"><span data-stu-id="787cf-109">toocomplete hello steps in this article, you will need hello following:</span></span>

* <span data-ttu-id="787cf-110">A Hadoop on HDInsight-fürt (Linux vagy a Windows-alapú)</span><span class="sxs-lookup"><span data-stu-id="787cf-110">A Hadoop on HDInsight cluster (Linux or Windows-based)</span></span>
* [<span data-ttu-id="787cf-111">Curl</span><span class="sxs-lookup"><span data-stu-id="787cf-111">Curl</span></span>](http://curl.haxx.se/)
* [<span data-ttu-id="787cf-112">jq</span><span class="sxs-lookup"><span data-stu-id="787cf-112">jq</span></span>](http://stedolan.github.io/jq/)

## <a name="submit-sqoop-jobs-by-using-curl"></a><span data-ttu-id="787cf-113">Sqoop feladatok elküldéséhez a Curl használatával</span><span class="sxs-lookup"><span data-stu-id="787cf-113">Submit Sqoop jobs by using Curl</span></span>
> [!NOTE]
> <span data-ttu-id="787cf-114">Használata Curl vagy más REST kommunikációt használ a Webhcattel, hitelesítenie kell hello kérelmek hello felhasználónevet és jelszót hello HDInsight fürt rendszergazdája által.</span><span class="sxs-lookup"><span data-stu-id="787cf-114">When using Curl or any other REST communication with WebHCat, you must authenticate hello requests by providing hello user name and password for hello HDInsight cluster administrator.</span></span> <span data-ttu-id="787cf-115">Hello egységes erőforrás-azonosító (URI) részeként használt toosend hello kérelmek toohello server hello fürtnév is kell használnia.</span><span class="sxs-lookup"><span data-stu-id="787cf-115">You must also use hello cluster name as part of hello Uniform Resource Identifier (URI) used toosend hello requests toohello server.</span></span>
> 
> <span data-ttu-id="787cf-116">Ezen szakasz parancsaiban hello, cserélje le **felhasználónév** hello felhasználói tooauthenticate toohello fürt, és cserélje ki a **jelszó** a hello hello felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="787cf-116">For hello commands in this section, replace **USERNAME** with hello user tooauthenticate toohello cluster, and replace **PASSWORD** with hello password for hello user account.</span></span> <span data-ttu-id="787cf-117">Cserélje le **CLUSTERNAME** hello néven a fürt.</span><span class="sxs-lookup"><span data-stu-id="787cf-117">Replace **CLUSTERNAME** with hello name of your cluster.</span></span>
> 
> <span data-ttu-id="787cf-118">hello REST API védelméről [az egyszerű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="787cf-118">hello REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="787cf-119">Mindig ügyeljen kérelmek biztonságos HTTP (HTTPS) toohelp győződjön meg arról, hogy a hitelesítő adatait biztonságos módon küldje toohello kiszolgáló használatával.</span><span class="sxs-lookup"><span data-stu-id="787cf-119">You should always make requests by using Secure HTTP (HTTPS) toohelp ensure that your credentials are securely sent toohello server.</span></span>
> 
> 

1. <span data-ttu-id="787cf-120">A parancssorból a következő parancs tooverify, hogy csatlakozhasson tooyour HDInsight-fürt hello használata:</span><span class="sxs-lookup"><span data-stu-id="787cf-120">From a command line, use hello following command tooverify that you can connect tooyour HDInsight cluster:</span></span>
   
        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
   
    <span data-ttu-id="787cf-121">Egy hasonló toohello következő választ kell kapnia:</span><span class="sxs-lookup"><span data-stu-id="787cf-121">You should receive a response similar toohello following:</span></span>
   
        {"status":"ok","version":"v1"}
   
    <span data-ttu-id="787cf-122">Ebben a parancsban használt hello paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="787cf-122">hello parameters used in this command are as follows:</span></span>
   
   * <span data-ttu-id="787cf-123">**-u** -hello felhasználónevet és jelszót használt tooauthenticate hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="787cf-123">**-u** - hello user name and password used tooauthenticate hello request.</span></span>
   * <span data-ttu-id="787cf-124">**-G** - Jelzi, hogy ez egy GET kérés.</span><span class="sxs-lookup"><span data-stu-id="787cf-124">**-G** - Indicates that this is a GET request.</span></span>
     
     <span data-ttu-id="787cf-125">hello hello URL-címe, eleje **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, minden olyan kérelem esetében hello azonos lesz.</span><span class="sxs-lookup"><span data-stu-id="787cf-125">hello beginning of hello URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, will be hello same for all requests.</span></span> <span data-ttu-id="787cf-126">hello elérési **/status**, mutatja, hogy hello kérés tooreturn WebHCat (más néven Templeton) állapota hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="787cf-126">hello path, **/status**, indicates that hello request is tooreturn a status of WebHCat (also known as Templeton) for hello server.</span></span> 
2. <span data-ttu-id="787cf-127">A következő toosubmit a sqoop feladat hello használata:</span><span class="sxs-lookup"><span data-stu-id="787cf-127">Use hello following toosubmit a sqoop job:</span></span>

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    <span data-ttu-id="787cf-128">Ebben a parancsban használt hello paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="787cf-128">hello parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="787cf-129">**-d** - óta `-G` nem használ, akkor hello kérelem alapértelmezés szerint használt érték toohello POST metódussal.</span><span class="sxs-lookup"><span data-stu-id="787cf-129">**-d** - Since `-G` is not used, hello request defaults toohello POST method.</span></span> <span data-ttu-id="787cf-130">`-d`Megadja a küldött hello adatértékek hello kérelemmel.</span><span class="sxs-lookup"><span data-stu-id="787cf-130">`-d` specifies hello data values that are sent with hello request.</span></span>

        * <span data-ttu-id="787cf-131">**User.name** -hello parancsot futtató felhasználónak hello.</span><span class="sxs-lookup"><span data-stu-id="787cf-131">**user.name** - hello user that is running hello command.</span></span>

        * <span data-ttu-id="787cf-132">**a parancs** -Sqoop parancs tooexecute hello.</span><span class="sxs-lookup"><span data-stu-id="787cf-132">**command** - hello Sqoop command tooexecute.</span></span>

        * <span data-ttu-id="787cf-133">**statusdir** – Ez a feladat állapotát hello hello könyvtár lesz írva.</span><span class="sxs-lookup"><span data-stu-id="787cf-133">**statusdir** - hello directory that hello status for this job will be written to.</span></span>

    <span data-ttu-id="787cf-134">Ez a parancs a feladat azonosítója, amely hello feladat állapotának használt toocheck hello kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="787cf-134">This command should return a job ID that can be used toocheck hello status of hello job.</span></span>

        {"id":"job_1415651640909_0026"}

1. <span data-ttu-id="787cf-135">hello feladat, a következő parancs használata hello toocheck hello állapota.</span><span class="sxs-lookup"><span data-stu-id="787cf-135">toocheck hello status of hello job, use hello following command.</span></span> <span data-ttu-id="787cf-136">Cserélje le **JOBID** hello előző lépés eredményeképpen visszakapott hello értékkel.</span><span class="sxs-lookup"><span data-stu-id="787cf-136">Replace **JOBID** with hello value returned in hello previous step.</span></span> <span data-ttu-id="787cf-137">Például ha hello visszatérési érték volt `{"id":"job_1415651640909_0026"}`, majd **JOBID** lenne `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="787cf-137">For example, if hello return value was `{"id":"job_1415651640909_0026"}`, then **JOBID** would be `job_1415651640909_0026`.</span></span>
   
        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
   
    <span data-ttu-id="787cf-138">Ha hello feladat befejeződött, hello állapotban lesz **sikeres**.</span><span class="sxs-lookup"><span data-stu-id="787cf-138">If hello job has finished, hello state will be **SUCCEEDED**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="787cf-139">A Curl kérelem egy JavaScript Object Notation (JSON) dokumentumot hello feladat; információkat ad vissza jq használt tooretrieve csak hello állapotérték.</span><span class="sxs-lookup"><span data-stu-id="787cf-139">This Curl request returns a JavaScript Object Notation (JSON) document with information about hello job; jq is used tooretrieve only hello state value.</span></span>
   > 
   > 
2. <span data-ttu-id="787cf-140">Miután hello feladat hello állapota megváltozott túl**sikeres**, az Azure Blob storage hello feladat eredményeinek hello kérheti le.</span><span class="sxs-lookup"><span data-stu-id="787cf-140">Once hello state of hello job has changed too**SUCCEEDED**, you can retrieve hello results of hello job from Azure Blob storage.</span></span> <span data-ttu-id="787cf-141">Hello `statusdir` hello lekérdezéssel átadott paraméter tartalmaz hello hely hello kimeneti fájl; ebben az esetben az **wasb: / / / Példa/curl**.</span><span class="sxs-lookup"><span data-stu-id="787cf-141">hello `statusdir` parameter passed with hello query contains hello location of hello output file; in this case, **wasb:///example/curl**.</span></span> <span data-ttu-id="787cf-142">Ez a cím hello feladat eredményének hello tárol hello **példa/curl** hello alapértelmezett tárolót a HDInsight-fürt által használt könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="787cf-142">This address stores hello output of hello job in hello **example/curl** directory on hello default storage container used by your HDInsight cluster.</span></span>
   
    <span data-ttu-id="787cf-143">Listáról, és ezek a fájlok letöltésére hello [Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="787cf-143">You can list and download these files by using hello [Azure CLI](../cli-install-nodejs.md).</span></span> <span data-ttu-id="787cf-144">Például adatbázisfájlok toolist **példa/curl**, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="787cf-144">For example, toolist files in **example/curl**, use hello following command:</span></span>
   
        azure storage blob list <container-name> example/curl
   
    <span data-ttu-id="787cf-145">egy fájl toodownload hello következő használja:</span><span class="sxs-lookup"><span data-stu-id="787cf-145">toodownload a file, use hello following:</span></span>
   
        azure storage blob download <container-name> <blob-name> <destination-file>
   
   > [!NOTE]
   > <span data-ttu-id="787cf-146">Meg kell adnia hello tárfiók neve, amely tartalmazza a hello blob hello segítségével `-a` és `-k` paramétereket, vagy a set hello **AZURE\_tárolási\_fiók** és **AZURE\_tárolási\_hozzáférés\_kulcs** környezeti változókat.</span><span class="sxs-lookup"><span data-stu-id="787cf-146">You must either specify hello storage account name that contains hello blob by using hello `-a` and `-k` parameters, or set hello **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS\_KEY** environment variables.</span></span> <span data-ttu-id="787cf-147">Lásd: < a href = "a hdinsight-feltöltési-data.md" target = "_blank" További információ.</span><span class="sxs-lookup"><span data-stu-id="787cf-147">See <a href="hdinsight-upload-data.md" target="_blank" for more information.</span></span>
   > 
   > 

## <a name="limitations"></a><span data-ttu-id="787cf-148">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="787cf-148">Limitations</span></span>
* <span data-ttu-id="787cf-149">Tömeges export - a Linux-alapú HDInsight, hello Sqoop használt tooexport adatok tooMicrosoft SQL Server vagy az Azure SQL Database jelenleg nem támogatja a tömeges beszúrások.</span><span class="sxs-lookup"><span data-stu-id="787cf-149">Bulk export - With Linux-based HDInsight, hello Sqoop connector used tooexport data tooMicrosoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="787cf-150">Kötegelés – a Linux-alapú hdinsight eszközzel, hello használatakor `-batch` Beszúrások végrehajtásakor kapcsoló, a Sqoop hajt végre több Beszúrások hello beszúrási műveletek kötegelése helyett.</span><span class="sxs-lookup"><span data-stu-id="787cf-150">Batching - With Linux-based HDInsight, When using hello `-batch` switch when performing inserts, Sqoop will perform multiple inserts instead of batching hello insert operations.</span></span>

## <a name="summary"></a><span data-ttu-id="787cf-151">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="787cf-151">Summary</span></span>
<span data-ttu-id="787cf-152">Ahogyan az a dokumentum, egy nyers HTTP-kérelem toorun, a figyelő és hello eredmények megtekintése a Sqoop feladatok használhatja a HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="787cf-152">As demonstrated in this document, you can use a raw HTTP request toorun, monitor, and view hello results of Sqoop jobs on your HDInsight cluster.</span></span>

<span data-ttu-id="787cf-153">A cikk ezt használja a hello REST-felület további információkért lásd: hello <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API útmutató</a>.</span><span class="sxs-lookup"><span data-stu-id="787cf-153">For more information on hello REST interface used in this article, see hello <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API guide</a>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="787cf-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="787cf-154">Next steps</span></span>
<span data-ttu-id="787cf-155">Általános információk a hdinsight Hive:</span><span class="sxs-lookup"><span data-stu-id="787cf-155">For general information on Hive with HDInsight:</span></span>

* [<span data-ttu-id="787cf-156">Sqoop használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="787cf-156">Use Sqoop with Hadoop on HDInsight</span></span>](hdinsight-use-sqoop.md)

<span data-ttu-id="787cf-157">Az egyéb lehetőségeiről a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="787cf-157">For information on other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="787cf-158">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="787cf-158">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="787cf-159">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="787cf-159">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="787cf-160">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="787cf-160">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

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


