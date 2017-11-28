---
title: ".NET és a HDInsight - Azure Sqoop feladatok futtatása |} Microsoft Docs"
description: "Megtudhatja, hogyan használhatja a HDInsight .NET SDK futtatása Sqoop importálása és exportálása egy Hadoop-fürt és az Azure SQL-adatbázis között."
keywords: sqoop feladat
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 87bacd13-7775-4b71-91da-161cb6224a96
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: c95641fc6d20e2911e007d1974b9e2c2398b3133
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a><span data-ttu-id="eec0d-104">A hdinsight Hadoop .NET SDK használatával Sqoop feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="eec0d-104">Run Sqoop jobs using .NET SDK for Hadoop in HDInsight</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="eec0d-105">Megtudhatja, hogyan használhatja a HDInsight .NET SDK Sqoop feladatok futtatása a Hdinsightban történő importálására és exportálására HDInsight-fürt és Azure SQL database vagy az SQL Server-adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="eec0d-105">Learn how to use HDInsight .NET SDK to run Sqoop jobs in HDInsight to import and export between HDInsight cluster and Azure SQL database or SQL Server database.</span></span>

> [!NOTE]
> <span data-ttu-id="eec0d-106">A cikkben leírt lépéseket is használható, vagy egy Windows-alapú vagy Linux-alapú HDInsight-fürthöz; azonban ezek csak a lépések Windows ügyfélről.</span><span class="sxs-lookup"><span data-stu-id="eec0d-106">The steps in this article can be used with either a Windows-based or Linux-based HDInsight cluster; however, these steps only work from a Windows client.</span></span> <span data-ttu-id="eec0d-107">Ez a cikk lapválasztót az egyéb módszerek kiválasztásához használható.</span><span class="sxs-lookup"><span data-stu-id="eec0d-107">Use the tab selector on the top of this article to choose other methods.</span></span>
> 
> 

### <a name="prerequisites"></a><span data-ttu-id="eec0d-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="eec0d-108">Prerequisites</span></span>
<span data-ttu-id="eec0d-109">Az oktatóanyag elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="eec0d-109">Before you begin this tutorial, you must have the following items:</span></span>

* <span data-ttu-id="eec0d-110">**Hdinsight Hadoop-fürthöz**.</span><span class="sxs-lookup"><span data-stu-id="eec0d-110">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="eec0d-111">Lásd: [fürt és az SQL-adatbázis létrehozása](hdinsight-use-sqoop.md#create-cluster-and-sql-database).</span><span class="sxs-lookup"><span data-stu-id="eec0d-111">See [Create cluster and SQL database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).</span></span>

## <a name="use-sqoop-on-hdinsight-clusters-using-net-sdk"></a><span data-ttu-id="eec0d-112">Sqoop használata a HDInsight-fürtökön .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="eec0d-112">Use Sqoop on HDInsight clusters using .NET SDK</span></span>
<span data-ttu-id="eec0d-113">A HDInsight .NET SDK biztosít a .NET ügyféloldali kódtáraknál, így azokat könnyebben működéséhez a .NET-HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="eec0d-113">The HDInsight .NET SDK provides .NET client libraries, which makes it easier to work with HDInsight clusters from .NET.</span></span> <span data-ttu-id="eec0d-114">Ebben a szakaszban hozzon létre egy C#-konzolalkalmazást a hivesampletable exportálhat az SQL-adatbázis táblája ebben az oktatóanyagban korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="eec0d-114">In this section, you create a C# console application to export the hivesampletable to the SQL Database table you created earlier in this tutorial.</span></span>

## <a name="submit-a-sqoop-job"></a><span data-ttu-id="eec0d-115">Sqoop feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="eec0d-115">Submit a Sqoop job</span></span>

1. <span data-ttu-id="eec0d-116">Hozzon létre egy C# konzolalkalmazást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="eec0d-116">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="eec0d-117">A Visual Studio Csomagkezelő konzolról a következő parancsot Nuget a csomag importálása.</span><span class="sxs-lookup"><span data-stu-id="eec0d-117">From the Visual Studio Package Manager Console, run the following Nuget command to import the package.</span></span>
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. <span data-ttu-id="eec0d-118">A következő kódot használja a Program.cs fájlban:</span><span class="sxs-lookup"><span data-stu-id="eec0d-118">Use the following code in the Program.cs file:</span></span>
   
        using System.Collections.Generic;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
   
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitSqoopJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
   
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
   
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
   
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
   
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
4. <span data-ttu-id="eec0d-119">Nyomja le az **F5** futtatni a programot.</span><span class="sxs-lookup"><span data-stu-id="eec0d-119">Press **F5** to run the program.</span></span> 

## <a name="limitations"></a><span data-ttu-id="eec0d-120">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="eec0d-120">Limitations</span></span>
* <span data-ttu-id="eec0d-121">Tömeges export - a Linux-alapú HDInsight, a Sqoop összekötő használt Microsoft SQL Server vagy az Azure SQL Database adatainak exportálása jelenleg nem támogatja a tömeges beszúrások.</span><span class="sxs-lookup"><span data-stu-id="eec0d-121">Bulk export - With Linux-based HDInsight, the Sqoop connector used to export data to Microsoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="eec0d-122">Kötegelés - és a Linux-alapú HDInsight együttes használata esetén a `-batch` beszúrása végrehajtásakor kapcsoló, a Sqoop több beszúrás helyett a beszúrási műveletek kötegelése hajt végre.</span><span class="sxs-lookup"><span data-stu-id="eec0d-122">Batching - With Linux-based HDInsight, When using the `-batch` switch when performing inserts, Sqoop performs multiple inserts instead of batching the insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eec0d-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eec0d-123">Next steps</span></span>
<span data-ttu-id="eec0d-124">Most megtanulhatta, hogyan használható a Sqoop.</span><span class="sxs-lookup"><span data-stu-id="eec0d-124">Now you have learned how to use Sqoop.</span></span> <span data-ttu-id="eec0d-125">További tudnivalókért lásd:</span><span class="sxs-lookup"><span data-stu-id="eec0d-125">To learn more, see:</span></span>

* <span data-ttu-id="eec0d-126">[Oozie használata a HDInsight](hdinsight-use-oozie.md): egy Oozie munkafolyamat használja Sqoop műveletét.</span><span class="sxs-lookup"><span data-stu-id="eec0d-126">[Use Oozie with HDInsight](hdinsight-use-oozie.md): Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="eec0d-127">[HDInsight eszközzel repülési késleltetés adatok elemzése](hdinsight-analyze-flight-delay-data.md): használja struktúra elemzése repülési késleltetés az adatok, és a Sqoop segítségével exportál adatokat az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="eec0d-127">[Analyze flight delay data using HDInsight](hdinsight-analyze-flight-delay-data.md): Use Hive to analyze flight delay data, and then use Sqoop to export data to an Azure SQL database.</span></span>
* <span data-ttu-id="eec0d-128">[Adatok feltöltése a HDInsight](hdinsight-upload-data.md): található adatok feltöltése a HDInsight vagy az Azure Blob storage más módszerrel.</span><span class="sxs-lookup"><span data-stu-id="eec0d-128">[Upload data to HDInsight](hdinsight-upload-data.md): Find other methods for uploading data to HDInsight/Azure Blob storage.</span></span>

