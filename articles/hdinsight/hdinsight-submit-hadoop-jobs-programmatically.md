---
title: "Hdinsight Hadoop feladatok elküldéséhez |} Microsoft Docs"
description: "Útmutató: Azure HDInsight Hadoop Hadoop feladatok elküldéséhez."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 50430b96-2329-4775-9713-19c5795b775f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 6829ff82afc7fcea9e027ad14ec7ed0c8015a5fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="submit-hadoop-jobs-in-hdinsight"></a><span data-ttu-id="62b48-103">Hadoop-feladatok elküldése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="62b48-103">Submit Hadoop jobs in HDInsight</span></span>

<span data-ttu-id="62b48-104">Elküldheti a Hadoop-feladatokat .NET SDK-val, a Curl és az Azure PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="62b48-104">You can submit Hadoop jobs using .NET SDK, Curl, and Azure PowerShell:</span></span>

- <span data-ttu-id="62b48-105">A .NET SDK használata</span><span class="sxs-lookup"><span data-stu-id="62b48-105">Use .NET SDK</span></span>

  - [<span data-ttu-id="62b48-106">.NET-alkalmazások létrehozása a nem interaktív hitelesítés</span><span class="sxs-lookup"><span data-stu-id="62b48-106">Create non-interactive authentication .NET applications</span></span>](hdinsight-create-non-interactive-authentication-dotnet-applications.md)
  - [<span data-ttu-id="62b48-107">A HDInsight .NET SDK használatával Hive-lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="62b48-107">Run Hive queries using HDInsight .NET SDK</span></span>](hdinsight-hadoop-use-hive-dotnet-sdk.md)
  - [<span data-ttu-id="62b48-108">A .NET SDK használatával a hdinsight Hadoop Pig feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="62b48-108">Run Pig jobs using the .NET SDK for Hadoop in HDInsight</span></span>](hdinsight-hadoop-use-pig-dotnet-sdk.md)
  - [<span data-ttu-id="62b48-109">A hdinsight Hadoop .NET SDK használatával Sqoop feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="62b48-109">Run Sqoop jobs using .NET SDK for Hadoop in HDInsight</span></span>](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)
  - [<span data-ttu-id="62b48-110">A HDInsight .NET SDK használatával MapReduce-feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="62b48-110">Run MapReduce jobs using HDInsight .NET SDK</span></span>](hdinsight-hadoop-use-mapreduce-dotnet-sdk.md)

- <span data-ttu-id="62b48-111">CURL</span><span class="sxs-lookup"><span data-stu-id="62b48-111">CURL</span></span>

  - [<span data-ttu-id="62b48-112">A Curl a HDInsight Hadoop Hive-lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="62b48-112">Run Hive queries with Hadoop in HDInsight with Curl</span></span>](hdinsight-hadoop-use-hive-curl.md)
  - [<span data-ttu-id="62b48-113">Pig feladatok futtatása a Hadoop on HDInsight Curl használatával</span><span class="sxs-lookup"><span data-stu-id="62b48-113">Run Pig jobs with Hadoop on HDInsight by using Curl</span></span>](hdinsight-hadoop-use-pig-curl.md)
  - [<span data-ttu-id="62b48-114">Sqoop feladatok futtatása a hadooppal a Hdinsightban a Curl</span><span class="sxs-lookup"><span data-stu-id="62b48-114">Run Sqoop jobs with Hadoop in HDInsight with Curl</span></span>](hdinsight-hadoop-use-sqoop-curl.md)
  - [<span data-ttu-id="62b48-115">Hadoop MapReduce-feladatok a HDInsight használata Curl használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="62b48-115">Run MapReduce jobs with Hadoop on HDInsight using Curl</span></span>](hdinsight-hadoop-use-mapreduce-curl.md)

- <span data-ttu-id="62b48-116">PowerShell</span><span class="sxs-lookup"><span data-stu-id="62b48-116">PowerShell</span></span>

  - [<span data-ttu-id="62b48-117">PowerShell-lel Hive-lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="62b48-117">Run Hive queries using PowerShell</span></span>](hdinsight-hadoop-use-hive-powershell.md)
  - [<span data-ttu-id="62b48-118">Futtassa a PowerShell használatával Pig-feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="62b48-118">Run Pig jobs using PowerShell</span></span>](hdinsight-hadoop-use-pig-powershell.md)
  - [<span data-ttu-id="62b48-119">Sqoop használata a hadooppal a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="62b48-119">Use Sqoop with Hadoop in HDInsight</span></span>](hdinsight-hadoop-use-sqoop-powershell.md)
  - [<span data-ttu-id="62b48-120">A PowerShell használatával HDInsight Hadoop MapReduce-feladatok futtassa</span><span class="sxs-lookup"><span data-stu-id="62b48-120">Run MapReduce jobs with Hadoop on HDInsight using PowerShell</span></span>](hdinsight-hadoop-use-mapreduce-powershell.md)

## <a name="see-also"></a><span data-ttu-id="62b48-121">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="62b48-121">See also</span></span>

- [<span data-ttu-id="62b48-122">Az Azure HDInsight-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="62b48-122">Azure HDInsight Documentation</span></span>](https://docs.microsoft.com/azure/hdinsight/)