---
title: "A Hive használata a hadooppal a webhelynapló elemzése - Azure HDInsight |} Microsoft Docs"
description: "Útmutató a Hive használata a Hdinsightban elemezhet. Egy naplófájlt használja bemenetként egy HDInsight táblába lesz, és a lekérdezést a HiveQL használatával."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6fb7b5c2-8df4-40b1-a9e2-6815080004f9
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: e1cdb786bb6049980aafc0213abf53013e342618
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-hive-with-windows-based-hdinsight-to-analyze-logs-from-websites"></a><span data-ttu-id="f6d30-104">A Hive használata a Windows-alapú hdinsight eszközzel webhelyek naplóinak elemzése</span><span class="sxs-lookup"><span data-stu-id="f6d30-104">Use Hive with Windows-based HDInsight to analyze logs from websites</span></span>
<span data-ttu-id="f6d30-105">Útmutató a HDInsight HiveQL ezen webhelyek naplóinak elemzése.</span><span class="sxs-lookup"><span data-stu-id="f6d30-105">Learn how to use HiveQL with HDInsight to analyze logs from a website.</span></span> <span data-ttu-id="f6d30-106">A webhelynapló elemzése is használható, szegmentálhatja a célközönséget, hasonló tevékenységek alapján, a látogatók demográfiai szerinti kategóriák, valamint tudja meg a tartalom azokat nézet, a webhelyek származnak, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="f6d30-106">Website log analysis can be used to segment your audience based on similar activities, categorize site visitors by demographics, and to find out the content they view, the websites they come from, and so on.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f6d30-107">A jelen dokumentumban leírt lépések csak a Windows-alapú HDInsight-fürtök dolgozhat.</span><span class="sxs-lookup"><span data-stu-id="f6d30-107">The steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="f6d30-108">HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához.</span><span class="sxs-lookup"><span data-stu-id="f6d30-108">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="f6d30-109">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="f6d30-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f6d30-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f6d30-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="f6d30-111">Ez a példa a HDInsight-fürtök használandó webhelynaplókat látogatások gyakoriságáról a webhelyet a külső webhelyekről egy nap webhely naplófájlok elemzése.</span><span class="sxs-lookup"><span data-stu-id="f6d30-111">In this sample, you will use an HDInsight cluster to analyze website log files to get insight into the frequency of visits to the website from external websites in a day.</span></span> <span data-ttu-id="f6d30-112">Lesz generálhat, hogy a felhasználók webhely hibák összegzését.</span><span class="sxs-lookup"><span data-stu-id="f6d30-112">You'll also generate a summary of website errors that the users experience.</span></span> <span data-ttu-id="f6d30-113">Megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="f6d30-113">You will learn how to:</span></span>

* <span data-ttu-id="f6d30-114">Egy Azure Blob Storage tárolóban, amely webhely naplófájlokat tartalmazó csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="f6d30-114">Connect to a Azure Blob storage, which contains website log files.</span></span>
* <span data-ttu-id="f6d30-115">Lesznek a naplók lekérdezni HIVE táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f6d30-115">Create HIVE tables to query those logs.</span></span>
* <span data-ttu-id="f6d30-116">Az adatok elemzése a HIVE-lekérdezések létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f6d30-116">Create HIVE queries to analyze the data.</span></span>
* <span data-ttu-id="f6d30-117">A Microsoft Excelben HDInsight csatlakozás (elemzett adatokat (ODBC). Nyissa meg az adatbázis-kapcsolat használatával.</span><span class="sxs-lookup"><span data-stu-id="f6d30-117">Use Microsoft Excel to connect to HDInsight (by using open database connectivity (ODBC) to retrieve the analyzed data.</span></span>

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

## <a name="prerequisites"></a><span data-ttu-id="f6d30-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f6d30-119">Prerequisites</span></span>
* <span data-ttu-id="f6d30-120">Az Azure HDInsight Hadoop-fürthöz van kiépítve.</span><span class="sxs-lookup"><span data-stu-id="f6d30-120">You must have provisioned a Hadoop cluster on Azure HDInsight.</span></span> <span data-ttu-id="f6d30-121">Útmutatásért lásd: [Provision HDInsight-fürtök][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="f6d30-121">For instructions, see [Provision HDInsight Clusters][hdinsight-provision].</span></span>
* <span data-ttu-id="f6d30-122">Rendelkeznie kell Microsoft Excel 2013 vagy az Excel 2010 telepítve.</span><span class="sxs-lookup"><span data-stu-id="f6d30-122">You must have Microsoft Excel 2013 or Excel 2010 installed.</span></span>
* <span data-ttu-id="f6d30-123">Rendelkeznie kell [Microsoft Hive ODBC-illesztő](http://www.microsoft.com/download/details.aspx?id=40886) Hive adatok importálása Excelbe.</span><span class="sxs-lookup"><span data-stu-id="f6d30-123">You must have [Microsoft Hive ODBC Driver](http://www.microsoft.com/download/details.aspx?id=40886) to import data from Hive into Excel.</span></span>

## <a name="to-run-the-sample"></a><span data-ttu-id="f6d30-124">A minta futtatásához</span><span class="sxs-lookup"><span data-stu-id="f6d30-124">To run the sample</span></span>
1. <span data-ttu-id="f6d30-125">Az a [Azure Portal](https://portal.azure.com/), a kezdőpultról (ha rögzítette a fürt nincs), kattintson a fürt csempéjére, amelyeken a minta futtatásához kívánja.</span><span class="sxs-lookup"><span data-stu-id="f6d30-125">From the [Azure Portal](https://portal.azure.com/), from the Startboard (if you pinned the cluster there), click the cluster tile on which you want to run the sample.</span></span>
2. <span data-ttu-id="f6d30-126">A fürt paneljén alatt **Gyorshivatkozások**, kattintson a **fürt irányítópult**, majd a **fürt irányítópult** panelen kattintson a **HDInsight-fürt Irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="f6d30-126">From the cluster blade, under **Quick Links**, click **Cluster Dashboard**, and then from the **Cluster Dashboard** blade, click **HDInsight Cluster Dashboard**.</span></span> <span data-ttu-id="f6d30-127">Másik megoldásként közvetlenül megnyitásához az irányítópulton a következő URL-cím segítségével:</span><span class="sxs-lookup"><span data-stu-id="f6d30-127">Alternatively, you can directly open the dashboard by using the following URL:</span></span>

         https://<clustername>.azurehdinsight.net

    <span data-ttu-id="f6d30-128">Amikor a rendszer kéri, a rendszergazdai felhasználónév és jelszó használatával a fürt létesítésekor használatával hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="f6d30-128">When prompted, authenticate by using the administrator user name and password you used when provisioning the cluster.</span></span>
3. <span data-ttu-id="f6d30-129">A megnyíló weblapon, kattintson a **Getting Started gyűjteménye** fülre, majd a a **mintaadatokkal megoldások** kategória, kattintson a **Webhelynapló elemzése** minta.</span><span class="sxs-lookup"><span data-stu-id="f6d30-129">From the web page that opens, click the **Getting Started Gallery** tab, and then under the **Solutions with Sample Data** category, click the **Website Log Analysis** sample.</span></span>
4. <span data-ttu-id="f6d30-130">Kövesse a megjelenő utasításokat a weblap a minta befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="f6d30-130">Follow the instructions provided on the web page to finish the sample.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f6d30-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6d30-131">Next steps</span></span>
<span data-ttu-id="f6d30-132">Próbálja meg a következő mintát: [Érzékelőadatok elemzése a HDInsight Hive eszközzel](hdinsight-hive-analyze-sensor-data.md).</span><span class="sxs-lookup"><span data-stu-id="f6d30-132">Try the following sample: [Analyzing sensor data using Hive with HDInsight](hdinsight-hive-analyze-sensor-data.md).</span></span>

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
