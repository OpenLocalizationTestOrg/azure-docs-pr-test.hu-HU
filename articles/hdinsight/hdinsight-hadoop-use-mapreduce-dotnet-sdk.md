---
title: "a HDInsight .NET SDK - Azure használatával aaaSubmit MapReduce-feladatok |} Microsoft Docs"
description: "Ismerje meg, hogyan toosubmit MapReduce feladatok tooAzure HDInsight Hadoop HDInsight .NET SDK használatával."
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
ms.openlocfilehash: d00e31400b8fa47982c31d00bfdcdb304bcb0b59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a><span data-ttu-id="0e905-103">A HDInsight .NET SDK használatával MapReduce-feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="0e905-103">Run MapReduce jobs using HDInsight .NET SDK</span></span>
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="0e905-104">Ismerje meg, hogyan toosubmit MapReduce feladatok HDInsight .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="0e905-104">Learn how toosubmit MapReduce jobs using HDInsight .NET SDK.</span></span> <span data-ttu-id="0e905-105">HDInsight fürtök rendelkeznek egy bizonyos MapReduce-minták a jar-fájlra.</span><span class="sxs-lookup"><span data-stu-id="0e905-105">HDInsight clusters come with a jar file with some MapReduce samples.</span></span> <span data-ttu-id="0e905-106">hello jar-fájlra van */example/jars/hadoop-mapreduce-examples.jar*.</span><span class="sxs-lookup"><span data-stu-id="0e905-106">hello jar file is */example/jars/hadoop-mapreduce-examples.jar*.</span></span>  <span data-ttu-id="0e905-107">Hello minták egyik *wordcount*.</span><span class="sxs-lookup"><span data-stu-id="0e905-107">One of hello samples is *wordcount*.</span></span> <span data-ttu-id="0e905-108">A C# konzol alkalmazás toosubmit a wordcount feladathoz fejleszt.</span><span class="sxs-lookup"><span data-stu-id="0e905-108">You develop a C# console application toosubmit a wordcount job.</span></span>  <span data-ttu-id="0e905-109">hello feladat beolvassa hello */example/data/gutenberg/davinci.txt* fájlt, és kimenetek hello eredmények túl*/example/data/davinciwordcount*.</span><span class="sxs-lookup"><span data-stu-id="0e905-109">hello job reads hello */example/data/gutenberg/davinci.txt* file, and outputs hello results too*/example/data/davinciwordcount*.</span></span>  <span data-ttu-id="0e905-110">Ha azt szeretné, hogy toorerun hello alkalmazás, kell tisztítása hello kimeneti mappa.</span><span class="sxs-lookup"><span data-stu-id="0e905-110">If you want toorerun hello application, you must clean up hello output folder.</span></span>

> [!NOTE]
> <span data-ttu-id="0e905-111">egy Windows ügyfél hello cikkben leírt lépéseket kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="0e905-111">hello steps in this article must be performed from a Windows client.</span></span> <span data-ttu-id="0e905-112">A tájékoztatást a Linux, OS X vagy UNIX rendszerű ügyfél toowork struktúra használja a hello lapon választó hello hello cikk tetején látható.</span><span class="sxs-lookup"><span data-stu-id="0e905-112">For information on using a Linux, OS X, or Unix client toowork with Hive, use hello tab selector shown on hello top of hello article.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="0e905-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0e905-113">Prerequisites</span></span>
<span data-ttu-id="0e905-114">Ez a cikk megkezdése előtt rendelkeznie kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="0e905-114">Before you begin this article, you must have hello following items:</span></span>

* <span data-ttu-id="0e905-115">**Hdinsight Hadoop-fürthöz**.</span><span class="sxs-lookup"><span data-stu-id="0e905-115">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="0e905-116">Lásd: [hdinsight Linux-alapú Hadoop használatának megkezdésében](./hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0e905-116">See [Get started using Linux-based Hadoop in HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
* <span data-ttu-id="0e905-117">**A Visual Studio 2013 vagy 2015/2017**.</span><span class="sxs-lookup"><span data-stu-id="0e905-117">**Visual Studio 2013/2015/2017**.</span></span>

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a><span data-ttu-id="0e905-118">A HDInsight .NET SDK használatával MapReduce-feladatok elküldése</span><span class="sxs-lookup"><span data-stu-id="0e905-118">Submit MapReduce jobs using HDInsight .NET SDK</span></span>
<span data-ttu-id="0e905-119">hello HDInsight .NET SDK biztosít a .NET ügyféloldali kódtáraknál, így azokat könnyebben toowork a .NET-HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="0e905-119">hello HDInsight .NET SDK provides .NET client libraries, which makes it easier toowork with HDInsight clusters from .NET.</span></span> 

<span data-ttu-id="0e905-120">**tooSubmit feladatok**</span><span class="sxs-lookup"><span data-stu-id="0e905-120">**tooSubmit jobs**</span></span>

1. <span data-ttu-id="0e905-121">Hozzon létre egy C# konzolalkalmazást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="0e905-121">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="0e905-122">Hello Nuget Package Manager Console futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="0e905-122">From hello Nuget Package Manager Console, run hello following command:</span></span>
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. <span data-ttu-id="0e905-123">A következő kód hello használata:</span><span class="sxs-lookup"><span data-stu-id="0e905-123">Use hello following code:</span></span>
   
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
                    System.Console.WriteLine("hello application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = existingClusterUsername, Password = existingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(existingClusterUri, clusterCredentials);
   
                    SubmitMRJob();
   
                    System.Console.WriteLine("Press ENTER toocontinue ...");
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
   
                    System.Console.WriteLine("Submitting hello MR job toohello cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitMapReduceJob(paras);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);
   
                    System.Console.WriteLine("Waiting for hello job completion ...");
   
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
                        // Create hello storage account object
                        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=" + 
                            defaultStorageAccountName + 
                            ";AccountKey=" + defaultStorageAccountKey);
        
                        // Create hello blob client.
                        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
                        // Retrieve reference tooa previously created container.
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
4. <span data-ttu-id="0e905-124">Nyomja le az **F5** toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0e905-124">Press **F5** toorun hello application.</span></span>

<span data-ttu-id="0e905-125">toorun hello feladat ebben az esetben meg kell változtatnia hello feladat kimeneti mappa neve, hello mintában, a "/ Példa/data/davinciwordcount".</span><span class="sxs-lookup"><span data-stu-id="0e905-125">toorun hello job again, you must change hello job output folder name, in hello sample, it is "/example/data/davinciwordcount".</span></span>

<span data-ttu-id="0e905-126">Ha hello feladat sikeresen befejeződik, a hello alkalmazás nyomtat hello kimeneti fájl "rész-r-00000" hello tartalma.</span><span class="sxs-lookup"><span data-stu-id="0e905-126">When hello job completes successfully, hello application prints hello content of hello output file "part-r-00000".</span></span>

## <a name="next-steps"></a><span data-ttu-id="0e905-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0e905-127">Next steps</span></span>
<span data-ttu-id="0e905-128">Ebben a cikkben megtanulta rendelkezik számos módon toocreate HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="0e905-128">In this article, you have learned several ways toocreate an HDInsight cluster.</span></span> <span data-ttu-id="0e905-129">toolearn több, tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="0e905-129">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="0e905-130">A Hive feladat elküldése, lásd: [HDInsight .NET SDK használatával futtassa Hive lekérdezések](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="0e905-130">For submitting a Hive job, see [Run Hive queries using HDInsight .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span>
* <span data-ttu-id="0e905-131">A HDInsight-fürtök létrehozására, lásd: [hdinsight létrehozása Linux-alapú Hadoop-fürtök](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="0e905-131">For creating HDInsight clusters, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="0e905-132">A HDInsight-fürtök kezelése, lásd: [Hadoop kezelése a HDInsight-fürtök](hdinsight-administer-use-portal-linux.md).</span><span class="sxs-lookup"><span data-stu-id="0e905-132">For managing HDInsight clusters, see [Manage Hadoop clusters in HDInsight](hdinsight-administer-use-portal-linux.md).</span></span>
* <span data-ttu-id="0e905-133">Tanulási hello HDInsight .NET SDK, lásd: [HDInsight .NET SDK referenciáit](https://msdn.microsoft.com/library/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="0e905-133">For learning hello HDInsight .NET SDK, see [HDInsight .NET SDK reference](https://msdn.microsoft.com/library/mt271028.aspx).</span></span>
* <span data-ttu-id="0e905-134">A nem interaktív hitelesítés tooAzure, lásd: [.NET HDInsight-alkalmazások létrehozása a nem interaktív hitelesítés](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span><span class="sxs-lookup"><span data-stu-id="0e905-134">For non-interactive authenticate tooAzure, see [Create non-interactive authentication .NET HDInsight applications](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span></span>

