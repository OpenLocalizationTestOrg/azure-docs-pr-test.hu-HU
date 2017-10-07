---
title: "aaaOptimize Hive lekérdezi az Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toooptimize a Hive lekérdezi a HDInsight Hadoop eszköze."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d6174c08-06aa-42ac-8e9b-8b8718d9978e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/26/2016
ms.author: jgao
ms.openlocfilehash: d27f8100e1e9f4823040ff9f693e7b78d6192c6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-hive-queries-in-azure-hdinsight"></a>Az Azure HDInsight Hive-lekérdezések optimalizálása

Hadoop-fürtök alapértelmezés szerint nincs optimalizálva a teljesítmény. Ez a cikk ismertet néhány leggyakoribb Hive teljesítmény optimalizálása módszerek tooyour lekérdezéseket is alkalmazható.

## <a name="scale-out-worker-nodes"></a>Horizontális felskálázás munkavégző csomópontokhoz

Hello adhatja meg a fürt feldolgozó csomópontjainak számát növelése kihasználhatják a további mappers és szűkítő toobe párhuzamosan futnak. Növelheti a Hdinsightban bővítő két módja van:

* Hello létesítése során a feldolgozó csomópontok hello Azure-portálon, az Azure PowerShell vagy a platformfüggetlen parancssori felületre hello száma is megadhat.  További információ: [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md). hello alábbi képernyőfelvételen látható hello munkavégző csomópont-konfiguráció hello Azure-portálon:
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
* A futásidejű hello is méretezheti ki a fürt létrehozása egy:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Hello HDInsight által támogatott különböző virtuális gépek kapcsolatos további információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="enable-tez"></a>Tez engedélyezése

[Apache Tez](http://hortonworks.com/hadoop/tez/) egy alternatív végrehajtó motor toohello MapReduce motor:

![tez_1][image-hdi-optimize-hive-tez_1]

Tez használata gyorsabb, mert:

* **Irányított aciklikus diagramhoz (DAG) végre önállóan hello MapReduce összetevőben**. hello DAG igényel minden egyes mappers toobe szűkítő egy készletét követ. Ennek hatására a MapReduce-feladatok több toobe minden egyes Hive lekérdezés hoz. Tez nem tartalmaz ilyen korlátozást, és így minimalizálja a feladat indítási többletterhelés egy feladatként összetett DAG tud feldolgozni.
* **Ezzel elkerülheti a szükségtelen írások**. A hello alatt hoz toomultiple feladatok miatt hello MapReduce motorban, minden feladat eredményének hello azonos Hive-lekérdezések írása tooHDFS köztes adatok. Mivel a Tez minimálisra minden Hive-lekérdezést is képes tooavoid szükségtelen írási feladatok számát.
* **Minimalizálja a kezdeti késések**. Tez jobban tudja toominimize indítási késleltetés el kell toostart és teljes is javítása optimalizálási mappers hello számának csökkentése.
* **A rendszer újból felhasználja tárolók**. Mindig, amikor lehetséges Tez képes tooreuse tárolók tooensure adott késés miatt be tárolók toostarting csökken.
* **Folyamatos optimalizálási technikákat**. Hagyományosan optimalizálási fordítási fázis során végezhető el. Azonban további információ a hello bemenetek érhető el, amelyek engedélyezik a hatékonyabb optimalizálás futásidőben. Tez használ, amely lehetővé teszi a hello futásidejű fázis toooptimize hello terv további folyamatos optimalizálási technikákat.

Ezekről a fogalmakról további részletekért lásd: [Apache TEZ](http://hortonworks.com/hadoop/tez/).

A Hive-lekérdezések engedélyezve van, az alábbi hello beállítású hello lekérdezés illesztésével Tez végezheti el:

    set hive.execution.engine=tez;

Linux-alapú HDInsight-fürtök Tez alapértelmezés szerint engedélyezve van.


## <a name="hive-partitioning"></a>Particionálás struktúra

I/o-művelet hello jelentős teljesítménybeli szűk keresztmetszetek a Hive-lekérdezések futtatásához. hello teljesítmény javítható, ha hello adatmennyiséget, amelyet a toobe olvasni lehet csökkenteni. Alapértelmezés szerint a Hive-lekérdezések teljes Hive táblák beolvasása. Ez a táblázatbeolvasás például lekérdezések nagy. Azonban tooscan kis mennyiségű adatokat (például a szűrési lekérdezések) csak igénylő lekérdezések esetén ez a viselkedés hoz létre a szükségtelen terhelés. Hive táblák Hive particionálás lehetővé teszi a Hive lekérdezések tooaccess csak hello szükséges adatok mennyiségét.

Átrendezése hello nyers adatokat az új könyvtárak mindegyik partíció rendelkezik saját könyvtár - ahol hello partíció hello felhasználó által megadott-e a Hive particionálás van megvalósítva. hello következő diagram azt ábrázolja, Hive tábla particionáló hello oszlop szerint *év*. Minden évben új könyvtár jön létre.

![Particionálás][image-hdi-optimize-hive-partitioning_1]

A particionáló szempontokat:

* **Nem a partíció szerint** -particionálási oszlop csak néhány értékekkel néhány partíciók okozhat. Például nemét a particionálás csak hoz létre (hímivarú és nőivarú) két partíció toobe, így csak hello késés csökkentésére legfeljebb fele.
* **Nem keresztül partíció** – a hello más extreme, több partíciót hoz létre egy egyedi értéket (például a felhasználói azonosítóját) tartalmazó oszlopon okoz. Partíció keresztül aktiválja az mekkora terhelés a hello fürt namenode, mert toohandle hello sok könyvtárat.
* **Adatok eltérésére elkerülése** -válassza ki a particionálási kulcs georeplikációt, hogy az összes partíció található, még akkor is, méret. Példa a particionáló *állapot* okozhat a kaliforniai toobe rekordok hello száma majdnem 30 x, Vermont feltöltési toohello eltérése miatt.

a partíciós tábla toocreate hello használata *particionálva által* záradékban:

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE            STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Hello particionált tábla létrehozása után létrehozhat particionálás statikus vagy dinamikus particionálást.

* **Statikus particionálás** azt jelenti, hogy rendelkezik-e már horizontálisan skálázott adatok hello a megfelelő könyvtárak és megkérheti a Hive partíciók manuálisan hello könyvtár azok pontos helye alapján. a következő kódrészletet hello csak példaként szolgál.
  
        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
  
        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
* **Dinamikus particionálást** azt jelenti, amelyet Hive toocreate partíciók automatikusan meg. Mivel azt már létrehozott előkészítési tábla hello tábla particionáló hello, csak meg kell toodo insert toohello particionált tábla:
  
        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
              L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as           L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as           L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as      L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

További részletekért lásd: [particionált táblák](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

## <a name="use-hello-orcfile-format"></a>Hello ORCFile formátum használata
Hive különböző fájlformátumok támogatja. Példa:

* **Szöveg**: hello alapértelmezett fájlformátuma ez, és együttműködik a legtöbb esetben
* **Az Avro**: együttműködési forgatókönyvek esetén működik
* **ORC/Parquet**: teljesítmény a legalkalmasabb

ORC (optimalizált sor oszlopos) egy rendkívül hatékony módot toostore Hive adatok formátuma. Az összehasonlított tooother formátumok, ORC a következő előnyöket hello rendelkezik:

* összetett típusok, beleértve a DateTime és félig strukturált és a komplex típusok támogatása
* too70 % tömörítési mentése
* minden 10 000 sorok, amelyek lehetővé teszik a rendszer kihagyja a sort indexeli
* futásidejű végrehajtási jelentős csökkenését

tooenable ORC formátumban, akkor először létre kell hoznia egy tábla hello záradékkal *ORC tárolt*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

A következő beszúrása a toohello ORC adattábla az előkészítési tábla hello. Példa:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
            L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

További hello ORC formátumának [Itt](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

## <a name="vectorization"></a>Vectorization

Vectorization lehetővé teszi, hogy a struktúra tooprocess 1024 köteg együtt helyett egy sor feldolgozása egyszerre sort. Azt jelenti, hogy egyszerű műveletek végzett gyorsabb, mert kevesebb belső kód toorun.

tooenable vectorization a Hive-lekérdezést a hello beállítása a következő előtag:

    set hive.vectorized.execution.enabled = true;

További információkért lásd: [lekérdezés-végrehajtás Vectorized](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).

## <a name="other-optimization-methods"></a>Más optimalizálási módszerek
Nincsenek további optimalizálás módszereket is fontolóra veheti, például:

* **Hive bucketing:** olyan módszer, amely lehetővé teszi, hogy toocluster vagy szegmens nagy mennyiségű adatok toooptimize a lekérdezések teljesítményét.
* **Csatlakozás optimalizálási:** struktúrájának lekérdezés-végrehajtás tooimprove tervezési optimalizálása hello illesztések hatékonyságát, és csökkentheti a felhasználó mutatók hello szükségességét. További információkért lásd: [optimalizálási csatlakozás](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
* **Növelje a szűkítő**.

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta rendelkezik Hive lekérdezés több közös optimalizálási módszert. toolearn több, tekintse meg a következő cikkek hello:

* [Apache Hive hdinsight használata](hdinsight-use-hive.md)
* [Repülési késleltetés adatok elemzése a Hive HDInsight használatával](hdinsight-analyze-flight-delay-data.md)
* [Hdinsight Hive eszközzel Twitter-adatok elemzése](hdinsight-analyze-twitter-data.md)
* [HDInsight hadoop Hive lekérdezés konzol hello segítségével érzékelőadatok elemzése](hdinsight-hive-analyze-sensor-data.md)
* [A Hive használata a HDInsight tooanalyze naplók webhelyek](hdinsight-hive-analyze-website-log.md)

[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
