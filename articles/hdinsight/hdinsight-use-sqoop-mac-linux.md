---
title: Hadoop - az Azure HDInsight az Apache Sqoop |} Microsoft Docs
description: "Útmutató Apache Sqoop használatával importálása és exportálása a HDInsight Hadoop és egy Azure SQL-adatbázis között."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
author: Blackmist
tags: azure-portal
keywords: hadoop sqoop, sqoop
ms.assetid: 303649a5-4be5-4933-bf1d-4b232083c354
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: larryfr
ms.openlocfilehash: 35dcbb91e6af1480685c9fd5b829c54277c1c605
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-hadoop-on-hdinsight-and-sql-database"></a><span data-ttu-id="338c0-104">Apache Sqoop használatával a HDInsight Hadoop és SQL-adatbázis közötti adatok importálása és exportálása</span><span class="sxs-lookup"><span data-stu-id="338c0-104">Use Apache Sqoop to import and export data between Hadoop on HDInsight and SQL Database</span></span>

[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="338c0-105">Útmutató Apache Sqoop használatával importálhat és exportálhat az Azure HDInsight Hadoop-fürthöz, és az Azure SQL Database vagy Microsoft SQL Server adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="338c0-105">Learn how to use Apache Sqoop to import and export between a Hadoop cluster in Azure HDInsight and Azure SQL Database or Microsoft SQL Server database.</span></span> <span data-ttu-id="338c0-106">A lépéseket, ez a dokumentum használatát a `sqoop` közvetlenül a Hadoop-fürt headnode parancsot.</span><span class="sxs-lookup"><span data-stu-id="338c0-106">The steps in this document use the `sqoop` command directly from the headnode of the Hadoop cluster.</span></span> <span data-ttu-id="338c0-107">SSH segítségével a átjárócsomópontjához csatlakozik, és futtassa a parancsokat ebben a dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="338c0-107">You use SSH to connect to the head node and run the commands in this document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="338c0-108">A jelen dokumentumban leírt lépések csak a HDInsight-fürtök Linux használó dolgozhat.</span><span class="sxs-lookup"><span data-stu-id="338c0-108">The steps in this document only work with HDInsight clusters that use Linux.</span></span> <span data-ttu-id="338c0-109">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="338c0-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="338c0-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="338c0-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="install-freetds"></a><span data-ttu-id="338c0-111">FreeTDS telepítése</span><span class="sxs-lookup"><span data-stu-id="338c0-111">Install FreeTDS</span></span>

1. <span data-ttu-id="338c0-112">Az SSH használata a HDInsight-fürthöz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="338c0-112">Use SSH to connect to the HDInsight cluster.</span></span> <span data-ttu-id="338c0-113">Például a következő parancs csatlakozik-e a fürt nevű elsődleges headnode `mycluster`:</span><span class="sxs-lookup"><span data-stu-id="338c0-113">For example, the following command connects to the primary headnode of a cluster named `mycluster`:</span></span>

    ```bash
    ssh CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="338c0-114">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="338c0-114">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="338c0-115">A következő paranccsal telepítse FreeTDS:</span><span class="sxs-lookup"><span data-stu-id="338c0-115">Use the following command to install FreeTDS:</span></span>

    ```bash
    sudo apt --assume-yes install freetds-dev freetds-bin
    ```

    <span data-ttu-id="338c0-116">FreeTDS számos lépést szerepel SQL adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="338c0-116">FreeTDS is used in several steps to connect to SQL Database.</span></span>

## <a name="create-the-table-in-sql-database"></a><span data-ttu-id="338c0-117">A tábla az SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="338c0-117">Create the table in SQL Database</span></span>

> [!IMPORTANT]
> <span data-ttu-id="338c0-118">Ha a HDInsight-fürtöt használ, és SQL-adatbázis létrehozása [fürt és az SQL-adatbázis létrehozása](hdinsight-use-sqoop.md), hagyja ki a jelen szakaszban szereplő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="338c0-118">If you are using the HDInsight cluster and SQL Database created in [Create cluster and SQL database](hdinsight-use-sqoop.md), skip the steps in this section.</span></span> <span data-ttu-id="338c0-119">Az adatbázis és tábla lépéseit részeként jöttek létre a [fürt és az SQL-adatbázis létrehozása](hdinsight-use-sqoop.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="338c0-119">The database and table were created as part of the steps in the [Create cluster and SQL database](hdinsight-use-sqoop.md) document.</span></span>

1. <span data-ttu-id="338c0-120">Az SSH-munkamenetet használja a következő parancsot az SQL Database-kiszolgálóhoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="338c0-120">From the SSH session, use the following command to connect to the SQL Database server.</span></span>

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    <span data-ttu-id="338c0-121">A kimenet az alábbihoz hasonló jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="338c0-121">You receive output similar to the following text:</span></span>

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

2. <span data-ttu-id="338c0-122">: A `1>` kéri, adja meg a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="338c0-122">At the `1>` prompt, enter the following query:</span></span>

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
    GO
    ```

    <span data-ttu-id="338c0-123">Ha a `GO` utasításban is meg kell adni, az előző utasítások kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="338c0-123">When the `GO` statement is entered, the previous statements are evaluated.</span></span> <span data-ttu-id="338c0-124">Első, a **mobiledata** tábla létrehozása, majd egy fürtözött index hozzáadni (SQL-adatbázis szükséges.)</span><span class="sxs-lookup"><span data-stu-id="338c0-124">First, the **mobiledata** table is created, then a clustered index is added to it (required by SQL Database.)</span></span>

    <span data-ttu-id="338c0-125">A következő lekérdezés segítségével győződjön meg arról, hogy a tábla jött létre:</span><span class="sxs-lookup"><span data-stu-id="338c0-125">Use the following query to verify that the table has been created:</span></span>

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="338c0-126">A kimenet az alábbihoz hasonló jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="338c0-126">You see output similar to the following text:</span></span>

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

3. <span data-ttu-id="338c0-127">Adja meg `exit` , a `1>` Rákérdezés a tsql segédprogram kilép.</span><span class="sxs-lookup"><span data-stu-id="338c0-127">Enter `exit` at the `1>` prompt to exit the tsql utility.</span></span>

## <a name="sqoop-export"></a><span data-ttu-id="338c0-128">Sqoop exportálása</span><span class="sxs-lookup"><span data-stu-id="338c0-128">Sqoop export</span></span>

1. <span data-ttu-id="338c0-129">Az SSH-kapcsolat a fürthöz ellenőrizheti, hogy a Sqoop látja-e az SQL-adatbázis alkalmazás a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="338c0-129">From the SSH connection to the cluster, use the following command to verify that Sqoop can see your SQL Database:</span></span>

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    <span data-ttu-id="338c0-130">Amikor a rendszer kéri, adja meg a jelszót az SQL adatbázis-bejelentkezési adatokat.</span><span class="sxs-lookup"><span data-stu-id="338c0-130">When prompted, enter the password for the SQL Database login.</span></span>

    <span data-ttu-id="338c0-131">Ez a parancs visszaadja adatbázislistáinak, beleértve a **sqooptest** korábban létrehozott adatbázis.</span><span class="sxs-lookup"><span data-stu-id="338c0-131">This command returns a list of databases, including the **sqooptest** database that you created earlier.</span></span>

2. <span data-ttu-id="338c0-132">Az exportálandó adatokat **hivesampletable** számára a **mobiledata** table, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="338c0-132">To export data from **hivesampletable** to the **mobiledata** table, use the following command:</span></span>

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --export-dir 'wasb:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1
    ```

    <span data-ttu-id="338c0-133">Ez a parancs utasítja a Sqoop való kapcsolódáshoz a **sqooptest** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="338c0-133">This command instructs Sqoop to connect to the **sqooptest** database.</span></span> <span data-ttu-id="338c0-134">Sqoop majd exportálja az adatokat a **wasb: / / / hive/adatraktár/hivesampletable** számára a **mobiledata** tábla.</span><span class="sxs-lookup"><span data-stu-id="338c0-134">Sqoop then exports data from **wasb:///hive/warehouse/hivesampletable** to the **mobiledata** table.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="338c0-135">Használjon `wasb:///` Ha az alapértelmezett tároló a fürt számára egy Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="338c0-135">Use `wasb:///` if the default storage for your cluster is an Azure Storage account.</span></span> <span data-ttu-id="338c0-136">Használjon `adl:///` egy Azure Data Lake Store esetén.</span><span class="sxs-lookup"><span data-stu-id="338c0-136">Use `adl:///` if it is an Azure Data Lake Store.</span></span>

3. <span data-ttu-id="338c0-137">A parancs befejezése után a következő paranccsal a TSQL használatával adatbázishoz való kapcsolódáshoz:</span><span class="sxs-lookup"><span data-stu-id="338c0-137">After the command completes, use the following command to connect to the database using TSQL:</span></span>

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P -p 1433 -D sqooptest
    ```

    <span data-ttu-id="338c0-138">A csatlakozás után használja az alábbi utasításokat győződjön meg arról, hogy a korábban exportált adatok a **mobiledata** tábla:</span><span class="sxs-lookup"><span data-stu-id="338c0-138">Once connected, use the following statements to verify that the data was exported to the **mobiledata** table:</span></span>

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata
    GO
    ```

    <span data-ttu-id="338c0-139">Meg kell jelennie a tábla adatainak listáját.</span><span class="sxs-lookup"><span data-stu-id="338c0-139">You should see a listing of data in the table.</span></span> <span data-ttu-id="338c0-140">Típus `exit` való kilépéshez a tsql segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="338c0-140">Type `exit` to exit the tsql utility.</span></span>

## <a name="sqoop-import"></a><span data-ttu-id="338c0-141">Sqoop importálása</span><span class="sxs-lookup"><span data-stu-id="338c0-141">Sqoop import</span></span>

1. <span data-ttu-id="338c0-142">Az alábbi parancs segítségével adatokat importálni a **mobiledata** a tábla az SQL-adatbázis, a **wasb: / / / oktatóanyagok/usesqoop/importeddata** HDInsight könyvtárába:</span><span class="sxs-lookup"><span data-stu-id="338c0-142">Use the following command to import data from the **mobiledata** table in SQL Database, to the **wasb:///tutorials/usesqoop/importeddata** directory on HDInsight:</span></span>

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    <span data-ttu-id="338c0-143">Az adatok mezőinek tabulátor választják el, és a sorok egy új sor karakter megszűnik.</span><span class="sxs-lookup"><span data-stu-id="338c0-143">The fields in the data are separated by a tab character, and the lines are terminated by a new-line character.</span></span>

2. <span data-ttu-id="338c0-144">Az importálás befejezése után a következő paranccsal kimenő a listára új könyvtárban:</span><span class="sxs-lookup"><span data-stu-id="338c0-144">Once the import has completed, use the following command to list out the data in the new directory:</span></span>

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a><span data-ttu-id="338c0-145">SQL Server használata</span><span class="sxs-lookup"><span data-stu-id="338c0-145">Using SQL Server</span></span>

<span data-ttu-id="338c0-146">Sqoop használatával adatok importálása és exportálása az SQL Server, vagy az adatközpont, vagy az Azure-ban futó virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="338c0-146">You can also use Sqoop to import and export data from SQL Server, either in your data center or on a Virtual Machine hosted in Azure.</span></span> <span data-ttu-id="338c0-147">Az SQL Database és SQL Server közötti különbségek a következők:</span><span class="sxs-lookup"><span data-stu-id="338c0-147">The differences between using SQL Database and SQL Server are:</span></span>

* <span data-ttu-id="338c0-148">HDInsight és az SQL Server azonos Azure virtuális hálózaton kell lennie.</span><span class="sxs-lookup"><span data-stu-id="338c0-148">Both HDInsight and SQL Server must be on the same Azure Virtual Network.</span></span>

    <span data-ttu-id="338c0-149">Egy vonatkozó példáért lásd: a [a helyszíni hálózathoz való csatlakozás HDInsight](./connect-on-premises-network.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="338c0-149">For an example, see the [Connect HDInsight to your on-premises network](./connect-on-premises-network.md) document.</span></span>

    <span data-ttu-id="338c0-150">A HDInsight használata az Azure virtuális hálózat további információkért lásd: a [kiterjesztése HDInsight az Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="338c0-150">For more information on using HDInsight with an Azure Virtual Network, see the [Extend HDInsight with Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span> <span data-ttu-id="338c0-151">Azure-beli virtuális hálózatra vonatkozó további információkért lásd: a [virtuális hálózat áttekintése](../virtual-network/virtual-networks-overview.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="338c0-151">For more information on Azure Virtual Network, see the [Virtual Network Overview](../virtual-network/virtual-networks-overview.md) document.</span></span>

* <span data-ttu-id="338c0-152">SQL Server SQL-hitelesítés engedélyezéséhez meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="338c0-152">SQL Server must be configured to allow SQL authentication.</span></span> <span data-ttu-id="338c0-153">További információkért lásd: a [válassza ki a kívánt hitelesítési mód](https://msdn.microsoft.com/ms144284.aspx) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="338c0-153">For more information, see the [Choose an Authentication Mode](https://msdn.microsoft.com/ms144284.aspx) document.</span></span>

* <span data-ttu-id="338c0-154">Előfordulhat, hogy az SQL Server távoli kapcsolatokat fogadjon konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="338c0-154">You may have to configure SQL Server to accept remote connections.</span></span> <span data-ttu-id="338c0-155">További információkért lásd: a [hibaelhárítása a Kapcsolódás az SQL Server adatbázismotor](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="338c0-155">For more information, see the [How to troubleshoot connecting to the SQL Server database engine](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) document.</span></span>

* <span data-ttu-id="338c0-156">Hozzon létre a **sqooptest** egy segédprogram segítségével például SQL Server adatbázis **SQL Server Management Studio** vagy **tsql**.</span><span class="sxs-lookup"><span data-stu-id="338c0-156">Create the **sqooptest** database in SQL Server using a utility such as **SQL Server Management Studio** or **tsql**.</span></span> <span data-ttu-id="338c0-157">A lépéseket az Azure parancssori felület használatával csak az Azure SQL Database működik.</span><span class="sxs-lookup"><span data-stu-id="338c0-157">The steps for using the Azure CLI only work for Azure SQL Database.</span></span>

    <span data-ttu-id="338c0-158">A következő Transact-SQL-utasítások létrehozásához használja a **mobiledata** tábla:</span><span class="sxs-lookup"><span data-stu-id="338c0-158">Use the following Transact-SQL statements to create the **mobiledata** table:</span></span>

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    ```

* <span data-ttu-id="338c0-159">Ha az SQL Server a HDInsight-ból, előfordulhat, hogy az SQL Server az IP-cím használatára.</span><span class="sxs-lookup"><span data-stu-id="338c0-159">When connecting to the SQL Server from HDInsight, you may have to use the IP address of the SQL Server.</span></span> <span data-ttu-id="338c0-160">Példa:</span><span class="sxs-lookup"><span data-stu-id="338c0-160">For example:</span></span>

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a><span data-ttu-id="338c0-161">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="338c0-161">Limitations</span></span>

* <span data-ttu-id="338c0-162">Tömeges export - a Linux-alapú HDInsight, a Sqoop összekötő használt Microsoft SQL Server vagy az Azure SQL Database adatainak exportálása jelenleg nem támogatja a tömeges beszúrások.</span><span class="sxs-lookup"><span data-stu-id="338c0-162">Bulk export - With Linux-based HDInsight, the Sqoop connector used to export data to Microsoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>

* <span data-ttu-id="338c0-163">Kötegelés - és a Linux-alapú HDInsight együttes használata esetén a `-batch` beszúrása végrehajtásakor kapcsoló, a Sqoop lehetővé teszi több beszúrás helyett a beszúrási műveletek kötegelése.</span><span class="sxs-lookup"><span data-stu-id="338c0-163">Batching - With Linux-based HDInsight, When using the `-batch` switch when performing inserts, Sqoop makes multiple inserts instead of batching the insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="338c0-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="338c0-164">Next steps</span></span>

<span data-ttu-id="338c0-165">Most megtanulhatta, hogyan használható a Sqoop.</span><span class="sxs-lookup"><span data-stu-id="338c0-165">Now you have learned how to use Sqoop.</span></span> <span data-ttu-id="338c0-166">További tudnivalókért lásd:</span><span class="sxs-lookup"><span data-stu-id="338c0-166">To learn more, see:</span></span>

* <span data-ttu-id="338c0-167">[Oozie használata a HDInsight][hdinsight-use-oozie]: egy Oozie munkafolyamat használja Sqoop műveletét.</span><span class="sxs-lookup"><span data-stu-id="338c0-167">[Use Oozie with HDInsight][hdinsight-use-oozie]: Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="338c0-168">[HDInsight eszközzel repülési késleltetés adatok elemzése][hdinsight-analyze-flight-data]: használja struktúra elemzése repülési késleltetés az adatok, és a Sqoop segítségével exportál adatokat az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="338c0-168">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]: Use Hive to analyze flight delay data, and then use Sqoop to export data to an Azure SQL database.</span></span>
* <span data-ttu-id="338c0-169">[Adatok feltöltése a HDInsight][hdinsight-upload-data]: található adatok feltöltése a HDInsight vagy az Azure Blob storage más módszerrel.</span><span class="sxs-lookup"><span data-stu-id="338c0-169">[Upload data to HDInsight][hdinsight-upload-data]: Find other methods for uploading data to HDInsight/Azure Blob storage.</span></span>

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
