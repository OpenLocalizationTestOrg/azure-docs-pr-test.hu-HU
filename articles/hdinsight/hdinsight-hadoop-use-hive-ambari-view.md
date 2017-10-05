---
title: "Az Ambari nézetek használhatja a Hive hdinsight (Hadoop) - Azure |} Microsoft Docs"
description: "Útmutató a Hive View webböngészőből elküldeni a Hive-lekérdezéseket. A Hive nézet az Ambari webes felhasználói felületén megadott a Linux-alapú HDInsight-fürt része."
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
ms.openlocfilehash: 80df3da4d62feb814ea2dd97c96e57954093c5c5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-the-hive-view-with-hadoop-in-hdinsight"></a><span data-ttu-id="663f7-104">A Hive nézet használata a hadooppal a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="663f7-104">Use the Hive View with Hadoop in HDInsight</span></span>

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="663f7-105">Megtudhatja, hogyan futtathat Hive-lekérdezéseket Ambari Hive nézet használatával.</span><span class="sxs-lookup"><span data-stu-id="663f7-105">Learn how to run Hive queries using Ambari Hive View.</span></span> <span data-ttu-id="663f7-106">Ambari egy felügyeleti és figyelési segédprogram Linux-alapú HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="663f7-106">Ambari is a management and monitoring utility provided with Linux-based HDInsight clusters.</span></span> <span data-ttu-id="663f7-107">Az Ambari keresztül elérhető szolgáltatások egyik webes felhasználói Felületet, amely segítségével futtathat Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="663f7-107">One of the features provided through Ambari is a Web UI that can be used to run Hive queries.</span></span>

> [!NOTE]
> <span data-ttu-id="663f7-108">Ambari rendelkezik számos lényeges képességét, hogy ez a dokumentum nem ismerteti.</span><span class="sxs-lookup"><span data-stu-id="663f7-108">Ambari has many capabilities that are not discussed in this document.</span></span> <span data-ttu-id="663f7-109">További információkért lásd: [kezelése HDInsight-fürtök az Ambari webes felhasználói felület használatával](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="663f7-109">For more information, see [Manage HDInsight clusters by using the Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="663f7-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="663f7-110">Prerequisites</span></span>

* <span data-ttu-id="663f7-111">A Linux-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="663f7-111">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="663f7-112">A fürt létrehozása információkért lásd: [Ismerkedés a Linux-alapú HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="663f7-112">For information on creating cluster, see [Get started with Linux-based HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="663f7-113">A jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="663f7-113">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="663f7-114">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="663f7-114">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="663f7-115">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="663f7-115">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="open-the-hive-view"></a><span data-ttu-id="663f7-116">Nyissa meg a Hive nézete</span><span class="sxs-lookup"><span data-stu-id="663f7-116">Open the Hive view</span></span>

<span data-ttu-id="663f7-117">Az Ambari nézetek is az Azure portálról; Válassza ki a HDInsight-fürt majd **Ambari nézetek** a a **Gyorshivatkozások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="663f7-117">You can Ambari Views from the Azure portal; select your HDInsight cluster and then select **Ambari Views** from the **Quick Links** section.</span></span>

![a portál Gyorshivatkozások szakasz](./media/hdinsight-hadoop-use-hive-ambari-view/quicklinks.png)

<span data-ttu-id="663f7-119">Válassza ki a listáról a nézetek, a __Hive View__.</span><span class="sxs-lookup"><span data-stu-id="663f7-119">From the list of views, select the __Hive View__.</span></span>

![A kiválasztott Hive nézete](./media/hdinsight-hadoop-use-hive-ambari-view/select-hive-view.png)

> [!NOTE]
> <span data-ttu-id="663f7-121">Ambari elérésekor a rendszer kéri, hogy a hely hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="663f7-121">When accessing Ambari, you are prompted to authenticate to the site.</span></span> <span data-ttu-id="663f7-122">Adja meg a rendszergazdai (alapértelmezett `admin`) nevét és jelszavát, a fürt létrehozásakor használt fiókot.</span><span class="sxs-lookup"><span data-stu-id="663f7-122">Enter the admin (default `admin`) account name and password you used when creating the cluster.</span></span>

<span data-ttu-id="663f7-123">Az alábbi képen hasonló lap kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="663f7-123">You should see a page similar to the following image:</span></span>

![A lekérdezés munkalap a Hive nézet képe](./media/hdinsight-hadoop-use-hive-ambari-view/ambari-hive-view.png)

## <span data-ttu-id="663f7-125"><a name="hivequery"></a>A lekérdezés futtatása</span><span class="sxs-lookup"><span data-stu-id="663f7-125"><a name="hivequery"></a>Run a query</span></span>

<span data-ttu-id="663f7-126">Hive-lekérdezések futtatásához használja a következő lépéseket a Hive nézetből.</span><span class="sxs-lookup"><span data-stu-id="663f7-126">To run a hive query, use the following steps from the Hive view.</span></span>

1. <span data-ttu-id="663f7-127">Az a __lekérdezés__ lapon, a következő hiveql illessze be a munkalapra:</span><span class="sxs-lookup"><span data-stu-id="663f7-127">From the __Query__ tab, paste the following HiveQL statements into the worksheet:</span></span>

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
    ```

    <span data-ttu-id="663f7-128">Ezekre az utasításokra hajtsa végre a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="663f7-128">These statements perform the following actions:</span></span>

   * <span data-ttu-id="663f7-129">`DROP TABLE`-Törli a táblázat és az adatfájl, abban az esetben, ha a tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="663f7-129">`DROP TABLE` - Deletes the table and the data file, in case the table already exists.</span></span>

   * <span data-ttu-id="663f7-130">`CREATE EXTERNAL TABLE`-Táblát hoz létre egy új "external" struktúra.</span><span class="sxs-lookup"><span data-stu-id="663f7-130">`CREATE EXTERNAL TABLE` - Creates a new "external" table in Hive.</span></span>
   <span data-ttu-id="663f7-131">Külső táblák csak a tábladefiníció Hive tárolja.</span><span class="sxs-lookup"><span data-stu-id="663f7-131">External tables store only the table definition in Hive.</span></span> <span data-ttu-id="663f7-132">Az adatok marad az eredeti helyen.</span><span class="sxs-lookup"><span data-stu-id="663f7-132">The data is left in the original location.</span></span>

   * <span data-ttu-id="663f7-133">`ROW FORMAT`-Az adatok formázását.</span><span class="sxs-lookup"><span data-stu-id="663f7-133">`ROW FORMAT` - How the data is formatted.</span></span> <span data-ttu-id="663f7-134">Ebben az esetben a mezőket az egyes naplókon szóközzel elválasztva.</span><span class="sxs-lookup"><span data-stu-id="663f7-134">In this case, the fields in each log are separated by a space.</span></span>

   * <span data-ttu-id="663f7-135">`STORED AS TEXTFILE LOCATION`-Az adatok tárolására, és szövegként tárolt.</span><span class="sxs-lookup"><span data-stu-id="663f7-135">`STORED AS TEXTFILE LOCATION` - Where the data is stored, and that it is stored as text.</span></span>

   * <span data-ttu-id="663f7-136">`SELECT`-Kijelöli az adott oszlop t4 értéke [hiba] összes sorok számát.</span><span class="sxs-lookup"><span data-stu-id="663f7-136">`SELECT` - Selects a count of all rows where column t4 contains the value [ERROR].</span></span>

     > [!NOTE]
     > <span data-ttu-id="663f7-137">Külső táblák kell használni, amikor külső forrásból frissítenie kell az alapul szolgáló adatokat várt.</span><span class="sxs-lookup"><span data-stu-id="663f7-137">External tables should be used when you expect the underlying data to be updated by an external source.</span></span> <span data-ttu-id="663f7-138">Például egy automatizált adatfeltöltés eljárásra, vagy egy másik MapReduce művelethez.</span><span class="sxs-lookup"><span data-stu-id="663f7-138">For example, an automated data upload process, or by another MapReduce operation.</span></span> <span data-ttu-id="663f7-139">A külső tábla eldobása does *nem* törli az adatokat, csak a tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="663f7-139">Dropping an external table does *not* delete the data, only the table definition.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="663f7-140">Hagyja a __adatbázis__ kiválasztottat __alapértelmezett__.</span><span class="sxs-lookup"><span data-stu-id="663f7-140">Leave the __Database__ selection at __default__.</span></span> <span data-ttu-id="663f7-141">Az ebben a dokumentumban a példákban HDInsight tartozó alapértelmezett adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="663f7-141">The examples in this document use the default database included with HDInsight.</span></span>

2. <span data-ttu-id="663f7-142">A lekérdezés indításához használja a **Execute** a munkalap gombra.</span><span class="sxs-lookup"><span data-stu-id="663f7-142">To start the query, use the **Execute** button below the worksheet.</span></span> <span data-ttu-id="663f7-143">Narancssárga változik, és a szöveg a következőre változik **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="663f7-143">It turns orange and the text changes to **Stop**.</span></span>

3. <span data-ttu-id="663f7-144">Ha a lekérdezés befejeződött, a **eredmények** lap megjeleníti a művelet eredménye.</span><span class="sxs-lookup"><span data-stu-id="663f7-144">Once the query has finished, The **Results** tab displays the results of the operation.</span></span> <span data-ttu-id="663f7-145">A következő szöveget a lekérdezés eredménye:</span><span class="sxs-lookup"><span data-stu-id="663f7-145">The following text is the result of the query:</span></span>

        sev       cnt
        [ERROR]   3

    <span data-ttu-id="663f7-146">A **naplók** lap segítségével hozta létre a feldolgozás naplózási információk megtekintése.</span><span class="sxs-lookup"><span data-stu-id="663f7-146">The **Logs** tab can be used to view the logging information created by the job.</span></span>

   > [!TIP]
   > <span data-ttu-id="663f7-147">A **-eredményeket menteni** a bal felső legördülő párbeszédpanelen a **lekérdezési folyamat eredményei** szakasz lehetővé teszi, hogy töltse le vagy -eredményeket menteni.</span><span class="sxs-lookup"><span data-stu-id="663f7-147">The **Save results** drop-down dialog in the upper left of the **Query Process Results** section allows you to download or save results.</span></span>

4. <span data-ttu-id="663f7-148">Válassza ki a lekérdezés első négy sorokat, majd válasszon **Execute**.</span><span class="sxs-lookup"><span data-stu-id="663f7-148">Select the first four lines of this query, then select **Execute**.</span></span> <span data-ttu-id="663f7-149">Figyelje meg, hogy nincsenek eredmény a feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="663f7-149">Notice that there are no results when the job completes.</span></span> <span data-ttu-id="663f7-150">Használja a **Execute** gomb, amikor a lekérdezés van kijelölve, csak a kijelölt utasításokat futtat.</span><span class="sxs-lookup"><span data-stu-id="663f7-150">Using the **Execute** button when part of the query is selected only runs the selected statements.</span></span> <span data-ttu-id="663f7-151">Ebben az esetben a kijelölés nem tartozik a végső utasítás, kiolvassa a sorokat a táblában.</span><span class="sxs-lookup"><span data-stu-id="663f7-151">In this case, the selection didn't include the final statement that retrieves rows from the table.</span></span> <span data-ttu-id="663f7-152">Ha csak a sor választja, és használjon **Execute**, láthatja, hogy a kívánt eredmény elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="663f7-152">If you select just that line and use **Execute**, you should see the expected results.</span></span>

5. <span data-ttu-id="663f7-153">A munkalap hozzáadásához használja a **új munkalapra lesznek** gomb alján a **Lekérdezésszerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="663f7-153">To add a worksheet, use the **New Worksheet** button at the bottom of the **Query Editor**.</span></span> <span data-ttu-id="663f7-154">Az új munkalapra adja meg a következő hiveql:</span><span class="sxs-lookup"><span data-stu-id="663f7-154">In the new worksheet, enter the following HiveQL statements:</span></span>

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
    ```

  <span data-ttu-id="663f7-155">Ezekre az utasításokra hajtsa végre a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="663f7-155">These statements perform the following actions:</span></span>

   * <span data-ttu-id="663f7-156">**Hozzon létre Ha nem létezik táblázat** -táblázatot hoz létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="663f7-156">**CREATE TABLE IF NOT EXISTS** - Creates a table, if it does not already exist.</span></span> <span data-ttu-id="663f7-157">Mivel a **külső** kulcsszó nem használható, egy belső tábla jön létre.</span><span class="sxs-lookup"><span data-stu-id="663f7-157">Since the **EXTERNAL** keyword is not used, an internal table is created.</span></span> <span data-ttu-id="663f7-158">Egy belső tábla a Hive-adatraktárban tárolja, és teljesen kezeli a struktúra.</span><span class="sxs-lookup"><span data-stu-id="663f7-158">An internal table is stored in the Hive data warehouse and is managed completely by Hive.</span></span> <span data-ttu-id="663f7-159">Ellentétben a külső táblákhoz az alapul szolgáló adatokat egy belső tábla eldobása törli.</span><span class="sxs-lookup"><span data-stu-id="663f7-159">Unlike external tables, dropping an internal table deletes the underlying data as well.</span></span>

   * <span data-ttu-id="663f7-160">**TÁROLT AS ORC** -tárolja az adatokat optimalizált sor oszlopos (ORC) formátumban.</span><span class="sxs-lookup"><span data-stu-id="663f7-160">**STORED AS ORC** - Stores the data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="663f7-161">ORC formátuma egy magas optimalizált és hatékony Hive adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="663f7-161">ORC is a highly optimized and efficient format for storing Hive data.</span></span>

   * <span data-ttu-id="663f7-162">**ÍRJA FELÜL AZ INSERT... Válassza ki** -sorát kiválasztja a **log4jLogs** tartalmazó `[ERROR]`, majd beilleszti az adatokat a **errorLogs** tábla.</span><span class="sxs-lookup"><span data-stu-id="663f7-162">**INSERT OVERWRITE ... SELECT** - Selects rows from the **log4jLogs** table that contain `[ERROR]`, and then inserts the data into the **errorLogs** table.</span></span>

     <span data-ttu-id="663f7-163">Használja a **Execute** gombra a lekérdezés futtatására.</span><span class="sxs-lookup"><span data-stu-id="663f7-163">Use the **Execute** button to run this query.</span></span> <span data-ttu-id="663f7-164">A **eredmények** lap tartalmaz adatokat, ha a lekérdezés nulla sort adja vissza.</span><span class="sxs-lookup"><span data-stu-id="663f7-164">The **Results** tab does not contain any information when the query returns zero rows.</span></span> <span data-ttu-id="663f7-165">A állapota kell **sikeres** a lekérdezés befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="663f7-165">The status should show as **SUCCEEDED** once the query completes.</span></span>

### <a name="visual-explain"></a><span data-ttu-id="663f7-166">Visual ismertetik.</span><span class="sxs-lookup"><span data-stu-id="663f7-166">Visual explain</span></span>

<span data-ttu-id="663f7-167">A lekérdezésterv a képi megjelenítés megjelenítéséhez jelölje ki a **Visual ismertetik** alatt a munkalap fülre.</span><span class="sxs-lookup"><span data-stu-id="663f7-167">To display a visualization of the query plan, select the **Visual Explain** tab below the worksheet.</span></span>

<span data-ttu-id="663f7-168">A **Visual ismertetik** lehet, hogy a nézet a lekérdezés a folyamatot, az összetett lekérdezések hasznos információkat.</span><span class="sxs-lookup"><span data-stu-id="663f7-168">The **Visual Explain** view of the query can be helpful in understanding the flow of complex queries.</span></span> <span data-ttu-id="663f7-169">A szöveges megfelelőjét e nézet használatával tekintheti a **magyarázat** gombra a lekérdezés-szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="663f7-169">You can view a textual equivalent of this view by using the **Explain** button in the Query Editor.</span></span>

### <a name="tez-ui"></a><span data-ttu-id="663f7-170">Tez felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="663f7-170">Tez UI</span></span>

<span data-ttu-id="663f7-171">A lekérdezés a Tez felhasználói felület megjelenítéséhez jelölje ki a **Tez** alatt a munkalap fülre.</span><span class="sxs-lookup"><span data-stu-id="663f7-171">To display the Tez UI for the query, select the **Tez** tab below the worksheet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="663f7-172">Tez nem használt összes lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="663f7-172">Tez is not used to resolve all queries.</span></span> <span data-ttu-id="663f7-173">Több lekérdezést orvosolni tudja Tez használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="663f7-173">Many queries can be resolved without using Tez.</span></span> 

<span data-ttu-id="663f7-174">Ha a Tez a lekérdezés feloldásához használt, az irányított aciklikus diagramhoz (DAG) jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="663f7-174">If Tez was used to resolve the query, the Directed Acyclic Graph (DAG) is displayed.</span></span> <span data-ttu-id="663f7-175">Ha meg szeretné tekinteni a DAG lekérdezések már futott az elmúlt, vagy a Tez folyamat hibakeresése használja a [Tez nézet](hdinsight-debug-ambari-tez-view.md) helyette.</span><span class="sxs-lookup"><span data-stu-id="663f7-175">If you want to view the DAG for queries you've ran in the past, or debug the Tez process, use the [Tez View](hdinsight-debug-ambari-tez-view.md) instead.</span></span>

## <a name="view-job-history"></a><span data-ttu-id="663f7-176">Feladatelőzmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="663f7-176">View job history</span></span>

<span data-ttu-id="663f7-177">A __feladatok__ lapon Hive-lekérdezések előzményeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="663f7-177">The __Jobs__ tab displays a history of Hive queries.</span></span>

![A feladatelőzmények képe](./media/hdinsight-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a><span data-ttu-id="663f7-179">Adatbázistáblák</span><span class="sxs-lookup"><span data-stu-id="663f7-179">Database tables</span></span>

<span data-ttu-id="663f7-180">Használhatja a __táblák__ lapon Hive adatbázisban lévő táblák együttműködni.</span><span class="sxs-lookup"><span data-stu-id="663f7-180">You can use the __Tables__ tab to work with tables within a Hive database.</span></span>

![A táblák lap képe](./media/hdinsight-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a><span data-ttu-id="663f7-182">Lekérdezések</span><span class="sxs-lookup"><span data-stu-id="663f7-182">Saved queries</span></span>

<span data-ttu-id="663f7-183">A lekérdezés lap, a lekérdezések opcionálisan mentheti.</span><span class="sxs-lookup"><span data-stu-id="663f7-183">From the Query tab, you can optionally save queries.</span></span> <span data-ttu-id="663f7-184">Miután menti, felhasználhatja a lekérdezés a __mentett lekérdezések__ fülre.</span><span class="sxs-lookup"><span data-stu-id="663f7-184">Once saved, you can reuse the query from the __Saved Queries__ tab.</span></span>

![Lekérdezések lap képe](./media/hdinsight-hadoop-use-hive-ambari-view/saved-queries.png)

## <a name="user-defined-functions"></a><span data-ttu-id="663f7-186">Felhasználó által definiált függvények</span><span class="sxs-lookup"><span data-stu-id="663f7-186">User-defined functions</span></span>

<span data-ttu-id="663f7-187">Hive is kiterjeszthető felhasználói függvény (UDF) keresztül.</span><span class="sxs-lookup"><span data-stu-id="663f7-187">Hive can also be extended through user-defined functions (UDF).</span></span> <span data-ttu-id="663f7-188">Egy UDF funkció vagy logika, amely nem egyszerű modellezve megvalósítását a HiveQL teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="663f7-188">A UDF allows you to implement functionality or logic that isn't easily modeled in HiveQL.</span></span>

<span data-ttu-id="663f7-189">Az UDF lap felső részén a Hive View teszi deklarál, és mentse a felhasználó által megadott függvények készlete.</span><span class="sxs-lookup"><span data-stu-id="663f7-189">The UDF tab at the top of the Hive View allows you to declare and save a set of UDFs.</span></span> <span data-ttu-id="663f7-190">A felhasználó által megadott függvények használhatók a **Lekérdezésszerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="663f7-190">These UDFs can be used with the **Query Editor**.</span></span>

![Az UDF lap képe](./media/hdinsight-hadoop-use-hive-ambari-view/user-defined-functions.png)

<span data-ttu-id="663f7-192">A Hive nézetet egy UDF hozzáadása után egy **helyezze be a felhasználó által megadott függvények** gomb alján megjelenik a **Lekérdezésszerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="663f7-192">Once you have added a UDF to the Hive View, an **Insert udfs** button appears at the bottom of the **Query Editor**.</span></span> <span data-ttu-id="663f7-193">Látható értesítések valamelyikének kiválasztásakor Ez a bejegyzés a Hive nézetben megadott felhasználó által megadott függvények legördülő listája.</span><span class="sxs-lookup"><span data-stu-id="663f7-193">Selecting this entry displays a drop-down list of the UDFs defined in the Hive View.</span></span> <span data-ttu-id="663f7-194">Az UDF-ben ahhoz, hogy a lekérdezés egy UDF kiválasztása HiveQL utasítás hozzá.</span><span class="sxs-lookup"><span data-stu-id="663f7-194">Selecting a UDF adds HiveQL statements to your query to enable the UDF.</span></span>

<span data-ttu-id="663f7-195">Például, ha meghatározta a UDF-ben a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="663f7-195">For example, if you have defined a UDF with the following properties:</span></span>

* <span data-ttu-id="663f7-196">Az erőforrásnév: myudfs</span><span class="sxs-lookup"><span data-stu-id="663f7-196">Resource name: myudfs</span></span>

* <span data-ttu-id="663f7-197">Erőforrás elérési útja: /myudfs.jar</span><span class="sxs-lookup"><span data-stu-id="663f7-197">Resource path: /myudfs.jar</span></span>

* <span data-ttu-id="663f7-198">Az UDF-name: myawesomeudf</span><span class="sxs-lookup"><span data-stu-id="663f7-198">UDF name: myawesomeudf</span></span>

* <span data-ttu-id="663f7-199">Az UDF osztálynév: com.myudfs.Awesome</span><span class="sxs-lookup"><span data-stu-id="663f7-199">UDF class name: com.myudfs.Awesome</span></span>

<span data-ttu-id="663f7-200">Használja a **helyezze be a felhasználó által megadott függvények** gomb megjelenik egy bejegyzés nevű **myudfs**, egy másik legördülő minden UDF definiálva az adott erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="663f7-200">Using the **Insert udfs** button displays an entry named **myudfs**, with another drop-down for each UDF defined for that resource.</span></span> <span data-ttu-id="663f7-201">Ebben az esetben **myawesomeudf**.</span><span class="sxs-lookup"><span data-stu-id="663f7-201">In this case, **myawesomeudf**.</span></span> <span data-ttu-id="663f7-202">A lekérdezés elejére válassza ezt a bejegyzést ad a következő:</span><span class="sxs-lookup"><span data-stu-id="663f7-202">Selecting this entry adds the following to the beginning of the query:</span></span>

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

<span data-ttu-id="663f7-203">Az UDF használhatja a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="663f7-203">You can then use the UDF in your query.</span></span> <span data-ttu-id="663f7-204">Például: `SELECT myawesomeudf(name) FROM people;`.</span><span class="sxs-lookup"><span data-stu-id="663f7-204">For example, `SELECT myawesomeudf(name) FROM people;`.</span></span>

<span data-ttu-id="663f7-205">A felhasználó által megadott függvények használata a HDInsight Hive további információkért lásd a következő dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="663f7-205">For more information on using UDFs with Hive on HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="663f7-206">Python használata a Hive és a Pig a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="663f7-206">Using Python with Hive and Pig in HDInsight</span></span>](hdinsight-python.md)
* [<span data-ttu-id="663f7-207">HDInsight egy egyéni Hive UDF felvétele</span><span class="sxs-lookup"><span data-stu-id="663f7-207">How to add a custom Hive UDF to HDInsight</span></span>](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a><span data-ttu-id="663f7-208">Hive-beállítások</span><span class="sxs-lookup"><span data-stu-id="663f7-208">Hive settings</span></span>

<span data-ttu-id="663f7-209">Beállítások segítségével különböző Hive-beállítások módosításához.</span><span class="sxs-lookup"><span data-stu-id="663f7-209">Settings can be used to change various Hive settings.</span></span> <span data-ttu-id="663f7-210">Például megváltoztathatja a-végrehajtó motor a Hive Tez (alapértelmezett) a MapReduce a.</span><span class="sxs-lookup"><span data-stu-id="663f7-210">For example, changing the execution engine for Hive from Tez (the default) to MapReduce.</span></span>

## <span data-ttu-id="663f7-211"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="663f7-211"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="663f7-212">Általános információk a hdinsight Hive:</span><span class="sxs-lookup"><span data-stu-id="663f7-212">For general information on Hive in HDInsight:</span></span>

* [<span data-ttu-id="663f7-213">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="663f7-213">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="663f7-214">Az egyéb lehetőségeiről a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="663f7-214">For information on other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="663f7-215">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="663f7-215">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="663f7-216">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="663f7-216">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
