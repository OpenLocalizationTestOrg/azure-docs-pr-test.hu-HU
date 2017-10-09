---
title: "Data Lake Store teljesítményének hangolása irányelveit aaaAzure |} Microsoft Docs"
description: "Az Azure Data Lake Store teljesítményhangolás irányelvek"
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: cgronlun
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: stewu
ms.openlocfilehash: 939faa068c0f81d18d9533956f4d336bc4d0cbe3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tuning-azure-data-lake-store-for-performance"></a>Azure Data Lake Store teljesítményének hangolása

Data Lake Store nagy átviteli intenzív i/o-analitikához és adatátvitelt jelölik a támogatja.  Az Azure Data Lake Store az összes elérhető átviteli sebesség – olvashassák el vagy másodpercenként írt adatok mennyiségét hello – használata fontos tooget hello legjobb teljesítmény érdekében.  Tetszőleges számú olvasási műveletek elvégzésével érhető el, és a lehető párhuzamosan írja.

![Data Lake Store-teljesítmény](./media/data-lake-store-performance-tuning-guidance/throughput.png)

Azure Data Lake Store méretezheti tooprovide hello szükséges átviteli összes analytics forgatókönyvhöz. Alapértelmezés szerint az Azure Data Lake Store-fiók toomeet hello igényeinek használati esetek széles körét biztosítja automatikusan elegendő teljesítményt. Az ügyfelek futtatják az alapértelmezett határérték hello hello esetben hello ADLS-fiók lehet konfigurált tooprovide további átviteli megszüntetéséhez forduljon a Microsoft támogatási szolgálatához.

## <a name="data-ingestion"></a>Adatfeldolgozás

A forrás rendszer tooADLS eseményközpontokból, fontos, hogy a forrás hardver, a forrás hálózati hardver és a hálózati kapcsolat tooADLS hello tooconsider hello szűk lehet.  

![Data Lake Store-teljesítmény](./media/data-lake-store-performance-tuning-guidance/bottleneck.png)

Fontos, hogy az adatátvitel hello tooensure nem érinti a tényezők.

### <a name="source-hardware"></a>Forrás hardver

Függetlenül attól, hogy a helyszíni gépeket vagy virtuális gépek Azure-ban, hello megfelelő hardver gondosan ki kell jelölni. A forrás lemez hardverekhez inkább az SSD-k tooHDDs, és válassza ki a gyorsabb forgórészek lemez hardver. A forrás hálózati hardverek használja a hello leggyorsabb hálózati adapterek lehetséges.  Azure-ban Azure D14 virtuális gépek, amelyeknek hello megfelelően hatékony lemez és a hálózati hardver ajánlott.

### <a name="network-connectivity-tooazure-data-lake-store"></a>A hálózati kapcsolat tooAzure Data Lake Store

hello hálózati kapcsolat a forrásadatok és az Azure Data Lake store között előfordulhat hello szűk keresztmetszetek. A forrásadatok esetén a helyszíni, érdemes lehet egy dedikált hivatkozás a [Azure ExpressRoute](https://azure.microsoft.com/en-us/services/expressroute/) . Ha a forrásadatok az Azure-ban, hello teljesítmény lesz legjobb ha hello adatok hello a Data Lake Store hello Azure megegyező régióban.

### <a name="configure-data-ingestion-tools-for-maximum-parallelization"></a>Maximális párhuzamos folyamatkezelést biztosítja az adatfeldolgozást eszközök konfigurálása

Miután foglalkoztak hello forrás hardver és a hálózati kapcsolat szűk keresztmetszetek a fenti, tooconfigure készen áll az adatfeldolgozást eszközök. hello következő táblázat összefoglalja számos népszerű adatfeldolgozást eszköz hello beállításait, és itt részletes teljesítményhangolás cikkek őket.  a forgatókönyvnek toouse eszköz több arról, hogy mely toolearn látogasson el a [cikk](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-data-scenarios).

| Eszköz               | Beállítások     | További részletekért                                                                 |
|--------------------|------------------------------------------------------|------------------------------|
| PowerShell       | PerFileThreadCount, ConcurrentFileCount |  [Hivatkozás](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-powershell#performance-guidance-while-using-powershell)   |
| AdlCopy    | Azure Data Lake Analytics-egységek  |   [Hivatkozás](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob#performance-considerations-for-using-adlcopy)         |
| Ból a DistCp            | -m (eseményleképező)   | [Hivatkozás](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-copy-data-wasb-distcp#performance-considerations-while-using-distcp)                             |
| Azure Data Factory| parallelCopies    | [Hivatkozás](../data-factory/data-factory-copy-activity-performance.md)                          |
| Sqoop           | FS.Azure.Block.size, -m (eseményleképező)    |   [Hivatkozás](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/)        |

## <a name="structure-your-data-set"></a>Az adatkészlet struktúra

A Data Lake Store hello fájlméret, tárolt adatok fájlok számát, és mappastruktúrát hatással a teljesítményt.  hello alábbi szakasz ismerteti ezek a területek a bevált gyakorlat.  

### <a name="file-size"></a>Fájlméret

Például a HDInsight és az Azure Data Lake Analytics analytics motorok általában egy fájl terhelés kell.  Ha az adatok annyi kis fájlok tárolásához, ez negatívan befolyásoló kódokat is teljesítményét.  

Az adatok általában rendezze nagyobb méretű fájlok, a jobb teljesítmény érdekében.  A szokásos megoldás, mint 256MB vagy annál nagyobb fájlok data készletek rendszerezését. Bizonyos esetekben – például képeket és a bináris adatokat, nincs lehetőség tooprocess párhuzamosan őket.  Ezekben az esetekben tookeep fájlokat a 2GB ajánlott.

Egyes esetekben adatok folyamatok ellenőrzése alatt tartja a hello nyers adatok, amelyek kis fájlok sok korlátozott.  Ajánlott toohave "főzés" generáló nagyobb fájlokká az alsóbb rétegbeli alkalmazásoknak toouse.  

### <a name="organizing-time-series-data-in-folders"></a>A mappákban Idősoros adatok rendezése

A Hive és ADLA munkaterhelésekhez partíció törlése a idősorozat adatok segítségével néhány lekérdezést, olvassa el a hello adat, amely javítja a teljesítményt, csak egy részét.    

Ezek a folyamatok, amelyek betöltik az idősorozat adatokat, gyakran a fájlok és mappák nagyon strukturált elnevezési fájljaik helyezze el. Alább látható adatok, amelyek dátum szerint szerkezete nagyon gyakori példa van:

    \DataSet\YYYY\MM\DD\datafile_YYYY_MM_DD.tsv

Figyelje meg, hogy hello datetime információkat egyaránt mappák és hello fájlnév megjelenik-e.

Dátum és idő hello most egy közös mintát

    \DataSet\YYYY\MM\DD\HH\mm\datafile_YYYY_MM_DD_HH_mm.tsv

Ebben az esetben hello hozott döntés hello mappához és fájlhoz szervezettel kell optimalizálása hello nagyobb méretűek és ésszerű száma minden mappában található fájlokat.

## <a name="optimizing-io-intensive-jobs-on-hadoop-and-spark-workloads-on-hdinsight"></a>A HDInsight Hadoop és a Spark munkaterhelések egy feladat intenzív i/o optimalizálása

Feladatok egyik hello a következő három kategóriába sorolhatók:

* **CPU-intenzív.**  Ezek a feladatok rendelkezik hosszú kiszámításához szükséges idő minimális i/o-idő feltüntetésével.  Ilyen például a gépi tanulási és természetes nyelvű feladatokat dolgoz fel.  
* **Memóriaigényes.**  Ezek a feladatok nagy mennyiségű memóriát használja.  Például PageRank és a valós idejű elemzési feladatot.  
* **I/o-igényes.**  Ezeket a feladatokat az idő során i/o legmagasabbak.  Ilyenek például egy másolási feladat, amely csak olvasási és írási műveletet.  Ezenkívül például az előkészítő feladatok, amelyek nagy mennyiségű adat olvasása, bizonyos adatok átalakítása hajt végre, és majd írások hello hátsó toohello adattár.  

a következő útmutatást hello csak alkalmazható tooI/O-igényes feladatok.

### <a name="general-considerations-for-an-hdinsight-cluster"></a>A HDInsight-fürtök általános szempontok

* **HDInsight-verziókról.** A legjobb teljesítmény érdekében használjon hello HDInsight legújabb kiadása.
* **Régiók.** Helyezze el hello Data Lake Store hello hello HDInsight-fürtöt és ugyanabban a régióban.  

HDInsight-fürtök két átjárócsomópontokkal és az egyes feldolgozó csomópontok állnak. Egyes feldolgozó csomópontok biztosít egy adott magok száma és memória, amely hello VM-típust határozza meg.  A feladat futtatásakor a YARN hello erőforrás egyeztető által lefoglalt hello rendelkezésre álló memória és magok toocreate tárolók.  Minden egyes tároló hello feladatok szükséges toocomplete hello feladatot futtat.  Tárolók gyorsan párhuzamos tooprocess feladatok futtatásához. Ezért a teljesítmény növelése a lehető legtöbb párhuzamos tárolók futtatásával.

Három réteg belül, amely bennünket tooincrease HDInsight-fürtök hello a tárolók száma, és használja az összes elérhető átviteli sebesség.  

* **Fizikai réteg**
* **YARN réteg**
* **Munkaterhelés réteg**

### <a name="physical-layer"></a>Fizikai réteg

**Futtassa a fürt további csomópontokat és/vagy nagyobb méretű virtuális gépeken.**  Automatikusan nagyobb fürt lehetővé teszi, toorun további YARN tárolók a hello az alábbi képen látható módon.

![Data Lake Store-teljesítmény](./media/data-lake-store-performance-tuning-guidance/VM.png)

**Használja a virtuális gépek nagyobb hálózati sávszélességet.**  hálózati sávszélesség mértékét hello lehet szűk keresztmetszet, ha kevesebb mint a Data Lake Store a sávszélesség.  Különböző virtuális gépek lesz a hálózati sávszélesség mérete változó.  Válassza ki a Virtuálisgép-hello legnagyobb lehetséges hálózati sávszélesség álljon.

### <a name="yarn-layer"></a>YARN réteg

**Kisebb YARN tárolók használata.**  Minden egyes YARN tároló toocreate hello méretének csökkentésére hello további tárolók ugyanannyi erőforrást.

![Data Lake Store-teljesítmény](./media/data-lake-store-performance-tuning-guidance/small-containers.png)

Attól függően, hogy a munkaterhelés mindig lesz egy minimális YARN tároló mérete szükséges. Ha túl kicsi a tároló válassza ki, a feladatok a kevés memória problémák fog futni. YARN tárolók általában nem 1GB-nál kisebbnek kell lennie. Közös toosee 3 GB-os YARN tárolók is. Bizonyos munkaterhelések esetén szükség lehet a nagyobb YARN tárolók.  

**Növelje a YARN tárolóban található magok.**  Növelje a hello tooeach tároló tooincrease hello száma párhuzamos minden egyes tárolóban futó feladatok lefoglalt magok száma.  Ez a módszer alkalmazások, például a Spark, amelyek több tevékenységek maximális száma tároló.  Az alkalmazások, mint a Hive, mely egyetlen minden egyes tároló esetén a szál futott van jobb toohave tároló további magok helyett további tárolókat.   

### <a name="workload-layer"></a>Munkaterhelés réteg

**Az összes rendelkezésre álló tárolók használata.**  Állítsa be feladatok toobe hello száma egyenlő vagy nagyobb, mint hello rendelkezésre álló tárolók, hogy az összes erőforrás ki van használva.

![Data Lake Store-teljesítmény](./media/data-lake-store-performance-tuning-guidance/use-containers.png)

**A sikertelen feladatokat is költséges.** Ha egyes feladatok nagy mennyiségű adatok tooprocess, feladat sikertelenségét az egy drága újrapróbálkozási eredményez.  Ezért jobban toocreate további feladatokat, amelyek kisebb mennyiségű adatot dolgozza fel.

Továbbá toohello általános irányelveket a fenti mindegyik alkalmazáshoz különböző paraméterek elérhető tootune az adott alkalmazás tartozik. hello az alábbi táblázat néhány hello paraméterek és hivatkozások tooget használatába teljesítményhangolás minden alkalmazáshoz.

| Számítási feladat               | A paraméter tooset feladatok                                                         |
|--------------------|-------------------------------------------------------------------------------------|
| [A Spark on HDInisight](data-lake-store-performance-tuning-spark.md)       | <ul><li>NUM-végrehajtója</li><li>Végrehajtó memória</li><li>Végrehajtó-magok</li></ul> |
| [A HDInsight Hive](data-lake-store-performance-tuning-hive.md)    | <ul><li>Hive.tez.Container.size</li></ul>         |
| [A HDInsight MapReduce](data-lake-store-performance-tuning-mapreduce.md)            | <ul><li>Mapreduce.Map.Memory</li><li>Mapreduce.job.Maps</li><li>Mapreduce.reduce.Memory</li><li>Mapreduce.job.reduces</li></ul> |
| [A HDInsight alatt futó Storm](data-lake-store-performance-tuning-storm.md)| <ul><li>Munkavégző folyamatok száma</li><li>Spout végrehajtó példányok száma</li><li>Bolt végrehajtó példányok száma </li><li>Spout feladatok száma</li><li>Bolt feladatok száma</li></ul>|

## <a name="see-also"></a>Lásd még:
* [Az Azure Data Lake Store áttekintése](data-lake-store-overview.md)
* [Ismerkedés az Azure Data Lake Analytics szolgáltatással](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
