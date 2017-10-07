---
title: "Azure HDInsight segítségével YARN aaaTroubleshoot |} Microsoft Docs"
description: "Válaszok az Apache Hadoop yarn rendszerre és az Azure HDInsight toocommon kérdésekre."
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
ms.openlocfilehash: 800d9738cb27e05a64db470ee58565af3b85aa99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-yarn-by-using-azure-hdinsight"></a><span data-ttu-id="a1edd-104">Hibaelhárítás YARN Azure HDInsight segítségével</span><span class="sxs-lookup"><span data-stu-id="a1edd-104">Troubleshoot YARN by using Azure HDInsight</span></span>

<span data-ttu-id="a1edd-105">Hello legfőbb problémákat és azok megoldásait ismerje meg az Apache Ambari az Apache Hadoop YARN Payload van jelen használatakor.</span><span class="sxs-lookup"><span data-stu-id="a1edd-105">Learn about hello top issues and their resolutions when working with Apache Hadoop YARN payloads in Apache Ambari.</span></span>

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a><span data-ttu-id="a1edd-106">Hogyan hozzon létre egy új YARN várólistát egy fürtön</span><span class="sxs-lookup"><span data-stu-id="a1edd-106">How do I create a new YARN queue on a cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="a1edd-107">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="a1edd-107">Resolution steps</span></span> 

<span data-ttu-id="a1edd-108">Használja hello alábbi új YARN várólista Ambari toocreate lépéseit, és majd egyenleg közötti összes hello várólisták hello kapacitásának elosztását.</span><span class="sxs-lookup"><span data-stu-id="a1edd-108">Use hello following steps in Ambari toocreate a new YARN queue, and then balance hello capacity allocation among all hello queues.</span></span> 

<span data-ttu-id="a1edd-109">Ebben a példában két meglévő várólisták (**alapértelmezett** és **thriftsvr**) is megváltoznak kapacitásából 50 % kapacitás too25 %, ami hello új várólista (külső) 50 % kapacitást biztosít.</span><span class="sxs-lookup"><span data-stu-id="a1edd-109">In this example, two existing queues (**default** and **thriftsvr**) both are changed from 50% capacity too25% capacity, which gives hello new queue (spark) 50% capacity.</span></span>
| <span data-ttu-id="a1edd-110">Várólista</span><span class="sxs-lookup"><span data-stu-id="a1edd-110">Queue</span></span> | <span data-ttu-id="a1edd-111">Kapacitás</span><span class="sxs-lookup"><span data-stu-id="a1edd-111">Capacity</span></span> | <span data-ttu-id="a1edd-112">Maximális kapacitás</span><span class="sxs-lookup"><span data-stu-id="a1edd-112">Maximum capacity</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a1edd-113">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="a1edd-113">default</span></span> | <span data-ttu-id="a1edd-114">25%</span><span class="sxs-lookup"><span data-stu-id="a1edd-114">25%</span></span> | <span data-ttu-id="a1edd-115">50%</span><span class="sxs-lookup"><span data-stu-id="a1edd-115">50%</span></span> |
| <span data-ttu-id="a1edd-116">thrftsvr</span><span class="sxs-lookup"><span data-stu-id="a1edd-116">thrftsvr</span></span> | <span data-ttu-id="a1edd-117">25%</span><span class="sxs-lookup"><span data-stu-id="a1edd-117">25%</span></span> | <span data-ttu-id="a1edd-118">50%</span><span class="sxs-lookup"><span data-stu-id="a1edd-118">50%</span></span> |
| <span data-ttu-id="a1edd-119">Spark</span><span class="sxs-lookup"><span data-stu-id="a1edd-119">spark</span></span> | <span data-ttu-id="a1edd-120">50%</span><span class="sxs-lookup"><span data-stu-id="a1edd-120">50%</span></span> | <span data-ttu-id="a1edd-121">50%</span><span class="sxs-lookup"><span data-stu-id="a1edd-121">50%</span></span> |

1. <span data-ttu-id="a1edd-122">Jelölje be hello **Ambari nézetek** ikonra, és jelölje ki hello rács mintát.</span><span class="sxs-lookup"><span data-stu-id="a1edd-122">Select hello **Ambari Views** icon, and then select hello grid pattern.</span></span> <span data-ttu-id="a1edd-123">Válassza ki, **YARN várólista-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="a1edd-123">Next, select **YARN Queue Manager**.</span></span>

    ![Válassza ki a hello Ambari nézetek ikon](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
2. <span data-ttu-id="a1edd-125">Jelölje be hello **alapértelmezett** várólista.</span><span class="sxs-lookup"><span data-stu-id="a1edd-125">Select hello **default** queue.</span></span>

    ![Válassza ki a hello alapértelmezett várólista](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
3. <span data-ttu-id="a1edd-127">A hello **alapértelmezett** várólista, módosítsa a hello **kapacitás** 50 % too25 %.</span><span class="sxs-lookup"><span data-stu-id="a1edd-127">For hello **default** queue, change hello **capacity** from 50% too25%.</span></span> <span data-ttu-id="a1edd-128">A hello **thriftsvr** várólista, módosítsa a hello **kapacitás** too25 %.</span><span class="sxs-lookup"><span data-stu-id="a1edd-128">For hello **thriftsvr** queue, change hello **capacity** too25%.</span></span>

    ![Hello kapacitás too25 % hello alapértelmezett és thriftsvr várólisták módosítása](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
4. <span data-ttu-id="a1edd-130">Válasszon egy új sor toocreate **hozzáadása várólista**.</span><span class="sxs-lookup"><span data-stu-id="a1edd-130">toocreate a new queue, select **Add Queue**.</span></span>

    ![Válassza ki a várólista hozzáadása](media/hdinsight-troubleshoot-yarn/create-queue-4.png)

5. <span data-ttu-id="a1edd-132">Hello új várólista neve.</span><span class="sxs-lookup"><span data-stu-id="a1edd-132">Name hello new queue.</span></span>

    ![Spark hello várólista neve](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  

6. <span data-ttu-id="a1edd-134">Hagyja hello **kapacitás** érték 50 %-át, és jelölje ki hello **műveletek** gombra.</span><span class="sxs-lookup"><span data-stu-id="a1edd-134">Leave hello **capacity** values at 50%, and then select hello **Actions** button.</span></span>

    ![Válassza ki a hello műveletek gomb](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
7. <span data-ttu-id="a1edd-136">Válassza ki **mentse és frissítse a várólisták**.</span><span class="sxs-lookup"><span data-stu-id="a1edd-136">Select **Save and Refresh Queues**.</span></span>

    ![Válassza ki, mentse, és a frissítést](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

<span data-ttu-id="a1edd-138">A módosítások azonnal a YARN Feladatütemező felhasználói felület hello láthatók.</span><span class="sxs-lookup"><span data-stu-id="a1edd-138">These changes are visible immediately on hello YARN Scheduler UI.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="a1edd-139">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="a1edd-139">Additional reading</span></span>

- [<span data-ttu-id="a1edd-140">YARN CapacityScheduler</span><span class="sxs-lookup"><span data-stu-id="a1edd-140">YARN CapacityScheduler</span></span>](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)


## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a><span data-ttu-id="a1edd-141">Hogyan le a YARN naplóit fürtből</span><span class="sxs-lookup"><span data-stu-id="a1edd-141">How do I download YARN logs from a cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="a1edd-142">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="a1edd-142">Resolution steps</span></span> 

1. <span data-ttu-id="a1edd-143">Toohello HDInsight-fürtjéhez Secure Shell (SSH) ügyfél csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="a1edd-143">Connect toohello HDInsight cluster by using a Secure Shell (SSH) client.</span></span> <span data-ttu-id="a1edd-144">További információkért lásd: [További olvasnivaló](#additional-reading-2).</span><span class="sxs-lookup"><span data-stu-id="a1edd-144">For more information, see [Additional reading](#additional-reading-2).</span></span>

2. <span data-ttu-id="a1edd-145">toolist összes hello Alkalmazásazonosítók hello YARN alkalmazások futnak, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="a1edd-145">toolist all hello application IDs of hello YARN applications that are currently running, run hello following command:</span></span>

    ```apache
    yarn top
    ```
    <span data-ttu-id="a1edd-146">hello azonosítók a hello **APPLICATIONID** oszlop.</span><span class="sxs-lookup"><span data-stu-id="a1edd-146">hello IDs are listed in hello **APPLICATIONID** column.</span></span> <span data-ttu-id="a1edd-147">Naplók letölthető hello **APPLICATIONID** oszlop.</span><span class="sxs-lookup"><span data-stu-id="a1edd-147">You can download logs from hello **APPLICATIONID** column.</span></span>

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

3. <span data-ttu-id="a1edd-148">toodownload YARN tároló naplók összes alkalmazás főkiszolgálók, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="a1edd-148">toodownload YARN container logs for all application masters, use hello following command:</span></span>
   
    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    <span data-ttu-id="a1edd-149">Ez a parancs egy amlogs.txt nevű naplófájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a1edd-149">This command creates a log file named amlogs.txt.</span></span> 

4. <span data-ttu-id="a1edd-150">toodownload YARN tároló naplók csak hello legfrissebb alkalmazás master, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="a1edd-150">toodownload YARN container logs for only hello latest application master, use hello following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    <span data-ttu-id="a1edd-151">Ez a parancs egy latestamlogs.txt nevű naplófájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a1edd-151">This command creates a log file named latestamlogs.txt.</span></span> 

4. <span data-ttu-id="a1edd-152">toodownload YARN tároló naplók hello első két alkalmazás főkiszolgálók, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="a1edd-152">toodownload YARN container logs for hello first two application masters, use hello following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt 
    ```

    <span data-ttu-id="a1edd-153">Ez a parancs egy first2amlogs.txt nevű naplófájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a1edd-153">This command creates a log file named first2amlogs.txt.</span></span> 

5. <span data-ttu-id="a1edd-154">toodownload tároló összes YARN naplóit, használjon hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a1edd-154">toodownload all YARN container logs, use hello following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    <span data-ttu-id="a1edd-155">Ez a parancs egy logs.txt nevű naplófájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a1edd-155">This command creates a log file named logs.txt.</span></span> 

6. <span data-ttu-id="a1edd-156">toodownload hello YARN tároló napló a egy adott tárolóhoz, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="a1edd-156">toodownload hello YARN container log for a specific container, use hello following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt 
    ```

    <span data-ttu-id="a1edd-157">Ez a parancs egy containerlogs.txt nevű naplófájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a1edd-157">This command creates a log file named containerlogs.txt.</span></span>

### <span data-ttu-id="a1edd-158"><a name="additional-reading-2"></a>További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="a1edd-158"><a name="additional-reading-2"></a>Additional reading</span></span>

- [<span data-ttu-id="a1edd-159">Kapcsolódás az SSH használatával tooHDInsight (Hadoop)</span><span class="sxs-lookup"><span data-stu-id="a1edd-159">Connect tooHDInsight (Hadoop) by using SSH</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [<span data-ttu-id="a1edd-160">Apache Hadoop YARN fogalmakat és alkalmazások</span><span class="sxs-lookup"><span data-stu-id="a1edd-160">Apache Hadoop YARN concepts and applications</span></span>](https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/)







