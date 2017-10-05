---
title: "Az Azure Data Lake Store Hive teljesítményének hangolása irányelvek |} Microsoft Docs"
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
ms.openlocfilehash: e10bf8f7cbae2b81d22823ff74fe652c6bcb2da3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-store"></a><span data-ttu-id="4ec63-103">Útmutatás a Hive HDInsight és az Azure Data Lake Store teljesítményhangolása</span><span class="sxs-lookup"><span data-stu-id="4ec63-103">Performance tuning guidance for Hive on HDInsight and Azure Data Lake Store</span></span>

<span data-ttu-id="4ec63-104">Az alapértelmezett beállításokat állították be, hogy a megfelelő teljesítmény számos különféle használati esetek biztosít.</span><span class="sxs-lookup"><span data-stu-id="4ec63-104">The default settings have been set to provide good performance across many different use cases.</span></span>  <span data-ttu-id="4ec63-105">I/o-igényes lekérdezések esetén a rendszer a Hive szabályozható ADLS és nagyobb teljesítményre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="4ec63-105">For I/O intensive queries, Hive can be tuned to get better performance with ADLS.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="4ec63-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4ec63-106">Prerequisites</span></span>

* <span data-ttu-id="4ec63-107">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="4ec63-107">**An Azure subscription**.</span></span> <span data-ttu-id="4ec63-108">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4ec63-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="4ec63-109">**Egy Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="4ec63-109">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="4ec63-110">Hogyan hozhat létre ilyet, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="4ec63-110">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="4ec63-111">**Az Azure HDInsight-fürt** a Data Lake Store-fiók eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="4ec63-111">**Azure HDInsight cluster** with access to a Data Lake Store account.</span></span> <span data-ttu-id="4ec63-112">Lásd: [HDInsight-fürtök létrehozása a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4ec63-112">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="4ec63-113">Győződjön meg arról, hogy a fürt számára engedélyezi a távoli asztal.</span><span class="sxs-lookup"><span data-stu-id="4ec63-113">Make sure you enable Remote Desktop for the cluster.</span></span>
* <span data-ttu-id="4ec63-114">**HDInsight Hive futó**.</span><span class="sxs-lookup"><span data-stu-id="4ec63-114">**Running Hive on HDInsight**.</span></span>  <span data-ttu-id="4ec63-115">A HDInsight Hive-feladatok futtatásával kapcsolatos további tudnivalókért lásd: a [használata a HDInsight Hive] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)</span><span class="sxs-lookup"><span data-stu-id="4ec63-115">To learn about running Hive jobs on HDInsight, see [Use Hive on HDInsight] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)</span></span>
* <span data-ttu-id="4ec63-116">**Teljesítményhangolás ADLS iránymutatást**.</span><span class="sxs-lookup"><span data-stu-id="4ec63-116">**Performance tuning guidelines on ADLS**.</span></span>  <span data-ttu-id="4ec63-117">Általános teljesítmény fogalmakat, lásd: [Data Lake Store teljesítmény hangolása útmutató](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span><span class="sxs-lookup"><span data-stu-id="4ec63-117">For general performance concepts, see [Data Lake Store Performance Tuning Guidance](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span></span>

## <a name="parameters"></a><span data-ttu-id="4ec63-118">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="4ec63-118">Parameters</span></span>

<span data-ttu-id="4ec63-119">Az alábbiakban a továbbfejlesztett ADLS-teljesítmény hangolására legfontosabb beállítások:</span><span class="sxs-lookup"><span data-stu-id="4ec63-119">Here are the most important settings to tune for improved ADLS performance:</span></span>

* <span data-ttu-id="4ec63-120">**Hive.tez.Container.size** – az egyes feladatok által használt memória mennyisége</span><span class="sxs-lookup"><span data-stu-id="4ec63-120">**hive.tez.container.size** – the amount of memory used by each tasks</span></span>

* <span data-ttu-id="4ec63-121">**tez.Grouping.min méretű** – minimális méret minden leképezője</span><span class="sxs-lookup"><span data-stu-id="4ec63-121">**tez.grouping.min-size** – minimum size of each mapper</span></span>

* <span data-ttu-id="4ec63-122">**tez.Grouping.max méretű** – maximális méret minden leképezője</span><span class="sxs-lookup"><span data-stu-id="4ec63-122">**tez.grouping.max-size** – maximum size of each mapper</span></span>

* <span data-ttu-id="4ec63-123">**Hive.Exec.reducer.bytes.per.reducer** – minden nyomáscsökkentő méret</span><span class="sxs-lookup"><span data-stu-id="4ec63-123">**hive.exec.reducer.bytes.per.reducer** – size of each reducer</span></span>

<span data-ttu-id="4ec63-124">**Hive.tez.Container.size** -tároló mérete határozza meg, hogy mennyi memória érhető el minden feladat esetében.</span><span class="sxs-lookup"><span data-stu-id="4ec63-124">**hive.tez.container.size** - The container size determines how much memory is available for each task.</span></span>  <span data-ttu-id="4ec63-125">Ez az a fő bemeneti a Hive párhuzamossági vezérlése.</span><span class="sxs-lookup"><span data-stu-id="4ec63-125">This is the main input for controlling the concurrency in Hive.</span></span>  

<span data-ttu-id="4ec63-126">**tez.Grouping.min méretű** – Ez a paraméter lehetővé teszi, hogy meg kell adnia minden leképező legkisebb méretét.</span><span class="sxs-lookup"><span data-stu-id="4ec63-126">**tez.grouping.min-size** – This parameter allows you to set the minimum size of each mapper.</span></span>  <span data-ttu-id="4ec63-127">Ha, amely Tez kiválasztja mappers száma kisebb, mint ez a paraméter értékét, majd Tez fogja használni az itt beállított érték.</span><span class="sxs-lookup"><span data-stu-id="4ec63-127">If the number of mappers that Tez chooses is smaller than the value of this parameter, then Tez will use the value set here.</span></span>  

<span data-ttu-id="4ec63-128">**tez.Grouping.max méretű** – a paraméter lehetővé teszi, hogy meg kell adnia minden leképező maximális méretét.</span><span class="sxs-lookup"><span data-stu-id="4ec63-128">**tez.grouping.max-size** – The parameter allows you to set the maximum size of each mapper.</span></span>  <span data-ttu-id="4ec63-129">Ha, amely Tez kiválasztja mappers száma nagyobb, mint ez a paraméter értékét, majd Tez fogja használni az itt beállított érték.</span><span class="sxs-lookup"><span data-stu-id="4ec63-129">If the number of mappers that Tez chooses is larger than the value of this parameter, then Tez will use the value set here.</span></span>  

<span data-ttu-id="4ec63-130">**Hive.Exec.reducer.bytes.per.reducer** – Ez a paraméter minden nyomáscsökkentő méretét állítja be.</span><span class="sxs-lookup"><span data-stu-id="4ec63-130">**hive.exec.reducer.bytes.per.reducer** – This parameter sets the size of each reducer.</span></span>  <span data-ttu-id="4ec63-131">Alapértelmezés szerint minden nyomáscsökkentő mérete 256MB.</span><span class="sxs-lookup"><span data-stu-id="4ec63-131">By default, each reducer is 256MB.</span></span>  

## <a name="guidance"></a><span data-ttu-id="4ec63-132">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="4ec63-132">Guidance</span></span>

<span data-ttu-id="4ec63-133">**Állítsa be a hive.exec.reducer.bytes.per.reducer** – az alapértelmezett érték az adatok tömörítetlen esetén is működik.</span><span class="sxs-lookup"><span data-stu-id="4ec63-133">**Set hive.exec.reducer.bytes.per.reducer** – The default value works well when the data is uncompressed.</span></span>  <span data-ttu-id="4ec63-134">A tömörített adatok csökkentse a nyomáscsökkentő méretét.</span><span class="sxs-lookup"><span data-stu-id="4ec63-134">For data that is compressed, you should reduce the size of the reducer.</span></span>  

<span data-ttu-id="4ec63-135">**Állítsa be a hive.tez.container.size** – minden csomóponton, a memória yarn.nodemanager.resource.memory MB-os által megadott és kell megfelelően beállítani a HDI-fürtnek alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="4ec63-135">**Set hive.tez.container.size** – In each node, memory is specified by yarn.nodemanager.resource.memory-mb and should be correctly set on HDI cluster by default.</span></span>  <span data-ttu-id="4ec63-136">A YARN a megfelelő memória beállításával kapcsolatos további információkért tekintse meg a [utáni](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).</span><span class="sxs-lookup"><span data-stu-id="4ec63-136">For additional information on setting the appropriate memory in YARN, see this [post](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).</span></span>

<span data-ttu-id="4ec63-137">I/o-igényes munkaterhelések is kihasználhatja a további párhuzamossági Tez tároló méretének csökkentésével.</span><span class="sxs-lookup"><span data-stu-id="4ec63-137">I/O intensive workloads can benefit from more parallelism by decreasing the Tez container size.</span></span> <span data-ttu-id="4ec63-138">Így a felhasználó további tárolókat, növelve a feldolgozási.</span><span class="sxs-lookup"><span data-stu-id="4ec63-138">This gives the user more containers which increases concurrency.</span></span>  <span data-ttu-id="4ec63-139">Bizonyos Hive-lekérdezések azonban jelentős mennyiségű memória (pl. MapJoin) szükséges.</span><span class="sxs-lookup"><span data-stu-id="4ec63-139">However, some Hive queries require a significant amount of memory (e.g. MapJoin).</span></span>  <span data-ttu-id="4ec63-140">Ha a feladat nem rendelkezik elég memóriával, memória kivétel futásidőben kívüli fog kapni.</span><span class="sxs-lookup"><span data-stu-id="4ec63-140">If the task does not have enough memory, you will get an out of memory exception during runtime.</span></span>  <span data-ttu-id="4ec63-141">Ha memória kivételek kívül, majd növelje a memória.</span><span class="sxs-lookup"><span data-stu-id="4ec63-141">If you receive out of memory exceptions, then you should increase the memory.</span></span>   

<span data-ttu-id="4ec63-142">A futó feladatok vagy párhuzamossági egyidejű száma a teljes YARN memória fog időpontjaihoz.</span><span class="sxs-lookup"><span data-stu-id="4ec63-142">The concurrent number of tasks running or parallelism will be bounded by the total YARN memory.</span></span>  <span data-ttu-id="4ec63-143">A YARN a tárolók száma szabja meg, hogy hány egyidejű feladatok futtathatók.</span><span class="sxs-lookup"><span data-stu-id="4ec63-143">The number of YARN containers will dictate how many concurrent tasks can run.</span></span>  <span data-ttu-id="4ec63-144">A YARN memória mennyisége megkereséséhez nyissa meg az Ambari.</span><span class="sxs-lookup"><span data-stu-id="4ec63-144">To find the YARN memory per node, you can go to Ambari.</span></span>  <span data-ttu-id="4ec63-145">A YARN és a Configs lapon.</span><span class="sxs-lookup"><span data-stu-id="4ec63-145">Navigate to YARN and view the Configs tab.</span></span>  <span data-ttu-id="4ec63-146">A YARN memória ebben az ablakban jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4ec63-146">The YARN memory is displayed in this window.</span></span>  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
<span data-ttu-id="4ec63-147">A fontos ADLS teljesítményének javítása, hogy a lehető legrövidebb egyidejűségi növeléséhez.</span><span class="sxs-lookup"><span data-stu-id="4ec63-147">The key to improving performance using ADLS is to increase the concurrency as much as possible.</span></span>  <span data-ttu-id="4ec63-148">Tez automatikusan számítja ki feladatokat, így nem kell állítsa be úgy kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4ec63-148">Tez automatically calculates the number of tasks that should be created so you do not need to set it.</span></span>   

## <a name="example-calculation"></a><span data-ttu-id="4ec63-149">Példa kiszámítása</span><span class="sxs-lookup"><span data-stu-id="4ec63-149">Example Calculation</span></span>

<span data-ttu-id="4ec63-150">Tegyük fel, egy 8 csomópont D14 fürt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4ec63-150">Let’s say you have an 8 node D14 cluster.</span></span>  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a><span data-ttu-id="4ec63-151">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="4ec63-151">Limitations</span></span>
<span data-ttu-id="4ec63-152">**ADLS-szabályozás**</span><span class="sxs-lookup"><span data-stu-id="4ec63-152">**ADLS throttling**</span></span> 

<span data-ttu-id="4ec63-153">UIf találati ADLS által biztosított sávszélesség határain, hogy feladat hibáihoz kezdenie.</span><span class="sxs-lookup"><span data-stu-id="4ec63-153">UIf you hit the limits of bandwidth provided by ADLS, you would start to see task failures.</span></span> <span data-ttu-id="4ec63-154">Ez azonosítható betartásával szabályozási hibák feladat naplókban által.</span><span class="sxs-lookup"><span data-stu-id="4ec63-154">This could be identified by observing throttling errors in task logs.</span></span>  <span data-ttu-id="4ec63-155">A párhuzamos végrehajtás Tez tároló méretének növelésével csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="4ec63-155">You can decrease the parallelism by increasing Tez container size.</span></span>  <span data-ttu-id="4ec63-156">Ha a feladat több egyidejű van szüksége, lépjen kapcsolatba velünk a következő címen.</span><span class="sxs-lookup"><span data-stu-id="4ec63-156">If you need more concurrency for your job, please contact us.</span></span>   

<span data-ttu-id="4ec63-157">Ha Ön első szabályozott ellenőrzéséhez szeretne engedélyezni a hibakeresési naplózás az ügyféloldalon.</span><span class="sxs-lookup"><span data-stu-id="4ec63-157">To check if you are getting throttled, you need to enable the debug logging on the client side.</span></span> <span data-ttu-id="4ec63-158">Ez hogyan azt teheti meg:</span><span class="sxs-lookup"><span data-stu-id="4ec63-158">Here’s how you can do that:</span></span>

1. <span data-ttu-id="4ec63-159">Helyezze el a következő tulajdonság a Hive-config log4j tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="4ec63-159">Put the following property in the log4j properties in Hive config.</span></span> <span data-ttu-id="4ec63-160">Ezt megteheti az Ambari nézetben: log4j.logger.com.microsoft.azure.datalake.store=DEBUG indítsa újra az összes a csomópontok/szolgáltatást a konfiguráció életbe léptetéséhez.</span><span class="sxs-lookup"><span data-stu-id="4ec63-160">This can be done from Ambari view: log4j.logger.com.microsoft.azure.datalake.store=DEBUG Restart all the nodes/service for the config to take effect.</span></span>

2. <span data-ttu-id="4ec63-161">Ha Ön első szabályozott, látni fogja, a hive naplófájlban a HTTP 429 hibakód.</span><span class="sxs-lookup"><span data-stu-id="4ec63-161">If you are getting throttled, you’ll see the HTTP 429 error code in the hive log file.</span></span> <span data-ttu-id="4ec63-162">A hive naplófájl van /tmp/&lt;felhasználói&gt;/hive.log</span><span class="sxs-lookup"><span data-stu-id="4ec63-162">The hive log file is in /tmp/&lt;user&gt;/hive.log</span></span>

## <a name="further-information-on-hive-tuning"></a><span data-ttu-id="4ec63-163">További információ a Hive hangolása</span><span class="sxs-lookup"><span data-stu-id="4ec63-163">Further information on Hive tuning</span></span>

<span data-ttu-id="4ec63-164">Az alábbiakban néhány rendszerek, amelyek segítségével finomhangolhatják a Hive-lekérdezéseket:</span><span class="sxs-lookup"><span data-stu-id="4ec63-164">Here are a few blogs that will help tune your Hive queries:</span></span>
* [<span data-ttu-id="4ec63-165">A hdinsight Hadoop Hive-lekérdezések optimalizálása</span><span class="sxs-lookup"><span data-stu-id="4ec63-165">Optimize Hive queries for Hadoop in HDInsight</span></span>](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [<span data-ttu-id="4ec63-166">Hibaelhárítás a Hive-lekérdezések teljesítményét</span><span class="sxs-lookup"><span data-stu-id="4ec63-166">Troubleshooting Hive query performance</span></span>](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [<span data-ttu-id="4ec63-167">Az ignite előadás a HDInsight Hive a optimalizálása</span><span class="sxs-lookup"><span data-stu-id="4ec63-167">Ignite talk on optimize Hive on HDInsight</span></span>](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
