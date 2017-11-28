---
title: "aaaCreate Hadoop-fürtök PowerShell - Azure HDInsight használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate Hadoop, HBase, Storm vagy Spark-fürtök Linux rendszeren a HDInsight Azure PowerShell használatával."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4208deca-d64a-45e1-8948-2673d5d7678c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 53afe4702f6b61a0720ceda48a4a34d7fa8797d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a><span data-ttu-id="56d95-103">Linux-alapú fürtök létrehozása a Hdinsightban Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="56d95-103">Create Linux-based clusters in HDInsight using Azure PowerShell</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="56d95-104">Az Azure PowerShell egy hatékony parancsfájl-kezelési környezet, hogy toocontrol használ, és hello telepítése és kezelése a Microsoft Azure-ban a feladatok automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="56d95-104">Azure PowerShell is a powerful scripting environment that you can use toocontrol and automate hello deployment and management of your workloads in Microsoft Azure.</span></span> <span data-ttu-id="56d95-105">Ez a dokumentum bemutatja, hogyan toocreate egy Linux-alapú HDInsight fürt Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="56d95-105">This document provides information about how toocreate a Linux-based HDInsight cluster by using Azure PowerShell.</span></span> <span data-ttu-id="56d95-106">Példa parancsfájl is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="56d95-106">It also includes an example script.</span></span>

> [!NOTE]
> <span data-ttu-id="56d95-107">Az Azure PowerShell a Windows-ügyfelek csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="56d95-107">Azure PowerShell is only available on Windows clients.</span></span> <span data-ttu-id="56d95-108">Ha a Linux, Unix vagy Mac OS X-ügyfelet kell használnia, lásd: [hozzon létre egy Linux-alapú HDInsight-fürt Azure parancssori felület használatával](hdinsight-hadoop-create-linux-clusters-azure-cli.md) hello Azure CLI toocreate fürt használatáról.</span><span class="sxs-lookup"><span data-stu-id="56d95-108">If you are using a Linux, Unix, or Mac OS X client, see [Create a Linux-based HDInsight cluster using Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) for information about using hello Azure CLI toocreate a cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56d95-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="56d95-109">Prerequisites</span></span>
<span data-ttu-id="56d95-110">Az eljárás megkezdése előtt hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="56d95-110">You must have hello following before starting this procedure:</span></span>

* <span data-ttu-id="56d95-111">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="56d95-111">An Azure subscription.</span></span> <span data-ttu-id="56d95-112">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="56d95-112">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* [<span data-ttu-id="56d95-113">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="56d95-113">Azure PowerShell</span></span>](/powershell/azure/install-azurerm-ps)

    > [!IMPORTANT]
    > <span data-ttu-id="56d95-114">A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnt.</span><span class="sxs-lookup"><span data-stu-id="56d95-114">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="56d95-115">a dokumentum használatát hello új HDInsight-parancsmagokat, amelyek működnek az Azure Resource Manager hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="56d95-115">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="56d95-116">Kövesse a lépéseket hello [Azure PowerShell telepítése](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) tooinstall hello legújabb verziót.</span><span class="sxs-lookup"><span data-stu-id="56d95-116">Please follow hello steps in [Install Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="56d95-117">Ha vannak olyan parancsprogramjai, hogy szükség toobe módosított toouse hello új parancsmagok használata Azure Resource Managerrel működő című [áttelepítése tooAzure fejlesztési Resource Manager-alapú HDInsight-fürtök eszközei](hdinsight-hadoop-development-using-azure-resource-manager.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="56d95-117">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

## <a name="create-cluster"></a><span data-ttu-id="56d95-118">Fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="56d95-118">Create cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="56d95-119">HDInsight-fürtök Azure PowerShell használatával toocreate, végre kell hajtania a következő eljárások hello:</span><span class="sxs-lookup"><span data-stu-id="56d95-119">toocreate an HDInsight cluster by using Azure PowerShell, you must complete hello following procedures:</span></span>

* <span data-ttu-id="56d95-120">Egy Azure erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="56d95-120">Create an Azure resource group</span></span>
* <span data-ttu-id="56d95-121">Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="56d95-121">Create an Azure Storage account</span></span>
* <span data-ttu-id="56d95-122">Egy Azure Blob-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="56d95-122">Create an Azure Blob container</span></span>
* <span data-ttu-id="56d95-123">HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="56d95-123">Create an HDInsight cluster</span></span>

<span data-ttu-id="56d95-124">hello alábbi parancsfájl bemutatja, hogyan toocreate új fürt:</span><span class="sxs-lookup"><span data-stu-id="56d95-124">hello following script demonstrates how toocreate a new cluster:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]

<span data-ttu-id="56d95-125">hello hello fürt bejelentkezési azonosítóhoz megadott használt toocreate hello hello fürt Hadoop-felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="56d95-125">hello values you specify for hello cluster login are used toocreate hello Hadoop user account for hello cluster.</span></span> <span data-ttu-id="56d95-126">Használja a fiók tooconnect tooservices, például a web UI vagy a REST API-k hello fürtön található.</span><span class="sxs-lookup"><span data-stu-id="56d95-126">Use this account tooconnect tooservices hosted on hello cluster such as web UIs or REST APIs.</span></span>

<span data-ttu-id="56d95-127">hello hello SSH-felhasználó számára megadott értékei használt toocreate hello SSH-felhasználó hello fürt.</span><span class="sxs-lookup"><span data-stu-id="56d95-127">hello values you specify for hello SSH user are used toocreate hello SSH user for hello cluster.</span></span> <span data-ttu-id="56d95-128">Ezzel a távoli SSH-munkamenet toostart fiókot hello fürtön, és a feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="56d95-128">Use this account toostart a remote SSH session on hello cluster and run jobs.</span></span> <span data-ttu-id="56d95-129">További információkért lásd: hello [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="56d95-129">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="56d95-130">Ha azt tervezi, toouse több mint 32 munkavégző csomópontokhoz (vagy a fürt létrehozásakor vagy méretezési hello a fürt létrehozása után), is meg kell egy átjárócsomóponttal mérete legalább 8 maggal és 14 GB RAM-mal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="56d95-130">If you plan toouse more than 32 worker nodes (either at cluster creation or by scaling hello cluster after creation), you must also specify a head node size with at least 8 cores and 14 GB of RAM.</span></span>
>
> <span data-ttu-id="56d95-131">A csomópont-méretek és a társuló költségeket további információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="56d95-131">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

<span data-ttu-id="56d95-132">Is eltarthat, too20 perc toocreate egy fürt.</span><span class="sxs-lookup"><span data-stu-id="56d95-132">It can take up too20 minutes toocreate a cluster.</span></span>

## <a name="create-cluster-configuration-object"></a><span data-ttu-id="56d95-133">Fürt létrehozása: konfigurációs objektum</span><span class="sxs-lookup"><span data-stu-id="56d95-133">Create cluster: Configuration object</span></span>

<span data-ttu-id="56d95-134">Egy HDInsight konfigurációs objektum használatával is létrehozhat `New-AzureRmHDInsightClusterConfig` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="56d95-134">You can also create an HDInsight configuration object using `New-AzureRmHDInsightClusterConfig` cmdlet.</span></span> <span data-ttu-id="56d95-135">Ezt követően módosíthatja a konfigurációs objektum tooenable további konfigurációs lehetőségek a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="56d95-135">You can then modify this configuration object tooenable additional configuration options for your cluster.</span></span> <span data-ttu-id="56d95-136">Végül, használja a hello `-Config` hello paramétere `New-AzureRmHDInsightCluster` parancsmag toouse hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="56d95-136">Finally, use hello `-Config` parameter of hello `New-AzureRmHDInsightCluster` cmdlet toouse hello configuration.</span></span>

<span data-ttu-id="56d95-137">hello következő parancsfájlt hoz létre egy konfigurációs objektum tooconfigure egyik R Server a HDInsight-fürt típusa.</span><span class="sxs-lookup"><span data-stu-id="56d95-137">hello following script creates a configuration object tooconfigure an R Server on HDInsight cluster type.</span></span> <span data-ttu-id="56d95-138">hello konfiguráció lehetővé teszi, hogy egy élcsomópontot Rstudióból és egy további storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="56d95-138">hello configuration enables an edge node, RStudio, and an additional storage account.</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]

> [!WARNING]
> <span data-ttu-id="56d95-139">A storage-fiók egy másik helyen található mint hello HDInsight-fürt használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="56d95-139">Using a storage account in a different location than hello HDInsight cluster is not supported.</span></span> <span data-ttu-id="56d95-140">Ez a példa használatakor hello további storage-fiók létrehozása a hello hello kiszolgáló ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="56d95-140">When using this example, create hello additional storage account in hello same location as hello server.</span></span>

## <a name="customize-clusters"></a><span data-ttu-id="56d95-141">Fürtök személyre szabása</span><span class="sxs-lookup"><span data-stu-id="56d95-141">Customize clusters</span></span>

* <span data-ttu-id="56d95-142">Lásd: [testreszabása HDInsight-fürtök használata rendszerindítási](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="56d95-142">See [Customize HDInsight clusters using Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).</span></span>
* <span data-ttu-id="56d95-143">Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="56d95-143">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="56d95-144">Hello fürt törlése</span><span class="sxs-lookup"><span data-stu-id="56d95-144">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="56d95-145">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="56d95-145">Troubleshoot</span></span>

<span data-ttu-id="56d95-146">Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="56d95-146">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="56d95-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="56d95-147">Next steps</span></span>

<span data-ttu-id="56d95-148">Most, hogy sikeresen létrehozott egy HDInsight-fürtre, használja a következő erőforrások toolearn hogyan hello toowork a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="56d95-148">Now that you have successfully created an HDInsight cluster, use hello following resources toolearn how toowork with your cluster.</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="56d95-149">Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="56d95-149">Hadoop clusters</span></span>

* [<span data-ttu-id="56d95-150">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="56d95-150">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="56d95-151">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="56d95-151">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="56d95-152">Use MapReduce with HDInsight</span><span class="sxs-lookup"><span data-stu-id="56d95-152">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="56d95-153">HBase-fürtökkel</span><span class="sxs-lookup"><span data-stu-id="56d95-153">HBase clusters</span></span>

* [<span data-ttu-id="56d95-154">Az a HDInsight HBase első lépései</span><span class="sxs-lookup"><span data-stu-id="56d95-154">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="56d95-155">A HDInsight HBase Java-alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="56d95-155">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="56d95-156">Storm-fürtök</span><span class="sxs-lookup"><span data-stu-id="56d95-156">Storm clusters</span></span>

* [<span data-ttu-id="56d95-157">Java-topológiák fejlesztése hdinsight alatt futó Storm</span><span class="sxs-lookup"><span data-stu-id="56d95-157">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="56d95-158">A HDInsight alatt futó Storm Python-összetevőket használnak</span><span class="sxs-lookup"><span data-stu-id="56d95-158">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="56d95-159">Telepítheti és figyelheti a HDInsight alatt futó Storm topológiák</span><span class="sxs-lookup"><span data-stu-id="56d95-159">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a><span data-ttu-id="56d95-160">A Spark-fürtök</span><span class="sxs-lookup"><span data-stu-id="56d95-160">Spark clusters</span></span>

* [<span data-ttu-id="56d95-161">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="56d95-161">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="56d95-162">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="56d95-162">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)
* [<span data-ttu-id="56d95-163">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="56d95-163">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="56d95-164">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="56d95-164">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="56d95-165">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="56d95-165">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)

