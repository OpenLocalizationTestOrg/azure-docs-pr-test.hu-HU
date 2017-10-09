---
title: a Hadoop - Azure HDInsight Sqoop aaaApache |} Microsoft Docs
description: "Megtudhatja, hogyan toouse Apache Sqoop tooimport és exportálás a HDInsight Hadoop és egy Azure SQL-adatbázis között."
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
ms.openlocfilehash: b256285659bbcf18ff05e220ccdf51c21eb8fbf7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-sqoop-tooimport-and-export-data-between-hadoop-on-hdinsight-and-sql-database"></a><span data-ttu-id="18ff6-104">Apache Sqoop tooimport használja, és a HDInsight Hadoop és SQL-adatbázis közötti adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="18ff6-104">Use Apache Sqoop tooimport and export data between Hadoop on HDInsight and SQL Database</span></span>

[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="18ff6-105">Ismerje meg, hogyan toouse Apache Sqoop tooimport között egy Hadoop exportálási fürtön és az Azure HDInsight és az Azure SQL Database vagy Microsoft SQL Server adatbázis.</span><span class="sxs-lookup"><span data-stu-id="18ff6-105">Learn how toouse Apache Sqoop tooimport and export between a Hadoop cluster in Azure HDInsight and Azure SQL Database or Microsoft SQL Server database.</span></span> <span data-ttu-id="18ff6-106">Ez a dokumentum használatát hello lépései hello `sqoop` közvetlenül hello headnode hello Hadoop-fürt parancsot.</span><span class="sxs-lookup"><span data-stu-id="18ff6-106">hello steps in this document use hello `sqoop` command directly from hello headnode of hello Hadoop cluster.</span></span> <span data-ttu-id="18ff6-107">SSH tooconnect toohello átjárócsomópont használ, és ez a dokumentum hello parancsok futtatását.</span><span class="sxs-lookup"><span data-stu-id="18ff6-107">You use SSH tooconnect toohello head node and run hello commands in this document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18ff6-108">hello ebben a dokumentumban csak a lépések Linux használó HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="18ff6-108">hello steps in this document only work with HDInsight clusters that use Linux.</span></span> <span data-ttu-id="18ff6-109">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="18ff6-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="18ff6-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="18ff6-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="install-freetds"></a><span data-ttu-id="18ff6-111">FreeTDS telepítése</span><span class="sxs-lookup"><span data-stu-id="18ff6-111">Install FreeTDS</span></span>

1. <span data-ttu-id="18ff6-112">SSH tooconnect toohello HDInsight-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="18ff6-112">Use SSH tooconnect toohello HDInsight cluster.</span></span> <span data-ttu-id="18ff6-113">Például a következő parancs hello csatlakozik toohello elsődleges headnode nevű fürt `mycluster`:</span><span class="sxs-lookup"><span data-stu-id="18ff6-113">For example, hello following command connects toohello primary headnode of a cluster named `mycluster`:</span></span>

    ```bash
    ssh CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="18ff6-114">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="18ff6-114">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="18ff6-115">A következő parancs tooinstall FreeTDS hello használata:</span><span class="sxs-lookup"><span data-stu-id="18ff6-115">Use hello following command tooinstall FreeTDS:</span></span>

    ```bash
    sudo apt --assume-yes install freetds-dev freetds-bin
    ```

    <span data-ttu-id="18ff6-116">FreeTDS több lépéseket tooconnect tooSQL adatbázis használatban van.</span><span class="sxs-lookup"><span data-stu-id="18ff6-116">FreeTDS is used in several steps tooconnect tooSQL Database.</span></span>

## <a name="create-hello-table-in-sql-database"></a><span data-ttu-id="18ff6-117">Az SQL-adatbázis hello tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="18ff6-117">Create hello table in SQL Database</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18ff6-118">Ha hello HDInsight-fürtöt használ, és SQL-adatbázis létrehozása [fürt és az SQL-adatbázis létrehozása](hdinsight-use-sqoop.md), hagyja ki a jelen szakaszban szereplő hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="18ff6-118">If you are using hello HDInsight cluster and SQL Database created in [Create cluster and SQL database](hdinsight-use-sqoop.md), skip hello steps in this section.</span></span> <span data-ttu-id="18ff6-119">hello adatbázis és tábla hoztak létre, hello részét szükséges lépések hello [fürt és az SQL-adatbázis létrehozása](hdinsight-use-sqoop.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="18ff6-119">hello database and table were created as part of hello steps in hello [Create cluster and SQL database](hdinsight-use-sqoop.md) document.</span></span>

1. <span data-ttu-id="18ff6-120">Hello SSH-munkamenetet használja a következő parancs tooconnect toohello SQL adatbázis-kiszolgáló hello.</span><span class="sxs-lookup"><span data-stu-id="18ff6-120">From hello SSH session, use hello following command tooconnect toohello SQL Database server.</span></span>

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    <span data-ttu-id="18ff6-121">Kimeneti hasonló toohello a következő szöveg jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="18ff6-121">You receive output similar toohello following text:</span></span>

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set toosqooptest
        1>

2. <span data-ttu-id="18ff6-122">A hello `1>` kéri, adja meg a következő lekérdezés hello:</span><span class="sxs-lookup"><span data-stu-id="18ff6-122">At hello `1>` prompt, enter hello following query:</span></span>

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

    <span data-ttu-id="18ff6-123">Ha hello `GO` utasításban is meg kell adni, hello előző utasítás kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="18ff6-123">When hello `GO` statement is entered, hello previous statements are evaluated.</span></span> <span data-ttu-id="18ff6-124">Első lépésként hello **mobiledata** tábla létrehozása, majd egy fürtözött index kerül tooit (SQL-adatbázis szükséges.)</span><span class="sxs-lookup"><span data-stu-id="18ff6-124">First, hello **mobiledata** table is created, then a clustered index is added tooit (required by SQL Database.)</span></span>

    <span data-ttu-id="18ff6-125">A következő lekérdezés tooverify, amelyek a tábla hello használata hello jött létre:</span><span class="sxs-lookup"><span data-stu-id="18ff6-125">Use hello following query tooverify that hello table has been created:</span></span>

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="18ff6-126">Kimeneti hasonló toohello a következő szöveg jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="18ff6-126">You see output similar toohello following text:</span></span>

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

3. <span data-ttu-id="18ff6-127">Adja meg `exit` : hello `1>` tooexit hello tsql segédprogram kérni.</span><span class="sxs-lookup"><span data-stu-id="18ff6-127">Enter `exit` at hello `1>` prompt tooexit hello tsql utility.</span></span>

## <a name="sqoop-export"></a><span data-ttu-id="18ff6-128">Sqoop exportálása</span><span class="sxs-lookup"><span data-stu-id="18ff6-128">Sqoop export</span></span>

1. <span data-ttu-id="18ff6-129">Hello SSH kapcsolat toohello fürtből a következő parancs tooverify, hogy a Sqoop látja-e az SQL-adatbázis hello használata:</span><span class="sxs-lookup"><span data-stu-id="18ff6-129">From hello SSH connection toohello cluster, use hello following command tooverify that Sqoop can see your SQL Database:</span></span>

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    <span data-ttu-id="18ff6-130">Amikor a rendszer kéri, adja meg hello jelszó hello SQL adatbázis-bejelentkezési adatokat.</span><span class="sxs-lookup"><span data-stu-id="18ff6-130">When prompted, enter hello password for hello SQL Database login.</span></span>

    <span data-ttu-id="18ff6-131">Ez a parancs visszaadja az adatbázisok, beleértve a hello listája **sqooptest** korábban létrehozott adatbázis.</span><span class="sxs-lookup"><span data-stu-id="18ff6-131">This command returns a list of databases, including hello **sqooptest** database that you created earlier.</span></span>

2. <span data-ttu-id="18ff6-132">tooexport adatait **hivesampletable** toohello **mobiledata** table, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="18ff6-132">tooexport data from **hivesampletable** toohello **mobiledata** table, use hello following command:</span></span>

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --export-dir 'wasb:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1
    ```

    <span data-ttu-id="18ff6-133">Ez a parancs utasítja a Sqoop tooconnect toohello **sqooptest** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="18ff6-133">This command instructs Sqoop tooconnect toohello **sqooptest** database.</span></span> <span data-ttu-id="18ff6-134">Sqoop majd exportálja az adatokat a **wasb: / / / hive/adatraktár/hivesampletable** toohello **mobiledata** tábla.</span><span class="sxs-lookup"><span data-stu-id="18ff6-134">Sqoop then exports data from **wasb:///hive/warehouse/hivesampletable** toohello **mobiledata** table.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="18ff6-135">Használjon `wasb:///` Ha hello alapértelmezett tároló a fürt számára egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="18ff6-135">Use `wasb:///` if hello default storage for your cluster is an Azure Storage account.</span></span> <span data-ttu-id="18ff6-136">Használjon `adl:///` egy Azure Data Lake Store esetén.</span><span class="sxs-lookup"><span data-stu-id="18ff6-136">Use `adl:///` if it is an Azure Data Lake Store.</span></span>

3. <span data-ttu-id="18ff6-137">Hello parancs befejezése után használja a következő parancs tooconnect toohello adatbázis TSQL használatával hello:</span><span class="sxs-lookup"><span data-stu-id="18ff6-137">After hello command completes, use hello following command tooconnect toohello database using TSQL:</span></span>

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P -p 1433 -D sqooptest
    ```

    <span data-ttu-id="18ff6-138">A csatlakozás után a következő utasítások tooverify, amely adatok hello használata hello lett-e az exportált toohello **mobiledata** tábla:</span><span class="sxs-lookup"><span data-stu-id="18ff6-138">Once connected, use hello following statements tooverify that hello data was exported toohello **mobiledata** table:</span></span>

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata
    GO
    ```

    <span data-ttu-id="18ff6-139">Meg kell jelennie hello tábla listáját.</span><span class="sxs-lookup"><span data-stu-id="18ff6-139">You should see a listing of data in hello table.</span></span> <span data-ttu-id="18ff6-140">Típus `exit` tooexit hello tsql segédprogram.</span><span class="sxs-lookup"><span data-stu-id="18ff6-140">Type `exit` tooexit hello tsql utility.</span></span>

## <a name="sqoop-import"></a><span data-ttu-id="18ff6-141">Sqoop importálása</span><span class="sxs-lookup"><span data-stu-id="18ff6-141">Sqoop import</span></span>

1. <span data-ttu-id="18ff6-142">Használjon hello következő parancsot a hello tooimport adatait **mobiledata** tábla az SQL-adatbázis, toohello **wasb: / / / oktatóanyagok/usesqoop/importeddata** HDInsight könyvtárába:</span><span class="sxs-lookup"><span data-stu-id="18ff6-142">Use hello following command tooimport data from hello **mobiledata** table in SQL Database, toohello **wasb:///tutorials/usesqoop/importeddata** directory on HDInsight:</span></span>

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    <span data-ttu-id="18ff6-143">hello adatok mezőinek hello tabulátor választják el, és hello sorok egy új sor karakter megszűnik.</span><span class="sxs-lookup"><span data-stu-id="18ff6-143">hello fields in hello data are separated by a tab character, and hello lines are terminated by a new-line character.</span></span>

2. <span data-ttu-id="18ff6-144">Hello importálás befejezése után használja a következő parancs toolist kimenő hello hello könyvtára hello:</span><span class="sxs-lookup"><span data-stu-id="18ff6-144">Once hello import has completed, use hello following command toolist out hello data in hello new directory:</span></span>

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a><span data-ttu-id="18ff6-145">SQL Server használata</span><span class="sxs-lookup"><span data-stu-id="18ff6-145">Using SQL Server</span></span>

<span data-ttu-id="18ff6-146">Is használhat a Sqoop tooimport és exportál adatokat az SQL Server, vagy az adatközpont, vagy az Azure-ban futó virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="18ff6-146">You can also use Sqoop tooimport and export data from SQL Server, either in your data center or on a Virtual Machine hosted in Azure.</span></span> <span data-ttu-id="18ff6-147">SQL Database és SQL Server használata között hello különbségek a következők:</span><span class="sxs-lookup"><span data-stu-id="18ff6-147">hello differences between using SQL Database and SQL Server are:</span></span>

* <span data-ttu-id="18ff6-148">HDInsight és az SQL Server kell lennie a hello azonos Azure virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="18ff6-148">Both HDInsight and SQL Server must be on hello same Azure Virtual Network.</span></span>

    <span data-ttu-id="18ff6-149">Egy vonatkozó példáért lásd: hello [csatlakozás HDInsight tooyour a helyszíni hálózat](./connect-on-premises-network.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="18ff6-149">For an example, see hello [Connect HDInsight tooyour on-premises network](./connect-on-premises-network.md) document.</span></span>

    <span data-ttu-id="18ff6-150">A HDInsight használata az Azure virtuális hálózat további információkért lásd: hello [kiterjesztése HDInsight az Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="18ff6-150">For more information on using HDInsight with an Azure Virtual Network, see hello [Extend HDInsight with Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span> <span data-ttu-id="18ff6-151">Azure-beli virtuális hálózatra vonatkozó további információkért lásd: hello [virtuális hálózat áttekintése](../virtual-network/virtual-networks-overview.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="18ff6-151">For more information on Azure Virtual Network, see hello [Virtual Network Overview](../virtual-network/virtual-networks-overview.md) document.</span></span>

* <span data-ttu-id="18ff6-152">SQL Server konfigurált tooallow SQL-hitelesítést kell lennie.</span><span class="sxs-lookup"><span data-stu-id="18ff6-152">SQL Server must be configured tooallow SQL authentication.</span></span> <span data-ttu-id="18ff6-153">További információkért lásd: hello [válassza ki a kívánt hitelesítési mód](https://msdn.microsoft.com/ms144284.aspx) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="18ff6-153">For more information, see hello [Choose an Authentication Mode](https://msdn.microsoft.com/ms144284.aspx) document.</span></span>

* <span data-ttu-id="18ff6-154">Előfordulhat, hogy tooconfigure SQL Server tooaccept távoli kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="18ff6-154">You may have tooconfigure SQL Server tooaccept remote connections.</span></span> <span data-ttu-id="18ff6-155">További információkért lásd: hello [hogyan tootroubleshoot csatlakozó toohello SQL Server adatbázis-motor](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="18ff6-155">For more information, see hello [How tootroubleshoot connecting toohello SQL Server database engine](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) document.</span></span>

* <span data-ttu-id="18ff6-156">Hozzon létre hello **sqooptest** egy segédprogram segítségével például SQL Server adatbázis **SQL Server Management Studio** vagy **tsql**.</span><span class="sxs-lookup"><span data-stu-id="18ff6-156">Create hello **sqooptest** database in SQL Server using a utility such as **SQL Server Management Studio** or **tsql**.</span></span> <span data-ttu-id="18ff6-157">hello hello Azure CLI használatára vonatkozó csak a lépések az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="18ff6-157">hello steps for using hello Azure CLI only work for Azure SQL Database.</span></span>

    <span data-ttu-id="18ff6-158">A következő Transact-SQL utasítás toocreate hello használata hello **mobiledata** tábla:</span><span class="sxs-lookup"><span data-stu-id="18ff6-158">Use hello following Transact-SQL statements toocreate hello **mobiledata** table:</span></span>

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

* <span data-ttu-id="18ff6-159">Ha toohello SQL Server a HDInsight-ból, előfordulhat, hogy az SQL Server hello toouse hello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="18ff6-159">When connecting toohello SQL Server from HDInsight, you may have toouse hello IP address of hello SQL Server.</span></span> <span data-ttu-id="18ff6-160">Példa:</span><span class="sxs-lookup"><span data-stu-id="18ff6-160">For example:</span></span>

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a><span data-ttu-id="18ff6-161">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="18ff6-161">Limitations</span></span>

* <span data-ttu-id="18ff6-162">Tömeges export - a Linux-alapú HDInsight, hello Sqoop használt tooexport adatok tooMicrosoft SQL Server vagy az Azure SQL Database jelenleg nem támogatja a tömeges beszúrások.</span><span class="sxs-lookup"><span data-stu-id="18ff6-162">Bulk export - With Linux-based HDInsight, hello Sqoop connector used tooexport data tooMicrosoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>

* <span data-ttu-id="18ff6-163">Kötegelés – a Linux-alapú hdinsight eszközzel, hello használatakor `-batch` Beszúrások végrehajtásakor kapcsoló, a Sqoop lehetővé teszi több Beszúrások hello beszúrási műveletek kötegelése helyett.</span><span class="sxs-lookup"><span data-stu-id="18ff6-163">Batching - With Linux-based HDInsight, When using hello `-batch` switch when performing inserts, Sqoop makes multiple inserts instead of batching hello insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18ff6-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="18ff6-164">Next steps</span></span>

<span data-ttu-id="18ff6-165">Most megtanulta, hogyan toouse Sqoop.</span><span class="sxs-lookup"><span data-stu-id="18ff6-165">Now you have learned how toouse Sqoop.</span></span> <span data-ttu-id="18ff6-166">toolearn több, lásd:</span><span class="sxs-lookup"><span data-stu-id="18ff6-166">toolearn more, see:</span></span>

* <span data-ttu-id="18ff6-167">[Oozie használata a HDInsight][hdinsight-use-oozie]: egy Oozie munkafolyamat használja Sqoop műveletét.</span><span class="sxs-lookup"><span data-stu-id="18ff6-167">[Use Oozie with HDInsight][hdinsight-use-oozie]: Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="18ff6-168">[HDInsight eszközzel repülési késleltetés adatok elemzése][hdinsight-analyze-flight-data]: tooanalyze felhőszolgáltató közötti átviteléhez használja Hive késleltetés az adatok, és majd a Sqoop tooexport tooan Azure SQL-adatbázis használata.</span><span class="sxs-lookup"><span data-stu-id="18ff6-168">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]: Use Hive tooanalyze flight delay data, and then use Sqoop tooexport data tooan Azure SQL database.</span></span>
* <span data-ttu-id="18ff6-169">[Töltse fel az adatok tooHDInsight][hdinsight-upload-data]: található adatok tooHDInsight vagy az Azure Blob storage feltöltése más módszerrel.</span><span class="sxs-lookup"><span data-stu-id="18ff6-169">[Upload data tooHDInsight][hdinsight-upload-data]: Find other methods for uploading data tooHDInsight/Azure Blob storage.</span></span>

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
