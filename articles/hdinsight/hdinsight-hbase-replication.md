---
title: "virtuális hálózatok – Azure belüli aaaConfigure HBase-fürt replikáció |} Microsoft Docs"
description: "Megtudhatja, hogyan a(z) terheléselosztást, a magas rendelkezésre állás, a nulla állásidő áttelepítés vagy frissítés egy HDInsight-verzió tooanother és vész-helyreállítási tooconfigure HBase-replikálás."
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
ms.openlocfilehash: ba1c44f26b7cbf4a7a88159b12b3e064ea9f9a20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hbase-cluster-replication-within-virtual-networks"></a><span data-ttu-id="d70ef-103">Virtuális hálózatok belül a HBase-fürt replikálás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d70ef-103">Configure HBase cluster replication within virtual networks</span></span>

<span data-ttu-id="d70ef-104">Megtudhatja, hogyan tooconfigure HBase-replikálás belül egy virtuális hálózathoz (VNet) vagy virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="d70ef-104">Learn how tooconfigure HBase replication within one virtual network (VNet) or between two virtual networks.</span></span>

<span data-ttu-id="d70ef-105">Fürt replikáció forrás-ügyfélleküldéses módszer használ.</span><span class="sxs-lookup"><span data-stu-id="d70ef-105">Cluster replication uses a source-push methodology.</span></span> <span data-ttu-id="d70ef-106">A cél- és a HBase-fürtöt lehet, vagy teljesíteni tudja mindkét szerepkörök egyszerre.</span><span class="sxs-lookup"><span data-stu-id="d70ef-106">An HBase cluster can be a source or a destination, or it can fulfill both roles at once.</span></span> <span data-ttu-id="d70ef-107">Replikációs aszinkron, és hello replikációs célja a végleges konzisztencia.</span><span class="sxs-lookup"><span data-stu-id="d70ef-107">Replication is asynchronous, and hello goal of replication is eventual consistency.</span></span> <span data-ttu-id="d70ef-108">Hello forrás egy Szerkesztés tooa oszlop termékcsalád megkapja a replikáció engedélyezett, a Szerkesztés esetén propagált tooall cél fürtök.</span><span class="sxs-lookup"><span data-stu-id="d70ef-108">When hello source receives an edit tooa column family with replication enabled, that edit is propagated tooall destination clusters.</span></span> <span data-ttu-id="d70ef-109">Adatokat a rendszer replikálja a egy fürt tooanother, hello forrásfürt és minden olyan fürt, amely már rendelkezik felhasznált hello adatok nyomon követett tooprevent replikációs hurkok.</span><span class="sxs-lookup"><span data-stu-id="d70ef-109">When data is replicated from one cluster tooanother, hello source cluster and all clusters that have already consumed hello data are tracked tooprevent replication loops.</span></span>

<span data-ttu-id="d70ef-110">Az oktatóanyag a forrás-cél replikációs állít.</span><span class="sxs-lookup"><span data-stu-id="d70ef-110">In this tutorial, you will configure a source-destination replication.</span></span> <span data-ttu-id="d70ef-111">Más fürt topológiája esetén lásd: [Apache HBase referencia-útmutató](http://hbase.apache.org/book.html#_cluster_replication).</span><span class="sxs-lookup"><span data-stu-id="d70ef-111">For other cluster topologies, see [Apache HBase Reference Guide](http://hbase.apache.org/book.html#_cluster_replication).</span></span>

<span data-ttu-id="d70ef-112">A HBase replikációs használati esetekben egyetlen virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="d70ef-112">HBase replication usage cases for a single virtual network:</span></span>

* <span data-ttu-id="d70ef-113">Terheléselosztás – például vizsgálatok vagy MapReduce-feladatok hello cél fürtön fut, és hello kiindulási fürt adatok bevitele</span><span class="sxs-lookup"><span data-stu-id="d70ef-113">Load balancing--for example, running scans or MapReduce jobs on hello destination cluster and ingesting data on hello source cluster</span></span>
* <span data-ttu-id="d70ef-114">Magas rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="d70ef-114">High availability</span></span>
* <span data-ttu-id="d70ef-115">A HBase-fürtöt egy tooanother adatok áttelepítése</span><span class="sxs-lookup"><span data-stu-id="d70ef-115">Migrating data from one HBase cluster tooanother</span></span>
* <span data-ttu-id="d70ef-116">Egy verzió tooanother Azure HDInsight-fürtök frissítése</span><span class="sxs-lookup"><span data-stu-id="d70ef-116">Upgrading an Azure HDInsight cluster from one version tooanother</span></span>

<span data-ttu-id="d70ef-117">A HBase replikációs használati esetek két virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="d70ef-117">HBase replication usage cases for two virtual networks:</span></span>

* <span data-ttu-id="d70ef-118">Vészhelyreállítás</span><span class="sxs-lookup"><span data-stu-id="d70ef-118">Disaster recovery</span></span>
* <span data-ttu-id="d70ef-119">Terheléselosztás és a particionálás hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d70ef-119">Load balancing and partitioning hello application</span></span>
* <span data-ttu-id="d70ef-120">Magas rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="d70ef-120">High availability</span></span>

<span data-ttu-id="d70ef-121">Fürtök használatával replikálja [parancsfájl-művelet](hdinsight-hadoop-customize-cluster-linux.md) található parancsfájlok [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).</span><span class="sxs-lookup"><span data-stu-id="d70ef-121">You replicate clusters by using [script action](hdinsight-hadoop-customize-cluster-linux.md) scripts located at [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d70ef-122">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d70ef-122">Prerequisites</span></span>
<span data-ttu-id="d70ef-123">Az oktatóanyag elindításához Azure-előfizetéssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="d70ef-123">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="d70ef-124">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="d70ef-124">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="configure-hello-environments"></a><span data-ttu-id="d70ef-125">Hello környezetek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d70ef-125">Configure hello environments</span></span>

<span data-ttu-id="d70ef-126">Három lehetséges konfiguráció van:</span><span class="sxs-lookup"><span data-stu-id="d70ef-126">There are three possible configurations:</span></span>

- <span data-ttu-id="d70ef-127">Egy Azure virtuális hálózatban két HBase fürtök</span><span class="sxs-lookup"><span data-stu-id="d70ef-127">Two HBase clusters in one Azure virtual network</span></span>
- <span data-ttu-id="d70ef-128">A két különböző virtuális hálózatokon, a HBase fürtök hello ugyanabban a régióban</span><span class="sxs-lookup"><span data-stu-id="d70ef-128">Two HBase clusters in two different virtual networks in hello same region</span></span>
- <span data-ttu-id="d70ef-129">Két HBase-fürtökkel a két különböző régiókban (georeplikáció) két különböző virtuális hálózatokon</span><span class="sxs-lookup"><span data-stu-id="d70ef-129">Two HBase clusters in two different virtual networks in two different regions (geo-replication)</span></span>

<span data-ttu-id="d70ef-130">toomake azt tooconfigure hello környezetek egyszerűbb, a rendszer készített néhány [Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d70ef-130">toomake it easier tooconfigure hello environments, we have created some [Azure Resource Manager templates](../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="d70ef-131">Ha jobban szeret tooconfigure hello környezetek egyéb módszerek, lásd:</span><span class="sxs-lookup"><span data-stu-id="d70ef-131">If you prefer tooconfigure hello environments by using other methods, see:</span></span>

- [<span data-ttu-id="d70ef-132">Linux-alapú Hadoop-fürtök létrehozása a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="d70ef-132">Create Linux-based Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
- [<span data-ttu-id="d70ef-133">Hozzon létre HBase-fürtökkel Azure virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="d70ef-133">Create HBase clusters in Azure Virtual Network</span></span>](hdinsight-hbase-provision-vnet.md)

### <a name="configure-one-virtual-network"></a><span data-ttu-id="d70ef-134">Egy virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d70ef-134">Configure one virtual network</span></span>

<span data-ttu-id="d70ef-135">Kattintson a következő kép toocreate két HBase-fürtökkel a hello hello ugyanazt a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="d70ef-135">Click hello following image toocreate two HBase clusters in hello same virtual network.</span></span> <span data-ttu-id="d70ef-136">hello sablon tárolódik [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).</span><span class="sxs-lookup"><span data-stu-id="d70ef-136">hello template is stored in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-one-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>

### <a name="configure-two-virtual-networks-in-hello-same-region"></a><span data-ttu-id="d70ef-137">Két virtuális hálózatok konfigurálása a hello ugyanabban a régióban</span><span class="sxs-lookup"><span data-stu-id="d70ef-137">Configure two virtual networks in hello same region</span></span>

<span data-ttu-id="d70ef-138">Kattintson a következő kép toocreate két virtuális hálózatokat és a Vnetben társviszony-létesítés és két HBase-fürtökkel a hello hello ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="d70ef-138">Click hello following image toocreate two virtual networks with VNet peering and two HBase clusters in hello same region.</span></span> <span data-ttu-id="d70ef-139">hello sablon tárolódik [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).</span><span class="sxs-lookup"><span data-stu-id="d70ef-139">hello template is stored in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-two-vnets-same-region%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>



<span data-ttu-id="d70ef-140">Ebben az esetben [Vnetben társviszony-létesítés](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d70ef-140">This scenario requires [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span> <span data-ttu-id="d70ef-141">hello sablon lehetővé teszi, hogy a Vnetben társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="d70ef-141">hello template enables VNet peering.</span></span>   

<span data-ttu-id="d70ef-142">HBase-replikálás hello ZooKeeper virtuális gépek IP-címét használja.</span><span class="sxs-lookup"><span data-stu-id="d70ef-142">HBase replication uses IP addresses of hello ZooKeeper VMs.</span></span> <span data-ttu-id="d70ef-143">Hello cél HBase ZooKeeper csomópontok statikus IP-címet kell konfigurálnia.</span><span class="sxs-lookup"><span data-stu-id="d70ef-143">You must configure static IP addresses for hello destination HBase ZooKeeper nodes.</span></span>

<span data-ttu-id="d70ef-144">**tooconfigure statikus IP-címek**</span><span class="sxs-lookup"><span data-stu-id="d70ef-144">**tooconfigure static IP addresses**</span></span>

1. <span data-ttu-id="d70ef-145">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d70ef-145">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="d70ef-146">Hello bal oldali menüben kattintson **erőforráscsoportok**.</span><span class="sxs-lookup"><span data-stu-id="d70ef-146">From hello left menu, click **Resource Groups**.</span></span>
3. <span data-ttu-id="d70ef-147">Kattintson a hello cél HBase-fürtöt tartalmazó erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="d70ef-147">Click your resource group that contains hello destination HBase cluster.</span></span> <span data-ttu-id="d70ef-148">Ez a hello Resource Manager sablon toocreate hello környezet használatakor a megadott erőforráscsoport hello.</span><span class="sxs-lookup"><span data-stu-id="d70ef-148">This is hello resource group that you specified when you used hello Resource Manager template toocreate hello environment.</span></span> <span data-ttu-id="d70ef-149">Hello szűrő toonarrow hello listából is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d70ef-149">You can use hello filter toonarrow down hello list.</span></span> <span data-ttu-id="d70ef-150">Hello két virtuális hálózatot tartalmaz erőforrás listáját láthatja.</span><span class="sxs-lookup"><span data-stu-id="d70ef-150">You can see a list of resources that contains hello two virtual networks.</span></span>
4. <span data-ttu-id="d70ef-151">Kattintson a virtuális hálózati hello hello cél HBase-fürtöt tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="d70ef-151">Click hello virtual network that contains hello destination HBase cluster.</span></span> <span data-ttu-id="d70ef-152">Kattintson például **xxxx-vnet2**.</span><span class="sxs-lookup"><span data-stu-id="d70ef-152">For example, click **xxxx-vnet2**.</span></span> <span data-ttu-id="d70ef-153">Megtekintheti a kezdetű névvel rendelkező három eszközök **nic-zookeepermode -**.</span><span class="sxs-lookup"><span data-stu-id="d70ef-153">You can see three devices with names that start with **nic-zookeepermode-**.</span></span> <span data-ttu-id="d70ef-154">Az eszközök három hello ZooKeeper virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="d70ef-154">Those devices are hello three ZooKeeper VMs.</span></span>
5. <span data-ttu-id="d70ef-155">Kattintson az egyik hello ZooKeeper virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="d70ef-155">Click one of hello ZooKeeper VMs.</span></span>
6. <span data-ttu-id="d70ef-156">Kattintson a **IP-konfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="d70ef-156">Click **IP configurations**.</span></span>
7. <span data-ttu-id="d70ef-157">Kattintson a **ipConfig1** hello listából.</span><span class="sxs-lookup"><span data-stu-id="d70ef-157">Click **ipConfig1** from hello list.</span></span>
8. <span data-ttu-id="d70ef-158">Kattintson a **statikus**, és jegyezze fel a hello tényleges IP-címet.</span><span class="sxs-lookup"><span data-stu-id="d70ef-158">Click **Static**, and write down hello actual IP address.</span></span> <span data-ttu-id="d70ef-159">Hello IP-címet kell hello parancsfájl művelet tooenable replikációs futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="d70ef-159">You will need hello IP address when you run hello script action tooenable replication.</span></span>

  ![HDInsight HBase replikációs ZooKeeper statikus IP-cím](./media/hdinsight-hbase-replication/hdinsight-hbase-replication-zookeeper-static-ip.png)

9. <span data-ttu-id="d70ef-161">Ismételje meg a lépés 6 tooset hello statikus IP-cím hello más két ZooKeeper csomópontok.</span><span class="sxs-lookup"><span data-stu-id="d70ef-161">Repeat step 6 tooset hello static IP address for hello other two ZooKeeper nodes.</span></span>

<span data-ttu-id="d70ef-162">A hello kereszt-VNet-forgatókönyv, kell használnia hello **- ip** Váltás a következő meghívásakor: hello **hdi_enable_replication.sh** parancsfájl-művelet.</span><span class="sxs-lookup"><span data-stu-id="d70ef-162">For hello cross-VNet scenario, you must use hello **-ip** switch when calling hello **hdi_enable_replication.sh** script action.</span></span>

### <a name="configure-two-virtual-networks-in-two-different-regions"></a><span data-ttu-id="d70ef-163">Két különböző régiókban két virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d70ef-163">Configure two virtual networks in two different regions</span></span>

<span data-ttu-id="d70ef-164">Kattintson a következő kép toocreate két virtuális hálózatok két különböző régiókban hello.</span><span class="sxs-lookup"><span data-stu-id="d70ef-164">Click hello following image toocreate two virtual networks in two different regions.</span></span> <span data-ttu-id="d70ef-165">hello sablon egy nyilvános Azure Blob-tároló tárolja.</span><span class="sxs-lookup"><span data-stu-id="d70ef-165">hello template is stored in a public Azure Blob container.</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fdeploy-hbase-geo-replication.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>

<span data-ttu-id="d70ef-166">Hozzon létre egy VPN-átjáró hello virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="d70ef-166">Create a VPN gateway between hello two virtual networks.</span></span> <span data-ttu-id="d70ef-167">Útmutatásért lásd: [hozzon létre egy Vnetet a pont-pont kapcsolat](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d70ef-167">For instructions, see [Create a VNet with a site-to-site connection](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).</span></span>

<span data-ttu-id="d70ef-168">HBase-replikálás hello ZooKeeper virtuális gépek IP-címét használja.</span><span class="sxs-lookup"><span data-stu-id="d70ef-168">HBase replication uses IP addresses of hello ZooKeeper VMs.</span></span> <span data-ttu-id="d70ef-169">Hello cél HBase ZooKeeper csomópontok statikus IP-címet kell konfigurálnia.</span><span class="sxs-lookup"><span data-stu-id="d70ef-169">You must configure static IP addresses for hello destination HBase ZooKeeper nodes.</span></span> <span data-ttu-id="d70ef-170">tooconfigure statikus IP-címet, lásd: hello "lévő virtuális hálózatok konfigurálása a két hello ugyanabban a régióban" című részben.</span><span class="sxs-lookup"><span data-stu-id="d70ef-170">tooconfigure static IP, see hello "Configure two virtual networks in hello same region" section in this article.</span></span>

<span data-ttu-id="d70ef-171">A hello kereszt-VNet-forgatókönyv, kell használnia hello **- ip** Váltás a következő meghívásakor: hello **hdi_enable_replication.sh** parancsfájl-művelet.</span><span class="sxs-lookup"><span data-stu-id="d70ef-171">For hello cross-VNet scenario, you must use hello **-ip** switch when calling hello **hdi_enable_replication.sh** script action.</span></span>

## <a name="load-test-data"></a><span data-ttu-id="d70ef-172">Vizsgálati adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="d70ef-172">Load test data</span></span>

<span data-ttu-id="d70ef-173">A replikált egy fürtöt, meg kell adnia hello táblák tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="d70ef-173">When you replicate a cluster, you must specify hello tables tooreplicate.</span></span> <span data-ttu-id="d70ef-174">Ez a szakasz néhány adat betölti hello forrás fürtbe.</span><span class="sxs-lookup"><span data-stu-id="d70ef-174">In this section, you will load some data into hello source cluster.</span></span> <span data-ttu-id="d70ef-175">A következő szakaszban hello engedélyezi hello fürtök közötti replikáció.</span><span class="sxs-lookup"><span data-stu-id="d70ef-175">In hello next section, you will enable replication between hello two clusters.</span></span>

<span data-ttu-id="d70ef-176">Hello utasításait követve [HBase oktatóanyag: az Apache HBase Linux-alapú hadooppal a Hdinsightban első lépéseiben](hdinsight-hbase-tutorial-get-started-linux.md) toocreate egy **névjegyek** tábla és adatok beszúrása hello táblába.</span><span class="sxs-lookup"><span data-stu-id="d70ef-176">Follow hello instructions in [HBase tutorial: Get started using Apache HBase with Linux-based Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) toocreate a **Contacts** table and insert some data into hello table.</span></span>

## <a name="enable-replication"></a><span data-ttu-id="d70ef-177">A replikáció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d70ef-177">Enable replication</span></span>

<span data-ttu-id="d70ef-178">hello lépések bemutatják, hogyan toocall hello parancsfájl parancsfájlművelet hello Azure-portálon a.</span><span class="sxs-lookup"><span data-stu-id="d70ef-178">hello following steps show how toocall hello script action script from hello Azure portal.</span></span> <span data-ttu-id="d70ef-179">Parancsfájl műveletek futtatása az Azure PowerShell és hello Azure parancssori felület (CLI) használatával, lásd: [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="d70ef-179">For running a script action by using Azure PowerShell and hello Azure command-line interface (CLI), see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="d70ef-180">**tooenable HBase replikáció hello Azure-portálon**</span><span class="sxs-lookup"><span data-stu-id="d70ef-180">**tooenable HBase replication from hello Azure portal**</span></span>

1. <span data-ttu-id="d70ef-181">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d70ef-181">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="d70ef-182">Nyissa meg a hello forrás HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="d70ef-182">Open hello source HBase cluster.</span></span>
3. <span data-ttu-id="d70ef-183">Hello fürt menüben kattintson **Parancsfájlműveletek**.</span><span class="sxs-lookup"><span data-stu-id="d70ef-183">From hello cluster menu, click **Script Actions**.</span></span>
4. <span data-ttu-id="d70ef-184">Kattintson a **nyújt új** a hello felső hello panelről.</span><span class="sxs-lookup"><span data-stu-id="d70ef-184">Click **Submit New** from hello top of hello blade.</span></span>
5. <span data-ttu-id="d70ef-185">Válassza ki vagy adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="d70ef-185">Select or enter hello following information:</span></span>

  - <span data-ttu-id="d70ef-186">**Név**: Adjon meg **engedélyezze a replikálást**.</span><span class="sxs-lookup"><span data-stu-id="d70ef-186">**Name**: Enter **Enable replication**.</span></span>
  - <span data-ttu-id="d70ef-187">**Parancsprogram URL-Címének bash**: Adjon meg **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.</span><span class="sxs-lookup"><span data-stu-id="d70ef-187">**Bash Script URL**: Enter **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.</span></span>
  - <span data-ttu-id="d70ef-188">**HEAD**: kiválasztott.</span><span class="sxs-lookup"><span data-stu-id="d70ef-188">**Head**: Selected.</span></span> <span data-ttu-id="d70ef-189">Törölje a jelet hello más csomóponttípusok.</span><span class="sxs-lookup"><span data-stu-id="d70ef-189">Clear hello other node types.</span></span>
  - <span data-ttu-id="d70ef-190">**Paraméterek**: hello alábbi paraméterek a replikáció engedélyezése az összes hello meglévő táblázatban példa, és az összes hello adatának átmásolásához hello forrás fürt toohello célfürtöt:</span><span class="sxs-lookup"><span data-stu-id="d70ef-190">**Parameters**: hello following sample parameters enable replication for all hello existing tables and copy all hello data from hello source cluster toohello destination cluster:</span></span>

            -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata

6. <span data-ttu-id="d70ef-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d70ef-191">Click **Create**.</span></span> <span data-ttu-id="d70ef-192">hello parancsfájl eltarthat egy ideig, különösen akkor hello - copydata argumentum használatakor.</span><span class="sxs-lookup"><span data-stu-id="d70ef-192">hello script can take some time, especially when hello -copydata argument is used.</span></span>

<span data-ttu-id="d70ef-193">Szükséges argumentumokkal:</span><span class="sxs-lookup"><span data-stu-id="d70ef-193">Required arguments:</span></span>

|<span data-ttu-id="d70ef-194">Név</span><span class="sxs-lookup"><span data-stu-id="d70ef-194">Name</span></span>|<span data-ttu-id="d70ef-195">Leírás</span><span class="sxs-lookup"><span data-stu-id="d70ef-195">Description</span></span>|
|----|-----------|
|<span data-ttu-id="d70ef-196">-s,--src-fürt</span><span class="sxs-lookup"><span data-stu-id="d70ef-196">-s, --src-cluster</span></span> | <span data-ttu-id="d70ef-197">Adja meg a DNS-neve hello hello forrás HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="d70ef-197">Specify hello DNS name of hello source HBase cluster.</span></span> <span data-ttu-id="d70ef-198">Például: -s hbsrccluster, a fürt--src = hbsrccluster</span><span class="sxs-lookup"><span data-stu-id="d70ef-198">For example: -s hbsrccluster, --src-cluster=hbsrccluster</span></span> |
|<span data-ttu-id="d70ef-199">-d, nyári időszámítás – a fürt</span><span class="sxs-lookup"><span data-stu-id="d70ef-199">-d, --dst-cluster</span></span> | <span data-ttu-id="d70ef-200">Adja meg a DNS-neve hello hello cél (replika) HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="d70ef-200">Specify hello DNS name of hello destination (replica) HBase cluster.</span></span> <span data-ttu-id="d70ef-201">Például: -s dsthbcluster, a fürt--src = dsthbcluster</span><span class="sxs-lookup"><span data-stu-id="d70ef-201">For example: -s dsthbcluster, --src-cluster=dsthbcluster</span></span> |
|<span data-ttu-id="d70ef-202">--src-ambari-jelszó - sp,</span><span class="sxs-lookup"><span data-stu-id="d70ef-202">-sp, --src-ambari-password</span></span> | <span data-ttu-id="d70ef-203">Ambari hello kiindulási HBase fürt hello rendszergazdai jelszó megadása.</span><span class="sxs-lookup"><span data-stu-id="d70ef-203">Specify hello admin password for Ambari on hello source HBase cluster.</span></span> |
|<span data-ttu-id="d70ef-204">-dp, nyári időszámítás –--jelszavaknak</span><span class="sxs-lookup"><span data-stu-id="d70ef-204">-dp, --dst-ambari-password</span></span> | <span data-ttu-id="d70ef-205">Ambari hello cél HBase fürtön hello rendszergazdai jelszó megadása.</span><span class="sxs-lookup"><span data-stu-id="d70ef-205">Specify hello admin password for Ambari on hello destination HBase cluster.</span></span>|

<span data-ttu-id="d70ef-206">Választható argumentumok:</span><span class="sxs-lookup"><span data-stu-id="d70ef-206">Optional arguments:</span></span>

|<span data-ttu-id="d70ef-207">Név</span><span class="sxs-lookup"><span data-stu-id="d70ef-207">Name</span></span>|<span data-ttu-id="d70ef-208">Leírás</span><span class="sxs-lookup"><span data-stu-id="d70ef-208">Description</span></span>|
|----|-----------|
|<span data-ttu-id="d70ef-209">--src-ambari-user - su,</span><span class="sxs-lookup"><span data-stu-id="d70ef-209">-su, --src-ambari-user</span></span> | <span data-ttu-id="d70ef-210">Hello kiindulási HBase fürt Ambari hello rendszergazda felhasználónevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="d70ef-210">Specify hello admin username for Ambari on hello source HBase cluster.</span></span> <span data-ttu-id="d70ef-211">hello alapértelmezett értéke **admin**.</span><span class="sxs-lookup"><span data-stu-id="d70ef-211">hello default value is **admin**.</span></span> |
|<span data-ttu-id="d70ef-212">-du, nyári időszámítás –-ambari-felhasználó</span><span class="sxs-lookup"><span data-stu-id="d70ef-212">-du, --dst-ambari-user</span></span> | <span data-ttu-id="d70ef-213">Hello cél HBase fürtön Ambari hello rendszergazda felhasználónevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="d70ef-213">Specify hello admin username for Ambari on hello destination HBase cluster.</span></span> <span data-ttu-id="d70ef-214">hello alapértelmezett értéke **admin**.</span><span class="sxs-lookup"><span data-stu-id="d70ef-214">hello default value is **admin**.</span></span> |
|<span data-ttu-id="d70ef-215">-t,--tábla-lista</span><span class="sxs-lookup"><span data-stu-id="d70ef-215">-t, --table-list</span></span> | <span data-ttu-id="d70ef-216">Adja meg a replikált hello táblák toobe.</span><span class="sxs-lookup"><span data-stu-id="d70ef-216">Specify hello tables toobe replicated.</span></span> <span data-ttu-id="d70ef-217">Például:--tábla-lista = "table1; table2; Tábl3".</span><span class="sxs-lookup"><span data-stu-id="d70ef-217">For example: --table-list="table1;table2;table3".</span></span> <span data-ttu-id="d70ef-218">Ha nem adja meg a táblák, az összes meglévő HBase táblák replikálódnak.</span><span class="sxs-lookup"><span data-stu-id="d70ef-218">If you don't specify tables, all existing HBase tables are replicated.</span></span>|
|<span data-ttu-id="d70ef-219">-m,--gép</span><span class="sxs-lookup"><span data-stu-id="d70ef-219">-m, --machine</span></span> | <span data-ttu-id="d70ef-220">Adja meg a hello átjárócsomópont ahol hello parancsfájlművelet futni fog.</span><span class="sxs-lookup"><span data-stu-id="d70ef-220">Specify hello head node where hello script action will run.</span></span> <span data-ttu-id="d70ef-221">hn1 vagy hn0 hello értéke.</span><span class="sxs-lookup"><span data-stu-id="d70ef-221">hello value is either hn1 or hn0.</span></span> <span data-ttu-id="d70ef-222">Mivel a hn0 általában busier, hn1 használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="d70ef-222">Because hn0 is usually busier, we recommend using hn1.</span></span> <span data-ttu-id="d70ef-223">Akkor használja ezt a beállítást, ha a számítógépén hello $0 parancsfájl parancsfájl műveletként hello HDInsight portálon vagy az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d70ef-223">You use this option when you're running hello $0 script as a script action from hello HDInsight portal or Azure PowerShell.</span></span>|
|<span data-ttu-id="d70ef-224">-ip</span><span class="sxs-lookup"><span data-stu-id="d70ef-224">-ip</span></span> | <span data-ttu-id="d70ef-225">Az argumentum megadása akkor kötelező, ha két virtuális hálózatok közötti replikáció folyamatban engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="d70ef-225">This argument is required when you're enabling replication between two virtual networks.</span></span> <span data-ttu-id="d70ef-226">Ez az argumentum egy kapcsoló toouse hello statikus IP-címek a ZooKeeper csomópontok replika fürtök FQDN-nevek helyett funkcionál.</span><span class="sxs-lookup"><span data-stu-id="d70ef-226">This argument acts as a switch toouse hello static IPs of ZooKeeper nodes from replica clusters instead of FQDN names.</span></span> <span data-ttu-id="d70ef-227">hello statikus IP-címet kell toobe előre konfigurált a replikáció engedélyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="d70ef-227">hello static IPs need toobe preconfigured before you enable replication.</span></span> |
|<span data-ttu-id="d70ef-228">-cp, - copydata</span><span class="sxs-lookup"><span data-stu-id="d70ef-228">-cp, -copydata</span></span> | <span data-ttu-id="d70ef-229">Hello áttelepítése a meglévő adatok hello táblákon, amelyben engedélyezve van a replikáció engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d70ef-229">Enable hello migration of existing data on hello tables where replication is enabled.</span></span> |
|<span data-ttu-id="d70ef-230">-fordulat/perces, - replikálás-phoenix-meta</span><span class="sxs-lookup"><span data-stu-id="d70ef-230">-rpm, -replicate-phoenix-meta</span></span> | <span data-ttu-id="d70ef-231">Engedélyezheti a replikálást Phoenix rendszertáblákra.</span><span class="sxs-lookup"><span data-stu-id="d70ef-231">Enable replication on Phoenix system tables.</span></span> <br><br><span data-ttu-id="d70ef-232">*Használja ezt a beállítást a kellő körültekintéssel járjon el.*</span><span class="sxs-lookup"><span data-stu-id="d70ef-232">*Use this option with caution.*</span></span> <span data-ttu-id="d70ef-233">Azt javasoljuk, hogy Ön hozza létre újból Phoenix táblák replika fürtökön használja ezt a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="d70ef-233">We recommend that you re-create Phoenix tables on replica clusters before you use this script.</span></span> |
|<span data-ttu-id="d70ef-234">-h, – Súgó</span><span class="sxs-lookup"><span data-stu-id="d70ef-234">-h, --help</span></span> | <span data-ttu-id="d70ef-235">Használati információk jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="d70ef-235">Display usage information.</span></span> |

<span data-ttu-id="d70ef-236">hello print_usage() szakasza hello [parancsfájl](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) paraméterek részletesen ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d70ef-236">hello print_usage() section of hello [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) provides a detailed explanation of parameters.</span></span>

<span data-ttu-id="d70ef-237">Hello parancsfájl művelet végeztével sikeresen telepített, akkor is SSH tooconnect toohello cél HBase-fürtöt használ, és ellenőrizze hello adatokat a rendszer replikálta.</span><span class="sxs-lookup"><span data-stu-id="d70ef-237">After hello script action is successfully deployed, you can use SSH tooconnect toohello destination HBase cluster, and verify hello data has been replicated.</span></span>

### <a name="replication-scenarios"></a><span data-ttu-id="d70ef-238">A replikáció eseteire</span><span class="sxs-lookup"><span data-stu-id="d70ef-238">Replication scenarios</span></span>

<span data-ttu-id="d70ef-239">hello alábbi lista mutatja azokat, néhány általános használati esetek és a paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="d70ef-239">hello following list shows you some general usage cases and their parameter settings:</span></span>

- <span data-ttu-id="d70ef-240">**Minden tábla hello két fürtök közötti replikáció engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="d70ef-240">**Enable replication on all tables between hello two clusters**.</span></span> <span data-ttu-id="d70ef-241">Ez a forgatókönyv nem igényel hello másolási/áttelepítése a meglévő adatok hello táblákon, és nem használ Phoenix táblákat.</span><span class="sxs-lookup"><span data-stu-id="d70ef-241">This scenario does not require hello copy/migration of existing data on hello tables, and it does not use Phoenix tables.</span></span> <span data-ttu-id="d70ef-242">A következő paraméterek hello használata:</span><span class="sxs-lookup"><span data-stu-id="d70ef-242">Use hello following parameters:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- <span data-ttu-id="d70ef-243">**Engedélyezze a replikálást a megadott táblák**.</span><span class="sxs-lookup"><span data-stu-id="d70ef-243">**Enable replication on specific tables**.</span></span> <span data-ttu-id="d70ef-244">Használja a következő paraméterek tooenable replikációs table1, table2 és Tábl3 hello:</span><span class="sxs-lookup"><span data-stu-id="d70ef-244">Use hello following parameters tooenable replication on table1, table2, and table3:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- <span data-ttu-id="d70ef-245">**A megadott táblák replikálás engedélyezése és hello meglévő adatok másolása**.</span><span class="sxs-lookup"><span data-stu-id="d70ef-245">**Enable replication on specific tables and copy hello existing data**.</span></span> <span data-ttu-id="d70ef-246">Használja a következő paraméterek tooenable replikációs table1, table2 és Tábl3 hello:</span><span class="sxs-lookup"><span data-stu-id="d70ef-246">Use hello following parameters tooenable replication on table1, table2, and table3:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- <span data-ttu-id="d70ef-247">**A phoenix metaadatok replikálásához forrás toodestination összes táblán a replikáció engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="d70ef-247">**Enable replication on all tables with replicating phoenix metadata from source toodestination**.</span></span> <span data-ttu-id="d70ef-248">Phoenix metaadatok replikációs nincs tökéletes megoldás, és engedélyezve legyenek-e figyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="d70ef-248">Phoenix metadata replication is not perfect and should be enabled with caution.</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a><span data-ttu-id="d70ef-249">Másolja, majd az adatok áttelepítése</span><span class="sxs-lookup"><span data-stu-id="d70ef-249">Copy and migrate data</span></span>

<span data-ttu-id="d70ef-250">Két külön parancsfájl művelet parancsprogramok másolása/történő áttelepítésére vonatkozó adatok a replikáció engedélyezése után:</span><span class="sxs-lookup"><span data-stu-id="d70ef-250">There are two separate script action scripts for copying/migrating data after replication is enabled:</span></span>

- <span data-ttu-id="d70ef-251">[Kis táblák parancsfájl](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (néhány gigabájttól méret és a teljes másolás várt toofinish kisebb, mint egy óra)</span><span class="sxs-lookup"><span data-stu-id="d70ef-251">[Script for small tables](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (a few gigabytes in size, and overall copy is expected toofinish in less than one hour)</span></span>

- <span data-ttu-id="d70ef-252">[Nagy méretű táblákra vonatkozó parancsfájl](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (várt hosszabb, mint egy órával toocopy tootake)</span><span class="sxs-lookup"><span data-stu-id="d70ef-252">[Script for large tables](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (expected tootake longer than one hour toocopy)</span></span>

<span data-ttu-id="d70ef-253">Kövesse ugyanezt az eljárást a hello [Replikálásengedélyezési](#enable-replication) toocall hello parancsfájl a művelet a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="d70ef-253">You can follow hello same procedure in [Enable replication](#enable-replication) toocall hello script action with hello following parameters:</span></span>

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

<span data-ttu-id="d70ef-254">hello print_usage() szakasza hello [parancsfájl](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) paraméterek részletes leírását.</span><span class="sxs-lookup"><span data-stu-id="d70ef-254">hello print_usage() section of hello [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) provides a detailed description of parameters.</span></span>

### <a name="scenarios"></a><span data-ttu-id="d70ef-255">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="d70ef-255">Scenarios</span></span>

- <span data-ttu-id="d70ef-256">**Adott táblákra (Teszt1 Teszt2 és Teszt3) másolja az összes sorát, vége: jelenleg szerkesztett (aktuális időbélyeg)**:</span><span class="sxs-lookup"><span data-stu-id="d70ef-256">**Copy specific tables (test1, test2, and test3) for all rows edited till now (current time stamp)**:</span></span>

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  <span data-ttu-id="d70ef-257">vagy</span><span class="sxs-lookup"><span data-stu-id="d70ef-257">or</span></span>

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- <span data-ttu-id="d70ef-258">**Másolja át a megadott időtartomány adott táblák**:</span><span class="sxs-lookup"><span data-stu-id="d70ef-258">**Copy specific tables with specified time range**:</span></span>

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a><span data-ttu-id="d70ef-259">Tiltsa le a replikációt</span><span class="sxs-lookup"><span data-stu-id="d70ef-259">Disable replication</span></span>

<span data-ttu-id="d70ef-260">toodisable replikációs parancsfájllal egy másik parancsfájl művelet található [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh).</span><span class="sxs-lookup"><span data-stu-id="d70ef-260">toodisable replication, you use another script action script located at [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh).</span></span> <span data-ttu-id="d70ef-261">Kövesse ugyanezt az eljárást a hello [Replikálásengedélyezési](#enable-replication) toocall hello parancsfájl a művelet a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="d70ef-261">You can follow hello same procedure in [Enable replication](#enable-replication) toocall hello script action with hello following parameters:</span></span>

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari Password> <-all|-t "table1;table2;...">  

<span data-ttu-id="d70ef-262">hello print_usage() szakasza hello [parancsfájl](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) paraméterek részletesen ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d70ef-262">hello print_usage() section of hello [script](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) provides a detailed explanation of parameters.</span></span>

### <a name="scenarios"></a><span data-ttu-id="d70ef-263">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="d70ef-263">Scenarios</span></span>

- <span data-ttu-id="d70ef-264">**Tiltsa le a replikációt minden tábla**:</span><span class="sxs-lookup"><span data-stu-id="d70ef-264">**Disable replication on all tables**:</span></span>

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  <span data-ttu-id="d70ef-265">vagy</span><span class="sxs-lookup"><span data-stu-id="d70ef-265">or</span></span>

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari username> --src-ambari-password=<source cluster Ambari password>

- <span data-ttu-id="d70ef-266">**Tiltsa le a replikációt a megadott táblák (table1 table2 és Tábl3)**:</span><span class="sxs-lookup"><span data-stu-id="d70ef-266">**Disable replication on specified tables (table1, table2, and table3)**:</span></span>

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a><span data-ttu-id="d70ef-267">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d70ef-267">Next steps</span></span>

<span data-ttu-id="d70ef-268">Ebben az oktatóanyagban, megtudta, hogyan tooconfigure HBase-replikálás két adatközpont között.</span><span class="sxs-lookup"><span data-stu-id="d70ef-268">In this tutorial, you learned how tooconfigure HBase replication across two datacenters.</span></span> <span data-ttu-id="d70ef-269">További információk a HDInsight és HBase, toolearn lásd:</span><span class="sxs-lookup"><span data-stu-id="d70ef-269">toolearn more about HDInsight and HBase, see:</span></span>

* <span data-ttu-id="d70ef-270">[Ismerkedés az Apache HBase a hdinsight eszközben][hdinsight-hbase-get-started]</span><span class="sxs-lookup"><span data-stu-id="d70ef-270">[Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started]</span></span>
* <span data-ttu-id="d70ef-271">[HDInsight HBase áttekintése][hdinsight-hbase-overview]</span><span class="sxs-lookup"><span data-stu-id="d70ef-271">[HDInsight HBase overview][hdinsight-hbase-overview]</span></span>
* <span data-ttu-id="d70ef-272">[Hozzon létre HBase-fürtökkel Azure virtuális hálózat][hdinsight-hbase-provision-vnet]</span><span class="sxs-lookup"><span data-stu-id="d70ef-272">[Create HBase clusters in Azure Virtual Network][hdinsight-hbase-provision-vnet]</span></span>
* <span data-ttu-id="d70ef-273">[A hbase eszközzel valós idejű Twitter sentiment elemzése][hdinsight-hbase-twitter-sentiment]</span><span class="sxs-lookup"><span data-stu-id="d70ef-273">[Analyze real-time Twitter sentiment with HBase][hdinsight-hbase-twitter-sentiment]</span></span>
* <span data-ttu-id="d70ef-274">[Érzékelőadatok elemzése a Storm és HBase a HDInsight (Hadoop)][hdinsight-sensor-data]</span><span class="sxs-lookup"><span data-stu-id="d70ef-274">[Analyzing sensor data with Storm and HBase in HDInsight (Hadoop)][hdinsight-sensor-data]</span></span>

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
