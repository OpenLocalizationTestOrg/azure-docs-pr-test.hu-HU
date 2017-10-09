---
title: ".NET és a HDInsight - Azure aaaRun Sqoop feladatok |} Microsoft Docs"
description: "Ismerje meg, hogy a toouse HDInsight .NET SDK toorun Sqoop hogyan importálhat és exportálhat egy Hadoop-fürt és az Azure SQL-adatbázis között."
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
ms.openlocfilehash: afa0a78ba5e5d89c04ba7be4b58dd24aea4f39ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a><span data-ttu-id="a7fa1-104">A hdinsight Hadoop .NET SDK használatával Sqoop feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="a7fa1-104">Run Sqoop jobs using .NET SDK for Hadoop in HDInsight</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="a7fa1-105">Megtudhatja, hogyan toouse HDInsight .NET SDK toorun Sqoop HDInsight tooimport feladat, és exportálja a HDInsight-fürt és Azure SQL database vagy az SQL Server-adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-105">Learn how toouse HDInsight .NET SDK toorun Sqoop jobs in HDInsight tooimport and export between HDInsight cluster and Azure SQL database or SQL Server database.</span></span>

> [!NOTE]
> <span data-ttu-id="a7fa1-106">a cikkben ismertetett hello használható vagy egy Windows-alapú vagy Linux-alapú HDInsight-fürthöz; azonban ezek csak a lépések Windows ügyfélről.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-106">hello steps in this article can be used with either a Windows-based or Linux-based HDInsight cluster; however, these steps only work from a Windows client.</span></span> <span data-ttu-id="a7fa1-107">Hello lapon választó, ez a cikk toochoose hello felül más módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-107">Use hello tab selector on hello top of this article toochoose other methods.</span></span>
> 
> 

### <a name="prerequisites"></a><span data-ttu-id="a7fa1-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a7fa1-108">Prerequisites</span></span>
<span data-ttu-id="a7fa1-109">Ez az oktatóanyag elkezdéséhez a következő elemek hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="a7fa1-109">Before you begin this tutorial, you must have hello following items:</span></span>

* <span data-ttu-id="a7fa1-110">**Hdinsight Hadoop-fürthöz**.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-110">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="a7fa1-111">Lásd: [fürt és az SQL-adatbázis létrehozása](hdinsight-use-sqoop.md#create-cluster-and-sql-database).</span><span class="sxs-lookup"><span data-stu-id="a7fa1-111">See [Create cluster and SQL database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).</span></span>

## <a name="use-sqoop-on-hdinsight-clusters-using-net-sdk"></a><span data-ttu-id="a7fa1-112">Sqoop használata a HDInsight-fürtökön .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="a7fa1-112">Use Sqoop on HDInsight clusters using .NET SDK</span></span>
<span data-ttu-id="a7fa1-113">hello HDInsight .NET SDK biztosít a .NET ügyféloldali kódtáraknál, így azokat könnyebben toowork a .NET-HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-113">hello HDInsight .NET SDK provides .NET client libraries, which makes it easier toowork with HDInsight clusters from .NET.</span></span> <span data-ttu-id="a7fa1-114">Ebben a szakaszban hozzon létre egy C# console application tooexport hello hivesampletable toohello SQL-adatbázis táblája ebben az oktatóanyagban korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-114">In this section, you create a C# console application tooexport hello hivesampletable toohello SQL Database table you created earlier in this tutorial.</span></span>

## <a name="submit-a-sqoop-job"></a><span data-ttu-id="a7fa1-115">Sqoop feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="a7fa1-115">Submit a Sqoop job</span></span>

1. <span data-ttu-id="a7fa1-116">Hozzon létre egy C# konzolalkalmazást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-116">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="a7fa1-117">Visual Studio Csomagkezelő konzol hello futtassa a következő Nuget parancs tooimport hello csomag hello.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-117">From hello Visual Studio Package Manager Console, run hello following Nuget command tooimport hello package.</span></span>
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. <span data-ttu-id="a7fa1-118">A következő kódot a Program.cs fájlban hello hello használata:</span><span class="sxs-lookup"><span data-stu-id="a7fa1-118">Use hello following code in hello Program.cs file:</span></span>
   
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
                    System.Console.WriteLine("hello application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitSqoopJob();
   
                    System.Console.WriteLine("Press ENTER toocontinue ...");
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
   
                    System.Console.WriteLine("Submitting hello Sqoop job toohello cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that hello response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating hello response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
4. <span data-ttu-id="a7fa1-119">Nyomja le az **F5** toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-119">Press **F5** toorun hello program.</span></span> 

## <a name="limitations"></a><span data-ttu-id="a7fa1-120">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="a7fa1-120">Limitations</span></span>
* <span data-ttu-id="a7fa1-121">Tömeges export - a Linux-alapú HDInsight, hello Sqoop használt tooexport adatok tooMicrosoft SQL Server vagy az Azure SQL Database jelenleg nem támogatja a tömeges beszúrások.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-121">Bulk export - With Linux-based HDInsight, hello Sqoop connector used tooexport data tooMicrosoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="a7fa1-122">Kötegelés – a Linux-alapú hdinsight eszközzel, hello használatakor `-batch` Beszúrások végrehajtásakor kapcsoló, a Sqoop hajt végre több Beszúrások hello beszúrási műveletek kötegelése helyett.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-122">Batching - With Linux-based HDInsight, When using hello `-batch` switch when performing inserts, Sqoop performs multiple inserts instead of batching hello insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7fa1-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a7fa1-123">Next steps</span></span>
<span data-ttu-id="a7fa1-124">Most megtanulta, hogyan toouse Sqoop.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-124">Now you have learned how toouse Sqoop.</span></span> <span data-ttu-id="a7fa1-125">toolearn több, lásd:</span><span class="sxs-lookup"><span data-stu-id="a7fa1-125">toolearn more, see:</span></span>

* <span data-ttu-id="a7fa1-126">[Oozie használata a HDInsight](hdinsight-use-oozie.md): egy Oozie munkafolyamat használja Sqoop műveletét.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-126">[Use Oozie with HDInsight](hdinsight-use-oozie.md): Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="a7fa1-127">[HDInsight eszközzel repülési késleltetés adatok elemzése](hdinsight-analyze-flight-delay-data.md): tooanalyze felhőszolgáltató közötti átviteléhez használja Hive késleltetés az adatok, és majd a Sqoop tooexport tooan Azure SQL-adatbázis használata.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-127">[Analyze flight delay data using HDInsight](hdinsight-analyze-flight-delay-data.md): Use Hive tooanalyze flight delay data, and then use Sqoop tooexport data tooan Azure SQL database.</span></span>
* <span data-ttu-id="a7fa1-128">[Töltse fel az adatok tooHDInsight](hdinsight-upload-data.md): található adatok tooHDInsight vagy az Azure Blob storage feltöltése más módszerrel.</span><span class="sxs-lookup"><span data-stu-id="a7fa1-128">[Upload data tooHDInsight](hdinsight-upload-data.md): Find other methods for uploading data tooHDInsight/Azure Blob storage.</span></span>

