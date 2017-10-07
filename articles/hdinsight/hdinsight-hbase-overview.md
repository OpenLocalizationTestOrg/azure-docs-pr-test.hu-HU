---
title: aaaWhat az a HBase az Azure HDInsight? | Microsoft Docs
description: "Egy bevezető tooApache HBase a HDInsight hadoop épített NoSQL-adatbázis. További információk a használati esetek, és hasonlítsa össze a HBase tooother Hadoop-fürtök."
keywords: "bigtable,nosql,mi az a hbase,apache hbase,hbase,habase áttekintés,"
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: d2a76d53-133a-4849-a30c-88d9c794391c
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 0d28378d07b1a168e38748548578be11310d2228
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>Mi az a HBase a HDInsight eszközben: Egy NoSQL-adatbázis, amely BigTable-szerű képességeket nyújt a Hadoophoz
Az Apache HBase egy nyílt forráskódú NoSQL-adatbázis, amely a Hadoopra épült, és a Google BigTable után van modellezve. A HBase véletlenszerű hozzáférést és erős konzisztenciát biztosít a nagy mennyiségű strukturálatlan és félig strukturált adatok számára egy séma nélküli adatbázisban oszlopcsaládok szerint rendezve.

Egy tábla soraira hello adatokat tárolja, és egy soron belüli adatok szerint vannak csoportosítva. A rendszer a HBase egy séma nélküli adatbázis abban, hogy sem az oszlopokat és a bennük tárolt adattípusokat hello típusú hello hello értelemben toobe használatuk előtt definiálni kell. hello nyílt forráskód arányosan lineárisan toohandle adatmennyiségig a több ezer. Adatredundanciát, kötegelt feldolgozáson és egyéb szolgáltatások hello Hadoop ökoszisztémájának alatt álló elosztott alkalmazás által biztosított támaszkodhat.

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>Hogyan van megvalósítva a HBase az Azure HDInsight eszközben?
A HDInsight hbase hello Azure-alapú környezetben az integrált felügyelt fürtként. hello fürtök olyan konfigurált toostore adatok közvetlenül a [Azure Storage](./hdinsight-hadoop-use-blob-storage.md) vagy [Azure Data Lake Store](./hdinsight-hadoop-use-data-lake-store.md), amely biztosítja, alacsony késést és nagyobb rugalmasságot teljesítményének és költséghatékonyságának választék. Ez lehetővé teszi az ügyfelek toobuild interaktív webhelyeket, amelyek nagy adatkészletekkel végpontok és tooanalyze több millió érzékelői és telemetriai adatokat tároló toobuild szolgáltatások működéséhez ezeket az adatokat Hadoop-feladatokkal. HBase és a Hadoop jó kezdőpont a big data-projektekhez az Azure rendszerben; különösen nagy adatkészletekkel valós idejű alkalmazások toowork is engedélyezhető.

hello HDInsight-implementáció kihasználja a HBase tooprovide automatikus horizontális táblák, erős konzisztenciát biztosít az olvasási és írási műveletek és automatikus feladatátvételt hello kibővített architektúrát. A teljesítményt a memóriába való gyorsítótárazás növeli az olvasáshoz, és a nagy streaming-kapacitás az írásokhoz. A HBase-fürt a virtuális hálózaton belül hozható létre. További információ: [HDInsight-fürtök létrehozása az Azure Virtual Network-ön][hbase-provision-vnet].

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Hogyan történik az adatok kezelése a HDInsight HBase-ben?
Adatok kezelhetők a HBase hello segítségével `create`, `get`, `put`, és `scan` hello HBase rendszerhéj parancsok. Adatot ír toohello adatbázis használatával `put` és olvasható `get`. Hello `scan` parancs egy tábla sorainak több használt tooobtain adatokat jelzi. Adatok hello fölött hello HBase REST API-t egy ügyfélkönyvtárat biztosít a HBase C# API használatával is kezelhetők. A HBase adatbázisok a Hive használatával is lekérdezhetők. Egy bevezető toothese programozási modellek, lásd: [HBase a Hadoop HDInsight használatának megkezdésében][hbase-get-started]. Társprocesszorok is elérhetők, amelyek lehetővé teszik az adatfeldolgozást hello csomópontok, hogy az állomás hello adatbázis.

> [!NOTE]
> A HBase nem támogatja a Thriftet a HDInsightban.
>

## <a name="scenarios-use-cases-for-hbase"></a>Forgatókönyvek: A HBase használati esetei
hello kanonikus használati eset, amelyhez a bigtable (és így a hbase) létrejött webes keresés. A keresőmotorok indexeket építenek, amelyek leképezése feltételek toohello weblapok azokat. A HBase azonban sok más használati esethez megfelelő – amelyek közül több ebben a szakaszban van felsorolva.

* Kulcs-érték tároló
  
    A HBase használható kulcs-érték tárolóként, és megfelelő az üzenetkezelési rendszerek kezeléséhez. A Facebook a HBase eszközt használja az üzenetkezelési rendszeréhez, amely ideális az internetes kommunikációk tárolásához és kezeléséhez. Webtábla használja a HBase toosearch táblázatok keresésére és kezelésére kinyert.
* Érzékelői adatok
  
    A HBase hasznos a növekményesen, különböző forrásokból gyűjtött adatok rögzítéséhez. Ebbe tartozik a közösségi hálók adatainak elemzése, az idősorozat, az interaktív irányítópultok naprakészen tartása trendekkel és számlálókkal, valamint a naplórendszerek kezelése. Példa erre a Bloomberg trader terminál és hello Open Time Series Database (OpenTSDB), amely tárolja, és hozzáférést toometrics biztosít gyűjtött server rendszerek hello állapotát.
* Valós idejű lekérdezés
  
    A [Phoenix](http://phoenix.apache.org/) az Apache HBase SQL lekérdezési motorja. JDBC-illesztőként érhető el, és lehetővé teszi a HBase táblák SQL eszközzel végzett lekérdezését és kezelését.
* A HBase platformként
  
    Az alkalmazások a HBase felett futhatnak adattárolóként. Erre példa a Phoenix, az OpenTSDB, a Kiji és a Titan. Az alkalmazások integrálhatók is a HBase eszközzel. Erre példa a Hive, Pig, Solr, Storm, Flume, Impala, Spark, Ganglia és Drill.

## <a name="next-steps"></a>Következő lépések
* [A HBase első lépései a Hadooppal a HDInsightban][hbase-get-started]
* [HDInsight-fürtök létrehozása az Azure Virtual Network-ön][hbase-provision-vnet]
* [HBase-replikálás konfigurálása a HDInsightban](hdinsight-hbase-replication.md)
* [Twitter-vélemények elemzése a HBase-szel a HDInsightban][hbase-twitter-sentiment]
* [Maven toobuild Java-alkalmazások, amelyek használják a HBase és a HDInsight (Hadoop) együttes használata][hbase-build-java-maven]

## <a name="see-also"></a>Lásd még:
* [Apache HBase](https://hbase.apache.org/)
* [Bigtable: Elosztott tárolórendszer strukturált adatokhoz](http://research.google.com/archive/bigtable.html)

[hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hbase-build-java-maven]: hdinsight-hbase-build-java-maven.md

[hdinsight-use-hive]: hdinsight-use-hive.md

[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md

[hbase-get-started]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
