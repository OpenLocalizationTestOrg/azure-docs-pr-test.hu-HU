---
title: "Data Lake Store Hive teljesítményének hangolása irányelveit aaaAzure |} Microsoft Docs"
description: "Az Azure Data Lake Store Hive teljesítményének hangolása irányelvek"
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: e44daeb6ad3b64e893c709df63b56444a330729f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-store"></a><span data-ttu-id="8a052-103">Útmutatás a Hive HDInsight és az Azure Data Lake Store teljesítményhangolása</span><span class="sxs-lookup"><span data-stu-id="8a052-103">Performance tuning guidance for Hive on HDInsight and Azure Data Lake Store</span></span>

<span data-ttu-id="8a052-104">hello alapértelmezett beállítások állítani tooprovide a megfelelő teljesítmény számos különféle használati esetek között.</span><span class="sxs-lookup"><span data-stu-id="8a052-104">hello default settings have been set tooprovide good performance across many different use cases.</span></span>  <span data-ttu-id="8a052-105">I/o-igényes lekérdezések esetén a Hive bennünket tooget jobb teljesítmény ADLS lehet.</span><span class="sxs-lookup"><span data-stu-id="8a052-105">For I/O intensive queries, Hive can be tuned tooget better performance with ADLS.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="8a052-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8a052-106">Prerequisites</span></span>

* <span data-ttu-id="8a052-107">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="8a052-107">**An Azure subscription**.</span></span> <span data-ttu-id="8a052-108">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8a052-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="8a052-109">**Egy Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="8a052-109">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="8a052-110">Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="8a052-110">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="8a052-111">**Az Azure HDInsight-fürt** a Data Lake Store-fiók hozzáférési tooa.</span><span class="sxs-lookup"><span data-stu-id="8a052-111">**Azure HDInsight cluster** with access tooa Data Lake Store account.</span></span> <span data-ttu-id="8a052-112">Lásd: [HDInsight-fürtök létrehozása a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8a052-112">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="8a052-113">Ellenőrizze, hogy engedélyezte a távoli asztal hello fürt.</span><span class="sxs-lookup"><span data-stu-id="8a052-113">Make sure you enable Remote Desktop for hello cluster.</span></span>
* <span data-ttu-id="8a052-114">**HDInsight Hive futó**.</span><span class="sxs-lookup"><span data-stu-id="8a052-114">**Running Hive on HDInsight**.</span></span>  <span data-ttu-id="8a052-115">a HDInsight Hive-feladatok futtatásával kapcsolatos toolearn lásd: [használata a HDInsight Hive] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)</span><span class="sxs-lookup"><span data-stu-id="8a052-115">toolearn about running Hive jobs on HDInsight, see [Use Hive on HDInsight] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)</span></span>
* <span data-ttu-id="8a052-116">**Teljesítményhangolás ADLS iránymutatást**.</span><span class="sxs-lookup"><span data-stu-id="8a052-116">**Performance tuning guidelines on ADLS**.</span></span>  <span data-ttu-id="8a052-117">Általános teljesítmény fogalmakat, lásd: [Data Lake Store teljesítmény hangolása útmutató](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span><span class="sxs-lookup"><span data-stu-id="8a052-117">For general performance concepts, see [Data Lake Store Performance Tuning Guidance](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span></span>

## <a name="parameters"></a><span data-ttu-id="8a052-118">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="8a052-118">Parameters</span></span>

<span data-ttu-id="8a052-119">Az alábbiakban a legfontosabb beállítások tootune hello jobb ADLS-teljesítmény:</span><span class="sxs-lookup"><span data-stu-id="8a052-119">Here are hello most important settings tootune for improved ADLS performance:</span></span>

* <span data-ttu-id="8a052-120">**Hive.tez.Container.size** – hello egyes feladatok által használt memória mennyisége</span><span class="sxs-lookup"><span data-stu-id="8a052-120">**hive.tez.container.size** – hello amount of memory used by each tasks</span></span>

* <span data-ttu-id="8a052-121">**tez.Grouping.min méretű** – minimális méret minden leképezője</span><span class="sxs-lookup"><span data-stu-id="8a052-121">**tez.grouping.min-size** – minimum size of each mapper</span></span>

* <span data-ttu-id="8a052-122">**tez.Grouping.max méretű** – maximális méret minden leképezője</span><span class="sxs-lookup"><span data-stu-id="8a052-122">**tez.grouping.max-size** – maximum size of each mapper</span></span>

* <span data-ttu-id="8a052-123">**Hive.Exec.reducer.bytes.per.reducer** – minden nyomáscsökkentő méret</span><span class="sxs-lookup"><span data-stu-id="8a052-123">**hive.exec.reducer.bytes.per.reducer** – size of each reducer</span></span>

<span data-ttu-id="8a052-124">**Hive.tez.Container.size** -hello tároló mérete határozza meg, hogy mennyi memóriát minden feladat számára.</span><span class="sxs-lookup"><span data-stu-id="8a052-124">**hive.tez.container.size** - hello container size determines how much memory is available for each task.</span></span>  <span data-ttu-id="8a052-125">Ez a hello fő a megadott ellenőrző hello párhuzamossági struktúrában.</span><span class="sxs-lookup"><span data-stu-id="8a052-125">This is hello main input for controlling hello concurrency in Hive.</span></span>  

<span data-ttu-id="8a052-126">**tez.Grouping.min méretű** – Ez a paraméter lehetővé teszi a tooset hello minimális mérete minden leképező.</span><span class="sxs-lookup"><span data-stu-id="8a052-126">**tez.grouping.min-size** – This parameter allows you tooset hello minimum size of each mapper.</span></span>  <span data-ttu-id="8a052-127">Ha mappers, amely Tez kiválasztja hello száma kisebb, mint a paraméter hello értékét, majd Tez itt hello értéket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="8a052-127">If hello number of mappers that Tez chooses is smaller than hello value of this parameter, then Tez will use hello value set here.</span></span>  

<span data-ttu-id="8a052-128">**tez.Grouping.max méretű** – hello paraméter lehetővé teszi a tooset hello maximális méretét minden leképező.</span><span class="sxs-lookup"><span data-stu-id="8a052-128">**tez.grouping.max-size** – hello parameter allows you tooset hello maximum size of each mapper.</span></span>  <span data-ttu-id="8a052-129">Ha mappers Tez úgy dönt, hogy hello száma nagyobb, mint a paraméter hello értékét, majd Tez itt hello értéket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="8a052-129">If hello number of mappers that Tez chooses is larger than hello value of this parameter, then Tez will use hello value set here.</span></span>  

<span data-ttu-id="8a052-130">**Hive.Exec.reducer.bytes.per.reducer** – Ez a paraméter beállítja az egyes nyomáscsökkentő hello méretét.</span><span class="sxs-lookup"><span data-stu-id="8a052-130">**hive.exec.reducer.bytes.per.reducer** – This parameter sets hello size of each reducer.</span></span>  <span data-ttu-id="8a052-131">Alapértelmezés szerint minden nyomáscsökkentő mérete 256MB.</span><span class="sxs-lookup"><span data-stu-id="8a052-131">By default, each reducer is 256MB.</span></span>  

## <a name="guidance"></a><span data-ttu-id="8a052-132">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="8a052-132">Guidance</span></span>

<span data-ttu-id="8a052-133">**Állítsa be a hive.exec.reducer.bytes.per.reducer** – hello alapértelmezett érték tömörítetlen hello adatok esetén is működik.</span><span class="sxs-lookup"><span data-stu-id="8a052-133">**Set hive.exec.reducer.bytes.per.reducer** – hello default value works well when hello data is uncompressed.</span></span>  <span data-ttu-id="8a052-134">A tömörített adatok csökkentse hello nyomáscsökkentő hello méretét.</span><span class="sxs-lookup"><span data-stu-id="8a052-134">For data that is compressed, you should reduce hello size of hello reducer.</span></span>  

<span data-ttu-id="8a052-135">**Állítsa be a hive.tez.container.size** – minden csomóponton, a memória yarn.nodemanager.resource.memory MB-os által megadott és kell megfelelően beállítani a HDI-fürtnek alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="8a052-135">**Set hive.tez.container.size** – In each node, memory is specified by yarn.nodemanager.resource.memory-mb and should be correctly set on HDI cluster by default.</span></span>  <span data-ttu-id="8a052-136">A YARN hello megfelelő memória beállításával kapcsolatos további információkért tekintse meg a [utáni](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).</span><span class="sxs-lookup"><span data-stu-id="8a052-136">For additional information on setting hello appropriate memory in YARN, see this [post](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).</span></span>

<span data-ttu-id="8a052-137">I/o-igényes munkaterhelések is kihasználhatja a további párhuzamossági hello Tez tároló méretének csökkentésével.</span><span class="sxs-lookup"><span data-stu-id="8a052-137">I/O intensive workloads can benefit from more parallelism by decreasing hello Tez container size.</span></span> <span data-ttu-id="8a052-138">Hello felhasználói így növelve a feldolgozási további tárolókat.</span><span class="sxs-lookup"><span data-stu-id="8a052-138">This gives hello user more containers which increases concurrency.</span></span>  <span data-ttu-id="8a052-139">Bizonyos Hive-lekérdezések azonban jelentős mennyiségű memória (pl. MapJoin) szükséges.</span><span class="sxs-lookup"><span data-stu-id="8a052-139">However, some Hive queries require a significant amount of memory (e.g. MapJoin).</span></span>  <span data-ttu-id="8a052-140">Ha hello feladat nem rendelkezik elég memóriával, memória kivétel futásidőben kívüli fog kapni.</span><span class="sxs-lookup"><span data-stu-id="8a052-140">If hello task does not have enough memory, you will get an out of memory exception during runtime.</span></span>  <span data-ttu-id="8a052-141">Ha memória kivételek kívül, majd növelje hello memória.</span><span class="sxs-lookup"><span data-stu-id="8a052-141">If you receive out of memory exceptions, then you should increase hello memory.</span></span>   

<span data-ttu-id="8a052-142">futó feladatok vagy párhuzamossági egyidejű száma hello fog időpontjaihoz hello a YARN memória teljes mérete.</span><span class="sxs-lookup"><span data-stu-id="8a052-142">hello concurrent number of tasks running or parallelism will be bounded by hello total YARN memory.</span></span>  <span data-ttu-id="8a052-143">YARN a tárolók száma hello szabja meg, hogy hány egyidejű feladatok futtathatók.</span><span class="sxs-lookup"><span data-stu-id="8a052-143">hello number of YARN containers will dictate how many concurrent tasks can run.</span></span>  <span data-ttu-id="8a052-144">toofind hello YARN memória mennyisége, tooAmbari lépjen.</span><span class="sxs-lookup"><span data-stu-id="8a052-144">toofind hello YARN memory per node, you can go tooAmbari.</span></span>  <span data-ttu-id="8a052-145">Keresse meg a tooYARN és hello Configs lapon.  hello YARN memória ebben az ablakban jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8a052-145">Navigate tooYARN and view hello Configs tab.  hello YARN memory is displayed in this window.</span></span>  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
<span data-ttu-id="8a052-146">hello kulcs tooimproving teljesítményét ADLS tooincrease hello párhuzamossági lehetőség szerint.</span><span class="sxs-lookup"><span data-stu-id="8a052-146">hello key tooimproving performance using ADLS is tooincrease hello concurrency as much as possible.</span></span>  <span data-ttu-id="8a052-147">Tez automatikusan kiszámítja hello számát feladatokat, így nem kell tooset létre kell azt.</span><span class="sxs-lookup"><span data-stu-id="8a052-147">Tez automatically calculates hello number of tasks that should be created so you do not need tooset it.</span></span>   

## <a name="example-calculation"></a><span data-ttu-id="8a052-148">Példa kiszámítása</span><span class="sxs-lookup"><span data-stu-id="8a052-148">Example Calculation</span></span>

<span data-ttu-id="8a052-149">Tegyük fel, egy 8 csomópont D14 fürt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8a052-149">Let’s say you have an 8 node D14 cluster.</span></span>  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a><span data-ttu-id="8a052-150">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="8a052-150">Limitations</span></span>
<span data-ttu-id="8a052-151">**ADLS-szabályozás**</span><span class="sxs-lookup"><span data-stu-id="8a052-151">**ADLS throttling**</span></span> 

<span data-ttu-id="8a052-152">Hello kattint UIf korlátozza a sávszélesség megadott által ADLS, toosee feladat hibáihoz kezdenie.</span><span class="sxs-lookup"><span data-stu-id="8a052-152">UIf you hit hello limits of bandwidth provided by ADLS, you would start toosee task failures.</span></span> <span data-ttu-id="8a052-153">Ez azonosítható betartásával szabályozási hibák feladat naplókban által.</span><span class="sxs-lookup"><span data-stu-id="8a052-153">This could be identified by observing throttling errors in task logs.</span></span>  <span data-ttu-id="8a052-154">Hello párhuzamossági Tez tároló méretének növelésével csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="8a052-154">You can decrease hello parallelism by increasing Tez container size.</span></span>  <span data-ttu-id="8a052-155">Ha a feladat több egyidejű van szüksége, lépjen kapcsolatba velünk a következő címen.</span><span class="sxs-lookup"><span data-stu-id="8a052-155">If you need more concurrency for your job, please contact us.</span></span>   

<span data-ttu-id="8a052-156">Ha Ön első szabályozott toocheck, tooenable hello hibakeresési naplózás hello ügyféloldalon kell.</span><span class="sxs-lookup"><span data-stu-id="8a052-156">toocheck if you are getting throttled, you need tooenable hello debug logging on hello client side.</span></span> <span data-ttu-id="8a052-157">Ez hogyan azt teheti meg:</span><span class="sxs-lookup"><span data-stu-id="8a052-157">Here’s how you can do that:</span></span>

1. <span data-ttu-id="8a052-158">Helyezze el a következő tulajdonság hello log4j tulajdonságai a Hive-config hello. Ezt megteheti az Ambari nézetben: log4j.logger.com.microsoft.azure.datalake.store=DEBUG indítsa újra az összes hello csomópontok/szolgáltatást hello config tootake hatást.</span><span class="sxs-lookup"><span data-stu-id="8a052-158">Put hello following property in hello log4j properties in Hive config. This can be done from Ambari view: log4j.logger.com.microsoft.azure.datalake.store=DEBUG Restart all hello nodes/service for hello config tootake effect.</span></span>

2. <span data-ttu-id="8a052-159">Ha Ön első szabályozott, látni fogja, hello HTTP 429 hibakód hello hive naplófájlban.</span><span class="sxs-lookup"><span data-stu-id="8a052-159">If you are getting throttled, you’ll see hello HTTP 429 error code in hello hive log file.</span></span> <span data-ttu-id="8a052-160">hello hive naplófájl van /tmp/&lt;felhasználói&gt;/hive.log</span><span class="sxs-lookup"><span data-stu-id="8a052-160">hello hive log file is in /tmp/&lt;user&gt;/hive.log</span></span>

## <a name="further-information-on-hive-tuning"></a><span data-ttu-id="8a052-161">További információ a Hive hangolása</span><span class="sxs-lookup"><span data-stu-id="8a052-161">Further information on Hive tuning</span></span>

<span data-ttu-id="8a052-162">Az alábbiakban néhány rendszerek, amelyek segítségével finomhangolhatják a Hive-lekérdezéseket:</span><span class="sxs-lookup"><span data-stu-id="8a052-162">Here are a few blogs that will help tune your Hive queries:</span></span>
* [<span data-ttu-id="8a052-163">A hdinsight Hadoop Hive-lekérdezések optimalizálása</span><span class="sxs-lookup"><span data-stu-id="8a052-163">Optimize Hive queries for Hadoop in HDInsight</span></span>](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [<span data-ttu-id="8a052-164">Hibaelhárítás a Hive-lekérdezések teljesítményét</span><span class="sxs-lookup"><span data-stu-id="8a052-164">Troubleshooting Hive query performance</span></span>](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [<span data-ttu-id="8a052-165">Az ignite előadás a HDInsight Hive a optimalizálása</span><span class="sxs-lookup"><span data-stu-id="8a052-165">Ignite talk on optimize Hive on HDInsight</span></span>](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
