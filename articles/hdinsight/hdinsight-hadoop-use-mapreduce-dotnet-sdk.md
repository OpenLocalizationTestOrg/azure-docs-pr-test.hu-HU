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
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>A HDInsight .NET SDK használatával MapReduce-feladatok futtatása
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Ismerje meg, hogyan toosubmit MapReduce feladatok HDInsight .NET SDK használatával. HDInsight fürtök rendelkeznek egy bizonyos MapReduce-minták a jar-fájlra. hello jar-fájlra van */example/jars/hadoop-mapreduce-examples.jar*.  Hello minták egyik *wordcount*. A C# konzol alkalmazás toosubmit a wordcount feladathoz fejleszt.  hello feladat beolvassa hello */example/data/gutenberg/davinci.txt* fájlt, és kimenetek hello eredmények túl*/example/data/davinciwordcount*.  Ha azt szeretné, hogy toorerun hello alkalmazás, kell tisztítása hello kimeneti mappa.

> [!NOTE]
> egy Windows ügyfél hello cikkben leírt lépéseket kell végrehajtani. A tájékoztatást a Linux, OS X vagy UNIX rendszerű ügyfél toowork struktúra használja a hello lapon választó hello hello cikk tetején látható.
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk megkezdése előtt rendelkeznie kell a következő elemek hello:

* **Hdinsight Hadoop-fürthöz**. Lásd: [hdinsight Linux-alapú Hadoop használatának megkezdésében](./hdinsight-hadoop-linux-tutorial-get-started.md).
* **A Visual Studio 2013 vagy 2015/2017**.

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>A HDInsight .NET SDK használatával MapReduce-feladatok elküldése
hello HDInsight .NET SDK biztosít a .NET ügyféloldali kódtáraknál, így azokat könnyebben toowork a .NET-HDInsight-fürtökkel. 

**tooSubmit feladatok**

1. Hozzon létre egy C# konzolalkalmazást a Visual Studióban.
2. Hello Nuget Package Manager Console futtassa a következő parancs hello:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. A következő kód hello használata:
   
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
4. Nyomja le az **F5** toorun hello alkalmazás.

toorun hello feladat ebben az esetben meg kell változtatnia hello feladat kimeneti mappa neve, hello mintában, a "/ Példa/data/davinciwordcount".

Ha hello feladat sikeresen befejeződik, a hello alkalmazás nyomtat hello kimeneti fájl "rész-r-00000" hello tartalma.

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta rendelkezik számos módon toocreate HDInsight-fürtöt. toolearn több, tekintse meg a következő cikkek hello:

* A Hive feladat elküldése, lásd: [HDInsight .NET SDK használatával futtassa Hive lekérdezések](hdinsight-hadoop-use-hive-dotnet-sdk.md).
* A HDInsight-fürtök létrehozására, lásd: [hdinsight létrehozása Linux-alapú Hadoop-fürtök](hdinsight-hadoop-provision-linux-clusters.md).
* A HDInsight-fürtök kezelése, lásd: [Hadoop kezelése a HDInsight-fürtök](hdinsight-administer-use-portal-linux.md).
* Tanulási hello HDInsight .NET SDK, lásd: [HDInsight .NET SDK referenciáit](https://msdn.microsoft.com/library/mt271028.aspx).
* A nem interaktív hitelesítés tooAzure, lásd: [.NET HDInsight-alkalmazások létrehozása a nem interaktív hitelesítés](hdinsight-create-non-interactive-authentication-dotnet-applications.md).

