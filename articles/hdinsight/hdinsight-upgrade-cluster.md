---
title: "aaaUpgrade HDInsight fürt tooa újabb verziója-Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooUpgrade HDInsight fürt tooa újabb verzióra."
services: hdinsight
documentationcenter: 
author: bhanupr
manager: asadk
editor: bhanupr
ms.assetid: 60eb573c-e639-4815-9fc6-ea8b106d8dbc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/04/2017
ms.author: bhanupr
ms.openlocfilehash: 5fff3c9bc88dfbcbc1ccb0188accdfbbec3a62f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-hdinsight-cluster-tooa-newer-version"></a>A HDInsight fürt tooa újabb verzióra frissítése
tootake előnyeit hello legújabb HDInsight-funkciókat, azt javasoljuk, hogy a HDInsight-fürtök kell-e a frissített toolatest verziója. Kövesse az alábbi irányelvek tooupgrade hello a HDInsight-fürt verziók.

> [!NOTE]
> HDInsight-fürt 3.2-es és 3.3-as verziója hamarosan kivezetési dátum. Információk a HDInsight támogatott verzióján: [HDInsight összetevő verziók](hdinsight-component-versioning.md#supported-hdinsight-versions).
>
>

## <a name="upgrade-tasks"></a>Frissítési feladatok
hello munkafolyamat tooupgrade a HDInsight-fürthöz az alábbiak szerint van.

![Frissítési munkafolyamat diagramja](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. Olvassa el a dokumentum minden szakaszát toounderstand módosítások, amelyek szükségesek lehetnek a HDInsight-fürt frissítésekor.
2. Hozzon létre egy fürtöt, a teszt/minőségi megbízhatósági környezetekben. A fürtök létrehozásáról további információk: [megtudhatja, hogyan toocreate Linux-alapú HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md)
3. Másolás meglévő feladatokat, adatforrások, és új környezet toohello fogadók esetében. Lásd: [adatok másolása tooTest környezet](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) további részleteket.
4. Tesztelési meg arról, hogy a feladatok hello új fürt a várt módon működik-e toomake végez.


Miután ellenőrizte, hogy minden megfelelően működik-e, tervezze hello áttelepítésre. Üzemszünet, során hello alábbi műveleteket:

1.  Bármely átmeneti hello fürtcsomópontokon helyben tárolt adatok biztonsági mentését. Ha például közvetlenül egy átjárócsomóponttal tárolt adatokat.
2.  Hello meglévő fürt törlése.
3.  Hozzon létre egy fürtöt hello ugyanazon alapértelmezett adattároló hello használt előző fürt azonos virtuális hálózaton a legújabb (vagy a támogatott) alhálózati HDI verzióját használja a hello. Ez lehetővé teszi a hello azt az új fürt toocontinue, működik-e a meglévő éles adatok alapján.
4.  Biztonsági másolatot készített az átmeneti adatok importálása.
5.  Kezdő feladatok/folytatni hello új fürt segítségével.

## <a name="next-steps"></a>Következő lépések
* [Ismerje meg, hogyan toocreate Linux-alapú HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md)
* [Csatlakozzon az SSH használatával tooHDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)
* [A Linux-alapú fürt Ambari kezelése](hdinsight-hadoop-manage-ambari.md)

