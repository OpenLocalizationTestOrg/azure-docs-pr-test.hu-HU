---
title: "aaaInstall vagy a HDInsight - Azure monó frissítése |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse HDInsight-fürthöz monó egy adott verziójához. Monó a .NET-alkalmazásokban használt toorun a Linux-alapú HDInsight-fürtökön."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: hdinsightactive
ms.openlocfilehash: 1e8a8aaeff231c93a9e232379448517b326da965
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-or-update-mono-on-hdinsight"></a><span data-ttu-id="c2a82-104">Telepítse vagy frissítse a HDInsight monó</span><span class="sxs-lookup"><span data-stu-id="c2a82-104">Install or update Mono on HDInsight</span></span>

<span data-ttu-id="c2a82-105">Megtudhatja, hogyan tooinstall egy adott verziójához a [monó](https://www.mono-project.com) HDInsight 3.4-es vagy újabb rendszerre.</span><span class="sxs-lookup"><span data-stu-id="c2a82-105">Learn how tooinstall a specific version of [Mono](https://www.mono-project.com) on HDInsight 3.4 or higher.</span></span>

<span data-ttu-id="c2a82-106">Monó HDInsight 3.4-es és újabb rendszer van telepítve, és használt toorun .NET-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c2a82-106">Mono is installed on HDInsight 3.4 and higher, and is used toorun .NET applications.</span></span> <span data-ttu-id="c2a82-107">Monó részét képező egyes HDInsight-verzió hello verzióján információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="c2a82-107">For information on hello version of Mono included with each HDInsight version, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="c2a82-108">egy másik verziót a fürtben, ez a dokumentum használatát hello parancsfájlművelet tooinstall.</span><span class="sxs-lookup"><span data-stu-id="c2a82-108">tooinstall a different version on your cluster, use hello script action in this document.</span></span> 

## <a name="how-it-works"></a><span data-ttu-id="c2a82-109">Működés</span><span class="sxs-lookup"><span data-stu-id="c2a82-109">How it works</span></span>

<span data-ttu-id="c2a82-110">Ez a parancsfájl fogadja el a következő paraméter hello:</span><span class="sxs-lookup"><span data-stu-id="c2a82-110">This script accepts hello following parameter:</span></span>

* <span data-ttu-id="c2a82-111">__Monó verziószáma__: monó tooinstall hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="c2a82-111">__Mono version number__: hello version of Mono tooinstall.</span></span> <span data-ttu-id="c2a82-112">hello verzió elérhetőnek kell lennie [https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).</span><span class="sxs-lookup"><span data-stu-id="c2a82-112">hello version must be available from [https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).</span></span>

<span data-ttu-id="c2a82-113">hello parancsfájl a következő monó csomagok hello telepíti:</span><span class="sxs-lookup"><span data-stu-id="c2a82-113">hello script installs hello following Mono packages:</span></span>

* <span data-ttu-id="c2a82-114">__Monó befejezése__</span><span class="sxs-lookup"><span data-stu-id="c2a82-114">__mono-complete__</span></span>

* <span data-ttu-id="c2a82-115">__CA-tanúsítványok-monó__</span><span class="sxs-lookup"><span data-stu-id="c2a82-115">__ca-certificates-mono__</span></span>

## <a name="hello-script"></a><span data-ttu-id="c2a82-116">hello parancsfájl</span><span class="sxs-lookup"><span data-stu-id="c2a82-116">hello script</span></span>

<span data-ttu-id="c2a82-117">__Parancsfájl-hely__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)</span><span class="sxs-lookup"><span data-stu-id="c2a82-117">__Script location__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)</span></span>

<span data-ttu-id="c2a82-118">__Követelmények__:</span><span class="sxs-lookup"><span data-stu-id="c2a82-118">__Requirements__:</span></span>

* <span data-ttu-id="c2a82-119">hello parancsfájl kell alkalmazni a hello __átjárócsomópontokat__ és __munkavégző csomópontokhoz__.</span><span class="sxs-lookup"><span data-stu-id="c2a82-119">hello script must be applied on hello __head nodes__ and __worker nodes__.</span></span>

## <a name="toouse-hello-script"></a><span data-ttu-id="c2a82-120">toouse hello parancsfájl</span><span class="sxs-lookup"><span data-stu-id="c2a82-120">toouse hello script</span></span>

<span data-ttu-id="c2a82-121">Információk hogyan toouse ezt a parancsfájlt a hdinsight eszközzel, lásd: hello [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="c2a82-121">For information on how toouse this script with HDInsight, see hello [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) document.</span></span> <span data-ttu-id="c2a82-122">Hello parancsfájllal hello Azure-portálon az Azure PowerShell használatával, vagy az Azure parancssori felület hello.</span><span class="sxs-lookup"><span data-stu-id="c2a82-122">You can use hello script through hello Azure portal, Azure PowerShell, or hello Azure CLI.</span></span>

<span data-ttu-id="c2a82-123">Közben a következő parancsfájl műveleti dokumentum hello, használja a következő URI hello:</span><span class="sxs-lookup"><span data-stu-id="c2a82-123">While following hello script action document, use hello following URI:</span></span>

    https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash

> [!NOTE]
> <span data-ttu-id="c2a82-124">Ezt a parancsfájlt a HDInsight konfigurálásakor jelölje meg hello parancsprogramot __megőrzött__.</span><span class="sxs-lookup"><span data-stu-id="c2a82-124">When configuring HDInsight with this script, mark hello script as __Persisted__.</span></span> <span data-ttu-id="c2a82-125">Ez a beállítás lehetővé teszi, hogy a HDInsight tooapply hello parancsfájl tooworker csomópontok műveletek a méretezés során hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="c2a82-125">This setting allows HDInsight tooapply hello script tooworker nodes added through scaling operations.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c2a82-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2a82-126">Next steps</span></span>

<span data-ttu-id="c2a82-127">Megtanulta, hogyan tooupgrade vagy monó adott verziójának telepítése a hdinsight platformon.</span><span class="sxs-lookup"><span data-stu-id="c2a82-127">You have learned how tooupgrade or install a specific version of Mono on HDInsight.</span></span> <span data-ttu-id="c2a82-128">A .NET-alkalmazások használata a HDInsight monó további információkért tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="c2a82-128">For more information on using .NET applications with Mono on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="c2a82-129">Az adatfolyamként történő MapReduce a HDInsight .NET használata</span><span class="sxs-lookup"><span data-stu-id="c2a82-129">Use .NET for streaming MapReduce on HDInsight</span></span>](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [<span data-ttu-id="c2a82-130">A Hive és a Pig a HDInsight .NET használata</span><span class="sxs-lookup"><span data-stu-id="c2a82-130">Use .NET with Hive and Pig on HDInsight</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)
* [<span data-ttu-id="c2a82-131">A HDInsight alatt futó Storm a C# megoldások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="c2a82-131">Develop C# solutions with Storm on HDInsight</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="c2a82-132">Telepítse át a .NET-megoldások tooLinux-alapú HDInsight</span><span class="sxs-lookup"><span data-stu-id="c2a82-132">Migrate .NET solutions tooLinux-based HDInsight</span></span>](hdinsight-hadoop-migrate-dotnet-to-linux.md)

<span data-ttu-id="c2a82-133">Parancsfájlműveletek használatával kapcsolatos további információkért lásd: [testreszabása Linux-alapú HDInsight-fürtök parancsfájlművelet használatával](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="c2a82-133">For more information on using script actions, see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>