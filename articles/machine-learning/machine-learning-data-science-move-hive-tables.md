---
title: "aaaCreate Hive táblák és az adatok betöltése az Azure Blob Storage |} Microsoft Docs"
description: "Hive táblák létrehozására és betöltésére blob toohive táblázatok adatait"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: cff9280d-18ce-4b66-a54f-19f358d1ad90
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 09622972bcac31c2971858393a8340f24e4b7390
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a><span data-ttu-id="f38e2-103">Hive táblák létrehozása és az adatok betöltése az Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="f38e2-103">Create Hive tables and load data from Azure Blob Storage</span></span>
<span data-ttu-id="f38e2-104">Ebből a témakörből megismerheti, hogy a Hive táblák létrehozása és az adatok betöltése az Azure blob storage általános Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="f38e2-104">This topic presents generic Hive queries that create Hive tables and load data from Azure blob storage.</span></span> <span data-ttu-id="f38e2-105">Néhány is útmutatást a Hive Táblák particionálása és hello optimalizált sor oszlopos (ORC) formázási tooimprove teljesítmény-küszöbérték használatával.</span><span class="sxs-lookup"><span data-stu-id="f38e2-105">Some guidance is also provided on partitioning Hive tables and on using hello Optimized Row Columnar (ORC) formatting tooimprove query performance.</span></span>

<span data-ttu-id="f38e2-106">Ez **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan tooingest adatok cél környezetekben, ahol hello adatok is tárolhatók és feldolgozhatók, során hello Team adatok tudományos folyamat (TDSP) tootopics.</span><span class="sxs-lookup"><span data-stu-id="f38e2-106">This **menu** links tootopics that describe how tooingest data into target environments where hello data can be stored and processed during hello Team Data Science Process (TDSP).</span></span>

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="f38e2-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f38e2-107">Prerequisites</span></span>
<span data-ttu-id="f38e2-108">Ez a cikk feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="f38e2-108">This article assumes that you have:</span></span>

* <span data-ttu-id="f38e2-109">Egy Azure storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f38e2-109">Created an Azure storage account.</span></span> <span data-ttu-id="f38e2-110">Ha módosítania kell az utasításokat, lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="f38e2-110">If you need instructions, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
* <span data-ttu-id="f38e2-111">A testre szabott Hadoop-fürt a HDInsight-szolgáltatás hello kiépítve.</span><span class="sxs-lookup"><span data-stu-id="f38e2-111">Provisioned a customized Hadoop cluster with hello HDInsight service.</span></span>  <span data-ttu-id="f38e2-112">Ha módosítania kell az utasításokat, lásd: [testreszabása az Azure HDInsight Hadoop-fürtök Speciális elemzésekre](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="f38e2-112">If you need instructions, see [Customize Azure HDInsight Hadoop clusters for advanced analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="f38e2-113">Engedélyezett távoli hozzáférési toohello fürt, jelentkezett be, és hello Hadoop parancssori konzol megnyitása.</span><span class="sxs-lookup"><span data-stu-id="f38e2-113">Enabled remote access toohello cluster, logged in, and opened hello Hadoop Command-Line console.</span></span> <span data-ttu-id="f38e2-114">Ha módosítania kell az utasításokat, lásd: [hozzáférés hello Head csomópont a Hadoop-fürt](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="f38e2-114">If you need instructions, see [Access hello Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

## <a name="upload-data-tooazure-blob-storage"></a><span data-ttu-id="f38e2-115">Töltse fel az adatok tooAzure blob-tároló</span><span class="sxs-lookup"><span data-stu-id="f38e2-115">Upload data tooAzure blob storage</span></span>
<span data-ttu-id="f38e2-116">Ha létrehozott egy Azure virtuális gép hello megjelenő utasításokat követve [állítson be egy Azure virtuális gép speciális elemzésekre](machine-learning-data-science-setup-virtual-machine.md), a parancsfájl letöltött toohello kellett volna *C:\\ Felhasználók\\\<felhasználónév\>\\dokumentumok\\adatok tudományos parancsfájlok* hello virtuális gép könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="f38e2-116">If you created an Azure virtual machine by following hello instructions provided in [Set up an Azure virtual machine for advanced analytics](machine-learning-data-science-setup-virtual-machine.md), this script file should have been downloaded toohello *C:\\Users\\\<user name\>\\Documents\\Data Science Scripts* directory on hello virtual machine.</span></span> <span data-ttu-id="f38e2-117">A következő Hive-lekérdezések csak szükséges, csatlakoztassa a saját adatok séma és az Azure blob storage konfigurációs hello megfelelő mezőket toobe készen áll, hogy elküldhesse a.</span><span class="sxs-lookup"><span data-stu-id="f38e2-117">These Hive queries only require that you plug in your own data schema and Azure blob storage configuration in hello appropriate fields toobe ready for submission.</span></span>

<span data-ttu-id="f38e2-118">Feltételezzük, hogy a Hive táblák hello adatok van egy **tömörítetlen** táblázatos formátumban, és hogy hello már feltöltött toohello alapértelmezett (vagy további tooan) tároló hello Hadoop-fürt által használt hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="f38e2-118">We assume that hello data for Hive tables is in an **uncompressed** tabular format, and that hello data has been uploaded toohello default (or tooan additional) container of hello storage account used by hello Hadoop cluster.</span></span>

<span data-ttu-id="f38e2-119">Ha azt szeretné, hogy a hello toopractice **NYC Taxi út adatok**, kell:</span><span class="sxs-lookup"><span data-stu-id="f38e2-119">If you want toopractice on hello **NYC Taxi Trip Data**, you need to:</span></span>

* <span data-ttu-id="f38e2-120">**Töltse le** hello 24 [NYC Taxi út adatok](http://www.andresmh.com/nyctaxitrips) (12 út fájlok és 12 jegy ára fájlok),</span><span class="sxs-lookup"><span data-stu-id="f38e2-120">**download** hello 24 [NYC Taxi Trip Data](http://www.andresmh.com/nyctaxitrips) files (12 Trip files and 12 Fare files),</span></span>
* <span data-ttu-id="f38e2-121">**Csomagolja ki** .csv fájlokat, az összes fájl, majd</span><span class="sxs-lookup"><span data-stu-id="f38e2-121">**unzip** all files into .csv files, and then</span></span>
* <span data-ttu-id="f38e2-122">**Töltse fel** őket toohello alapértelmezett (vagy a megfelelő tárolót) az Azure storage-fiók hello hello ismertetett eljárással létrehozott hello [testreszabása az Azure HDInsight Hadoop-fürtök Advanced Analytics folyamat és a technológia ](machine-learning-data-science-customize-hadoop-cluster.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="f38e2-122">**upload** them toohello default (or appropriate container) of hello Azure storage account that was created by hello procedure outlined in hello [Customize Azure HDInsight Hadoop clusters for Advanced Analytics Process and Technology](machine-learning-data-science-customize-hadoop-cluster.md) topic.</span></span> <span data-ttu-id="f38e2-123">Itt található hello folyamat tooupload hello .csv fájlok toohello alapértelmezett tároló hello tárfiók [lap](machine-learning-data-science-process-hive-walkthrough.md#upload).</span><span class="sxs-lookup"><span data-stu-id="f38e2-123">hello process tooupload hello .csv files toohello default container on hello storage account can be found on this [page](machine-learning-data-science-process-hive-walkthrough.md#upload).</span></span>

## <span data-ttu-id="f38e2-124"><a name="submit"></a>Hogyan toosubmit Hive-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="f38e2-124"><a name="submit"></a>How toosubmit Hive queries</span></span>
<span data-ttu-id="f38e2-125">Hive-lekérdezések segítségével küldheti el:</span><span class="sxs-lookup"><span data-stu-id="f38e2-125">Hive queries can be submitted by using:</span></span>

1. [<span data-ttu-id="f38e2-126">A Hadoop-fürt headnode keresztül Hadoop parancssori Hive-lekérdezések elküldése</span><span class="sxs-lookup"><span data-stu-id="f38e2-126">Submit Hive queries through Hadoop Command Line in headnode of Hadoop cluster</span></span>](#headnode)
2. [<span data-ttu-id="f38e2-127">A Hive szerkesztő hello Hive-lekérdezések elküldése</span><span class="sxs-lookup"><span data-stu-id="f38e2-127">Submit Hive queries with hello Hive Editor</span></span>](#hive-editor)
3. [<span data-ttu-id="f38e2-128">Küldje el az Azure PowerShell-parancsokkal Hive-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="f38e2-128">Submit Hive queries with Azure PowerShell Commands</span></span>](#ps)

<span data-ttu-id="f38e2-129">Hive-lekérdezések SQL-szerű.</span><span class="sxs-lookup"><span data-stu-id="f38e2-129">Hive queries are SQL-like.</span></span> <span data-ttu-id="f38e2-130">Ha ismeri az SQL, azt tapasztalhatja hello [SQL felhasználók Cheat lap Hive](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) hasznos.</span><span class="sxs-lookup"><span data-stu-id="f38e2-130">If you are familiar with SQL, you may find hello [Hive for SQL Users Cheat Sheet](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) useful.</span></span>

<span data-ttu-id="f38e2-131">A Hive-lekérdezés elküldésekor azt is meghatározhatja, Hive-lekérdezések eredményének hello hello céljának hello képernyő vagy tooa helyi fájl hello átjárócsomópont vagy az Azure blob tooan legyen.</span><span class="sxs-lookup"><span data-stu-id="f38e2-131">When submitting a Hive query, you can also control hello destination of hello output from Hive queries, whether it be on hello screen or tooa local file on hello head node or tooan Azure blob.</span></span>

### <span data-ttu-id="f38e2-132"><a name="headnode"></a> 1. A Hadoop-fürt headnode keresztül Hadoop parancssori Hive-lekérdezések elküldése</span><span class="sxs-lookup"><span data-stu-id="f38e2-132"><a name="headnode"></a> 1. Submit Hive queries through Hadoop Command Line in headnode of Hadoop cluster</span></span>
<span data-ttu-id="f38e2-133">Hello Hive lekérdezés esetén összetett, közvetlenül a hello hello Hadoop-fürt átjárócsomópontjához általában vezet toofaster kapcsolja körül, mint a Hive szerkesztő vagy az Azure PowerShell-parancsfájlok továbbítás elküldése.</span><span class="sxs-lookup"><span data-stu-id="f38e2-133">If hello Hive query is complex, submitting it directly in hello head node of hello Hadoop cluster typically leads toofaster turn around than submitting it with a Hive Editor or Azure PowerShell scripts.</span></span>

<span data-ttu-id="f38e2-134">Jelentkezzen be toohello hello Hadoop-fürt átjárócsomópontjához, hello Hadoop parancssori hello központi csomópont hello asztalon nyissa meg, és írja be a parancs `cd %hive_home%\bin`.</span><span class="sxs-lookup"><span data-stu-id="f38e2-134">Log in toohello head node of hello Hadoop cluster, open hello Hadoop Command Line on hello desktop of hello head node, and enter command `cd %hive_home%\bin`.</span></span>

<span data-ttu-id="f38e2-135">Három módon toosubmit Hive-lekérdezések hello Hadoop parancssori rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="f38e2-135">You have three ways toosubmit Hive queries in hello Hadoop Command Line:</span></span>

* <span data-ttu-id="f38e2-136">közvetlenül</span><span class="sxs-lookup"><span data-stu-id="f38e2-136">directly</span></span>
* <span data-ttu-id="f38e2-137">.hql fájlok használata</span><span class="sxs-lookup"><span data-stu-id="f38e2-137">using .hql files</span></span>
* <span data-ttu-id="f38e2-138">a hello Hive parancskonzolról</span><span class="sxs-lookup"><span data-stu-id="f38e2-138">with hello Hive command console</span></span>

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a><span data-ttu-id="f38e2-139">Küldje el közvetlenül a Hadoop parancssori Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="f38e2-139">Submit Hive queries directly in Hadoop Command Line.</span></span>
<span data-ttu-id="f38e2-140">Például a parancs futtatása `hive -e "<your hive query>;` toosubmit egyszerű Hive-lekérdezéseket közvetlenül a Hadoop parancssor.</span><span class="sxs-lookup"><span data-stu-id="f38e2-140">You can run command like `hive -e "<your hive query>;` toosubmit simple Hive queries directly in Hadoop Command Line.</span></span> <span data-ttu-id="f38e2-141">Íme egy példa, ahol piros hello mezőben vázol fel hello parancs, amellyel hello Hive-lekérdezést, és hello zöld mező vázol fel hello kimenete hello Hive-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="f38e2-141">Here is an example, where hello red box outlines hello command that submits hello Hive query, and hello green box outlines hello output from hello Hive query.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a><span data-ttu-id="f38e2-143">Küldje el a Hive-lekérdezéseket a .hql fájlok</span><span class="sxs-lookup"><span data-stu-id="f38e2-143">Submit Hive queries in .hql files</span></span>
<span data-ttu-id="f38e2-144">Hello Hive-lekérdezések bonyolultabb, és több vonal van, a parancssor vagy a Hive parancskonzolról lekérdezések szerkesztése esetén nem gyakorlati.</span><span class="sxs-lookup"><span data-stu-id="f38e2-144">When hello Hive query is more complicated and has multiple lines, editing queries in command line or Hive command console is not practical.</span></span> <span data-ttu-id="f38e2-145">Alternatív hello átjárócsomópontjához hello Hadoop fürthöz toosave hello Hive-lekérdezések egy helyi könyvtárat a hello átjárócsomópont .hql fájlban toouse egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="f38e2-145">An alternative is toouse a text editor in hello head node of hello Hadoop cluster toosave hello Hive queries in a .hql file in a local directory of hello head node.</span></span> <span data-ttu-id="f38e2-146">Ezután a Hive-lekérdezések hello hello .hql fájlban hello segítségével küldheti el `-f` argumentum az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f38e2-146">Then hello Hive query in hello .hql file can be submitted by using hello `-f` argument as follows:</span></span>

    hive -f "<path toohello .hql file>"

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)

<span data-ttu-id="f38e2-148">**Ne jelenjen meg többé folyamat állapota képernyőn nyomtatás Hive-lekérdezések**</span><span class="sxs-lookup"><span data-stu-id="f38e2-148">**Suppress progress status screen print of Hive queries**</span></span>

<span data-ttu-id="f38e2-149">Alapértelmezés szerint a Hadoop parancssorban Hive-lekérdezés elküldése után hello hello térkép vagy csökkentse a feladat előrehaladását nyomtatása a képernyőn.</span><span class="sxs-lookup"><span data-stu-id="f38e2-149">By default, after Hive query is submitted in Hadoop Command Line, hello progress of hello Map/Reduce job is printed out on screen.</span></span> <span data-ttu-id="f38e2-150">toosuppress hello hello térkép vagy csökkentse a feladat előrehaladását a képernyő Nyomtatás, argumentumot használja `-S` ("S" nagybetűvel) a hello a parancssor az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f38e2-150">toosuppress hello screen print of hello Map/Reduce job progress, you can use an argument `-S` ("S" in upper case) in hello command line as follows:</span></span>

    hive -S -f "<path toohello .hql file>"
<span data-ttu-id="f38e2-151">.</span><span class="sxs-lookup"><span data-stu-id="f38e2-151">.</span></span>    <span data-ttu-id="f38e2-152">Hive -S -e "<Hive queries>"</span><span class="sxs-lookup"><span data-stu-id="f38e2-152">hive -S -e "<Hive queries>"</span></span>

#### <a name="submit-hive-queries-in-hive-command-console"></a><span data-ttu-id="f38e2-153">Küldje el a Hive parancskonzolról Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="f38e2-153">Submit Hive queries in Hive command console.</span></span>
<span data-ttu-id="f38e2-154">Először is megadhat az hello Hive parancskonzolról parancs futtatásával `hive` a Hadoop parancssor, és küldje el a Hive parancskonzolról Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="f38e2-154">You can also first enter hello Hive command console by running command `hive` in Hadoop Command Line, and then submit Hive queries in Hive command console.</span></span> <span data-ttu-id="f38e2-155">Íme egy példa.</span><span class="sxs-lookup"><span data-stu-id="f38e2-155">Here is an example.</span></span> <span data-ttu-id="f38e2-156">Ebben a példában hello két piros mezőkbe kiemelési hello használt parancsok tooenter hello Hive parancskonzolról, és Hive-lekérdezés Hive parancskonzolról, illetve benyújtott hello.</span><span class="sxs-lookup"><span data-stu-id="f38e2-156">In this example, hello two red boxes highlight hello commands used tooenter hello Hive command console, and hello Hive query submitted in Hive command console, respectively.</span></span> <span data-ttu-id="f38e2-157">zöld hello mezőben hello Hive-lekérdezések hello kimenetét mutatja be.</span><span class="sxs-lookup"><span data-stu-id="f38e2-157">hello green box highlights hello output from hello Hive query.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

<span data-ttu-id="f38e2-159">hello előző példák közvetlenül kimeneti hello Hive lekérdezés eredményeit a képernyőn.</span><span class="sxs-lookup"><span data-stu-id="f38e2-159">hello previous examples directly output hello Hive query results on screen.</span></span> <span data-ttu-id="f38e2-160">Hello kimeneti tooa helyi fájlt hello átjárócsomópont, vagy az Azure blob tooan is írhat.</span><span class="sxs-lookup"><span data-stu-id="f38e2-160">You can also write hello output tooa local file on hello head node, or tooan Azure blob.</span></span> <span data-ttu-id="f38e2-161">Ezután használhatja más eszközök toofurther hello Hive-lekérdezések kimenetének elemzése.</span><span class="sxs-lookup"><span data-stu-id="f38e2-161">Then, you can use other tools toofurther analyze hello output of Hive queries.</span></span>

<span data-ttu-id="f38e2-162">**Kimeneti Hive lekérdezés eredményei tooa helyi fájl.**</span><span class="sxs-lookup"><span data-stu-id="f38e2-162">**Output Hive query results tooa local file.**</span></span>
<span data-ttu-id="f38e2-163">toooutput Hive lekérdezés eredményei tooa helyi könyvtárába hello átjárócsomópont, rendelkezik toosubmit hello Hive-lekérdezések hello Hadoop parancssor az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f38e2-163">toooutput Hive query results tooa local directory on hello head node, you have toosubmit hello Hive query in hello Hadoop Command Line as follows:</span></span>

    hive -e "<hive query>" > <local path in hello head node>

<span data-ttu-id="f38e2-164">A következő példa hello, Hive-lekérdezések eredményének hello egy fájlba írása `hivequeryoutput.txt` könyvtárban `C:\apps\temp`.</span><span class="sxs-lookup"><span data-stu-id="f38e2-164">In hello following example, hello output of Hive query is written into a file `hivequeryoutput.txt` in directory `C:\apps\temp`.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

<span data-ttu-id="f38e2-166">**Kimeneti Hive lekérdezés eredményei tooan Azure blob**</span><span class="sxs-lookup"><span data-stu-id="f38e2-166">**Output Hive query results tooan Azure blob**</span></span>

<span data-ttu-id="f38e2-167">Emellett a kimenetre küldheti hello Hive lekérdezés eredményei tooan Azure blob, hello alapértelmezett tároló hello Hadoop-fürt belül.</span><span class="sxs-lookup"><span data-stu-id="f38e2-167">You can also output hello Hive query results tooan Azure blob, within hello default container of hello Hadoop cluster.</span></span> <span data-ttu-id="f38e2-168">hello Hive-lekérdezést a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="f38e2-168">hello Hive query for this is as follows:</span></span>

    insert overwrite directory wasb:///<directory within hello default container> <select clause from ...>

<span data-ttu-id="f38e2-169">A következő példa hello, a Hive-lekérdezések eredményének hello íródott tooa blob directory `queryoutputdir` hello alapértelmezett tároló hello Hadoop-fürt belül.</span><span class="sxs-lookup"><span data-stu-id="f38e2-169">In hello following example, hello output of Hive query is written tooa blob directory `queryoutputdir` within hello default container of hello Hadoop cluster.</span></span> <span data-ttu-id="f38e2-170">Itt csak akkor kell tooprovide hello könyvtár nevét, hello blob név nélkül.</span><span class="sxs-lookup"><span data-stu-id="f38e2-170">Here, you only need tooprovide hello directory name, without hello blob name.</span></span> <span data-ttu-id="f38e2-171">Hiba történt kívánja megadni a címtár és a blob neve, mint például `wasb:///queryoutputdir/queryoutput.txt`.</span><span class="sxs-lookup"><span data-stu-id="f38e2-171">An error is thrown if you provide both directory and blob names, such as `wasb:///queryoutputdir/queryoutput.txt`.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

<span data-ttu-id="f38e2-173">Hello alapértelmezett tároló hello Hadoop-fürt használata Azure Tártallózó nyit meg, ha hello Hive-lekérdezések eredményének hello láthatja, ahogy az ábra a következő hello.</span><span class="sxs-lookup"><span data-stu-id="f38e2-173">If you open hello default container of hello Hadoop cluster using Azure Storage Explorer, you can see hello output of hello Hive query as shown in hello following figure.</span></span> <span data-ttu-id="f38e2-174">Hello (vörös téglalappal által kiemelt) szűrő tooonly lekérése hello blob nevek megadott betűjét is alkalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="f38e2-174">You can apply hello filter (highlighted by red box) tooonly retrieve hello blob with specified letters in names.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

### <span data-ttu-id="f38e2-176"><a name="hive-editor"></a> 2. A Hive szerkesztő hello Hive-lekérdezések elküldése</span><span class="sxs-lookup"><span data-stu-id="f38e2-176"><a name="hive-editor"></a> 2. Submit Hive queries with hello Hive Editor</span></span>
<span data-ttu-id="f38e2-177">Hello lekérdezés konzol (Hive szerkesztő) használhatja a hello URL-cím megadásával *https://&#60; Hadoop-fürt neve >.azurehdinsight.net/Home/HiveEditor* egy webböngészőbe.</span><span class="sxs-lookup"><span data-stu-id="f38e2-177">You can also use hello Query Console (Hive Editor) by entering a URL of hello form *https://&#60;Hadoop cluster name>.azurehdinsight.net/Home/HiveEditor* into a web browser.</span></span> <span data-ttu-id="f38e2-178">Kell hello itt talál: naplózza ezt a konzolt, és így a Hadoop fürthöz hitelesítő adatait itt kell.</span><span class="sxs-lookup"><span data-stu-id="f38e2-178">You must be logged in hello see this console and so you need your Hadoop cluster credentials here.</span></span>

### <span data-ttu-id="f38e2-179"><a name="ps"></a> 3. Küldje el az Azure PowerShell-parancsokkal Hive-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="f38e2-179"><a name="ps"></a> 3. Submit Hive queries with Azure PowerShell Commands</span></span>
<span data-ttu-id="f38e2-180">PowerShell toosubmit Hive-lekérdezéseket is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f38e2-180">You can also use PowerShell toosubmit Hive queries.</span></span> <span data-ttu-id="f38e2-181">Útmutatásért lásd: [elküldeni a Hive feladatok PowerShell-lel](../hdinsight/hdinsight-hadoop-use-hive-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f38e2-181">For instructions, see [Submit Hive jobs using PowerShell](../hdinsight/hdinsight-hadoop-use-hive-powershell.md).</span></span>

## <span data-ttu-id="f38e2-182"><a name="create-tables"></a>Hive-adatbázis és tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="f38e2-182"><a name="create-tables"></a>Create Hive database and tables</span></span>
<span data-ttu-id="f38e2-183">hello Hive-lekérdezéseket is meg van osztva hello [GitHub-tárházban](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) , és innen tölthető le.</span><span class="sxs-lookup"><span data-stu-id="f38e2-183">hello Hive queries are shared in hello [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) and can be downloaded from there.</span></span>

<span data-ttu-id="f38e2-184">Itt található hello Hive-lekérdezést, amely egy Hive táblát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f38e2-184">Here is hello Hive query that creates a Hive table.</span></span>

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

<span data-ttu-id="f38e2-185">Az alábbiakban hello mezőket, amelyekre szüksége van a tooplug és egyéb beállításokra hello leírása:</span><span class="sxs-lookup"><span data-stu-id="f38e2-185">Here are hello descriptions of hello fields that you need tooplug in and other configurations:</span></span>

* <span data-ttu-id="f38e2-186">**&#60; adatbázis neve >**: hello adatbázis, amelyet az toocreate hello nevét.</span><span class="sxs-lookup"><span data-stu-id="f38e2-186">**&#60;database name>**: hello name of hello database that you want toocreate.</span></span> <span data-ttu-id="f38e2-187">Ha csak toouse hello alapértelmezett adatbázis, a lekérdezés hello *adatbázis létrehozása...*  kihagyható.</span><span class="sxs-lookup"><span data-stu-id="f38e2-187">If you just want toouse hello default database, hello query *create database...* can be omitted.</span></span>
* <span data-ttu-id="f38e2-188">**&#60; tábla neve >**: hello tábla, amelyet az toocreate hello megadott adatbázison belül hello neve.</span><span class="sxs-lookup"><span data-stu-id="f38e2-188">**&#60;table name>**: hello name of hello table that you want toocreate within hello specified database.</span></span> <span data-ttu-id="f38e2-189">Ha azt szeretné, hogy toouse hello alapértelmezett adatbázis, hello tábla is közvetlenül elé *&#60; tábla neve >* nélkül &#60; adatbázis neve >.</span><span class="sxs-lookup"><span data-stu-id="f38e2-189">If you want toouse hello default database, hello table can be directly referred by *&#60;table name>* without &#60;database name>.</span></span>
* <span data-ttu-id="f38e2-190">**&#60; mező elválasztó >**: hello elválasztó hello adatok fájl toobe mezőinek határoló feltöltött toohello Hive tábla.</span><span class="sxs-lookup"><span data-stu-id="f38e2-190">**&#60;field separator>**: hello separator that delimits fields in hello data file toobe uploaded toohello Hive table.</span></span>
* <span data-ttu-id="f38e2-191">**&#60; Sorelválasztó >**: hello adatfájl sorainak határoló hello elválasztó.</span><span class="sxs-lookup"><span data-stu-id="f38e2-191">**&#60;line separator>**: hello separator that delimits lines in hello data file.</span></span>
* <span data-ttu-id="f38e2-192">**&#60; tárolási helye >**: az Azure storage toosave hello helyadatok Hive táblák hello.</span><span class="sxs-lookup"><span data-stu-id="f38e2-192">**&#60;storage location>**: hello Azure storage location toosave hello data of Hive tables.</span></span> <span data-ttu-id="f38e2-193">Ha nincs megadva *hely &#60; a tárolási helye >*, adatbázis hello és hello táblák tárolódnak *hive/adatraktár/* könyvtárban lévő hello alapértelmezett tároló hello Hive fürt alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="f38e2-193">If you do not specify *LOCATION &#60;storage location>*, hello database and hello tables are stored in *hive/warehouse/* directory in hello default container of hello Hive cluster by default.</span></span> <span data-ttu-id="f38e2-194">Ha azt szeretné, hogy toospecify hello tárolási helye, hello tárolási helye van toobe hello alapértelmezett tárolóban hello adatbázis és a táblára.</span><span class="sxs-lookup"><span data-stu-id="f38e2-194">If you want toospecify hello storage location, hello storage location has toobe within hello default container for hello database and tables.</span></span> <span data-ttu-id="f38e2-195">Ezen a helyen van hely relatív toohello alapértelmezett tároló hello fürt hello formátumban néven toobe *"wasb: / / / &#60; 1 könyvtár > /"* vagy *"wasb: / / / &#60; 1 könyvtár > / &#60; Directory 2 > / "*stb. Miután hello lekérdezés végrehajtása, hello relatív könyvtárak hello alapértelmezett tárolóban jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="f38e2-195">This location has toobe referred as location relative toohello default container of hello cluster in hello format of *'wasb:///&#60;directory 1>/'* or *'wasb:///&#60;directory 1>/&#60;directory 2>/'*, etc. After hello query is executed, hello relative directories are created within hello default container.</span></span>
* <span data-ttu-id="f38e2-196">**TBLPROPERTIES("Skip.Header.line.Count"="1")**: Ha hello adatfájl fejléc rendelkezik, akkor tooadd ezt a tulajdonságot **hello végén** a hello *tábla létrehozása* lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="f38e2-196">**TBLPROPERTIES("skip.header.line.count"="1")**: If hello data file has a header line, you have tooadd this property **at hello end** of hello *create table* query.</span></span> <span data-ttu-id="f38e2-197">Ellenkező esetben hello fejlécsort rekord toohello táblázatként be van töltve.</span><span class="sxs-lookup"><span data-stu-id="f38e2-197">Otherwise, hello header line is loaded as a record toohello table.</span></span> <span data-ttu-id="f38e2-198">Hello adatok fájlban nincs fejléc, ha ez a konfiguráció elhagyható hello lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="f38e2-198">If hello data file does not have a header line, this configuration can be omitted in hello query.</span></span>

## <span data-ttu-id="f38e2-199"><a name="load-data"></a>Adattáblák tooHive betöltése</span><span class="sxs-lookup"><span data-stu-id="f38e2-199"><a name="load-data"></a>Load data tooHive tables</span></span>
<span data-ttu-id="f38e2-200">Itt található, amely adatokat tölt be egy Hive tábla hello Hive-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="f38e2-200">Here is hello Hive query that loads data into a Hive table.</span></span>

    LOAD DATA INPATH '<path tooblob data>' INTO TABLE <database name>.<table name>;

* <span data-ttu-id="f38e2-201">**&#60; elérési út tooblob adatok >**: hello alapértelmezett tárolót a HDInsight Hadoop-fürt hello hello blob fájl feltöltése toobe toohello Hive tábla esetén hello *&#60; elérési út tooblob adatok >* hello formátumúnak kell lennie *"wasb: / / / &#60; ebben a tárolóban directory > / &#60; blob fájl neve >'*.</span><span class="sxs-lookup"><span data-stu-id="f38e2-201">**&#60;path tooblob data>**: If hello blob file toobe uploaded toohello Hive table is in hello default container of hello HDInsight Hadoop cluster, hello *&#60;path tooblob data>* should be in hello format *'wasb:///&#60;directory in this container>/&#60;blob file name>'*.</span></span> <span data-ttu-id="f38e2-202">hello fájlját egy további hello HDInsight Hadoop-fürt-tárolót is lehet.</span><span class="sxs-lookup"><span data-stu-id="f38e2-202">hello blob file can also be in an additional container of hello HDInsight Hadoop cluster.</span></span> <span data-ttu-id="f38e2-203">Ebben az esetben *&#60; elérési út tooblob adatok >* hello formátumúnak kell lennie *"wasb: / / &#60; a tároló neve > @&#60; tárfiók neve >.blob.core.windows.net/ &#60; a blob-fájl neve >'*.</span><span class="sxs-lookup"><span data-stu-id="f38e2-203">In this case, *&#60;path tooblob data>* should be in hello format *'wasb://&#60;container name>@&#60;storage account name>.blob.core.windows.net/&#60;blob file name>'*.</span></span>

  > [!NOTE]
  > <span data-ttu-id="f38e2-204">hello blob adatok feltöltése toobe tooHive táblához toobe hello alapértelmezett vagy hello storage-fiókjának hello Hadoop-fürt további tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f38e2-204">hello blob data toobe uploaded tooHive table has toobe in hello default or additional container of hello storage account for hello Hadoop cluster.</span></span> <span data-ttu-id="f38e2-205">Ellenkező esetben hello *adatok betöltése* lekérdezés nem sikerült panaszos, hogy hello adatok nem férhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="f38e2-205">Otherwise, hello *LOAD DATA* query fails complaining that it cannot access hello data.</span></span>
  >
  >

## <span data-ttu-id="f38e2-206"><a name="partition-orc"></a>Speciális témakörök: particionált tábla és a tároló Hive adatok ORC formátumban</span><span class="sxs-lookup"><span data-stu-id="f38e2-206"><a name="partition-orc"></a>Advanced topics: partitioned table and store Hive data in ORC format</span></span>
<span data-ttu-id="f38e2-207">Hello adatok mérete nagy, ha a lekérdezések csak tooscan hello tábla partíciója néhány hasznos hello tábla particionáló.</span><span class="sxs-lookup"><span data-stu-id="f38e2-207">If hello data is large, partitioning hello table is beneficial for queries that only need tooscan a few partitions of hello table.</span></span> <span data-ttu-id="f38e2-208">Például akkor ésszerű toopartition hello naplóadatokat egy webhely dátuma alapján.</span><span class="sxs-lookup"><span data-stu-id="f38e2-208">For instance, it is reasonable toopartition hello log data of a web site by dates.</span></span>

<span data-ttu-id="f38e2-209">Továbbá toopartitioning a Hive táblákat, a is előnyös toostore hello Hive adatok hello optimalizált sor oszlopos (ORC) formátumban.</span><span class="sxs-lookup"><span data-stu-id="f38e2-209">In addition toopartitioning Hive tables, it is also beneficial toostore hello Hive data in hello Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="f38e2-210">A formázás ORC további információkért lásd: <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">használatával ORC fájlokat javítja a teljesítményt, ha Hive olvasása, írása, és adatfeldolgozás</a>.</span><span class="sxs-lookup"><span data-stu-id="f38e2-210">For more information on ORC formatting, see <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">Using ORC files improves performance when Hive is reading, writing, and processing data</a>.</span></span>

### <a name="partitioned-table"></a><span data-ttu-id="f38e2-211">Particionált tábla</span><span class="sxs-lookup"><span data-stu-id="f38e2-211">Partitioned table</span></span>
<span data-ttu-id="f38e2-212">Itt található hello Hive-lekérdezést, amely létrehoz egy particionált táblához, és adatokat tölt be.</span><span class="sxs-lookup"><span data-stu-id="f38e2-212">Here is hello Hive query that creates a partitioned table and loads data into it.</span></span>

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path toohello source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

<span data-ttu-id="f38e2-213">Particionált táblák lekérdezésekor ajánlott tooadd hello partíció feltételének hello **kezdete** a hello `where` záradék, ez növeli a hello jelentősen keresés hatékonyságát.</span><span class="sxs-lookup"><span data-stu-id="f38e2-213">When querying partitioned tables, it is recommended tooadd hello partition condition in hello **beginning** of hello `where` clause as this improves hello efficacy of searching significantly.</span></span>

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <span data-ttu-id="f38e2-214"><a name="orc"></a>Hive adattárolásra ORC formátumban</span><span class="sxs-lookup"><span data-stu-id="f38e2-214"><a name="orc"></a>Store Hive data in ORC format</span></span>
<span data-ttu-id="f38e2-215">Ön közvetlenül adatai nem tölthetők be blobtárolóból hello ORC formátumban tárolt Hive táblákba.</span><span class="sxs-lookup"><span data-stu-id="f38e2-215">You cannot directly load data from blob storage into Hive tables that is stored in hello ORC format.</span></span> <span data-ttu-id="f38e2-216">Az alábbiakban hello lépéseket, hogy szüksége van az Azure-ból tootake tooload adatok hello blobok tooHive táblák ORC formátumban tárolja.</span><span class="sxs-lookup"><span data-stu-id="f38e2-216">Here are hello steps that hello you need tootake tooload data from Azure blobs tooHive tables stored in ORC format.</span></span>

<span data-ttu-id="f38e2-217">Létrehoz egy külső táblát **tárolt AS TEXTFILE** és az adatok betöltése a blob storage toohello táblából.</span><span class="sxs-lookup"><span data-stu-id="f38e2-217">Create an external table **STORED AS TEXTFILE** and load data from blob storage toohello table.</span></span>

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path toohello source file>' INTO TABLE <database name>.<table name>;

<span data-ttu-id="f38e2-218">Egy belső tábla létrehozása a hello hello külső tábla 1. lépésben hello ugyanaz a mezőhatároló, és tárolja az azonos sémából hello Hive adatok hello ORC formátumban.</span><span class="sxs-lookup"><span data-stu-id="f38e2-218">Create an internal table with hello same schema as hello external table in step 1, with hello same field delimiter, and store hello Hive data in hello ORC format.</span></span>

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

<span data-ttu-id="f38e2-219">Válassza ki az adatok az 1. lépésben hello külső táblázatból, majd szúrja be a hello ORC táblába</span><span class="sxs-lookup"><span data-stu-id="f38e2-219">Select data from hello external table in step 1 and insert into hello ORC table</span></span>

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> <span data-ttu-id="f38e2-220">Ha hello TEXTFILE tábla *&#60; adatbázis neve >. &#60; külső textfile táblanév >* partíciókkal rendelkezik, a 3. LÉPÉSBEN, hello `SELECT * FROM <database name>.<external textfile table name>` parancsot választja ki hello partíció változó hello mező adatkészlet visszaadott formában.</span><span class="sxs-lookup"><span data-stu-id="f38e2-220">If hello TEXTFILE table *&#60;database name>.&#60;external textfile table name>* has partitions, in STEP 3, hello `SELECT * FROM <database name>.<external textfile table name>` command selects hello partition variable as a field in hello returned data set.</span></span> <span data-ttu-id="f38e2-221">Hello való beillesztése történik *&#60; adatbázis neve >. &#60; ORC táblanév >* óta sikertelen *&#60; adatbázis neve >. &#60; ORC táblanév >* nincs hello partíció változó, egy hello táblaséma mező.</span><span class="sxs-lookup"><span data-stu-id="f38e2-221">Inserting it into hello *&#60;database name>.&#60;ORC table name>* fails since *&#60;database name>.&#60;ORC table name>* does not have hello partition variable as a field in hello table schema.</span></span> <span data-ttu-id="f38e2-222">Ebben az esetben szüksége toospecifically válassza hello mezők toobe túl beszúrni*&#60; adatbázis neve >. &#60; ORC táblanév >* az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f38e2-222">In this case, you need toospecifically select hello fields toobe inserted too*&#60;database name>.&#60;ORC table name>* as follows:</span></span>
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

<span data-ttu-id="f38e2-223">Biztonságos toodrop hello *&#60; külső textfile táblanév >* amikor lekérdezés után az összes adat a következő hello segítségével e behelyezve *&#60; adatbázis neve >. &#60; ORC táblanév >*:</span><span class="sxs-lookup"><span data-stu-id="f38e2-223">It is safe toodrop hello *&#60;external textfile table name>* when using hello following query after all data has been inserted into *&#60;database name>.&#60;ORC table name>*:</span></span>

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

<span data-ttu-id="f38e2-224">A következő eljárással, miután hello ORC formátum készen toouse-adatokat tartalmazó táblát kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f38e2-224">After following this procedure, you should have a table with data in hello ORC format ready toouse.</span></span>  
