---
title: a Hadoop - Azure HDInsight .NET SDK-val feladatok aaaRun Apache Pig |} Microsoft Docs
description: Ismerje meg, hogyan toouse hello .NET SDK-val a HDInsight Hadoop toosubmit Pig feladatok tooHadoop.
services: hdinsight
documentationcenter: .net
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: fa11d49a-328c-47e7-b16d-e7ed2a453195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: 1d4ceebd7c168372d23fe29a088f04676686de30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-using-hello-net-sdk-for-hadoop-in-hdinsight"></a>A hdinsight Hadoop hello .NET SDK használatával a Pig-feladatok futtatása

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Ismerje meg, hogyan toouse hello .NET SDK a Hadoop toosubmit Apache Pig feladatok tooHadoop on Azure HDInsight.

HDInsight .NET SDK hello .NET ügyféloldali kódtáraknál, amely megkönnyíti a HDInsight-fürtökkel a .NET-könnyebb toowork biztosít. Pig lehetővé teszi toocreate MapReduce műveletek modellezési adatátalakítást sorozata alapján. Ez a dokumentum elsajátíthatja, hogyan feladat toouse alapvető C# alkalmazás toosubmit a Pig-tooan HDInsight-fürthöz.

## <a name="prerequisites"></a>Előfeltételek

toocomplete hello cikkben leírt lépéseket, hello következő kell.

* (A HDInsight Hadoop) Azure HDInsight-fürtök (vagy a Windows vagy Linux-alapú).

  > [!IMPORTANT]
  > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* A Visual Studio 2012, 2013, 2015-öt vagy 2017.

## <a name="create-hello-application"></a>Hello alkalmazás létrehozása

hello HDInsight .NET SDK biztosít a .NET ügyféloldali kódtáraknál, így azokat könnyebben toowork a .NET-HDInsight-fürtökkel.

1. A hello **fájl** elemét a Visual Studióban, válassza ki **új** majd **projekt**.

2. Hello új projekt Ha típusa, vagy jelölje be a következő hello értékeket:

   | Tulajdonság | Érték |
   | ------ | ------ |
   | Kategória | Sablonok/Visual C#/Windows |
   | Sablon | Konzolalkalmazás |
   | Név | SubmitPigJob |

3. Kattintson a **OK** toocreate hello projekt.

4. A hello **eszközök** menüjében válassza **Kódtárcsomag-kezelő** vagy **Nuget-Csomagkezelő**, majd válassza ki **Csomagkezelő konzol**.

5. tooinstall hello .NET SDK csomagot, a következő parancs hello használata:

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. A Megoldáskezelőben kattintson duplán a **Program.cs** tooopen azt. Cserélje le a meglévő kódot hello hello következő.

    ```csharp
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

                SubmitPigJob();

                System.Console.WriteLine("Press ENTER toocontinue ...");
                System.Console.ReadLine();
            }

            private static void SubmitPigJob()
            {
                var parameters = new PigJobSubmissionParameters
                {
                    Query = @"LOGS = LOAD '/example/data/sample.log';
                                LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                RESULT = order FREQUENCIES by COUNT desc;
                                DUMP RESULT;"
                };

                System.Console.WriteLine("Submitting hello Pig job toohello cluster...");
                var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                System.Console.WriteLine("Validating that hello response is as expected...");
                System.Console.WriteLine("Response status code is " + response.StatusCode);
                System.Console.WriteLine("Validating hello response object...");
                System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
            }
        }
    }
    ```

7. toostart hello alkalmazás, nyomja meg az **F5**.

8. tooexit hello alkalmazás, nyomja meg az **ENTER**.

## <a name="summary"></a>Összefoglalás

Ahogy látja, a .NET SDK for Hadoop hello lehetővé teszi toocreate .NET alkalmazások elküldeni a Pig feladatot tooan HDInsight-fürt, amely hello feladat állapotának figyelésére.

## <a name="next-steps"></a>Következő lépések

Információk a hdinsight Pig: [a Pig használata a hdinsight Hadoop](hdinsight-use-pig.md).

A HDInsight Hadoop használatával kapcsolatos további információkért tekintse meg a következő dokumentumok hello:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)
* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
