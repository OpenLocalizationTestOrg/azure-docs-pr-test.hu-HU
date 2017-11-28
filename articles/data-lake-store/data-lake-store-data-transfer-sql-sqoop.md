---
title: "Data Lake Store és a Sqoop használatával Azure SQL-adatbázis közötti aaaCopy adatok |} Microsoft Docs"
description: "Sqoop toocopy adatokat az Azure SQL Database és a Data Lake Store között"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3f914b2a-83cc-4950-b3f7-69c921851683
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: f58483455f0ebe9544673a1d5c5884f2721c800c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a><span data-ttu-id="4aca5-103">Data Lake Store és a Sqoop használatával Azure SQL-adatbázis közötti adatok másolása</span><span class="sxs-lookup"><span data-stu-id="4aca5-103">Copy data between Data Lake Store and Azure SQL database using Sqoop</span></span>
<span data-ttu-id="4aca5-104">Megtudhatja, hogyan toouse Apache Sqoop tooimport és exportálási Azure SQL Database és a Data Lake Store között adatokat.</span><span class="sxs-lookup"><span data-stu-id="4aca5-104">Learn how toouse Apache Sqoop tooimport and export data between Azure SQL Database and Data Lake Store.</span></span>

## <a name="what-is-sqoop"></a><span data-ttu-id="4aca5-105">Mi az a Sqoop?</span><span class="sxs-lookup"><span data-stu-id="4aca5-105">What is Sqoop?</span></span>
<span data-ttu-id="4aca5-106">Big data-alkalmazások olyan természetes téve a strukturálatlan és félig strukturált adatok, például a naplókat, valamint a fájlok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="4aca5-106">Big data applications are a natural choice for processing unstructured and semi-structured data, such as logs and files.</span></span> <span data-ttu-id="4aca5-107">Azonban előfordulhat, hogy is van a szükséges strukturált tooprocess relációs adatbázisokban tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="4aca5-107">However, there may also be a need tooprocess structured data that is stored in relational databases.</span></span>

<span data-ttu-id="4aca5-108">[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) egy eszköz tootransfer adatokat relációs adatbázisok és a big Data típusú adatok tára, például a Data Lake Store között van.</span><span class="sxs-lookup"><span data-stu-id="4aca5-108">[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) is a tool designed tootransfer data between  relational databases and a big data repository, such as Data Lake Store.</span></span> <span data-ttu-id="4aca5-109">Is használhatja az tooimport adatait egy relációs adatbázis-kezelő rendszer (RDBMS) például az Azure SQL Database Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4aca5-109">You can use it tooimport data from a relational database management system (RDBMS) such as Azure SQL Database into Data Lake Store.</span></span> <span data-ttu-id="4aca5-110">Majd átalakíthatja és adatelemzés hello segítségével a big data-számítási feladatok és majd hello adatok exportálása vissza egy RDBMS.</span><span class="sxs-lookup"><span data-stu-id="4aca5-110">You can then transform and analyze hello data using big data workloads and then export hello data back into an RDBMS.</span></span> <span data-ttu-id="4aca5-111">Ebben az oktatóanyagban az Azure SQL Database, a relációs adatbázis tooimport/exportálása a használja.</span><span class="sxs-lookup"><span data-stu-id="4aca5-111">In this tutorial, you use an Azure SQL Database as your relational database tooimport/export from.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4aca5-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4aca5-112">Prerequisites</span></span>
<span data-ttu-id="4aca5-113">Ez a cikk elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="4aca5-113">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="4aca5-114">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="4aca5-114">**An Azure subscription**.</span></span> <span data-ttu-id="4aca5-115">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4aca5-115">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="4aca5-116">**Egy Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="4aca5-116">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="4aca5-117">Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="4aca5-117">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="4aca5-118">**Az Azure HDInsight-fürt** a Data Lake Store-fiók hozzáférési tooa.</span><span class="sxs-lookup"><span data-stu-id="4aca5-118">**Azure HDInsight cluster** with access tooa Data Lake Store account.</span></span> <span data-ttu-id="4aca5-119">Lásd: [HDInsight-fürtök létrehozása a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4aca5-119">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="4aca5-120">Ez a cikk feltételezi, hogy rendelkezik-e egy Data Lake Store-hozzáféréssel rendelkező HDInsight Linux-fürt.</span><span class="sxs-lookup"><span data-stu-id="4aca5-120">This article assumes you have an HDInsight Linux cluster with Data Lake Store access.</span></span>
* <span data-ttu-id="4aca5-121">**Azure SQL Database**</span><span class="sxs-lookup"><span data-stu-id="4aca5-121">**Azure SQL Database**.</span></span> <span data-ttu-id="4aca5-122">Útmutatást toocreate egy, lásd: [Azure SQL-adatbázis létrehozása](../sql-database/sql-database-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="4aca5-122">For instructions on how toocreate one, see [Create an Azure SQL database](../sql-database/sql-database-get-started.md)</span></span>

## <a name="do-you-learn-fast-with-videos"></a><span data-ttu-id="4aca5-123">Gyorsan tanul videók segítségével?</span><span class="sxs-lookup"><span data-stu-id="4aca5-123">Do you learn fast with videos?</span></span>
<span data-ttu-id="4aca5-124">[A videó](https://mix.office.com/watch/1butcdjxmu114) hogyan toocopy adatok Azure Storage Blobs és a Data Lake Store használatának ból a DistCp között.</span><span class="sxs-lookup"><span data-stu-id="4aca5-124">[Watch this video](https://mix.office.com/watch/1butcdjxmu114) on how toocopy data between Azure Storage Blobs and Data Lake Store using DistCp.</span></span>

## <a name="create-sample-tables-in-hello-azure-sql-database"></a><span data-ttu-id="4aca5-125">A minta táblázatok hello Azure SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="4aca5-125">Create sample tables in hello Azure SQL Database</span></span>
1. <span data-ttu-id="4aca5-126">toostart rendelkező, két minta táblázatok hello Azure SQL-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4aca5-126">toostart with, create two sample tables in hello Azure SQL Database.</span></span> <span data-ttu-id="4aca5-127">Használjon [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) vagy a Visual Studio tooconnect toohello Azure SQL Database lekérdezések futtatása hello kövesse.</span><span class="sxs-lookup"><span data-stu-id="4aca5-127">Use [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) or Visual Studio tooconnect toohello Azure SQL Database and then run hello following queries.</span></span>

    <span data-ttu-id="4aca5-128">**Table1 létrehozása**</span><span class="sxs-lookup"><span data-stu-id="4aca5-128">**Create Table1**</span></span>

        CREATE TABLE [dbo].[Table1](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO

    <span data-ttu-id="4aca5-129">**Table2 létrehozása**</span><span class="sxs-lookup"><span data-stu-id="4aca5-129">**Create Table2**</span></span>

        CREATE TABLE [dbo].[Table2](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO
2. <span data-ttu-id="4aca5-130">A **Table1**, vegye fel néhány adatot.</span><span class="sxs-lookup"><span data-stu-id="4aca5-130">In **Table1**, add some sample data.</span></span> <span data-ttu-id="4aca5-131">Hagyja **Table2** üres.</span><span class="sxs-lookup"><span data-stu-id="4aca5-131">Leave **Table2** empty.</span></span> <span data-ttu-id="4aca5-132">Az adatok fogunk importálni **Table1** a Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4aca5-132">We will import data from **Table1** into Data Lake Store.</span></span> <span data-ttu-id="4aca5-133">Majd, hogy exportálja az adatokat a Data Lake Store az **Table2**.</span><span class="sxs-lookup"><span data-stu-id="4aca5-133">Then, we will export data from Data Lake Store into **Table2**.</span></span> <span data-ttu-id="4aca5-134">Futtassa a következő kódrészletet hello.</span><span class="sxs-lookup"><span data-stu-id="4aca5-134">Run hello following snippet.</span></span>

        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');


## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-toodata-lake-store"></a><span data-ttu-id="4aca5-135">A HDInsight fürtök használata Sqoop tooData Lake áruház eléréséhez</span><span class="sxs-lookup"><span data-stu-id="4aca5-135">Use Sqoop from an HDInsight cluster with access tooData Lake Store</span></span>
<span data-ttu-id="4aca5-136">HDInsight-fürtök már hello Sqoop csomagok érhető el.</span><span class="sxs-lookup"><span data-stu-id="4aca5-136">An HDInsight cluster already has hello Sqoop packages available.</span></span> <span data-ttu-id="4aca5-137">Ha hello HDInsight fürt toouse Data Lake Store további tárterületként állította be, használhatja (nélkül végrehajtott bármilyen konfigurációs módosításokat) Sqoop tooimport/exportálja az adatokat egy relációs adatbázisban (a példában az Azure SQL Database) között és egy Data Lake Store fiók.</span><span class="sxs-lookup"><span data-stu-id="4aca5-137">If you have configured hello HDInsight cluster toouse Data Lake Store as an additional storage, you can use Sqoop (without any configuration changes) tooimport/export data between a relational database (in this example, Azure SQL Database) and a Data Lake Store account.</span></span>

1. <span data-ttu-id="4aca5-138">Ebben az oktatóanyagban feltételezzük, hogy létrehozott egy Linux-fürt, hogy az SSH tooconnect toohello fürt kell használni.</span><span class="sxs-lookup"><span data-stu-id="4aca5-138">For this tutorial, we assume you created a Linux cluster so you should use SSH tooconnect toohello cluster.</span></span> <span data-ttu-id="4aca5-139">Lásd: [Connect tooa Linux-alapú HDInsight-fürt](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="4aca5-139">See [Connect tooa Linux-based HDInsight cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>
2. <span data-ttu-id="4aca5-140">Győződjön meg arról, hogy van-e hozzáférési hello Data Lake Store-fiók hello fürtből.</span><span class="sxs-lookup"><span data-stu-id="4aca5-140">Verify whether you can access hello Data Lake Store account from hello cluster.</span></span> <span data-ttu-id="4aca5-141">Futtassa a parancsot követő hello SSH parancssorból hello:</span><span class="sxs-lookup"><span data-stu-id="4aca5-141">Run hello following command from hello SSH prompt:</span></span>

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    <span data-ttu-id="4aca5-142">Ez biztosítania kell a fájlok és mappák a Data Lake Store-fiók hello listáját.</span><span class="sxs-lookup"><span data-stu-id="4aca5-142">This should provide a list of files/folders in hello Data Lake Store account.</span></span>

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a><span data-ttu-id="4aca5-143">Adatokat importálhat az Azure SQL Database Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="4aca5-143">Import data from Azure SQL Database into Data Lake Store</span></span>
1. <span data-ttu-id="4aca5-144">Keresse meg a toohello könyvtár, ahol a Sqoop csomagok érhetők el.</span><span class="sxs-lookup"><span data-stu-id="4aca5-144">Navigate toohello directory where Sqoop packages are available.</span></span> <span data-ttu-id="4aca5-145">Általában ez lesz `/usr/hdp/<version>/sqoop/bin`.</span><span class="sxs-lookup"><span data-stu-id="4aca5-145">Typically, this will be at `/usr/hdp/<version>/sqoop/bin`.</span></span>
2. <span data-ttu-id="4aca5-146">Hello adatokat importálni **Table1** hello Data Lake Store-fiók be.</span><span class="sxs-lookup"><span data-stu-id="4aca5-146">Import hello data from **Table1** into hello Data Lake Store account.</span></span> <span data-ttu-id="4aca5-147">A következő szintaxist hello használata:</span><span class="sxs-lookup"><span data-stu-id="4aca5-147">Use hello following syntax:</span></span>

        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    <span data-ttu-id="4aca5-148">Vegye figyelembe, hogy **sql-adatbázis-kiszolgálónév** helyőrző hello hello hello Azure SQL-adatbázist futtató kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="4aca5-148">Note that **sql-database-server-name** placeholder represents hello name of hello server where hello Azure SQL database is running.</span></span> <span data-ttu-id="4aca5-149">**SQL-adatbázis-neve** helyőrző hello tényleges adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="4aca5-149">**sql-database-name** placeholder represents hello actual database name.</span></span>

    <span data-ttu-id="4aca5-150">Például:</span><span class="sxs-lookup"><span data-stu-id="4aca5-150">For example,</span></span>


        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

1. <span data-ttu-id="4aca5-151">Győződjön meg arról, hogy korábban már hello átvitt toohello Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="4aca5-151">Verify that hello data has been transferred toohello Data Lake Store account.</span></span> <span data-ttu-id="4aca5-152">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4aca5-152">Run hello following command:</span></span>

        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    <span data-ttu-id="4aca5-153">A következő kimeneti hello kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="4aca5-153">You should see hello following output.</span></span>


        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    <span data-ttu-id="4aca5-154">Minden egyes **rész-m -*** fájl megfelel tooa sor hello forrástáblában, **Table1**.</span><span class="sxs-lookup"><span data-stu-id="4aca5-154">Each **part-m-*** file corresponds tooa row in hello source table, **Table1**.</span></span> <span data-ttu-id="4aca5-155">Megtekintheti a hello rész - m - hello tartalmát * tooverify fájlok.</span><span class="sxs-lookup"><span data-stu-id="4aca5-155">You can view hello contents of hello part-m-* files tooverify.</span></span>


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a><span data-ttu-id="4aca5-156">Adatok exportálása az Azure SQL Database Data Lake Store-ból</span><span class="sxs-lookup"><span data-stu-id="4aca5-156">Export data from Data Lake Store into Azure SQL Database</span></span>
1. <span data-ttu-id="4aca5-157">A Data Lake Store fiók toohello üres táblából, hello adatok exportálása **Table2**, az Azure SQL Database hello.</span><span class="sxs-lookup"><span data-stu-id="4aca5-157">Export hello data from Data Lake Store account toohello empty table, **Table2**, in hello Azure SQL Database.</span></span> <span data-ttu-id="4aca5-158">A következő szintaxist hello használata.</span><span class="sxs-lookup"><span data-stu-id="4aca5-158">Use hello following syntax.</span></span>

        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    <span data-ttu-id="4aca5-159">Például:</span><span class="sxs-lookup"><span data-stu-id="4aca5-159">For example,</span></span>


        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. <span data-ttu-id="4aca5-160">Győződjön meg arról, hogy hello adatok lett feltöltve toohello SQL-adatbázis táblája.</span><span class="sxs-lookup"><span data-stu-id="4aca5-160">Verify that hello data was uploaded toohello SQL Database table.</span></span> <span data-ttu-id="4aca5-161">Használjon [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) vagy a Visual Studio tooconnect toohello Azure SQL Database és a lekérdezés futtatása hello kövesse.</span><span class="sxs-lookup"><span data-stu-id="4aca5-161">Use [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) or Visual Studio tooconnect toohello Azure SQL Database and then run hello following query.</span></span>

        SELECT * FROM TABLE2

    <span data-ttu-id="4aca5-162">Ez a következő kimeneti hello kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="4aca5-162">This should have hello following output.</span></span>

         ID  FName   LName
        ------------------
        1    Neal    Kell
        2    Lila    Fulton
        3    Erna    Myers
        4    Annette    Simpson

## <a name="performance-considerations-while-using-sqoop"></a><span data-ttu-id="4aca5-163">Sqoop használatakor a teljesítménnyel kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="4aca5-163">Performance considerations while using Sqoop</span></span>

<span data-ttu-id="4aca5-164">Teljesítményhangolás a Sqoop toocopy adattár tooData Lake feladat, a következő témakörben: [Sqoop teljesítmény dokumentum](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).</span><span class="sxs-lookup"><span data-stu-id="4aca5-164">For performance tuning your Sqoop job toocopy data tooData Lake Store, see [Sqoop performance document](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).</span></span>

## <a name="see-also"></a><span data-ttu-id="4aca5-165">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4aca5-165">See also</span></span>
* [<span data-ttu-id="4aca5-166">Adatok másolása az Azure Storage Blobs tooData Lake Store-ból</span><span class="sxs-lookup"><span data-stu-id="4aca5-166">Copy data from Azure Storage Blobs tooData Lake Store</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
* [<span data-ttu-id="4aca5-167">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="4aca5-167">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="4aca5-168">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="4aca5-168">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="4aca5-169">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="4aca5-169">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
