---
title: "Adatait átmásolja WASB érkező vagy oda irányuló Data Lake Store használatának ból a Distcp |} Microsoft Docs"
description: "Adatok másolása és az Azure Storage Blobs Data Lake Store-ból a Distcp eszközzel"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ae2e9506-69dd-4b95-8759-4dadca37ea70
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 356a93d330e9e8ce3eb3c6c982fc5c2e087846a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a><span data-ttu-id="04ee3-103">Adatmásolás az Azure Storage-blobok és a Data Lake Store között a DistCp-vel</span><span class="sxs-lookup"><span data-stu-id="04ee3-103">Use Distcp to copy data between Azure Storage Blobs and Data Lake Store</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="04ee3-104">A DistCp használata</span><span class="sxs-lookup"><span data-stu-id="04ee3-104">Using DistCp</span></span>](data-lake-store-copy-data-wasb-distcp.md)
> * [<span data-ttu-id="04ee3-105">Az AdlCopy használata</span><span class="sxs-lookup"><span data-stu-id="04ee3-105">Using AdlCopy</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
>
>

<span data-ttu-id="04ee3-106">Miután létrehozta a HDInsight-fürtöt, amely egy Data Lake Store-fiók hozzáféréssel rendelkezik, használhatja adatok másolása Hadoop-ökoszisztémával eszközök például ból a Distcp **a** egy HDInsight fürt storage (WASB) a Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="04ee3-106">Once you have created an HDInsight cluster that has access to a Data Lake Store account, you can use Hadoop ecosystem tools like Distcp to copy data **to and from** an HDInsight cluster storage (WASB) into a Data Lake Store account.</span></span> <span data-ttu-id="04ee3-107">Ez a cikk útmutatás ennek érdekében.</span><span class="sxs-lookup"><span data-stu-id="04ee3-107">This article provides instructions on how to achieve this.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="04ee3-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="04ee3-108">Prerequisites</span></span>
<span data-ttu-id="04ee3-109">A cikk elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="04ee3-109">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="04ee3-110">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="04ee3-110">**An Azure subscription**.</span></span> <span data-ttu-id="04ee3-111">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="04ee3-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="04ee3-112">**Egy Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="04ee3-112">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="04ee3-113">Hogyan hozhat létre ilyet, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="04ee3-113">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="04ee3-114">**Az Azure HDInsight-fürt** a Data Lake Store-fiók eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="04ee3-114">**Azure HDInsight cluster** with access to a Data Lake Store account.</span></span> <span data-ttu-id="04ee3-115">Lásd: [HDInsight-fürtök létrehozása a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="04ee3-115">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="04ee3-116">Győződjön meg arról, hogy a fürt számára engedélyezi a távoli asztal.</span><span class="sxs-lookup"><span data-stu-id="04ee3-116">Make sure you enable Remote Desktop for the cluster.</span></span>

## <a name="do-you-learn-fast-with-videos"></a><span data-ttu-id="04ee3-117">Gyorsan tanul videók segítségével?</span><span class="sxs-lookup"><span data-stu-id="04ee3-117">Do you learn fast with videos?</span></span>
<span data-ttu-id="04ee3-118">[A videó](https://mix.office.com/watch/1liuojvdx6sie) az adatok Azure Storage Blobs és a Data Lake Store használatának ból a DistCp közötti másolása.</span><span class="sxs-lookup"><span data-stu-id="04ee3-118">[Watch this video](https://mix.office.com/watch/1liuojvdx6sie) on how to copy data between Azure Storage Blobs and Data Lake Store using DistCp.</span></span>

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a><span data-ttu-id="04ee3-119">HDInsight Linux fürtök ból a Distcp használata</span><span class="sxs-lookup"><span data-stu-id="04ee3-119">Use Distcp from an HDInsight Linux cluster</span></span>

<span data-ttu-id="04ee3-120">HDInsight-fürtök a ból a Distcp segédprogramot, amely segítségével különböző forrásokból származó adatok másolása HDInsight-fürtöt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="04ee3-120">An HDInsight cluster comes with the Distcp utility, which can be used to copy data from different sources into an HDInsight cluster.</span></span> <span data-ttu-id="04ee3-121">Ha konfigurálta a HDInsight-fürt használata a Data Lake Store további tárterületként, a ból a Distcp segédprogram lehet használt, a-kész adatok másolása, illetve onnan, valamint egy Data Lake Store-fiókot.</span><span class="sxs-lookup"><span data-stu-id="04ee3-121">If you have configured the HDInsight cluster to use Data Lake Store as an additional storage, the Distcp utility can be used out-of-the-box to copy data to and from a Data Lake Store account as well.</span></span> <span data-ttu-id="04ee3-122">Ebben a szakaszban úgy tekintünk a ból a Distcp segédprogrammal is.</span><span class="sxs-lookup"><span data-stu-id="04ee3-122">In this section we look at how to use the Distcp utility.</span></span>

1. <span data-ttu-id="04ee3-123">Az asztalról a SSH parancs használatával csatlakozzon a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="04ee3-123">From your desktop, use SSH to connect to the cluster.</span></span> <span data-ttu-id="04ee3-124">Lásd: [csatlakozás egy Linux-alapú HDInsight-fürt](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="04ee3-124">See [Connect to a Linux-based HDInsight cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> <span data-ttu-id="04ee3-125">Futtassa a parancsokat az SSH-kérés.</span><span class="sxs-lookup"><span data-stu-id="04ee3-125">Run the commands from the SSH prompt.</span></span>

2. <span data-ttu-id="04ee3-126">Győződjön meg arról, hogy az Azure Storage Blobs (WASB) végezheti el.</span><span class="sxs-lookup"><span data-stu-id="04ee3-126">Verify whether you can access the Azure Storage Blobs (WASB).</span></span> <span data-ttu-id="04ee3-127">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="04ee3-127">Run the following command:</span></span>

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    <span data-ttu-id="04ee3-128">Ez a tárolási blob tartalmát listáját kell biztosítania.</span><span class="sxs-lookup"><span data-stu-id="04ee3-128">This should provide a list of contents in the storage blob.</span></span>
3. <span data-ttu-id="04ee3-129">Ehhez hasonlóan ellenőrizze, hogy a Data Lake Store-fiók hozzáférhet a fürtből.</span><span class="sxs-lookup"><span data-stu-id="04ee3-129">Similarly, verify whether you can access the Data Lake Store account from the cluster.</span></span> <span data-ttu-id="04ee3-130">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="04ee3-130">Run the following command:</span></span>

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    <span data-ttu-id="04ee3-131">Ez biztosítania kell a fájlok és mappák a Data Lake Store-fiókban listáját.</span><span class="sxs-lookup"><span data-stu-id="04ee3-131">This should provide a list of files/folders in the Data Lake Store account.</span></span>
4. <span data-ttu-id="04ee3-132">Az ból a Distcp WASB a Data Lake Store-fiók adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="04ee3-132">Use Distcp to copy data from WASB to a Data Lake Store account.</span></span>

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    <span data-ttu-id="04ee3-133">Ezzel másolatot készít a tartalmát a **/példa/data/gutenberg/** a WASB mappájában **/myfolder** a Data Lake Store-fiókban.</span><span class="sxs-lookup"><span data-stu-id="04ee3-133">This will copy the contents of the **/example/data/gutenberg/** folder in WASB to **/myfolder** in the Data Lake Store account.</span></span>
5. <span data-ttu-id="04ee3-134">Ehhez hasonlóan ból a Distcp segítségével WASB Data Lake Store-fiók adatainak másolása.</span><span class="sxs-lookup"><span data-stu-id="04ee3-134">Similarly, use Distcp to copy data from Data Lake Store account to WASB.</span></span>

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    <span data-ttu-id="04ee3-135">Ez lesz a tartalom másolása **/myfolder** a Data Lake Store-fiók **/példa/data/gutenberg/** WASB mappájában.</span><span class="sxs-lookup"><span data-stu-id="04ee3-135">This will copy the contents of **/myfolder** in the Data Lake Store account to **/example/data/gutenberg/** folder in WASB.</span></span>

## <a name="performance-considerations-while-using-distcp"></a><span data-ttu-id="04ee3-136">Teljesítménnyel kapcsolatos szempontok ból a DistCp használata során</span><span class="sxs-lookup"><span data-stu-id="04ee3-136">Performance considerations while using DistCp</span></span>

<span data-ttu-id="04ee3-137">Mivel a legalacsonyabb granularitási ból a DistCp meg egyetlen fájl, a legfontosabb paraméter optimalizálása, szemben a Data Lake Store egyidejű példányok maximális száma.</span><span class="sxs-lookup"><span data-stu-id="04ee3-137">Because DistCp’s lowest granularity is a single file, setting the maximum number of simultaneous copies is the most important parameter to optimize it against Data Lake Store.</span></span> <span data-ttu-id="04ee3-138">Ez vezérli mappers számának beállítása (a perceké 'M ') paraméter a parancssorban.</span><span class="sxs-lookup"><span data-stu-id="04ee3-138">This is controlled by setting the number of mappers (‘m’) parameter on the command line.</span></span> <span data-ttu-id="04ee3-139">Ez a paraméter-adatok másolása használandó mappers maximális számát.</span><span class="sxs-lookup"><span data-stu-id="04ee3-139">This parameter specifies the maximum number of mappers that will be used to copy data.</span></span> <span data-ttu-id="04ee3-140">Alapértelmezett érték 20.</span><span class="sxs-lookup"><span data-stu-id="04ee3-140">Default value is 20.</span></span>

<span data-ttu-id="04ee3-141">**Példa**</span><span class="sxs-lookup"><span data-stu-id="04ee3-141">**Example**</span></span>

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a><span data-ttu-id="04ee3-142">Hogyan állapítható meg a használandó mappers száma?</span><span class="sxs-lookup"><span data-stu-id="04ee3-142">How do I determine the number of mappers to use?</span></span>

<span data-ttu-id="04ee3-143">Az alábbiakban olvashat némi útmutatást ezzel kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="04ee3-143">Here's some guidance that you can use.</span></span>

* <span data-ttu-id="04ee3-144">**1. lépés: A teljes YARN memória meghatározása** – az első lépés az, hogy határozza meg a YARN memória az a ból a DistCp feladat futtató fürtre.</span><span class="sxs-lookup"><span data-stu-id="04ee3-144">**Step 1: Determine total YARN memory** - The first step is to determine the YARN memory available to the cluster where you run the DistCp job.</span></span> <span data-ttu-id="04ee3-145">Ez az információ az Ambari portálon a fürthöz tartozó érhető el.</span><span class="sxs-lookup"><span data-stu-id="04ee3-145">This information is available in the Ambari portal associated with the cluster.</span></span> <span data-ttu-id="04ee3-146">A YARN és a Configs lapon tekintse meg a YARN memória.</span><span class="sxs-lookup"><span data-stu-id="04ee3-146">Navigate to YARN and view the Configs tab to see the YARN memory.</span></span> <span data-ttu-id="04ee3-147">Ahhoz, hogy a teljes YARN memória, szorozza meg a YARN csomópontonkénti memória a csomópontok számát a fürt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="04ee3-147">To get the total YARN memory, multiply the YARN memory per node with the number of nodes you have in your cluster.</span></span>

* <span data-ttu-id="04ee3-148">**2. lépés: Mappers számának kiszámítása** -értékének **m** megegyezik a YARN tároló mérete elosztva teljes YARN memória hányadosa.</span><span class="sxs-lookup"><span data-stu-id="04ee3-148">**Step 2: Calculate the number of mappers** - The value of **m** is equal to the quotient of total YARN memory divided by the YARN container size.</span></span> <span data-ttu-id="04ee3-149">A YARN tároló mérete információ áll rendelkezésre, valamint az Ambari portálon.</span><span class="sxs-lookup"><span data-stu-id="04ee3-149">The YARN container size information is available in the Ambari portal as well.</span></span> <span data-ttu-id="04ee3-150">A YARN és a Configs lapon.</span><span class="sxs-lookup"><span data-stu-id="04ee3-150">Navigate to YARN and view the Configs tab.</span></span> <span data-ttu-id="04ee3-151">A YARN tároló mérete ebben az ablakban jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="04ee3-151">The YARN container size is displayed in this window.</span></span> <span data-ttu-id="04ee3-152">A kiszolgálófarmban lévő mappers száma összefüggést (**m**) van</span><span class="sxs-lookup"><span data-stu-id="04ee3-152">The equation to arrive at the number of mappers (**m**) is</span></span>

        m = (number of nodes * YARN memory for each node) / YARN container size

<span data-ttu-id="04ee3-153">**Példa**</span><span class="sxs-lookup"><span data-stu-id="04ee3-153">**Example**</span></span>

<span data-ttu-id="04ee3-154">Tegyük fel, hogy a fürt rendelkezik egy 4 D14v2s csomópontok pedig 10TB-nyi adat átvitele 10 különböző mappák kívánt.</span><span class="sxs-lookup"><span data-stu-id="04ee3-154">Let’s assume that you have a 4 D14v2s nodes in the cluster and you are trying to transfer 10TB of data from 10 different folders.</span></span> <span data-ttu-id="04ee3-155">Mappánkénti változó mennyiségű adatot tartalmaznak, és az egyes mappákban lévő fájlméret eltérőek.</span><span class="sxs-lookup"><span data-stu-id="04ee3-155">Each of the folders contain varying amounts of data and the file sizes within each folder are different.</span></span>

* <span data-ttu-id="04ee3-156">Összes YARN memória - az Ambari portal annak meghatározását, hogy-e a YARN memória 96GB egy D14 csomópont.</span><span class="sxs-lookup"><span data-stu-id="04ee3-156">Total YARN memory - From the Ambari portal you determine that the YARN memory is 96GB for a D14 node.</span></span> <span data-ttu-id="04ee3-157">Igen a teljes YARN memória 4 csomópontot tartalmazó fürtben van:</span><span class="sxs-lookup"><span data-stu-id="04ee3-157">So, total YARN memory for 4 node cluster is:</span></span> 

        YARN memory = 4 * 96GB = 384GB

* <span data-ttu-id="04ee3-158">Ennyi mappers - az Ambari portal annak meghatározását, hogy a YARN tároló mérete 3072 D14 fürtcsomópont esetén.</span><span class="sxs-lookup"><span data-stu-id="04ee3-158">Number of mappers - From the Ambari portal you determine that the YARN container size is 3072 for a D14 cluster node.</span></span> <span data-ttu-id="04ee3-159">Igen van mappers száma:</span><span class="sxs-lookup"><span data-stu-id="04ee3-159">So, number of mappers is:</span></span>

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

<span data-ttu-id="04ee3-160">Ha más alkalmazásokat memóriát használ, akkor választhatja használandó csak egy részét a fürt YARN memória ból a DistCp.</span><span class="sxs-lookup"><span data-stu-id="04ee3-160">If other applications are using memory, then you can choose to only use a portion of your cluster’s YARN memory for DistCp.</span></span>

### <a name="copying-large-datasets"></a><span data-ttu-id="04ee3-161">Nagy adatkészletek másolása</span><span class="sxs-lookup"><span data-stu-id="04ee3-161">Copying large datasets</span></span>

<span data-ttu-id="04ee3-162">Amikor az adatkészlet lesz áthelyezve mérete túl nagy (pl. > 1 TB-os), vagy ha több különböző mappa, érdemes lehet használni több ból a DistCp feladat.</span><span class="sxs-lookup"><span data-stu-id="04ee3-162">When the size of the dataset to be moved is very large (e.g. >1TB) or if you have many different folders, you should consider using multiple DistCp jobs.</span></span> <span data-ttu-id="04ee3-163">Valószínűleg nincs nincs jobb teljesítménye, de azt fogja felülbírálásokkal a feladatokat, hogy a feladat sikertelen lesz, ha csak akkor indítsa újra a teljes feladat ahelyett, hogy adott feladat.</span><span class="sxs-lookup"><span data-stu-id="04ee3-163">There is likely no performance gain, but it will spread out the jobs so that if any job fails, you will only need to restart that specific job rather than the entire job.</span></span>

### <a name="limitations"></a><span data-ttu-id="04ee3-164">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="04ee3-164">Limitations</span></span>

* <span data-ttu-id="04ee3-165">Ból a DistCp megpróbál létrehozni mappers hasonló-nál, teljesítményének optimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="04ee3-165">DistCp tries to create mappers that are similar in size to optimize performance.</span></span> <span data-ttu-id="04ee3-166">Növelje meg a mappers nem mindig növelheti a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="04ee3-166">Increasing the number of mappers may not always increase performance.</span></span>

* <span data-ttu-id="04ee3-167">Ból a DistCp fájlonként csak egy leképező korlátozódik.</span><span class="sxs-lookup"><span data-stu-id="04ee3-167">DistCp is limited to only one mapper per file.</span></span> <span data-ttu-id="04ee3-168">Emiatt nem kell további mappers, mint a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="04ee3-168">Therefore, you should not have more mappers than you have files.</span></span> <span data-ttu-id="04ee3-169">Ból a DistCp hozzárendelhet egy leképező fájlba, mert ez megoldással csökkenthető a feldolgozási nagy fájlok másolása használható.</span><span class="sxs-lookup"><span data-stu-id="04ee3-169">Since DistCp can only assign one mapper to a file, this limits the amount of concurrency that can be used to copy large files.</span></span>

* <span data-ttu-id="04ee3-170">Ha kis számú nagyméretű fájlokat, majd meg kell bontva 256MB fájl adattömbök további lehetséges egyidejűségi Önnek.</span><span class="sxs-lookup"><span data-stu-id="04ee3-170">If you have a small number of large files, then you should split them into 256MB file chunks to give you more potential concurrency.</span></span> 
 
* <span data-ttu-id="04ee3-171">Az Azure Blob Storage-fiók másolása, a másolási feladat lehet, hogy szabályozva a blob storage oldalon.</span><span class="sxs-lookup"><span data-stu-id="04ee3-171">If you are copying from an Azure Blob Storage account, your copy job may be throttled on the blob storage side.</span></span> <span data-ttu-id="04ee3-172">Ez csökkenti a teljesítményt, a másolat.</span><span class="sxs-lookup"><span data-stu-id="04ee3-172">This will degrade the performance of your copy job.</span></span> <span data-ttu-id="04ee3-173">Az Azure Blob Storage korlátai által megszabott kapcsolatos további információkért lásd: Azure Storage-korlátok, [Azure-előfizetés és a szolgáltatásra vonatkozó korlátozások](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="04ee3-173">To learn more about the limits of Azure Blob Storage, see Azure Storage limits at [Azure subscription and service limits](../azure-subscription-service-limits.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="04ee3-174">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="04ee3-174">See also</span></span>
* [<span data-ttu-id="04ee3-175">Adatok másolása az Azure Storage Blobs Data Lake Store-bA</span><span class="sxs-lookup"><span data-stu-id="04ee3-175">Copy data from Azure Storage Blobs to Data Lake Store</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
* [<span data-ttu-id="04ee3-176">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="04ee3-176">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="04ee3-177">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="04ee3-177">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="04ee3-178">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="04ee3-178">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
