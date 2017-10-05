---
title: "Hibaelhárítás YARN Azure HDInsight segítségével |} Microsoft Docs"
description: "Az Apache Hadoop yarn rendszerre és az Azure HDInsight kapcsolatos gyakori kérdésekre adott válaszok."
keywords: "Az Azure HDInsight, YARN, gyakori kérdések hibaelhárítási útmutatója, gyakori kérdések"
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: F76786A9-99AB-4B85-9B15-CA03528FC4CD
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: arijitt
ms.openlocfilehash: 63f2d88ad59661b7fbcffd0aaeb94c58d40bdb73
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-yarn-by-using-azure-hdinsight"></a><span data-ttu-id="360d7-104">Hibaelhárítás YARN Azure HDInsight segítségével</span><span class="sxs-lookup"><span data-stu-id="360d7-104">Troubleshoot YARN by using Azure HDInsight</span></span>

<span data-ttu-id="360d7-105">A legfőbb problémákat és azok megoldásait ismerje meg az Apache Ambari az Apache Hadoop YARN Payload van jelen használatakor.</span><span class="sxs-lookup"><span data-stu-id="360d7-105">Learn about the top issues and their resolutions when working with Apache Hadoop YARN payloads in Apache Ambari.</span></span>

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a><span data-ttu-id="360d7-106">Hogyan hozzon létre egy új YARN várólistát egy fürtön</span><span class="sxs-lookup"><span data-stu-id="360d7-106">How do I create a new YARN queue on a cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="360d7-107">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="360d7-107">Resolution steps</span></span> 

<span data-ttu-id="360d7-108">Ambari az alábbi lépések segítségével hozzon létre egy új YARN várólistát, majd válassza az között az összes várólistán kapacitás lefoglalása egyenleg.</span><span class="sxs-lookup"><span data-stu-id="360d7-108">Use the following steps in Ambari to create a new YARN queue, and then balance the capacity allocation among all the queues.</span></span> 

<span data-ttu-id="360d7-109">Ebben a példában két meglévő várólisták (**alapértelmezett** és **thriftsvr**) is megváltoznak 50 % kapacitásából 25 % kapacitás, amely az új várólista (külső) 50 % kapacitást biztosít.</span><span class="sxs-lookup"><span data-stu-id="360d7-109">In this example, two existing queues (**default** and **thriftsvr**) both are changed from 50% capacity to 25% capacity, which gives the new queue (spark) 50% capacity.</span></span>
| <span data-ttu-id="360d7-110">Várólista</span><span class="sxs-lookup"><span data-stu-id="360d7-110">Queue</span></span> | <span data-ttu-id="360d7-111">Kapacitás</span><span class="sxs-lookup"><span data-stu-id="360d7-111">Capacity</span></span> | <span data-ttu-id="360d7-112">Maximális kapacitás</span><span class="sxs-lookup"><span data-stu-id="360d7-112">Maximum capacity</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="360d7-113">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="360d7-113">default</span></span> | <span data-ttu-id="360d7-114">25%</span><span class="sxs-lookup"><span data-stu-id="360d7-114">25%</span></span> | <span data-ttu-id="360d7-115">50%</span><span class="sxs-lookup"><span data-stu-id="360d7-115">50%</span></span> |
| <span data-ttu-id="360d7-116">thrftsvr</span><span class="sxs-lookup"><span data-stu-id="360d7-116">thrftsvr</span></span> | <span data-ttu-id="360d7-117">25%</span><span class="sxs-lookup"><span data-stu-id="360d7-117">25%</span></span> | <span data-ttu-id="360d7-118">50%</span><span class="sxs-lookup"><span data-stu-id="360d7-118">50%</span></span> |
| <span data-ttu-id="360d7-119">Spark</span><span class="sxs-lookup"><span data-stu-id="360d7-119">spark</span></span> | <span data-ttu-id="360d7-120">50%</span><span class="sxs-lookup"><span data-stu-id="360d7-120">50%</span></span> | <span data-ttu-id="360d7-121">50%</span><span class="sxs-lookup"><span data-stu-id="360d7-121">50%</span></span> |

1. <span data-ttu-id="360d7-122">Válassza ki a **Ambari nézetek** ikonra, és válassza a rács mintát.</span><span class="sxs-lookup"><span data-stu-id="360d7-122">Select the **Ambari Views** icon, and then select the grid pattern.</span></span> <span data-ttu-id="360d7-123">Válassza ki, **YARN várólista-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="360d7-123">Next, select **YARN Queue Manager**.</span></span>

    ![Válassza ki az Ambari nézetek ikon](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
2. <span data-ttu-id="360d7-125">Válassza ki a **alapértelmezett** várólista.</span><span class="sxs-lookup"><span data-stu-id="360d7-125">Select the **default** queue.</span></span>

    ![Válassza ki az alapértelmezett várólista](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
3. <span data-ttu-id="360d7-127">Az a **alapértelmezett** várólista, módosítsa a **kapacitás** 50 % 25 %-át.</span><span class="sxs-lookup"><span data-stu-id="360d7-127">For the **default** queue, change the **capacity** from 50% to 25%.</span></span> <span data-ttu-id="360d7-128">Az a **thriftsvr** várólista, módosítsa a **kapacitás** 25 %-át.</span><span class="sxs-lookup"><span data-stu-id="360d7-128">For the **thriftsvr** queue, change the **capacity** to 25%.</span></span>

    ![A kapacitás módosítsa az alapértelmezett és thriftsvr várólisták 25 %-át](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
4. <span data-ttu-id="360d7-130">Hozzon létre egy új várólistát, jelölje be **hozzáadása várólista**.</span><span class="sxs-lookup"><span data-stu-id="360d7-130">To create a new queue, select **Add Queue**.</span></span>

    ![Válassza ki a várólista hozzáadása](media/hdinsight-troubleshoot-yarn/create-queue-4.png)

5. <span data-ttu-id="360d7-132">Az új várólista neve.</span><span class="sxs-lookup"><span data-stu-id="360d7-132">Name the new queue.</span></span>

    ![A várólista Spark neve](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  

6. <span data-ttu-id="360d7-134">Hagyja a **kapacitás** érték 50 %-át, és válassza ki azt a **műveletek** gombra.</span><span class="sxs-lookup"><span data-stu-id="360d7-134">Leave the **capacity** values at 50%, and then select the **Actions** button.</span></span>

    ![A műveletek gomb kiválasztása](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
7. <span data-ttu-id="360d7-136">Válassza ki **mentse és frissítse a várólisták**.</span><span class="sxs-lookup"><span data-stu-id="360d7-136">Select **Save and Refresh Queues**.</span></span>

    ![Válassza ki, mentse, és a frissítést](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

<span data-ttu-id="360d7-138">A módosítások azonnal a YARN Feladatütemező felhasználói felület az láthatók.</span><span class="sxs-lookup"><span data-stu-id="360d7-138">These changes are visible immediately on the YARN Scheduler UI.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="360d7-139">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="360d7-139">Additional reading</span></span>

- [<span data-ttu-id="360d7-140">YARN CapacityScheduler</span><span class="sxs-lookup"><span data-stu-id="360d7-140">YARN CapacityScheduler</span></span>](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)


## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a><span data-ttu-id="360d7-141">Hogyan le a YARN naplóit fürtből</span><span class="sxs-lookup"><span data-stu-id="360d7-141">How do I download YARN logs from a cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="360d7-142">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="360d7-142">Resolution steps</span></span> 

1. <span data-ttu-id="360d7-143">Csatlakozás egy Secure Shell (SSH) ügyfél segítségével a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="360d7-143">Connect to the HDInsight cluster by using a Secure Shell (SSH) client.</span></span> <span data-ttu-id="360d7-144">További információkért lásd: [További olvasnivaló](#additional-reading-2).</span><span class="sxs-lookup"><span data-stu-id="360d7-144">For more information, see [Additional reading](#additional-reading-2).</span></span>

2. <span data-ttu-id="360d7-145">A YARN alkalmazások éppen futó összes alkalmazás azonosítót felsorolásához futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="360d7-145">To list all the application IDs of the YARN applications that are currently running, run the following command:</span></span>

    ```apache
    yarn top
    ```
    <span data-ttu-id="360d7-146">Az azonosítók a a **APPLICATIONID** oszlop.</span><span class="sxs-lookup"><span data-stu-id="360d7-146">The IDs are listed in the **APPLICATIONID** column.</span></span> <span data-ttu-id="360d7-147">Letöltheti a naplói a **APPLICATIONID** oszlop.</span><span class="sxs-lookup"><span data-stu-id="360d7-147">You can download logs from the **APPLICATIONID** column.</span></span>

    ```apache
    YARN top - 18:00:07, up 19d, 0:14, 0 active users, queue(s): root
    NodeManager(s): 4 total, 4 active, 0 unhealthy, 0 decommissioned, 0 lost, 0 rebooted
    Queue(s) Applications: 2 running, 10 submitted, 0 pending, 8 completed, 0 killed, 0 failed
    Queue(s) Mem(GB): 97 available, 3 allocated, 0 pending, 0 reserved
    Queue(s) VCores: 58 available, 2 allocated, 0 pending, 0 reserved
    Queue(s) Containers: 2 allocated, 0 pending, 0 reserved

                      APPLICATIONID USER             TYPE      QUEUE   #CONT  #RCONT  VCORES RVCORES     MEM    RMEM  VCORESECS    MEMSECS %PROGR       TIME NAME
     application_1490377567345_0007 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628407    2442611  10.00   18:20:20 Thrift JDBC/ODBC Server
     application_1490377567345_0006 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628430    2442645  10.00   18:20:20 Thrift JDBC/ODBC Server
    ```

3. <span data-ttu-id="360d7-148">Minden alkalmazás főkiszolgálók YARN tároló naplók letöltéséhez a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="360d7-148">To download YARN container logs for all application masters, use the following command:</span></span>
   
    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    <span data-ttu-id="360d7-149">Ez a parancs egy amlogs.txt nevű naplófájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="360d7-149">This command creates a log file named amlogs.txt.</span></span> 

4. <span data-ttu-id="360d7-150">Csak a legfrissebb alkalmazás főkiszolgáló YARN tároló naplók letöltéséhez a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="360d7-150">To download YARN container logs for only the latest application master, use the following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    <span data-ttu-id="360d7-151">Ez a parancs egy latestamlogs.txt nevű naplófájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="360d7-151">This command creates a log file named latestamlogs.txt.</span></span> 

4. <span data-ttu-id="360d7-152">Az első két alkalmazás főkiszolgálók YARN tároló naplók letöltéséhez a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="360d7-152">To download YARN container logs for the first two application masters, use the following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt 
    ```

    <span data-ttu-id="360d7-153">Ez a parancs egy first2amlogs.txt nevű naplófájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="360d7-153">This command creates a log file named first2amlogs.txt.</span></span> 

5. <span data-ttu-id="360d7-154">Minden YARN tároló naplók letöltéséhez a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="360d7-154">To download all YARN container logs, use the following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    <span data-ttu-id="360d7-155">Ez a parancs egy logs.txt nevű naplófájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="360d7-155">This command creates a log file named logs.txt.</span></span> 

6. <span data-ttu-id="360d7-156">A YARN tároló egy adott tárolóhoz tartozó naplók letöltéséhez a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="360d7-156">To download the YARN container log for a specific container, use the following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt 
    ```

    <span data-ttu-id="360d7-157">Ez a parancs egy containerlogs.txt nevű naplófájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="360d7-157">This command creates a log file named containerlogs.txt.</span></span>

### <span data-ttu-id="360d7-158"><a name="additional-reading-2"></a>További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="360d7-158"><a name="additional-reading-2"></a>Additional reading</span></span>

- [<span data-ttu-id="360d7-159">Csatlakozás HDInsight (Hadoop) SSH használatával</span><span class="sxs-lookup"><span data-stu-id="360d7-159">Connect to HDInsight (Hadoop) by using SSH</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [<span data-ttu-id="360d7-160">Apache Hadoop YARN fogalmakat és alkalmazások</span><span class="sxs-lookup"><span data-stu-id="360d7-160">Apache Hadoop YARN concepts and applications</span></span>](https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/)







