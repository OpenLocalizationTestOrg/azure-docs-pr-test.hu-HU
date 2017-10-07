---
title: "aaaUse MapReduce és a Hadoop - Azure HDInsight PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse PowerShell tooremotely hibaüzenettel MapReduce-feladatok Hadoop on HDInsight."
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
ms.openlocfilehash: 59524f0e8813d4c017f92bccb2e50d4c018acf71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>A PowerShell használatával HDInsight Hadoop MapReduce-feladatok futtassa

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

A dokumentum a egy Hadoop MapReduce feladatot biztosít Azure PowerShell toorun használatának példája a HDInsight-fürthöz.

## <a id="prereq"></a>Előfeltételek

* **(A HDInsight Hadoop) Azure HDInsight-fürtök**

  > [!IMPORTANT]
  > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Munkaállomás Azure PowerShell-lel**.

## <a id="powershell"></a>A MapReduce feladatot az Azure PowerShell használatával futtassa

Az Azure PowerShell biztosít *parancsmagok* , amelyek lehetővé teszik MapReduce-feladatok futtatása tooremotely a hdinsight platformon. Belsőleg, mindez túl REST-hívások segítségével[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (korábbi nevén lépni a Templeton) futó hello HDInsight-fürthöz.

hello következő parancsmagok használhatók egy távoli HDInsight-fürt MapReduce-feladatok futásakor.

* **Login-AzureRmAccount**: hitelesíti az Azure PowerShell tooyour Azure-előfizetés.

* **Új AzureRmHDInsightMapReduceJobDefinition**: létrehoz egy új *definition feladat* hello segítségével megadott MapReduce információkat.

* **Start-AzureRmHDInsightJob**: hello feladat definition tooHDInsight küld, hello feladat elindul, és adja vissza egy *feladat* objektum, amely használt toocheck hello hello feladat állapota lehet.

* **Várjon, amíg-AzureRmHDInsightJob**: hello objektum toocheck hello feladatállapot hello feladat használja. Arra vár, amíg hello feladat befejeződik, vagy hello várakozási ideje lejár.

* **Get-AzureRmHDInsightJobOutput**: hello feladat eredményének tooretrieve hello használt.

hello következő lépések bemutatják, hogyan toouse ezen parancsmagok toorun egy feladat a HDInsight-fürthöz.

1. Egy szerkesztővel, mentse a következő kódot hello **mapreducejob.ps1**.

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]

2. Nyisson meg egy új **Azure PowerShell** parancssort. Hello könyvtárak toohello módosítani **mapreducejob.ps1** fájlt, majd a következő parancsfájl toorun hello hello használata:

        .\mapreducejob.ps1

    Hello parancsprogram futtatásakor hello hello HDInsight-fürt nevét és a hello HTTPS/rendszergazda fiók nevét és a jelszó hello fürt kéri. Azure-előfizetés. kért tooauthenticate tooyour is lehet.

3. Hello feladat befejeződik, a szöveg a következő kimeneti hasonló toohello jelenhet meg:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    A kimenet hello feladat sikeresen befejeződött.

    > [!NOTE]
    > Ha hello **ExitCode** értéke csak 0, lásd: [hibaelhárítás](#troubleshooting).

    Ebben a példában is tárolja a letöltött hello fájlok tooan **kimenet.txt** fájl hello hello parancsfájlt futtató.

### <a name="view-output"></a>Nézet kimeneti

Nyissa meg hello **kimenet.txt** szavak és hello feladat által előállított adatokra is egy text editor toosee hello fájlban.

> [!NOTE]
> hello kimeneti fájlokat a MapReduce-feladatok nem módosíthatók. Így ha ez a minta újrafuttatásához kell toochange hello hello kimeneti fájl nevét.

## <a id="troubleshooting"></a>Hibaelhárítás

Ha nem áll rendelkezésre információ ad vissza, ha hello feladat befejeződik, egy meghibásodott feldolgozása során. Ez a feladat információi tooview hiba hozzáadása a következő parancs toohello vége hello hello **mapreducejob.ps1** fájl, mentse, majd futtassa újból.

```powershell
# Print hello output of hello WordCount job.
Write-Host "Display hello standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Ez a parancsmag írt tooSTDERR hello kiszolgálón hello feladat futtatásakor hello-adatait adja vissza, és segíthet meghatározni, miért hello feladat sikertelen.

## <a id="summary"></a>Summary (Összefoglalás)

Ahogy látja, Azure PowerShell Ez egy egyszerű módot toorun MapReduce-feladatok egy HDInsight-fürthöz, a figyelő hello feladat állapotát, és a lekérése hello kimeneti.

## <a id="nextsteps"></a>Következő lépések

Általános információk a hdinsight MapReduce-feladatok:

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)
* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)
