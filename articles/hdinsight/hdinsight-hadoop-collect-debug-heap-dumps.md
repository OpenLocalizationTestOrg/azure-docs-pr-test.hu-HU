---
title: "aaaDebug és elemzése a Hadoop-szolgáltatás a halommemória memóriaképek - Azure |} Microsoft Docs"
description: "Automatikusan Hadoop szolgáltatások halommemória memóriaképek összegyűjtése és hello Azure Blob storage-fiók Hibakeresés és elemzési belül helyezhető el."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: e4ec4ebb-fd32-4668-8382-f956581485c4
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 70fbc2d6d97d35b0d7b1d9149673b02ae1878eb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collect-heap-dumps-in-blob-storage-toodebug-and-analyze-hadoop-services"></a><span data-ttu-id="3ab37-103">A Blob storage toodebug halommemória memóriaképek gyűjtése és elemzése a Hadoop-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="3ab37-103">Collect heap dumps in Blob storage toodebug and analyze Hadoop services</span></span>
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

<span data-ttu-id="3ab37-104">Halommemória memóriaképek tartalmazhat hello alkalmazás memória, beleértve a változók értékeinek hello hello időpontban hello memóriakép létrehozása egy pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="3ab37-104">Heap dumps contain a snapshot of hello application's memory, including hello values of variables at hello time hello dump was created.</span></span> <span data-ttu-id="3ab37-105">Ezért futás közben felmerülő problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="3ab37-105">So they are useful for diagnosing problems that occur at run-time.</span></span> <span data-ttu-id="3ab37-106">Halommemória memóriaképek automatikusan Hadoop-szolgáltatás által gyűjtött és hello Azure Blob storage-fiók alatt HDInsightHeapDumps felhasználói belül elhelyezett /.</span><span class="sxs-lookup"><span data-stu-id="3ab37-106">Heap dumps can be automatically collected for Hadoop services and placed inside hello Azure Blob storage account of a user under HDInsightHeapDumps/.</span></span>

<span data-ttu-id="3ab37-107">egyes fürtökön szolgáltatások engedélyezni kell a különböző szolgáltatások halommemória memóriaképek hello gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="3ab37-107">hello collection of heap dumps for various services must be enabled for services on individual clusters.</span></span> <span data-ttu-id="3ab37-108">hello Ez a funkció alapértelmezés szerint toobe ki a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="3ab37-108">hello default for this feature is toobe off for a cluster.</span></span> <span data-ttu-id="3ab37-109">Ezek halommemória memóriaképek nagy, lehet, így ajánlott toomonitor hello Blob storage-fiók is, ahol azok menti hello gyűjtemény engedélyezése után.</span><span class="sxs-lookup"><span data-stu-id="3ab37-109">These heap dumps can be large, so it is advisable toomonitor hello Blob storage account where they are being saved once hello collection has been enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3ab37-110">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="3ab37-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3ab37-111">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="3ab37-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="3ab37-112">a cikkben szereplő információkat hello csak érvényes tooWindows-alapú HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3ab37-112">hello information in this article only applies tooWindows-based HDInsight.</span></span> <span data-ttu-id="3ab37-113">Információ a Linux-alapú HDInsight: [engedélyezése halommemória a Linux-alapú HDInsight Hadoop-szolgáltatások listázása](hdinsight-hadoop-collect-debug-heap-dump-linux.md)</span><span class="sxs-lookup"><span data-stu-id="3ab37-113">For information on Linux-based HDInsight, see [Enable heap dumps for Hadoop services on Linux-based HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)</span></span>


## <a name="eligible-services-for-heap-dumps"></a><span data-ttu-id="3ab37-114">A halommemória memóriaképek jogosult szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="3ab37-114">Eligible services for heap dumps</span></span>
<span data-ttu-id="3ab37-115">A következő szolgáltatások hello halommemória memóriaképek engedélyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="3ab37-115">You can enable heap dumps for hello following services:</span></span>

* <span data-ttu-id="3ab37-116">**hcatalog** -tempelton</span><span class="sxs-lookup"><span data-stu-id="3ab37-116">**hcatalog** - tempelton</span></span>
* <span data-ttu-id="3ab37-117">**Hive** -hiveserver2-n, metaadattárhoz, derbyserver</span><span class="sxs-lookup"><span data-stu-id="3ab37-117">**hive** - hiveserver2, metastore, derbyserver</span></span>
* <span data-ttu-id="3ab37-118">**mapreduce** -jobhistoryserver</span><span class="sxs-lookup"><span data-stu-id="3ab37-118">**mapreduce** - jobhistoryserver</span></span>
* <span data-ttu-id="3ab37-119">**yarn** -resourcemanager, nodemanager, timelineserver</span><span class="sxs-lookup"><span data-stu-id="3ab37-119">**yarn** - resourcemanager, nodemanager, timelineserver</span></span>
* <span data-ttu-id="3ab37-120">**hdfs** -datanode, secondarynamenode, namenode</span><span class="sxs-lookup"><span data-stu-id="3ab37-120">**hdfs** - datanode, secondarynamenode, namenode</span></span>

## <a name="configuration-elements-that-enable-heap-dumps"></a><span data-ttu-id="3ab37-121">Konfigurációs elemek, amelyek lehetővé teszik a halommemória memóriaképek</span><span class="sxs-lookup"><span data-stu-id="3ab37-121">Configuration elements that enable heap dumps</span></span>
<span data-ttu-id="3ab37-122">a halommemória memóriaképek szolgáltatás tooturn, kell tooset hello megfelelő konfigurációs elemek hello szakaszban, hogy a szolgáltatás által megadott **service_name**.</span><span class="sxs-lookup"><span data-stu-id="3ab37-122">tooturn on heap dumps for a service, you need tooset hello appropriate configuration elements in hello section for that service, which is specified by **service_name**.</span></span>

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

<span data-ttu-id="3ab37-123">hello értékének **service_name** az itt felsorolt hello szolgáltatások bármelyike lehet: tempelton, hiveserver2-n metaadattárhoz, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, vagy namenode.</span><span class="sxs-lookup"><span data-stu-id="3ab37-123">hello value of **service_name** can be any of hello services listed here: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, or namenode.</span></span>

## <a name="enable-using-azure-powershell"></a><span data-ttu-id="3ab37-124">Engedélyezze az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="3ab37-124">Enable using Azure PowerShell</span></span>
<span data-ttu-id="3ab37-125">Például a halommemória memóriaképek jobhistoryserver az Azure PowerShell használatával tooturn, használhatja a következő parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="3ab37-125">For example, tooturn on heap dumps by using Azure PowerShell for jobhistoryserver, you can use hello following script:</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a><span data-ttu-id="3ab37-126">Engedélyezze a .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="3ab37-126">Enable using .NET SDK</span></span>
<span data-ttu-id="3ab37-127">Például a halommemória memóriaképek jobhistoryserver hello Azure HDInsight .NET SDK használatával tooturn, használhatja a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="3ab37-127">For example, tooturn on heap dumps by using hello Azure HDInsight .NET SDK for jobhistoryserver, you can use hello following code:</span></span>

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
