---
title: "aaaCopy adatok tooand a ból a Distcp használatával a Data Lake Store WASB |} Microsoft Docs"
description: "Ból a Distcp eszköz toocopy adatok tooand használja az Azure Storage Blobs tooData Lake Store-ból"
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
ms.openlocfilehash: ec23410bbab0f82449a475412bc3b097c4a8fccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-distcp-toocopy-data-between-azure-storage-blobs-and-data-lake-store"></a><span data-ttu-id="bed2b-103">Azure Storage Blobs és a Data Lake Store ból a Distcp toocopy adatok használata</span><span class="sxs-lookup"><span data-stu-id="bed2b-103">Use Distcp toocopy data between Azure Storage Blobs and Data Lake Store</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bed2b-104">A DistCp használata</span><span class="sxs-lookup"><span data-stu-id="bed2b-104">Using DistCp</span></span>](data-lake-store-copy-data-wasb-distcp.md)
> * [<span data-ttu-id="bed2b-105">Az AdlCopy használata</span><span class="sxs-lookup"><span data-stu-id="bed2b-105">Using AdlCopy</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
>
>

<span data-ttu-id="bed2b-106">Miután létrehozta a HDInsight-fürtöt, amely rendelkezik hozzáférési tooa Data Lake Store-fiók, például ból a Distcp toocopy adatokat Hadoop-ökoszisztémával eszközök használhatja **tooand a** egy HDInsight fürt storage (WASB) a Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="bed2b-106">Once you have created an HDInsight cluster that has access tooa Data Lake Store account, you can use Hadoop ecosystem tools like Distcp toocopy data **tooand from** an HDInsight cluster storage (WASB) into a Data Lake Store account.</span></span> <span data-ttu-id="bed2b-107">Ez a cikk útmutatás tooachieve ez.</span><span class="sxs-lookup"><span data-stu-id="bed2b-107">This article provides instructions on how tooachieve this.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bed2b-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bed2b-108">Prerequisites</span></span>
<span data-ttu-id="bed2b-109">Ez a cikk elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="bed2b-109">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="bed2b-110">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="bed2b-110">**An Azure subscription**.</span></span> <span data-ttu-id="bed2b-111">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bed2b-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="bed2b-112">**Egy Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="bed2b-112">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="bed2b-113">Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="bed2b-113">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="bed2b-114">**Az Azure HDInsight-fürt** a Data Lake Store-fiók hozzáférési tooa.</span><span class="sxs-lookup"><span data-stu-id="bed2b-114">**Azure HDInsight cluster** with access tooa Data Lake Store account.</span></span> <span data-ttu-id="bed2b-115">Lásd: [HDInsight-fürtök létrehozása a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bed2b-115">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="bed2b-116">Ellenőrizze, hogy engedélyezte a távoli asztal hello fürt.</span><span class="sxs-lookup"><span data-stu-id="bed2b-116">Make sure you enable Remote Desktop for hello cluster.</span></span>

## <a name="do-you-learn-fast-with-videos"></a><span data-ttu-id="bed2b-117">Gyorsan tanul videók segítségével?</span><span class="sxs-lookup"><span data-stu-id="bed2b-117">Do you learn fast with videos?</span></span>
<span data-ttu-id="bed2b-118">[A videó](https://mix.office.com/watch/1liuojvdx6sie) hogyan toocopy adatok Azure Storage Blobs és a Data Lake Store használatának ból a DistCp között.</span><span class="sxs-lookup"><span data-stu-id="bed2b-118">[Watch this video](https://mix.office.com/watch/1liuojvdx6sie) on how toocopy data between Azure Storage Blobs and Data Lake Store using DistCp.</span></span>

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a><span data-ttu-id="bed2b-119">HDInsight Linux fürtök ból a Distcp használata</span><span class="sxs-lookup"><span data-stu-id="bed2b-119">Use Distcp from an HDInsight Linux cluster</span></span>

<span data-ttu-id="bed2b-120">HDInsight-fürtök hello ból a Distcp segédprogram, amely lehet használt toocopy adatokat a HDInsight-fürtök különböző forrásokból származnak.</span><span class="sxs-lookup"><span data-stu-id="bed2b-120">An HDInsight cluster comes with hello Distcp utility, which can be used toocopy data from different sources into an HDInsight cluster.</span></span> <span data-ttu-id="bed2b-121">Ha konfigurálta a hello HDInsight fürt toouse Data Lake Store további tárterületként, hello ból a Distcp segédprogram lehet használt out-of-az-box toocopy adatok tooand, valamint egy Data Lake Store-fiókból.</span><span class="sxs-lookup"><span data-stu-id="bed2b-121">If you have configured hello HDInsight cluster toouse Data Lake Store as an additional storage, hello Distcp utility can be used out-of-the-box toocopy data tooand from a Data Lake Store account as well.</span></span> <span data-ttu-id="bed2b-122">Ebben a szakaszban úgy tekintünk, hogyan toouse hello ból a Distcp segédprogram.</span><span class="sxs-lookup"><span data-stu-id="bed2b-122">In this section we look at how toouse hello Distcp utility.</span></span>

1. <span data-ttu-id="bed2b-123">Az asztalról SSH tooconnect toohello-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="bed2b-123">From your desktop, use SSH tooconnect toohello cluster.</span></span> <span data-ttu-id="bed2b-124">Lásd: [Connect tooa Linux-alapú HDInsight-fürt](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="bed2b-124">See [Connect tooa Linux-based HDInsight cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> <span data-ttu-id="bed2b-125">Hello parancsokat hello SSH parancssorból.</span><span class="sxs-lookup"><span data-stu-id="bed2b-125">Run hello commands from hello SSH prompt.</span></span>

2. <span data-ttu-id="bed2b-126">Győződjön meg arról, hogy van-e hozzáférési hello Azure Storage Blobs (WASB).</span><span class="sxs-lookup"><span data-stu-id="bed2b-126">Verify whether you can access hello Azure Storage Blobs (WASB).</span></span> <span data-ttu-id="bed2b-127">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="bed2b-127">Run hello following command:</span></span>

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    <span data-ttu-id="bed2b-128">Így kell hello tárolási blob tartalmának listáját.</span><span class="sxs-lookup"><span data-stu-id="bed2b-128">This should provide a list of contents in hello storage blob.</span></span>
3. <span data-ttu-id="bed2b-129">Ehhez hasonlóan ellenőrizze, hogy van-e hozzáférési hello Data Lake Store-fiók hello fürtből.</span><span class="sxs-lookup"><span data-stu-id="bed2b-129">Similarly, verify whether you can access hello Data Lake Store account from hello cluster.</span></span> <span data-ttu-id="bed2b-130">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="bed2b-130">Run hello following command:</span></span>

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    <span data-ttu-id="bed2b-131">Ez biztosítania kell a fájlok és mappák a Data Lake Store-fiók hello listáját.</span><span class="sxs-lookup"><span data-stu-id="bed2b-131">This should provide a list of files/folders in hello Data Lake Store account.</span></span>
4. <span data-ttu-id="bed2b-132">Data Lake Store-fiók WASB tooa ból a Distcp toocopy adatait használják.</span><span class="sxs-lookup"><span data-stu-id="bed2b-132">Use Distcp toocopy data from WASB tooa Data Lake Store account.</span></span>

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    <span data-ttu-id="bed2b-133">Ezzel másolatot készít a hello hello tartalmát **/példa/data/gutenberg/** WASB mappájában túl**/myfolder** a hello Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="bed2b-133">This will copy hello contents of hello **/example/data/gutenberg/** folder in WASB too**/myfolder** in hello Data Lake Store account.</span></span>
5. <span data-ttu-id="bed2b-134">Hasonlóképpen használja a Data Lake Store-fiók tooWASB ból a Distcp toocopy adatait.</span><span class="sxs-lookup"><span data-stu-id="bed2b-134">Similarly, use Distcp toocopy data from Data Lake Store account tooWASB.</span></span>

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    <span data-ttu-id="bed2b-135">Ezzel másolatot készít a hello tartalmát **/myfolder** hello Data Lake Store a fiókja túl**/példa/data/gutenberg/** WASB mappájában.</span><span class="sxs-lookup"><span data-stu-id="bed2b-135">This will copy hello contents of **/myfolder** in hello Data Lake Store account too**/example/data/gutenberg/** folder in WASB.</span></span>

## <a name="performance-considerations-while-using-distcp"></a><span data-ttu-id="bed2b-136">Teljesítménnyel kapcsolatos szempontok ból a DistCp használata során</span><span class="sxs-lookup"><span data-stu-id="bed2b-136">Performance considerations while using DistCp</span></span>

<span data-ttu-id="bed2b-137">Mivel ból a DistCp tartozó legalacsonyabb granularitási egyetlen fájl, a beállítás hello a példányok maximális száma párhuzamos hello legfontosabb paraméter toooptimize, Data Lake Store ellen.</span><span class="sxs-lookup"><span data-stu-id="bed2b-137">Because DistCp’s lowest granularity is a single file, setting hello maximum number of simultaneous copies is hello most important parameter toooptimize it against Data Lake Store.</span></span> <span data-ttu-id="bed2b-138">Ez úgy, hogy mappers hello számát szabályozza (a perceké 'M ') hello paraméter.</span><span class="sxs-lookup"><span data-stu-id="bed2b-138">This is controlled by setting hello number of mappers (‘m’) parameter on hello command line.</span></span> <span data-ttu-id="bed2b-139">Ez a paraméter, amely lesz használt toocopy adatok mappers hello maximális számát.</span><span class="sxs-lookup"><span data-stu-id="bed2b-139">This parameter specifies hello maximum number of mappers that will be used toocopy data.</span></span> <span data-ttu-id="bed2b-140">Alapértelmezett érték 20.</span><span class="sxs-lookup"><span data-stu-id="bed2b-140">Default value is 20.</span></span>

<span data-ttu-id="bed2b-141">**Példa**</span><span class="sxs-lookup"><span data-stu-id="bed2b-141">**Example**</span></span>

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder -m 100

### <a name="how-do-i-determine-hello-number-of-mappers-toouse"></a><span data-ttu-id="bed2b-142">Hogyan állapítható meg mappers toouse hello száma?</span><span class="sxs-lookup"><span data-stu-id="bed2b-142">How do I determine hello number of mappers toouse?</span></span>

<span data-ttu-id="bed2b-143">Az alábbiakban olvashat némi útmutatást ezzel kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="bed2b-143">Here's some guidance that you can use.</span></span>

* <span data-ttu-id="bed2b-144">**1. lépés: A teljes YARN memória meghatározása** -hello első lépéseként toodetermine hello YARN memória rendelkezésre álló toohello fürt hello-ból a DistCp feladat futtató.</span><span class="sxs-lookup"><span data-stu-id="bed2b-144">**Step 1: Determine total YARN memory** - hello first step is toodetermine hello YARN memory available toohello cluster where you run hello DistCp job.</span></span> <span data-ttu-id="bed2b-145">Ez az információ hello Ambari portálon hello-fürthöz tartozó érhető el.</span><span class="sxs-lookup"><span data-stu-id="bed2b-145">This information is available in hello Ambari portal associated with hello cluster.</span></span> <span data-ttu-id="bed2b-146">Keresse meg a tooYARN és hello Configs lapon toosee hello YARN memória megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="bed2b-146">Navigate tooYARN and view hello Configs tab toosee hello YARN memory.</span></span> <span data-ttu-id="bed2b-147">tooget hello teljes YARN memória, többszörösének hello YARN memória mennyisége a csomópontok száma hello rendelkezik a fürtön.</span><span class="sxs-lookup"><span data-stu-id="bed2b-147">tooget hello total YARN memory, multiply hello YARN memory per node with hello number of nodes you have in your cluster.</span></span>

* <span data-ttu-id="bed2b-148">**2. lépés: Kiszámítása hello mappers** -értékének hello **m** egyenlő toohello hányadosa hello YARN tároló mérete elosztva teljes YARN memória van.</span><span class="sxs-lookup"><span data-stu-id="bed2b-148">**Step 2: Calculate hello number of mappers** - hello value of **m** is equal toohello quotient of total YARN memory divided by hello YARN container size.</span></span> <span data-ttu-id="bed2b-149">hello YARN tároló információi hello Ambari portálon is érhető el.</span><span class="sxs-lookup"><span data-stu-id="bed2b-149">hello YARN container size information is available in hello Ambari portal as well.</span></span> <span data-ttu-id="bed2b-150">Keresse meg a tooYARN és nézet hello konfigurációk esetén külön-külön hello YARN tároló mérete ebben az ablakban jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bed2b-150">Navigate tooYARN and view hello Configs tab. hello YARN container size is displayed in this window.</span></span> <span data-ttu-id="bed2b-151">hello egyenlet tooarrive mappers hello számú (**m**) van</span><span class="sxs-lookup"><span data-stu-id="bed2b-151">hello equation tooarrive at hello number of mappers (**m**) is</span></span>

        m = (number of nodes * YARN memory for each node) / YARN container size

<span data-ttu-id="bed2b-152">**Példa**</span><span class="sxs-lookup"><span data-stu-id="bed2b-152">**Example**</span></span>

<span data-ttu-id="bed2b-153">Tegyük fel, hogy egy 4 D14v2s csomópontok hello fürt rendelkezik, és 10 TB-os tootransfer próbált adatok 10 mappákból.</span><span class="sxs-lookup"><span data-stu-id="bed2b-153">Let’s assume that you have a 4 D14v2s nodes in hello cluster and you are trying tootransfer 10TB of data from 10 different folders.</span></span> <span data-ttu-id="bed2b-154">Hello Mappánkénti változó mennyiségű adatot tartalmaz, és hello fájlméret egyes mappákban lévő eltérőek.</span><span class="sxs-lookup"><span data-stu-id="bed2b-154">Each of hello folders contain varying amounts of data and hello file sizes within each folder are different.</span></span>

* <span data-ttu-id="bed2b-155">Teljes YARN memória - hello határozhatja meg, hogy hello YARN memória Ambari portal 96GB D14 csomópont.</span><span class="sxs-lookup"><span data-stu-id="bed2b-155">Total YARN memory - From hello Ambari portal you determine that hello YARN memory is 96GB for a D14 node.</span></span> <span data-ttu-id="bed2b-156">Igen a teljes YARN memória 4 csomópontot tartalmazó fürtben van:</span><span class="sxs-lookup"><span data-stu-id="bed2b-156">So, total YARN memory for 4 node cluster is:</span></span> 

        YARN memory = 4 * 96GB = 384GB

* <span data-ttu-id="bed2b-157">Mappers - hello Ambari portal annak meghatározását, hogy hello YARN tároló mérete 3072 D14 fürtcsomópont esetén a száma.</span><span class="sxs-lookup"><span data-stu-id="bed2b-157">Number of mappers - From hello Ambari portal you determine that hello YARN container size is 3072 for a D14 cluster node.</span></span> <span data-ttu-id="bed2b-158">Igen van mappers száma:</span><span class="sxs-lookup"><span data-stu-id="bed2b-158">So, number of mappers is:</span></span>

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

<span data-ttu-id="bed2b-159">Ha más alkalmazásokat memóriát használ, akkor választhatja tooonly használja a fürt YARN memória azon része a ból a DistCp.</span><span class="sxs-lookup"><span data-stu-id="bed2b-159">If other applications are using memory, then you can choose tooonly use a portion of your cluster’s YARN memory for DistCp.</span></span>

### <a name="copying-large-datasets"></a><span data-ttu-id="bed2b-160">Nagy adatkészletek másolása</span><span class="sxs-lookup"><span data-stu-id="bed2b-160">Copying large datasets</span></span>

<span data-ttu-id="bed2b-161">Nagyon nagy hello dataset toobe hello mérete áthelyezésekor (pl. > 1 TB-os), vagy ha több különböző mappa, érdemes lehet használni több ból a DistCp feladat.</span><span class="sxs-lookup"><span data-stu-id="bed2b-161">When hello size of hello dataset toobe moved is very large (e.g. >1TB) or if you have many different folders, you should consider using multiple DistCp jobs.</span></span> <span data-ttu-id="bed2b-162">Valószínűleg nincs nincs jobb teljesítménye, de azt fogja felülbírálásokkal hello feladatok, hogy a feladat sikertelen lesz, ha csak akkor toorestart hello teljes feladatot ahelyett, hogy adott feladat.</span><span class="sxs-lookup"><span data-stu-id="bed2b-162">There is likely no performance gain, but it will spread out hello jobs so that if any job fails, you will only need toorestart that specific job rather than hello entire job.</span></span>

### <a name="limitations"></a><span data-ttu-id="bed2b-163">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="bed2b-163">Limitations</span></span>

* <span data-ttu-id="bed2b-164">Ból a DistCp megpróbál toocreate mappers hasonló mérete toooptimize teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="bed2b-164">DistCp tries toocreate mappers that are similar in size toooptimize performance.</span></span> <span data-ttu-id="bed2b-165">Mappers hello számának növelése nem mindig növelheti a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="bed2b-165">Increasing hello number of mappers may not always increase performance.</span></span>

* <span data-ttu-id="bed2b-166">Ból a DistCp korlátozott tooonly egy leképező fájlonként.</span><span class="sxs-lookup"><span data-stu-id="bed2b-166">DistCp is limited tooonly one mapper per file.</span></span> <span data-ttu-id="bed2b-167">Emiatt nem kell további mappers, mint a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="bed2b-167">Therefore, you should not have more mappers than you have files.</span></span> <span data-ttu-id="bed2b-168">Ból a DistCp hozzárendelhet egy leképező tooa fájlt, mivel ez használható használt toocopy nagy fájlok feldolgozási hello mennyisége korlátozza.</span><span class="sxs-lookup"><span data-stu-id="bed2b-168">Since DistCp can only assign one mapper tooa file, this limits hello amount of concurrency that can be used toocopy large files.</span></span>

* <span data-ttu-id="bed2b-169">Ha kis számú nagy fájlok, akkor kell 256MB fájl adattömbök toogive ossza meg több lehetséges egyidejűségi.</span><span class="sxs-lookup"><span data-stu-id="bed2b-169">If you have a small number of large files, then you should split them into 256MB file chunks toogive you more potential concurrency.</span></span> 
 
* <span data-ttu-id="bed2b-170">Az Azure Blob Storage-fiók másolása, a másolási feladat lehet, hogy szabályozva hello blob storage ügyféloldali.</span><span class="sxs-lookup"><span data-stu-id="bed2b-170">If you are copying from an Azure Blob Storage account, your copy job may be throttled on hello blob storage side.</span></span> <span data-ttu-id="bed2b-171">Ez csökkenti a másolási feladat hello teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="bed2b-171">This will degrade hello performance of your copy job.</span></span> <span data-ttu-id="bed2b-172">További információ az Azure Blob Storage hello korlátai által megszabott toolearn tekintse meg az Azure Storage-korlátok, [Azure-előfizetés és a szolgáltatásra vonatkozó korlátozások](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="bed2b-172">toolearn more about hello limits of Azure Blob Storage, see Azure Storage limits at [Azure subscription and service limits](../azure-subscription-service-limits.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="bed2b-173">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="bed2b-173">See also</span></span>
* [<span data-ttu-id="bed2b-174">Adatok másolása az Azure Storage Blobs tooData Lake Store-ból</span><span class="sxs-lookup"><span data-stu-id="bed2b-174">Copy data from Azure Storage Blobs tooData Lake Store</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
* [<span data-ttu-id="bed2b-175">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="bed2b-175">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="bed2b-176">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="bed2b-176">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="bed2b-177">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="bed2b-177">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
