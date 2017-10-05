---
title: "Data Lake Store és a Sqoop használatával Azure SQL-adatbázis közötti adatok másolása |} Microsoft Docs"
description: "Másolja az adatokat az Azure SQL Database és a Data Lake Store között a Sqoop használatával"
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
ms.openlocfilehash: 53bf33f6027f1f365bd92251490d5c851fb83f8b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a><span data-ttu-id="2473f-103">Data Lake Store és a Sqoop használatával Azure SQL-adatbázis közötti adatok másolása</span><span class="sxs-lookup"><span data-stu-id="2473f-103">Copy data between Data Lake Store and Azure SQL database using Sqoop</span></span>
<span data-ttu-id="2473f-104">Útmutató Apache Sqoop használatával importálhat és exportálhat adatokat az Azure SQL Database és a Data Lake Store között.</span><span class="sxs-lookup"><span data-stu-id="2473f-104">Learn how to use Apache Sqoop to import and export data between Azure SQL Database and Data Lake Store.</span></span>

## <a name="what-is-sqoop"></a><span data-ttu-id="2473f-105">Mi az a Sqoop?</span><span class="sxs-lookup"><span data-stu-id="2473f-105">What is Sqoop?</span></span>
<span data-ttu-id="2473f-106">Big data-alkalmazások olyan természetes téve a strukturálatlan és félig strukturált adatok, például a naplókat, valamint a fájlok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="2473f-106">Big data applications are a natural choice for processing unstructured and semi-structured data, such as logs and files.</span></span> <span data-ttu-id="2473f-107">Azonban is lehet a relációs adatbázisok tárolt strukturált adatok feldolgozása érdekében.</span><span class="sxs-lookup"><span data-stu-id="2473f-107">However, there may also be a need to process structured data that is stored in relational databases.</span></span>

<span data-ttu-id="2473f-108">[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) eszköz a relációs adatbázisok és a big Data típusú adatok tára, például a Data Lake Store között továbbított adatok.</span><span class="sxs-lookup"><span data-stu-id="2473f-108">[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) is a tool designed to transfer data between  relational databases and a big data repository, such as Data Lake Store.</span></span> <span data-ttu-id="2473f-109">Adatokat importálhat egy relációs adatbázis-kezelő rendszer (RDBMS) például az Azure SQL Database Data Lake Store az használhatja.</span><span class="sxs-lookup"><span data-stu-id="2473f-109">You can use it to import data from a relational database management system (RDBMS) such as Azure SQL Database into Data Lake Store.</span></span> <span data-ttu-id="2473f-110">Majd átalakíthatja és elemezheti az adatokat, használja a big data-számítási feladatok és exportálhatja az adatokat egy RDBMS programba.</span><span class="sxs-lookup"><span data-stu-id="2473f-110">You can then transform and analyze the data using big data workloads and then export the data back into an RDBMS.</span></span> <span data-ttu-id="2473f-111">Ebben az oktatóanyagban az Azure SQL Database mint segítségével a relációs adatbázis az importálási/exportálási.</span><span class="sxs-lookup"><span data-stu-id="2473f-111">In this tutorial, you use an Azure SQL Database as your relational database to import/export from.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2473f-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2473f-112">Prerequisites</span></span>
<span data-ttu-id="2473f-113">A cikk elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="2473f-113">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="2473f-114">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="2473f-114">**An Azure subscription**.</span></span> <span data-ttu-id="2473f-115">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2473f-115">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="2473f-116">**Egy Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="2473f-116">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="2473f-117">Hogyan hozhat létre ilyet, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="2473f-117">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="2473f-118">**Az Azure HDInsight-fürt** a Data Lake Store-fiók eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="2473f-118">**Azure HDInsight cluster** with access to a Data Lake Store account.</span></span> <span data-ttu-id="2473f-119">Lásd: [HDInsight-fürtök létrehozása a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2473f-119">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="2473f-120">Ez a cikk feltételezi, hogy rendelkezik-e egy Data Lake Store-hozzáféréssel rendelkező HDInsight Linux-fürt.</span><span class="sxs-lookup"><span data-stu-id="2473f-120">This article assumes you have an HDInsight Linux cluster with Data Lake Store access.</span></span>
* <span data-ttu-id="2473f-121">**Az Azure SQL adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="2473f-121">**Azure SQL Database**.</span></span> <span data-ttu-id="2473f-122">Hogyan hozhat létre ilyet, lásd: [Azure SQL-adatbázis létrehozása](../sql-database/sql-database-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="2473f-122">For instructions on how to create one, see [Create an Azure SQL database](../sql-database/sql-database-get-started.md)</span></span>

## <a name="do-you-learn-fast-with-videos"></a><span data-ttu-id="2473f-123">Gyorsan tanul videók segítségével?</span><span class="sxs-lookup"><span data-stu-id="2473f-123">Do you learn fast with videos?</span></span>
<span data-ttu-id="2473f-124">[A videó](https://mix.office.com/watch/1butcdjxmu114) az adatok Azure Storage Blobs és a Data Lake Store használatának ból a DistCp közötti másolása.</span><span class="sxs-lookup"><span data-stu-id="2473f-124">[Watch this video](https://mix.office.com/watch/1butcdjxmu114) on how to copy data between Azure Storage Blobs and Data Lake Store using DistCp.</span></span>

## <a name="create-sample-tables-in-the-azure-sql-database"></a><span data-ttu-id="2473f-125">A minta-táblázatok létrehozása az Azure SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="2473f-125">Create sample tables in the Azure SQL Database</span></span>
1. <span data-ttu-id="2473f-126">Először hozzon létre két minta tábla az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="2473f-126">To start with, create two sample tables in the Azure SQL Database.</span></span> <span data-ttu-id="2473f-127">Használjon [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) vagy a Visual Studio használatával csatlakozzon az Azure SQL Database, és futtassa az alábbi lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="2473f-127">Use [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) or Visual Studio to connect to the Azure SQL Database and then run the following queries.</span></span>

    <span data-ttu-id="2473f-128">**Table1 létrehozása**</span><span class="sxs-lookup"><span data-stu-id="2473f-128">**Create Table1**</span></span>

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

    <span data-ttu-id="2473f-129">**Table2 létrehozása**</span><span class="sxs-lookup"><span data-stu-id="2473f-129">**Create Table2**</span></span>

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
2. <span data-ttu-id="2473f-130">A **Table1**, vegye fel néhány adatot.</span><span class="sxs-lookup"><span data-stu-id="2473f-130">In **Table1**, add some sample data.</span></span> <span data-ttu-id="2473f-131">Hagyja **Table2** üres.</span><span class="sxs-lookup"><span data-stu-id="2473f-131">Leave **Table2** empty.</span></span> <span data-ttu-id="2473f-132">Az adatok fogunk importálni **Table1** a Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="2473f-132">We will import data from **Table1** into Data Lake Store.</span></span> <span data-ttu-id="2473f-133">Majd, hogy exportálja az adatokat a Data Lake Store az **Table2**.</span><span class="sxs-lookup"><span data-stu-id="2473f-133">Then, we will export data from Data Lake Store into **Table2**.</span></span> <span data-ttu-id="2473f-134">Futtassa a következő kódrészletet.</span><span class="sxs-lookup"><span data-stu-id="2473f-134">Run the following snippet.</span></span>

        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');


## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a><span data-ttu-id="2473f-135">HDInsight-fürtök hozzáféréssel rendelkező Sqoop használatával Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="2473f-135">Use Sqoop from an HDInsight cluster with access to Data Lake Store</span></span>
<span data-ttu-id="2473f-136">HDInsight-fürtök már elérhető a Sqoop csomagokat.</span><span class="sxs-lookup"><span data-stu-id="2473f-136">An HDInsight cluster already has the Sqoop packages available.</span></span> <span data-ttu-id="2473f-137">Ha konfigurálta a HDInsight-fürt használata a Data Lake Store további tárterületként, segítségével Sqoop (nélkül végrehajtott bármilyen konfigurációs módosításokat) importálási/exportálási között (a példában az Azure SQL Database) a relációs adatok és a Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="2473f-137">If you have configured the HDInsight cluster to use Data Lake Store as an additional storage, you can use Sqoop (without any configuration changes) to import/export data between a relational database (in this example, Azure SQL Database) and a Data Lake Store account.</span></span>

1. <span data-ttu-id="2473f-138">Ebben az oktatóanyagban feltételezzük, hogy létrehozott egy Linux-fürt, csatlakozzon a fürthöz SSH kell használnia.</span><span class="sxs-lookup"><span data-stu-id="2473f-138">For this tutorial, we assume you created a Linux cluster so you should use SSH to connect to the cluster.</span></span> <span data-ttu-id="2473f-139">Lásd: [csatlakozás egy Linux-alapú HDInsight-fürt](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="2473f-139">See [Connect to a Linux-based HDInsight cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>
2. <span data-ttu-id="2473f-140">Győződjön meg arról, hogy a Data Lake Store-fiók hozzáférhet a fürtből.</span><span class="sxs-lookup"><span data-stu-id="2473f-140">Verify whether you can access the Data Lake Store account from the cluster.</span></span> <span data-ttu-id="2473f-141">A következő parancsot az SSH-parancssorból:</span><span class="sxs-lookup"><span data-stu-id="2473f-141">Run the following command from the SSH prompt:</span></span>

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    <span data-ttu-id="2473f-142">Ez biztosítania kell a fájlok és mappák a Data Lake Store-fiókban listáját.</span><span class="sxs-lookup"><span data-stu-id="2473f-142">This should provide a list of files/folders in the Data Lake Store account.</span></span>

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a><span data-ttu-id="2473f-143">Adatokat importálhat az Azure SQL Database Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="2473f-143">Import data from Azure SQL Database into Data Lake Store</span></span>
1. <span data-ttu-id="2473f-144">Navigáljon ahhoz a könyvtárhoz, ahol a Sqoop csomagok érhetők el.</span><span class="sxs-lookup"><span data-stu-id="2473f-144">Navigate to the directory where Sqoop packages are available.</span></span> <span data-ttu-id="2473f-145">Általában ez lesz `/usr/hdp/<version>/sqoop/bin`.</span><span class="sxs-lookup"><span data-stu-id="2473f-145">Typically, this will be at `/usr/hdp/<version>/sqoop/bin`.</span></span>
2. <span data-ttu-id="2473f-146">Az adatimportálás **Table1** be a Data Lake Store-fiókba.</span><span class="sxs-lookup"><span data-stu-id="2473f-146">Import the data from **Table1** into the Data Lake Store account.</span></span> <span data-ttu-id="2473f-147">A következő szintaxissal:</span><span class="sxs-lookup"><span data-stu-id="2473f-147">Use the following syntax:</span></span>

        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    <span data-ttu-id="2473f-148">Vegye figyelembe, hogy **sql-adatbázis-kiszolgálónév** helyőrző az Azure SQL-adatbázist futtató kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="2473f-148">Note that **sql-database-server-name** placeholder represents the name of the server where the Azure SQL database is running.</span></span> <span data-ttu-id="2473f-149">**SQL-adatbázis-neve** helyőrző az aktuális adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="2473f-149">**sql-database-name** placeholder represents the actual database name.</span></span>

    <span data-ttu-id="2473f-150">Például:</span><span class="sxs-lookup"><span data-stu-id="2473f-150">For example,</span></span>


        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

1. <span data-ttu-id="2473f-151">Győződjön meg arról, hogy az adatok átvitele volt-e a Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="2473f-151">Verify that the data has been transferred to the Data Lake Store account.</span></span> <span data-ttu-id="2473f-152">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="2473f-152">Run the following command:</span></span>

        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    <span data-ttu-id="2473f-153">A következő kimenetet kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="2473f-153">You should see the following output.</span></span>


        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    <span data-ttu-id="2473f-154">Minden egyes **rész-m -*** fájl megfelel-e a forrástábla sorában **Table1**.</span><span class="sxs-lookup"><span data-stu-id="2473f-154">Each **part-m-*** file corresponds to a row in the source table, **Table1**.</span></span> <span data-ttu-id="2473f-155">Megtekintheti az - m - elem tartalmának * fájlok ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="2473f-155">You can view the contents of the part-m-* files to verify.</span></span>


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a><span data-ttu-id="2473f-156">Adatok exportálása az Azure SQL Database Data Lake Store-ból</span><span class="sxs-lookup"><span data-stu-id="2473f-156">Export data from Data Lake Store into Azure SQL Database</span></span>
1. <span data-ttu-id="2473f-157">Exportálja az adatokat a Data Lake Store-fiókot az üres táblázat **Table2**, az Azure SQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="2473f-157">Export the data from Data Lake Store account to the empty table, **Table2**, in the Azure SQL Database.</span></span> <span data-ttu-id="2473f-158">A következő szintaxissal.</span><span class="sxs-lookup"><span data-stu-id="2473f-158">Use the following syntax.</span></span>

        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    <span data-ttu-id="2473f-159">Például:</span><span class="sxs-lookup"><span data-stu-id="2473f-159">For example,</span></span>


        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. <span data-ttu-id="2473f-160">Győződjön meg arról, hogy az SQL-adatbázis táblája feltöltött adatokat.</span><span class="sxs-lookup"><span data-stu-id="2473f-160">Verify that the data was uploaded to the SQL Database table.</span></span> <span data-ttu-id="2473f-161">Használjon [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) vagy a Visual Studio használatával csatlakozzon az Azure SQL Database, és futtassa a következő lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="2473f-161">Use [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) or Visual Studio to connect to the Azure SQL Database and then run the following query.</span></span>

        SELECT * FROM TABLE2

    <span data-ttu-id="2473f-162">Ez a következő kimenetet kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="2473f-162">This should have the following output.</span></span>

         ID  FName   LName
        ------------------
        1    Neal    Kell
        2    Lila    Fulton
        3    Erna    Myers
        4    Annette    Simpson

## <a name="performance-considerations-while-using-sqoop"></a><span data-ttu-id="2473f-163">Sqoop használatakor a teljesítménnyel kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="2473f-163">Performance considerations while using Sqoop</span></span>

<span data-ttu-id="2473f-164">Másolja az adatokat a Data Lake Store a Sqoop feladat teljesítményhangolás, lásd: [Sqoop teljesítmény dokumentum](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).</span><span class="sxs-lookup"><span data-stu-id="2473f-164">For performance tuning your Sqoop job to copy data to Data Lake Store, see [Sqoop performance document](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).</span></span>

## <a name="see-also"></a><span data-ttu-id="2473f-165">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="2473f-165">See also</span></span>
* [<span data-ttu-id="2473f-166">Adatok másolása az Azure Storage Blobs Data Lake Store-bA</span><span class="sxs-lookup"><span data-stu-id="2473f-166">Copy data from Azure Storage Blobs to Data Lake Store</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
* [<span data-ttu-id="2473f-167">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="2473f-167">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="2473f-168">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="2473f-168">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="2473f-169">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="2473f-169">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
