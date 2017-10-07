---
title: "aaaUse MapReduce és a hadooppal a Hdinsightban - Azure Curl |} Microsoft Docs"
description: "Ismerje meg, hogyan tooremotely hibaüzenettel MapReduce-feladatok Hadoop on HDInsight használata Curl használatával."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: bc6daf37-fcdc-467a-a8a8-6fb2f0f773d1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 16920205bacf9699f88090568099e0508a172b3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a><span data-ttu-id="5d8ce-103">A HDInsight a többi hadooppal futtatási MapReduce-feladatok</span><span class="sxs-lookup"><span data-stu-id="5d8ce-103">Run MapReduce jobs with Hadoop on HDInsight using REST</span></span>

<span data-ttu-id="5d8ce-104">Ismerje meg, hogyan toouse hello WebHCat REST API toorun MapReduce a hadoop on HDInsight-fürt feladatokat.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-104">Learn how toouse hello WebHCat REST API toorun MapReduce jobs on a Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="5d8ce-105">Curl, és együttműködését HDInsight nyers HTTP kérelmeket toorun MapReduce-feladatok használatával használt toodemonstrate.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-105">Curl is used toodemonstrate how you can interact with HDInsight by using raw HTTP requests toorun MapReduce jobs.</span></span>

> [!NOTE]
> <span data-ttu-id="5d8ce-106">Ha már ismeri a Linux-alapú Hadoop-kiszolgálókat használ, de új tooHDInsight áll, tekintse meg a hello [mire van szüksége a HDInsight Linux-alapú Hadooppal kapcsolatos tooknow](hdinsight-hadoop-linux-information.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-106">If you are already familiar with using Linux-based Hadoop servers, but you are new tooHDInsight, see hello [What you need tooknow about Linux-based Hadoop on HDInsight](hdinsight-hadoop-linux-information.md) document.</span></span>


## <span data-ttu-id="5d8ce-107"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5d8ce-107"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="5d8ce-108">A Hadoop on HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="5d8ce-108">A Hadoop on HDInsight cluster</span></span>
* [<span data-ttu-id="5d8ce-109">Curl</span><span class="sxs-lookup"><span data-stu-id="5d8ce-109">Curl</span></span>](http://curl.haxx.se/)
* [<span data-ttu-id="5d8ce-110">jq</span><span class="sxs-lookup"><span data-stu-id="5d8ce-110">jq</span></span>](http://stedolan.github.io/jq/)

## <span data-ttu-id="5d8ce-111"><a id="curl"></a>MapReduce-feladatok használata Curl használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="5d8ce-111"><a id="curl"></a>Run MapReduce jobs using Curl</span></span>

> [!NOTE]
> <span data-ttu-id="5d8ce-112">Használatakor Curl vagy más REST kommunikációt használ a Webhcattel, hitelesítenie kell hello kérelmek hello HDInsight fürt rendszergazdai felhasználónév és jelszó megadásával.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-112">When you use Curl or any other REST communication with WebHCat, you must authenticate hello requests by providing hello HDInsight cluster administrator user name and password.</span></span> <span data-ttu-id="5d8ce-113">Hello fürtnév hello használt toosend hello kérelmek toohello kiszolgáló URI részeként kell használnia.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-113">You must use hello cluster name as part of hello URI that is used toosend hello requests toohello server.</span></span>
>
> <span data-ttu-id="5d8ce-114">Ezen szakasz parancsaiban hello, cserélje le **felhasználónév** hello felhasználói tooauthenticate toohello fürttel, és **jelszó** a hello hello felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-114">For hello commands in this section, replace **USERNAME** with hello user tooauthenticate toohello cluster, and **PASSWORD** with hello password for hello user account.</span></span> <span data-ttu-id="5d8ce-115">Cserélje le **CLUSTERNAME** hello néven a fürt.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-115">Replace **CLUSTERNAME** with hello name of your cluster.</span></span>
>
> <span data-ttu-id="5d8ce-116">hello REST API által védett használatával [alapszintű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="5d8ce-116">hello REST API is secured by using [basic access authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="5d8ce-117">Mindig ügyeljen kérelmek HTTPS tooensure, hogy a hitelesítő adatait biztonságos módon küldje toohello kiszolgáló használatával.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-117">You should always make requests by using HTTPS tooensure that your credentials are securely sent toohello server.</span></span>


1. <span data-ttu-id="5d8ce-118">A parancssorból a következő parancs tooverify, hogy csatlakozhasson tooyour HDInsight-fürt hello használata:</span><span class="sxs-lookup"><span data-stu-id="5d8ce-118">From a command line, use hello following command tooverify that you can connect tooyour HDInsight cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    <span data-ttu-id="5d8ce-119">A válasz a következő JSON hasonló toohello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="5d8ce-119">You should receive a response similar toohello following JSON:</span></span>

        {"status":"ok","version":"v1"}

    <span data-ttu-id="5d8ce-120">Ebben a parancsban használt hello paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="5d8ce-120">hello parameters used in this command are as follows:</span></span>

   * <span data-ttu-id="5d8ce-121">**-u**: azt jelzi, hogy hello felhasználónevet és jelszót használt tooauthenticate hello kérelem</span><span class="sxs-lookup"><span data-stu-id="5d8ce-121">**-u**: Indicates hello user name and password used tooauthenticate hello request</span></span>
   * <span data-ttu-id="5d8ce-122">**-G**: azt jelzi, hogy ez a művelet egy GET kérés</span><span class="sxs-lookup"><span data-stu-id="5d8ce-122">**-G**: Indicates that this operation is a GET request</span></span>

     <span data-ttu-id="5d8ce-123">URI, hello eleje hello **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, van hello azonos minden olyan kérelem esetében.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-123">hello beginning of hello URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hello same for all requests.</span></span>

2. <span data-ttu-id="5d8ce-124">toosubmit MapReduce feladatot, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="5d8ce-124">toosubmit a MapReduce job, use hello following command:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
    ```

    <span data-ttu-id="5d8ce-125">hello (vagy mapreduce/jar) URI hello végét közli a WebHCat, hogy a kérelem osztályból jar-fájlra a MapReduce feladatot elindul.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-125">hello end of hello URI (/mapreduce/jar) tells WebHCat that this request starts a MapReduce job from a class in a jar file.</span></span> <span data-ttu-id="5d8ce-126">Ebben a parancsban használt hello paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="5d8ce-126">hello parameters used in this command are as follows:</span></span>

   * <span data-ttu-id="5d8ce-127">**-d**: `-G` nem használja, így hello kérelem alapértelmezés szerint használt érték toohello POST metódussal.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-127">**-d**: `-G` is not used, so hello request defaults toohello POST method.</span></span> <span data-ttu-id="5d8ce-128">`-d`Megadja a küldött hello adatértékek hello kérelemmel.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-128">`-d` specifies hello data values that are sent with hello request.</span></span>
    * <span data-ttu-id="5d8ce-129">**User.name**: hello hello parancsot futtató felhasználó</span><span class="sxs-lookup"><span data-stu-id="5d8ce-129">**user.name**: hello user who is running hello command</span></span>
    * <span data-ttu-id="5d8ce-130">**JAR**: hello jar-fájlt tartalmazó osztály toobe hello helyét futott</span><span class="sxs-lookup"><span data-stu-id="5d8ce-130">**jar**: hello location of hello jar file that contains class toobe ran</span></span>
    * <span data-ttu-id="5d8ce-131">**osztály**: hello hello MapReduce programot tartalmazó osztály</span><span class="sxs-lookup"><span data-stu-id="5d8ce-131">**class**: hello class that contains hello MapReduce logic</span></span>
    * <span data-ttu-id="5d8ce-132">**arg**: hello argumentumok toobe átadott toohello MapReduce feladatot.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-132">**arg**: hello arguments toobe passed toohello MapReduce job.</span></span> <span data-ttu-id="5d8ce-133">Ebben az esetben hello adjon meg szöveget a fájl- és hello directory hello kimenethez használt</span><span class="sxs-lookup"><span data-stu-id="5d8ce-133">In this case, hello input text file and hello directory that are used for hello output</span></span>

     <span data-ttu-id="5d8ce-134">Ez a parancs visszaadja-e a feladat azonosítója, amely használt toocheck hello hello feladat állapota:</span><span class="sxs-lookup"><span data-stu-id="5d8ce-134">This command should return a job ID that can be used toocheck hello status of hello job:</span></span>

       <span data-ttu-id="5d8ce-135">{"id": "job_1415651640909_0026"}</span><span class="sxs-lookup"><span data-stu-id="5d8ce-135">{"id":"job_1415651640909_0026"}</span></span>

3. <span data-ttu-id="5d8ce-136">hello feladat, a következő parancs használata hello toocheck hello állapota:</span><span class="sxs-lookup"><span data-stu-id="5d8ce-136">toocheck hello status of hello job, use hello following command:</span></span>

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    <span data-ttu-id="5d8ce-137">Cserélje le a hello **JOBID** hello előző lépés eredményeképpen visszakapott hello értékkel.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-137">Replace hello **JOBID** with hello value returned in hello previous step.</span></span> <span data-ttu-id="5d8ce-138">Például ha hello visszatérési érték volt `{"id":"job_1415651640909_0026"}`, majd JOBID lenne hello `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-138">For example, if hello return value was `{"id":"job_1415651640909_0026"}`, then hello JOBID would be `job_1415651640909_0026`.</span></span>

    <span data-ttu-id="5d8ce-139">Ha hello folyamat befejeződik, hello visszaküldött állapot `SUCCEEDED`.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-139">If hello job is complete, hello state returned is `SUCCEEDED`.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5d8ce-140">A Curl kérelem hello feladat adatait tartalmazó JSON-dokumentumból adja vissza.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-140">This Curl request returns a JSON document with information about hello job.</span></span> <span data-ttu-id="5d8ce-141">Jq használt tooretrieve csak hello állapotérték.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-141">Jq is used tooretrieve only hello state value.</span></span>

4. <span data-ttu-id="5d8ce-142">Ha hello állapota hello feladat túl megváltozott,`SUCCEEDED`, az Azure Blob storage hello feladat eredményeinek hello kérheti le.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-142">When hello state of hello job has changed too`SUCCEEDED`, you can retrieve hello results of hello job from Azure Blob storage.</span></span> <span data-ttu-id="5d8ce-143">Hello `statusdir` hello lekérdezéssel átadott paraméter hello kimeneti fájl helye hello tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-143">hello `statusdir` parameter that is passed with hello query contains hello location of hello output file.</span></span> <span data-ttu-id="5d8ce-144">Ebben a példában hello helye `/example/curl`.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-144">In this example, hello location is `/example/curl`.</span></span> <span data-ttu-id="5d8ce-145">Ez a cím hello feladat eredményének hello tárol hello fürtök alapértelmezett tárolás `/example/curl`.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-145">This address stores hello output of hello job in hello clusters default storage at `/example/curl`.</span></span>

<span data-ttu-id="5d8ce-146">Listáról, és ezek a fájlok letöltésére hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="5d8ce-146">You can list and download these files by using hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="5d8ce-147">A BLOB az Azure CLI hello munkáról bővebben lásd: hello [Using hello Azure CLI 2.0 az Azure Storage](../storage/common/storage-azure-cli.md#create-and-manage-blobs) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="5d8ce-147">For more information on working with blobs from hello Azure CLI, see hello [Using hello Azure CLI 2.0 with Azure Storage](../storage/common/storage-azure-cli.md#create-and-manage-blobs) document.</span></span>

## <span data-ttu-id="5d8ce-148"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5d8ce-148"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="5d8ce-149">Általános információk a hdinsight MapReduce-feladatok:</span><span class="sxs-lookup"><span data-stu-id="5d8ce-149">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="5d8ce-150">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="5d8ce-150">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="5d8ce-151">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="5d8ce-151">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="5d8ce-152">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="5d8ce-152">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="5d8ce-153">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="5d8ce-153">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="5d8ce-154">Ebben a cikkben használt hello REST-felület kapcsolatos további információkért lásd: hello [WebHCat hivatkozás](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span><span class="sxs-lookup"><span data-stu-id="5d8ce-154">For more information about hello REST interface that is used in this article, see hello [WebHCat Reference](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span></span>
