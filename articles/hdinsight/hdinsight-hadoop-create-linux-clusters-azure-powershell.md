---
title: "PowerShell - Azure HDInsight Hadoop-fürtök létrehozása |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre Hadoop, HBase, Storm vagy Spark-fürtök Linux rendszeren működő HDInsight-hoz Azure PowerShell használatával."
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
ms.openlocfilehash: ca75974e6ec4f60739137d4cb5458bbfd735de3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a><span data-ttu-id="a1cb5-103">Linux-alapú fürtök létrehozása a Hdinsightban Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="a1cb5-103">Create Linux-based clusters in HDInsight using Azure PowerShell</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="a1cb5-104">Az Azure PowerShell egy hatékony parancsfájl-kezelési környezet, melyekkel automatizálhatja a központi telepítési és felügyeleti a alkalmazások a Microsoft Azure-ban, és szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-104">Azure PowerShell is a powerful scripting environment that you can use to control and automate the deployment and management of your workloads in Microsoft Azure.</span></span> <span data-ttu-id="a1cb5-105">Ez a dokumentum tájékoztatást ad azokról a Linux-alapú HDInsight-fürt létrehozása az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-105">This document provides information about how to create a Linux-based HDInsight cluster by using Azure PowerShell.</span></span> <span data-ttu-id="a1cb5-106">Példa parancsfájl is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-106">It also includes an example script.</span></span>

> [!NOTE]
> <span data-ttu-id="a1cb5-107">Az Azure PowerShell a Windows-ügyfelek csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-107">Azure PowerShell is only available on Windows clients.</span></span> <span data-ttu-id="a1cb5-108">Ha a Linux, Unix vagy Mac OS X-ügyfelet kell használnia, lásd: [hozzon létre egy Linux-alapú HDInsight-fürt Azure parancssori felület használatával](hdinsight-hadoop-create-linux-clusters-azure-cli.md) fürt létrehozása az Azure parancssori felület használatával kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-108">If you are using a Linux, Unix, or Mac OS X client, see [Create a Linux-based HDInsight cluster using Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) for information about using the Azure CLI to create a cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a1cb5-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a1cb5-109">Prerequisites</span></span>
<span data-ttu-id="a1cb5-110">Az eljárás megkezdése előtt a következőket kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="a1cb5-110">You must have the following before starting this procedure:</span></span>

* <span data-ttu-id="a1cb5-111">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-111">An Azure subscription.</span></span> <span data-ttu-id="a1cb5-112">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="a1cb5-112">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* [<span data-ttu-id="a1cb5-113">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a1cb5-113">Azure PowerShell</span></span>](/powershell/azure/install-azurerm-ps)

    > [!IMPORTANT]
    > <span data-ttu-id="a1cb5-114">A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnt.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-114">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="a1cb5-115">A jelen dokumentumban leírt lépések az új HDInsight-parancsmagokat használják, amelyek az Azure Resource Managerrel működnek.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-115">The steps in this document use the new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="a1cb5-116">Kövesse a lépéseket a [Azure PowerShell telepítése](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) Azure PowerShell legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-116">Please follow the steps in [Install Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) to install the latest version of Azure PowerShell.</span></span> <span data-ttu-id="a1cb5-117">Ha vannak olyan parancsprogramjai, amelyeket módosítani kell az új, az Azure Resource Managerrel működő parancsmagok használatához, tekintse meg az alábbi cikket: [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) (Az Azure Resource Manager-alapú fejlesztési eszközökre való áttérés HDInsight-fürtök esetén).</span><span class="sxs-lookup"><span data-stu-id="a1cb5-117">If you have scripts that need to be modified to use the new cmdlets that work with Azure Resource Manager, see [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

## <a name="create-cluster"></a><span data-ttu-id="a1cb5-118">Fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1cb5-118">Create cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="a1cb5-119">HDInsight-fürtök létrehozása az Azure PowerShell használatával, a következő eljárásokat kell végrehajtania:</span><span class="sxs-lookup"><span data-stu-id="a1cb5-119">To create an HDInsight cluster by using Azure PowerShell, you must complete the following procedures:</span></span>

* <span data-ttu-id="a1cb5-120">Egy Azure erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1cb5-120">Create an Azure resource group</span></span>
* <span data-ttu-id="a1cb5-121">Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1cb5-121">Create an Azure Storage account</span></span>
* <span data-ttu-id="a1cb5-122">Egy Azure Blob-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1cb5-122">Create an Azure Blob container</span></span>
* <span data-ttu-id="a1cb5-123">HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1cb5-123">Create an HDInsight cluster</span></span>

<span data-ttu-id="a1cb5-124">Az alábbi parancsfájl bemutatja, hogyan hozhat létre egy új fürtöt:</span><span class="sxs-lookup"><span data-stu-id="a1cb5-124">The following script demonstrates how to create a new cluster:</span></span>

<span data-ttu-id="a1cb5-125">[!code-powershell[fő](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]</span><span class="sxs-lookup"><span data-stu-id="a1cb5-125">[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]</span></span>

<span data-ttu-id="a1cb5-126">A fürt bejelentkezési azonosítóhoz megadott értékek a Hadoop felhasználói fiók a fürt létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-126">The values you specify for the cluster login are used to create the Hadoop user account for the cluster.</span></span> <span data-ttu-id="a1cb5-127">Ez a fiók használatával csatlakozhat például a web UI vagy a REST API-k a fürtön tárolt szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-127">Use this account to connect to services hosted on the cluster such as web UIs or REST APIs.</span></span>

<span data-ttu-id="a1cb5-128">Az SSH-felhasználó számára megadott érték az SSH-felhasználó a fürt létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-128">The values you specify for the SSH user are used to create the SSH user for the cluster.</span></span> <span data-ttu-id="a1cb5-129">Ez a fiók használatával indítson el egy távoli SSH-munkamenetet a fürtön, és a feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-129">Use this account to start a remote SSH session on the cluster and run jobs.</span></span> <span data-ttu-id="a1cb5-130">További információ: [SSH használata a HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="a1cb5-130">For more information, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a1cb5-131">Ha több mint 32 munkavégző csomópontokhoz (vagy a fürt létrehozásakor, vagy a fürt létrehozása után skálázással) használja, is meg kell egy átjárócsomóponttal mérete legalább 8 maggal és 14 GB RAM-mal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-131">If you plan to use more than 32 worker nodes (either at cluster creation or by scaling the cluster after creation), you must also specify a head node size with at least 8 cores and 14 GB of RAM.</span></span>
>
> <span data-ttu-id="a1cb5-132">A csomópont-méretek és a társuló költségeket további információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="a1cb5-132">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

<span data-ttu-id="a1cb5-133">A fürt létrehozásához akár 20 percig is tarthat.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-133">It can take up to 20 minutes to create a cluster.</span></span>

## <a name="create-cluster-configuration-object"></a><span data-ttu-id="a1cb5-134">Fürt létrehozása: konfigurációs objektum</span><span class="sxs-lookup"><span data-stu-id="a1cb5-134">Create cluster: Configuration object</span></span>

<span data-ttu-id="a1cb5-135">Egy HDInsight konfigurációs objektum használatával is létrehozhat `New-AzureRmHDInsightClusterConfig` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-135">You can also create an HDInsight configuration object using `New-AzureRmHDInsightClusterConfig` cmdlet.</span></span> <span data-ttu-id="a1cb5-136">Ezt követően módosíthatja a konfigurációs objektum ahhoz, hogy a fürt további konfigurációs beállítások.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-136">You can then modify this configuration object to enable additional configuration options for your cluster.</span></span> <span data-ttu-id="a1cb5-137">Végül a `-Config` paramétere a `New-AzureRmHDInsightCluster` parancsmag segítségével használja a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-137">Finally, use the `-Config` parameter of the `New-AzureRmHDInsightCluster` cmdlet to use the configuration.</span></span>

<span data-ttu-id="a1cb5-138">A következő parancsfájl egy konfigurációs objektumot az R-kiszolgáló konfigurálása a HDInsight-fürt típusa hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-138">The following script creates a configuration object to configure an R Server on HDInsight cluster type.</span></span> <span data-ttu-id="a1cb5-139">A konfiguráció lehetővé teszi, hogy egy élcsomópontot Rstudióból és egy további storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-139">The configuration enables an edge node, RStudio, and an additional storage account.</span></span>

<span data-ttu-id="a1cb5-140">[!code-powershell[fő](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]</span><span class="sxs-lookup"><span data-stu-id="a1cb5-140">[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]</span></span>

> [!WARNING]
> <span data-ttu-id="a1cb5-141">A storage-fiók egy másik helyen, mint a HDInsight-fürt használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-141">Using a storage account in a different location than the HDInsight cluster is not supported.</span></span> <span data-ttu-id="a1cb5-142">Ha ebben a példában, és a kiszolgáló ugyanazon a helyen hozza létre a további tárhely fiókot.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-142">When using this example, create the additional storage account in the same location as the server.</span></span>

## <a name="customize-clusters"></a><span data-ttu-id="a1cb5-143">Fürtök személyre szabása</span><span class="sxs-lookup"><span data-stu-id="a1cb5-143">Customize clusters</span></span>

* <span data-ttu-id="a1cb5-144">Lásd: [testreszabása HDInsight-fürtök használata rendszerindítási](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="a1cb5-144">See [Customize HDInsight clusters using Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).</span></span>
* <span data-ttu-id="a1cb5-145">Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="a1cb5-145">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="a1cb5-146">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="a1cb5-146">Delete the cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="a1cb5-147">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="a1cb5-147">Troubleshoot</span></span>

<span data-ttu-id="a1cb5-148">Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="a1cb5-148">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a1cb5-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1cb5-149">Next steps</span></span>

<span data-ttu-id="a1cb5-150">Most, hogy sikeresen létrehozta a HDInsight-fürtöt, a következő források segítségével megtudhatja, hogyan működnek a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="a1cb5-150">Now that you have successfully created an HDInsight cluster, use the following resources to learn how to work with your cluster.</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="a1cb5-151">Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="a1cb5-151">Hadoop clusters</span></span>

* [<span data-ttu-id="a1cb5-152">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="a1cb5-152">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="a1cb5-153">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="a1cb5-153">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="a1cb5-154">Use MapReduce with HDInsight</span><span class="sxs-lookup"><span data-stu-id="a1cb5-154">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="a1cb5-155">HBase-fürtökkel</span><span class="sxs-lookup"><span data-stu-id="a1cb5-155">HBase clusters</span></span>

* [<span data-ttu-id="a1cb5-156">Az a HDInsight HBase első lépései</span><span class="sxs-lookup"><span data-stu-id="a1cb5-156">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="a1cb5-157">A HDInsight HBase Java-alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="a1cb5-157">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="a1cb5-158">Storm-fürtök</span><span class="sxs-lookup"><span data-stu-id="a1cb5-158">Storm clusters</span></span>

* [<span data-ttu-id="a1cb5-159">Java-topológiák fejlesztése hdinsight alatt futó Storm</span><span class="sxs-lookup"><span data-stu-id="a1cb5-159">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="a1cb5-160">A HDInsight alatt futó Storm Python-összetevőket használnak</span><span class="sxs-lookup"><span data-stu-id="a1cb5-160">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="a1cb5-161">Telepítheti és figyelheti a HDInsight alatt futó Storm topológiák</span><span class="sxs-lookup"><span data-stu-id="a1cb5-161">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a><span data-ttu-id="a1cb5-162">A Spark-fürtök</span><span class="sxs-lookup"><span data-stu-id="a1cb5-162">Spark clusters</span></span>

* [<span data-ttu-id="a1cb5-163">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="a1cb5-163">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="a1cb5-164">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="a1cb5-164">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)
* [<span data-ttu-id="a1cb5-165">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="a1cb5-165">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="a1cb5-166">Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére</span><span class="sxs-lookup"><span data-stu-id="a1cb5-166">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="a1cb5-167">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="a1cb5-167">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)

