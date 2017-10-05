---
title: "Hibakeresés és elemzése a Hadoop-szolgáltatás a halommemória memóriaképek - Azure |} Microsoft Docs"
description: "Automatikusan Hadoop szolgáltatások halommemória memóriaképek összegyűjtése és az Azure Blob storage-fiók Hibakeresés és elemzési belül helyezhető el."
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
ms.openlocfilehash: 6d1d4d47d279eb7a1f0bf1f587445683f0ace7a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a><span data-ttu-id="59511-103">A gyűjtés halommemória kiírja a Blob Storage tárolóban hibakeresését és elemzése a Hadoop-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="59511-103">Collect heap dumps in Blob storage to debug and analyze Hadoop services</span></span>
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

<span data-ttu-id="59511-104">Halommemória memóriaképek tartalmazza az alkalmazás memória, beleértve a változók értékeit a biztonsági másolat létrehozásakor pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="59511-104">Heap dumps contain a snapshot of the application's memory, including the values of variables at the time the dump was created.</span></span> <span data-ttu-id="59511-105">Ezért futás közben felmerülő problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="59511-105">So they are useful for diagnosing problems that occur at run-time.</span></span> <span data-ttu-id="59511-106">Halommemória memóriaképek automatikusan Hadoop-szolgáltatás által gyűjtött és az Azure Blob storage-fiók alatt HDInsightHeapDumps felhasználói belül elhelyezett /.</span><span class="sxs-lookup"><span data-stu-id="59511-106">Heap dumps can be automatically collected for Hadoop services and placed inside the Azure Blob storage account of a user under HDInsightHeapDumps/.</span></span>

<span data-ttu-id="59511-107">Szolgáltatások egyes fürtökön engedélyezni kell a különböző szolgáltatások halommemória memóriaképek gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="59511-107">The collection of heap dumps for various services must be enabled for services on individual clusters.</span></span> <span data-ttu-id="59511-108">Ez a funkció alapértelmezés szerint a fürt ki lehet.</span><span class="sxs-lookup"><span data-stu-id="59511-108">The default for this feature is to be off for a cluster.</span></span> <span data-ttu-id="59511-109">Ezek halommemória memóriaképek nagy, lehet, így célszerű figyelni a Blob storage-fiók ahol azokat a rendszer mentette a gyűjtemény engedélyezése után.</span><span class="sxs-lookup"><span data-stu-id="59511-109">These heap dumps can be large, so it is advisable to monitor the Blob storage account where they are being saved once the collection has been enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="59511-110">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="59511-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="59511-111">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="59511-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="59511-112">A cikkben szereplő információk csak a Windows-alapú HDInsight vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="59511-112">The information in this article only applies to Windows-based HDInsight.</span></span> <span data-ttu-id="59511-113">Információ a Linux-alapú HDInsight: [engedélyezése halommemória a Linux-alapú HDInsight Hadoop-szolgáltatások listázása](hdinsight-hadoop-collect-debug-heap-dump-linux.md)</span><span class="sxs-lookup"><span data-stu-id="59511-113">For information on Linux-based HDInsight, see [Enable heap dumps for Hadoop services on Linux-based HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)</span></span>


## <a name="eligible-services-for-heap-dumps"></a><span data-ttu-id="59511-114">A halommemória memóriaképek jogosult szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="59511-114">Eligible services for heap dumps</span></span>
<span data-ttu-id="59511-115">A következő szolgáltatások halommemória memóriaképek engedélyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="59511-115">You can enable heap dumps for the following services:</span></span>

* <span data-ttu-id="59511-116">**hcatalog** -tempelton</span><span class="sxs-lookup"><span data-stu-id="59511-116">**hcatalog** - tempelton</span></span>
* <span data-ttu-id="59511-117">**Hive** -hiveserver2-n, metaadattárhoz, derbyserver</span><span class="sxs-lookup"><span data-stu-id="59511-117">**hive** - hiveserver2, metastore, derbyserver</span></span>
* <span data-ttu-id="59511-118">**mapreduce** -jobhistoryserver</span><span class="sxs-lookup"><span data-stu-id="59511-118">**mapreduce** - jobhistoryserver</span></span>
* <span data-ttu-id="59511-119">**yarn** -resourcemanager, nodemanager, timelineserver</span><span class="sxs-lookup"><span data-stu-id="59511-119">**yarn** - resourcemanager, nodemanager, timelineserver</span></span>
* <span data-ttu-id="59511-120">**hdfs** -datanode, secondarynamenode, namenode</span><span class="sxs-lookup"><span data-stu-id="59511-120">**hdfs** - datanode, secondarynamenode, namenode</span></span>

## <a name="configuration-elements-that-enable-heap-dumps"></a><span data-ttu-id="59511-121">Konfigurációs elemek, amelyek lehetővé teszik a halommemória memóriaképek</span><span class="sxs-lookup"><span data-stu-id="59511-121">Configuration elements that enable heap dumps</span></span>
<span data-ttu-id="59511-122">Kapcsolja be a halommemória memóriaképek egy szolgáltatás, állítsa be a megfelelő konfigurációs elemek, hogy a szolgáltatás, amely megadja a szakasz kell **service_name**.</span><span class="sxs-lookup"><span data-stu-id="59511-122">To turn on heap dumps for a service, you need to set the appropriate configuration elements in the section for that service, which is specified by **service_name**.</span></span>

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

<span data-ttu-id="59511-123">Értékének **service_name** is lehet a szolgáltatások az itt felsorolt: tempelton, hiveserver2-n metaadattárhoz, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, vagy a namenode.</span><span class="sxs-lookup"><span data-stu-id="59511-123">The value of **service_name** can be any of the services listed here: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, or namenode.</span></span>

## <a name="enable-using-azure-powershell"></a><span data-ttu-id="59511-124">Engedélyezze az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="59511-124">Enable using Azure PowerShell</span></span>
<span data-ttu-id="59511-125">Például bekapcsolása halommemória memóriaképek jobhistoryserver az Azure PowerShell használatával, használhatja a következő parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="59511-125">For example, to turn on heap dumps by using Azure PowerShell for jobhistoryserver, you can use the following script:</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a><span data-ttu-id="59511-126">Engedélyezze a .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="59511-126">Enable using .NET SDK</span></span>
<span data-ttu-id="59511-127">Például bekapcsolásához halommemória memóriaképek jobhistoryserver az Azure HDInsight .NET SDK használatával a következő kódot használhatja:</span><span class="sxs-lookup"><span data-stu-id="59511-127">For example, to turn on heap dumps by using the Azure HDInsight .NET SDK for jobhistoryserver, you can use the following code:</span></span>

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
