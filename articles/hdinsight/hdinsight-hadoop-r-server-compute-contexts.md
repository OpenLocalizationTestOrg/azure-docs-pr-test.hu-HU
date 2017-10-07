---
title: "a HDInsight - Azure R Server aaaCompute adatkörnyezet beállításai |} Microsoft Docs"
description: "Hello különböző számítási környezet beállítások elérhető toousers R Server on HDInsight megismerése"
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0deb0b1c-4094-459b-94fc-ec9b774c1f8a
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: b3b0d0cc3caa390797dcff8c73d66cd3ad78bcaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="compute-context-options-for-r-server-on-hdinsight"></a>Számítási környezet lehetőségek R Server on HDInsight

A Microsoft az R Server on Azure HDInsight szabályozza hívások végrehajtásának módját a beállítás hello számítási környezet által. Ez a cikk ismerteti, amelyek e elérhető toospecify hello-beállítások, és hogyan végrehajtási hello élcsomópont vagy HDInsight-fürt magokon párhuzamos működésű.

hello élcsomópont fürt kényelmesen tooconnect toohello fürt és biztosít toorun az R parancsfájlok. Egy élcsomópontot az informatikai részleg hello lehetőség a párhuzamos működésű futó hello elosztott feladatai ScaleR hello mag hello peremhálózati csomópont kiszolgáló között. Is futtathatja őket hello hello fürt csomópontjai között ScaleR a Hadoop térkép csökkentheti vagy Spark számítási környezeteket.

## <a name="microsoft-r-server-on-azure-hdinsight"></a>A Microsoft az R Server on Azure HDInsight
[A Microsoft az R Server on Azure HDInsight](hdinsight-hadoop-r-server-overview.md) hello legújabb képességeket biztosít az R-alapú használatával. Egy a HDFS-tárolóban tárolt adatokat képes használni a [Azure Blob](../storage/common/storage-introduction.md "Azure Blob Storage tárolóban") tárfiókot, egy Data Lake store vagy a helyi Linux fájlrendszer hello. R Server nyílt forráskódú R épül, mivel hello R-alapú alkalmazások létrehozása és hello 8000 + nyílt forráskódú R csomagok bármelyikét alkalmazhatja. Szolgáltatást is alkalmazhatja hello rutinjai [RevoScaleR](https://msdn.microsoft.com/microsoft-r/scaler/scaler), a Microsoft big data elemzés csomag R Server részét képező.  

## <a name="compute-contexts-for-an-edge-node"></a>Számítási környezetek számára egy élcsomópontot
R Server hello peremhálózati csomóponton futó R-parancsfájl általában hello R parancsértelmező belül fut, ezen a csomóponton. hello kivételek olyan ScaleR függvény témakör lépéseit. hello ScaleR hívások határozza meg, hogyan állíthatja hello ScaleR számítási környezet számítási környezetben fusson.  Az R-parancsfájl egy élcsomópontot történő futtatásakor a lehetséges értékei hello hello számítási környezetben vannak:

- szekvenciális helyi (*"local"*)
- helyi párhuzamos (*"localpar"*)
- Térkép csökkentése
- Spark

Hello *"local"* és *"localpar"* más-más lehetőségek csak a hogyan **rxExec** hívások végrehajtása. Mindkettő végrehajtani más rx függvényhívások párhuzamos módon összes elérhető mag között másképp hello ScaleR használata **numCoresToUse** beállítás, például `rxOptions(numCoresToUse=6)`. Párhuzamos végrehajtás beállítások optimális teljesítményt nyújtanak.

hello a következő táblázat összefoglalja a különböző számítási környezet beállítások tooset hívások végrehajtásának módját hello:

| Számítási környezet  | Hogyan tooset                      | A végrehajtási környezet                        |
| ---------------- | ------------------------------- | ---------------------------------------- |
| Szekvenciális helyi | rxSetComputeContext('local')    | Párhuzamos működésű végrehajtásának hello magokon hello biztonsági csomópont kiszolgálón kivételével Feladattervek végrehajtásának rxExec hívások |
| Helyi párhuzamos   | rxSetComputeContext('localpar') | Párhuzamos működésű végrehajtásának hello magokon hello biztonsági csomópont kiszolgáló |
| Spark            | RxSpark()                       | Párhuzamos működésű elosztott végrehajtásának keresztül hello csomópontjai között hello HDI-fürtöt |
| Térkép csökkentése       | RxHadoopMR()                    | Párhuzamos működésű elosztott végrehajtásának keresztül térkép csökkentése hello csomópontjai között hello HDI-fürtöt |

## <a name="guidelines-for-deciding-on-a-compute-context"></a>A számítási környezet meg vonatkozó irányelvek

Hello választja, adja meg a végrehajtási párhuzamos működésű három lehetőség közül melyiket hello jellegét a analytics munka, hello mérete és az adatok helye hello függ. Nem egyszerű képlet, amely azt ismerteti, amelyek számítási környezet toouse van. Vannak, azonban néhány alapelvek, amelyek segítségével hello jobb választás, vagy legalább, szűkítheti a választási lehetőségek egy teljesítményteszt futtatása előtt. Ezek az irányadó elvek a következők:

- hello helyi Linux fájlrendszere gyorsabb, mint a HDFS.
- Ismétlődő elemzések gyorsabbak, ha hello adatok helyi, és ha XDF.
- Akkor érdemes toostream kis mennyiségű szöveg adatforrásból származó adatokat. Ha az adatok hello összege nagyobb, alakítsa tooXDF elemzés előtt.
- hello terhelését, másolása és adatfolyam-hello adatok toohello élcsomóponthoz elemzéshez válik Kezelhetetlen nagyon nagy mennyiségű adat.
- A Spark gyorsabb, mint az térkép elemzése a Hadoop csökkenti az.

Megadott alapelvek, hello alábbiakban kínál néhány általános szabályok megoldás a számítási környezet kijelöléséhez.

### <a name="local"></a>Helyi
* Ha adatok tooanalyze hello mennyisége kicsi, és nem igényel ismételt elemzése, majd adatfolyamként közvetlenül a hello elemzés rutin *"local"* vagy *"localpar"*.
* Ha hello mennyisége adatok tooanalyze kis és közepes méretű és ismételt elemzése szükséges, majd másolja toohello helyi fájlrendszer, importálja a tooXDF, és elemezze keresztül *"local"* vagy *"localpar"*.

### <a name="hadoop-spark"></a>Hadoop Spark
* Ha az adatok tooanalyze hello mérete nagy, majd importálhatja azokat tooa Spark DataFrame használatával **RxHiveData** vagy **RxParquetData**, vagy tooXDF HDFS-ben (kivéve, ha probléma a tároló), és elemezze a Spark hello használata a számítási környezet.

### <a name="hadoop-map-reduce"></a>Hadoop térkép csökkentése
* Hello térkép csökkentése számítási környezet használata, csak akkor, ha a hello Spark számítási környezet módon hibát tapasztal, mivel általában lassabban működik.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Beágyazott súgójának rxSetComputeContext
További információt és példákat a ScaleR számítási környezetek című hello beágyazott megkönnyíti a R hello rxSetComputeContext metódus, például:

    > ?rxSetComputeContext

Is érdemes toohello "[ScaleR elosztott számítástechnikai útmutató](https://msdn.microsoft.com/microsoft-r/scaler-distributed-computing)", amely megtalálható a hello [R Server MSDN](https://msdn.microsoft.com/library/mt674634.aspx "R Server, az MSDN Webhelyén") könyvtár.

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, amelyek a rendelkezésre álló toospecify hello lehetőségeivel kapcsolatos, hogyan végrehajtási párhuzamos működésű-e mag hello élcsomópont vagy HDInsight-fürt között. toolearn hogyan toouse R Server a HDInsight-fürtök, kapcsolatos további információkért tekintse meg a következő témakörök hello:

* [R Server, a Hadoop – áttekintés](hdinsight-hadoop-r-server-overview.md)
* [R Server a Hadoop első lépései](hdinsight-hadoop-r-server-get-started.md)
* [Adja hozzá a Rstudióból Server tooHDInsight (Ha nem adja hozzá a fürt létrehozása során)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Azure Storage lehetőségek a HDInsighton belüli R Server esetében](hdinsight-hadoop-r-server-storage.md)

