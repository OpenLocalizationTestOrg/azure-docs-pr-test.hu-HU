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
# <a name="introducing-apache-kafka-on-hdinsight-preview"></a>A HDInsight alatt futó Apache Kafka (előzetes verzió) bemutatása

[Apache Kafka](https://kafka.apache.org) van egy nyílt forráskódú elosztott adatfolyam platform, amely valós idejű használt toobuild streaming adatok folyamatok és alkalmazások. Kafka emellett üzenet broker funkció hasonló tooa üzenet-várólista, ahol közzététele és előfizetés toonamed adatfolyamokat. A HDInsight Kafka biztosít a felügyelt magas szinten méretezhető és magas rendelkezésre állású szolgáltatás hello Microsoft Azure felhőben.

## <a name="why-use-kafka-on-hdinsight"></a>Miért érdemes a HDInsight alatt futó Kafkát használni?

Kafka hello a következő szolgáltatásokat nyújtja:

* Üzenetkezelési minta közzétételi-feliratkozási: Kafka egy gyártó API alkalmazást biztosít rekordok tooa Kafka témakör közzétételéhez. hello fogyasztói API tooa témakör előfizetés használatos.

* Streamfeldolgozás: A Kafkát gyakran használják valós idejű streamfeldolgozásra az Apache Stormmal vagy Sparkkal. Kafka 0.10.0.0 (HDInsight 3.5-ös verziója) egy streamelési API-t, amely lehetővé teszi a megoldások anélkül, hogy a Storm vagy Spark streaming toobuild vezette be.

* Horizontális skálázhatóságot: Kafka adatfolyamok particionálja hello hello HDInsight-fürt csomópontjai között. Az egyes partíciók tooprovide terheléselosztás amikor rekordok fel fogyasztói folyamatok társítható.

* Sorrendben kézbesítési: minden partíción belül rekordok hello adatfolyam hello ahhoz, hogy azok érkezett vannak tárolva. Partíciónként egy fogyasztói folyamat társításával garantálhatja, hogy a rekordok feldolgozása érkezési sorrendben történjen.

* Hibatűrő: Partíciók replikálható tooprovide hibatűrést csomópontok között.

* Integráció az Azure Managed lemezek: felügyelt lemezek nagyobb méretezés és teljesítmény biztosít hello virtuális gépek hello HDInsight fürt által használt hello lemezek.

    Felügyelt lemezek alapértelmezés szerint engedélyezve vannak a hdinsight Kafka, és csomópontonként a használt lemezeket hello száma konfigurálható HDInsight létrehozása során. További tudnivalók a felügyelt lemezekről: [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md).

    További tudnivalók a felügyelt lemezek HDInsight-alapú Kafkával való konfigurálásáról: [A HDInsight-alapú Kafka méretezhetőségének javítása](hdinsight-apache-kafka-scalability.md).

## <a name="use-cases"></a>Használati esetek

* **Üzenetküldési**: mivel hello támogatja üzenet mintát közzétételi-feliratkozási, egy üzenet broker gyakran használt Kafka.

* **Követés tevékenység**: mivel Kafka biztosít sorrendben naplózási bejegyzések, is használt tootrack kell, és hozza létre a tevékenységeket. Ilyen tevékenységek például a felhasználók műveletei egy webhelyen vagy egy alkalmazásban.

* **Összesítési**: az adatfolyam-feldolgozást használó összesíteni az adatokat a különböző adatfolyamokba toocombine és központosítása hello adatokat az operatív adatok be.

* **Átalakítás**: streamfeldolgozás használatával egyesítheti és bővítheti az adatokat több bemeneti témakörből egy vagy több kimeneti témakörbe.

## <a name="next-steps"></a>Következő lépések

Használjon hello következő hivatkozásait toolearn hogyan toouse Apache Kafka hdinsight:

* [A HDInsighton futó Kafka használatának első lépései](hdinsight-apache-kafka-get-started.md)

* [A HDInsight MirrorMaker toocreate Kafka másolatának használata](hdinsight-apache-kafka-mirroring.md)

* [Az Apache Storm használata a HDInsighton futó Kafkával](hdinsight-apache-storm-with-kafka.md)

* [Az Apache Spark használata a Kafkával a HDInsighton](hdinsight-apache-spark-with-kafka.md)

* [Csatlakozás tooKafka egy Azure virtuális hálózaton keresztül](hdinsight-apache-kafka-connect-vpn-gateway.md)
