---
title: "Data Lake Store Hive teljesítményének hangolása irányelveit aaaAzure |} Microsoft Docs"
description: "Az Azure Data Lake Store Hive teljesítményének hangolása irányelvek"
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: e44daeb6ad3b64e893c709df63b56444a330729f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-store"></a>Útmutatás a Hive HDInsight és az Azure Data Lake Store teljesítményhangolása

hello alapértelmezett beállítások állítani tooprovide a megfelelő teljesítmény számos különféle használati esetek között.  I/o-igényes lekérdezések esetén a Hive bennünket tooget jobb teljesítmény ADLS lehet.  

## <a name="prerequisites"></a>Előfeltételek

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Egy Azure Data Lake Store-fiók**. Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Az Azure HDInsight-fürt** a Data Lake Store-fiók hozzáférési tooa. Lásd: [HDInsight-fürtök létrehozása a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Ellenőrizze, hogy engedélyezte a távoli asztal hello fürt.
* **HDInsight Hive futó**.  a HDInsight Hive-feladatok futtatásával kapcsolatos toolearn lásd: [használata a HDInsight Hive] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)
* **Teljesítményhangolás ADLS iránymutatást**.  Általános teljesítmény fogalmakat, lásd: [Data Lake Store teljesítmény hangolása útmutató](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)

## <a name="parameters"></a>Paraméterek

Az alábbiakban a legfontosabb beállítások tootune hello jobb ADLS-teljesítmény:

* **Hive.tez.Container.size** – hello egyes feladatok által használt memória mennyisége

* **tez.Grouping.min méretű** – minimális méret minden leképezője

* **tez.Grouping.max méretű** – maximális méret minden leképezője

* **Hive.Exec.reducer.bytes.per.reducer** – minden nyomáscsökkentő méret

**Hive.tez.Container.size** -hello tároló mérete határozza meg, hogy mennyi memóriát minden feladat számára.  Ez a hello fő a megadott ellenőrző hello párhuzamossági struktúrában.  

**tez.Grouping.min méretű** – Ez a paraméter lehetővé teszi a tooset hello minimális mérete minden leképező.  Ha mappers, amely Tez kiválasztja hello száma kisebb, mint a paraméter hello értékét, majd Tez itt hello értéket fogja használni.  

**tez.Grouping.max méretű** – hello paraméter lehetővé teszi a tooset hello maximális méretét minden leképező.  Ha mappers Tez úgy dönt, hogy hello száma nagyobb, mint a paraméter hello értékét, majd Tez itt hello értéket fogja használni.  

**Hive.Exec.reducer.bytes.per.reducer** – Ez a paraméter beállítja az egyes nyomáscsökkentő hello méretét.  Alapértelmezés szerint minden nyomáscsökkentő mérete 256MB.  

## <a name="guidance"></a>Útmutatás

**Állítsa be a hive.exec.reducer.bytes.per.reducer** – hello alapértelmezett érték tömörítetlen hello adatok esetén is működik.  A tömörített adatok csökkentse hello nyomáscsökkentő hello méretét.  

**Állítsa be a hive.tez.container.size** – minden csomóponton, a memória yarn.nodemanager.resource.memory MB-os által megadott és kell megfelelően beállítani a HDI-fürtnek alapértelmezés szerint.  A YARN hello megfelelő memória beállításával kapcsolatos további információkért tekintse meg a [utáni](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).

I/o-igényes munkaterhelések is kihasználhatja a további párhuzamossági hello Tez tároló méretének csökkentésével. Hello felhasználói így növelve a feldolgozási további tárolókat.  Bizonyos Hive-lekérdezések azonban jelentős mennyiségű memória (pl. MapJoin) szükséges.  Ha hello feladat nem rendelkezik elég memóriával, memória kivétel futásidőben kívüli fog kapni.  Ha memória kivételek kívül, majd növelje hello memória.   

futó feladatok vagy párhuzamossági egyidejű száma hello fog időpontjaihoz hello a YARN memória teljes mérete.  YARN a tárolók száma hello szabja meg, hogy hány egyidejű feladatok futtathatók.  toofind hello YARN memória mennyisége, tooAmbari lépjen.  Keresse meg a tooYARN és hello Configs lapon.  hello YARN memória ebben az ablakban jelenik meg.  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
hello kulcs tooimproving teljesítményét ADLS tooincrease hello párhuzamossági lehetőség szerint.  Tez automatikusan kiszámítja hello számát feladatokat, így nem kell tooset létre kell azt.   

## <a name="example-calculation"></a>Példa kiszámítása

Tegyük fel, egy 8 csomópont D14 fürt rendelkezik.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a>Korlátozások
**ADLS-szabályozás** 

Hello kattint UIf korlátozza a sávszélesség megadott által ADLS, toosee feladat hibáihoz kezdenie. Ez azonosítható betartásával szabályozási hibák feladat naplókban által.  Hello párhuzamossági Tez tároló méretének növelésével csökkenthető.  Ha a feladat több egyidejű van szüksége, lépjen kapcsolatba velünk a következő címen.   

Ha Ön első szabályozott toocheck, tooenable hello hibakeresési naplózás hello ügyféloldalon kell. Ez hogyan azt teheti meg:

1. Helyezze el a következő tulajdonság hello log4j tulajdonságai a Hive-config hello. Ezt megteheti az Ambari nézetben: log4j.logger.com.microsoft.azure.datalake.store=DEBUG indítsa újra az összes hello csomópontok/szolgáltatást hello config tootake hatást.

2. Ha Ön első szabályozott, látni fogja, hello HTTP 429 hibakód hello hive naplófájlban. hello hive naplófájl van /tmp/&lt;felhasználói&gt;/hive.log

## <a name="further-information-on-hive-tuning"></a>További információ a Hive hangolása

Az alábbiakban néhány rendszerek, amelyek segítségével finomhangolhatják a Hive-lekérdezéseket:
* [A hdinsight Hadoop Hive-lekérdezések optimalizálása](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Hibaelhárítás a Hive-lekérdezések teljesítményét](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [Az ignite előadás a HDInsight Hive a optimalizálása](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
