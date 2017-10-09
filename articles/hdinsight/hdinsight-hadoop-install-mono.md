---
title: "aaaInstall vagy a HDInsight - Azure monó frissítése |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse HDInsight-fürthöz monó egy adott verziójához. Monó a .NET-alkalmazásokban használt toorun a Linux-alapú HDInsight-fürtökön."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: hdinsightactive
ms.openlocfilehash: 1e8a8aaeff231c93a9e232379448517b326da965
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-or-update-mono-on-hdinsight"></a>Telepítse vagy frissítse a HDInsight monó

Megtudhatja, hogyan tooinstall egy adott verziójához a [monó](https://www.mono-project.com) HDInsight 3.4-es vagy újabb rendszerre.

Monó HDInsight 3.4-es és újabb rendszer van telepítve, és használt toorun .NET-alkalmazások. Monó részét képező egyes HDInsight-verzió hello verzióján információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md). egy másik verziót a fürtben, ez a dokumentum használatát hello parancsfájlművelet tooinstall. 

## <a name="how-it-works"></a>Működés

Ez a parancsfájl fogadja el a következő paraméter hello:

* __Monó verziószáma__: monó tooinstall hello verzióját. hello verzió elérhetőnek kell lennie [https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).

hello parancsfájl a következő monó csomagok hello telepíti:

* __Monó befejezése__

* __CA-tanúsítványok-monó__

## <a name="hello-script"></a>hello parancsfájl

__Parancsfájl-hely__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)

__Követelmények__:

* hello parancsfájl kell alkalmazni a hello __átjárócsomópontokat__ és __munkavégző csomópontokhoz__.

## <a name="toouse-hello-script"></a>toouse hello parancsfájl

Információk hogyan toouse ezt a parancsfájlt a hdinsight eszközzel, lásd: hello [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentum. Hello parancsfájllal hello Azure-portálon az Azure PowerShell használatával, vagy az Azure parancssori felület hello.

Közben a következő parancsfájl műveleti dokumentum hello, használja a következő URI hello:

    https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash

> [!NOTE]
> Ezt a parancsfájlt a HDInsight konfigurálásakor jelölje meg hello parancsprogramot __megőrzött__. Ez a beállítás lehetővé teszi, hogy a HDInsight tooapply hello parancsfájl tooworker csomópontok műveletek a méretezés során hozzáadott.


## <a name="next-steps"></a>Következő lépések

Megtanulta, hogyan tooupgrade vagy monó adott verziójának telepítése a hdinsight platformon. A .NET-alkalmazások használata a HDInsight monó további információkért tekintse meg a következő dokumentumok hello:

* [Az adatfolyamként történő MapReduce a HDInsight .NET használata](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [A Hive és a Pig a HDInsight .NET használata](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)
* [A HDInsight alatt futó Storm a C# megoldások fejlesztése](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [Telepítse át a .NET-megoldások tooLinux-alapú HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md)

Parancsfájlműveletek használatával kapcsolatos további információkért lásd: [testreszabása Linux-alapú HDInsight-fürtök parancsfájlművelet használatával](hdinsight-hadoop-customize-cluster-linux.md)