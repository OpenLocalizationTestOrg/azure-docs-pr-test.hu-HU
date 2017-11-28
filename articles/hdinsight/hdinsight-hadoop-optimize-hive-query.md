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
# <a name="optimize-hive-queries-in-azure-hdinsight"></a><span data-ttu-id="327e2-103">Az Azure HDInsight Hive-lekérdezések optimalizálása</span><span class="sxs-lookup"><span data-stu-id="327e2-103">Optimize Hive queries in Azure HDInsight</span></span>

<span data-ttu-id="327e2-104">Hadoop-fürtök alapértelmezés szerint nincs optimalizálva a teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="327e2-104">By default, Hadoop clusters are not optimized for performance.</span></span> <span data-ttu-id="327e2-105">Ez a cikk ismertet néhány leggyakoribb Hive teljesítmény optimalizálása módszerek tooyour lekérdezéseket is alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="327e2-105">This article covers some most common Hive performance optimization methods that you can apply tooyour queries.</span></span>

## <a name="scale-out-worker-nodes"></a><span data-ttu-id="327e2-106">Horizontális felskálázás munkavégző csomópontokhoz</span><span class="sxs-lookup"><span data-stu-id="327e2-106">Scale out worker nodes</span></span>

<span data-ttu-id="327e2-107">Hello adhatja meg a fürt feldolgozó csomópontjainak számát növelése kihasználhatják a további mappers és szűkítő toobe párhuzamosan futnak.</span><span class="sxs-lookup"><span data-stu-id="327e2-107">Increasing hello number of worker nodes in a cluster can leverage more mappers and reducers toobe run in parallel.</span></span> <span data-ttu-id="327e2-108">Növelheti a Hdinsightban bővítő két módja van:</span><span class="sxs-lookup"><span data-stu-id="327e2-108">There are two ways you can increase scale out in HDInsight:</span></span>

* <span data-ttu-id="327e2-109">Hello létesítése során a feldolgozó csomópontok hello Azure-portálon, az Azure PowerShell vagy a platformfüggetlen parancssori felületre hello száma is megadhat.</span><span class="sxs-lookup"><span data-stu-id="327e2-109">At hello provision time, you can specify hello number of worker nodes using hello Azure portal, Azure PowerShell, or Cross-platform command-line interface.</span></span>  <span data-ttu-id="327e2-110">További információ: [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="327e2-110">For more information, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="327e2-111">hello alábbi képernyőfelvételen látható hello munkavégző csomópont-konfiguráció hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="327e2-111">hello following screenshot shows hello worker node configuration on hello Azure portal:</span></span>
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
* <span data-ttu-id="327e2-113">A futásidejű hello is méretezheti ki a fürt létrehozása egy:</span><span class="sxs-lookup"><span data-stu-id="327e2-113">At hello run time, you can also scale out a cluster without recreating one:</span></span>

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

<span data-ttu-id="327e2-115">Hello HDInsight által támogatott különböző virtuális gépek kapcsolatos további információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="327e2-115">For more information about hello different virtual machines supported by HDInsight, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

## <a name="enable-tez"></a><span data-ttu-id="327e2-116">Tez engedélyezése</span><span class="sxs-lookup"><span data-stu-id="327e2-116">Enable Tez</span></span>

<span data-ttu-id="327e2-117">[Apache Tez](http://hortonworks.com/hadoop/tez/) egy alternatív végrehajtó motor toohello MapReduce motor:</span><span class="sxs-lookup"><span data-stu-id="327e2-117">[Apache Tez](http://hortonworks.com/hadoop/tez/) is an alternative execution engine toohello MapReduce engine:</span></span>

![tez_1][image-hdi-optimize-hive-tez_1]

<span data-ttu-id="327e2-119">Tez használata gyorsabb, mert:</span><span class="sxs-lookup"><span data-stu-id="327e2-119">Tez is faster because:</span></span>

* <span data-ttu-id="327e2-120">**Irányított aciklikus diagramhoz (DAG) végre önállóan hello MapReduce összetevőben**.</span><span class="sxs-lookup"><span data-stu-id="327e2-120">**Execute Directed Acyclic Graph (DAG) as a single job in hello MapReduce engine**.</span></span> <span data-ttu-id="327e2-121">hello DAG igényel minden egyes mappers toobe szűkítő egy készletét követ.</span><span class="sxs-lookup"><span data-stu-id="327e2-121">hello DAG requires each set of mappers toobe followed by one set of reducers.</span></span> <span data-ttu-id="327e2-122">Ennek hatására a MapReduce-feladatok több toobe minden egyes Hive lekérdezés hoz.</span><span class="sxs-lookup"><span data-stu-id="327e2-122">This causes multiple MapReduce jobs toobe spun off for each Hive query.</span></span> <span data-ttu-id="327e2-123">Tez nem tartalmaz ilyen korlátozást, és így minimalizálja a feladat indítási többletterhelés egy feladatként összetett DAG tud feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="327e2-123">Tez does not have such constraint and can process complex DAG as one job thus minimizing job startup overhead.</span></span>
* <span data-ttu-id="327e2-124">**Ezzel elkerülheti a szükségtelen írások**.</span><span class="sxs-lookup"><span data-stu-id="327e2-124">**Avoids unnecessary writes**.</span></span> <span data-ttu-id="327e2-125">A hello alatt hoz toomultiple feladatok miatt hello MapReduce motorban, minden feladat eredményének hello azonos Hive-lekérdezések írása tooHDFS köztes adatok.</span><span class="sxs-lookup"><span data-stu-id="327e2-125">Due toomultiple jobs being spun for hello same Hive query in hello MapReduce engine, hello output of each job is written tooHDFS for intermediate data.</span></span> <span data-ttu-id="327e2-126">Mivel a Tez minimálisra minden Hive-lekérdezést is képes tooavoid szükségtelen írási feladatok számát.</span><span class="sxs-lookup"><span data-stu-id="327e2-126">Since Tez minimizes number of jobs for each Hive query it is able tooavoid unnecessary write.</span></span>
* <span data-ttu-id="327e2-127">**Minimalizálja a kezdeti késések**.</span><span class="sxs-lookup"><span data-stu-id="327e2-127">**Minimizes start-up delays**.</span></span> <span data-ttu-id="327e2-128">Tez jobban tudja toominimize indítási késleltetés el kell toostart és teljes is javítása optimalizálási mappers hello számának csökkentése.</span><span class="sxs-lookup"><span data-stu-id="327e2-128">Tez is better able toominimize start-up delay by reducing hello number of mappers it needs toostart and also improving optimization throughout.</span></span>
* <span data-ttu-id="327e2-129">**A rendszer újból felhasználja tárolók**.</span><span class="sxs-lookup"><span data-stu-id="327e2-129">**Reuses containers**.</span></span> <span data-ttu-id="327e2-130">Mindig, amikor lehetséges Tez képes tooreuse tárolók tooensure adott késés miatt be tárolók toostarting csökken.</span><span class="sxs-lookup"><span data-stu-id="327e2-130">Whenever possible Tez is able tooreuse containers tooensure that latency due toostarting up containers is reduced.</span></span>
* <span data-ttu-id="327e2-131">**Folyamatos optimalizálási technikákat**.</span><span class="sxs-lookup"><span data-stu-id="327e2-131">**Continuous optimization techniques**.</span></span> <span data-ttu-id="327e2-132">Hagyományosan optimalizálási fordítási fázis során végezhető el.</span><span class="sxs-lookup"><span data-stu-id="327e2-132">Traditionally optimization was done during compilation phase.</span></span> <span data-ttu-id="327e2-133">Azonban további információ a hello bemenetek érhető el, amelyek engedélyezik a hatékonyabb optimalizálás futásidőben.</span><span class="sxs-lookup"><span data-stu-id="327e2-133">However more information about hello inputs is available that allow for better optimization during runtime.</span></span> <span data-ttu-id="327e2-134">Tez használ, amely lehetővé teszi a hello futásidejű fázis toooptimize hello terv további folyamatos optimalizálási technikákat.</span><span class="sxs-lookup"><span data-stu-id="327e2-134">Tez uses continuous optimization techniques that allows it toooptimize hello plan further into hello runtime phase.</span></span>

<span data-ttu-id="327e2-135">Ezekről a fogalmakról további részletekért lásd: [Apache TEZ](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="327e2-135">For more details on these concepts, see [Apache TEZ](http://hortonworks.com/hadoop/tez/).</span></span>

<span data-ttu-id="327e2-136">A Hive-lekérdezések engedélyezve van, az alábbi hello beállítású hello lekérdezés illesztésével Tez végezheti el:</span><span class="sxs-lookup"><span data-stu-id="327e2-136">You can make any Hive query Tez enabled by prefixing hello query with hello setting below:</span></span>

    set hive.execution.engine=tez;

<span data-ttu-id="327e2-137">Linux-alapú HDInsight-fürtök Tez alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="327e2-137">Linux-based HDInsight clusters have Tez enabled by default.</span></span>


## <a name="hive-partitioning"></a><span data-ttu-id="327e2-138">Particionálás struktúra</span><span class="sxs-lookup"><span data-stu-id="327e2-138">Hive partitioning</span></span>

<span data-ttu-id="327e2-139">I/o-művelet hello jelentős teljesítménybeli szűk keresztmetszetek a Hive-lekérdezések futtatásához.</span><span class="sxs-lookup"><span data-stu-id="327e2-139">I/O operation is hello major performance bottleneck for running Hive queries.</span></span> <span data-ttu-id="327e2-140">hello teljesítmény javítható, ha hello adatmennyiséget, amelyet a toobe olvasni lehet csökkenteni.</span><span class="sxs-lookup"><span data-stu-id="327e2-140">hello performance can be improved if hello amount of data that needs toobe read can be reduced.</span></span> <span data-ttu-id="327e2-141">Alapértelmezés szerint a Hive-lekérdezések teljes Hive táblák beolvasása.</span><span class="sxs-lookup"><span data-stu-id="327e2-141">By default, Hive queries scan entire Hive tables.</span></span> <span data-ttu-id="327e2-142">Ez a táblázatbeolvasás például lekérdezések nagy.</span><span class="sxs-lookup"><span data-stu-id="327e2-142">This is great for queries like table scans.</span></span> <span data-ttu-id="327e2-143">Azonban tooscan kis mennyiségű adatokat (például a szűrési lekérdezések) csak igénylő lekérdezések esetén ez a viselkedés hoz létre a szükségtelen terhelés.</span><span class="sxs-lookup"><span data-stu-id="327e2-143">However for queries that only need tooscan a small amount of data (for example, queries with filtering), this behavior creates unnecessary overhead.</span></span> <span data-ttu-id="327e2-144">Hive táblák Hive particionálás lehetővé teszi a Hive lekérdezések tooaccess csak hello szükséges adatok mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="327e2-144">Hive partitioning allows Hive queries tooaccess only hello necessary amount of data in Hive tables.</span></span>

<span data-ttu-id="327e2-145">Átrendezése hello nyers adatokat az új könyvtárak mindegyik partíció rendelkezik saját könyvtár - ahol hello partíció hello felhasználó által megadott-e a Hive particionálás van megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="327e2-145">Hive partitioning is implemented by reorganizing hello raw data into new directories with each partition having its own directory - where hello partition is defined by hello user.</span></span> <span data-ttu-id="327e2-146">hello következő diagram azt ábrázolja, Hive tábla particionáló hello oszlop szerint *év*.</span><span class="sxs-lookup"><span data-stu-id="327e2-146">hello following diagram illustrates partitioning a Hive table by hello column *Year*.</span></span> <span data-ttu-id="327e2-147">Minden évben új könyvtár jön létre.</span><span class="sxs-lookup"><span data-stu-id="327e2-147">A new directory is created for each year.</span></span>

![Particionálás][image-hdi-optimize-hive-partitioning_1]

<span data-ttu-id="327e2-149">A particionáló szempontokat:</span><span class="sxs-lookup"><span data-stu-id="327e2-149">Some partitioning considerations:</span></span>

* <span data-ttu-id="327e2-150">**Nem a partíció szerint** -particionálási oszlop csak néhány értékekkel néhány partíciók okozhat.</span><span class="sxs-lookup"><span data-stu-id="327e2-150">**Do not under partition** - Partitioning on columns with only a few values can cause few partitions.</span></span> <span data-ttu-id="327e2-151">Például nemét a particionálás csak hoz létre (hímivarú és nőivarú) két partíció toobe, így csak hello késés csökkentésére legfeljebb fele.</span><span class="sxs-lookup"><span data-stu-id="327e2-151">For example, partitioning on gender only creates two partitions toobe created (male and female), thus only reduce hello latency by a maximum of half.</span></span>
* <span data-ttu-id="327e2-152">**Nem keresztül partíció** – a hello más extreme, több partíciót hoz létre egy egyedi értéket (például a felhasználói azonosítóját) tartalmazó oszlopon okoz.</span><span class="sxs-lookup"><span data-stu-id="327e2-152">**Do not over partition** - On hello other extreme, creating a partition on a column with a unique value (for example, userid) causes multiple partitions.</span></span> <span data-ttu-id="327e2-153">Partíció keresztül aktiválja az mekkora terhelés a hello fürt namenode, mert toohandle hello sok könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="327e2-153">Over partition causes much stress on hello cluster namenode as it has toohandle hello large number of directories.</span></span>
* <span data-ttu-id="327e2-154">**Adatok eltérésére elkerülése** -válassza ki a particionálási kulcs georeplikációt, hogy az összes partíció található, még akkor is, méret.</span><span class="sxs-lookup"><span data-stu-id="327e2-154">**Avoid data skew** - Choose your partitioning key wisely so that all partitions are even size.</span></span> <span data-ttu-id="327e2-155">Példa a particionáló *állapot* okozhat a kaliforniai toobe rekordok hello száma majdnem 30 x, Vermont feltöltési toohello eltérése miatt.</span><span class="sxs-lookup"><span data-stu-id="327e2-155">An example is partitioning on *State* may cause hello number of records under California toobe almost 30x that of Vermont due toohello difference in population.</span></span>

<span data-ttu-id="327e2-156">a partíciós tábla toocreate hello használata *particionálva által* záradékban:</span><span class="sxs-lookup"><span data-stu-id="327e2-156">toocreate a partition table, use hello *Partitioned By* clause:</span></span>

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE            STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

<span data-ttu-id="327e2-157">Hello particionált tábla létrehozása után létrehozhat particionálás statikus vagy dinamikus particionálást.</span><span class="sxs-lookup"><span data-stu-id="327e2-157">Once hello partitioned table is created, you can either create static partitioning or dynamic partitioning.</span></span>

* <span data-ttu-id="327e2-158">**Statikus particionálás** azt jelenti, hogy rendelkezik-e már horizontálisan skálázott adatok hello a megfelelő könyvtárak és megkérheti a Hive partíciók manuálisan hello könyvtár azok pontos helye alapján.</span><span class="sxs-lookup"><span data-stu-id="327e2-158">**Static partitioning** means that you have already sharded data in hello appropriate directories and you can ask Hive partitions manually based on hello directory location.</span></span> <span data-ttu-id="327e2-159">a következő kódrészletet hello csak példaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="327e2-159">hello following code snippet is an example.</span></span>
  
        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
  
        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
* <span data-ttu-id="327e2-160">**Dinamikus particionálást** azt jelenti, amelyet Hive toocreate partíciók automatikusan meg.</span><span class="sxs-lookup"><span data-stu-id="327e2-160">**Dynamic partitioning** means that you want Hive toocreate partitions automatically for you.</span></span> <span data-ttu-id="327e2-161">Mivel azt már létrehozott előkészítési tábla hello tábla particionáló hello, csak meg kell toodo insert toohello particionált tábla:</span><span class="sxs-lookup"><span data-stu-id="327e2-161">Since we have already created hello partitioning table from hello staging table, all we need toodo is insert data toohello partitioned table:</span></span>
  
        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
              L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as           L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as           L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as      L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

<span data-ttu-id="327e2-162">További részletekért lásd: [particionált táblák](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).</span><span class="sxs-lookup"><span data-stu-id="327e2-162">For more details, see [Partitioned Tables](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).</span></span>

## <a name="use-hello-orcfile-format"></a><span data-ttu-id="327e2-163">Hello ORCFile formátum használata</span><span class="sxs-lookup"><span data-stu-id="327e2-163">Use hello ORCFile format</span></span>
<span data-ttu-id="327e2-164">Hive különböző fájlformátumok támogatja.</span><span class="sxs-lookup"><span data-stu-id="327e2-164">Hive supports different file formats.</span></span> <span data-ttu-id="327e2-165">Példa:</span><span class="sxs-lookup"><span data-stu-id="327e2-165">For example:</span></span>

* <span data-ttu-id="327e2-166">**Szöveg**: hello alapértelmezett fájlformátuma ez, és együttműködik a legtöbb esetben</span><span class="sxs-lookup"><span data-stu-id="327e2-166">**Text**: this is hello default file format and works with most scenarios</span></span>
* <span data-ttu-id="327e2-167">**Az Avro**: együttműködési forgatókönyvek esetén működik</span><span class="sxs-lookup"><span data-stu-id="327e2-167">**Avro**: works well for interoperability scenarios</span></span>
* <span data-ttu-id="327e2-168">**ORC/Parquet**: teljesítmény a legalkalmasabb</span><span class="sxs-lookup"><span data-stu-id="327e2-168">**ORC/Parquet**: best suited for performance</span></span>

<span data-ttu-id="327e2-169">ORC (optimalizált sor oszlopos) egy rendkívül hatékony módot toostore Hive adatok formátuma.</span><span class="sxs-lookup"><span data-stu-id="327e2-169">ORC (Optimized Row Columnar) format is a highly efficient way toostore Hive data.</span></span> <span data-ttu-id="327e2-170">Az összehasonlított tooother formátumok, ORC a következő előnyöket hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="327e2-170">Compared tooother formats, ORC has hello following advantages:</span></span>

* <span data-ttu-id="327e2-171">összetett típusok, beleértve a DateTime és félig strukturált és a komplex típusok támogatása</span><span class="sxs-lookup"><span data-stu-id="327e2-171">support for complex types including DateTime and complex and semi-structured types</span></span>
* <span data-ttu-id="327e2-172">too70 % tömörítési mentése</span><span class="sxs-lookup"><span data-stu-id="327e2-172">up too70% compression</span></span>
* <span data-ttu-id="327e2-173">minden 10 000 sorok, amelyek lehetővé teszik a rendszer kihagyja a sort indexeli</span><span class="sxs-lookup"><span data-stu-id="327e2-173">indexes every 10,000 rows, which allow skipping rows</span></span>
* <span data-ttu-id="327e2-174">futásidejű végrehajtási jelentős csökkenését</span><span class="sxs-lookup"><span data-stu-id="327e2-174">a significant drop in run-time execution</span></span>

<span data-ttu-id="327e2-175">tooenable ORC formátumban, akkor először létre kell hoznia egy tábla hello záradékkal *ORC tárolt*:</span><span class="sxs-lookup"><span data-stu-id="327e2-175">tooenable ORC format, you first create a table with hello clause *Stored as ORC*:</span></span>

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

<span data-ttu-id="327e2-176">A következő beszúrása a toohello ORC adattábla az előkészítési tábla hello.</span><span class="sxs-lookup"><span data-stu-id="327e2-176">Next, you insert data toohello ORC table from hello staging table.</span></span> <span data-ttu-id="327e2-177">Példa:</span><span class="sxs-lookup"><span data-stu-id="327e2-177">For example:</span></span>

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

<span data-ttu-id="327e2-178">További hello ORC formátumának [Itt](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).</span><span class="sxs-lookup"><span data-stu-id="327e2-178">You can read more on hello ORC format [here](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).</span></span>

## <a name="vectorization"></a><span data-ttu-id="327e2-179">Vectorization</span><span class="sxs-lookup"><span data-stu-id="327e2-179">Vectorization</span></span>

<span data-ttu-id="327e2-180">Vectorization lehetővé teszi, hogy a struktúra tooprocess 1024 köteg együtt helyett egy sor feldolgozása egyszerre sort.</span><span class="sxs-lookup"><span data-stu-id="327e2-180">Vectorization allows Hive tooprocess a batch of 1024 rows together instead of processing one row at a time.</span></span> <span data-ttu-id="327e2-181">Azt jelenti, hogy egyszerű műveletek végzett gyorsabb, mert kevesebb belső kód toorun.</span><span class="sxs-lookup"><span data-stu-id="327e2-181">It means that simple operations are done faster because less internal code needs toorun.</span></span>

<span data-ttu-id="327e2-182">tooenable vectorization a Hive-lekérdezést a hello beállítása a következő előtag:</span><span class="sxs-lookup"><span data-stu-id="327e2-182">tooenable vectorization prefix your Hive query with hello following setting:</span></span>

    set hive.vectorized.execution.enabled = true;

<span data-ttu-id="327e2-183">További információkért lásd: [lekérdezés-végrehajtás Vectorized](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).</span><span class="sxs-lookup"><span data-stu-id="327e2-183">For more information, see [Vectorized query execution](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).</span></span>

## <a name="other-optimization-methods"></a><span data-ttu-id="327e2-184">Más optimalizálási módszerek</span><span class="sxs-lookup"><span data-stu-id="327e2-184">Other optimization methods</span></span>
<span data-ttu-id="327e2-185">Nincsenek további optimalizálás módszereket is fontolóra veheti, például:</span><span class="sxs-lookup"><span data-stu-id="327e2-185">There are more optimization methods that you can consider, for example:</span></span>

* <span data-ttu-id="327e2-186">**Hive bucketing:** olyan módszer, amely lehetővé teszi, hogy toocluster vagy szegmens nagy mennyiségű adatok toooptimize a lekérdezések teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="327e2-186">**Hive bucketing:** a technique that allows toocluster or segment large sets of data toooptimize query performance.</span></span>
* <span data-ttu-id="327e2-187">**Csatlakozás optimalizálási:** struktúrájának lekérdezés-végrehajtás tooimprove tervezési optimalizálása hello illesztések hatékonyságát, és csökkentheti a felhasználó mutatók hello szükségességét.</span><span class="sxs-lookup"><span data-stu-id="327e2-187">**Join optimization:** optimization of Hive's query execution planning tooimprove hello efficiency of joins and reduce hello need for user hints.</span></span> <span data-ttu-id="327e2-188">További információkért lásd: [optimalizálási csatlakozás](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).</span><span class="sxs-lookup"><span data-stu-id="327e2-188">For more information, see [Join optimization](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).</span></span>
* <span data-ttu-id="327e2-189">**Növelje a szűkítő**.</span><span class="sxs-lookup"><span data-stu-id="327e2-189">**Increase Reducers**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="327e2-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="327e2-190">Next steps</span></span>
<span data-ttu-id="327e2-191">Ebben a cikkben megtanulta rendelkezik Hive lekérdezés több közös optimalizálási módszert.</span><span class="sxs-lookup"><span data-stu-id="327e2-191">In this article, you have learned several common Hive query optimization methods.</span></span> <span data-ttu-id="327e2-192">toolearn több, tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="327e2-192">toolearn more, see hello following articles:</span></span>

* [<span data-ttu-id="327e2-193">Apache Hive hdinsight használata</span><span class="sxs-lookup"><span data-stu-id="327e2-193">Use Apache Hive in HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="327e2-194">Repülési késleltetés adatok elemzése a Hive HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="327e2-194">Analyze flight delay data by using Hive in HDInsight</span></span>](hdinsight-analyze-flight-delay-data.md)
* [<span data-ttu-id="327e2-195">Hdinsight Hive eszközzel Twitter-adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="327e2-195">Analyze Twitter data using Hive in HDInsight</span></span>](hdinsight-analyze-twitter-data.md)
* [<span data-ttu-id="327e2-196">HDInsight hadoop Hive lekérdezés konzol hello segítségével érzékelőadatok elemzése</span><span class="sxs-lookup"><span data-stu-id="327e2-196">Analyze sensor data using hello Hive Query Console on Hadoop in HDInsight</span></span>](hdinsight-hive-analyze-sensor-data.md)
* [<span data-ttu-id="327e2-197">A Hive használata a HDInsight tooanalyze naplók webhelyek</span><span class="sxs-lookup"><span data-stu-id="327e2-197">Use Hive with HDInsight tooanalyze logs from websites</span></span>](hdinsight-hive-analyze-website-log.md)

[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
