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
# <a name="run-pig-jobs-using-hello-net-sdk-for-hadoop-in-hdinsight"></a><span data-ttu-id="edb41-103">A hdinsight Hadoop hello .NET SDK használatával a Pig-feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="edb41-103">Run Pig jobs using hello .NET SDK for Hadoop in HDInsight</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="edb41-104">Ismerje meg, hogyan toouse hello .NET SDK a Hadoop toosubmit Apache Pig feladatok tooHadoop on Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="edb41-104">Learn how toouse hello .NET SDK for Hadoop toosubmit Apache Pig jobs tooHadoop on Azure HDInsight.</span></span>

<span data-ttu-id="edb41-105">HDInsight .NET SDK hello .NET ügyféloldali kódtáraknál, amely megkönnyíti a HDInsight-fürtökkel a .NET-könnyebb toowork biztosít.</span><span class="sxs-lookup"><span data-stu-id="edb41-105">hello HDInsight .NET SDK provides .NET client libraries that makes it easier toowork with HDInsight clusters from .NET.</span></span> <span data-ttu-id="edb41-106">Pig lehetővé teszi toocreate MapReduce műveletek modellezési adatátalakítást sorozata alapján.</span><span class="sxs-lookup"><span data-stu-id="edb41-106">Pig allows you toocreate MapReduce operations by modeling a series of data transformations.</span></span> <span data-ttu-id="edb41-107">Ez a dokumentum elsajátíthatja, hogyan feladat toouse alapvető C# alkalmazás toosubmit a Pig-tooan HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="edb41-107">In this document, you learn how toouse a basic C# application toosubmit a Pig job tooan HDInsight cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="edb41-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="edb41-108">Prerequisites</span></span>

<span data-ttu-id="edb41-109">toocomplete hello cikkben leírt lépéseket, hello következő kell.</span><span class="sxs-lookup"><span data-stu-id="edb41-109">toocomplete hello steps in this article, you need hello following.</span></span>

* <span data-ttu-id="edb41-110">(A HDInsight Hadoop) Azure HDInsight-fürtök (vagy a Windows vagy Linux-alapú).</span><span class="sxs-lookup"><span data-stu-id="edb41-110">An Azure HDInsight (Hadoop on HDInsight) cluster (either Windows or Linux-based).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="edb41-111">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="edb41-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="edb41-112">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="edb41-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="edb41-113">A Visual Studio 2012, 2013, 2015-öt vagy 2017.</span><span class="sxs-lookup"><span data-stu-id="edb41-113">Visual Studio 2012, 2013, 2015 or 2017.</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="edb41-114">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="edb41-114">Create hello application</span></span>

<span data-ttu-id="edb41-115">hello HDInsight .NET SDK biztosít a .NET ügyféloldali kódtáraknál, így azokat könnyebben toowork a .NET-HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="edb41-115">hello HDInsight .NET SDK provides .NET client libraries, which makes it easier toowork with HDInsight clusters from .NET.</span></span>

1. <span data-ttu-id="edb41-116">A hello **fájl** elemét a Visual Studióban, válassza ki **új** majd **projekt**.</span><span class="sxs-lookup"><span data-stu-id="edb41-116">From hello **File** menu in Visual Studio, select **New** and then select **Project**.</span></span>

2. <span data-ttu-id="edb41-117">Hello új projekt Ha típusa, vagy jelölje be a következő hello értékeket:</span><span class="sxs-lookup"><span data-stu-id="edb41-117">For hello new project, type or select hello following values:</span></span>

   | <span data-ttu-id="edb41-118">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="edb41-118">Property</span></span> | <span data-ttu-id="edb41-119">Érték</span><span class="sxs-lookup"><span data-stu-id="edb41-119">Value</span></span> |
   | ------ | ------ |
   | <span data-ttu-id="edb41-120">Kategória</span><span class="sxs-lookup"><span data-stu-id="edb41-120">Category</span></span> | <span data-ttu-id="edb41-121">Sablonok/Visual C#/Windows</span><span class="sxs-lookup"><span data-stu-id="edb41-121">Templates/Visual C#/Windows</span></span> |
   | <span data-ttu-id="edb41-122">Sablon</span><span class="sxs-lookup"><span data-stu-id="edb41-122">Template</span></span> | <span data-ttu-id="edb41-123">Konzolalkalmazás</span><span class="sxs-lookup"><span data-stu-id="edb41-123">Console Application</span></span> |
   | <span data-ttu-id="edb41-124">Név</span><span class="sxs-lookup"><span data-stu-id="edb41-124">Name</span></span> | <span data-ttu-id="edb41-125">SubmitPigJob</span><span class="sxs-lookup"><span data-stu-id="edb41-125">SubmitPigJob</span></span> |

3. <span data-ttu-id="edb41-126">Kattintson a **OK** toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="edb41-126">Click **OK** toocreate hello project.</span></span>

4. <span data-ttu-id="edb41-127">A hello **eszközök** menüjében válassza **Kódtárcsomag-kezelő** vagy **Nuget-Csomagkezelő**, majd válassza ki **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="edb41-127">From hello **Tools** menu, select **Library Package Manager** or **Nuget Package Manager**, and then select **Package Manager Console**.</span></span>

5. <span data-ttu-id="edb41-128">tooinstall hello .NET SDK csomagot, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="edb41-128">tooinstall hello .NET SDK packages, use hello following command:</span></span>

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. <span data-ttu-id="edb41-129">A Megoldáskezelőben kattintson duplán a **Program.cs** tooopen azt.</span><span class="sxs-lookup"><span data-stu-id="edb41-129">From Solution Explorer, double-click **Program.cs** tooopen it.</span></span> <span data-ttu-id="edb41-130">Cserélje le a meglévő kódot hello hello következő.</span><span class="sxs-lookup"><span data-stu-id="edb41-130">Replace hello existing code with hello following.</span></span>

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

7. <span data-ttu-id="edb41-131">toostart hello alkalmazás, nyomja meg az **F5**.</span><span class="sxs-lookup"><span data-stu-id="edb41-131">toostart hello application, press **F5**.</span></span>

8. <span data-ttu-id="edb41-132">tooexit hello alkalmazás, nyomja meg az **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="edb41-132">tooexit hello application, press **ENTER**.</span></span>

## <a name="summary"></a><span data-ttu-id="edb41-133">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="edb41-133">Summary</span></span>

<span data-ttu-id="edb41-134">Ahogy látja, a .NET SDK for Hadoop hello lehetővé teszi toocreate .NET alkalmazások elküldeni a Pig feladatot tooan HDInsight-fürt, amely hello feladat állapotának figyelésére.</span><span class="sxs-lookup"><span data-stu-id="edb41-134">As you can see, hello .NET SDK for Hadoop allows you toocreate .NET applications that submit Pig jobs tooan HDInsight cluster, and monitor hello job status.</span></span>

## <a name="next-steps"></a><span data-ttu-id="edb41-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="edb41-135">Next steps</span></span>

<span data-ttu-id="edb41-136">Információk a hdinsight Pig: [a Pig használata a hdinsight Hadoop](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="edb41-136">For information on Pig in HDInsight, see [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

<span data-ttu-id="edb41-137">A HDInsight Hadoop használatával kapcsolatos további információkért tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="edb41-137">For more information on using Hadoop on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="edb41-138">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="edb41-138">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="edb41-139">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="edb41-139">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
