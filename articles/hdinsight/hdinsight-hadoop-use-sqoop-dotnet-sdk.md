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
# <a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>A hdinsight Hadoop .NET SDK használatával Sqoop feladatok futtatása
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Megtudhatja, hogyan toouse HDInsight .NET SDK toorun Sqoop HDInsight tooimport feladat, és exportálja a HDInsight-fürt és Azure SQL database vagy az SQL Server-adatbázis között.

> [!NOTE]
> a cikkben ismertetett hello használható vagy egy Windows-alapú vagy Linux-alapú HDInsight-fürthöz; azonban ezek csak a lépések Windows ügyfélről. Hello lapon választó, ez a cikk toochoose hello felül más módszerekkel.
> 
> 

### <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez a következő elemek hello kell rendelkeznie:

* **Hdinsight Hadoop-fürthöz**. Lásd: [fürt és az SQL-adatbázis létrehozása](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="use-sqoop-on-hdinsight-clusters-using-net-sdk"></a>Sqoop használata a HDInsight-fürtökön .NET SDK használatával
hello HDInsight .NET SDK biztosít a .NET ügyféloldali kódtáraknál, így azokat könnyebben toowork a .NET-HDInsight-fürtökkel. Ebben a szakaszban hozzon létre egy C# console application tooexport hello hivesampletable toohello SQL-adatbázis táblája ebben az oktatóanyagban korábban létrehozott.

## <a name="submit-a-sqoop-job"></a>Sqoop feladat elküldése

1. Hozzon létre egy C# konzolalkalmazást a Visual Studióban.
2. Visual Studio Csomagkezelő konzol hello futtassa a következő Nuget parancs tooimport hello csomag hello.
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. A következő kódot a Program.cs fájlban hello hello használata:
   
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
4. Nyomja le az **F5** toorun hello program. 

## <a name="limitations"></a>Korlátozások
* Tömeges export - a Linux-alapú HDInsight, hello Sqoop használt tooexport adatok tooMicrosoft SQL Server vagy az Azure SQL Database jelenleg nem támogatja a tömeges beszúrások.
* Kötegelés – a Linux-alapú hdinsight eszközzel, hello használatakor `-batch` Beszúrások végrehajtásakor kapcsoló, a Sqoop hajt végre több Beszúrások hello beszúrási műveletek kötegelése helyett.

## <a name="next-steps"></a>Következő lépések
Most megtanulta, hogyan toouse Sqoop. toolearn több, lásd:

* [Oozie használata a HDInsight](hdinsight-use-oozie.md): egy Oozie munkafolyamat használja Sqoop műveletét.
* [HDInsight eszközzel repülési késleltetés adatok elemzése](hdinsight-analyze-flight-delay-data.md): tooanalyze felhőszolgáltató közötti átviteléhez használja Hive késleltetés az adatok, és majd a Sqoop tooexport tooan Azure SQL-adatbázis használata.
* [Töltse fel az adatok tooHDInsight](hdinsight-upload-data.md): található adatok tooHDInsight vagy az Azure Blob storage feltöltése más módszerrel.

