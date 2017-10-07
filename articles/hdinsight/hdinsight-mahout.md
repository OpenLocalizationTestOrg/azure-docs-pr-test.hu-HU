---
title: "PowerShell - Azure Mahout HDInsight segítségével aaaGenerate javaslatok |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Apache Mahout gépi tanulási a szalagtár toogenerate movie javaslatok és a HDInsight (Hadoop) együttes az ügyfélszámítógépen futó PowerShell parancsfájl."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 07b57208-32aa-4e59-900a-6c934fa1b7a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: 675a2cd8ecaf7fc797d6cd094e4e58f9aca7ed92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight-powershell"></a>Film javaslatok generálása Apache Mahout a hadooppal a Hdinsightban (PowerShell)

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Megtudhatja, hogyan toouse hello [Apache Mahout](http://mahout.apache.org) machine learning függvénytár Azure HDInsight toogenerate movie javaslatokat. hello ebben a dokumentumban példa Azure PowerShell toorun Mahout feladatok.

## <a name="prerequisites"></a>Előfeltételek

* A Linux-alapú HDInsight-fürtöt. Egy létrehozásával kapcsolatos további információkért lásd: [hdinsight Linux-alapú Hadoop használatának megkezdésében][getstarted].

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Azure PowerShell](/powershell/azure/overview)

## <a name="recommendations"></a>Javaslatok generálása Azure PowerShell használatával

> [!WARNING]
> Ebben a szakaszban hello feladat Azure PowerShell használatával működik. Mahout megadott hello osztályok többsége nem jelenleg Azure PowerShell-lel dolgozni. Nem működnek az Azure PowerShell osztályok listáját lásd: hello [hibaelhárítás](#troubleshooting) szakasz.
>
> Az SSH tooconnect tooHDInsight és futtatási Mahout példák segítségével közvetlenül hello fürtön példáért lásd: [Mahout és a HDInsight-(SSH) használatával movie javaslatok generálása](hdinsight-hadoop-mahout-linux-mac.md).

Mahout által biztosított hello függvények egyike egy javaslat motor. Ez a motor hello formátumban adatokat fogad `userID`, `itemId`, és `prefValue` (hello hello elem felhasználók beállításait). Mahout hello adatok toodetermine felhasználók hasonló-cikk beállítások, amelyek használt toomake javaslatok is használ.

hello következő példája egy egyszerűsített segédlet hello javaslat folyamat működését:

* **közös előfordulási**: Joe, Ágnes és minden tetszését Bob *csillag ütközések*, *Empire sztrájkok vissza hello*, és *Return a hello Jedi*. Mahout határozza meg, hogy a felhasználók, akik például ezek filmek egyikét sem is, például más két hello.

* **közös előfordulási**: Bálint és Alice is tetszett *látszólagos támadása hello*, *hello klónja támadás*, és *megtorlás hello Sith a*. Mahout határozza meg, hogy felhasználók, akik hello előző három filmek is tetszett, például a filmek.

* **Hasonlóság ajánlás**: mivel Joe tetszését hello első három filmek, Mahout ellenőrzi, hogy az filmek, hogy más, hasonló beállítások tetszett, de Joe rendelkezik nem figyelt (tetszését/névleges). Ebben az esetben Mahout javasolja *látszólagos támadása hello*, *támadás hello klónja*, és *a hello Sith megtorlás*.

### <a name="understanding-hello-data"></a>Hello adatok ismertetése

[GroupLens kutatási] [ movielens] minősítés adatokat biztosít a filmek formátuma nem kompatibilis a Mahout. Hello alapértelmezett tároló, a fürt számára érhetők el az adatok `/HdiSamples//HdiSamples/MahoutMovieData`.

Két fájlt `moviedb.txt` (hello filmek információ) és `user-ratings.txt`. Hello `user-ratings.txt` fájllal elemzése során. Hello `moviedb.txt` fájl használt tooprovide felhasználóbarát szöveg hello elemzés eredményeinek hello megjelenítésekor.

hello felhasználói-ratings.txt szereplő adatok rendelkezik egy szerkezete `userID`, `movieID`, `userRating`, és `timestamp`, amely közli a gép magas hogyan minden felhasználó besorolású film. Íme egy példa a hello adatok:

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

### <a name="run-hello-job"></a>Hello feladat futtatása

A következő Windows PowerShell parancsfájl toorun egy feladatot, amely hello Mahout javaslat motort használja, hello movie adatokkal hello használata:

> [!NOTE]
> Ez a fájl információkat kér, amely használt tooconnect tooyour HDInsight-fürtöt és futtatási feladatok. Ez a hello feladatok toocomplete néhány percet igénybe vehet, és hello kimenet.txt fájl letöltése.

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=5-98)]

> [!NOTE]
> Mahout feladatok hello feladat feldolgozása közben létrehozott ideiglenes adatok nem távolítja el. Hello `--tempDir` paraméter megadott hello példa tooisolate hello ideiglenes feladatfájlokhoz. egy adott könyvtárba.

hello Mahout feladat hello kimeneti tooSTDOUT nem ad vissza. Ehelyett azt tárolja hello megadott kimeneti könyvtár, **rész-r-00000**. hello parancsfájl letölti a fájl túl**kimenet.txt** hello aktuális könyvtárban a munkaállomáson.

hello következő szöveg látható egy példa a fájl tartalma hello:

    1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

hello első oszlopa hello `userID`. hello található értékek ' ["és"] "vannak `movieId`:`recommendationScore`.

hello parancsfájlt is letölti hello `moviedb.txt` és `user-ratings.txt` fájlokat, amelyek szükséges tooformat hello kimeneti toobe olvashatóbbá.

### <a name="view-hello-output"></a>Nézet hello kimeneti

Bár hello létrehozott kimeneti lehetnek OK alkalmazásban használható, nincs felhasználóbarát. Hello `moviedb.txt` hello kiszolgáló lehet használt tooresolve hello `movieId` tooa movie nevét. A következő PowerShell parancsfájl toodisplay javaslatok movie nevű hello használata:

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=106-180)]

A következő parancs toodisplay hello javaslatok felhasználóbarát formátumban hello használata: 

```powershell
.\show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt
```

a kimeneti hello hasonló toohello a következő szöveget:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, hello (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of hello Dove, hello (1997)            4.6666665
    People vs. Larry Flynt, hello (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="cannot-overwrite-files"></a>Fájlok nem írható felül.

Mahout feladatok adatformátuma nem karbantartása feldolgozás során létrehozott ideiglenes fájlokat. Ezenkívül hello feladatok fájl felülírásának mellőzése meglévő kimeneti.

Mahout feladatok futtatásakor tooavoid hibák között futtatása ideiglenes és a kimeneti fájlok törlése. létrehozott tooremove hello fájlok hello a jelen dokumentum korábbi parancsfájlok használja a következő PowerShell-parancsfájl hello:

```powershell
# Login tooyour Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter hello HDInsight cluster name"
$creds=Get-Credential -Message "Enter hello login for hello cluster"

#Get hello cluster info so we can get hello resource group, storage, etc.
$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
$container = $clusterInfo.DefaultStorageContainer
$storageAccountKey = (Get-AzureRmStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage context and upload hello file
$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

#Azure PowerShell can't delete blobs using wildcard,
#so have tooget a list and delete one at a time
# Start with hello output
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/out"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
# Next hello temp files
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/temp"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
```

### <a name="nopowershell"></a>Olyan osztállyal, amelynek nem működnek az Azure PowerShell

A következő osztályok hello használó mahout feladatok vissza különböző hibaüzenetek a Windows PowerShell használata esetén:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Ezeket az osztályokat használó toorun feladatok toohello HDInsight-fürthöz SSH használatával csatlakozzon, és futtassa a hello feladatok hello parancssorból. Például egy SSH toorun Mahout feladatok használatával, lásd: [Mahout és a HDInsight-(SSH) használatával movie javaslatok generálása](hdinsight-hadoop-mahout-linux-mac.md).

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta, hogyan toouse Mahout, felderíteni a más módon a HDInsight-adatokkal:

* [A HDInsight Hive](hdinsight-use-hive.md)
* [Pig with HDInsight](hdinsight-use-pig.md)
* [MapReduce a hdinsight eszközzel](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: /powershell/azureps-cmdlets-docs
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
