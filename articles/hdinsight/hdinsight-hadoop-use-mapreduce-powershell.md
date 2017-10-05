---
title: "MapReduce és a PowerShell használata a Hadoop - az Azure HDInsight |} Microsoft Docs"
description: "Tudnivalók a PowerShell használatával távolról ugyanúgy futtathatják a HDInsight Hadoop a MapReduce-feladatok."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21b56d32-1785-4d44-8ae8-94467c12cfba
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: c3801573808709f29cb1e563ac803f225a28cafc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>A PowerShell használatával HDInsight Hadoop MapReduce-feladatok futtassa

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Ez a dokumentum egy példát a MapReduce-feladatok futtatásához egy Hadoop on HDInsight-fürt Azure PowerShell használatával.

## <a id="prereq"></a>Előfeltételek

* **(A HDInsight Hadoop) Azure HDInsight-fürtök**

  > [!IMPORTANT]
  > A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Munkaállomás Azure PowerShell-lel**.

## <a id="powershell"></a>A MapReduce feladatot az Azure PowerShell használatával futtassa

Az Azure PowerShell biztosít *parancsmagok* , amelyek lehetővé teszik, hogy távolról a HDInsight a MapReduce-feladatok futtatását. Belsőleg, mindez REST-hívások segítségével [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (korábbi nevén lépni a Templeton) fut a HDInsight-fürthöz.

A következő parancsmagokat használja a MapReduce-feladatok futtatása egy távoli HDInsight-fürt.

* **Login-AzureRmAccount**: Azure PowerShell hitelesíti az Azure-előfizetéshez.

* **Új AzureRmHDInsightMapReduceJobDefinition**: létrehoz egy új *definition feladat* MapReduce megadott információk segítségével.

* **Start-AzureRmHDInsightJob**: a feladat definíciójához küld HDInsight, elindítja a feladatot, és adja vissza egy *feladat* objektum, amely segítségével a feladat állapotának ellenőrzése.

* **Várjon, amíg-AzureRmHDInsightJob**: a feladat állapotának ellenőrzése a feladatobjektum használja. Arra vár, amíg a feladat befejeződik, vagy a várakozási ideje lejár.

* **Get-AzureRmHDInsightJobOutput**: a feladat kimenetének beolvasása.

A következő lépések bemutatják, hogyan lehet ezeket a parancsmagokat használja a feladat futtatásához a HDInsight-fürtön.

1. Egy szerkesztővel, az alábbi kód, Mentés **mapreducejob.ps1**.

    [!code-powershell[fő](../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]

2. Nyisson meg egy új **Azure PowerShell** parancssort. Módosítsa a könyvtárat, hol található a **mapreducejob.ps1** fájlt, majd futtassa a parancsfájlt a következő paranccsal:

        .\mapreducejob.ps1

    A parancsprogram futtatásakor kéri a HDInsight-fürt nevét és a HTTPS/rendszergazda fiók nevét és a jelszót a fürthöz. Is kérheti, hogy az Azure-előfizetéshez hitelesítést.

3. A feladat befejeződik, a kimenet az alábbihoz hasonló jelenhet meg:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    A kimeneti azt jelzi, hogy a feladat sikeresen befejeződött-e.

    > [!NOTE]
    > Ha a **ExitCode** értéke csak 0, lásd: [hibaelhárítás](#troubleshooting).

    Ebben a példában a letöltött fájlokat tárolja egy **kimenet.txt** fájl a könyvtárban, amely futtatja a parancsfájlt.

### <a name="view-output"></a>Nézet kimeneti

Nyissa meg a **kimenet.txt** fájlt egy szövegszerkesztőben, a szavakat, és a feladat által létrehozott számát.

> [!NOTE]
> A MapReduce feladatot, kimeneti fájlok nem módosíthatók. Ezért ez a minta fut újra, ha módosítani szeretné a kimeneti fájl nevét.

## <a id="troubleshooting"></a>Hibaelhárítás

Ha nem áll rendelkezésre információ ad vissza, ha a feladat befejeződik, egy meghibásodott feldolgozása során. Hiba történt a feladat információinak megtekintése, vegye fel a következő parancsot végén a **mapreducejob.ps1** fájl, mentse, majd futtassa újból.

```powershell
# Print the output of the WordCount job.
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Ez a parancsmag írt ezzel a kiszolgálón a feladat futtatásakor olyan információkat ad vissza, és segíthet meghatározni, miért nem sikerült a feladat.

## <a id="summary"></a>Summary (Összefoglalás)

Ahogy látja, Azure PowerShell könnyedén MapReduce-feladatok futtatása a HDInsight-fürtöt, figyelheti a feladat állapotát és a kimeneti beolvasása.

## <a id="nextsteps"></a>Következő lépések

Általános információk a hdinsight MapReduce-feladatok:

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)
* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)
