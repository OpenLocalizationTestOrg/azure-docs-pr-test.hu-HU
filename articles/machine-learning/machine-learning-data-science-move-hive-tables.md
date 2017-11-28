---
title: "Hive táblák létrehozása és az adatok betöltése az Azure Blob Storage |} Microsoft Docs"
description: "Hive táblák létrehozása és a hive táblák blob adatainak betöltése"
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
ms.openlocfilehash: eca4ecd8f639bb9816903f4b1d1f999755da819c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a><span data-ttu-id="3000c-103">Hive táblák létrehozása és az adatok betöltése az Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3000c-103">Create Hive tables and load data from Azure Blob Storage</span></span>
<span data-ttu-id="3000c-104">Ebből a témakörből megismerheti, hogy a Hive táblák létrehozása és az adatok betöltése az Azure blob storage általános Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="3000c-104">This topic presents generic Hive queries that create Hive tables and load data from Azure blob storage.</span></span> <span data-ttu-id="3000c-105">Néhány is útmutatást a Hive Táblák particionálása és az optimalizált sor oszlopos (ORC) lekérdezés teljesítmény javítása érdekében formázás használatával.</span><span class="sxs-lookup"><span data-stu-id="3000c-105">Some guidance is also provided on partitioning Hive tables and on using the Optimized Row Columnar (ORC) formatting to improve query performance.</span></span>

<span data-ttu-id="3000c-106">Ez **menü** betöltik az adatokat tároló környezetekben, ahol az adatok is tárolhatók és feldolgozhatók, a csapat adatok tudományos folyamat (TDSP) során módját leíró témakörök hivatkozásait.</span><span class="sxs-lookup"><span data-stu-id="3000c-106">This **menu** links to topics that describe how to ingest data into target environments where the data can be stored and processed during the Team Data Science Process (TDSP).</span></span>

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="3000c-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3000c-107">Prerequisites</span></span>
<span data-ttu-id="3000c-108">Ez a cikk feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="3000c-108">This article assumes that you have:</span></span>

* <span data-ttu-id="3000c-109">Egy Azure storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3000c-109">Created an Azure storage account.</span></span> <span data-ttu-id="3000c-110">Ha módosítania kell az utasításokat, lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="3000c-110">If you need instructions, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
* <span data-ttu-id="3000c-111">A HDInsight szolgáltatásban egy testreszabott Hadoop-fürt üzembe helyezve.</span><span class="sxs-lookup"><span data-stu-id="3000c-111">Provisioned a customized Hadoop cluster with the HDInsight service.</span></span>  <span data-ttu-id="3000c-112">Ha módosítania kell az utasításokat, lásd: [testreszabása az Azure HDInsight Hadoop-fürtök Speciális elemzésekre](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="3000c-112">If you need instructions, see [Customize Azure HDInsight Hadoop clusters for advanced analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="3000c-113">A fürthöz engedélyezett távelérési jelentkezett be, és a Hadoop parancssori konzol megnyitása.</span><span class="sxs-lookup"><span data-stu-id="3000c-113">Enabled remote access to the cluster, logged in, and opened the Hadoop Command-Line console.</span></span> <span data-ttu-id="3000c-114">Ha módosítania kell az utasításokat, lásd: [a Head csomópont a Hadoop-fürt eléréséhez](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="3000c-114">If you need instructions, see [Access the Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

## <a name="upload-data-to-azure-blob-storage"></a><span data-ttu-id="3000c-115">Adatok feltöltése az Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="3000c-115">Upload data to Azure blob storage</span></span>
<span data-ttu-id="3000c-116">Ha a megjelenő utasításokat követve létrehozott Azure virtuális gép [állítson be egy Azure virtuális gép speciális elemzésekre](machine-learning-data-science-setup-virtual-machine.md), a parancsfájl kell lettek töltve az *C:\\felhasználók \\ \<felhasználónév\>\\dokumentumok\\adatok tudományos parancsfájlok* könyvtárhoz, a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="3000c-116">If you created an Azure virtual machine by following the instructions provided in [Set up an Azure virtual machine for advanced analytics](machine-learning-data-science-setup-virtual-machine.md), this script file should have been downloaded to the *C:\\Users\\\<user name\>\\Documents\\Data Science Scripts* directory on the virtual machine.</span></span> <span data-ttu-id="3000c-117">A következő Hive-lekérdezések csak szükséges, csatlakoztassa a saját adatok séma és az Azure blob storage konfiguráció készen áll arra, hogy elküldhesse a megfelelő mezőket.</span><span class="sxs-lookup"><span data-stu-id="3000c-117">These Hive queries only require that you plug in your own data schema and Azure blob storage configuration in the appropriate fields to be ready for submission.</span></span>

<span data-ttu-id="3000c-118">Feltételezzük, hogy a Hive táblák adatai van egy **tömörítetlen** táblázatos formátumban, és, hogy az adatok az alapértelmezett (vagy egy további) feltöltötte-e a tárfiók a Hadoop-fürt által használt tároló.</span><span class="sxs-lookup"><span data-stu-id="3000c-118">We assume that the data for Hive tables is in an **uncompressed** tabular format, and that the data has been uploaded to the default (or to an additional) container of the storage account used by the Hadoop cluster.</span></span>

<span data-ttu-id="3000c-119">Ha meg szeretné gyakorlat a **NYC Taxi út adatok**, kell:</span><span class="sxs-lookup"><span data-stu-id="3000c-119">If you want to practice on the **NYC Taxi Trip Data**, you need to:</span></span>

* <span data-ttu-id="3000c-120">**Töltse le** a 24 [NYC Taxi út adatok](http://www.andresmh.com/nyctaxitrips) (12 út fájlok és 12 jegy ára fájlok),</span><span class="sxs-lookup"><span data-stu-id="3000c-120">**download** the 24 [NYC Taxi Trip Data](http://www.andresmh.com/nyctaxitrips) files (12 Trip files and 12 Fare files),</span></span>
* <span data-ttu-id="3000c-121">**Csomagolja ki** .csv fájlokat, az összes fájl, majd</span><span class="sxs-lookup"><span data-stu-id="3000c-121">**unzip** all files into .csv files, and then</span></span>
* <span data-ttu-id="3000c-122">**Töltse fel** őket az Azure storage-fiók ismertetett eljárással létrehozott alapértelmezett (vagy megfelelő tárolót) a [testreszabása az Azure HDInsight Hadoop-fürtök Advanced Analytics folyamat és a technológia](machine-learning-data-science-customize-hadoop-cluster.md)témakör.</span><span class="sxs-lookup"><span data-stu-id="3000c-122">**upload** them to the default (or appropriate container) of the Azure storage account that was created by the procedure outlined in the [Customize Azure HDInsight Hadoop clusters for Advanced Analytics Process and Technology](machine-learning-data-science-customize-hadoop-cluster.md) topic.</span></span> <span data-ttu-id="3000c-123">A folyamat a .csv fájlok feltöltése az alapértelmezett tároló a tárfiók itt található [lap](machine-learning-data-science-process-hive-walkthrough.md#upload).</span><span class="sxs-lookup"><span data-stu-id="3000c-123">The process to upload the .csv files to the default container on the storage account can be found on this [page](machine-learning-data-science-process-hive-walkthrough.md#upload).</span></span>

## <span data-ttu-id="3000c-124"><a name="submit"></a>Hogyan lehet elküldeni a Hive-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="3000c-124"><a name="submit"></a>How to submit Hive queries</span></span>
<span data-ttu-id="3000c-125">Hive-lekérdezések segítségével küldheti el:</span><span class="sxs-lookup"><span data-stu-id="3000c-125">Hive queries can be submitted by using:</span></span>

1. [<span data-ttu-id="3000c-126">A Hadoop-fürt headnode keresztül Hadoop parancssori Hive-lekérdezések elküldése</span><span class="sxs-lookup"><span data-stu-id="3000c-126">Submit Hive queries through Hadoop Command Line in headnode of Hadoop cluster</span></span>](#headnode)
2. [<span data-ttu-id="3000c-127">A Hive szerkesztő Hive-lekérdezések elküldése</span><span class="sxs-lookup"><span data-stu-id="3000c-127">Submit Hive queries with the Hive Editor</span></span>](#hive-editor)
3. [<span data-ttu-id="3000c-128">Küldje el az Azure PowerShell-parancsokkal Hive-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="3000c-128">Submit Hive queries with Azure PowerShell Commands</span></span>](#ps)

<span data-ttu-id="3000c-129">Hive-lekérdezések SQL-szerű.</span><span class="sxs-lookup"><span data-stu-id="3000c-129">Hive queries are SQL-like.</span></span> <span data-ttu-id="3000c-130">Ha ismeri az SQL, azt tapasztalhatja a [SQL felhasználók Cheat lap Hive](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) hasznos.</span><span class="sxs-lookup"><span data-stu-id="3000c-130">If you are familiar with SQL, you may find the [Hive for SQL Users Cheat Sheet](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) useful.</span></span>

<span data-ttu-id="3000c-131">A Hive-lekérdezés elküldésekor azt is meghatározhatja, és Hive-lekérdezések eredményének a képernyőn vagy az átjárócsomópont helyi fájlba vagy egy Azure-blobba legyen.</span><span class="sxs-lookup"><span data-stu-id="3000c-131">When submitting a Hive query, you can also control the destination of the output from Hive queries, whether it be on the screen or to a local file on the head node or to an Azure blob.</span></span>

### <span data-ttu-id="3000c-132"><a name="headnode"></a> 1. A Hadoop-fürt headnode keresztül Hadoop parancssori Hive-lekérdezések elküldése</span><span class="sxs-lookup"><span data-stu-id="3000c-132"><a name="headnode"></a> 1. Submit Hive queries through Hadoop Command Line in headnode of Hadoop cluster</span></span>
<span data-ttu-id="3000c-133">A Hive lekérdezés túl összetett, ha közvetlenül a Hadoop átjárócsomópontjához a továbbítás fürt általában vezet, mint a Hive szerkesztő vagy az Azure PowerShell parancsfájlok továbbítás gyorsabban kapcsolja.</span><span class="sxs-lookup"><span data-stu-id="3000c-133">If the Hive query is complex, submitting it directly in the head node of the Hadoop cluster typically leads to faster turn around than submitting it with a Hive Editor or Azure PowerShell scripts.</span></span>

<span data-ttu-id="3000c-134">Jelentkezzen be a Hadoop-fürt átjárócsomópontjához, nyissa meg a Hadoop parancssort az átjárócsomópont asztalán, és írja be a parancs `cd %hive_home%\bin`.</span><span class="sxs-lookup"><span data-stu-id="3000c-134">Log in to the head node of the Hadoop cluster, open the Hadoop Command Line on the desktop of the head node, and enter command `cd %hive_home%\bin`.</span></span>

<span data-ttu-id="3000c-135">Háromféleképpen elküldeni a Hive-lekérdezéseket a Hadoop parancssorban:</span><span class="sxs-lookup"><span data-stu-id="3000c-135">You have three ways to submit Hive queries in the Hadoop Command Line:</span></span>

* <span data-ttu-id="3000c-136">közvetlenül</span><span class="sxs-lookup"><span data-stu-id="3000c-136">directly</span></span>
* <span data-ttu-id="3000c-137">.hql fájlok használata</span><span class="sxs-lookup"><span data-stu-id="3000c-137">using .hql files</span></span>
* <span data-ttu-id="3000c-138">a Hive parancs konzol</span><span class="sxs-lookup"><span data-stu-id="3000c-138">with the Hive command console</span></span>

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a><span data-ttu-id="3000c-139">Küldje el közvetlenül a Hadoop parancssori Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="3000c-139">Submit Hive queries directly in Hadoop Command Line.</span></span>
<span data-ttu-id="3000c-140">Például a parancs futtatása `hive -e "<your hive query>;` egyszerű Hive-lekérdezéseket közvetlenül a Hadoop parancssori elküldeni.</span><span class="sxs-lookup"><span data-stu-id="3000c-140">You can run command like `hive -e "<your hive query>;` to submit simple Hive queries directly in Hadoop Command Line.</span></span> <span data-ttu-id="3000c-141">Íme egy példa, ahol a vörös téglalappal ismerteti a parancs, amellyel a Hive-lekérdezést, és a zöld lista ismerteti a Hive-lekérdezések eredményének.</span><span class="sxs-lookup"><span data-stu-id="3000c-141">Here is an example, where the red box outlines the command that submits the Hive query, and the green box outlines the output from the Hive query.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a><span data-ttu-id="3000c-143">Küldje el a Hive-lekérdezéseket a .hql fájlok</span><span class="sxs-lookup"><span data-stu-id="3000c-143">Submit Hive queries in .hql files</span></span>
<span data-ttu-id="3000c-144">A Hive-lekérdezések bonyolultabb, és több vonal van, a parancssor vagy a Hive parancskonzolról lekérdezések szerkesztése esetén nem gyakorlati.</span><span class="sxs-lookup"><span data-stu-id="3000c-144">When the Hive query is more complicated and has multiple lines, editing queries in command line or Hive command console is not practical.</span></span> <span data-ttu-id="3000c-145">A másik lehetőség a Hadoop-fürt átjárócsomópontjához egy szövegszerkesztő segítségével egy helyi könyvtárban, az átjárócsomópont .hql fájlba mentése a Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="3000c-145">An alternative is to use a text editor in the head node of the Hadoop cluster to save the Hive queries in a .hql file in a local directory of the head node.</span></span> <span data-ttu-id="3000c-146">A Hive-lekérdezést a .hql fájl segítségével küldheti el, majd a `-f` argumentum az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="3000c-146">Then the Hive query in the .hql file can be submitted by using the `-f` argument as follows:</span></span>

    hive -f "<path to the .hql file>"

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)

<span data-ttu-id="3000c-148">**Ne jelenjen meg többé folyamat állapota képernyőn nyomtatás Hive-lekérdezések**</span><span class="sxs-lookup"><span data-stu-id="3000c-148">**Suppress progress status screen print of Hive queries**</span></span>

<span data-ttu-id="3000c-149">Alapértelmezés szerint a Hadoop parancssorban Hive-lekérdezés elküldése után a térkép vagy csökkentse a feladat előrehaladását nyomtatása a képernyőn.</span><span class="sxs-lookup"><span data-stu-id="3000c-149">By default, after Hive query is submitted in Hadoop Command Line, the progress of the Map/Reduce job is printed out on screen.</span></span> <span data-ttu-id="3000c-150">A képernyő Nyomtatás a térkép vagy csökkentse a feladat előrehaladását a mellőzése, argumentumot is használhat `-S` ("S" nagybetűvel) a parancsban sor az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="3000c-150">To suppress the screen print of the Map/Reduce job progress, you can use an argument `-S` ("S" in upper case) in the command line as follows:</span></span>

    hive -S -f "<path to the .hql file>"
<span data-ttu-id="3000c-151">.</span><span class="sxs-lookup"><span data-stu-id="3000c-151">.</span></span>    <span data-ttu-id="3000c-152">Hive -S -e "<Hive queries>"</span><span class="sxs-lookup"><span data-stu-id="3000c-152">hive -S -e "<Hive queries>"</span></span>

#### <a name="submit-hive-queries-in-hive-command-console"></a><span data-ttu-id="3000c-153">Küldje el a Hive parancskonzolról Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="3000c-153">Submit Hive queries in Hive command console.</span></span>
<span data-ttu-id="3000c-154">Először is megadhat a Hive parancskonzolról parancs futtatásával `hive` a Hadoop parancssor, és küldje el a Hive parancskonzolról Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="3000c-154">You can also first enter the Hive command console by running command `hive` in Hadoop Command Line, and then submit Hive queries in Hive command console.</span></span> <span data-ttu-id="3000c-155">Íme egy példa.</span><span class="sxs-lookup"><span data-stu-id="3000c-155">Here is an example.</span></span> <span data-ttu-id="3000c-156">Ebben a példában a két piros mezőkbe írja be a Hive parancskonzolról használt parancsok, és a Hive-lekérdezés Hive parancskonzolról, illetve benyújtott jelöljön ki.</span><span class="sxs-lookup"><span data-stu-id="3000c-156">In this example, the two red boxes highlight the commands used to enter the Hive command console, and the Hive query submitted in Hive command console, respectively.</span></span> <span data-ttu-id="3000c-157">A zöld mező a Hive-lekérdezések eredményének mutatja be.</span><span class="sxs-lookup"><span data-stu-id="3000c-157">The green box highlights the output from the Hive query.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

<span data-ttu-id="3000c-159">Az előző példákban közvetlenül kimeneti a Hive-lekérdezések eredményeit a képernyőn.</span><span class="sxs-lookup"><span data-stu-id="3000c-159">The previous examples directly output the Hive query results on screen.</span></span> <span data-ttu-id="3000c-160">Is kiírhatja a kimenetet egy helyi fájlba az átjárócsomóponthoz, vagy egy Azure-blobba.</span><span class="sxs-lookup"><span data-stu-id="3000c-160">You can also write the output to a local file on the head node, or to an Azure blob.</span></span> <span data-ttu-id="3000c-161">Más eszközök segítségével, majd további a Hive-lekérdezések kimenetének elemzése.</span><span class="sxs-lookup"><span data-stu-id="3000c-161">Then, you can use other tools to further analyze the output of Hive queries.</span></span>

<span data-ttu-id="3000c-162">**A kimeneti Hive lekérdezés eredményei egy helyi fájlba.**</span><span class="sxs-lookup"><span data-stu-id="3000c-162">**Output Hive query results to a local file.**</span></span>
<span data-ttu-id="3000c-163">Kimeneti Hive lekérdezés eredményei az átjárócsomópont helyi könyvtárába, van a következőképpen elküldeni a Hive-lekérdezést a Hadoop parancssorban:</span><span class="sxs-lookup"><span data-stu-id="3000c-163">To output Hive query results to a local directory on the head node, you have to submit the Hive query in the Hadoop Command Line as follows:</span></span>

    hive -e "<hive query>" > <local path in the head node>

<span data-ttu-id="3000c-164">A következő példa a Hive-lekérdezések eredményének egy fájlba írása `hivequeryoutput.txt` könyvtárban `C:\apps\temp`.</span><span class="sxs-lookup"><span data-stu-id="3000c-164">In the following example, the output of Hive query is written into a file `hivequeryoutput.txt` in directory `C:\apps\temp`.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

<span data-ttu-id="3000c-166">**Kimeneti Hive lekérdezés eredményei egy Azure-blobba**</span><span class="sxs-lookup"><span data-stu-id="3000c-166">**Output Hive query results to an Azure blob**</span></span>

<span data-ttu-id="3000c-167">A Hive lekérdezés eredményeit az Azure-blobba, az alapértelmezett tároló, a Hadoop-fürt belül is készíthető.</span><span class="sxs-lookup"><span data-stu-id="3000c-167">You can also output the Hive query results to an Azure blob, within the default container of the Hadoop cluster.</span></span> <span data-ttu-id="3000c-168">A Hive-lekérdezést a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="3000c-168">The Hive query for this is as follows:</span></span>

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

<span data-ttu-id="3000c-169">A következő példában a Hive-lekérdezések eredményének beíródik egy blob könyvtár `queryoutputdir` a Hadoop-fürt alapértelmezett tárolóban.</span><span class="sxs-lookup"><span data-stu-id="3000c-169">In the following example, the output of Hive query is written to a blob directory `queryoutputdir` within the default container of the Hadoop cluster.</span></span> <span data-ttu-id="3000c-170">Itt csak szüksége arra, hogy a könyvtár nevét, a blob neve nélkül.</span><span class="sxs-lookup"><span data-stu-id="3000c-170">Here, you only need to provide the directory name, without the blob name.</span></span> <span data-ttu-id="3000c-171">Hiba történt kívánja megadni a címtár és a blob neve, mint például `wasb:///queryoutputdir/queryoutput.txt`.</span><span class="sxs-lookup"><span data-stu-id="3000c-171">An error is thrown if you provide both directory and blob names, such as `wasb:///queryoutputdir/queryoutput.txt`.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

<span data-ttu-id="3000c-173">Ha az alapértelmezett tároló, a Hadoop-fürt használata Azure Tártallózó megnyitni, láthatja az alábbi ábrán látható módon a Hive-lekérdezések eredményének.</span><span class="sxs-lookup"><span data-stu-id="3000c-173">If you open the default container of the Hadoop cluster using Azure Storage Explorer, you can see the output of the Hive query as shown in the following figure.</span></span> <span data-ttu-id="3000c-174">Csak a nevek a megadott meghajtóbetűjellel rendelkező blob beolvasása a szűrő (vörös téglalappal által kiemelt) is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="3000c-174">You can apply the filter (highlighted by red box) to only retrieve the blob with specified letters in names.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

### <span data-ttu-id="3000c-176"><a name="hive-editor"></a> 2. A Hive szerkesztő Hive-lekérdezések elküldése</span><span class="sxs-lookup"><span data-stu-id="3000c-176"><a name="hive-editor"></a> 2. Submit Hive queries with the Hive Editor</span></span>
<span data-ttu-id="3000c-177">A lekérdezés konzol (Hive szerkesztő) írja be egy URL-cím is használható *https://&#60; Hadoop-fürt neve >.azurehdinsight.net/Home/HiveEditor* egy webböngészőbe.</span><span class="sxs-lookup"><span data-stu-id="3000c-177">You can also use the Query Console (Hive Editor) by entering a URL of the form *https://&#60;Hadoop cluster name>.azurehdinsight.net/Home/HiveEditor* into a web browser.</span></span> <span data-ttu-id="3000c-178">Végezhetik el a további részletekért lásd a konzol bejelentkezett, és így a Hadoop fürthöz hitelesítő adatait itt kell.</span><span class="sxs-lookup"><span data-stu-id="3000c-178">You must be logged in the see this console and so you need your Hadoop cluster credentials here.</span></span>

### <span data-ttu-id="3000c-179"><a name="ps"></a> 3. Küldje el az Azure PowerShell-parancsokkal Hive-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="3000c-179"><a name="ps"></a> 3. Submit Hive queries with Azure PowerShell Commands</span></span>
<span data-ttu-id="3000c-180">PowerShell elküldeni a Hive-lekérdezéseket is használható.</span><span class="sxs-lookup"><span data-stu-id="3000c-180">You can also use PowerShell to submit Hive queries.</span></span> <span data-ttu-id="3000c-181">Útmutatásért lásd: [elküldeni a Hive feladatok PowerShell-lel](../hdinsight/hdinsight-hadoop-use-hive-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="3000c-181">For instructions, see [Submit Hive jobs using PowerShell](../hdinsight/hdinsight-hadoop-use-hive-powershell.md).</span></span>

## <span data-ttu-id="3000c-182"><a name="create-tables"></a>Hive-adatbázis és tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="3000c-182"><a name="create-tables"></a>Create Hive database and tables</span></span>
<span data-ttu-id="3000c-183">A Hive-lekérdezéseket is meg van osztva a [GitHub-tárházban](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) , és innen tölthető le.</span><span class="sxs-lookup"><span data-stu-id="3000c-183">The Hive queries are shared in the [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) and can be downloaded from there.</span></span>

<span data-ttu-id="3000c-184">Ez a Hive lekérdezés, amely egy Hive táblát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3000c-184">Here is the Hive query that creates a Hive table.</span></span>

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

<span data-ttu-id="3000c-185">Az alábbiakban a mezőket, amelyeknek kell csatlakoztatni és egyéb beállításokra leírása:</span><span class="sxs-lookup"><span data-stu-id="3000c-185">Here are the descriptions of the fields that you need to plug in and other configurations:</span></span>

* <span data-ttu-id="3000c-186">**&#60; adatbázis neve >**: a létrehozni kívánt adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="3000c-186">**&#60;database name>**: the name of the database that you want to create.</span></span> <span data-ttu-id="3000c-187">Ha szeretné használni az alapértelmezett adatbázis, a lekérdezés *adatbázis létrehozása...*  kihagyható.</span><span class="sxs-lookup"><span data-stu-id="3000c-187">If you just want to use the default database, the query *create database...* can be omitted.</span></span>
* <span data-ttu-id="3000c-188">**&#60; tábla neve >**: a táblázat, amely szeretne létrehozni a megadott adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="3000c-188">**&#60;table name>**: the name of the table that you want to create within the specified database.</span></span> <span data-ttu-id="3000c-189">Ha szeretné használni az alapértelmezett adatbázis, a tábla is közvetlenül elé *&#60; tábla neve >* nélkül &#60; adatbázis neve >.</span><span class="sxs-lookup"><span data-stu-id="3000c-189">If you want to use the default database, the table can be directly referred by *&#60;table name>* without &#60;database name>.</span></span>
* <span data-ttu-id="3000c-190">**&#60; mező elválasztó >**: az elválasztó, amely az adatfájlban a Hive tábla feltölteni kívánt mezőket.</span><span class="sxs-lookup"><span data-stu-id="3000c-190">**&#60;field separator>**: the separator that delimits fields in the data file to be uploaded to the Hive table.</span></span>
* <span data-ttu-id="3000c-191">**&#60; Sorelválasztó >**: az elválasztó, amely az adatfájl sorainak.</span><span class="sxs-lookup"><span data-stu-id="3000c-191">**&#60;line separator>**: the separator that delimits lines in the data file.</span></span>
* <span data-ttu-id="3000c-192">**&#60; tárolási helye >**: az Azure storage-helyre menteni az adatokat a Hive táblák.</span><span class="sxs-lookup"><span data-stu-id="3000c-192">**&#60;storage location>**: the Azure storage location to save the data of Hive tables.</span></span> <span data-ttu-id="3000c-193">Ha nincs megadva *hely &#60; a tárolási helye >*, az adatbázis és a táblázatok tárolja *hive/adatraktár/* könyvtárban lévő az alapértelmezett tároló alapértelmezés szerint a Hive-fürt.</span><span class="sxs-lookup"><span data-stu-id="3000c-193">If you do not specify *LOCATION &#60;storage location>*, the database and the tables are stored in *hive/warehouse/* directory in the default container of the Hive cluster by default.</span></span> <span data-ttu-id="3000c-194">Ha meg szeretné határozni a tárolási helye, a tárolási hely nem lehet az adatbázis és a táblák alapértelmezett tárolóban.</span><span class="sxs-lookup"><span data-stu-id="3000c-194">If you want to specify the storage location, the storage location has to be within the default container for the database and tables.</span></span> <span data-ttu-id="3000c-195">Ezen a helyen van, a fürt formátumban viszonyítva az alapértelmezett tároló helye elé *"wasb: / / / &#60; könyvtár 1 > /"* vagy *"wasb: / / / &#60; 1 könyvtár > / &#60; directory 2 > / "*stb. A lekérdezés végrehajtása után a relatív könyvtárak alapértelmezett tárolóban jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="3000c-195">This location has to be referred as location relative to the default container of the cluster in the format of *'wasb:///&#60;directory 1>/'* or *'wasb:///&#60;directory 1>/&#60;directory 2>/'*, etc. After the query is executed, the relative directories are created within the default container.</span></span>
* <span data-ttu-id="3000c-196">**TBLPROPERTIES("Skip.Header.line.Count"="1")**: Ha az adatfájl fejléc, fel kell vennie a tulajdonság **végén** , a *tábla létrehozása* lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="3000c-196">**TBLPROPERTIES("skip.header.line.count"="1")**: If the data file has a header line, you have to add this property **at the end** of the *create table* query.</span></span> <span data-ttu-id="3000c-197">Ellenkező esetben a fejlécsort a táblázathoz rekordként be van töltve.</span><span class="sxs-lookup"><span data-stu-id="3000c-197">Otherwise, the header line is loaded as a record to the table.</span></span> <span data-ttu-id="3000c-198">Ha az adatok fájlban nincs fejléc, ez a konfiguráció elhagyható a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="3000c-198">If the data file does not have a header line, this configuration can be omitted in the query.</span></span>

## <span data-ttu-id="3000c-199"><a name="load-data"></a>Adatok betöltése a Hive táblák</span><span class="sxs-lookup"><span data-stu-id="3000c-199"><a name="load-data"></a>Load data to Hive tables</span></span>
<span data-ttu-id="3000c-200">Ez a Hive-lekérdezés, amely adatokat tölt be egy Hive tábla.</span><span class="sxs-lookup"><span data-stu-id="3000c-200">Here is the Hive query that loads data into a Hive table.</span></span>

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

* <span data-ttu-id="3000c-201">**&#60; blob adatok elérési útja >**: az alapértelmezett tárolóban, a HDInsight Hadoop-fürt, a blob-fájlt fel kell tölteni a Hive tábla esetén a *&#60; blob adatok elérési útja >* formátumban kell megadni *" wasb: / / / &#60; ebben a tárolóban directory > / &#60; blob fájl neve >'*.</span><span class="sxs-lookup"><span data-stu-id="3000c-201">**&#60;path to blob data>**: If the blob file to be uploaded to the Hive table is in the default container of the HDInsight Hadoop cluster, the *&#60;path to blob data>* should be in the format *'wasb:///&#60;directory in this container>/&#60;blob file name>'*.</span></span> <span data-ttu-id="3000c-202">A blob fájl is lehet egy további tárolót a HDInsight Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="3000c-202">The blob file can also be in an additional container of the HDInsight Hadoop cluster.</span></span> <span data-ttu-id="3000c-203">Ebben az esetben *&#60; blob adatok elérési útja >* formátumban kell megadni *"wasb: / / &#60; a tároló neve > @&#60; tárfiók neve >.blob.core.windows.net/ &#60; a blob-fájl neve >'*.</span><span class="sxs-lookup"><span data-stu-id="3000c-203">In this case, *&#60;path to blob data>* should be in the format *'wasb://&#60;container name>@&#60;storage account name>.blob.core.windows.net/&#60;blob file name>'*.</span></span>

  > [!NOTE]
  > <span data-ttu-id="3000c-204">A blobadatokat feltöltendő Hive táblát nem lehet az alapértelmezett vagy a tárfiók a Hadoop-fürt további tárolóban.</span><span class="sxs-lookup"><span data-stu-id="3000c-204">The blob data to be uploaded to Hive table has to be in the default or additional container of the storage account for the Hadoop cluster.</span></span> <span data-ttu-id="3000c-205">Ellenkező esetben a *adatok betöltése* lekérdezés nem sikerült panaszos, hogy az adatok nem férhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="3000c-205">Otherwise, the *LOAD DATA* query fails complaining that it cannot access the data.</span></span>
  >
  >

## <span data-ttu-id="3000c-206"><a name="partition-orc"></a>Speciális témakörök: particionált tábla és a tároló Hive adatok ORC formátumban</span><span class="sxs-lookup"><span data-stu-id="3000c-206"><a name="partition-orc"></a>Advanced topics: partitioned table and store Hive data in ORC format</span></span>
<span data-ttu-id="3000c-207">Az adatok nagy, a tábla particionáló akkor hasznos, amelyeket csak a táblázat néhány partíciók vizsgálata lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="3000c-207">If the data is large, partitioning the table is beneficial for queries that only need to scan a few partitions of the table.</span></span> <span data-ttu-id="3000c-208">Például akkor ésszerű a naplózási adatokat egy webhely particionálásához dátuma alapján.</span><span class="sxs-lookup"><span data-stu-id="3000c-208">For instance, it is reasonable to partition the log data of a web site by dates.</span></span>

<span data-ttu-id="3000c-209">Hive Táblák particionálása mellett célszerű is a Hive adatokat tároló optimalizált sor oszlopos (ORC) formátumban.</span><span class="sxs-lookup"><span data-stu-id="3000c-209">In addition to partitioning Hive tables, it is also beneficial to store the Hive data in the Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="3000c-210">A formázás ORC további információkért lásd: <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">használatával ORC fájlokat javítja a teljesítményt, ha Hive olvasása, írása, és adatfeldolgozás</a>.</span><span class="sxs-lookup"><span data-stu-id="3000c-210">For more information on ORC formatting, see <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">Using ORC files improves performance when Hive is reading, writing, and processing data</a>.</span></span>

### <a name="partitioned-table"></a><span data-ttu-id="3000c-211">Particionált tábla</span><span class="sxs-lookup"><span data-stu-id="3000c-211">Partitioned table</span></span>
<span data-ttu-id="3000c-212">Ez a Hive-lekérdezés, amely létrehoz egy particionált táblához, és adatokat tölt be.</span><span class="sxs-lookup"><span data-stu-id="3000c-212">Here is the Hive query that creates a partitioned table and loads data into it.</span></span>

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

<span data-ttu-id="3000c-213">Particionált táblák lekérdezésekor javasoljuk, hogy a partíció feltétel hozzáadása a **kezdete** , a `where` záradék, ez növeli a jelentősen keresés hatékonyságát.</span><span class="sxs-lookup"><span data-stu-id="3000c-213">When querying partitioned tables, it is recommended to add the partition condition in the **beginning** of the `where` clause as this improves the efficacy of searching significantly.</span></span>

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <span data-ttu-id="3000c-214"><a name="orc"></a>Hive adattárolásra ORC formátumban</span><span class="sxs-lookup"><span data-stu-id="3000c-214"><a name="orc"></a>Store Hive data in ORC format</span></span>
<span data-ttu-id="3000c-215">Ön nem közvetlenül adatok betöltése az blob-tároló az ORC formátumban tárolt Hive táblákat.</span><span class="sxs-lookup"><span data-stu-id="3000c-215">You cannot directly load data from blob storage into Hive tables that is stored in the ORC format.</span></span> <span data-ttu-id="3000c-216">Az alábbiakban a lépéseket, amelyek a kell venni a betöltése az Azure-ból adatokat blobok Hive táblákat ORC formátumban tárolja.</span><span class="sxs-lookup"><span data-stu-id="3000c-216">Here are the steps that the you need to take to load data from Azure blobs to Hive tables stored in ORC format.</span></span>

<span data-ttu-id="3000c-217">Létrehoz egy külső táblát **tárolt AS TEXTFILE** és az adatok betöltése az blob storage a táblába.</span><span class="sxs-lookup"><span data-stu-id="3000c-217">Create an external table **STORED AS TEXTFILE** and load data from blob storage to the table.</span></span>

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

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

<span data-ttu-id="3000c-218">Hozzon létre egy belső tábla, mint a külső tábla 1. lépésben az azonos a mezőhatárolóval ugyanazon séma, és a Hive adatokat tároló ORC formátumban.</span><span class="sxs-lookup"><span data-stu-id="3000c-218">Create an internal table with the same schema as the external table in step 1, with the same field delimiter, and store the Hive data in the ORC format.</span></span>

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

<span data-ttu-id="3000c-219">Válassza az 1. lépésben a külső tábla az adatok és az ORC táblázat beszúrása</span><span class="sxs-lookup"><span data-stu-id="3000c-219">Select data from the external table in step 1 and insert into the ORC table</span></span>

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> <span data-ttu-id="3000c-220">Ha a TEXTFILE tábla *&#60; adatbázis neve >. &#60; külső textfile táblanév >* partíciókkal rendelkezik, a 3. LÉPÉSBEN, a `SELECT * FROM <database name>.<external textfile table name>` parancsot választja a partíció változó a visszaadott adatkészlet mező.</span><span class="sxs-lookup"><span data-stu-id="3000c-220">If the TEXTFILE table *&#60;database name>.&#60;external textfile table name>* has partitions, in STEP 3, the `SELECT * FROM <database name>.<external textfile table name>` command selects the partition variable as a field in the returned data set.</span></span> <span data-ttu-id="3000c-221">Beszúrása be azt a *&#60; adatbázis neve >. &#60; ORC táblanév >* óta sikertelen *&#60; adatbázis neve >. &#60; ORC táblanév >* nem rendelkezik a partíció változó mezőként az a következő tábla sémáját.</span><span class="sxs-lookup"><span data-stu-id="3000c-221">Inserting it into the *&#60;database name>.&#60;ORC table name>* fails since *&#60;database name>.&#60;ORC table name>* does not have the partition variable as a field in the table schema.</span></span> <span data-ttu-id="3000c-222">Ebben az esetben kell kifejezetten válassza ki a mezőket a beszúrni *&#60; adatbázis neve >. &#60; ORC táblanév >* az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="3000c-222">In this case, you need to specifically select the fields to be inserted to *&#60;database name>.&#60;ORC table name>* as follows:</span></span>
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

<span data-ttu-id="3000c-223">Biztonságos dobja el a *&#60; külső textfile táblanév >* amikor után minden adat a következő lekérdezéssel e behelyezve *&#60; adatbázis neve >. &#60; ORC táblanév >*:</span><span class="sxs-lookup"><span data-stu-id="3000c-223">It is safe to drop the *&#60;external textfile table name>* when using the following query after all data has been inserted into *&#60;database name>.&#60;ORC table name>*:</span></span>

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

<span data-ttu-id="3000c-224">A következő eljárással, után készen áll a használatra ORC formátumú adatokat tartalmazó táblát kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="3000c-224">After following this procedure, you should have a table with data in the ORC format ready to use.</span></span>  
