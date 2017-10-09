---
title: a hdinsight - Azure PowerShell Hadoop Pig aaaUse |} Microsoft Docs
description: "Ismerje meg, hogyan toosubmit Pig feladatok tooa Hadoop on HDInsight az Azure PowerShell fürt."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 737089c1-b494-4387-9def-7b4dac3be532
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 771617df203011eaec715a0dba6f5014a42877f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-powershell-toorun-pig-jobs-with-hdinsight"></a>Azure PowerShell toorun Pig-feladatokhoz és a HDInsight együttes használata

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Ez a dokumentum egy példát az Azure PowerShell toosubmit Pig feladatok tooa Hadoop on HDInsight-fürt használatával. A Pig lehetővé teszi egy nyelv (a Pig latin betűs) adatátalakítást, modellek segítségével toowrite MapReduce-feladatok helyett képezi le, és csökkentheti a funkciók.

> [!NOTE]
> Ez a dokumentum nem biztosít részletes leírása a hello Pig Latin utasítások hello a példákban szereplő tegye. Ebben a példában használt Pig Latin hello kapcsolatos információkért lásd: [a Pig használata a hdinsight Hadoop](hdinsight-use-pig.md).

## <a id="prereq"></a>Előfeltételek

* **Egy Azure HDInsight-fürt**

  > [!IMPORTANT]
  > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Munkaállomás Azure PowerShell-lel**.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a id="powershell"></a>Futtassa a PowerShell használatával Pig-feladatokhoz

Az Azure PowerShell biztosít *parancsmagok* , amelyek lehetővé teszik futtatása tooremotely Pig-feladatokhoz a HDInsight-on. Belső, a PowerShell használ REST-hívások túl[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) hello HDInsight-fürtön futó.

hello következő parancsmagok használhatók egy távoli HDInsight-fürt Pig-feladatokhoz futtatásakor:

* **Login-AzureRmAccount**: hitelesíti az Azure PowerShell tooyour Azure-előfizetés
* **Új AzureRmHDInsightPigJobDefinition**: létrehoz egy *definition feladat* hello segítségével megadott Pig Latin utasítások
* **Start-AzureRmHDInsightJob**: hello feladat definition tooHDInsight küld, hello feladat elindul, és adja vissza egy *feladat* objektum, amely lehet használt toocheck hello hello feladat állapota
* **Várjon, amíg-AzureRmHDInsightJob**: hello objektum toocheck hello feladatállapot hello feladat használja. Arra vár, amíg hello feladat befejeződött, vagy hello várakozási idő túl lett lépve.
* **Get-AzureRmHDInsightJobOutput**: hello feladat eredményének tooretrieve hello használt

hello következő lépések bemutatják, hogyan toouse ezen parancsmagok toorun egy feladatot a HDInsight-fürtre.

1. Egy szerkesztővel, mentse a következő kódot hello **pigjob.ps1**.

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]

1. Nyisson meg egy új Windows PowerShell parancssort. Hello könyvtárak toohello módosítani **pigjob.ps1** fájlt, majd a következő parancsfájl toorun hello hello használata:

        .\pigjob.ps1

    Az Azure-előfizetés tooyour rákérdezéses toolog áll. Ezt követően felkérést hello HTTPs/rendszergazdai fiók felhasználónevét és jelszavát hello HDInsight-fürthöz.

2. Hello feladat befejezése után az információkat a következő szöveg hasonló toohello kell visszaadnia:

        Start hello Pig job ...
        Wait for hello Pig job toocomplete ...
        Display hello standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="troubleshooting"></a>Hibaelhárítás

Ha nem áll rendelkezésre információ ad vissza, ha hello feladat befejeződik, egy meghibásodott feldolgozása során. Ez a feladat információi tooview hiba hozzáadása a következő parancs toohello vége hello hello **pigjob.ps1** fájl, mentse, majd futtassa újból.

    # Print hello output of hello Pig job.
    Write-Host "Display hello standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Írt tooSTDERR hello kiszolgálón hello feladat futtatásakor hello adatait adja vissza. Ez, és segíthet meghatározni, miért hello feladat sikertelen.

## <a id="summary"></a>Summary (Összefoglalás)
Ahogy látja, Azure PowerShell Ez egy egyszerű módot toorun Pig-feladatokhoz egy HDInsight-fürthöz, a figyelő hello feladat állapotát, és a lekérése hello kimeneti.

## <a id="nextsteps"></a>Következő lépések
Általános információk a hdinsight Pig:

* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)

Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)
* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)
