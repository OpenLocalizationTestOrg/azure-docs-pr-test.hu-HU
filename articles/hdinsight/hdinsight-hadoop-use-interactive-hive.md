---
title: "A HDInsight - Azure interaktív struktúra |} Microsoft Docs"
description: "Megtudhatja, hogyan interaktív struktúra (Hive LLAP a) a hdinsight eszközben."
keywords: 
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0957643c-4936-48a3-84a3-5dc83db4ab1a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: e7874b55fc72f14d8e2c801872359e823cb2ba34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-interactive-hive-in-hdinsight-preview"></a><span data-ttu-id="fc229-103">Interaktív Hive használata a Hdinsightban (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="fc229-103">Use Interactive Hive in HDInsight (Preview)</span></span>
<span data-ttu-id="fc229-104">Interaktív (más néven struktúra</span><span class="sxs-lookup"><span data-stu-id="fc229-104">Interactive Hive (A.K.A.</span></span> <span data-ttu-id="fc229-105">[Hosszú Live és a folyamat](https://cwiki.apache.org/confluence/display/Hive/LLAP)) az új HDInsight [típusú fürt](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="fc229-105">[Live Long and Process](https://cwiki.apache.org/confluence/display/Hive/LLAP)) is a new HDInsight [cluster type](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>  <span data-ttu-id="fc229-106">Interaktív Hive lehetővé teszi, hogy a memóriába való gyorsítótárazás növeli, amelynek eredményeképpen a Hive-lekérdezéseket, sokkal több interaktív és gyorsabb lett.</span><span class="sxs-lookup"><span data-stu-id="fc229-106">Interactive Hive allows in memory caching that makes Hive queries much more interactive and faster.</span></span> <span data-ttu-id="fc229-107">Ezen új szolgáltatás révén HDInsight a világ legtöbb performant, rugalmas, és nyissa meg a Big Data-megoldások a felhő a memóriában lévő gyorsítótárát (a Hive és a Spark használatával) és advanced analytics átfogóan integrálja az R Services keresztül.</span><span class="sxs-lookup"><span data-stu-id="fc229-107">This new feature makes HDInsight one of the world’s most performant, flexible, and open Big Data solutions on the cloud with in-memory caches (using Hive and Spark) and advanced analytics through deep integration with R Services.</span></span> 

<span data-ttu-id="fc229-108">Az interaktív Hive fürt eltér a Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="fc229-108">The Interactive Hive cluster is different from the Hadoop cluster.</span></span> <span data-ttu-id="fc229-109">Csak a Hive szolgáltatást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="fc229-109">It only contains the Hive service.</span></span> 

> [!NOTE]
> <span data-ttu-id="fc229-110">MapReduce, a Pig, a Sqoop, a Oozie és egyéb szolgáltatások törlődik a fürt típusa hamarosan.</span><span class="sxs-lookup"><span data-stu-id="fc229-110">MapReduce, Pig, Sqoop, Oozie, and other services will be removed from this cluster type soon.</span></span>
> <span data-ttu-id="fc229-111">A Hive-szolgáltatás a interaktív Hive fürt csak az az Ambari Hive view Beeline és Hive ODBC keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="fc229-111">The Hive service in the Interactive Hive cluster is only accessible via the Ambari Hive view, Beeline, and Hive ODBC.</span></span> <span data-ttu-id="fc229-112">Nem érhető el Hive konzol, lépni a Templeton, az Azure parancssori felület és Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="fc229-112">It can’t be accessed via Hive console, Templeton, Azure CLI, and Azure PowerShell.</span></span> 
> 
> 

## <a name="create-an-interactive-hive-cluster"></a><span data-ttu-id="fc229-113">Az interaktív Hive-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="fc229-113">Create an Interactive Hive cluster</span></span>
<span data-ttu-id="fc229-114">Interaktív Hive fürt csak Linux-alapú fürtökön támogatott.</span><span class="sxs-lookup"><span data-stu-id="fc229-114">Interactive Hive cluster is only supported on Linux-based clusters.</span></span> <span data-ttu-id="fc229-115">A HDInsight-fürtök létrehozásával kapcsolatos további információkért lásd: [hdinsight létrehozása Linux-alapú Hadoop-fürtök](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="fc229-115">For information about creating HDInsight clusters, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="execute-hive-from-interactive-hive"></a><span data-ttu-id="fc229-116">Az interaktív Hive Hive végrehajtás</span><span class="sxs-lookup"><span data-stu-id="fc229-116">Execute Hive from Interactive Hive</span></span>
<span data-ttu-id="fc229-117">Nincsenek a másik lehetőség, hogyan futtathat Hive-lekérdezéseket:</span><span class="sxs-lookup"><span data-stu-id="fc229-117">There are different options how you can execute Hive queries:</span></span>

* <span data-ttu-id="fc229-118">Futtassa a Hive használata a Ambari Hive nézete</span><span class="sxs-lookup"><span data-stu-id="fc229-118">Run Hive using the Ambari Hive view</span></span>
  
    <span data-ttu-id="fc229-119">A Hive nézet használatával kapcsolatos információkért lásd: [Hive nézet használata a hadooppal a Hdinsightban](hdinsight-hadoop-use-hive-ambari-view.md).</span><span class="sxs-lookup"><span data-stu-id="fc229-119">For the information about using the Hive View, see [Use the Hive View with Hadoop in HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span></span>
* <span data-ttu-id="fc229-120">Hive Beeline használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="fc229-120">Run Hive using Beeline</span></span>
  
    <span data-ttu-id="fc229-121">Beeline a HDInsight használatáról információkért lásd: [használja a Beeline a HDInsight Hadoop Hive](hdinsight-hadoop-use-hive-beeline.md).</span><span class="sxs-lookup"><span data-stu-id="fc229-121">For the information on using Beeline on HDInsight, see [Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md).</span></span>
  
    <span data-ttu-id="fc229-122">A headnode vagy egy üres élcsomópontot Beeline használja.</span><span class="sxs-lookup"><span data-stu-id="fc229-122">You use Beeline from either the headnode or an empty edge node.</span></span>  <span data-ttu-id="fc229-123">Az egy üres élcsomópontot Beeline használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="fc229-123">Using Beeline from an empty edge node is recommended.</span></span>  <span data-ttu-id="fc229-124">Információk a HDInsight-fürtöt hoz létre egy üres edgenode: [üres peremhálózati csomópontok használata a Hdinsightban](hdinsight-apps-use-edge-node.md).</span><span class="sxs-lookup"><span data-stu-id="fc229-124">For information on creating an HDInsight cluster with an empty edgenode, see [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md).</span></span>
* <span data-ttu-id="fc229-125">Hive Hive ODBC használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="fc229-125">Run Hive using Hive ODBC</span></span>
  
    <span data-ttu-id="fc229-126">A Hive ODBC használatával kapcsolatos információkért lásd: [kapcsolódás Excel és a Microsoft Hive ODBC-illesztőprogram Hadoop](hdinsight-connect-excel-hive-odbc-driver.md).</span><span class="sxs-lookup"><span data-stu-id="fc229-126">For the information on using Hive ODBC, see [Connect Excel to Hadoop with the Microsoft Hive ODBC driver](hdinsight-connect-excel-hive-odbc-driver.md).</span></span>

<span data-ttu-id="fc229-127">**A JDBC-kapcsolati karakterlánc megkeresése:**</span><span class="sxs-lookup"><span data-stu-id="fc229-127">**To find the JDBC connection string:**</span></span>

1. <span data-ttu-id="fc229-128">Jelentkezzen be a következő URL-cím segítségével Ambari: https://<ClusterName>. AzureHDInsight.net.</span><span class="sxs-lookup"><span data-stu-id="fc229-128">Sign on to Ambari using the following URL: https://<ClusterName>.AzureHDInsight.net.</span></span>
2. <span data-ttu-id="fc229-129">Kattintson a **Hive** a bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="fc229-129">Click **Hive** from the left menu.</span></span>
3. <span data-ttu-id="fc229-130">Kattintson a kijelölt ikonra másolja az URL-címet:</span><span class="sxs-lookup"><span data-stu-id="fc229-130">Click the highlighted icon to copy the URL:</span></span>
   
   ![HDInsight Hadoop Hive interaktív LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a><span data-ttu-id="fc229-132">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="fc229-132">See also</span></span>
* <span data-ttu-id="fc229-133">[Linux-alapú Hadoop-fürtök létrehozása a Hdinsightban](hdinsight-hadoop-provision-linux-clusters.md): útmutató hdinsight fürtök interaktív Hive létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="fc229-133">[Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Learn how to create Interactive Hive clusters in HDInsight.</span></span>
* <span data-ttu-id="fc229-134">[A Hive használata a hadooppal a Hdinsightban az Beeline](hdinsight-hadoop-use-hive-beeline.md): Beeline elküldeni a Hive-lekérdezések használata.</span><span class="sxs-lookup"><span data-stu-id="fc229-134">[Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md): Learn how to use Beeline to submit Hive queries.</span></span>
* <span data-ttu-id="fc229-135">[Az Excel és a Hadoop csatlakoztatása a Microsoft Hive ODBC-illesztőprogram a](hdinsight-connect-excel-hive-odbc-driver.md): megtudhatja, hogyan csatlakoztathatja az Excelt struktúra.</span><span class="sxs-lookup"><span data-stu-id="fc229-135">[Connect Excel to Hadoop with the Microsoft Hive ODBC driver](hdinsight-connect-excel-hive-odbc-driver.md): learn how to connect Excel to Hive.</span></span>

