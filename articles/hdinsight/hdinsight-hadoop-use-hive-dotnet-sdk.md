---
title: "a HDInsight .NET SDK - Azure használatával aaaRun Hive-lekérdezések |} Microsoft Docs"
description: "Ismerje meg, hogyan toosubmit Hadoop feladatok tooAzure HDInsight Hadoop HDInsight .NET SDK használatával."
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
ms.openlocfilehash: 11f07d90405d3e804774610e242813927df59a03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>A HDInsight .NET SDK használatával Hive-lekérdezések futtatása
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Ismerje meg, hogyan toosubmit Hive lekérdezi a HDInsight .NET SDK használatával. A C# programban toosubmit Hive táblák listázása a Hive-lekérdezések írása, és hello eredmények megjelenítéséhez.

> [!NOTE]
> egy Windows ügyfél hello cikkben leírt lépéseket kell végrehajtani. A tájékoztatást a Linux, OS X vagy UNIX rendszerű ügyfél toowork struktúra használja a hello lapon választó hello hello cikk tetején látható.
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk megkezdése előtt rendelkeznie kell a következő elemek hello:

* **Hdinsight Hadoop-fürthöz**. Lásd: [hdinsight Linux-alapú Hadoop használatának megkezdésében](./hdinsight-hadoop-linux-tutorial-get-started.md).
* **A Visual Studio 2013 vagy 2015/2017**.

## <a name="submit-hive-queries-using-hdinsight-net-sdk"></a>Küldje el a Hive-lekérdezéseket a HDInsight .NET SDK használatával
hello HDInsight .NET SDK biztosít a .NET ügyféloldali kódtáraknál, így azokat könnyebben toowork a .NET-HDInsight-fürtökkel. 

**tooSubmit feladatok**

1. Hozzon létre egy C# konzolalkalmazást a Visual Studióban.
2. Hello Nuget Package Manager Console futtassa a következő parancs hello:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. A következő kód hello használata:

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
                    System.Console.WriteLine("hello application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitHiveJob();
   
                    System.Console.WriteLine("Press ENTER toocontinue ...");
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
   
                    System.Console.WriteLine("Submitting hello Hive job toohello cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
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
4. Nyomja le az **F5** toorun hello alkalmazás.

kell, hogy hasonló hello alkalmazás hello kimenete:

![HDInsight Hadoop Hive feladat kimenetére](./media/hdinsight-hadoop-use-hive-dotnet-sdk/hdinsight-hadoop-use-hive-net-sdk-output.png)

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta rendelkezik számos módon toocreate HDInsight-fürtöt. toolearn több, tekintse meg a következő cikkek hello:

* [Azure HDInsight – első lépések][hdinsight-get-started]
* [Hdinsight Hadoop-fürtök létrehozása][hdinsight-provision]
* [Hdinsight Hadoop-fürtök kezelése hello Azure-portál használatával](hdinsight-administer-use-management-portal.md)
* [A HDInsight .NET SDK-referencia](https://msdn.microsoft.com/library/mt271028.aspx)
* [A Pig használata a HDInsightban](hdinsight-use-pig.md)
* [Use Sqoop with HDInsight](hdinsight-use-sqoop-mac-linux.md)
* [Nem interaktív hitelesítéssel ellátott .Net HDInsight-alkalmazások létrehozása](hdinsight-create-non-interactive-authentication-dotnet-applications.md)

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md


