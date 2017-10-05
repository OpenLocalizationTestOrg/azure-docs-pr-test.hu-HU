---
title: "Küldje el a MapReduce-feladatok használata a HDInsight .NET SDK - Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan elküldeni a MapReduce-feladatok az Azure HDInsight Hadoop HDInsight .NET SDK használatával."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: c85e44b0-85fd-4185-ad1c-c34a9fe5ef44
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jgao
ms.openlocfilehash: 015435270c31bafea0ebf5303b459338755c1410
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a><span data-ttu-id="72fd3-103">A HDInsight .NET SDK használatával MapReduce-feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="72fd3-103">Run MapReduce jobs using HDInsight .NET SDK</span></span>
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="72fd3-104">Megtudhatja, hogyan elküldeni a MapReduce-feladatok HDInsight .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="72fd3-104">Learn how to submit MapReduce jobs using HDInsight .NET SDK.</span></span> <span data-ttu-id="72fd3-105">HDInsight fürtök rendelkeznek egy bizonyos MapReduce-minták a jar-fájlra.</span><span class="sxs-lookup"><span data-stu-id="72fd3-105">HDInsight clusters come with a jar file with some MapReduce samples.</span></span> <span data-ttu-id="72fd3-106">A jar-fájlra van */example/jars/hadoop-mapreduce-examples.jar*.</span><span class="sxs-lookup"><span data-stu-id="72fd3-106">The jar file is */example/jars/hadoop-mapreduce-examples.jar*.</span></span>  <span data-ttu-id="72fd3-107">A minták egyik *wordcount*.</span><span class="sxs-lookup"><span data-stu-id="72fd3-107">One of the samples is *wordcount*.</span></span> <span data-ttu-id="72fd3-108">A C# Konzolalkalmazás a wordcount feladat elküldése fejleszt.</span><span class="sxs-lookup"><span data-stu-id="72fd3-108">You develop a C# console application to submit a wordcount job.</span></span>  <span data-ttu-id="72fd3-109">A feladat beolvassa a */example/data/gutenberg/davinci.txt* fájlt, és az eredmények */example/data/davinciwordcount*.</span><span class="sxs-lookup"><span data-stu-id="72fd3-109">The job reads the */example/data/gutenberg/davinci.txt* file, and outputs the results to */example/data/davinciwordcount*.</span></span>  <span data-ttu-id="72fd3-110">Ha szeretné újra futtatni az alkalmazást, a kimeneti mappa kell tisztítása.</span><span class="sxs-lookup"><span data-stu-id="72fd3-110">If you want to rerun the application, you must clean up the output folder.</span></span>

> [!NOTE]
> <span data-ttu-id="72fd3-111">A cikkben leírt lépéseket egy Windows ügyfél kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="72fd3-111">The steps in this article must be performed from a Windows client.</span></span> <span data-ttu-id="72fd3-112">Kapcsolatos információkért használata a Linux, OS X vagy UNIX rendszerű ügyfélnél Hive használata a lap választó, a cikk tetején látható.</span><span class="sxs-lookup"><span data-stu-id="72fd3-112">For information on using a Linux, OS X, or Unix client to work with Hive, use the tab selector shown on the top of the article.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="72fd3-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="72fd3-113">Prerequisites</span></span>
<span data-ttu-id="72fd3-114">Ez a cikk megkezdése előtt rendelkeznie kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="72fd3-114">Before you begin this article, you must have the following items:</span></span>

* <span data-ttu-id="72fd3-115">**Hdinsight Hadoop-fürthöz**.</span><span class="sxs-lookup"><span data-stu-id="72fd3-115">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="72fd3-116">Lásd: [hdinsight Linux-alapú Hadoop használatának megkezdésében](./hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="72fd3-116">See [Get started using Linux-based Hadoop in HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
* <span data-ttu-id="72fd3-117">**A Visual Studio 2013 vagy 2015/2017**.</span><span class="sxs-lookup"><span data-stu-id="72fd3-117">**Visual Studio 2013/2015/2017**.</span></span>

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a><span data-ttu-id="72fd3-118">A HDInsight .NET SDK használatával MapReduce-feladatok elküldése</span><span class="sxs-lookup"><span data-stu-id="72fd3-118">Submit MapReduce jobs using HDInsight .NET SDK</span></span>
<span data-ttu-id="72fd3-119">A HDInsight .NET SDK biztosít a .NET ügyféloldali kódtáraknál, így azokat könnyebben működéséhez a .NET-HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="72fd3-119">The HDInsight .NET SDK provides .NET client libraries, which makes it easier to work with HDInsight clusters from .NET.</span></span> 

<span data-ttu-id="72fd3-120">**Feladatok küldéséhez**</span><span class="sxs-lookup"><span data-stu-id="72fd3-120">**To Submit jobs**</span></span>

1. <span data-ttu-id="72fd3-121">Hozzon létre egy C# konzolalkalmazást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="72fd3-121">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="72fd3-122">A Nuget-Csomagkezelő konzolról futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="72fd3-122">From the Nuget Package Manager Console, run the following command:</span></span>
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. <span data-ttu-id="72fd3-123">A következő kód használható:</span><span class="sxs-lookup"><span data-stu-id="72fd3-123">Use the following code:</span></span>
   
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string existingClusterName = "<Your HDInsight Cluster Name>";
                private const string existingClusterUri = existingClusterName + ".azurehdinsight.net";
                private const string existingClusterUsername = "<Cluster Username>";
                private const string existingClusterPassword = "<Cluster User Password>";
   
                private const string defaultStorageAccountName = "<Default Storage Account Name>"; //<StorageAccountName>.blob.core.windows.net
                private const string defaultStorageAccountKey = "<Default Storage Account Key>";
                private const string defaultStorageContainerName = "<Default Blob Container Name>";

                private const string sourceFile = "/example/data/gutenberg/davinci.txt";  
                private const string outputFolder = "/example/data/davinciwordcount";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = existingClusterUsername, Password = existingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(existingClusterUri, clusterCredentials);
   
                    SubmitMRJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitMRJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };
   
                    var paras = new MapReduceJobSubmissionParameters
                    {
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting the MR job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitMapReduceJob(paras);
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
                    System.Console.WriteLine("Job output is: ");
                    var storageAccess = new AzureStorageAccess(defaultStorageAccountName, defaultStorageAccountKey,
                        defaultStorageContainerName);
        
                    if (jobDetail.ExitValue == 0)
                    {
                        // Create the storage account object
                        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=" + 
                            defaultStorageAccountName + 
                            ";AccountKey=" + defaultStorageAccountKey);
        
                        // Create the blob client.
                        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
                        // Retrieve reference to a previously created container.
                        CloudBlobContainer container = blobClient.GetContainerReference(defaultStorageContainerName);
        
                        CloudBlockBlob blockBlob = container.GetBlockBlobReference(outputFolder.Substring(1) + "/part-r-00000");
        
                        using (var stream = blockBlob.OpenRead())
                        {
                            using (StreamReader reader = new StreamReader(stream))
                            {
                                while (!reader.EndOfStream)
                                {
                                    System.Console.WriteLine(reader.ReadLine());
                                }
                            }
                        }
                    }
                    else
                    {
                        // fetch stderr output in case of failure
                        var output = _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); 
        
                        using (var reader = new StreamReader(output, Encoding.UTF8))
                        {
                            string value = reader.ReadToEnd();
                            System.Console.WriteLine(value);
                        }
        
                    }
                }
            }
        }
4. <span data-ttu-id="72fd3-124">Az alkalmazás futtatásához nyomja le az **F5** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="72fd3-124">Press **F5** to run the application.</span></span>

<span data-ttu-id="72fd3-125">Futtassa újra a feladatot, módosítania kell a feladat kimeneti mappa nevét, a példában az "/ Példa/data/davinciwordcount".</span><span class="sxs-lookup"><span data-stu-id="72fd3-125">To run the job again, you must change the job output folder name, in the sample, it is "/example/data/davinciwordcount".</span></span>

<span data-ttu-id="72fd3-126">Ha a feladat sikeresen befejeződik, az alkalmazás a kimeneti fájl "rész-r-00000" tartalom nyomtatása.</span><span class="sxs-lookup"><span data-stu-id="72fd3-126">When the job completes successfully, the application prints the content of the output file "part-r-00000".</span></span>

## <a name="next-steps"></a><span data-ttu-id="72fd3-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="72fd3-127">Next steps</span></span>
<span data-ttu-id="72fd3-128">Ebben a cikkben megtanulta rendelkezik többféle módon hozhat létre HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="72fd3-128">In this article, you have learned several ways to create an HDInsight cluster.</span></span> <span data-ttu-id="72fd3-129">További tudnivalókért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="72fd3-129">To learn more, see the following articles:</span></span>

* <span data-ttu-id="72fd3-130">A Hive feladat elküldése, lásd: [HDInsight .NET SDK használatával futtassa Hive lekérdezések](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="72fd3-130">For submitting a Hive job, see [Run Hive queries using HDInsight .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span>
* <span data-ttu-id="72fd3-131">A HDInsight-fürtök létrehozására, lásd: [hdinsight létrehozása Linux-alapú Hadoop-fürtök](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="72fd3-131">For creating HDInsight clusters, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="72fd3-132">A HDInsight-fürtök kezelése, lásd: [Hadoop kezelése a HDInsight-fürtök](hdinsight-administer-use-portal-linux.md).</span><span class="sxs-lookup"><span data-stu-id="72fd3-132">For managing HDInsight clusters, see [Manage Hadoop clusters in HDInsight](hdinsight-administer-use-portal-linux.md).</span></span>
* <span data-ttu-id="72fd3-133">A HDInsight .NET SDK tanulási, lásd: [HDInsight .NET SDK referenciáit](https://msdn.microsoft.com/library/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="72fd3-133">For learning the HDInsight .NET SDK, see [HDInsight .NET SDK reference](https://msdn.microsoft.com/library/mt271028.aspx).</span></span>
* <span data-ttu-id="72fd3-134">A nem interaktív hitelesítés az Azure-ba, lásd: [.NET HDInsight-alkalmazások létrehozása a nem interaktív hitelesítés](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span><span class="sxs-lookup"><span data-stu-id="72fd3-134">For non-interactive authenticate to Azure, see [Create non-interactive authentication .NET HDInsight applications](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span></span>

