---
title: "Apache Pig feladatok futtatásához a .NET SDK-val a Hadoop - Azure HDInsight |} Microsoft Docs"
description: "Megtudhatja, hogyan használhatja a .NET SDK a Hadoop-elküldeni a Pig-feladatokhoz és a hdinsight Hadoop."
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
ms.openlocfilehash: e40d152821b36852c447d5a3adfd39114edbbace
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a><span data-ttu-id="bc715-103">A .NET SDK használatával a hdinsight Hadoop Pig feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="bc715-103">Run Pig jobs using the .NET SDK for Hadoop in HDInsight</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="bc715-104">Megtudhatja, hogyan használható a .NET SDK a Hadoop on Azure HDInsight Hadoop Apache Pig feladatok elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="bc715-104">Learn how to use the .NET SDK for Hadoop to submit Apache Pig jobs to Hadoop on Azure HDInsight.</span></span>

<span data-ttu-id="bc715-105">A HDInsight .NET SDK .NET ügyféloldali kódtáraknál, amely megkönnyíti a HDInsight-fürtök a .NET-együttműködve biztosítja.</span><span class="sxs-lookup"><span data-stu-id="bc715-105">The HDInsight .NET SDK provides .NET client libraries that makes it easier to work with HDInsight clusters from .NET.</span></span> <span data-ttu-id="bc715-106">A Pig hozhat létre MapReduce műveletek modellezési adatátalakítást sorozata alapján.</span><span class="sxs-lookup"><span data-stu-id="bc715-106">Pig allows you to create MapReduce operations by modeling a series of data transformations.</span></span> <span data-ttu-id="bc715-107">Ebből a dokumentumból megismerheti, hogyan elküldeni a Pig feladatot egy HDInsight-fürthöz egy alapszintű C#-alkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="bc715-107">In this document, you learn how to use a basic C# application to submit a Pig job to an HDInsight cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc715-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bc715-108">Prerequisites</span></span>

<span data-ttu-id="bc715-109">A cikkben leírt lépéseket a következőkre lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="bc715-109">To complete the steps in this article, you need the following.</span></span>

* <span data-ttu-id="bc715-110">(A HDInsight Hadoop) Azure HDInsight-fürtök (vagy a Windows vagy Linux-alapú).</span><span class="sxs-lookup"><span data-stu-id="bc715-110">An Azure HDInsight (Hadoop on HDInsight) cluster (either Windows or Linux-based).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="bc715-111">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="bc715-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="bc715-112">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="bc715-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="bc715-113">A Visual Studio 2012, 2013, 2015-öt vagy 2017.</span><span class="sxs-lookup"><span data-stu-id="bc715-113">Visual Studio 2012, 2013, 2015 or 2017.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="bc715-114">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc715-114">Create the application</span></span>

<span data-ttu-id="bc715-115">A HDInsight .NET SDK biztosít a .NET ügyféloldali kódtáraknál, így azokat könnyebben működéséhez a .NET-HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="bc715-115">The HDInsight .NET SDK provides .NET client libraries, which makes it easier to work with HDInsight clusters from .NET.</span></span>

1. <span data-ttu-id="bc715-116">Az a **fájl** elemét a Visual Studióban, válassza ki **új** majd **projekt**.</span><span class="sxs-lookup"><span data-stu-id="bc715-116">From the **File** menu in Visual Studio, select **New** and then select **Project**.</span></span>

2. <span data-ttu-id="bc715-117">Az új projektbe futnak írja be vagy válassza ki a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="bc715-117">For the new project, type or select the following values:</span></span>

   | <span data-ttu-id="bc715-118">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bc715-118">Property</span></span> | <span data-ttu-id="bc715-119">Érték</span><span class="sxs-lookup"><span data-stu-id="bc715-119">Value</span></span> |
   | ------ | ------ |
   | <span data-ttu-id="bc715-120">Kategória</span><span class="sxs-lookup"><span data-stu-id="bc715-120">Category</span></span> | <span data-ttu-id="bc715-121">Sablonok/Visual C#/Windows</span><span class="sxs-lookup"><span data-stu-id="bc715-121">Templates/Visual C#/Windows</span></span> |
   | <span data-ttu-id="bc715-122">Sablon</span><span class="sxs-lookup"><span data-stu-id="bc715-122">Template</span></span> | <span data-ttu-id="bc715-123">Konzolalkalmazás</span><span class="sxs-lookup"><span data-stu-id="bc715-123">Console Application</span></span> |
   | <span data-ttu-id="bc715-124">Név</span><span class="sxs-lookup"><span data-stu-id="bc715-124">Name</span></span> | <span data-ttu-id="bc715-125">SubmitPigJob</span><span class="sxs-lookup"><span data-stu-id="bc715-125">SubmitPigJob</span></span> |

3. <span data-ttu-id="bc715-126">A projekt létrehozásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc715-126">Click **OK** to create the project.</span></span>

4. <span data-ttu-id="bc715-127">Az a **eszközök** menüjében válassza **Kódtárcsomag-kezelő** vagy **Nuget-Csomagkezelő**, majd válassza ki **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="bc715-127">From the **Tools** menu, select **Library Package Manager** or **Nuget Package Manager**, and then select **Package Manager Console**.</span></span>

5. <span data-ttu-id="bc715-128">A .NET SDK-csomagok telepítéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="bc715-128">To install the .NET SDK packages, use the following command:</span></span>

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. <span data-ttu-id="bc715-129">A Megoldáskezelőben kattintson duplán a **Program.cs** való megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="bc715-129">From Solution Explorer, double-click **Program.cs** to open it.</span></span> <span data-ttu-id="bc715-130">Cserélje le a meglévő kódot a következő.</span><span class="sxs-lookup"><span data-stu-id="bc715-130">Replace the existing code with the following.</span></span>

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
                System.Console.WriteLine("The application is running ...");

                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                SubmitPigJob();

                System.Console.WriteLine("Press ENTER to continue ...");
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

                System.Console.WriteLine("Submitting the Pig job to the cluster...");
                var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                System.Console.WriteLine("Validating that the response is as expected...");
                System.Console.WriteLine("Response status code is " + response.StatusCode);
                System.Console.WriteLine("Validating the response object...");
                System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
            }
        }
    }
    ```

7. <span data-ttu-id="bc715-131">Az alkalmazás elindításához nyomja le az **F5**.</span><span class="sxs-lookup"><span data-stu-id="bc715-131">To start the application, press **F5**.</span></span>

8. <span data-ttu-id="bc715-132">Kilép az alkalmazásból, nyomja le az **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="bc715-132">To exit the application, press **ENTER**.</span></span>

## <a name="summary"></a><span data-ttu-id="bc715-133">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="bc715-133">Summary</span></span>

<span data-ttu-id="bc715-134">Ahogy látja, a .NET SDK a Hadoop lehetővé teszi egy HDInsight-fürt Pig feladatok elküldéséhez .NET-alkalmazások létrehozása, és figyelheti a feladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="bc715-134">As you can see, the .NET SDK for Hadoop allows you to create .NET applications that submit Pig jobs to an HDInsight cluster, and monitor the job status.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc715-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc715-135">Next steps</span></span>

<span data-ttu-id="bc715-136">Információk a hdinsight Pig: [a Pig használata a hdinsight Hadoop](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="bc715-136">For information on Pig in HDInsight, see [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

<span data-ttu-id="bc715-137">A HDInsight Hadoop használatával kapcsolatos további információkért lásd a következő dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="bc715-137">For more information on using Hadoop on HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="bc715-138">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="bc715-138">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="bc715-139">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="bc715-139">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
