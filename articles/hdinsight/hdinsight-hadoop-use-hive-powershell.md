---
title: "aaaUse Hadoop Hive hdinsight - Azure PowerShell használatával |} Microsoft Docs"
description: "A hdinsight Hadoop PowerShell toorun Hive-lekérdezéseket használhat."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cb795b7c-bcd0-497a-a7f0-8ed18ef49195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: 9e0b72a25c5b12431f837b1a34a63ecc06223528
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-powershell"></a>PowerShell-lel Hive-lekérdezések futtatása
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Ez a dokumentum egy példát az hello Azure erőforráscsoport mód toorun Hive-lekérdezéseket a Hadoop on HDInsight-fürt az Azure PowerShell használatával.

> [!NOTE]
> Ez a dokumentum nem biztosít részletes leírása a hello HiveQL utasítások hello példákban használt tegye. Ebben a példában használt HiveQL hello információkért lásd: [használata a hdinsight Hadoop Hive](hdinsight-use-hive.md).

**Előfeltételek**

* **Egy Azure HDInsight fürt**: nem számít, hogy az hello fürt Windows vagy Linux-alapú.

  > [!IMPORTANT]
  > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Munkaállomás Azure PowerShell-lel**.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-hive-queries-using-azure-powershell"></a>Az Azure PowerShell Hive-lekérdezések futtatása

Az Azure PowerShell biztosít *parancsmagok* , amelyek lehetővé teszik Hive-lekérdezések futtatása tooremotely a hdinsight platformon. Belsőleg, hello parancsmagok hívások REST túl[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) hello HDInsight-fürtre.

hello alábbi parancsmagok használata, amikor egy távoli HDInsight-fürt Hive-lekérdezések futtatása:

* **Adja hozzá-AzureRmAccount**: hitelesíti az Azure PowerShell tooyour Azure-előfizetés
* **Új AzureRmHDInsightHiveJobDefinition**: létrehoz egy *definition feladat* hello segítségével megadott HiveQL utasítások
* **Start-AzureRmHDInsightJob**: hello feladat definition tooHDInsight küld, hello feladat elindul, és adja vissza egy *feladat* objektum, amely lehet használt toocheck hello hello feladat állapota
* **Várjon, amíg-AzureRmHDInsightJob**: hello objektum toocheck hello feladatállapot hello feladat használja. Arra vár, amíg hello feladat befejeződik, vagy hello várakozási ideje lejár.
* **Get-AzureRmHDInsightJobOutput**: hello feladat eredményének tooretrieve hello használt
* **Invoke-AzureRmHDInsightHiveJob**: toorun HiveQL utasítás használható. Ez a parancsmag blokkok hello lekérdezés befejeződött, majd hello eredményt ad vissza.
* **Használjon-AzureRmHDInsightCluster**: készletek hello hello az aktuális fürt toouse **Invoke-AzureRmHDInsightHiveJob** parancs

hello következő lépések bemutatják, hogyan toouse ezen parancsmagok toorun egy feladat a HDInsight-fürt:

1. Egy szerkesztővel, mentse a következő kódot hello **hivejob.ps1**.

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. Nyisson meg egy új **Azure PowerShell** parancssort. Hello könyvtárak toohello módosítani **hivejob.ps1** fájlt, majd a következő parancsfájl toorun hello hello használata:

        .\hivejob.ps1

    Hello parancsprogram futtatásakor felszólító tooenter hello fürt nevét és hello HTTPS/rendszergazdai fiók hitelesítő adatait hello fürt áll. Az Azure-előfizetés tooyour rákérdezéses toolog is lehet.

3. Hello feladat befejezése után információkat a következő thext hasonló toohello adja vissza:

        Display hello standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. A korábbiak **Invoke-struktúra** is használt toorun lekérdezés lehet és hello válaszra. A következő parancsfájl toosee Invoke-struktúra működése hello használata:

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    hello kimenete a következő szöveg hello néz ki:

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > Hosszabb HiveQL lekérdezések esetén használhatja hello Azure PowerShell **ide-karakterláncok** parancsmag vagy a HiveQL parancsfájlok. a következő kódrészletet mutat be hogyan hello toouse hello **Invoke-struktúra** parancsmag toorun HiveQL-parancsfájlt. hello HiveQL-parancsfájlt kell feltöltött toowasb: / /.
   >
   > `Invoke-AzureRmHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > További információ **ide-karakterláncok**, lásd: <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">használata a Windows PowerShell ide-karakterláncok</a>.

## <a name="troubleshooting"></a>Hibaelhárítás

Ha nem áll rendelkezésre információ ad vissza, ha hello feladat befejeződik, egy meghibásodott feldolgozása során. Ez a feladat információi tooview hiba hozzáadása hello hello toohello végét a következő **hivejob.ps1** fájl, mentse, majd futtassa újból.

```powershell
# Print hello output of hello Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Ez a parancsmag hello írt információ tooSTDERR hello kiszolgálón hello feladat futtatásakor adja vissza.

## <a name="summary"></a>Összefoglalás

Ahogy látja, Azure PowerShell biztosít egy egyszerűen toorun Hive-lekérdezéseket a HDInsight-fürtöt, a figyelő hello feladat állapota, hello kimeneti beolvasása.

## <a name="next-steps"></a>Következő lépések

Általános információk a hdinsight Hive:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)

Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:

* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)
* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)
