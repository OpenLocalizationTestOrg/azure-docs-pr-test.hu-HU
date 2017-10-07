---
title: "a webhelynapló elemzése - Azure HDInsight Hive hadooppal aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogy miként naplózza az toouse Hive HDInsight tooanalyze webhelyet. A naplófájlt használja bemenetként egy HDInsight táblába lesz, és HiveQL tooquery hello adatok felhasználásával."
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
ms.openlocfilehash: 9cbce3cc8cf8bc3ad104dc4ca6a5628802c8fe89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hive-with-windows-based-hdinsight-tooanalyze-logs-from-websites"></a><span data-ttu-id="21556-104">A Hive használata a Windows-alapú HDInsight tooanalyze naplók webhelyek</span><span class="sxs-lookup"><span data-stu-id="21556-104">Use Hive with Windows-based HDInsight tooanalyze logs from websites</span></span>
<span data-ttu-id="21556-105">Ismerje meg, hogy miként naplózza az toouse HiveQL a HDInsight tooanalyze egy webhelyről.</span><span class="sxs-lookup"><span data-stu-id="21556-105">Learn how toouse HiveQL with HDInsight tooanalyze logs from a website.</span></span> <span data-ttu-id="21556-106">A webhelynapló elemzése használt toosegment a célközönséget, hasonló tevékenységek alapján kell, a látogatók demográfiai és kimenő hello tartalom megtekinthető, hello webhelyek származnak, és egyéb toofind kategorizálását.</span><span class="sxs-lookup"><span data-stu-id="21556-106">Website log analysis can be used toosegment your audience based on similar activities, categorize site visitors by demographics, and toofind out hello content they view, hello websites they come from, and so on.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21556-107">Ez a dokumentum csak olyan feladaton végezhető Windows-alapú HDInsight-fürtökkel hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="21556-107">hello steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="21556-108">HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához.</span><span class="sxs-lookup"><span data-stu-id="21556-108">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="21556-109">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="21556-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="21556-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="21556-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="21556-111">Ez a minta egy HDInsight fürt tooanalyze webhely napló fájlok tooget betekintést hello gyakoriság látogatások toohello webhely a külső webhelyekről egy nap fogja használni.</span><span class="sxs-lookup"><span data-stu-id="21556-111">In this sample, you will use an HDInsight cluster tooanalyze website log files tooget insight into hello frequency of visits toohello website from external websites in a day.</span></span> <span data-ttu-id="21556-112">Lesz generálhat, hogy a felhasználók hello webhely hibák összegzését.</span><span class="sxs-lookup"><span data-stu-id="21556-112">You'll also generate a summary of website errors that hello users experience.</span></span> <span data-ttu-id="21556-113">Megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="21556-113">You will learn how to:</span></span>

* <span data-ttu-id="21556-114">Csatlakozás az Azure Blob Storage tárolóban, amely webhely naplófájlokat tartalmazó tooa.</span><span class="sxs-lookup"><span data-stu-id="21556-114">Connect tooa Azure Blob storage, which contains website log files.</span></span>
* <span data-ttu-id="21556-115">Hozzon létre HIVE táblák tooquery lesznek a naplók.</span><span class="sxs-lookup"><span data-stu-id="21556-115">Create HIVE tables tooquery those logs.</span></span>
* <span data-ttu-id="21556-116">HIVE-lekérdezések tooanalyze hello adatok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="21556-116">Create HIVE queries tooanalyze hello data.</span></span>
* <span data-ttu-id="21556-117">Használja a Microsoft Excel tooconnect tooHDInsight (adatbázis megnyitása (ODBC) tooretrieve elemzett hello kapcsolatadatainak használatával.</span><span class="sxs-lookup"><span data-stu-id="21556-117">Use Microsoft Excel tooconnect tooHDInsight (by using open database connectivity (ODBC) tooretrieve hello analyzed data.</span></span>

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

## <a name="prerequisites"></a><span data-ttu-id="21556-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="21556-119">Prerequisites</span></span>
* <span data-ttu-id="21556-120">Az Azure HDInsight Hadoop-fürthöz van kiépítve.</span><span class="sxs-lookup"><span data-stu-id="21556-120">You must have provisioned a Hadoop cluster on Azure HDInsight.</span></span> <span data-ttu-id="21556-121">Útmutatásért lásd: [Provision HDInsight-fürtök][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="21556-121">For instructions, see [Provision HDInsight Clusters][hdinsight-provision].</span></span>
* <span data-ttu-id="21556-122">Rendelkeznie kell Microsoft Excel 2013 vagy az Excel 2010 telepítve.</span><span class="sxs-lookup"><span data-stu-id="21556-122">You must have Microsoft Excel 2013 or Excel 2010 installed.</span></span>
* <span data-ttu-id="21556-123">Rendelkeznie kell [Microsoft Hive ODBC-illesztő](http://www.microsoft.com/download/details.aspx?id=40886) Excelbe Hive tooimport adatait.</span><span class="sxs-lookup"><span data-stu-id="21556-123">You must have [Microsoft Hive ODBC Driver](http://www.microsoft.com/download/details.aspx?id=40886) tooimport data from Hive into Excel.</span></span>

## <a name="toorun-hello-sample"></a><span data-ttu-id="21556-124">toorun hello minta</span><span class="sxs-lookup"><span data-stu-id="21556-124">toorun hello sample</span></span>
1. <span data-ttu-id="21556-125">A hello [Azure Portal](https://portal.azure.com/), a hello Kezdőpulton (ha rögzítette hello fürt nincs), kattintson a hello fürt csempe kívánja toorun hello minta.</span><span class="sxs-lookup"><span data-stu-id="21556-125">From hello [Azure Portal](https://portal.azure.com/), from hello Startboard (if you pinned hello cluster there), click hello cluster tile on which you want toorun hello sample.</span></span>
2. <span data-ttu-id="21556-126">Hello a fürt paneljén a **Gyorshivatkozások**, kattintson a **fürt irányítópult**, és majd a hello **fürt irányítópult** panelen kattintson a **HDInsight-fürt Irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="21556-126">From hello cluster blade, under **Quick Links**, click **Cluster Dashboard**, and then from hello **Cluster Dashboard** blade, click **HDInsight Cluster Dashboard**.</span></span> <span data-ttu-id="21556-127">Képes közvetlenül megnyitható hello irányítópult hello URL-cím a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="21556-127">Alternatively, you can directly open hello dashboard by using hello following URL:</span></span>

         https://<clustername>.azurehdinsight.net

    <span data-ttu-id="21556-128">Amikor a rendszer kéri, hello rendszergazdai felhasználónév és jelszó használatával a hello fürt létesítésekor használatával hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="21556-128">When prompted, authenticate by using hello administrator user name and password you used when provisioning hello cluster.</span></span>
3. <span data-ttu-id="21556-129">Weblapról hello megnyíló, kattintson a hello **Getting Started gyűjtemény** fülre, majd a hello **mintaadatokkal megoldások** kategória, hello kattintson **Webhelynapló elemzése** minta.</span><span class="sxs-lookup"><span data-stu-id="21556-129">From hello web page that opens, click hello **Getting Started Gallery** tab, and then under hello **Solutions with Sample Data** category, click hello **Website Log Analysis** sample.</span></span>
4. <span data-ttu-id="21556-130">Kövesse a hello weblap toofinish hello minta hello megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="21556-130">Follow hello instructions provided on hello web page toofinish hello sample.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21556-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="21556-131">Next steps</span></span>
<span data-ttu-id="21556-132">Próbálja meg a következő minta hello: [Érzékelőadatok elemzése a HDInsight Hive eszközzel](hdinsight-hive-analyze-sensor-data.md).</span><span class="sxs-lookup"><span data-stu-id="21556-132">Try hello following sample: [Analyzing sensor data using Hive with HDInsight](hdinsight-hive-analyze-sensor-data.md).</span></span>

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
