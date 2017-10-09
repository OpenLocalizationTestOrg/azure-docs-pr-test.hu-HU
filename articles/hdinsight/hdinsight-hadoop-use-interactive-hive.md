---
title: "Interaktív Hive hdinsight - Azure aaaUse |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse interaktív struktúra (Hive LLAP a) a hdinsight eszközben."
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
ms.openlocfilehash: 9e751a08091d18bc1b3d070468feef87f6828c61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-interactive-hive-in-hdinsight-preview"></a><span data-ttu-id="4201a-103">Interaktív Hive használata a Hdinsightban (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="4201a-103">Use Interactive Hive in HDInsight (Preview)</span></span>
<span data-ttu-id="4201a-104">Interaktív (más néven struktúra</span><span class="sxs-lookup"><span data-stu-id="4201a-104">Interactive Hive (A.K.A.</span></span> <span data-ttu-id="4201a-105">[Hosszú Live és a folyamat](https://cwiki.apache.org/confluence/display/Hive/LLAP)) az új HDInsight [típusú fürt](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="4201a-105">[Live Long and Process](https://cwiki.apache.org/confluence/display/Hive/LLAP)) is a new HDInsight [cluster type](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>  <span data-ttu-id="4201a-106">Interaktív Hive lehetővé teszi, hogy a memóriába való gyorsítótárazás növeli, amelynek eredményeképpen a Hive-lekérdezéseket, sokkal több interaktív és gyorsabb lett.</span><span class="sxs-lookup"><span data-stu-id="4201a-106">Interactive Hive allows in memory caching that makes Hive queries much more interactive and faster.</span></span> <span data-ttu-id="4201a-107">Ezen új szolgáltatás révén hello egyik HDInsight világ legtöbb performant, rugalmas, és nyissa meg a Big Data-megoldások hello felhő a memóriában lévő gyorsítótárát (a Hive és a Spark használatával) és advanced analytics átfogóan integrálja az R Services keresztül.</span><span class="sxs-lookup"><span data-stu-id="4201a-107">This new feature makes HDInsight one of hello world’s most performant, flexible, and open Big Data solutions on hello cloud with in-memory caches (using Hive and Spark) and advanced analytics through deep integration with R Services.</span></span> 

<span data-ttu-id="4201a-108">hello interaktív Hive fürt Hadoop-fürt hello eltér.</span><span class="sxs-lookup"><span data-stu-id="4201a-108">hello Interactive Hive cluster is different from hello Hadoop cluster.</span></span> <span data-ttu-id="4201a-109">Csak hello struktúra szolgáltatást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4201a-109">It only contains hello Hive service.</span></span> 

> [!NOTE]
> <span data-ttu-id="4201a-110">MapReduce, a Pig, a Sqoop, a Oozie és egyéb szolgáltatások törlődik a fürt típusa hamarosan.</span><span class="sxs-lookup"><span data-stu-id="4201a-110">MapReduce, Pig, Sqoop, Oozie, and other services will be removed from this cluster type soon.</span></span>
> <span data-ttu-id="4201a-111">hello Hive szolgáltatás hello interaktív Hive fürt csak az Ambari Hive nézete hello Beeline és Hive ODBC keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="4201a-111">hello Hive service in hello Interactive Hive cluster is only accessible via hello Ambari Hive view, Beeline, and Hive ODBC.</span></span> <span data-ttu-id="4201a-112">Nem érhető el Hive konzol, lépni a Templeton, az Azure parancssori felület és Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="4201a-112">It can’t be accessed via Hive console, Templeton, Azure CLI, and Azure PowerShell.</span></span> 
> 
> 

## <a name="create-an-interactive-hive-cluster"></a><span data-ttu-id="4201a-113">Az interaktív Hive-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="4201a-113">Create an Interactive Hive cluster</span></span>
<span data-ttu-id="4201a-114">Interaktív Hive fürt csak Linux-alapú fürtökön támogatott.</span><span class="sxs-lookup"><span data-stu-id="4201a-114">Interactive Hive cluster is only supported on Linux-based clusters.</span></span> <span data-ttu-id="4201a-115">A HDInsight-fürtök létrehozásával kapcsolatos további információkért lásd: [hdinsight létrehozása Linux-alapú Hadoop-fürtök](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="4201a-115">For information about creating HDInsight clusters, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="execute-hive-from-interactive-hive"></a><span data-ttu-id="4201a-116">Az interaktív Hive Hive végrehajtás</span><span class="sxs-lookup"><span data-stu-id="4201a-116">Execute Hive from Interactive Hive</span></span>
<span data-ttu-id="4201a-117">Nincsenek a másik lehetőség, hogyan futtathat Hive-lekérdezéseket:</span><span class="sxs-lookup"><span data-stu-id="4201a-117">There are different options how you can execute Hive queries:</span></span>

* <span data-ttu-id="4201a-118">Futtassa a Hive hello Ambari Hive nézete segítségével</span><span class="sxs-lookup"><span data-stu-id="4201a-118">Run Hive using hello Ambari Hive view</span></span>
  
    <span data-ttu-id="4201a-119">Hello hello Hive nézet használatával kapcsolatos információkért lásd: [használata hello a HDInsight Hadoop Hive nézet](hdinsight-hadoop-use-hive-ambari-view.md).</span><span class="sxs-lookup"><span data-stu-id="4201a-119">For hello information about using hello Hive View, see [Use hello Hive View with Hadoop in HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span></span>
* <span data-ttu-id="4201a-120">Hive Beeline használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="4201a-120">Run Hive using Beeline</span></span>
  
    <span data-ttu-id="4201a-121">Hello Beeline a HDInsight használatáról információkért lásd: [használja a Beeline a HDInsight Hadoop Hive](hdinsight-hadoop-use-hive-beeline.md).</span><span class="sxs-lookup"><span data-stu-id="4201a-121">For hello information on using Beeline on HDInsight, see [Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md).</span></span>
  
    <span data-ttu-id="4201a-122">Hello headnode vagy egy üres élcsomópontot Beeline használja.</span><span class="sxs-lookup"><span data-stu-id="4201a-122">You use Beeline from either hello headnode or an empty edge node.</span></span>  <span data-ttu-id="4201a-123">Az egy üres élcsomópontot Beeline használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="4201a-123">Using Beeline from an empty edge node is recommended.</span></span>  <span data-ttu-id="4201a-124">Információk a HDInsight-fürtöt hoz létre egy üres edgenode: [üres peremhálózati csomópontok használata a Hdinsightban](hdinsight-apps-use-edge-node.md).</span><span class="sxs-lookup"><span data-stu-id="4201a-124">For information on creating an HDInsight cluster with an empty edgenode, see [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md).</span></span>
* <span data-ttu-id="4201a-125">Hive Hive ODBC használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="4201a-125">Run Hive using Hive ODBC</span></span>
  
    <span data-ttu-id="4201a-126">Hello Hive ODBC használatával kapcsolatos információkért lásd: [kapcsolódás Excel tooHadoop hello Microsoft Hive ODBC-illesztővel rendelkező](hdinsight-connect-excel-hive-odbc-driver.md).</span><span class="sxs-lookup"><span data-stu-id="4201a-126">For hello information on using Hive ODBC, see [Connect Excel tooHadoop with hello Microsoft Hive ODBC driver](hdinsight-connect-excel-hive-odbc-driver.md).</span></span>

<span data-ttu-id="4201a-127">**toofind hello JDBC kapcsolati karakterlánc:**</span><span class="sxs-lookup"><span data-stu-id="4201a-127">**toofind hello JDBC connection string:**</span></span>

1. <span data-ttu-id="4201a-128">Használja a következő URL-cím hello tooAmbari bejelentkezés: https://<ClusterName>. AzureHDInsight.net.</span><span class="sxs-lookup"><span data-stu-id="4201a-128">Sign on tooAmbari using hello following URL: https://<ClusterName>.AzureHDInsight.net.</span></span>
2. <span data-ttu-id="4201a-129">Kattintson a **Hive** hello bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="4201a-129">Click **Hive** from hello left menu.</span></span>
3. <span data-ttu-id="4201a-130">Kattintson a kijelölt hello ikon toocopy hello URL-címe:</span><span class="sxs-lookup"><span data-stu-id="4201a-130">Click hello highlighted icon toocopy hello URL:</span></span>
   
   ![HDInsight Hadoop Hive interaktív LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a><span data-ttu-id="4201a-132">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4201a-132">See also</span></span>
* <span data-ttu-id="4201a-133">[Linux-alapú Hadoop-fürtök létrehozása a Hdinsightban](hdinsight-hadoop-provision-linux-clusters.md): megtudhatja, hogyan toocreate interaktív Hive hdinsight clusters.</span><span class="sxs-lookup"><span data-stu-id="4201a-133">[Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Learn how toocreate Interactive Hive clusters in HDInsight.</span></span>
* <span data-ttu-id="4201a-134">[A Hive használata a hadooppal a Hdinsightban az Beeline](hdinsight-hadoop-use-hive-beeline.md): megtudhatja, hogyan toouse Beeline toosubmit Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="4201a-134">[Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md): Learn how toouse Beeline toosubmit Hive queries.</span></span>
* <span data-ttu-id="4201a-135">[Csatlakozás Excel tooHadoop hello Microsoft Hive ODBC-illesztőprogram](hdinsight-connect-excel-hive-odbc-driver.md): megtudhatja, hogyan tooconnect Excel tooHive.</span><span class="sxs-lookup"><span data-stu-id="4201a-135">[Connect Excel tooHadoop with hello Microsoft Hive ODBC driver](hdinsight-connect-excel-hive-odbc-driver.md): learn how tooconnect Excel tooHive.</span></span>

