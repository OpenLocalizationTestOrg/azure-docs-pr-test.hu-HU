---
title: "Az Azure HDInsight-alapú Apache Kafka bemutatása | Microsoft Docs"
description: "Ismerje meg a HDInsight-alapú Apache Kafkát: Mi ez, mire való, hol találhat rá példákat, és hol találhatja meg az első lépésekre vonatkozó információt?"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: f284b6e3-5f3b-4a50-b455-917e77588069
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/15/2017
ms.author: larryfr
ms.openlocfilehash: 1976c52bd7fa56bb07104e205ab3699b2dfa4c50
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="introducing-apache-kafka-on-hdinsight-preview"></a><span data-ttu-id="05c62-103">A HDInsight alatt futó Apache Kafka (előzetes verzió) bemutatása</span><span class="sxs-lookup"><span data-stu-id="05c62-103">Introducing Apache Kafka on HDInsight (preview)</span></span>

<span data-ttu-id="05c62-104">Az [Apache Kafka](https://kafka.apache.org) egy nyílt forráskódú elosztott streamelési platform streamadatfolyamatok és -alkalmazások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="05c62-104">[Apache Kafka](https://kafka.apache.org) is an open-source distributed streaming platform that can be used to build real-time streaming data pipelines and applications.</span></span> <span data-ttu-id="05c62-105">A Kafka az üzenetsorokhoz hasonló üzenetközvetítő funkciót is biztosít, amellyel adatstreameket tehet közzé, illetve feliratkozhat rájuk.</span><span class="sxs-lookup"><span data-stu-id="05c62-105">Kafka also provides message broker functionality similar to a message queue, where you can publish and subscribe to named data streams.</span></span> <span data-ttu-id="05c62-106">A HDInsight alatt futó Kafka felügyelt, rugalmasan méretezhető és magas rendelkezésre állású szolgáltatást biztosít önnek a Microsoft Azure-felhőben.</span><span class="sxs-lookup"><span data-stu-id="05c62-106">Kafka on HDInsight provides you with a managed, highly scalable, and highly available service in the Microsoft Azure cloud.</span></span>

## <a name="why-use-kafka-on-hdinsight"></a><span data-ttu-id="05c62-107">Miért érdemes a HDInsight alatt futó Kafkát használni?</span><span class="sxs-lookup"><span data-stu-id="05c62-107">Why use Kafka on HDInsight?</span></span>

<span data-ttu-id="05c62-108">A Kafka a következő funkciókat biztosítja:</span><span class="sxs-lookup"><span data-stu-id="05c62-108">Kafka provides the following features:</span></span>

* <span data-ttu-id="05c62-109">Közzétételi-feliratkozási üzenetkezelési minta: A előállítói API-t biztosít a rekordok Kafka-témakörökbe való közzétételéhez.</span><span class="sxs-lookup"><span data-stu-id="05c62-109">Publish-subscribe messaging pattern: Kafka provides a Producer API for publishing records to a Kafka topic.</span></span> <span data-ttu-id="05c62-110">A fogyasztói API-ra a témakörökre való feliratkozáskor van szükség.</span><span class="sxs-lookup"><span data-stu-id="05c62-110">The Consumer API is used when subscribing to a topic.</span></span>

* <span data-ttu-id="05c62-111">Streamfeldolgozás: A Kafkát gyakran használják valós idejű streamfeldolgozásra az Apache Stormmal vagy Sparkkal.</span><span class="sxs-lookup"><span data-stu-id="05c62-111">Stream processing: Kafka is often used with Apache Storm or Spark for real-time stream processing.</span></span> <span data-ttu-id="05c62-112">A Kafka 0.10.0.0 (HDInsight 3.5-ös verzió) egy olyan streamelési API-t vezetett be, amely lehetővé teszi a streammegoldások Storm vagy Spark nélküli fejlesztését.</span><span class="sxs-lookup"><span data-stu-id="05c62-112">Kafka 0.10.0.0 (HDInsight version 3.5) introduced a streaming API that allows you to build streaming solutions without requiring Storm or Spark.</span></span>

* <span data-ttu-id="05c62-113">Horizontális skálázhatóság: a Kafka szétosztja a streameket a HDInsight-fürtben található csomópontok között.</span><span class="sxs-lookup"><span data-stu-id="05c62-113">Horizontal scale: Kafka partitions streams across the nodes in the HDInsight cluster.</span></span> <span data-ttu-id="05c62-114">A fogyasztói folyamatok társíthatók az egyes partíciókkal, így biztosítható a terheléselosztás a rekordok használatakor.</span><span class="sxs-lookup"><span data-stu-id="05c62-114">Consumer processes can be associated with individual partitions to provide load balancing when consuming records.</span></span>

* <span data-ttu-id="05c62-115">Érkezési sorrendben történő kézbesítés: a stream minden egyes partíción belül érkezési sorrendben tárolja a rekordokat.</span><span class="sxs-lookup"><span data-stu-id="05c62-115">In-order delivery: Within each partition, records are stored in the stream in the order that they were received.</span></span> <span data-ttu-id="05c62-116">Partíciónként egy fogyasztói folyamat társításával garantálhatja, hogy a rekordok feldolgozása érkezési sorrendben történjen.</span><span class="sxs-lookup"><span data-stu-id="05c62-116">By associating one consumer process per partition, you can guarantee that records are processed in-order.</span></span>

* <span data-ttu-id="05c62-117">Hibatűrés: A partíciók replikálhatók a csomópontok között a hibatűrés biztosításához.</span><span class="sxs-lookup"><span data-stu-id="05c62-117">Fault-tolerant: Partitions can be replicated between nodes to provide fault tolerance.</span></span>

* <span data-ttu-id="05c62-118">Integráció az Azure Managed Disks szolgáltatással: Az Azure Managed Disks jobb méretezést és teljesítményt biztosít a HDInsight-fürt által használt virtuális gépek lemezei számára.</span><span class="sxs-lookup"><span data-stu-id="05c62-118">Integration with Azure Managed Disks: Managed disks provides higher scale and throughput for the disks used by the virtual machines in the HDInsight cluster.</span></span>

    <span data-ttu-id="05c62-119">Alapértelmezés szerint a felügyelt lemezek engedélyezve vannak a HDInsight-alapú Kafka számára, és a csomópontonként használt lemezek száma konfigurálható a HDInsight létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="05c62-119">Managed disks are enabled by default for Kafka on HDInsight, and the number of disks used per node can be configured during HDInsight creation.</span></span> <span data-ttu-id="05c62-120">További tudnivalók a felügyelt lemezekről: [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="05c62-120">For more information on managed disks, see [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md).</span></span>

    <span data-ttu-id="05c62-121">További tudnivalók a felügyelt lemezek HDInsight-alapú Kafkával való konfigurálásáról: [A HDInsight-alapú Kafka méretezhetőségének javítása](hdinsight-apache-kafka-scalability.md).</span><span class="sxs-lookup"><span data-stu-id="05c62-121">For information on configuring managed disks with Kafka on HDInsight, see [Increase scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md).</span></span>

## <a name="use-cases"></a><span data-ttu-id="05c62-122">Használati esetek</span><span class="sxs-lookup"><span data-stu-id="05c62-122">Use cases</span></span>

* <span data-ttu-id="05c62-123">**Üzenetkezelés**: sokszor használják a Kafkát üzenetközvetítőként, mivel támogatja a közzétételi-feliratkozási üzenetmintát.</span><span class="sxs-lookup"><span data-stu-id="05c62-123">**Messaging**: Since it supports the publish-subscribe message pattern, Kafka is often used as a message broker.</span></span>

* <span data-ttu-id="05c62-124">**Tevékenységkövetés**: mivel a Kafka lehetővé teszi a rekordok érkezési sorrend szerinti naplózását, használható tevékenységek nyomon követésére és ismételt létrehozására.</span><span class="sxs-lookup"><span data-stu-id="05c62-124">**Activity tracking**: Since Kafka provides in-order logging of records, it can be used to track and re-create activities.</span></span> <span data-ttu-id="05c62-125">Ilyen tevékenységek például a felhasználók műveletei egy webhelyen vagy egy alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="05c62-125">For example, user actions on a web site or within an application.</span></span>

* <span data-ttu-id="05c62-126">**Összesítés**: streamfeldolgozással összesítheti a különböző streamek információit, hogy működési adatokká egyesítse és központosítsa az információt.</span><span class="sxs-lookup"><span data-stu-id="05c62-126">**Aggregation**: Using stream processing, you can aggregate information from different streams to combine and centralize the information into operational data.</span></span>

* <span data-ttu-id="05c62-127">**Átalakítás**: streamfeldolgozás használatával egyesítheti és bővítheti az adatokat több bemeneti témakörből egy vagy több kimeneti témakörbe.</span><span class="sxs-lookup"><span data-stu-id="05c62-127">**Transformation**: Using stream processing, you can combine and enrich data from multiple input topics into one or more output topics.</span></span>

## <a name="next-steps"></a><span data-ttu-id="05c62-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="05c62-128">Next steps</span></span>

<span data-ttu-id="05c62-129">A HDInsighton futó Apache Kafka használatának megismeréséhez tekintse meg a következő hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="05c62-129">Use the following links to learn how to use Apache Kafka on HDInsight:</span></span>

* [<span data-ttu-id="05c62-130">A HDInsighton futó Kafka használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="05c62-130">Get started with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)

* [<span data-ttu-id="05c62-131">A MirrorMaker használata a Kafka replikájának HDInsighton való létrehozásához</span><span class="sxs-lookup"><span data-stu-id="05c62-131">Use MirrorMaker to create a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)

* [<span data-ttu-id="05c62-132">Az Apache Storm használata a HDInsighton futó Kafkával</span><span class="sxs-lookup"><span data-stu-id="05c62-132">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)

* [<span data-ttu-id="05c62-133">Az Apache Spark használata a Kafkával a HDInsighton</span><span class="sxs-lookup"><span data-stu-id="05c62-133">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)

* [<span data-ttu-id="05c62-134">Csatlakozás a Kafkához Azure Virtual Networkön keresztül</span><span class="sxs-lookup"><span data-stu-id="05c62-134">Connect to Kafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)