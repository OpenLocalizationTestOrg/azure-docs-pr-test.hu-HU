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
# <a name="collect-heap-dumps-in-blob-storage-toodebug-and-analyze-hadoop-services"></a>A Blob storage toodebug halommemória memóriaképek gyűjtése és elemzése a Hadoop-szolgáltatás
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Halommemória memóriaképek tartalmazhat hello alkalmazás memória, beleértve a változók értékeinek hello hello időpontban hello memóriakép létrehozása egy pillanatkép. Ezért futás közben felmerülő problémák diagnosztizálásához. Halommemória memóriaképek automatikusan Hadoop-szolgáltatás által gyűjtött és hello Azure Blob storage-fiók alatt HDInsightHeapDumps felhasználói belül elhelyezett /.

egyes fürtökön szolgáltatások engedélyezni kell a különböző szolgáltatások halommemória memóriaképek hello gyűjteménye. hello Ez a funkció alapértelmezés szerint toobe ki a fürthöz. Ezek halommemória memóriaképek nagy, lehet, így ajánlott toomonitor hello Blob storage-fiók is, ahol azok menti hello gyűjtemény engedélyezése után.

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement). a cikkben szereplő információkat hello csak érvényes tooWindows-alapú HDInsight. Információ a Linux-alapú HDInsight: [engedélyezése halommemória a Linux-alapú HDInsight Hadoop-szolgáltatások listázása](hdinsight-hadoop-collect-debug-heap-dump-linux.md)


## <a name="eligible-services-for-heap-dumps"></a>A halommemória memóriaképek jogosult szolgáltatások
A következő szolgáltatások hello halommemória memóriaképek engedélyezéséhez:

* **hcatalog** -tempelton
* **Hive** -hiveserver2-n, metaadattárhoz, derbyserver
* **mapreduce** -jobhistoryserver
* **yarn** -resourcemanager, nodemanager, timelineserver
* **hdfs** -datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Konfigurációs elemek, amelyek lehetővé teszik a halommemória memóriaképek
a halommemória memóriaképek szolgáltatás tooturn, kell tooset hello megfelelő konfigurációs elemek hello szakaszban, hogy a szolgáltatás által megadott **service_name**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

hello értékének **service_name** az itt felsorolt hello szolgáltatások bármelyike lehet: tempelton, hiveserver2-n metaadattárhoz, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, vagy namenode.

## <a name="enable-using-azure-powershell"></a>Engedélyezze az Azure PowerShell használatával
Például a halommemória memóriaképek jobhistoryserver az Azure PowerShell használatával tooturn, használhatja a következő parancsfájl hello:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Engedélyezze a .NET SDK használatával
Például a halommemória memóriaképek jobhistoryserver hello Azure HDInsight .NET SDK használatával tooturn, használhatja a következő kód hello:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
