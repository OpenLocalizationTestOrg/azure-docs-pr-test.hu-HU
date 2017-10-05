---
title: "Virtuális hálózatok - Azure belül a HBase-fürt replikálás konfigurálása |} Microsoft Docs"
description: "Megtudhatja, hogyan kell a terheléselosztást, magas rendelkezésre állású, nulla állásidő áttelepítési/frissítési egy HDInsight-verzióról a másikra, és vész-helyreállítási HBase-replikálás konfigurálása."
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 895709391486acb4a9d7a54ef046956539913f7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-hbase-cluster-replication-within-virtual-networks"></a><span data-ttu-id="f1ab7-103">Virtuális hálózatok belül a HBase-fürt replikálás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1ab7-103">Configure HBase cluster replication within virtual networks</span></span>

<span data-ttu-id="f1ab7-104">Megtudhatja, hogyan konfigurálja a HBase-replikálás belül egy virtuális hálózathoz (VNet) vagy virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-104">Learn how to configure HBase replication within one virtual network (VNet) or between two virtual networks.</span></span>

<span data-ttu-id="f1ab7-105">Fürt replikáció forrás-ügyfélleküldéses módszer használ.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-105">Cluster replication uses a source-push methodology.</span></span> <span data-ttu-id="f1ab7-106">A cél- és a HBase-fürtöt lehet, vagy teljesíteni tudja mindkét szerepkörök egyszerre.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-106">An HBase cluster can be a source or a destination, or it can fulfill both roles at once.</span></span> <span data-ttu-id="f1ab7-107">Replikációs aszinkron, és a cél replikációs végleges konzisztencia.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-107">Replication is asynchronous, and the goal of replication is eventual consistency.</span></span> <span data-ttu-id="f1ab7-108">Amikor a forrás egy oszlop családhoz szerkesztési replikáció engedélyezett kap, hogy a szerkesztési összes cél fürt propagálja.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-108">When the source receives an edit to a column family with replication enabled, that edit is propagated to all destination clusters.</span></span> <span data-ttu-id="f1ab7-109">Ha adatok az egyik fürtről a másikra replikálódik, a forrás fürt és az összes fürt, amely már rendelkezik felhasznált adatok replikációs hurkok megelőzése érdekében nyomon követi.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-109">When data is replicated from one cluster to another, the source cluster and all clusters that have already consumed the data are tracked to prevent replication loops.</span></span>

<span data-ttu-id="f1ab7-110">Az oktatóanyag a forrás-cél replikációs állít.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-110">In this tutorial, you will configure a source-destination replication.</span></span> <span data-ttu-id="f1ab7-111">Más fürt topológiája esetén lásd: [Apache HBase referencia-útmutató](http://hbase.apache.org/book.html#_cluster_replication).</span><span class="sxs-lookup"><span data-stu-id="f1ab7-111">For other cluster topologies, see [Apache HBase Reference Guide](http://hbase.apache.org/book.html#_cluster_replication).</span></span>

<span data-ttu-id="f1ab7-112">A HBase replikációs használati esetekben egyetlen virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-112">HBase replication usage cases for a single virtual network:</span></span>

* <span data-ttu-id="f1ab7-113">Terheléselosztás – például a cél fürtön futó vizsgálatokat vagy MapReduce-feladatok és a kiindulási fürt adatok bevitele</span><span class="sxs-lookup"><span data-stu-id="f1ab7-113">Load balancing--for example, running scans or MapReduce jobs on the destination cluster and ingesting data on the source cluster</span></span>
* <span data-ttu-id="f1ab7-114">Magas rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="f1ab7-114">High availability</span></span>
* <span data-ttu-id="f1ab7-115">Adatok áttelepítése a HBase egyik fürtről a másikra</span><span class="sxs-lookup"><span data-stu-id="f1ab7-115">Migrating data from one HBase cluster to another</span></span>
* <span data-ttu-id="f1ab7-116">Azure HDInsight-fürtök egy-es verzióról a másikra</span><span class="sxs-lookup"><span data-stu-id="f1ab7-116">Upgrading an Azure HDInsight cluster from one version to another</span></span>

<span data-ttu-id="f1ab7-117">A HBase replikációs használati esetek két virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-117">HBase replication usage cases for two virtual networks:</span></span>

* <span data-ttu-id="f1ab7-118">Vészhelyreállítás</span><span class="sxs-lookup"><span data-stu-id="f1ab7-118">Disaster recovery</span></span>
* <span data-ttu-id="f1ab7-119">Terheléselosztás és az alkalmazás particionálás</span><span class="sxs-lookup"><span data-stu-id="f1ab7-119">Load balancing and partitioning the application</span></span>
* <span data-ttu-id="f1ab7-120">Magas rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="f1ab7-120">High availability</span></span>

<span data-ttu-id="f1ab7-121">Fürtök használatával replikálja [parancsfájl-művelet](hdinsight-hadoop-customize-cluster-linux.md) található parancsfájlok [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).</span><span class="sxs-lookup"><span data-stu-id="f1ab7-121">You replicate clusters by using [script action](hdinsight-hadoop-customize-cluster-linux.md) scripts located at [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f1ab7-122">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f1ab7-122">Prerequisites</span></span>
<span data-ttu-id="f1ab7-123">Az oktatóanyag elindításához Azure-előfizetéssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-123">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="f1ab7-124">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="f1ab7-124">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="configure-the-environments"></a><span data-ttu-id="f1ab7-125">A környezet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1ab7-125">Configure the environments</span></span>

<span data-ttu-id="f1ab7-126">Három lehetséges konfiguráció van:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-126">There are three possible configurations:</span></span>

- <span data-ttu-id="f1ab7-127">Egy Azure virtuális hálózatban két HBase fürtök</span><span class="sxs-lookup"><span data-stu-id="f1ab7-127">Two HBase clusters in one Azure virtual network</span></span>
- <span data-ttu-id="f1ab7-128">Két HBase-fürtökkel a ugyanabban a régióban két különböző virtuális hálózatokon</span><span class="sxs-lookup"><span data-stu-id="f1ab7-128">Two HBase clusters in two different virtual networks in the same region</span></span>
- <span data-ttu-id="f1ab7-129">Két HBase-fürtökkel a két különböző régiókban (georeplikáció) két különböző virtuális hálózatokon</span><span class="sxs-lookup"><span data-stu-id="f1ab7-129">Two HBase clusters in two different virtual networks in two different regions (geo-replication)</span></span>

<span data-ttu-id="f1ab7-130">Konfigurálja a környezetek megkönnyítése létrehoztunk egy [Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f1ab7-130">To make it easier to configure the environments, we have created some [Azure Resource Manager templates](../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="f1ab7-131">Ha inkább a környezetek egyéb módszerek konfigurálása, lásd:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-131">If you prefer to configure the environments by using other methods, see:</span></span>

- [<span data-ttu-id="f1ab7-132">Linux-alapú Hadoop-fürtök létrehozása a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="f1ab7-132">Create Linux-based Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
- [<span data-ttu-id="f1ab7-133">Hozzon létre HBase-fürtökkel Azure virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="f1ab7-133">Create HBase clusters in Azure Virtual Network</span></span>](hdinsight-hbase-provision-vnet.md)

### <a name="configure-one-virtual-network"></a><span data-ttu-id="f1ab7-134">Egy virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1ab7-134">Configure one virtual network</span></span>

<span data-ttu-id="f1ab7-135">Kattintson az alábbi képre kattintva hozzon létre két HBase-fürtökkel ugyanabban a virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-135">Click the following image to create two HBase clusters in the same virtual network.</span></span> <span data-ttu-id="f1ab7-136">A sablon tárolódik [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).</span><span class="sxs-lookup"><span data-stu-id="f1ab7-136">The template is stored in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-one-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

### <a name="configure-two-virtual-networks-in-the-same-region"></a><span data-ttu-id="f1ab7-137">Két virtuális hálózat ugyanabban a régióban konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1ab7-137">Configure two virtual networks in the same region</span></span>

<span data-ttu-id="f1ab7-138">Kattintson az alábbi képre kattintva hozzon létre két virtuális hálózatokat és a Vnetben társviszony-létesítés és két HBase-fürtökkel ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-138">Click the following image to create two virtual networks with VNet peering and two HBase clusters in the same region.</span></span> <span data-ttu-id="f1ab7-139">A sablon tárolódik [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).</span><span class="sxs-lookup"><span data-stu-id="f1ab7-139">The template is stored in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-two-vnets-same-region%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>



<span data-ttu-id="f1ab7-140">Ebben az esetben [Vnetben társviszony-létesítés](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f1ab7-140">This scenario requires [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span> <span data-ttu-id="f1ab7-141">A sablon lehetővé teszi, hogy a Vnetben társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-141">The template enables VNet peering.</span></span>   

<span data-ttu-id="f1ab7-142">HBase-replikálás ZooKeeper virtuális gépek IP-címeket használ.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-142">HBase replication uses IP addresses of the ZooKeeper VMs.</span></span> <span data-ttu-id="f1ab7-143">A cél HBase ZooKeeper csomópontok statikus IP-címet kell konfigurálnia.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-143">You must configure static IP addresses for the destination HBase ZooKeeper nodes.</span></span>

<span data-ttu-id="f1ab7-144">**Statikus IP-címek konfigurálása**</span><span class="sxs-lookup"><span data-stu-id="f1ab7-144">**To configure static IP addresses**</span></span>

1. <span data-ttu-id="f1ab7-145">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f1ab7-145">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f1ab7-146">Kattintson a bal oldali menü **erőforráscsoportok**.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-146">From the left menu, click **Resource Groups**.</span></span>
3. <span data-ttu-id="f1ab7-147">Kattintson az erőforráscsoport, amely a célként megadott HBase-fürtöt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-147">Click your resource group that contains the destination HBase cluster.</span></span> <span data-ttu-id="f1ab7-148">Ez az az erőforráscsoport, amelyet a környezet létrehozása a Resource Manager-sablon használatakor.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-148">This is the resource group that you specified when you used the Resource Manager template to create the environment.</span></span> <span data-ttu-id="f1ab7-149">A szűrő segítségével listáját.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-149">You can use the filter to narrow down the list.</span></span> <span data-ttu-id="f1ab7-150">Láthatja, hogy az erőforrások listáját, amelyek a két virtuális hálózatot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-150">You can see a list of resources that contains the two virtual networks.</span></span>
4. <span data-ttu-id="f1ab7-151">Kattintson a virtuális hálózat, amely a célként megadott HBase-fürtöt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-151">Click the virtual network that contains the destination HBase cluster.</span></span> <span data-ttu-id="f1ab7-152">Kattintson például **xxxx-vnet2**.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-152">For example, click **xxxx-vnet2**.</span></span> <span data-ttu-id="f1ab7-153">Megtekintheti a kezdetű névvel rendelkező három eszközök **nic-zookeepermode -**.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-153">You can see three devices with names that start with **nic-zookeepermode-**.</span></span> <span data-ttu-id="f1ab7-154">Az eszközöket a három ZooKeeper virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-154">Those devices are the three ZooKeeper VMs.</span></span>
5. <span data-ttu-id="f1ab7-155">Kattintson a ZooKeeper virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-155">Click one of the ZooKeeper VMs.</span></span>
6. <span data-ttu-id="f1ab7-156">Kattintson a **IP-konfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-156">Click **IP configurations**.</span></span>
7. <span data-ttu-id="f1ab7-157">Kattintson a **ipConfig1** a listából.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-157">Click **ipConfig1** from the list.</span></span>
8. <span data-ttu-id="f1ab7-158">Kattintson a **statikus**, és jegyezze fel az aktuális IP-címet.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-158">Click **Static**, and write down the actual IP address.</span></span> <span data-ttu-id="f1ab7-159">A replikáció engedélyezése parancsfájlművelet futtatásakor szüksége lesz az IP-címet.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-159">You will need the IP address when you run the script action to enable replication.</span></span>

  ![HDInsight HBase replikációs ZooKeeper statikus IP-cím](./media/hdinsight-hbase-replication/hdinsight-hbase-replication-zookeeper-static-ip.png)

9. <span data-ttu-id="f1ab7-161">Ismételje meg a 6. a két ZooKeeper csomópontok a statikus IP-címének beállítása.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-161">Repeat step 6 to set the static IP address for the other two ZooKeeper nodes.</span></span>

<span data-ttu-id="f1ab7-162">A kereszt-VNet-forgatókönyv kell használnia a **- ip** kapcsoló meghívásakor a **hdi_enable_replication.sh** parancsfájl-művelet.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-162">For the cross-VNet scenario, you must use the **-ip** switch when calling the **hdi_enable_replication.sh** script action.</span></span>

### <a name="configure-two-virtual-networks-in-two-different-regions"></a><span data-ttu-id="f1ab7-163">Két különböző régiókban két virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1ab7-163">Configure two virtual networks in two different regions</span></span>

<span data-ttu-id="f1ab7-164">Kattintson az alábbi képen két különböző régiókban két virtuális hálózatok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-164">Click the following image to create two virtual networks in two different regions.</span></span> <span data-ttu-id="f1ab7-165">A sablon egy nyilvános Azure Blob-tároló tárolja.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-165">The template is stored in a public Azure Blob container.</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fdeploy-hbase-geo-replication.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

<span data-ttu-id="f1ab7-166">Hozzon létre két virtuális hálózatok közötti VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-166">Create a VPN gateway between the two virtual networks.</span></span> <span data-ttu-id="f1ab7-167">Útmutatásért lásd: [hozzon létre egy Vnetet a pont-pont kapcsolat](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f1ab7-167">For instructions, see [Create a VNet with a site-to-site connection](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).</span></span>

<span data-ttu-id="f1ab7-168">HBase-replikálás ZooKeeper virtuális gépek IP-címeket használ.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-168">HBase replication uses IP addresses of the ZooKeeper VMs.</span></span> <span data-ttu-id="f1ab7-169">A cél HBase ZooKeeper csomópontok statikus IP-címet kell konfigurálnia.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-169">You must configure static IP addresses for the destination HBase ZooKeeper nodes.</span></span> <span data-ttu-id="f1ab7-170">Statikus IP-cím konfigurálása, a cikkben "Konfigurálása két virtuális hálózat ugyanabban a régióban" című szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-170">To configure static IP, see the "Configure two virtual networks in the same region" section in this article.</span></span>

<span data-ttu-id="f1ab7-171">A kereszt-VNet-forgatókönyv kell használnia a **- ip** kapcsoló meghívásakor a **hdi_enable_replication.sh** parancsfájl-művelet.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-171">For the cross-VNet scenario, you must use the **-ip** switch when calling the **hdi_enable_replication.sh** script action.</span></span>

## <a name="load-test-data"></a><span data-ttu-id="f1ab7-172">Vizsgálati adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="f1ab7-172">Load test data</span></span>

<span data-ttu-id="f1ab7-173">A replikált egy fürtöt, meg kell adnia replikálásához táblák.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-173">When you replicate a cluster, you must specify the tables to replicate.</span></span> <span data-ttu-id="f1ab7-174">Ebben a szakaszban néhány adat betölti a forrás-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-174">In this section, you will load some data into the source cluster.</span></span> <span data-ttu-id="f1ab7-175">A következő szakaszban akkor engedélyezi a fürtök közötti replikáció.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-175">In the next section, you will enable replication between the two clusters.</span></span>

<span data-ttu-id="f1ab7-176">Kövesse az utasításokat a [HBase oktatóanyag: az Apache HBase Linux-alapú hadooppal a Hdinsightban első lépéseiben](hdinsight-hbase-tutorial-get-started-linux.md) létrehozásához egy **névjegyek** tábla és adatok beszúrása a táblába.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-176">Follow the instructions in [HBase tutorial: Get started using Apache HBase with Linux-based Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) to create a **Contacts** table and insert some data into the table.</span></span>

## <a name="enable-replication"></a><span data-ttu-id="f1ab7-177">A replikáció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f1ab7-177">Enable replication</span></span>

<span data-ttu-id="f1ab7-178">A következő lépések bemutatják, hogyan hívhatja meg a parancsfájl parancsfájlművelet Azure-portálról.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-178">The following steps show how to call the script action script from the Azure portal.</span></span> <span data-ttu-id="f1ab7-179">Parancsfájl műveletek futtatása az Azure PowerShell és az Azure parancssori felület (CLI) használatával, lásd: [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="f1ab7-179">For running a script action by using Azure PowerShell and the Azure command-line interface (CLI), see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="f1ab7-180">**Azure-portálról HBase-replikálás engedélyezése**</span><span class="sxs-lookup"><span data-stu-id="f1ab7-180">**To enable HBase replication from the Azure portal**</span></span>

1. <span data-ttu-id="f1ab7-181">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f1ab7-181">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f1ab7-182">Nyissa meg a forrás HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-182">Open the source HBase cluster.</span></span>
3. <span data-ttu-id="f1ab7-183">Kattintson a fürt menü **Parancsfájlműveletek**.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-183">From the cluster menu, click **Script Actions**.</span></span>
4. <span data-ttu-id="f1ab7-184">Kattintson a **nyújt új** a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-184">Click **Submit New** from the top of the blade.</span></span>
5. <span data-ttu-id="f1ab7-185">Válassza ki vagy adja meg a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-185">Select or enter the following information:</span></span>

  - <span data-ttu-id="f1ab7-186">**Név**: Adjon meg **engedélyezze a replikálást**.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-186">**Name**: Enter **Enable replication**.</span></span>
  - <span data-ttu-id="f1ab7-187">**Parancsprogram URL-Címének bash**: Adjon meg **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-187">**Bash Script URL**: Enter **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.</span></span>
  - <span data-ttu-id="f1ab7-188">**HEAD**: kiválasztott.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-188">**Head**: Selected.</span></span> <span data-ttu-id="f1ab7-189">Törölje a csomóponttípusok.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-189">Clear the other node types.</span></span>
  - <span data-ttu-id="f1ab7-190">**Paraméterek**: A következő minta paraméterek engedélyezze a replikációját a létező táblák, és másolja az adatokat a forráskiszolgálóról a fürt a fürt céltárhelyét, az összes:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-190">**Parameters**: The following sample parameters enable replication for all the existing tables and copy all the data from the source cluster to the destination cluster:</span></span>

            -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata

6. <span data-ttu-id="f1ab7-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-191">Click **Create**.</span></span> <span data-ttu-id="f1ab7-192">A parancsfájl eltarthat egy ideig, különösen a - copydata argumentum használatakor.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-192">The script can take some time, especially when the -copydata argument is used.</span></span>

<span data-ttu-id="f1ab7-193">Szükséges argumentumokkal:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-193">Required arguments:</span></span>

|<span data-ttu-id="f1ab7-194">Név</span><span class="sxs-lookup"><span data-stu-id="f1ab7-194">Name</span></span>|<span data-ttu-id="f1ab7-195">Leírás</span><span class="sxs-lookup"><span data-stu-id="f1ab7-195">Description</span></span>|
|----|-----------|
|<span data-ttu-id="f1ab7-196">-s,--src-fürt</span><span class="sxs-lookup"><span data-stu-id="f1ab7-196">-s, --src-cluster</span></span> | <span data-ttu-id="f1ab7-197">Adja meg a forrás HBase fürt DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-197">Specify the DNS name of the source HBase cluster.</span></span> <span data-ttu-id="f1ab7-198">Például: -s hbsrccluster, a fürt--src = hbsrccluster</span><span class="sxs-lookup"><span data-stu-id="f1ab7-198">For example: -s hbsrccluster, --src-cluster=hbsrccluster</span></span> |
|<span data-ttu-id="f1ab7-199">-d, nyári időszámítás – a fürt</span><span class="sxs-lookup"><span data-stu-id="f1ab7-199">-d, --dst-cluster</span></span> | <span data-ttu-id="f1ab7-200">Adja meg a cél (replika) HBase-fürtöt DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-200">Specify the DNS name of the destination (replica) HBase cluster.</span></span> <span data-ttu-id="f1ab7-201">Például: -s dsthbcluster, a fürt--src = dsthbcluster</span><span class="sxs-lookup"><span data-stu-id="f1ab7-201">For example: -s dsthbcluster, --src-cluster=dsthbcluster</span></span> |
|<span data-ttu-id="f1ab7-202">--src-ambari-jelszó - sp,</span><span class="sxs-lookup"><span data-stu-id="f1ab7-202">-sp, --src-ambari-password</span></span> | <span data-ttu-id="f1ab7-203">Adja meg a rendszergazdai jelszó az Ambari a kiindulási HBase fürt.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-203">Specify the admin password for Ambari on the source HBase cluster.</span></span> |
|<span data-ttu-id="f1ab7-204">-dp, nyári időszámítás –--jelszavaknak</span><span class="sxs-lookup"><span data-stu-id="f1ab7-204">-dp, --dst-ambari-password</span></span> | <span data-ttu-id="f1ab7-205">Adja meg a rendszergazdai jelszó az Ambari a HBase célfürtön.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-205">Specify the admin password for Ambari on the destination HBase cluster.</span></span>|

<span data-ttu-id="f1ab7-206">Választható argumentumok:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-206">Optional arguments:</span></span>

|<span data-ttu-id="f1ab7-207">Név</span><span class="sxs-lookup"><span data-stu-id="f1ab7-207">Name</span></span>|<span data-ttu-id="f1ab7-208">Leírás</span><span class="sxs-lookup"><span data-stu-id="f1ab7-208">Description</span></span>|
|----|-----------|
|<span data-ttu-id="f1ab7-209">--src-ambari-user - su,</span><span class="sxs-lookup"><span data-stu-id="f1ab7-209">-su, --src-ambari-user</span></span> | <span data-ttu-id="f1ab7-210">A kiindulási HBase fürt Ambari a rendszergazda felhasználónevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-210">Specify the admin username for Ambari on the source HBase cluster.</span></span> <span data-ttu-id="f1ab7-211">Az alapértelmezett érték **admin**.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-211">The default value is **admin**.</span></span> |
|<span data-ttu-id="f1ab7-212">-du, nyári időszámítás –-ambari-felhasználó</span><span class="sxs-lookup"><span data-stu-id="f1ab7-212">-du, --dst-ambari-user</span></span> | <span data-ttu-id="f1ab7-213">A rendszergazda felhasználóneve az Ambari meg a cél HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-213">Specify the admin username for Ambari on the destination HBase cluster.</span></span> <span data-ttu-id="f1ab7-214">Az alapértelmezett érték **admin**.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-214">The default value is **admin**.</span></span> |
|<span data-ttu-id="f1ab7-215">-t,--tábla-lista</span><span class="sxs-lookup"><span data-stu-id="f1ab7-215">-t, --table-list</span></span> | <span data-ttu-id="f1ab7-216">Adja meg a táblák replikálni.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-216">Specify the tables to be replicated.</span></span> <span data-ttu-id="f1ab7-217">Például:--tábla-lista = "table1; table2; Tábl3".</span><span class="sxs-lookup"><span data-stu-id="f1ab7-217">For example: --table-list="table1;table2;table3".</span></span> <span data-ttu-id="f1ab7-218">Ha nem adja meg a táblák, az összes meglévő HBase táblák replikálódnak.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-218">If you don't specify tables, all existing HBase tables are replicated.</span></span>|
|<span data-ttu-id="f1ab7-219">-m,--gép</span><span class="sxs-lookup"><span data-stu-id="f1ab7-219">-m, --machine</span></span> | <span data-ttu-id="f1ab7-220">Adja meg az átjárócsomóponthoz, ahol a parancsfájlművelet futni fog.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-220">Specify the head node where the script action will run.</span></span> <span data-ttu-id="f1ab7-221">Hn1 vagy hn0 értéke.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-221">The value is either hn1 or hn0.</span></span> <span data-ttu-id="f1ab7-222">Mivel a hn0 általában busier, hn1 használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-222">Because hn0 is usually busier, we recommend using hn1.</span></span> <span data-ttu-id="f1ab7-223">Akkor használja ezt a beállítást, ha a számítógépén a $0 parancsfájl parancsfájl műveletként a HDInsight portálon vagy az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-223">You use this option when you're running the $0 script as a script action from the HDInsight portal or Azure PowerShell.</span></span>|
|<span data-ttu-id="f1ab7-224">-ip</span><span class="sxs-lookup"><span data-stu-id="f1ab7-224">-ip</span></span> | <span data-ttu-id="f1ab7-225">Az argumentum megadása akkor kötelező, ha két virtuális hálózatok közötti replikáció folyamatban engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-225">This argument is required when you're enabling replication between two virtual networks.</span></span> <span data-ttu-id="f1ab7-226">Ezt az argumentumot úgy működik, mint a kapcsoló használatához a statikus IP-címek a ZooKeeper csomópontok replika fürtök FQDN-nevek helyett.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-226">This argument acts as a switch to use the static IPs of ZooKeeper nodes from replica clusters instead of FQDN names.</span></span> <span data-ttu-id="f1ab7-227">A statikus IP-címet kell előre konfigurált replikáció engedélyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-227">The static IPs need to be preconfigured before you enable replication.</span></span> |
|<span data-ttu-id="f1ab7-228">-cp, - copydata</span><span class="sxs-lookup"><span data-stu-id="f1ab7-228">-cp, -copydata</span></span> | <span data-ttu-id="f1ab7-229">Az áttelepítés a meglévő adatok a táblák, amelyben engedélyezve van a replikáció engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-229">Enable the migration of existing data on the tables where replication is enabled.</span></span> |
|<span data-ttu-id="f1ab7-230">-fordulat/perces, - replikálás-phoenix-meta</span><span class="sxs-lookup"><span data-stu-id="f1ab7-230">-rpm, -replicate-phoenix-meta</span></span> | <span data-ttu-id="f1ab7-231">Engedélyezheti a replikálást Phoenix rendszertáblákra.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-231">Enable replication on Phoenix system tables.</span></span> <br><br><span data-ttu-id="f1ab7-232">*Használja ezt a beállítást a kellő körültekintéssel járjon el.*</span><span class="sxs-lookup"><span data-stu-id="f1ab7-232">*Use this option with caution.*</span></span> <span data-ttu-id="f1ab7-233">Azt javasoljuk, hogy Ön hozza létre újból Phoenix táblák replika fürtökön használja ezt a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-233">We recommend that you re-create Phoenix tables on replica clusters before you use this script.</span></span> |
|<span data-ttu-id="f1ab7-234">-h, – Súgó</span><span class="sxs-lookup"><span data-stu-id="f1ab7-234">-h, --help</span></span> | <span data-ttu-id="f1ab7-235">Használati információk jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-235">Display usage information.</span></span> |

<span data-ttu-id="f1ab7-236">A print_usage() szakasza a [parancsfájl](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) paraméterek részletesen ismerteti.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-236">The print_usage() section of the [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) provides a detailed explanation of parameters.</span></span>

<span data-ttu-id="f1ab7-237">A parancsfájlművelet sikeres telepítése után SSH segítségével csatlakozzon a cél HBase-fürtöt, és ellenőrizze az adatokat a rendszer replikálta.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-237">After the script action is successfully deployed, you can use SSH to connect to the destination HBase cluster, and verify the data has been replicated.</span></span>

### <a name="replication-scenarios"></a><span data-ttu-id="f1ab7-238">A replikáció eseteire</span><span class="sxs-lookup"><span data-stu-id="f1ab7-238">Replication scenarios</span></span>

<span data-ttu-id="f1ab7-239">Az alábbi listában láthatók az egyes általános használati esetek és a paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-239">The following list shows you some general usage cases and their parameter settings:</span></span>

- <span data-ttu-id="f1ab7-240">**Minden tábla a fürtök közötti replikáció engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-240">**Enable replication on all tables between the two clusters**.</span></span> <span data-ttu-id="f1ab7-241">Ez a forgatókönyv nem igényel a Másolás/áttelepítése a meglévő adatok a táblák, és nem használ Phoenix táblák.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-241">This scenario does not require the copy/migration of existing data on the tables, and it does not use Phoenix tables.</span></span> <span data-ttu-id="f1ab7-242">Használja a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-242">Use the following parameters:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- <span data-ttu-id="f1ab7-243">**Engedélyezze a replikálást a megadott táblák**.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-243">**Enable replication on specific tables**.</span></span> <span data-ttu-id="f1ab7-244">A table1, table2 és Tábl3 replikáció engedélyezéséhez használja a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-244">Use the following parameters to enable replication on table1, table2, and table3:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- <span data-ttu-id="f1ab7-245">**A megadott táblák replikálás engedélyezése és a meglévő adatok másolása**.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-245">**Enable replication on specific tables and copy the existing data**.</span></span> <span data-ttu-id="f1ab7-246">A table1, table2 és Tábl3 replikáció engedélyezéséhez használja a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-246">Use the following parameters to enable replication on table1, table2, and table3:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- <span data-ttu-id="f1ab7-247">**Engedélyezze a replikálást a forrás célhelyre phoenix metaadatok replikálásához minden tábla**.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-247">**Enable replication on all tables with replicating phoenix metadata from source to destination**.</span></span> <span data-ttu-id="f1ab7-248">Phoenix metaadatok replikációs nincs tökéletes megoldás, és engedélyezve legyenek-e figyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-248">Phoenix metadata replication is not perfect and should be enabled with caution.</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a><span data-ttu-id="f1ab7-249">Másolja, majd az adatok áttelepítése</span><span class="sxs-lookup"><span data-stu-id="f1ab7-249">Copy and migrate data</span></span>

<span data-ttu-id="f1ab7-250">Két külön parancsfájl művelet parancsprogramok másolása/történő áttelepítésére vonatkozó adatok a replikáció engedélyezése után:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-250">There are two separate script action scripts for copying/migrating data after replication is enabled:</span></span>

- <span data-ttu-id="f1ab7-251">[Kis táblák parancsfájl](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (néhány gigabájttól méret és a teljes másolás várható befejezés kisebb, mint egy óra)</span><span class="sxs-lookup"><span data-stu-id="f1ab7-251">[Script for small tables](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (a few gigabytes in size, and overall copy is expected to finish in less than one hour)</span></span>

- <span data-ttu-id="f1ab7-252">[Nagy méretű táblákra vonatkozó parancsfájl](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (várható másolása egy óránál több időt vesz igénybe)</span><span class="sxs-lookup"><span data-stu-id="f1ab7-252">[Script for large tables](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (expected to take longer than one hour to copy)</span></span>

<span data-ttu-id="f1ab7-253">Hajtsa végre az ugyanezt az eljárást [engedélyezze a replikálást](#enable-replication) hívni a parancsfájl művelet a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-253">You can follow the same procedure in [Enable replication](#enable-replication) to call the script action with the following parameters:</span></span>

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

<span data-ttu-id="f1ab7-254">A print_usage() szakasza a [parancsfájl](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) paraméterek részletes leírását.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-254">The print_usage() section of the [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) provides a detailed description of parameters.</span></span>

### <a name="scenarios"></a><span data-ttu-id="f1ab7-255">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="f1ab7-255">Scenarios</span></span>

- <span data-ttu-id="f1ab7-256">**Adott táblákra (Teszt1 Teszt2 és Teszt3) másolja az összes sorát, vége: jelenleg szerkesztett (aktuális időbélyeg)**:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-256">**Copy specific tables (test1, test2, and test3) for all rows edited till now (current time stamp)**:</span></span>

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  <span data-ttu-id="f1ab7-257">vagy</span><span class="sxs-lookup"><span data-stu-id="f1ab7-257">or</span></span>

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- <span data-ttu-id="f1ab7-258">**Másolja át a megadott időtartomány adott táblák**:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-258">**Copy specific tables with specified time range**:</span></span>

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a><span data-ttu-id="f1ab7-259">Tiltsa le a replikációt</span><span class="sxs-lookup"><span data-stu-id="f1ab7-259">Disable replication</span></span>

<span data-ttu-id="f1ab7-260">Tiltsa le a replikációt, helye: egy másik parancsfájl parancsfájlművelet használja [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh).</span><span class="sxs-lookup"><span data-stu-id="f1ab7-260">To disable replication, you use another script action script located at [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh).</span></span> <span data-ttu-id="f1ab7-261">Hajtsa végre az ugyanezt az eljárást [engedélyezze a replikálást](#enable-replication) hívni a parancsfájl művelet a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-261">You can follow the same procedure in [Enable replication](#enable-replication) to call the script action with the following parameters:</span></span>

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari Password> <-all|-t "table1;table2;...">  

<span data-ttu-id="f1ab7-262">A print_usage() szakasza a [parancsfájl](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) paraméterek részletesen ismerteti.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-262">The print_usage() section of the [script](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) provides a detailed explanation of parameters.</span></span>

### <a name="scenarios"></a><span data-ttu-id="f1ab7-263">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="f1ab7-263">Scenarios</span></span>

- <span data-ttu-id="f1ab7-264">**Tiltsa le a replikációt minden tábla**:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-264">**Disable replication on all tables**:</span></span>

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  <span data-ttu-id="f1ab7-265">vagy</span><span class="sxs-lookup"><span data-stu-id="f1ab7-265">or</span></span>

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari username> --src-ambari-password=<source cluster Ambari password>

- <span data-ttu-id="f1ab7-266">**Tiltsa le a replikációt a megadott táblák (table1 table2 és Tábl3)**:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-266">**Disable replication on specified tables (table1, table2, and table3)**:</span></span>

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a><span data-ttu-id="f1ab7-267">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1ab7-267">Next steps</span></span>

<span data-ttu-id="f1ab7-268">Ebben az oktatóprogramban megismerte HBase-replikálás konfigurálása két adatközpont között.</span><span class="sxs-lookup"><span data-stu-id="f1ab7-268">In this tutorial, you learned how to configure HBase replication across two datacenters.</span></span> <span data-ttu-id="f1ab7-269">HDInsight és HBase kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="f1ab7-269">To learn more about HDInsight and HBase, see:</span></span>

* <span data-ttu-id="f1ab7-270">[Ismerkedés az Apache HBase a hdinsight eszközben][hdinsight-hbase-get-started]</span><span class="sxs-lookup"><span data-stu-id="f1ab7-270">[Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started]</span></span>
* <span data-ttu-id="f1ab7-271">[HDInsight HBase áttekintése][hdinsight-hbase-overview]</span><span class="sxs-lookup"><span data-stu-id="f1ab7-271">[HDInsight HBase overview][hdinsight-hbase-overview]</span></span>
* <span data-ttu-id="f1ab7-272">[Hozzon létre HBase-fürtökkel Azure virtuális hálózat][hdinsight-hbase-provision-vnet]</span><span class="sxs-lookup"><span data-stu-id="f1ab7-272">[Create HBase clusters in Azure Virtual Network][hdinsight-hbase-provision-vnet]</span></span>
* <span data-ttu-id="f1ab7-273">[A hbase eszközzel valós idejű Twitter sentiment elemzése][hdinsight-hbase-twitter-sentiment]</span><span class="sxs-lookup"><span data-stu-id="f1ab7-273">[Analyze real-time Twitter sentiment with HBase][hdinsight-hbase-twitter-sentiment]</span></span>
* <span data-ttu-id="f1ab7-274">[Érzékelőadatok elemzése a Storm és HBase a HDInsight (Hadoop)][hdinsight-sensor-data]</span><span class="sxs-lookup"><span data-stu-id="f1ab7-274">[Analyzing sensor data with Storm and HBase in HDInsight (Hadoop)][hdinsight-sensor-data]</span></span>

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started-linux.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
