---
title: a hdinsight - Azure Curl Hadoop Hive aaaUse |} Microsoft Docs
description: "Ismerje meg, hogyan tooremotely submit Pig feladatok tooHDInsight használata Curl használatával."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6ce18163-63b5-4df6-9bb6-8fcbd4db05d8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: e725829ad2adcf3540f44375e3e87b7cdaebd15e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a><span data-ttu-id="9923b-103">A többi használatával HDInsight Hadoop Hive-lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="9923b-103">Run Hive queries with Hadoop in HDInsight using REST</span></span>

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="9923b-104">Ismerje meg, hogyan toouse hello WebHCat REST API toorun Hive lekérdezi a Hadoop on Azure HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="9923b-104">Learn how toouse hello WebHCat REST API toorun Hive queries with Hadoop on Azure HDInsight cluster.</span></span>

<span data-ttu-id="9923b-105">[Curl](http://curl.haxx.se/) használt toodemonstrate, és együttműködését HDInsight nyers HTTP-kérelmek használatával van.</span><span class="sxs-lookup"><span data-stu-id="9923b-105">[Curl](http://curl.haxx.se/) is used toodemonstrate how you can interact with HDInsight by using raw HTTP requests.</span></span> <span data-ttu-id="9923b-106">Hello [jq](http://stedolan.github.io/jq/) segédprogram használt tooprocess hello JSON-adatokat adott vissza a többi kérelmek.</span><span class="sxs-lookup"><span data-stu-id="9923b-106">hello [jq](http://stedolan.github.io/jq/) utility is used tooprocess hello JSON data returned from REST requests.</span></span>

> [!NOTE]
> <span data-ttu-id="9923b-107">Ha ismeri a Linux-alapú Hadoop-kiszolgálókat használ, de új tooHDInsight, tekintse meg a hello [mire van szüksége a Linux-alapú HDInsight Hadoop kapcsolatos tooknow](hdinsight-hadoop-linux-information.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="9923b-107">If you are already familiar with using Linux-based Hadoop servers, but are new tooHDInsight, see hello [What you need tooknow about Hadoop on Linux-based HDInsight](hdinsight-hadoop-linux-information.md) document.</span></span>

## <span data-ttu-id="9923b-108"><a id="curl"></a>Hive-lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="9923b-108"><a id="curl"></a>Run Hive queries</span></span>

> [!NOTE]
> <span data-ttu-id="9923b-109">Használata cURL vagy más REST kommunikációt használ a Webhcattel, hitelesítenie kell hello kérelmek hello felhasználónevet és jelszót hello HDInsight fürt rendszergazdája által.</span><span class="sxs-lookup"><span data-stu-id="9923b-109">When using cURL or any other REST communication with WebHCat, you must authenticate hello requests by providing hello user name and password for hello HDInsight cluster administrator.</span></span>
>
> <span data-ttu-id="9923b-110">Ezen szakasz parancsaiban hello, cserélje le **felhasználónév** hello felhasználói tooauthenticate toohello fürt, és cserélje ki a **jelszó** a hello hello felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="9923b-110">For hello commands in this section, replace **USERNAME** with hello user tooauthenticate toohello cluster, and replace **PASSWORD** with hello password for hello user account.</span></span> <span data-ttu-id="9923b-111">Cserélje le **CLUSTERNAME** hello néven a fürt.</span><span class="sxs-lookup"><span data-stu-id="9923b-111">Replace **CLUSTERNAME** with hello name of your cluster.</span></span>
>
> <span data-ttu-id="9923b-112">hello REST API védelméről [az egyszerű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="9923b-112">hello REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="9923b-113">toohelp győződjön meg arról, hogy a hitelesítő adatait biztonságos toohello kiszolgáló küldött, mindig kéréseket HTTP Secure (HTTPS) használatával.</span><span class="sxs-lookup"><span data-stu-id="9923b-113">toohelp ensure that your credentials are securely sent toohello server, always make requests by using Secure HTTP (HTTPS).</span></span>

1. <span data-ttu-id="9923b-114">A parancssorból a következő parancs tooverify, hogy csatlakozhasson tooyour HDInsight-fürt hello használata:</span><span class="sxs-lookup"><span data-stu-id="9923b-114">From a command line, use hello following command tooverify that you can connect tooyour HDInsight cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    <span data-ttu-id="9923b-115">Egy hasonló toohello választ, a következő szöveg jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="9923b-115">You receive a response similar toohello following text:</span></span>

        {"status":"ok","version":"v1"}

    <span data-ttu-id="9923b-116">Ebben a parancsban használt hello paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="9923b-116">hello parameters used in this command are as follows:</span></span>

   * <span data-ttu-id="9923b-117">**-u** -hello felhasználónevet és jelszót használt tooauthenticate hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="9923b-117">**-u** - hello user name and password used tooauthenticate hello request.</span></span>
   * <span data-ttu-id="9923b-118">**-G** -azt jelzi, hogy a kérelem a GET műveletet.</span><span class="sxs-lookup"><span data-stu-id="9923b-118">**-G** - Indicates that this request is a GET operation.</span></span>

     <span data-ttu-id="9923b-119">hello hello URL-címe, eleje **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, van hello azonos minden olyan kérelem esetében.</span><span class="sxs-lookup"><span data-stu-id="9923b-119">hello beginning of hello URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hello same for all requests.</span></span> <span data-ttu-id="9923b-120">hello elérési **/status**, mutatja, hogy hello kérés tooreturn WebHCat (más néven Templeton) állapota hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="9923b-120">hello path, **/status**, indicates that hello request is tooreturn a status of WebHCat (also known as Templeton) for hello server.</span></span> <span data-ttu-id="9923b-121">Hive hello verzióját is kérheti hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="9923b-121">You can also request hello version of Hive by using hello following command:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

     <span data-ttu-id="9923b-122">A kérelem a válasz a következő szöveg hasonló toohello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="9923b-122">This request returns a response similar toohello following text:</span></span>

       <span data-ttu-id="9923b-123">{"module": "hive", "version": "0.13.0.2.1.6.0-2103"}</span><span class="sxs-lookup"><span data-stu-id="9923b-123">{"module":"hive","version":"0.13.0.2.1.6.0-2103"}</span></span>

2. <span data-ttu-id="9923b-124">A következő nevű tábla toocreate használata hello **log4jLogs**:</span><span class="sxs-lookup"><span data-stu-id="9923b-124">Use hello following toocreate a table named **log4jLogs**:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    <span data-ttu-id="9923b-125">a következő a kérelem paramétereit hello:</span><span class="sxs-lookup"><span data-stu-id="9923b-125">hello following parameters used with this request:</span></span>

   * <span data-ttu-id="9923b-126">**-d** - óta `-G` nem használ, akkor hello kérelem alapértelmezés szerint használt érték toohello POST metódussal.</span><span class="sxs-lookup"><span data-stu-id="9923b-126">**-d** - Since `-G` is not used, hello request defaults toohello POST method.</span></span> <span data-ttu-id="9923b-127">`-d`Megadja a küldött hello adatértékek hello kérelemmel.</span><span class="sxs-lookup"><span data-stu-id="9923b-127">`-d` specifies hello data values that are sent with hello request.</span></span>

     * <span data-ttu-id="9923b-128">**User.name** -hello parancsot futtató felhasználónak hello.</span><span class="sxs-lookup"><span data-stu-id="9923b-128">**user.name** - hello user that is running hello command.</span></span>
     * <span data-ttu-id="9923b-129">**végrehajtás** -HiveQL utasítások tooexecute hello.</span><span class="sxs-lookup"><span data-stu-id="9923b-129">**execute** - hello HiveQL statements tooexecute.</span></span>
     * <span data-ttu-id="9923b-130">**statusdir** – Ez a feladat állapotát hello hello könyvtár nevével.</span><span class="sxs-lookup"><span data-stu-id="9923b-130">**statusdir** - hello directory that hello status for this job is written to.</span></span>

     <span data-ttu-id="9923b-131">Ezekre az utasításokra hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="9923b-131">These statements perform hello following actions:</span></span>
   * <span data-ttu-id="9923b-132">**DROP TABLE** -Ha hello tábla már létezik, akkor az törlődik.</span><span class="sxs-lookup"><span data-stu-id="9923b-132">**DROP TABLE** - If hello table already exists, it is deleted.</span></span>
   * <span data-ttu-id="9923b-133">**A külső tábla létrehozása** -táblát hoz létre egy új "külső" struktúra.</span><span class="sxs-lookup"><span data-stu-id="9923b-133">**CREATE EXTERNAL TABLE** - Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="9923b-134">Külső táblák csak hello tábladefiníció Hive tárolja.</span><span class="sxs-lookup"><span data-stu-id="9923b-134">External tables store only hello table definition in Hive.</span></span> <span data-ttu-id="9923b-135">hello adatok hello eredeti helyen marad.</span><span class="sxs-lookup"><span data-stu-id="9923b-135">hello data is left in hello original location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="9923b-136">Külső táblák kell használni, amikor hello frissíteni az külső forrás alapjául szolgáló adatok toobe várt.</span><span class="sxs-lookup"><span data-stu-id="9923b-136">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="9923b-137">Például egy automatizált adatok feltöltési folyamat vagy egy másik MapReduce művelet.</span><span class="sxs-lookup"><span data-stu-id="9923b-137">For example, an automated data upload process or another MapReduce operation.</span></span>
     >
     > <span data-ttu-id="9923b-138">A külső tábla eldobása does **nem** törölni hello adatokat, csak a hello tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="9923b-138">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>

   * <span data-ttu-id="9923b-139">**SOR formátum** – hogyan hello adatok formátuma.</span><span class="sxs-lookup"><span data-stu-id="9923b-139">**ROW FORMAT** - How hello data is formatted.</span></span> <span data-ttu-id="9923b-140">az egyes naplókon hello mezők vannak szóközzel elválasztva.</span><span class="sxs-lookup"><span data-stu-id="9923b-140">hello fields in each log are separated by a space.</span></span>
   * <span data-ttu-id="9923b-141">**AS TEXTFILE helyen tárolt** - Ha hello tárolja (hello példaadatokat/könyvtár) és szövegként tárolt.</span><span class="sxs-lookup"><span data-stu-id="9923b-141">**STORED AS TEXTFILE LOCATION** - Where hello data is stored (hello example/data directory) and that it is stored as text.</span></span>
   * <span data-ttu-id="9923b-142">**Válassza ki** -választja ki az összes sorok számát ahol oszlop **t4** hello értéket tartalmaz **[hiba]**.</span><span class="sxs-lookup"><span data-stu-id="9923b-142">**SELECT** - Selects a count of all rows where column **t4** contains hello value **[ERROR]**.</span></span> <span data-ttu-id="9923b-143">A jelen nyilatkozat értéket ad vissza, **3** ahány három ezt az értéket tartalmazó sorok.</span><span class="sxs-lookup"><span data-stu-id="9923b-143">This statement returns a value of **3** as there are three rows that contain this value.</span></span>

     > [!NOTE]
     > <span data-ttu-id="9923b-144">Figyelje meg, hogy HiveQL utasítások hello szóközt helyébe hello `+` karakter Curl használata esetén.</span><span class="sxs-lookup"><span data-stu-id="9923b-144">Notice that hello spaces between HiveQL statements are replaced by hello `+` character when used with Curl.</span></span> <span data-ttu-id="9923b-145">Szóközt, például a hello elválasztó idézőjelek közé zárt értékek nem helyébe kell `+`.</span><span class="sxs-lookup"><span data-stu-id="9923b-145">Quoted values that contain a space, such as hello delimiter, should not be replaced by `+`.</span></span>

   * <span data-ttu-id="9923b-146">**INPUT__FILE__NAME PÉLDÁUL "% 25.log"** - e utasítás korlátok hello keresési tooonly használata fájlok végződése. napló.</span><span class="sxs-lookup"><span data-stu-id="9923b-146">**INPUT__FILE__NAME LIKE '%25.log'** - This statement limits hello search tooonly use files ending in .log.</span></span>

     > [!NOTE]
     > <span data-ttu-id="9923b-147">Hello `%25` hello URL-kódolású formátum %-os van, így a tényleges feltétele hello `like '%.log'`.</span><span class="sxs-lookup"><span data-stu-id="9923b-147">hello `%25` is hello URL encoded form of %, so hello actual condition is `like '%.log'`.</span></span> <span data-ttu-id="9923b-148">hello % toobe URL-címe, többek között kódolhatók, akkor a rendszer egy különleges karakter URL-van.</span><span class="sxs-lookup"><span data-stu-id="9923b-148">hello % has toobe URL encoded, as it is treated as a special character in URLs.</span></span>

     <span data-ttu-id="9923b-149">Ez a parancs a feladat azonosítója, amely hello feladat állapotának használt toocheck hello kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="9923b-149">This command should return a job ID that can be used toocheck hello status of hello job.</span></span>

       <span data-ttu-id="9923b-150">{"id": "job_1415651640909_0026"}</span><span class="sxs-lookup"><span data-stu-id="9923b-150">{"id":"job_1415651640909_0026"}</span></span>

3. <span data-ttu-id="9923b-151">hello feladat, a következő parancs használata hello toocheck hello állapota:</span><span class="sxs-lookup"><span data-stu-id="9923b-151">toocheck hello status of hello job, use hello following command:</span></span>

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    <span data-ttu-id="9923b-152">Cserélje le **JOBID** hello előző lépés eredményeképpen visszakapott hello értékkel.</span><span class="sxs-lookup"><span data-stu-id="9923b-152">Replace **JOBID** with hello value returned in hello previous step.</span></span> <span data-ttu-id="9923b-153">Például ha hello visszatérési érték volt `{"id":"job_1415651640909_0026"}`, majd **JOBID** lenne `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="9923b-153">For example, if hello return value was `{"id":"job_1415651640909_0026"}`, then **JOBID** would be `job_1415651640909_0026`.</span></span>

    <span data-ttu-id="9923b-154">Ha hello feladat befejeződött, hello állapota **sikeres**.</span><span class="sxs-lookup"><span data-stu-id="9923b-154">If hello job has finished, hello state is **SUCCEEDED**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="9923b-155">A Curl kérelem hello feladat adatait tartalmazó JavaScript Object Notation (JSON) dokumentum adja vissza.</span><span class="sxs-lookup"><span data-stu-id="9923b-155">This Curl request returns a JavaScript Object Notation (JSON) document with information about hello job.</span></span> <span data-ttu-id="9923b-156">Jq használt tooretrieve csak hello állapotérték.</span><span class="sxs-lookup"><span data-stu-id="9923b-156">Jq is used tooretrieve only hello state value.</span></span>

4. <span data-ttu-id="9923b-157">Miután hello feladat hello állapota megváltozott túl**sikeres**, az Azure Blob storage hello feladat eredményeinek hello kérheti le.</span><span class="sxs-lookup"><span data-stu-id="9923b-157">Once hello state of hello job has changed too**SUCCEEDED**, you can retrieve hello results of hello job from Azure Blob storage.</span></span> <span data-ttu-id="9923b-158">Hello `statusdir` hello lekérdezéssel átadott paraméter tartalmaz hello hely hello kimeneti fájl; ebben az esetben az **/példa/curl**.</span><span class="sxs-lookup"><span data-stu-id="9923b-158">hello `statusdir` parameter passed with hello query contains hello location of hello output file; in this case, **/example/curl**.</span></span> <span data-ttu-id="9923b-159">Ez a cím hello kimeneti tárol hello **példa/curl** könyvtárat a hello fürtök alapértelmezett tároló.</span><span class="sxs-lookup"><span data-stu-id="9923b-159">This address stores hello output in hello **example/curl** directory in hello clusters default storage.</span></span>

    <span data-ttu-id="9923b-160">Listáról, és ezek a fájlok letöltésére hello [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9923b-160">You can list and download these files by using hello [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="9923b-161">Az Azure Storage hello Azure CLI használatáról további információkért lásd: hello [Azure CLI 2.0 használata Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli#create-and-manage-blobs) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="9923b-161">For more information on using hello Azure CLI with Azure Storage, see hello [Use Azure CLI 2.0 with Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli#create-and-manage-blobs) document.</span></span>

5. <span data-ttu-id="9923b-162">A következő utasítások toocreate nevű új "belső" tábla használata hello **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="9923b-162">Use hello following statements toocreate a new 'internal' table named **errorLogs**:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    <span data-ttu-id="9923b-163">Ezekre az utasításokra hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="9923b-163">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="9923b-164">**Hozzon létre Ha nem létezik táblázat** -táblázatot hoz létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="9923b-164">**CREATE TABLE IF NOT EXISTS** - Creates a table, if it does not already exist.</span></span> <span data-ttu-id="9923b-165">A jelen nyilatkozat egy belső tábla, amely hello Hive-adatraktárban tárolja, és teljesen kezeli a Hive hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9923b-165">This statement creates an internal table, which is stored in hello Hive data warehouse and is managed completely by Hive.</span></span>

     > [!NOTE]
     > <span data-ttu-id="9923b-166">Külső táblák eltérően eldobását egy belső tábla törlése hello, valamint az alapul szolgáló adatokat.</span><span class="sxs-lookup"><span data-stu-id="9923b-166">Unlike external tables, dropping an internal table deletes hello underlying data as well.</span></span>

   * <span data-ttu-id="9923b-167">**TÁROLT AS ORC** -hello eltárolja optimalizált sor oszlopos (ORC) formátumban.</span><span class="sxs-lookup"><span data-stu-id="9923b-167">**STORED AS ORC** - Stores hello data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="9923b-168">ORC formátuma egy magas optimalizált és hatékony Hive adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="9923b-168">ORC is a highly optimized and efficient format for storing Hive data.</span></span>
   * <span data-ttu-id="9923b-169">**ÍRJA FELÜL AZ INSERT... Válassza ki** -sorok kiválaszt hello **log4jLogs** tartalmazó **[hiba]**, majd Beszúrások adatok hello hello be **errorLogs** tábla.</span><span class="sxs-lookup"><span data-stu-id="9923b-169">**INSERT OVERWRITE ... SELECT** - Selects rows from hello **log4jLogs** table that contain **[ERROR]**, then inserts hello data into hello **errorLogs** table.</span></span>
   * <span data-ttu-id="9923b-170">**Válassza ki** -kiválaszt minden sor új hello **errorLogs** tábla.</span><span class="sxs-lookup"><span data-stu-id="9923b-170">**SELECT** - Selects all rows from hello new **errorLogs** table.</span></span>

6. <span data-ttu-id="9923b-171">Használja a hello Feladatazonosító hello feladat toocheck hello állapotot adott vissza.</span><span class="sxs-lookup"><span data-stu-id="9923b-171">Use hello job ID returned toocheck hello status of hello job.</span></span> <span data-ttu-id="9923b-172">Ha sikeres, a hello Azure CLI korábban leírt toodownload és hello az eredmények.</span><span class="sxs-lookup"><span data-stu-id="9923b-172">Once it has succeeded, use hello Azure CLI as described previously toodownload and view hello results.</span></span> <span data-ttu-id="9923b-173">hello kimeneti tartalmaznia kell a három sort, ezek mindegyike tartalmazhat **[hiba]**.</span><span class="sxs-lookup"><span data-stu-id="9923b-173">hello output should contain three lines, all of which contain **[ERROR]**.</span></span>

## <span data-ttu-id="9923b-174"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9923b-174"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="9923b-175">Általános információk a hdinsight Hive:</span><span class="sxs-lookup"><span data-stu-id="9923b-175">For general information on Hive with HDInsight:</span></span>

* [<span data-ttu-id="9923b-176">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="9923b-176">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="9923b-177">Az egyéb lehetőségeiről a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="9923b-177">For information on other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="9923b-178">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="9923b-178">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="9923b-179">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="9923b-179">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="9923b-180">Ha a Hive Tez használ, tekintse meg a hello dokumentumok hibakeresési információkat a következő:</span><span class="sxs-lookup"><span data-stu-id="9923b-180">If you are using Tez with Hive, see hello following documents for debugging information:</span></span>

* [<span data-ttu-id="9923b-181">A Linux-alapú HDInsight Ambari Tez nézet hello használata</span><span class="sxs-lookup"><span data-stu-id="9923b-181">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

<span data-ttu-id="9923b-182">Hello REST API-t Itt további információkért lásd: hello [WebHCat hivatkozás](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="9923b-182">For more information on hello REST API used in this document, see hello [WebHCat reference](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) document.</span></span>

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


