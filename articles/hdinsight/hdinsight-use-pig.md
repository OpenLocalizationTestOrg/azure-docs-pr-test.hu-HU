---
title: hdinsight Hadoop Pig aaaUse |} Microsoft Docs
description: "Megtudhatja, hogyan toouse a HDInsight hadooppal sertésfelmérés."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: acfeb52b-4b81-4a7d-af77-3e9908407404
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: 90850f2c742b8954c66ce277127e01b14fc3906f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-pig-with-hadoop-on-hdinsight"></a>A Pig használata a HDInsight Hadoop

Megtudhatja, hogyan toouse [Apache Pig](http://pig.apache.org/) a hdinsight eszközzel...

A Pig egy platform programok létrehozásához a Hadoop által ismert eljárási nyelv használatával *Pig Latin*. A Pig egy alternatív tooJava létrehozásához *MapReduce* megoldások, és az Azure HDInsight része. A következő tábla toodiscover hello módszert arról, hogy használható-e a Pig with HDInsight hello használata:

| **Ezzel** Ha azt szeretné... | .. .an **interaktív** rendszerhéj | ... **kötegelt** feldolgozása | és mivel ez **fürt operációs rendszer** | .. .from ez **ügyfél operációs rendszer** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](hdinsight-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X vagy Windows |
| [REST API](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux- vagy Windows |Linux, Unix, Mac OS X vagy Windows |
| [.NET SDK a Hadoophoz](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux- vagy Windows |(Egyelőre) Windows |
| [A Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux- vagy Windows |Windows |
| [A távoli asztal](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2-es és 3.3-as) |✔ |✔ |Windows |Windows |

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="why"></a>Miért érdemes használni a Pig

A feldolgozó logika csak térkép és a reduce függvény használatával végrehajtó egyik hello kihívásai adatfeldolgozás MapReduce a Hadoop használatával. Gyakran összetett feldolgozási nincs több MapReduce műveleteket történő feldolgozás toobreak összeláncolt tooachieve szükséges hello eredménye.

A Pig lehetővé teszi, hogy az adatfolyamok keresztül tooproduce szükséges hello kimeneti hello átalakítások sorozataként feldolgozása toodefine.

hello Pig Latin nyelv lehetővé teszi, hogy Ön toodescribe hello adatfolyama nyers bemeneti, egy vagy több átalakítások, tooproduce szükséges hello kimeneti keresztül. A Pig Latin programok kövesse az ebben a mintában általános:

* **Betöltési**: hello fájlrendszerből választéka adatok toobe olvasása

* **Átalakítás**: kezelhetők hello adatok

* **Memóriakép, vagy tárolja**: kimeneti adatok toohello képernyőjén, vagy tárolja el azt a feldolgozás

### <a name="user-defined-functions"></a>Felhasználó által definiált függvények

A Pig Latin is támogatja a felhasználói függvény (UDF), amely lehetővé teszi a Pig Latin nehéz toomodel logikát megvalósító tooinvoke külső összetevők.

A Pig Latin kapcsolatos további információkért lásd: [Pig Latin hivatkozás manuális 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) és [Pig Latin hivatkozás manuális 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Például egy felhasználó által megadott függvények használata a Pig tekintse meg a következő dokumentumok hello:

* [DataFu használata a Hdinsightban Pig](hdinsight-hadoop-use-pig-datafu-udf.md) -DataFu hasznos felhasználó által megadott függvények Apache által fenntartott gyűjteménye
* [Python az Pig és a Hive HDInsight használata](hdinsight-python.md)
* [C# az Hive és a hdinsight a Pig használata](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

## <a id="data"></a>Példa adatok

A HDInsight lehetővé különböző példa adatkészletek, hello tárolt `/example/data` és `/HdiSamples` könyvtárak. Ezeket a könyvtárakat hello alapértelmezett tárolási a fürt szerepelnek. hello Pig ebben a dokumentumban példában hello *log4j* fájlként `/example/data/sample.log`.

Egyes naplókon belül hello fájl tartalmaz egy mezőket tartalmazó sor egy `[LOG LEVEL]` tooshow hello típusát és hello súlyossági, például mezőben:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Hello előző példában hello naplózási szint: Hiba történt.

> [!NOTE]
> Hello segítségével is létrehozhat egy log4j fájl [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) eszköz naplózása és az, hogy a fájl tooyour blob majd feltölteni. Lásd: [adatok feltöltése tooHDInsight](hdinsight-upload-data.md) utasításokat. Az Azure storage blobs használata a hdinsight eszközzel kapcsolatos további információkért lásd: [az Azure Blob Storage a hdinsight eszközzel](hdinsight-hadoop-use-blob-storage.md).

## <a id="job"></a>Példa feladat

hello következő Pig Latin feladat betölti hello `sample.log` hello alapértelmezett tárolási fájlból a HDInsight-fürthöz. Végrehajtja eredményező hogyan számítja sokszor minden naplózási szint keletkezett hello bemeneti adatok átalakítások sorozata. hello eredmények tooSTDOUT készültek.

    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

hello következő kép bemutatja, milyen minden átalakítása does toohello adatok összegzését.

![Hello átalakítások grafikus ábrázolása][image-hdi-pig-data-transformation]

## <a id="run"></a>Hello Pig Latin feladat futtatása

A Pig Latin feladatok futtatásával a HDInsight többféle módszerrel. Használja a következő tábla toodecide, melyik módszert részesíti az Ön számára legmegfelelőbb hello, majd kövesse az útmutatást hello hivatkozásra.

| **Ezzel** Ha azt szeretné... | .. .an **interaktív** rendszerhéj | ... **kötegelt** feldolgozása | és mivel ez **fürt operációs rendszer** | .. .from ez **ügyfél** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](hdinsight-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X vagy Windows |
| [Curl](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux- vagy Windows |Linux, Unix, Mac OS X vagy Windows |
| [.NET SDK a Hadoophoz](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux- vagy Windows |(Egyelőre) Windows |
| [A Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux- vagy Windows |Windows |
| [A távoli asztal](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2-es és 3.3-as) |✔ |✔ |Windows |Windows |

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="pig-and-sql-server-integration-services"></a>A Pig és az SQL Server Integration Services

SQL Server Integration Services (SSIS) toorun a Pig feladatot is használhatja. hello Azure funkciócsomag SSIS biztosít, amelyek használhatók a Pig-feladatokhoz a HDInsight összetevők a következő hello.

* [Az Azure HDInsight a Pig feladatot][pigtask]

* [Az Azure előfizetés Csatlakozáskezelő][connectionmanager]

További információk hello Azure funkciócsomag SSIS [Itt][ssispack].

## <a id="nextsteps"></a>Következő lépések
Most, hogy megtanulta, hogyan toouse Pig with HDInsight, a következő használatát hello hivatkozásait tooexplore más módokon toowork Azure hdinsightban.

* [Adatok tooHDInsight feltöltése][hdinsight-upload-data]
* [A Hive használata a HDInsightban][hdinsight-use-hive]
* [Use Sqoop with HDInsight](hdinsight-use-sqoop.md)
* [Oozie használata a hdinsight eszközzel](hdinsight-use-oozie.md)
* [MapReduce-feladatok használata a hdinsight eszközzel][hdinsight-use-mapreduce]

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx


[hdinsight-upload-data]: hdinsight-upload-data.md

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx


[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
