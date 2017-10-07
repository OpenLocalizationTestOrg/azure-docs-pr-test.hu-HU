---
title: "aaaAn bemutatása tooApache a HDInsight - Azure Kafka |} Microsoft Docs"
description: "További információk a HDInsight Apache Kafka: Mi, működés, és ahol toofind példák és az első lépések információkat."
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
ms.openlocfilehash: 1bc198d4cf93a4682030d4fa5f71030f49ad64be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introducing-apache-kafka-on-hdinsight-preview"></a><span data-ttu-id="a9232-103">A HDInsight alatt futó Apache Kafka (előzetes verzió) bemutatása</span><span class="sxs-lookup"><span data-stu-id="a9232-103">Introducing Apache Kafka on HDInsight (preview)</span></span>

<span data-ttu-id="a9232-104">[Apache Kafka](https://kafka.apache.org) van egy nyílt forráskódú elosztott adatfolyam platform, amely valós idejű használt toobuild streaming adatok folyamatok és alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="a9232-104">[Apache Kafka](https://kafka.apache.org) is an open-source distributed streaming platform that can be used toobuild real-time streaming data pipelines and applications.</span></span> <span data-ttu-id="a9232-105">Kafka emellett üzenet broker funkció hasonló tooa üzenet-várólista, ahol közzététele és előfizetés toonamed adatfolyamokat.</span><span class="sxs-lookup"><span data-stu-id="a9232-105">Kafka also provides message broker functionality similar tooa message queue, where you can publish and subscribe toonamed data streams.</span></span> <span data-ttu-id="a9232-106">A HDInsight Kafka biztosít a felügyelt magas szinten méretezhető és magas rendelkezésre állású szolgáltatás hello Microsoft Azure felhőben.</span><span class="sxs-lookup"><span data-stu-id="a9232-106">Kafka on HDInsight provides you with a managed, highly scalable, and highly available service in hello Microsoft Azure cloud.</span></span>

## <a name="why-use-kafka-on-hdinsight"></a><span data-ttu-id="a9232-107">Miért érdemes a HDInsight alatt futó Kafkát használni?</span><span class="sxs-lookup"><span data-stu-id="a9232-107">Why use Kafka on HDInsight?</span></span>

<span data-ttu-id="a9232-108">Kafka hello a következő szolgáltatásokat nyújtja:</span><span class="sxs-lookup"><span data-stu-id="a9232-108">Kafka provides hello following features:</span></span>

* <span data-ttu-id="a9232-109">Üzenetkezelési minta közzétételi-feliratkozási: Kafka egy gyártó API alkalmazást biztosít rekordok tooa Kafka témakör közzétételéhez.</span><span class="sxs-lookup"><span data-stu-id="a9232-109">Publish-subscribe messaging pattern: Kafka provides a Producer API for publishing records tooa Kafka topic.</span></span> <span data-ttu-id="a9232-110">hello fogyasztói API tooa témakör előfizetés használatos.</span><span class="sxs-lookup"><span data-stu-id="a9232-110">hello Consumer API is used when subscribing tooa topic.</span></span>

* <span data-ttu-id="a9232-111">Streamfeldolgozás: A Kafkát gyakran használják valós idejű streamfeldolgozásra az Apache Stormmal vagy Sparkkal.</span><span class="sxs-lookup"><span data-stu-id="a9232-111">Stream processing: Kafka is often used with Apache Storm or Spark for real-time stream processing.</span></span> <span data-ttu-id="a9232-112">Kafka 0.10.0.0 (HDInsight 3.5-ös verziója) egy streamelési API-t, amely lehetővé teszi a megoldások anélkül, hogy a Storm vagy Spark streaming toobuild vezette be.</span><span class="sxs-lookup"><span data-stu-id="a9232-112">Kafka 0.10.0.0 (HDInsight version 3.5) introduced a streaming API that allows you toobuild streaming solutions without requiring Storm or Spark.</span></span>

* <span data-ttu-id="a9232-113">Horizontális skálázhatóságot: Kafka adatfolyamok particionálja hello hello HDInsight-fürt csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="a9232-113">Horizontal scale: Kafka partitions streams across hello nodes in hello HDInsight cluster.</span></span> <span data-ttu-id="a9232-114">Az egyes partíciók tooprovide terheléselosztás amikor rekordok fel fogyasztói folyamatok társítható.</span><span class="sxs-lookup"><span data-stu-id="a9232-114">Consumer processes can be associated with individual partitions tooprovide load balancing when consuming records.</span></span>

* <span data-ttu-id="a9232-115">Sorrendben kézbesítési: minden partíción belül rekordok hello adatfolyam hello ahhoz, hogy azok érkezett vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="a9232-115">In-order delivery: Within each partition, records are stored in hello stream in hello order that they were received.</span></span> <span data-ttu-id="a9232-116">Partíciónként egy fogyasztói folyamat társításával garantálhatja, hogy a rekordok feldolgozása érkezési sorrendben történjen.</span><span class="sxs-lookup"><span data-stu-id="a9232-116">By associating one consumer process per partition, you can guarantee that records are processed in-order.</span></span>

* <span data-ttu-id="a9232-117">Hibatűrő: Partíciók replikálható tooprovide hibatűrést csomópontok között.</span><span class="sxs-lookup"><span data-stu-id="a9232-117">Fault-tolerant: Partitions can be replicated between nodes tooprovide fault tolerance.</span></span>

* <span data-ttu-id="a9232-118">Integráció az Azure Managed lemezek: felügyelt lemezek nagyobb méretezés és teljesítmény biztosít hello virtuális gépek hello HDInsight fürt által használt hello lemezek.</span><span class="sxs-lookup"><span data-stu-id="a9232-118">Integration with Azure Managed Disks: Managed disks provides higher scale and throughput for hello disks used by hello virtual machines in hello HDInsight cluster.</span></span>

    <span data-ttu-id="a9232-119">Felügyelt lemezek alapértelmezés szerint engedélyezve vannak a hdinsight Kafka, és csomópontonként a használt lemezeket hello száma konfigurálható HDInsight létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="a9232-119">Managed disks are enabled by default for Kafka on HDInsight, and hello number of disks used per node can be configured during HDInsight creation.</span></span> <span data-ttu-id="a9232-120">További tudnivalók a felügyelt lemezekről: [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a9232-120">For more information on managed disks, see [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md).</span></span>

    <span data-ttu-id="a9232-121">További tudnivalók a felügyelt lemezek HDInsight-alapú Kafkával való konfigurálásáról: [A HDInsight-alapú Kafka méretezhetőségének javítása](hdinsight-apache-kafka-scalability.md).</span><span class="sxs-lookup"><span data-stu-id="a9232-121">For information on configuring managed disks with Kafka on HDInsight, see [Increase scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md).</span></span>

## <a name="use-cases"></a><span data-ttu-id="a9232-122">Használati esetek</span><span class="sxs-lookup"><span data-stu-id="a9232-122">Use cases</span></span>

* <span data-ttu-id="a9232-123">**Üzenetküldési**: mivel hello támogatja üzenet mintát közzétételi-feliratkozási, egy üzenet broker gyakran használt Kafka.</span><span class="sxs-lookup"><span data-stu-id="a9232-123">**Messaging**: Since it supports hello publish-subscribe message pattern, Kafka is often used as a message broker.</span></span>

* <span data-ttu-id="a9232-124">**Követés tevékenység**: mivel Kafka biztosít sorrendben naplózási bejegyzések, is használt tootrack kell, és hozza létre a tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="a9232-124">**Activity tracking**: Since Kafka provides in-order logging of records, it can be used tootrack and re-create activities.</span></span> <span data-ttu-id="a9232-125">Ilyen tevékenységek például a felhasználók műveletei egy webhelyen vagy egy alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a9232-125">For example, user actions on a web site or within an application.</span></span>

* <span data-ttu-id="a9232-126">**Összesítési**: az adatfolyam-feldolgozást használó összesíteni az adatokat a különböző adatfolyamokba toocombine és központosítása hello adatokat az operatív adatok be.</span><span class="sxs-lookup"><span data-stu-id="a9232-126">**Aggregation**: Using stream processing, you can aggregate information from different streams toocombine and centralize hello information into operational data.</span></span>

* <span data-ttu-id="a9232-127">**Átalakítás**: streamfeldolgozás használatával egyesítheti és bővítheti az adatokat több bemeneti témakörből egy vagy több kimeneti témakörbe.</span><span class="sxs-lookup"><span data-stu-id="a9232-127">**Transformation**: Using stream processing, you can combine and enrich data from multiple input topics into one or more output topics.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9232-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a9232-128">Next steps</span></span>

<span data-ttu-id="a9232-129">Használjon hello következő hivatkozásait toolearn hogyan toouse Apache Kafka hdinsight:</span><span class="sxs-lookup"><span data-stu-id="a9232-129">Use hello following links toolearn how toouse Apache Kafka on HDInsight:</span></span>

* [<span data-ttu-id="a9232-130">A HDInsighton futó Kafka használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="a9232-130">Get started with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)

* [<span data-ttu-id="a9232-131">A HDInsight MirrorMaker toocreate Kafka másolatának használata</span><span class="sxs-lookup"><span data-stu-id="a9232-131">Use MirrorMaker toocreate a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)

* [<span data-ttu-id="a9232-132">Az Apache Storm használata a HDInsighton futó Kafkával</span><span class="sxs-lookup"><span data-stu-id="a9232-132">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)

* [<span data-ttu-id="a9232-133">Az Apache Spark használata a Kafkával a HDInsighton</span><span class="sxs-lookup"><span data-stu-id="a9232-133">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)

* [<span data-ttu-id="a9232-134">Csatlakozás tooKafka egy Azure virtuális hálózaton keresztül</span><span class="sxs-lookup"><span data-stu-id="a9232-134">Connect tooKafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)
