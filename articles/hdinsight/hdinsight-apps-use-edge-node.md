---
title: "Üres peremhálózati csomópontok használata a hdinsight - Azure Hadoop-fürtök |} Microsoft Docs"
description: "Hogyan adhat hozzá egy üres élcsomópontot ügyfélként is használható, amely majd vizsgálati és a gazdagép a HDInsight-alkalmazások HDInsight-fürtöt."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: 
ms.assetid: cdc7d1b4-15d7-4d4d-a13f-c7d3a694b4fb
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jgao
ms.openlocfilehash: e21dabcc6999b1f1047d334e782f723d0c03c2cb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-empty-edge-nodes-on-hadoop-clusters-in-hdinsight"></a><span data-ttu-id="36637-103">Üres peremhálózati csomópontok használata a hdinsight Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="36637-103">Use empty edge nodes on Hadoop clusters in HDInsight</span></span>

<span data-ttu-id="36637-104">Ismerje meg, egy üres élcsomópontot felvétele a HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="36637-104">Learn how to add an empty edge node to an HDInsight cluster.</span></span> <span data-ttu-id="36637-105">Egy üres élcsomópontot ügyfél eszközök telepítése és konfigurálása, mint a headnodes, de nincs Hadoop-szolgáltatás fut a Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="36637-105">An empty edge node is a Linux virtual machine with the same client tools installed and configured as in the headnodes, but with no Hadoop services running.</span></span> <span data-ttu-id="36637-106">A fürt eléréséhez, az ügyfél alkalmazások tesztelése és az ügyfélalkalmazások üzemeltető élcsomópont is használhatja.</span><span class="sxs-lookup"><span data-stu-id="36637-106">You can use the edge node for accessing the cluster, testing your client applications, and hosting your client applications.</span></span> 

<span data-ttu-id="36637-107">Egy üres élcsomópontot is hozzáadhat egy meglévő HDInsight-fürthöz az új fürtre a fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="36637-107">You can add an empty edge node to an existing HDInsight cluster, to a new cluster when you create the cluster.</span></span> <span data-ttu-id="36637-108">Történik, egy üres élcsomópontot hozzáadása Azure Resource Manager-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="36637-108">Adding an empty edge node is done using Azure Resource Manager template.</span></span>  <span data-ttu-id="36637-109">A következő példa bemutatja, hogyan történik a sablon használatával:</span><span class="sxs-lookup"><span data-stu-id="36637-109">The following sample demonstrates how it is done using a template:</span></span>

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [ "[concat('Microsoft.HDInsight/clusters/',parameters('clusterName'))]" ],
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "Standard_D3"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "[parameters('installScriptAction')]",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

<span data-ttu-id="36637-110">Mintában látható módon a, opcionálisan hívása egy [parancsfájl-művelet](hdinsight-hadoop-customize-cluster-linux.md) további konfigurációra, mint például telepítése [Apache Hue](hdinsight-hadoop-hue-linux.md) peremhálózati csomópontjában.</span><span class="sxs-lookup"><span data-stu-id="36637-110">As shown in the sample, you can optionally call a [script action](hdinsight-hadoop-customize-cluster-linux.md) to perform additional configuration, such as installing [Apache Hue](hdinsight-hadoop-hue-linux.md) in the edge node.</span></span> <span data-ttu-id="36637-111">A parancsfájl parancsfájlművelet nyilvánosan elérhető a weben kell lennie.</span><span class="sxs-lookup"><span data-stu-id="36637-111">The script action script must be publicly accessible on the web.</span></span>  <span data-ttu-id="36637-112">Például ha a parancsfájl az Azure-tárfiókba, használja a nyilvános tárolók vagy nyilvános blobok.</span><span class="sxs-lookup"><span data-stu-id="36637-112">For example, if the script is stored in Azure storage, use either public containers or public blobs.</span></span>

<span data-ttu-id="36637-113">A peremhálózati csomópont virtuálisgép-méret meg kell felelnie a HDInsight fürt munkavégző csomópont virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="36637-113">The edge node virtual machine size must meet the HDInsight cluster worker node vm size requirements.</span></span> <span data-ttu-id="36637-114">Az ajánlott munkavégző csomópont virtuálisgép-méretek, lásd: [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="36637-114">For the recommended worker node vm sizes, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>

<span data-ttu-id="36637-115">Miután létrehozott egy élcsomópontot, csatlakozás az élcsomóponthoz SSH használatával, és futtassa a hdinsight Hadoop-fürt eléréséhez ügyféleszközök elől.</span><span class="sxs-lookup"><span data-stu-id="36637-115">After you have created an edge node, you can connect to the edge node using SSH, and run client tools to access the Hadoop cluster in HDInsight.</span></span>

> [!WARNING] 
> <span data-ttu-id="36637-116">Egy üres élcsomópontot használata a HDInsight jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="36637-116">Using an empty edge node with HDInsight is currently in preview.</span></span> <span data-ttu-id="36637-117">Egyéni összetevők élcsomópont telepített minden üzleti szempontból ésszerű támogatást kaphatnak a Microsofttól.</span><span class="sxs-lookup"><span data-stu-id="36637-117">Custom components that are installed on the edge node receive commercially reasonable support from Microsoft.</span></span> <span data-ttu-id="36637-118">Emiatt előfordulhat, hogy a felmerülő problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="36637-118">This might result in resolving problems you encounter.</span></span> <span data-ttu-id="36637-119">Vagy, ha további segítségre van szüksége a közösségi források lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="36637-119">Or you may be referred to community resources for further assistance.</span></span> <span data-ttu-id="36637-120">Az alábbiakban a segítségkérés a Közösségtől a legtöbb aktív helyek:</span><span class="sxs-lookup"><span data-stu-id="36637-120">The following are some of the most active sites for getting help from the community:</span></span>
>
> * [<span data-ttu-id="36637-121">A HDInsight MSDN fórum</span><span class="sxs-lookup"><span data-stu-id="36637-121">MSDN forum for HDInsight</span></span>](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)
> * <span data-ttu-id="36637-122">[http://stackoverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="36637-122">[http://stackoverflow.com](http://stackoverflow.com).</span></span>
>
> <span data-ttu-id="36637-123">Az Apache-okat használ, ha esetleg találni az Apache keresztül projekt helyek [http://apache.org](http://apache.org), például a [Hadoop](http://hadoop.apache.org/) hely.</span><span class="sxs-lookup"><span data-stu-id="36637-123">If you are using an Apache technology, you may be able to find assistance through the Apache project sites on [http://apache.org](http://apache.org), such as the [Hadoop](http://hadoop.apache.org/) site.</span></span>

## <a name="add-an-edge-node-to-an-existing-cluster"></a><span data-ttu-id="36637-124">Meglévő fürt egy élcsomópontot hozzáadása</span><span class="sxs-lookup"><span data-stu-id="36637-124">Add an edge node to an existing cluster</span></span>
<span data-ttu-id="36637-125">Ebben a szakaszban egy Resource Manager-sablon egy élcsomópontot hozzáadása egy meglévő HDInsight-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="36637-125">In this section, you use a Resource Manager template to add an edge node to an existing HDInsight cluster.</span></span>  <span data-ttu-id="36637-126">A Resource Manager-sablon található [GitHub](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/).</span><span class="sxs-lookup"><span data-stu-id="36637-126">The Resource Manager template can be found in [GitHub](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/).</span></span> <span data-ttu-id="36637-127">A Resource Manager-sablon meghívja a parancsfájlművelet https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh helyen. A parancsfájl nem más műveletet végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="36637-127">The Resource Manager template calls a script action located at https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. The script doesn't perform any actions.</span></span>  <span data-ttu-id="36637-128">A Resource Manager-sablon a hívó parancsfájlművelet bemutatásához.</span><span class="sxs-lookup"><span data-stu-id="36637-128">It is to demonstrate calling script action from a Resource Manager template.</span></span>

<span data-ttu-id="36637-129">**Egy üres élcsomópontot hozzáadása egy meglévő fürthöz**</span><span class="sxs-lookup"><span data-stu-id="36637-129">**To add an empty edge node to an existing cluster**</span></span>

1. <span data-ttu-id="36637-130">HDInsight-fürtök létrehozása, ha még nincs ilyen.</span><span class="sxs-lookup"><span data-stu-id="36637-130">Create an HDInsight cluster if you don't have one yet.</span></span>  <span data-ttu-id="36637-131">Lásd: [Hadoop oktatóanyag: Hadoop használatának megkezdésében a HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="36637-131">See [Hadoop tutorial: Get started using Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
2. <span data-ttu-id="36637-132">Kattintson az alábbi képre kattintva jelentkezzen be az Azure-ba, és nyissa meg az Azure Resource Manager-sablon az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="36637-132">Click the following image to sign in to Azure and open the Azure Resource Manager template in the Azure portal.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-add-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy to Azure"></a>
3. <span data-ttu-id="36637-133">Konfigurálja a következő tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="36637-133">Configure the following properties:</span></span>
   
   * <span data-ttu-id="36637-134">**Előfizetés**: válassza ki a fürt létrehozásához használt Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="36637-134">**Subscription**: Select an Azure subscription used for creating the cluster.</span></span>
   * <span data-ttu-id="36637-135">**Erőforráscsoport**: válasszon a meglévő HDInsight-fürt erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="36637-135">**Resource group**: Select the resource group used for the existing HDInsight cluster.</span></span>
   * <span data-ttu-id="36637-136">**Hely**: válassza ki azt a helyet a meglévő HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="36637-136">**Location**: Select the location of the existing HDInsight cluster.</span></span>
   * <span data-ttu-id="36637-137">**A fürt neve**: Adja meg egy meglévő HDInsight-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="36637-137">**Cluster Name**: Enter the name of an existing HDInsight cluster.</span></span>
   * <span data-ttu-id="36637-138">**A szegély csomópont méretének**: válassza ki a Virtuálisgép-méretek.</span><span class="sxs-lookup"><span data-stu-id="36637-138">**Edge Node Size**: Select one of the VM sizes.</span></span> <span data-ttu-id="36637-139">A virtuálisgép-méretet meg kell felelnie a munkavégző csomópont virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="36637-139">The vm size must meet the worker node vm size requirements.</span></span> <span data-ttu-id="36637-140">Az ajánlott munkavégző csomópont virtuálisgép-méretek, lásd: [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="36637-140">For the recommended worker node vm sizes, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>
   * <span data-ttu-id="36637-141">**A szegély csomópont előtag**: az alapértelmezett érték **új**.</span><span class="sxs-lookup"><span data-stu-id="36637-141">**Edge Node Prefix**: The default value is **new**.</span></span>  <span data-ttu-id="36637-142">Az alapértelmezett érték, a peremhálózati csomópontnév használata **új edgenode**.</span><span class="sxs-lookup"><span data-stu-id="36637-142">Using the default value, the edge node name is **new-edgenode**.</span></span>  <span data-ttu-id="36637-143">Testre szabhatja az előtag a portálról.</span><span class="sxs-lookup"><span data-stu-id="36637-143">You can customize the prefix from the portal.</span></span> <span data-ttu-id="36637-144">Testre szabhatja a teljes nevet, a sablon alapján.</span><span class="sxs-lookup"><span data-stu-id="36637-144">You can also customize the full name from the template.</span></span>

4. <span data-ttu-id="36637-145">Ellenőrizze **elfogadom a feltételeket és a fenti feltételek**, és kattintson a **beszerzési** élcsomópont létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="36637-145">Check **I agree to the terms and conditions stated above**, and then click  **Purchase** to create the edge node.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="36637-146">Ügyeljen arra, hogy a meglévő HDInsight-fürthöz az Azure erőforráscsoport kiválasztását.</span><span class="sxs-lookup"><span data-stu-id="36637-146">Make sure to select the Azure resource group for the existing HDInsight cluster.</span></span>  <span data-ttu-id="36637-147">Ellenkező esetben a következő üzenet jelenik "nem hajtható végre a kért művelet beágyazott erőforráson.</span><span class="sxs-lookup"><span data-stu-id="36637-147">Otherwise, you get the error message "Can not perform requested operation on nested resource.</span></span> <span data-ttu-id="36637-148">Szülő erőforrás "&lt;ClusterName >" nem található. "</span><span class="sxs-lookup"><span data-stu-id="36637-148">Parent resource '&lt;ClusterName>' not found."</span></span>

## <a name="add-an-edge-node-when-creating-a-cluster"></a><span data-ttu-id="36637-149">Adja hozzá egy élcsomópontot, ha a fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="36637-149">Add an edge node when creating a cluster</span></span>
<span data-ttu-id="36637-150">Ebben a szakaszban a Resource Manager-sablon létrehozása a HDInsight-fürt egy élcsomópontot a használhatja.</span><span class="sxs-lookup"><span data-stu-id="36637-150">In this section, you use a Resource Manager template to create HDInsight cluster with an edge node.</span></span>  <span data-ttu-id="36637-151">A Resource Manager-sablon megtalálható a [Azure gyors üzembe helyezés sablontárban](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/).</span><span class="sxs-lookup"><span data-stu-id="36637-151">The Resource Manager template can be found in the [Azure QuickStart Templates gallery](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/).</span></span> <span data-ttu-id="36637-152">A Resource Manager-sablon meghívja a parancsfájlművelet https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh helyen. A parancsfájl nem más műveletet végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="36637-152">The Resource Manager template calls a script action located at https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. The script doesn't perform any actions.</span></span>  <span data-ttu-id="36637-153">A Resource Manager-sablon a hívó parancsfájlművelet bemutatásához.</span><span class="sxs-lookup"><span data-stu-id="36637-153">It is to demonstrate calling script action from a Resource Manager template.</span></span>

<span data-ttu-id="36637-154">**Egy üres élcsomópontot hozzáadása egy meglévő fürthöz**</span><span class="sxs-lookup"><span data-stu-id="36637-154">**To add an empty edge node to an existing cluster**</span></span>

1. <span data-ttu-id="36637-155">HDInsight-fürtök létrehozása, ha még nincs ilyen.</span><span class="sxs-lookup"><span data-stu-id="36637-155">Create an HDInsight cluster if you don't have one yet.</span></span>  <span data-ttu-id="36637-156">Lásd: [Hadoop használatának megkezdésében a HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="36637-156">See [Get started using Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
2. <span data-ttu-id="36637-157">Kattintson az alábbi képre kattintva jelentkezzen be az Azure-ba, és nyissa meg az Azure Resource Manager-sablon az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="36637-157">Click the following image to sign in to Azure and open the Azure Resource Manager template in the Azure portal.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy to Azure"></a>
3. <span data-ttu-id="36637-158">Konfigurálja a következő tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="36637-158">Configure the following properties:</span></span>
   
   * <span data-ttu-id="36637-159">**Előfizetés**: válassza ki a fürt létrehozásához használt Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="36637-159">**Subscription**: Select an Azure subscription used for creating the cluster.</span></span>
   * <span data-ttu-id="36637-160">**Erőforráscsoport**: hozzon létre egy új erőforráscsoportot a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="36637-160">**Resource group**: Create a new resource group used for the cluster.</span></span>
   * <span data-ttu-id="36637-161">**Hely**: Válasszon egy helyet az erőforráscsoportnak.</span><span class="sxs-lookup"><span data-stu-id="36637-161">**Location**: Select a location for the resource group.</span></span>
   * <span data-ttu-id="36637-162">**A fürt neve**: Adjon meg egy nevet az új fürt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="36637-162">**Cluster Name**: Enter a name for the new cluster to create.</span></span>
   * <span data-ttu-id="36637-163">**A fürt bejelentkezési felhasználónevét**: a Hadoop HTTP felhasználónevet adja meg.</span><span class="sxs-lookup"><span data-stu-id="36637-163">**Cluster Login User Name**: Enter the Hadoop HTTP user name.</span></span>  <span data-ttu-id="36637-164">Az alapértelmezett név az **admin**.</span><span class="sxs-lookup"><span data-stu-id="36637-164">The default name is **admin**.</span></span>
   * <span data-ttu-id="36637-165">**A fürt bejelentkezési jelszó**: a Hadoop HTTP felhasználói jelszó.</span><span class="sxs-lookup"><span data-stu-id="36637-165">**Cluster Login Password**: Enter the Hadoop HTTP user password.</span></span>
   * <span data-ttu-id="36637-166">**Ssh felhasználónév**: Adja meg az SSH-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="36637-166">**Ssh User Name**: Enter the SSH user name.</span></span> <span data-ttu-id="36637-167">Az alapértelmezett név az **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="36637-167">The default name is **sshuser**.</span></span>
   * <span data-ttu-id="36637-168">**Ssh jelszó**: Adja meg az SSH-felhasználó jelszót.</span><span class="sxs-lookup"><span data-stu-id="36637-168">**Ssh Password**: Enter the SSH user password.</span></span>
   * <span data-ttu-id="36637-169">**Telepítse a parancsfájlművelet**: tartsa meg az oktatóanyag lépéseinek az alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="36637-169">**Install Script Action**: Keep the default value for going through this tutorial.</span></span>
     
     <span data-ttu-id="36637-170">Néhány tulajdonság lett volna a sablonban szoftveresen kötött: fürt típusa, a fürt feldolgozó csomópontok száma, a peremhálózati csomópont mérete és a peremhálózati csomópont neve.</span><span class="sxs-lookup"><span data-stu-id="36637-170">Some properties have been hardcoded in the template: Cluster type, Cluster worker node count, Edge node size, and Edge node name.</span></span>
4. <span data-ttu-id="36637-171">Ellenőrizze **elfogadom a feltételeket és a fenti feltételek**, és kattintson a **beszerzési** élcsomópont a fürt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="36637-171">Check **I agree to the terms and conditions stated above**, and then click  **Purchase** to create the cluster with the edge node.</span></span>

## <a name="access-an-edge-node"></a><span data-ttu-id="36637-172">Hozzáférés egy élcsomópontot</span><span class="sxs-lookup"><span data-stu-id="36637-172">Access an edge node</span></span>
<span data-ttu-id="36637-173">Élcsomópont ssh-végpont esetében &lt;EdgeNodeName >.&lt; ClusterName >-ssh.azurehdinsight.net:22.</span><span class="sxs-lookup"><span data-stu-id="36637-173">The edge node ssh endpoint is &lt;EdgeNodeName>.&lt;ClusterName>-ssh.azurehdinsight.net:22.</span></span>  <span data-ttu-id="36637-174">Például új-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.</span><span class="sxs-lookup"><span data-stu-id="36637-174">For example, new-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.</span></span>

<span data-ttu-id="36637-175">Élcsomópont egy alkalmazást az Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="36637-175">The edge node appears as an application on the Azure portal.</span></span>  <span data-ttu-id="36637-176">A portál jelenít meg az adatokat a élcsomópontjához SSH eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="36637-176">The portal gives you the information to access the edge node using SSH.</span></span>

<span data-ttu-id="36637-177">**A peremhálózati csomópont SSH végpont ellenőrzése**</span><span class="sxs-lookup"><span data-stu-id="36637-177">**To verify the edge node SSH endpoint**</span></span>

1. <span data-ttu-id="36637-178">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="36637-178">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="36637-179">Nyissa meg a HDInsight-fürt egy élcsomópontot.</span><span class="sxs-lookup"><span data-stu-id="36637-179">Open the HDInsight cluster with an edge node.</span></span>
3. <span data-ttu-id="36637-180">Kattintson a **alkalmazások** a fürt paneljén.</span><span class="sxs-lookup"><span data-stu-id="36637-180">Click **Applications** from the cluster blade.</span></span> <span data-ttu-id="36637-181">Ekkor megjelenik az élcsomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="36637-181">You shall see the edge node.</span></span>  <span data-ttu-id="36637-182">Az alapértelmezett név az **új edgenode**.</span><span class="sxs-lookup"><span data-stu-id="36637-182">The default name is **new-edgenode**.</span></span>
4. <span data-ttu-id="36637-183">Kattintson az élcsomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="36637-183">Click the edge node.</span></span> <span data-ttu-id="36637-184">Ekkor megjelenik az SSH-végpontot.</span><span class="sxs-lookup"><span data-stu-id="36637-184">You shall see the SSH endpoint.</span></span>

<span data-ttu-id="36637-185">**A Hive használata a peremhálózati csomóponton**</span><span class="sxs-lookup"><span data-stu-id="36637-185">**To use Hive on the edge node**</span></span>

1. <span data-ttu-id="36637-186">Az SSH használata a csatlakozás az élcsomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="36637-186">Use SSH to connect to the edge node.</span></span> <span data-ttu-id="36637-187">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="36637-187">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="36637-188">Az SSH használatával élcsomóponthoz csatlakoztatása után a Hive-konzolt használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="36637-188">After you have connected to the edge node using SSH, use the following command to open the Hive console:</span></span>
   
        hive
3. <span data-ttu-id="36637-189">A következő parancsot a fürt megjelenítése a Hive táblák:</span><span class="sxs-lookup"><span data-stu-id="36637-189">Run the following command to show Hive tables in the cluster:</span></span>
   
        show tables;

## <a name="delete-an-edge-node"></a><span data-ttu-id="36637-190">Egy élcsomópontot törlése</span><span class="sxs-lookup"><span data-stu-id="36637-190">Delete an edge node</span></span>
<span data-ttu-id="36637-191">Azure-portálról egy élcsomópontot törölheti.</span><span class="sxs-lookup"><span data-stu-id="36637-191">You can delete an edge node from the Azure portal.</span></span>

<span data-ttu-id="36637-192">**Egy élcsomópontot eléréséhez**</span><span class="sxs-lookup"><span data-stu-id="36637-192">**To access an edge node**</span></span>

1. <span data-ttu-id="36637-193">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="36637-193">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="36637-194">Nyissa meg a HDInsight-fürt egy élcsomópontot.</span><span class="sxs-lookup"><span data-stu-id="36637-194">Open the HDInsight cluster with an edge node.</span></span>
3. <span data-ttu-id="36637-195">Kattintson a **alkalmazások** a fürt paneljén.</span><span class="sxs-lookup"><span data-stu-id="36637-195">Click **Applications** from the cluster blade.</span></span> <span data-ttu-id="36637-196">Peremhálózati lista megjelenik.</span><span class="sxs-lookup"><span data-stu-id="36637-196">You shall see a list of edge nodes.</span></span>  
4. <span data-ttu-id="36637-197">Kattintson a jobb gombbal az élcsomóponthoz, törlése, és kattintson a kívánt **törlése**.</span><span class="sxs-lookup"><span data-stu-id="36637-197">Right-click the edge node you want to delete, and then click **Delete**.</span></span>
5. <span data-ttu-id="36637-198">Kattintson a **Yes** (Igen) gombra a megerősítéshez.</span><span class="sxs-lookup"><span data-stu-id="36637-198">Click **Yes** to confirm.</span></span>

## <a name="next-steps"></a><span data-ttu-id="36637-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="36637-199">Next steps</span></span>
<span data-ttu-id="36637-200">Ebben a cikkben megtanulhatta, egy élcsomópontot hozzáadása és az élcsomóponthoz elérését.</span><span class="sxs-lookup"><span data-stu-id="36637-200">In this article, you have learned how to add an edge node and how to access the edge node.</span></span> <span data-ttu-id="36637-201">További tudnivalókért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="36637-201">To learn more, see the following articles:</span></span>

* <span data-ttu-id="36637-202">[HDInsight-alkalmazások telepítése](hdinsight-apps-install-applications.md): Megtudhatja, hogyan telepíthet HDInsight-alkalmazásokat a fürtjeire.</span><span class="sxs-lookup"><span data-stu-id="36637-202">[Install HDInsight applications](hdinsight-apps-install-applications.md): Learn how to install an HDInsight application to your clusters.</span></span>
* <span data-ttu-id="36637-203">[Egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md): telepítése egy közzé nem tett HDInsight-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="36637-203">[Install custom HDInsight applications](hdinsight-apps-install-custom-applications.md): learn how to deploy an unpublished HDInsight application to HDInsight.</span></span>
* <span data-ttu-id="36637-204">[HDInsight-alkalmazások közzététele](hdinsight-apps-publish-applications.md): Megtudhatja, hogyan teheti közzé egyéni HDInsight-alkalmazásait az Azure Piactéren.</span><span class="sxs-lookup"><span data-stu-id="36637-204">[Publish HDInsight applications](hdinsight-apps-publish-applications.md): Learn how to publish your custom HDInsight applications to Azure Marketplace.</span></span>
* <span data-ttu-id="36637-205">[MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: HDInsight-alkalmazás telepítése): Megtudhatja, hogyan adhat meg HDInsight-alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="36637-205">[MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx): Learn how to define HDInsight applications.</span></span>
* <span data-ttu-id="36637-206">[Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md) (Linux-alapú HDInsight-fürtök testreszabása parancsfájlműveletek segítségével): megtudhatja, hogyan telepíthet további alkalmazásokat parancsfájlműveletek használatával.</span><span class="sxs-lookup"><span data-stu-id="36637-206">[Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md): learn how to use Script Action to install additional applications.</span></span>
* <span data-ttu-id="36637-207">[Create Linux-based Hadoop clusters in HDInsight using Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md) (Linux-alapú Hadoop-fürtök létrehozása a HDInsightban Resource Manager-sablonok segítségével): Megtudhatja, hogyan hívhat meg Resource Manager-sablonokat HDInsight-fürtök létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="36637-207">[Create Linux-based Hadoop clusters in HDInsight using Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md): learn how to call Resource Manager templates to create HDInsight clusters.</span></span>

