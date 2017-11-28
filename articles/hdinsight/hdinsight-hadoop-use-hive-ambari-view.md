---
title: "a Hive hdinsight (Hadoop) - Azure aaaUse Ambari nézetek toowork |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello a webes böngésző toosubmit Hive-lekérdezéseket a Hive nézet. hello Hive View hello Ambari webes felhasználói felületén megadott a Linux-alapú HDInsight-fürt része."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1abe9104-f4b2-41b9-9161-abbc43de8294
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: f9a77b652e70d34a0ff9165fbb8c2e16d3401ae0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-hive-view-with-hadoop-in-hdinsight"></a><span data-ttu-id="8dfdf-104">A HDInsight Hadoop Hive View hello használata</span><span class="sxs-lookup"><span data-stu-id="8dfdf-104">Use hello Hive View with Hadoop in HDInsight</span></span>

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="8dfdf-105">Ismerje meg, hogyan toorun Hive lekérdezi az Ambari Hive nézet használatával.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-105">Learn how toorun Hive queries using Ambari Hive View.</span></span> <span data-ttu-id="8dfdf-106">Ambari egy felügyeleti és figyelési segédprogram Linux-alapú HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-106">Ambari is a management and monitoring utility provided with Linux-based HDInsight clusters.</span></span> <span data-ttu-id="8dfdf-107">Ambari keresztül elérhető hello szolgáltatások egyik webes felhasználói Felületet, amely használt toorun Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-107">One of hello features provided through Ambari is a Web UI that can be used toorun Hive queries.</span></span>

> [!NOTE]
> <span data-ttu-id="8dfdf-108">Ambari rendelkezik számos lényeges képességét, hogy ez a dokumentum nem ismerteti.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-108">Ambari has many capabilities that are not discussed in this document.</span></span> <span data-ttu-id="8dfdf-109">További információkért lásd: [kezelése HDInsight-fürtök hello Ambari webes felhasználói felület használatával](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="8dfdf-109">For more information, see [Manage HDInsight clusters by using hello Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8dfdf-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8dfdf-110">Prerequisites</span></span>

* <span data-ttu-id="8dfdf-111">A Linux-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-111">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="8dfdf-112">A fürt létrehozása információkért lásd: [Ismerkedés a Linux-alapú HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="8dfdf-112">For information on creating cluster, see [Get started with Linux-based HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8dfdf-113">hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-113">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="8dfdf-114">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-114">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="8dfdf-115">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="8dfdf-115">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="open-hello-hive-view"></a><span data-ttu-id="8dfdf-116">Nyissa meg a hello Hive nézete</span><span class="sxs-lookup"><span data-stu-id="8dfdf-116">Open hello Hive view</span></span>

<span data-ttu-id="8dfdf-117">Az Ambari nézetek teheti a hello Azure-portálon; Válassza ki a HDInsight-fürt majd **Ambari nézetek** a hello **Gyorshivatkozások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-117">You can Ambari Views from hello Azure portal; select your HDInsight cluster and then select **Ambari Views** from hello **Quick Links** section.</span></span>

![Gyorshivatkozások szakasz hello portál](./media/hdinsight-hadoop-use-hive-ambari-view/quicklinks.png)

<span data-ttu-id="8dfdf-119">Nézetek hello listáról válassza ki a hello __Hive View__.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-119">From hello list of views, select hello __Hive View__.</span></span>

![hello kijelölt Hive nézete](./media/hdinsight-hadoop-use-hive-ambari-view/select-hive-view.png)

> [!NOTE]
> <span data-ttu-id="8dfdf-121">Ambari elérésekor felszólító tooauthenticate toohello hely áll.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-121">When accessing Ambari, you are prompted tooauthenticate toohello site.</span></span> <span data-ttu-id="8dfdf-122">Adja meg a Üdvözöljük a rendszergazdákat (alapértelmezett `admin`) fiók nevét és jelszavát létrehozásakor használt hello fürt.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-122">Enter hello admin (default `admin`) account name and password you used when creating hello cluster.</span></span>

<span data-ttu-id="8dfdf-123">A kép a következő lap hasonló toohello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="8dfdf-123">You should see a page similar toohello following image:</span></span>

![Hello lekérdezés munkalap hello Hive nézet képe](./media/hdinsight-hadoop-use-hive-ambari-view/ambari-hive-view.png)

## <span data-ttu-id="8dfdf-125"><a name="hivequery"></a>A lekérdezés futtatása</span><span class="sxs-lookup"><span data-stu-id="8dfdf-125"><a name="hivequery"></a>Run a query</span></span>

<span data-ttu-id="8dfdf-126">toorun hive-lekérdezések használata hello hello Hive nézete a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-126">toorun a hive query, use hello following steps from hello Hive view.</span></span>

1. <span data-ttu-id="8dfdf-127">A hello __lekérdezés__ lapra, illessze be a következő HiveQL utasítások hello munkalapra hello:</span><span class="sxs-lookup"><span data-stu-id="8dfdf-127">From hello __Query__ tab, paste hello following HiveQL statements into hello worksheet:</span></span>

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
    ```

    <span data-ttu-id="8dfdf-128">Ezekre az utasításokra hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="8dfdf-128">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="8dfdf-129">`DROP TABLE`-Hello tábla és törlése hello adatfájlt, abban az esetben hello tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-129">`DROP TABLE` - Deletes hello table and hello data file, in case hello table already exists.</span></span>

   * <span data-ttu-id="8dfdf-130">`CREATE EXTERNAL TABLE`-Táblát hoz létre egy új "external" struktúra.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-130">`CREATE EXTERNAL TABLE` - Creates a new "external" table in Hive.</span></span>
   <span data-ttu-id="8dfdf-131">Külső táblák csak hello tábladefiníció Hive tárolja.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-131">External tables store only hello table definition in Hive.</span></span> <span data-ttu-id="8dfdf-132">hello adatok hello eredeti helyen marad.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-132">hello data is left in hello original location.</span></span>

   * <span data-ttu-id="8dfdf-133">`ROW FORMAT`-Hello adatok formázását.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-133">`ROW FORMAT` - How hello data is formatted.</span></span> <span data-ttu-id="8dfdf-134">Ebben az esetben az egyes naplókon hello mezők szóközzel elválasztva.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-134">In this case, hello fields in each log are separated by a space.</span></span>

   * <span data-ttu-id="8dfdf-135">`STORED AS TEXTFILE LOCATION`-Hello adatok tárolására, és szövegként tárolt.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-135">`STORED AS TEXTFILE LOCATION` - Where hello data is stored, and that it is stored as text.</span></span>

   * <span data-ttu-id="8dfdf-136">`SELECT`-Kijelöli az adott oszlop t4 hello értéket [hiba] tartalmaz minden sorok számát.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-136">`SELECT` - Selects a count of all rows where column t4 contains hello value [ERROR].</span></span>

     > [!NOTE]
     > <span data-ttu-id="8dfdf-137">Külső táblák kell használni, amikor hello frissíteni az külső forrás alapjául szolgáló adatok toobe várt.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-137">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="8dfdf-138">Például egy automatizált adatfeltöltés eljárásra, vagy egy másik MapReduce művelethez.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-138">For example, an automated data upload process, or by another MapReduce operation.</span></span> <span data-ttu-id="8dfdf-139">A külső tábla eldobása does *nem* törölni hello adatokat, csak a hello tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-139">Dropping an external table does *not* delete hello data, only hello table definition.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="8dfdf-140">Hagyja hello __adatbázis__ kiválasztottat __alapértelmezett__.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-140">Leave hello __Database__ selection at __default__.</span></span> <span data-ttu-id="8dfdf-141">e dokumentumban szereplő példák hello hello alapértelmezett adatbázis tartalmazza a hdinsight eszközzel használható.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-141">hello examples in this document use hello default database included with HDInsight.</span></span>

2. <span data-ttu-id="8dfdf-142">toostart hello lekérdezés használata hello **Execute** hello munkalap gombra.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-142">toostart hello query, use hello **Execute** button below hello worksheet.</span></span> <span data-ttu-id="8dfdf-143">Narancssárga és hello szövegének változásait túl változik**leállítása**.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-143">It turns orange and hello text changes too**Stop**.</span></span>

3. <span data-ttu-id="8dfdf-144">Miután hello lekérdezés befejeződött, hello **eredmények** lap hello művelet hello eredményeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-144">Once hello query has finished, hello **Results** tab displays hello results of hello operation.</span></span> <span data-ttu-id="8dfdf-145">a következő szöveg hello hello hello lekérdezés eredményében:</span><span class="sxs-lookup"><span data-stu-id="8dfdf-145">hello following text is hello result of hello query:</span></span>

        sev       cnt
        [ERROR]   3

    <span data-ttu-id="8dfdf-146">Hello **naplók** lapon használt tooview hello naplózási információk hello feladat által létrehozott lehet.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-146">hello **Logs** tab can be used tooview hello logging information created by hello job.</span></span>

   > [!TIP]
   > <span data-ttu-id="8dfdf-147">Hello **-eredményeket menteni** hello felső legördülő párbeszédpanelje hello a bal oldali **lekérdezési folyamat eredményei** szakasz lehetővé teszi a toodownload vagy -eredményeket menteni.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-147">hello **Save results** drop-down dialog in hello upper left of hello **Query Process Results** section allows you toodownload or save results.</span></span>

4. <span data-ttu-id="8dfdf-148">Válassza ki a négy hello a lekérdezés, majd válassza ki **Execute**.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-148">Select hello first four lines of this query, then select **Execute**.</span></span> <span data-ttu-id="8dfdf-149">Figyelje meg, hogy nincsenek eredmény hello feladat befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-149">Notice that there are no results when hello job completes.</span></span> <span data-ttu-id="8dfdf-150">Hello segítségével **Execute** hello lekérdezés részeként gomb kiválasztott csak futtatása hello kijelölt utasításokat.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-150">Using hello **Execute** button when part of hello query is selected only runs hello selected statements.</span></span> <span data-ttu-id="8dfdf-151">Ebben az esetben a hello kijelölés nem tartozik hello végső utasítás, amely sorok hello táblából.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-151">In this case, hello selection didn't include hello final statement that retrieves rows from hello table.</span></span> <span data-ttu-id="8dfdf-152">Ha csak a sor választja, és használjon **Execute**, hello várt eredményeket kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-152">If you select just that line and use **Execute**, you should see hello expected results.</span></span>

5. <span data-ttu-id="8dfdf-153">a munkalap tooadd hello használata **új munkalapra lesznek** hello hello alján gomb **Lekérdezésszerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-153">tooadd a worksheet, use hello **New Worksheet** button at hello bottom of hello **Query Editor**.</span></span> <span data-ttu-id="8dfdf-154">Adja meg a következő HiveQL utasítások hello hello új munkalapon:</span><span class="sxs-lookup"><span data-stu-id="8dfdf-154">In hello new worksheet, enter hello following HiveQL statements:</span></span>

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
    ```

  <span data-ttu-id="8dfdf-155">Ezekre az utasításokra hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="8dfdf-155">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="8dfdf-156">**Hozzon létre Ha nem létezik táblázat** -táblázatot hoz létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-156">**CREATE TABLE IF NOT EXISTS** - Creates a table, if it does not already exist.</span></span> <span data-ttu-id="8dfdf-157">Hello óta **külső** kulcsszó nem használható, egy belső tábla jön létre.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-157">Since hello **EXTERNAL** keyword is not used, an internal table is created.</span></span> <span data-ttu-id="8dfdf-158">Egy belső tábla hello Hive-adatraktárban tárolja, és Hive teljesen kezeli.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-158">An internal table is stored in hello Hive data warehouse and is managed completely by Hive.</span></span> <span data-ttu-id="8dfdf-159">Külső táblák eltérően eldobását egy belső tábla törlése hello, valamint az alapul szolgáló adatokat.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-159">Unlike external tables, dropping an internal table deletes hello underlying data as well.</span></span>

   * <span data-ttu-id="8dfdf-160">**TÁROLT AS ORC** -hello eltárolja optimalizált sor oszlopos (ORC) formátumban.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-160">**STORED AS ORC** - Stores hello data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="8dfdf-161">ORC formátuma egy magas optimalizált és hatékony Hive adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-161">ORC is a highly optimized and efficient format for storing Hive data.</span></span>

   * <span data-ttu-id="8dfdf-162">**ÍRJA FELÜL AZ INSERT... Válassza ki** -sorok kiválaszt hello **log4jLogs** tartalmazó `[ERROR]`, majd beszúrása hello hello az adatok és **errorLogs** tábla.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-162">**INSERT OVERWRITE ... SELECT** - Selects rows from hello **log4jLogs** table that contain `[ERROR]`, and then inserts hello data into hello **errorLogs** table.</span></span>

     <span data-ttu-id="8dfdf-163">Használjon hello **Execute** toorun gombra a lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-163">Use hello **Execute** button toorun this query.</span></span> <span data-ttu-id="8dfdf-164">Hello **eredmények** lap tartalmaz adatokat, amikor hello lekérdezés nulla sort adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-164">hello **Results** tab does not contain any information when hello query returns zero rows.</span></span> <span data-ttu-id="8dfdf-165">hello kell állapota **sikeres** hello lekérdezés befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-165">hello status should show as **SUCCEEDED** once hello query completes.</span></span>

### <a name="visual-explain"></a><span data-ttu-id="8dfdf-166">Visual ismertetik.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-166">Visual explain</span></span>

<span data-ttu-id="8dfdf-167">hello lekérdezésterv, jelölje be hello a képi megjelenítés toodisplay **Visual ismertetik** alatt hello munkalap fülre.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-167">toodisplay a visualization of hello query plan, select hello **Visual Explain** tab below hello worksheet.</span></span>

<span data-ttu-id="8dfdf-168">Hello **Visual ismertetik** hello lekérdezés lehet, az összetett lekérdezések hello folyamata hasznos információkat.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-168">hello **Visual Explain** view of hello query can be helpful in understanding hello flow of complex queries.</span></span> <span data-ttu-id="8dfdf-169">Ez a nézet a szöveges megfelelőjét hello segítségével tekintheti **magyarázat** hello Lekérdezésszerkesztő gombjára.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-169">You can view a textual equivalent of this view by using hello **Explain** button in hello Query Editor.</span></span>

### <a name="tez-ui"></a><span data-ttu-id="8dfdf-170">Tez felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="8dfdf-170">Tez UI</span></span>

<span data-ttu-id="8dfdf-171">toodisplay Tez felhasználói felület hello hello lekérdezés, jelölje be hello **Tez** alatt hello munkalap fülre.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-171">toodisplay hello Tez UI for hello query, select hello **Tez** tab below hello worksheet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8dfdf-172">Tez, ezért nem használható tooresolve összes.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-172">Tez is not used tooresolve all queries.</span></span> <span data-ttu-id="8dfdf-173">Több lekérdezést orvosolni tudja Tez használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-173">Many queries can be resolved without using Tez.</span></span> 

<span data-ttu-id="8dfdf-174">Ha a Tez nem használt tooresolve hello lekérdezés, hello irányított aciklikus diagramhoz (DAG) jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-174">If Tez was used tooresolve hello query, hello Directed Acyclic Graph (DAG) is displayed.</span></span> <span data-ttu-id="8dfdf-175">Ha a lekérdezések már futott az elmúlt hello, vagy a hibakeresési hello Tez folyamat kívánja tooview hello DAG, használja hello [Tez nézet](hdinsight-debug-ambari-tez-view.md) helyette.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-175">If you want tooview hello DAG for queries you've ran in hello past, or debug hello Tez process, use hello [Tez View](hdinsight-debug-ambari-tez-view.md) instead.</span></span>

## <a name="view-job-history"></a><span data-ttu-id="8dfdf-176">Feladatelőzmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="8dfdf-176">View job history</span></span>

<span data-ttu-id="8dfdf-177">Hello __feladatok__ lapon Hive-lekérdezések előzményeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-177">hello __Jobs__ tab displays a history of Hive queries.</span></span>

![Hello feladatelőzményekben képe](./media/hdinsight-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a><span data-ttu-id="8dfdf-179">Adatbázistáblák</span><span class="sxs-lookup"><span data-stu-id="8dfdf-179">Database tables</span></span>

<span data-ttu-id="8dfdf-180">Használhatja a hello __táblák__ Hive adatbázisban lévő táblák toowork fülre.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-180">You can use hello __Tables__ tab toowork with tables within a Hive database.</span></span>

![Hello táblák lap képe](./media/hdinsight-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a><span data-ttu-id="8dfdf-182">Lekérdezések</span><span class="sxs-lookup"><span data-stu-id="8dfdf-182">Saved queries</span></span>

<span data-ttu-id="8dfdf-183">Hello lekérdezés lap, a lekérdezések opcionálisan mentheti.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-183">From hello Query tab, you can optionally save queries.</span></span> <span data-ttu-id="8dfdf-184">Miután menti, felhasználhatja hello hello lekérdezést __mentett lekérdezések__ fülre.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-184">Once saved, you can reuse hello query from hello __Saved Queries__ tab.</span></span>

![Lekérdezések lap képe](./media/hdinsight-hadoop-use-hive-ambari-view/saved-queries.png)

## <a name="user-defined-functions"></a><span data-ttu-id="8dfdf-186">Felhasználó által definiált függvények</span><span class="sxs-lookup"><span data-stu-id="8dfdf-186">User-defined functions</span></span>

<span data-ttu-id="8dfdf-187">Hive is kiterjeszthető felhasználói függvény (UDF) keresztül.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-187">Hive can also be extended through user-defined functions (UDF).</span></span> <span data-ttu-id="8dfdf-188">Egy UDF lehetővé teszi tooimplement funkcionalitással kapcsolatban, vagy nem könnyen modellezve a HiveQL logika.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-188">A UDF allows you tooimplement functionality or logic that isn't easily modeled in HiveQL.</span></span>

<span data-ttu-id="8dfdf-189">hello UDF lapon hello Hive View hello tetején toodeclare lehetővé teszi, és mentse a felhasználó által megadott függvények készlete.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-189">hello UDF tab at hello top of hello Hive View allows you toodeclare and save a set of UDFs.</span></span> <span data-ttu-id="8dfdf-190">A felhasználó által megadott függvények használhatók hello **Lekérdezésszerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-190">These UDFs can be used with hello **Query Editor**.</span></span>

![Az UDF lap képe](./media/hdinsight-hadoop-use-hive-ambari-view/user-defined-functions.png)

<span data-ttu-id="8dfdf-192">Egy UDF toohello Hive nézet hozzáadása után egy **helyezze be a felhasználó által megadott függvények** hello hello alján megjelenik gomb **Lekérdezésszerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-192">Once you have added a UDF toohello Hive View, an **Insert udfs** button appears at hello bottom of hello **Query Editor**.</span></span> <span data-ttu-id="8dfdf-193">Látható értesítések valamelyikének kiválasztásakor Ez a bejegyzés a legördülő listából válassza ki a felhasználó által megadott függvények hello Hive View definiált hello.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-193">Selecting this entry displays a drop-down list of hello UDFs defined in hello Hive View.</span></span> <span data-ttu-id="8dfdf-194">Egy UDF választva a rendszer hozzáadja HiveQL utasítások tooyour lekérdezés tooenable hello UDF.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-194">Selecting a UDF adds HiveQL statements tooyour query tooenable hello UDF.</span></span>

<span data-ttu-id="8dfdf-195">Ha például egy UDF definiált a következő tulajdonságai hello:</span><span class="sxs-lookup"><span data-stu-id="8dfdf-195">For example, if you have defined a UDF with hello following properties:</span></span>

* <span data-ttu-id="8dfdf-196">Az erőforrásnév: myudfs</span><span class="sxs-lookup"><span data-stu-id="8dfdf-196">Resource name: myudfs</span></span>

* <span data-ttu-id="8dfdf-197">Erőforrás elérési útja: /myudfs.jar</span><span class="sxs-lookup"><span data-stu-id="8dfdf-197">Resource path: /myudfs.jar</span></span>

* <span data-ttu-id="8dfdf-198">Az UDF-name: myawesomeudf</span><span class="sxs-lookup"><span data-stu-id="8dfdf-198">UDF name: myawesomeudf</span></span>

* <span data-ttu-id="8dfdf-199">Az UDF osztálynév: com.myudfs.Awesome</span><span class="sxs-lookup"><span data-stu-id="8dfdf-199">UDF class name: com.myudfs.Awesome</span></span>

<span data-ttu-id="8dfdf-200">Hello segítségével **helyezze be a felhasználó által megadott függvények** gomb megjelenik egy bejegyzés nevű **myudfs**, egy másik legördülő minden UDF definiálva az adott erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-200">Using hello **Insert udfs** button displays an entry named **myudfs**, with another drop-down for each UDF defined for that resource.</span></span> <span data-ttu-id="8dfdf-201">Ebben az esetben **myawesomeudf**.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-201">In this case, **myawesomeudf**.</span></span> <span data-ttu-id="8dfdf-202">Ez a bejegyzés választva a rendszer hozzáadja a következő hello lekérdezés toohello elejére hello:</span><span class="sxs-lookup"><span data-stu-id="8dfdf-202">Selecting this entry adds hello following toohello beginning of hello query:</span></span>

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

<span data-ttu-id="8dfdf-203">A lekérdezés hello UDF használhatja.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-203">You can then use hello UDF in your query.</span></span> <span data-ttu-id="8dfdf-204">Például: `SELECT myawesomeudf(name) FROM people;`.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-204">For example, `SELECT myawesomeudf(name) FROM people;`.</span></span>

<span data-ttu-id="8dfdf-205">A felhasználó által megadott függvények használata a HDInsight Hive további információkért tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="8dfdf-205">For more information on using UDFs with Hive on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="8dfdf-206">Python használata a Hive és a Pig a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="8dfdf-206">Using Python with Hive and Pig in HDInsight</span></span>](hdinsight-python.md)
* [<span data-ttu-id="8dfdf-207">Hogyan tooadd egy egyéni Hive UDF tooHDInsight</span><span class="sxs-lookup"><span data-stu-id="8dfdf-207">How tooadd a custom Hive UDF tooHDInsight</span></span>](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a><span data-ttu-id="8dfdf-208">Hive-beállítások</span><span class="sxs-lookup"><span data-stu-id="8dfdf-208">Hive settings</span></span>

<span data-ttu-id="8dfdf-209">Beállítások is lehet használt toochange különböző Hive-beállításokat.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-209">Settings can be used toochange various Hive settings.</span></span> <span data-ttu-id="8dfdf-210">Például módosítása hello végrehajtó motorja a Hive Tez (hello alapértelmezett) tooMapReduce.</span><span class="sxs-lookup"><span data-stu-id="8dfdf-210">For example, changing hello execution engine for Hive from Tez (hello default) tooMapReduce.</span></span>

## <span data-ttu-id="8dfdf-211"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8dfdf-211"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="8dfdf-212">Általános információk a hdinsight Hive:</span><span class="sxs-lookup"><span data-stu-id="8dfdf-212">For general information on Hive in HDInsight:</span></span>

* [<span data-ttu-id="8dfdf-213">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="8dfdf-213">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="8dfdf-214">Az egyéb lehetőségeiről a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="8dfdf-214">For information on other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="8dfdf-215">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="8dfdf-215">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="8dfdf-216">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="8dfdf-216">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
