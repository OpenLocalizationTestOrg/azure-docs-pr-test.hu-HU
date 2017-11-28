---
title: "aaaApache Kafka méretezés - Azure HDInsight növelése |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure kezelése az Azure HDInsight tooincrease méretezhetőség Apache Kafka fürt lemezek."
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
ms.date: 06/14/2017
ms.author: larryfr
ms.openlocfilehash: b51114b33359f2c385f057a7a7a5b134cad27e51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-storage-and-scalability-for-apache-kafka-on-hdinsight"></a><span data-ttu-id="26803-103">Tárhely és méretezhetőség konfigurálása HDInsight-beli Apache Kafka platformon</span><span class="sxs-lookup"><span data-stu-id="26803-103">Configure storage and scalability for Apache Kafka on HDInsight</span></span>

<span data-ttu-id="26803-104">Ismerje meg, hogyan tooconfigure hello lemezeinek száma miatt felügyelt Apache Kafka a HDInsight használja.</span><span class="sxs-lookup"><span data-stu-id="26803-104">Learn how tooconfigure hello number of managed disks used by Apache Kafka on HDInsight.</span></span>

<span data-ttu-id="26803-105">A HDInsight Kafka hello helyi lemez hello virtuális gépek hello HDInsight-fürtöt használ.</span><span class="sxs-lookup"><span data-stu-id="26803-105">Kafka on HDInsight uses hello local disk of hello virtual machines in hello HDInsight cluster.</span></span> <span data-ttu-id="26803-106">Mivel a Kafka nagyon nehéz, I/O [Azure felügyelt lemezek](../virtual-machines/windows/managed-disks-overview.md) használt tooprovide magas teljesítmény és csomópontonként nagyobb tárterületet biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="26803-106">Since Kafka is very I/O heavy, [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md) is used tooprovide high throughput and provide more storage per node.</span></span> <span data-ttu-id="26803-107">Hagyományos virtuális merevlemezek (VHD) használták Kafka, minden csomópont esetén korlátozott too1 TB.</span><span class="sxs-lookup"><span data-stu-id="26803-107">If traditional virtual hard drives (VHD) were used for Kafka, each node is limited too1 TB.</span></span> <span data-ttu-id="26803-108">A felügyelt lemezek esetében is használhat több lemezek tooachieve 16 TB hello fürt minden csomópontja számára.</span><span class="sxs-lookup"><span data-stu-id="26803-108">With managed disks, you can use multiple disks tooachieve 16 TB for each node in hello cluster.</span></span>

<span data-ttu-id="26803-109">hello következő diagram biztosít a felügyelt lemezek előtt HDInsight Kafka és Kafka összehasonlítása a HDInsight kezelt lemezek:</span><span class="sxs-lookup"><span data-stu-id="26803-109">hello following diagram provides a comparison between Kafka on HDInsight before managed disks, and Kafka on HDInsight with managed disks:</span></span>

![Ábra a HDInsight-beli Kafka platformról virtuális gépenként egy virtuális merevlemezzel és virtuális gépenként több felügyelt lemezzel](./media/hdinsight-apache-kafka-scalability/kafka-with-managed-disks-architecture.png)

## <a name="configure-managed-disks-azure-portal"></a><span data-ttu-id="26803-111">Felügyelt lemezek konfigurálása: Azure Portal</span><span class="sxs-lookup"><span data-stu-id="26803-111">Configure managed disks: Azure portal</span></span>

1. <span data-ttu-id="26803-112">Hello kövesse hello [HDInsight-fürtök létrehozása](hdinsight-hadoop-create-linux-clusters-portal.md) toounderstand hello közös lépések toocreate egy fürt hello portál használatával.</span><span class="sxs-lookup"><span data-stu-id="26803-112">Follow hello steps in hello [Create an HDInsight cluster](hdinsight-hadoop-create-linux-clusters-portal.md) toounderstand hello common steps toocreate a cluster using hello portal.</span></span> <span data-ttu-id="26803-113">Ne hajtsa végre a hello portál létrehozási folyamata.</span><span class="sxs-lookup"><span data-stu-id="26803-113">Do not complete hello portal creation process.</span></span>

2. <span data-ttu-id="26803-114">A hello __a fürt méretét__ panelen, használjon hello __egyes feldolgozó csomópontok lemezek__ mezőben tooconfigure hello lemezeinek száma miatt.</span><span class="sxs-lookup"><span data-stu-id="26803-114">From hello __Cluster size__ blade, use hello __Disks per worker node__ field tooconfigure hello number of disks.</span></span>

    > [!NOTE]
    > <span data-ttu-id="26803-115">hello felügyelt lemezes típusú lehet __szabványos__ (HDD) vagy __prémium__ (SSD).</span><span class="sxs-lookup"><span data-stu-id="26803-115">hello type of managed disk can be either __Standard__ (HDD) or __Premium__ (SSD).</span></span> <span data-ttu-id="26803-116">Prémium lemezeket DS és GS sorozatbeli virtuális gépek használnak.</span><span class="sxs-lookup"><span data-stu-id="26803-116">Premium disks are used with DS and GS series VMs.</span></span> <span data-ttu-id="26803-117">Minden más virtuálisgép-típus standard lemezeket használ.</span><span class="sxs-lookup"><span data-stu-id="26803-117">All other VM types use standard.</span></span>

    ![Hello fürt méret panelen a kijelölt munkavégző csomópontonként hello lemezzel képe](./media/hdinsight-apache-kafka-scalability/set-managed-disks-portal.png)

## <a name="configure-managed-disks-resource-manager-template"></a><span data-ttu-id="26803-119">Felügyelt lemezek használata: Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="26803-119">Configure managed disks: Resource Manager template</span></span>

<span data-ttu-id="26803-120">hello munkavégző csomópontokhoz Kafka fürtben, a következő szakasz hello sablon használata hello által használt lemezek toocontrol hello száma:</span><span class="sxs-lookup"><span data-stu-id="26803-120">toocontrol hello number of disks used by hello worker nodes in a Kafka cluster, use hello following section of hello template:</span></span>

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

<span data-ttu-id="26803-121">Egy teljes sablont, amely bemutatja, hogyan kezeli az tooconfigure a lemezek található [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).</span><span class="sxs-lookup"><span data-stu-id="26803-121">You can find a complete template that demonstrates how tooconfigure managed disks at [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="26803-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="26803-122">Next steps</span></span>

<span data-ttu-id="26803-123">További információk a hdinsight Kafka tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="26803-123">For more information on working with Kafka on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="26803-124">A HDInsight MirrorMaker toocreate Kafka másolatának használata</span><span class="sxs-lookup"><span data-stu-id="26803-124">Use MirrorMaker toocreate a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
* [<span data-ttu-id="26803-125">Az Apache Storm használata a HDInsighton futó Kafkával</span><span class="sxs-lookup"><span data-stu-id="26803-125">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)
* [<span data-ttu-id="26803-126">Az Apache Spark használata a Kafkával a HDInsighton</span><span class="sxs-lookup"><span data-stu-id="26803-126">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)
* [<span data-ttu-id="26803-127">Csatlakozás tooKafka egy Azure virtuális hálózaton keresztül</span><span class="sxs-lookup"><span data-stu-id="26803-127">Connect tooKafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)

* [<span data-ttu-id="26803-128">HDInsight blog a felügyelt lemezek Kafka platformmal való használatáról</span><span class="sxs-lookup"><span data-stu-id="26803-128">HDInsight blog on managed disks with Kafka</span></span>](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)