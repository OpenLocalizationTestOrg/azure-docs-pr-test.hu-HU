---
title: "az Apache Kafka - Azure HDInsight aaaHigh elérhetősége |} Microsoft Docs"
description: "Megtudhatja, hogyan tooensure a magas rendelkezésre állás, az Azure HDInsight az Apache Kafka. Ismerje meg, hogyan toorebalance partíció-e a Kafka replikákat, hogy a különböző tartalék tartományok hello belül legyenek az Azure-régió, amely tartalmazza a HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 337468f36b531f83c2999e87907de89cf3d19dd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-of-your-data-with-apache-kafka-preview-on-hdinsight"></a><span data-ttu-id="97c78-104">Magas rendelkezésre állású adatok a HDInsightban az Apache Kafka platformmal (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="97c78-104">High availability of your data with Apache Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="97c78-105">Ismerje meg, hogyan tooconfigure partíció replikák Kafka témakörök tootake előny az alapul szolgáló hardver állványra konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="97c78-105">Learn how tooconfigure partition replicas for Kafka topics tootake advantage of underlying hardware rack configuration.</span></span> <span data-ttu-id="97c78-106">Ez a konfiguráció hello Apache Kafka a HDInsight-on tárolt adatok rendelkezésre állását biztosítja.</span><span class="sxs-lookup"><span data-stu-id="97c78-106">This configuration ensures hello availability of data stored in Apache Kafka on HDInsight.</span></span>

## <a name="fault-and-update-domains-with-kafka"></a><span data-ttu-id="97c78-107">Tartalék és frissítési tartományok a Kafka platformmal</span><span class="sxs-lookup"><span data-stu-id="97c78-107">Fault and update domains with Kafka</span></span>

<span data-ttu-id="97c78-108">A tartalék tartomány az alapul szolgáló hardver logikai csoportosítása egy Azure-adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="97c78-108">A fault domain is a logical grouping of underlying hardware in an Azure data center.</span></span> <span data-ttu-id="97c78-109">Mindegyik tartalék tartomány közös áramforrással és hálózati kapcsolóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="97c78-109">Each fault domain shares a common power source and network switch.</span></span> <span data-ttu-id="97c78-110">a tartalék tartományok elosztott hello virtuális gépek és a felügyelt lemezek, amelyek megvalósítják az hello egy HDInsight-fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="97c78-110">hello virtual machines and managed disks that implement hello nodes within an HDInsight cluster are distributed across these fault domains.</span></span> <span data-ttu-id="97c78-111">Ez az architektúra korlátozza hello célgyűjtemény fizikai hardver meghibásodása.</span><span class="sxs-lookup"><span data-stu-id="97c78-111">This architecture limits hello potential impact of physical hardware failures.</span></span>

<span data-ttu-id="97c78-112">Mindegyik Azure-régió meghatározott számú tartalék tartománnyal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="97c78-112">Each Azure region has a specific number of fault domains.</span></span> <span data-ttu-id="97c78-113">Tartományok és a tartalék tartományok bennük hello száma listájáért lásd: hello [rendelkezésre állási készletek](../virtual-machines/linux/regions-and-availability.md#availability-sets) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="97c78-113">For a list of domains and hello number of fault domains they contain, see hello [Availability sets](../virtual-machines/linux/regions-and-availability.md#availability-sets) documentation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="97c78-114">A Kafka nem kezeli a tartalék tartományokat.</span><span class="sxs-lookup"><span data-stu-id="97c78-114">Kafka is not aware of fault domains.</span></span> <span data-ttu-id="97c78-115">Amikor létrehoz egy témát a Kafka, hello az összes partíció replika tárolja azonos tartalék tartományban.</span><span class="sxs-lookup"><span data-stu-id="97c78-115">When you create a topic in Kafka, it may store all partition replicas in hello same fault domain.</span></span> <span data-ttu-id="97c78-116">toosolve probléma nyújtunk hello [Kafka partíció egyensúlyozza ki újra eszköz](https://github.com/hdinsight/hdinsight-kafka-tools).</span><span class="sxs-lookup"><span data-stu-id="97c78-116">toosolve this problem, we provide hello [Kafka partition rebalance tool](https://github.com/hdinsight/hdinsight-kafka-tools).</span></span>

## <a name="when-toorebalance-partition-replicas"></a><span data-ttu-id="97c78-117">Ha toorebalance partícióazonosító replikák</span><span class="sxs-lookup"><span data-stu-id="97c78-117">When toorebalance partition replicas</span></span>

<span data-ttu-id="97c78-118">tooensure hello lehető legmagasabb rendelkezésre állásának Kafka adatait, a témakör a következő alkalommal hello kell egyensúlyba hello partíció replikák:</span><span class="sxs-lookup"><span data-stu-id="97c78-118">tooensure hello highest availability of your Kafka data, you should rebalance hello partition replicas for your topic at hello following times:</span></span>

* <span data-ttu-id="97c78-119">Új témakör vagy partíció létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="97c78-119">When a new topic or partition is created</span></span>

* <span data-ttu-id="97c78-120">Fürt vertikális felskálázásakor</span><span class="sxs-lookup"><span data-stu-id="97c78-120">When you scale up a cluster</span></span>

## <a name="replication-factor"></a><span data-ttu-id="97c78-121">Replikációs tényező</span><span class="sxs-lookup"><span data-stu-id="97c78-121">Replication factor</span></span>

> [!IMPORTANT]
> <span data-ttu-id="97c78-122">Javasoljuk, hogy olyan Azure-régiót használjon, amely három tartalék tartományt tartalmaz, és használjon 3-as replikációs tényezőt.</span><span class="sxs-lookup"><span data-stu-id="97c78-122">We recommend using an Azure region that contains three fault domains, and using a replication factor of 3.</span></span>

<span data-ttu-id="97c78-123">Ha csak két tartalék tartományok tartalmazó egy régiót kell használnia, replikációs tényezőt használni 4 toospread hello replikák egyenletesen hello két tartalék tartományok között.</span><span class="sxs-lookup"><span data-stu-id="97c78-123">If you must use a region that contains only two fault domains, use a replication factor of 4 toospread hello replicas evenly across hello two fault domains.</span></span>

<span data-ttu-id="97c78-124">Üzenettémák és -beállítás hello replikációs tényező létrehozására láthat példát, lásd: hello [indítsa el a HDInsight Kafka](hdinsight-apache-kafka-get-started.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="97c78-124">For an example of creating topics and setting hello replication factor, see hello [Start with Kafka on HDInsight](hdinsight-apache-kafka-get-started.md) document.</span></span>

## <a name="how-toorebalance-partition-replicas"></a><span data-ttu-id="97c78-125">Hogyan toorebalance partícióazonosító replikák</span><span class="sxs-lookup"><span data-stu-id="97c78-125">How toorebalance partition replicas</span></span>

<span data-ttu-id="97c78-126">Használjon hello [Kafka partíció egyensúlyozza ki újra eszköz](https://github.com/hdinsight/hdinsight-kafka-tools) toorebalance témakör kijelölve.</span><span class="sxs-lookup"><span data-stu-id="97c78-126">Use hello [Kafka partition rebalance tool](https://github.com/hdinsight/hdinsight-kafka-tools) toorebalance selected topics.</span></span> <span data-ttu-id="97c78-127">Ez az eszköz kell futott, egy SSH munkamenet toohello központi csomópontból a Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="97c78-127">This tool must be ran from an SSH session toohello head node of your Kafka cluster.</span></span>

<span data-ttu-id="97c78-128">Az SSH használatával tooHDInsight kapcsolódás további információkért lásd: a [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="97c78-128">For more information on connecting tooHDInsight using SSH, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="97c78-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="97c78-129">Next steps</span></span>

* [<span data-ttu-id="97c78-130">A Kafka méretezhetősége a HDInsighton</span><span class="sxs-lookup"><span data-stu-id="97c78-130">Scalability of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-scalability.md)
* [<span data-ttu-id="97c78-131">Tükrözés Kafkával a HDInsighton</span><span class="sxs-lookup"><span data-stu-id="97c78-131">Mirroring with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)