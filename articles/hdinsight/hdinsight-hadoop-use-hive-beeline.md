---
title: az Apache Hive - Azure HDInsight Beeline aaaUse |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello Beeline ügyfél toorun Hive HDInsight a hadooppal lekérdezi. Beeline olyan eszköz, amellyel a hiveserver2-n keresztül JDBC használata."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "beeline struktúra, hive beeline"
ms.assetid: 3adfb1ba-8924-4a13-98db-10a67ab24fca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: e788ff39f33d928808cfcb83a92f62ac9ae8ca09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-beeline-client-with-apache-hive"></a><span data-ttu-id="23f32-105">Apache Hive hello Beeline ügyfél használata</span><span class="sxs-lookup"><span data-stu-id="23f32-105">Use hello Beeline client with Apache Hive</span></span>

<span data-ttu-id="23f32-106">Megtudhatja, hogyan toouse [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) toorun Hive hdinsight lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="23f32-106">Learn how toouse [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) toorun Hive queries on HDInsight.</span></span>

<span data-ttu-id="23f32-107">Beeline szerepel-e a HDInsight-fürt hello átjárócsomópontokkal Hive ügyfél.</span><span class="sxs-lookup"><span data-stu-id="23f32-107">Beeline is a Hive client that is included on hello head nodes of your HDInsight cluster.</span></span> <span data-ttu-id="23f32-108">Beeline JDBC tooconnect tooHiveServer2, egy a HDInsight fürt által futtatott szolgáltatást használ.</span><span class="sxs-lookup"><span data-stu-id="23f32-108">Beeline uses JDBC tooconnect tooHiveServer2, a service hosted on your HDInsight cluster.</span></span> <span data-ttu-id="23f32-109">Is használhatja Beeline tooaccess Hive hdinsight távolról több mint hello internet.</span><span class="sxs-lookup"><span data-stu-id="23f32-109">You can also use Beeline tooaccess Hive on HDInsight remotely over hello internet.</span></span> <span data-ttu-id="23f32-110">a következő táblázat hello kapcsolati karakterláncok Beeline való használatra biztosítja:</span><span class="sxs-lookup"><span data-stu-id="23f32-110">hello following table provides connection strings for use with Beeline:</span></span>

| <span data-ttu-id="23f32-111">A Beeline futtató</span><span class="sxs-lookup"><span data-stu-id="23f32-111">Where you run Beeline from</span></span> | <span data-ttu-id="23f32-112">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="23f32-112">Parameters</span></span> |
| --- | --- | --- |
| <span data-ttu-id="23f32-113">Egy SSH kapcsolat tooa headnode vagy peremhálózati csomópont</span><span class="sxs-lookup"><span data-stu-id="23f32-113">An SSH connection tooa headnode or edge node</span></span> | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
| <span data-ttu-id="23f32-114">Külső hello fürt</span><span class="sxs-lookup"><span data-stu-id="23f32-114">Outside hello cluster</span></span> | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

> [!NOTE]
> <span data-ttu-id="23f32-115">Cserélje le `admin` hello fürt bejelentkezési fiókkal a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="23f32-115">Replace `admin` with hello cluster login account for your cluster.</span></span>
>
> <span data-ttu-id="23f32-116">Cserélje le `password` hello fürt bejelentkezési fiók hello jelszóval.</span><span class="sxs-lookup"><span data-stu-id="23f32-116">Replace `password` with hello password for hello cluster login account.</span></span>
>
> <span data-ttu-id="23f32-117">Cserélje le `clustername` hello nevet, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="23f32-117">Replace `clustername` with hello name of your HDInsight cluster.</span></span>

## <span data-ttu-id="23f32-118"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="23f32-118"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="23f32-119">A Linux-based Hadoop on HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="23f32-119">A Linux-based Hadoop on HDInsight cluster.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="23f32-120">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="23f32-120">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="23f32-121">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="23f32-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="23f32-122">Egy SSH-ügyfél vagy a helyi Beeline ügyfél.</span><span class="sxs-lookup"><span data-stu-id="23f32-122">An SSH client or a local Beeline client.</span></span> <span data-ttu-id="23f32-123">A legtöbb hello jelen dokumentumban leírt lépések azt feltételezik, hogy használt Beeline SSH munkamenet toohello fürtök.</span><span class="sxs-lookup"><span data-stu-id="23f32-123">Most of hello steps in this document assume that you are using Beeline from an SSH session toohello cluster.</span></span> <span data-ttu-id="23f32-124">A külső hello fürtből Beeline futó információkért lásd: hello [Beeline távolról használható](#remote) szakasz.</span><span class="sxs-lookup"><span data-stu-id="23f32-124">For information on running Beeline from outside hello cluster, see hello [use Beeline remotely](#remote) section.</span></span>

    <span data-ttu-id="23f32-125">További információ az SSH használatával, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="23f32-125">For more information on using SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="23f32-126"><a id="beeline"></a>Beeline használata</span><span class="sxs-lookup"><span data-stu-id="23f32-126"><a id="beeline"></a>Use Beeline</span></span>

1. <span data-ttu-id="23f32-127">Beeline indításakor kell megadnia egy kapcsolati karakterláncot a hiveserver2-n a HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="23f32-127">When starting Beeline, you must provide a connection string for HiveServer2 on your HDInsight cluster.</span></span> <span data-ttu-id="23f32-128">toorun hello parancs külső hello fürtből, meg kell adni hello fürt bejelentkezési fiók neve (alapértelmezett `admin`) és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="23f32-128">toorun hello command from outside hello cluster, you must also provide hello cluster login account name (default `admin`) and password.</span></span> <span data-ttu-id="23f32-129">A következő tábla toofind hello kapcsolati karakterlánc formátuma és paraméterek toouse hello használata:</span><span class="sxs-lookup"><span data-stu-id="23f32-129">Use hello following table toofind hello connection string format and parameters toouse:</span></span>

    | <span data-ttu-id="23f32-130">A Beeline futtató</span><span class="sxs-lookup"><span data-stu-id="23f32-130">Where you run Beeline from</span></span> | <span data-ttu-id="23f32-131">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="23f32-131">Parameters</span></span> |
    | --- | --- | --- |
    | <span data-ttu-id="23f32-132">Egy SSH kapcsolat tooa headnode vagy peremhálózati csomópont</span><span class="sxs-lookup"><span data-stu-id="23f32-132">An SSH connection tooa headnode or edge node</span></span> | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
    | <span data-ttu-id="23f32-133">Külső hello fürt</span><span class="sxs-lookup"><span data-stu-id="23f32-133">Outside hello cluster</span></span> | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

    <span data-ttu-id="23f32-134">Például a következő parancs hello lehet használt toostart Beeline SSH munkamenet toohello fürtök:</span><span class="sxs-lookup"><span data-stu-id="23f32-134">For example, hello following command can be used toostart Beeline from an SSH session toohello cluster:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    <span data-ttu-id="23f32-135">A parancs hello Beeline ügyfél elindul, és a hello átjárócsomóponthoz tooHiveServer2 csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="23f32-135">This command starts hello Beeline client, and connects tooHiveServer2 on hello cluster head node.</span></span> <span data-ttu-id="23f32-136">Hello parancs után megérkezik a egy `jdbc:hive2://headnodehost:10001/>` kérdés.</span><span class="sxs-lookup"><span data-stu-id="23f32-136">Once hello command completes, you arrive at a `jdbc:hive2://headnodehost:10001/>` prompt.</span></span>

2. <span data-ttu-id="23f32-137">Beeline parancsok kezdődhet egy `!` karakter, például `!help` megjeleníti a súgót.</span><span class="sxs-lookup"><span data-stu-id="23f32-137">Beeline commands begin with a `!` character, for example `!help` displays help.</span></span> <span data-ttu-id="23f32-138">Azonban hello `!` egyes parancsok kihagyható.</span><span class="sxs-lookup"><span data-stu-id="23f32-138">However hello `!` can be omitted for some commands.</span></span> <span data-ttu-id="23f32-139">Például `help` is működik.</span><span class="sxs-lookup"><span data-stu-id="23f32-139">For example, `help` also works.</span></span>

    <span data-ttu-id="23f32-140">Nincs olyan `!sql`, mely van használt tooexecute HiveQL utasításokat.</span><span class="sxs-lookup"><span data-stu-id="23f32-140">There is a `!sql`, which is used tooexecute HiveQL statements.</span></span> <span data-ttu-id="23f32-141">Azonban HiveQL ezért általában arra használják, akkor kihagyhatja az előző hello `!sql`.</span><span class="sxs-lookup"><span data-stu-id="23f32-141">However, HiveQL is so commonly used that you can omit hello preceding `!sql`.</span></span> <span data-ttu-id="23f32-142">a következő két utasítások hello egyenértékűek:</span><span class="sxs-lookup"><span data-stu-id="23f32-142">hello following two statements are equivalent:</span></span>

    ```hiveql
    !sql show tables;
    show tables;
    ```

    <span data-ttu-id="23f32-143">Az új fürtön csak egy tábla szerepel: **hivesampletable**.</span><span class="sxs-lookup"><span data-stu-id="23f32-143">On a new cluster, only one table is listed: **hivesampletable**.</span></span>

3. <span data-ttu-id="23f32-144">A következő parancs toodisplay hello séma hello hivesampletable hello használata:</span><span class="sxs-lookup"><span data-stu-id="23f32-144">Use hello following command toodisplay hello schema for hello hivesampletable:</span></span>

    ```hiveql
    describe hivesampletable;
    ```

    <span data-ttu-id="23f32-145">Ez a parancs visszaadja a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="23f32-145">This command returns hello following information:</span></span>

        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    <span data-ttu-id="23f32-146">Ez a témakör hello hello tábla oszlopai.</span><span class="sxs-lookup"><span data-stu-id="23f32-146">This information describes hello columns in hello table.</span></span> <span data-ttu-id="23f32-147">Most tudta elvégezni egyes lekérdezések adatok alapján, amíg inkább hozzon létre egy új tábla toodemonstrate hogyan tooload adatokat a Hive és a séma alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="23f32-147">While we could perform some queries against this data, let's instead create a brand new table toodemonstrate how tooload data into Hive and apply a schema.</span></span>

4. <span data-ttu-id="23f32-148">Adja meg a következő utasítások toocreate nevű tábla hello **log4jLogs** hello HDInsight-fürthöz megadott mintaadatokat használatával:</span><span class="sxs-lookup"><span data-stu-id="23f32-148">Enter hello following statements toocreate a table named **log4jLogs** by using sample data provided with hello HDInsight cluster:</span></span>

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    <span data-ttu-id="23f32-149">Ezekre az utasításokra hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="23f32-149">These statements perform hello following actions:</span></span>

    * <span data-ttu-id="23f32-150">`DROP TABLE`-Ha hello tábla létezik, akkor az törlődik.</span><span class="sxs-lookup"><span data-stu-id="23f32-150">`DROP TABLE` - If hello table exists, it is deleted.</span></span>

    * <span data-ttu-id="23f32-151">`CREATE EXTERNAL TABLE`-Hoz létre egy **külső** Hive táblát.</span><span class="sxs-lookup"><span data-stu-id="23f32-151">`CREATE EXTERNAL TABLE` - Creates an **external** table in Hive.</span></span> <span data-ttu-id="23f32-152">Külső táblák csak tárolása Hive hello tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="23f32-152">External tables only store hello table definition in Hive.</span></span> <span data-ttu-id="23f32-153">hello adatok hello eredeti helyen marad.</span><span class="sxs-lookup"><span data-stu-id="23f32-153">hello data is left in hello original location.</span></span>

    * <span data-ttu-id="23f32-154">`ROW FORMAT`-Hello adatok formázását.</span><span class="sxs-lookup"><span data-stu-id="23f32-154">`ROW FORMAT` - How hello data is formatted.</span></span> <span data-ttu-id="23f32-155">Ebben az esetben az egyes naplókon hello mezők szóközzel elválasztva.</span><span class="sxs-lookup"><span data-stu-id="23f32-155">In this case, hello fields in each log are separated by a space.</span></span>

    * <span data-ttu-id="23f32-156">`STORED AS TEXTFILE LOCATION`-Ha hello tárolja, és milyen formátumban.</span><span class="sxs-lookup"><span data-stu-id="23f32-156">`STORED AS TEXTFILE LOCATION` - Where hello data is stored and in what file format.</span></span>

    * <span data-ttu-id="23f32-157">`SELECT`-Választja ki az összes sorok számát ahol oszlop **t4** hello értéket tartalmaz **[hiba]**.</span><span class="sxs-lookup"><span data-stu-id="23f32-157">`SELECT` - Selects a count of all rows where column **t4** contains hello value **[ERROR]**.</span></span> <span data-ttu-id="23f32-158">A lekérdezés által visszaadott érték **3** ahány három ezt az értéket tartalmazó sorok.</span><span class="sxs-lookup"><span data-stu-id="23f32-158">This query returns a value of **3** as there are three rows that contain this value.</span></span>

    * <span data-ttu-id="23f32-159">`INPUT__FILE__NAME LIKE '%.log'`-Hive megpróbál tooapply hello tooall sémafájlok hello könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="23f32-159">`INPUT__FILE__NAME LIKE '%.log'` - Hive attempts tooapply hello schema tooall files in hello directory.</span></span> <span data-ttu-id="23f32-160">Hello directory ebben az esetben nem egyeznek meg a hello séma fájlokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="23f32-160">In this case, hello directory contains files that do not match hello schema.</span></span> <span data-ttu-id="23f32-161">tooprevent szemétgyűjtési adatok hello eredmények között, a jelen nyilatkozat közli struktúra, hogy azt kell csak vissza adatokat fájlok végződése. napló.</span><span class="sxs-lookup"><span data-stu-id="23f32-161">tooprevent garbage data in hello results, this statement tells Hive that we should only return data from files ending in .log.</span></span>

  > [!NOTE]
  > <span data-ttu-id="23f32-162">Külső táblák kell használni, amikor hello frissíteni az külső forrás alapjául szolgáló adatok toobe várt.</span><span class="sxs-lookup"><span data-stu-id="23f32-162">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="23f32-163">Például egy automatizált adatok feltöltési folyamat vagy a MapReduce művelethez.</span><span class="sxs-lookup"><span data-stu-id="23f32-163">For example, an automated data upload process or a MapReduce operation.</span></span>
  >
  > <span data-ttu-id="23f32-164">A külső tábla eldobása does **nem** törölni hello adatokat, csak a hello tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="23f32-164">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>

    <span data-ttu-id="23f32-165">hello kimeneti, a parancs van hasonló toohello a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="23f32-165">hello output of this command is similar toohello following text:</span></span>

        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :

        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)

        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

5. <span data-ttu-id="23f32-166">tooexit Beeline, használjon `!exit`.</span><span class="sxs-lookup"><span data-stu-id="23f32-166">tooexit Beeline, use `!exit`.</span></span>

## <span data-ttu-id="23f32-167"><a id="file"></a>Beeline toorun HiveQL fájl használata</span><span class="sxs-lookup"><span data-stu-id="23f32-167"><a id="file"></a>Use Beeline toorun a HiveQL file</span></span>

<span data-ttu-id="23f32-168">A következő lépéseket toocreate egy fájlt, majd futtassa azt Beeline segítségével hello használata.</span><span class="sxs-lookup"><span data-stu-id="23f32-168">Use hello following steps toocreate a file, then run it using Beeline.</span></span>

1. <span data-ttu-id="23f32-169">Használjon hello következő parancsot a toocreate nevű fájlt **query.hql**:</span><span class="sxs-lookup"><span data-stu-id="23f32-169">Use hello following command toocreate a file named **query.hql**:</span></span>

    ```bash
    nano query.hql
    ```

2. <span data-ttu-id="23f32-170">Szöveg hello hello fájl tartalmát, a következő hello használata.</span><span class="sxs-lookup"><span data-stu-id="23f32-170">Use hello following text as hello contents of hello file.</span></span> <span data-ttu-id="23f32-171">Ez a lekérdezés táblázatot hoz létre új "belső" nevű **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="23f32-171">This query creates a new 'internal' table named **errorLogs**:</span></span>

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    <span data-ttu-id="23f32-172">Ezekre az utasításokra hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="23f32-172">These statements perform hello following actions:</span></span>

    * <span data-ttu-id="23f32-173">**Hozzon létre Ha nem létezik táblázat** -hello tábla még nem létezik, ha a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="23f32-173">**CREATE TABLE IF NOT EXISTS** - If hello table does not already exist, it is created.</span></span> <span data-ttu-id="23f32-174">Hello óta **külső** kulcsszó nem használható, a jelen nyilatkozat egy belső táblát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="23f32-174">Since hello **EXTERNAL** keyword is not used, this statement creates an internal table.</span></span> <span data-ttu-id="23f32-175">Belső tábla hello Hive adatraktár tárolódnak, és Hive teljesen kezeli.</span><span class="sxs-lookup"><span data-stu-id="23f32-175">Internal tables are stored in hello Hive data warehouse and are managed completely by Hive.</span></span>
    * <span data-ttu-id="23f32-176">**TÁROLT AS ORC** -hello eltárolja optimalizált sor oszlopos (ORC) formátumban.</span><span class="sxs-lookup"><span data-stu-id="23f32-176">**STORED AS ORC** - Stores hello data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="23f32-177">ORC Hive adattárolás magas optimalizált és hatékony formátuma érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="23f32-177">ORC format is a highly optimized and efficient format for storing Hive data.</span></span>
    * <span data-ttu-id="23f32-178">**ÍRJA FELÜL AZ INSERT... Válassza ki** -sorok kiválaszt hello **log4jLogs** tartalmazó **[hiba]**, majd Beszúrások adatok hello hello be **errorLogs** tábla.</span><span class="sxs-lookup"><span data-stu-id="23f32-178">**INSERT OVERWRITE ... SELECT** - Selects rows from hello **log4jLogs** table that contain **[ERROR]**, then inserts hello data into hello **errorLogs** table.</span></span>

    > [!NOTE]
    > <span data-ttu-id="23f32-179">Külső táblák eltérően eldobását egy belső tábla törlése hello, valamint az alapul szolgáló adatokat.</span><span class="sxs-lookup"><span data-stu-id="23f32-179">Unlike external tables, dropping an internal table deletes hello underlying data as well.</span></span>

3. <span data-ttu-id="23f32-180">toosave hello fájl használata **Ctrl**+**_X**, majd adja meg **Y**, és végül **Enter**.</span><span class="sxs-lookup"><span data-stu-id="23f32-180">toosave hello file, use **Ctrl**+**_X**, then enter **Y**, and finally **Enter**.</span></span>

4. <span data-ttu-id="23f32-181">Használja a következő toorun hello fájl használatával Beeline hello:</span><span class="sxs-lookup"><span data-stu-id="23f32-181">Use hello following toorun hello file using Beeline:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]
    > <span data-ttu-id="23f32-182">Hello `-i` paraméter Beeline elindul, a futtatása hello utasítások hello query.hql fájlban.</span><span class="sxs-lookup"><span data-stu-id="23f32-182">hello `-i` parameter starts Beeline, runs hello statements in hello query.hql file.</span></span> <span data-ttu-id="23f32-183">Miután hello lekérdezés befejeződött, megérkezik a hello `jdbc:hive2://headnodehost:10001/>` kérdés.</span><span class="sxs-lookup"><span data-stu-id="23f32-183">Once hello query completes, you arrive at hello `jdbc:hive2://headnodehost:10001/>` prompt.</span></span> <span data-ttu-id="23f32-184">Is futtathatja egy fájlt, hello `-f` paraméter, amely Beeline kilép hello lekérdezés befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="23f32-184">You can also run a file using hello `-f` parameter, which exits Beeline after hello query completes.</span></span>

5. <span data-ttu-id="23f32-185">amely hello tooverify **errorLogs** táblázat létrejött, használja a következő utasítás tooreturn összes sorát hello hello **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="23f32-185">tooverify that hello **errorLogs** table was created, use hello following statement tooreturn all hello rows from **errorLogs**:</span></span>

    ```hiveql
    SELECT * from errorLogs;
    ```

    <span data-ttu-id="23f32-186">Három sor adat vissza kell adni, az összes tartalmazó **[hiba]** az oszlop t4:</span><span class="sxs-lookup"><span data-stu-id="23f32-186">Three rows of data should be returned, all containing **[ERROR]** in column t4:</span></span>

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <span data-ttu-id="23f32-187"><a id="remote"></a>Beeline távolról használható</span><span class="sxs-lookup"><span data-stu-id="23f32-187"><a id="remote"></a>Use Beeline remotely</span></span>

<span data-ttu-id="23f32-188">Ha helyileg telepített Beeline, vagy használja a Docker-lemezkép használatával, mint [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/), a következő paraméterek hello kell használnia:</span><span class="sxs-lookup"><span data-stu-id="23f32-188">If you have Beeline installed locally, or are using it through a Docker image such as [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/), you must use hello following parameters:</span></span>

* <span data-ttu-id="23f32-189">__Kapcsolati karakterlánc__:`-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`</span><span class="sxs-lookup"><span data-stu-id="23f32-189">__Connection string__: `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`</span></span>

* <span data-ttu-id="23f32-190">__Fürt bejelentkezési neve__:`-n admin`</span><span class="sxs-lookup"><span data-stu-id="23f32-190">__Cluster login name__: `-n admin`</span></span>

* <span data-ttu-id="23f32-191">__Fürt bejelentkezési jelszó__`-p 'password'`</span><span class="sxs-lookup"><span data-stu-id="23f32-191">__Cluster login password__ `-p 'password'`</span></span>

<span data-ttu-id="23f32-192">Cserélje le a hello `clustername` a HDInsight-fürt hello nevű hello kapcsolódási karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="23f32-192">Replace hello `clustername` in hello connection string with hello name of your HDInsight cluster.</span></span>

<span data-ttu-id="23f32-193">Cserélje le `admin` hello nevet, a fürt bejelentkezési és a név felülírandó `password` hello jelszóval a fürt bejelentkezési azonosítóhoz.</span><span class="sxs-lookup"><span data-stu-id="23f32-193">Replace `admin` with hello name of your cluster login, and replace `password` with hello password for your cluster login.</span></span>

## <span data-ttu-id="23f32-194"><a id="sparksql"></a>Beeline használata Spark</span><span class="sxs-lookup"><span data-stu-id="23f32-194"><a id="sparksql"></a>Use Beeline with Spark</span></span>

<span data-ttu-id="23f32-195">Spark hiveserver2-n, ami általában beruházások tooas hello Spark Thrift-kiszolgáló a saját végrehajtásának biztosít.</span><span class="sxs-lookup"><span data-stu-id="23f32-195">Spark provides its own implementation of HiveServer2, which is often refered tooas hello Spark Thrift server.</span></span> <span data-ttu-id="23f32-196">Ez a szolgáltatás az Spark SQL tooresolve lekérdezések Hive helyett, és attól függően, hogy a lekérdezés jobb teljesítményt biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="23f32-196">This service uses Spark SQL tooresolve queries instead of Hive, and may provide better performance depending on your query.</span></span>

<span data-ttu-id="23f32-197">tooconnect toohello Spark Thrift-kiszolgáló, a Spark on HDInsight-fürt használata port `10002` helyett `10001`.</span><span class="sxs-lookup"><span data-stu-id="23f32-197">tooconnect toohello Spark Thrift server of a Spark on HDInsight cluster, use port `10002` instead of `10001`.</span></span> <span data-ttu-id="23f32-198">Például: `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.</span><span class="sxs-lookup"><span data-stu-id="23f32-198">For example, `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="23f32-199">hello Spark Thrift-kiszolgáló nem érhető el over internet hello közvetlen van.</span><span class="sxs-lookup"><span data-stu-id="23f32-199">hello Spark Thrift server is not directly accessible over hello internet.</span></span> <span data-ttu-id="23f32-200">Csak tooit csatlakozzon az SSH-munkamenetet, vagy belül hello azonos Azure virtuális hálózatban, a HDInsight-fürt hello.</span><span class="sxs-lookup"><span data-stu-id="23f32-200">You can only connect tooit from an SSH session or inside hello same Azure Virtual Network as hello HDInsight cluster.</span></span>

## <span data-ttu-id="23f32-201"><a id="summary"></a><a id="nextsteps"></a>További lépések</span><span class="sxs-lookup"><span data-stu-id="23f32-201"><a id="summary"></a><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="23f32-202">A Hive HDInsight további általános információkért tekintse meg a következő dokumentum hello:</span><span class="sxs-lookup"><span data-stu-id="23f32-202">For more general information on Hive in HDInsight, see hello following document:</span></span>

* [<span data-ttu-id="23f32-203">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="23f32-203">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="23f32-204">A HDInsight Hadoop dolgozhat egyéb lehetőségeiről további információkért lásd: a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="23f32-204">For more information on other ways you can work with Hadoop on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="23f32-205">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="23f32-205">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="23f32-206">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="23f32-206">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="23f32-207">Ha Tez Hive használ, tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="23f32-207">If you are using Tez with Hive, see hello following documents:</span></span>

* [<span data-ttu-id="23f32-208">A Windows-alapú HDInsight hello Tez felhasználói felület használata</span><span class="sxs-lookup"><span data-stu-id="23f32-208">Use hello Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="23f32-209">A Linux-alapú HDInsight Ambari Tez nézet hello használata</span><span class="sxs-lookup"><span data-stu-id="23f32-209">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
