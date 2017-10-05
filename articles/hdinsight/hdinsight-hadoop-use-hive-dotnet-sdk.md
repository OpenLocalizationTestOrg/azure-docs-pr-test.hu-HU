---
title: "A HDInsight .NET SDK - Azure használatával Hive-lekérdezések futtatása |} Microsoft Docs"
description: "Útmutató a Hadoop-feladatok Azure HDInsight Hadoop HDInsight .NET SDK használatával való elküldéséhez."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 4e291890-f8b4-426c-b5e8-d4fd512ff042
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jgao
ms.openlocfilehash: 7b1a5f7ea3b2bda438727dc75a85557ea7930280
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="run-hive-queries-using-hdinsight-net-sdk"></a><span data-ttu-id="3b64f-103">A HDInsight .NET SDK használatával Hive-lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="3b64f-103">Run Hive queries using HDInsight .NET SDK</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="3b64f-104">Megtudhatja, hogyan elküldeni a Hive-lekérdezéseket a HDInsight .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="3b64f-104">Learn how to submit Hive queries using HDInsight .NET SDK.</span></span> <span data-ttu-id="3b64f-105">A C# programban elküldeni a Hive táblák listázása a Hive-lekérdezések írása, és az eredmények megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3b64f-105">You write a C# program to submit a Hive query for listing Hive tables, and display the results.</span></span>

> [!NOTE]
> <span data-ttu-id="3b64f-106">A cikkben leírt lépéseket egy Windows ügyfél kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="3b64f-106">The steps in this article must be performed from a Windows client.</span></span> <span data-ttu-id="3b64f-107">Kapcsolatos információkért használata a Linux, OS X vagy UNIX rendszerű ügyfélnél Hive használata a lap választó, a cikk tetején látható.</span><span class="sxs-lookup"><span data-stu-id="3b64f-107">For information on using a Linux, OS X, or Unix client to work with Hive, use the tab selector shown on the top of the article.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="3b64f-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3b64f-108">Prerequisites</span></span>
<span data-ttu-id="3b64f-109">Ez a cikk megkezdése előtt rendelkeznie kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="3b64f-109">Before you begin this article, you must have the following items:</span></span>

* <span data-ttu-id="3b64f-110">**Hdinsight Hadoop-fürthöz**.</span><span class="sxs-lookup"><span data-stu-id="3b64f-110">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="3b64f-111">Lásd: [hdinsight Linux-alapú Hadoop használatának megkezdésében](./hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3b64f-111">See [Get started using Linux-based Hadoop in HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
* <span data-ttu-id="3b64f-112">**A Visual Studio 2013 vagy 2015/2017**.</span><span class="sxs-lookup"><span data-stu-id="3b64f-112">**Visual Studio 2013/2015/2017**.</span></span>

## <a name="submit-hive-queries-using-hdinsight-net-sdk"></a><span data-ttu-id="3b64f-113">Küldje el a Hive-lekérdezéseket a HDInsight .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="3b64f-113">Submit Hive queries using HDInsight .NET SDK</span></span>
<span data-ttu-id="3b64f-114">A HDInsight .NET SDK biztosít a .NET ügyféloldali kódtáraknál, így azokat könnyebben működéséhez a .NET-HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="3b64f-114">The HDInsight .NET SDK provides .NET client libraries, which makes it easier to work with HDInsight clusters from .NET.</span></span> 

<span data-ttu-id="3b64f-115">**Feladatok küldéséhez**</span><span class="sxs-lookup"><span data-stu-id="3b64f-115">**To Submit jobs**</span></span>

1. <span data-ttu-id="3b64f-116">Hozzon létre egy C# konzolalkalmazást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="3b64f-116">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="3b64f-117">A Nuget-Csomagkezelő konzolról futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="3b64f-117">From the Nuget Package Manager Console, run the following command:</span></span>
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. <span data-ttu-id="3b64f-118">A következő kód használható:</span><span class="sxs-lookup"><span data-stu-id="3b64f-118">Use the following code:</span></span>

    ```csharp
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
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
   
                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitHiveJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "tez" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);
   
                    System.Console.WriteLine("Waiting for the job completion ...");
   
                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }
   
                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure
   
                    System.Console.WriteLine("Job output is: ");
   
                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }
    ```
4. <span data-ttu-id="3b64f-119">Az alkalmazás futtatásához nyomja le az **F5** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="3b64f-119">Press **F5** to run the application.</span></span>

<span data-ttu-id="3b64f-120">Kell, hogy az alkalmazás kimenete hasonló:</span><span class="sxs-lookup"><span data-stu-id="3b64f-120">The output of the application shall be similar to:</span></span>

![HDInsight Hadoop Hive feladat kimenetére](./media/hdinsight-hadoop-use-hive-dotnet-sdk/hdinsight-hadoop-use-hive-net-sdk-output.png)

## <a name="next-steps"></a><span data-ttu-id="3b64f-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3b64f-122">Next steps</span></span>
<span data-ttu-id="3b64f-123">Ebben a cikkben megtanulta rendelkezik többféle módon hozhat létre HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="3b64f-123">In this article, you have learned several ways to create an HDInsight cluster.</span></span> <span data-ttu-id="3b64f-124">További tudnivalókért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="3b64f-124">To learn more, see the following articles:</span></span>

* <span data-ttu-id="3b64f-125">[Azure HDInsight – első lépések][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="3b64f-125">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="3b64f-126">[Hdinsight Hadoop-fürtök létrehozása][hdinsight-provision]</span><span class="sxs-lookup"><span data-stu-id="3b64f-126">[Create Hadoop clusters in HDInsight][hdinsight-provision]</span></span>
* [<span data-ttu-id="3b64f-127">Hdinsight Hadoop-fürtök kezelése az Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="3b64f-127">Manage Hadoop clusters in HDInsight by using the Azure portal</span></span>](hdinsight-administer-use-management-portal.md)
* [<span data-ttu-id="3b64f-128">A HDInsight .NET SDK-referencia</span><span class="sxs-lookup"><span data-stu-id="3b64f-128">HDInsight .NET SDK reference</span></span>](https://msdn.microsoft.com/library/mt271028.aspx)
* [<span data-ttu-id="3b64f-129">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="3b64f-129">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="3b64f-130">Use Sqoop with HDInsight</span><span class="sxs-lookup"><span data-stu-id="3b64f-130">Use Sqoop with HDInsight</span></span>](hdinsight-use-sqoop-mac-linux.md)
* [<span data-ttu-id="3b64f-131">Nem interaktív hitelesítéssel ellátott .Net HDInsight-alkalmazások létrehozása</span><span class="sxs-lookup"><span data-stu-id="3b64f-131">Create non-interactive authentication .NET HDInsight applications</span></span>](hdinsight-create-non-interactive-authentication-dotnet-applications.md)

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md


