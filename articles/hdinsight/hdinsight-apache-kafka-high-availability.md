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
# <a name="high-availability-of-your-data-with-apache-kafka-preview-on-hdinsight"></a>Magas rendelkezésre állású adatok a HDInsightban az Apache Kafka platformmal (előzetes verzió)

Ismerje meg, hogyan tooconfigure partíció replikák Kafka témakörök tootake előny az alapul szolgáló hardver állványra konfigurációs. Ez a konfiguráció hello Apache Kafka a HDInsight-on tárolt adatok rendelkezésre állását biztosítja.

## <a name="fault-and-update-domains-with-kafka"></a>Tartalék és frissítési tartományok a Kafka platformmal

A tartalék tartomány az alapul szolgáló hardver logikai csoportosítása egy Azure-adatközpontban. Mindegyik tartalék tartomány közös áramforrással és hálózati kapcsolóval rendelkezik. a tartalék tartományok elosztott hello virtuális gépek és a felügyelt lemezek, amelyek megvalósítják az hello egy HDInsight-fürt csomópontja. Ez az architektúra korlátozza hello célgyűjtemény fizikai hardver meghibásodása.

Mindegyik Azure-régió meghatározott számú tartalék tartománnyal rendelkezik. Tartományok és a tartalék tartományok bennük hello száma listájáért lásd: hello [rendelkezésre állási készletek](../virtual-machines/linux/regions-and-availability.md#availability-sets) dokumentációját.

> [!IMPORTANT]
> A Kafka nem kezeli a tartalék tartományokat. Amikor létrehoz egy témát a Kafka, hello az összes partíció replika tárolja azonos tartalék tartományban. toosolve probléma nyújtunk hello [Kafka partíció egyensúlyozza ki újra eszköz](https://github.com/hdinsight/hdinsight-kafka-tools).

## <a name="when-toorebalance-partition-replicas"></a>Ha toorebalance partícióazonosító replikák

tooensure hello lehető legmagasabb rendelkezésre állásának Kafka adatait, a témakör a következő alkalommal hello kell egyensúlyba hello partíció replikák:

* Új témakör vagy partíció létrehozásakor

* Fürt vertikális felskálázásakor

## <a name="replication-factor"></a>Replikációs tényező

> [!IMPORTANT]
> Javasoljuk, hogy olyan Azure-régiót használjon, amely három tartalék tartományt tartalmaz, és használjon 3-as replikációs tényezőt.

Ha csak két tartalék tartományok tartalmazó egy régiót kell használnia, replikációs tényezőt használni 4 toospread hello replikák egyenletesen hello két tartalék tartományok között.

Üzenettémák és -beállítás hello replikációs tényező létrehozására láthat példát, lásd: hello [indítsa el a HDInsight Kafka](hdinsight-apache-kafka-get-started.md) dokumentum.

## <a name="how-toorebalance-partition-replicas"></a>Hogyan toorebalance partícióazonosító replikák

Használjon hello [Kafka partíció egyensúlyozza ki újra eszköz](https://github.com/hdinsight/hdinsight-kafka-tools) toorebalance témakör kijelölve. Ez az eszköz kell futott, egy SSH munkamenet toohello központi csomópontból a Kafka fürt.

Az SSH használatával tooHDInsight kapcsolódás további információkért lásd: a [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.

## <a name="next-steps"></a>Következő lépések

* [A Kafka méretezhetősége a HDInsighton](hdinsight-apache-kafka-scalability.md)
* [Tükrözés Kafkával a HDInsighton](hdinsight-apache-kafka-mirroring.md)