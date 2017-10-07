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
# <a name="configure-storage-and-scalability-for-apache-kafka-on-hdinsight"></a>Tárhely és méretezhetőség konfigurálása HDInsight-beli Apache Kafka platformon

Ismerje meg, hogyan tooconfigure hello lemezeinek száma miatt felügyelt Apache Kafka a HDInsight használja.

A HDInsight Kafka hello helyi lemez hello virtuális gépek hello HDInsight-fürtöt használ. Mivel a Kafka nagyon nehéz, I/O [Azure felügyelt lemezek](../virtual-machines/windows/managed-disks-overview.md) használt tooprovide magas teljesítmény és csomópontonként nagyobb tárterületet biztosítanak. Hagyományos virtuális merevlemezek (VHD) használták Kafka, minden csomópont esetén korlátozott too1 TB. A felügyelt lemezek esetében is használhat több lemezek tooachieve 16 TB hello fürt minden csomópontja számára.

hello következő diagram biztosít a felügyelt lemezek előtt HDInsight Kafka és Kafka összehasonlítása a HDInsight kezelt lemezek:

![Ábra a HDInsight-beli Kafka platformról virtuális gépenként egy virtuális merevlemezzel és virtuális gépenként több felügyelt lemezzel](./media/hdinsight-apache-kafka-scalability/kafka-with-managed-disks-architecture.png)

## <a name="configure-managed-disks-azure-portal"></a>Felügyelt lemezek konfigurálása: Azure Portal

1. Hello kövesse hello [HDInsight-fürtök létrehozása](hdinsight-hadoop-create-linux-clusters-portal.md) toounderstand hello közös lépések toocreate egy fürt hello portál használatával. Ne hajtsa végre a hello portál létrehozási folyamata.

2. A hello __a fürt méretét__ panelen, használjon hello __egyes feldolgozó csomópontok lemezek__ mezőben tooconfigure hello lemezeinek száma miatt.

    > [!NOTE]
    > hello felügyelt lemezes típusú lehet __szabványos__ (HDD) vagy __prémium__ (SSD). Prémium lemezeket DS és GS sorozatbeli virtuális gépek használnak. Minden más virtuálisgép-típus standard lemezeket használ.

    ![Hello fürt méret panelen a kijelölt munkavégző csomópontonként hello lemezzel képe](./media/hdinsight-apache-kafka-scalability/set-managed-disks-portal.png)

## <a name="configure-managed-disks-resource-manager-template"></a>Felügyelt lemezek használata: Resource Manager-sablon

hello munkavégző csomópontokhoz Kafka fürtben, a következő szakasz hello sablon használata hello által használt lemezek toocontrol hello száma:

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

Egy teljes sablont, amely bemutatja, hogyan kezeli az tooconfigure a lemezek található [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).

## <a name="next-steps"></a>Következő lépések

További információk a hdinsight Kafka tekintse meg a következő dokumentumok hello:

* [A HDInsight MirrorMaker toocreate Kafka másolatának használata](hdinsight-apache-kafka-mirroring.md)
* [Az Apache Storm használata a HDInsighton futó Kafkával](hdinsight-apache-storm-with-kafka.md)
* [Az Apache Spark használata a Kafkával a HDInsighton](hdinsight-apache-spark-with-kafka.md)
* [Csatlakozás tooKafka egy Azure virtuális hálózaton keresztül](hdinsight-apache-kafka-connect-vpn-gateway.md)

* [HDInsight blog a felügyelt lemezek Kafka platformmal való használatáról](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)