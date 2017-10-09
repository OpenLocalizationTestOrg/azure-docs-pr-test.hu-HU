---
title: "aaaUse üres Hadoop-fürtöket a HDInsight - Azure peremhálózati csomópontok |} Microsoft Docs"
description: "Hogyan tooadd egy üres biztonsági csomópont tooan HDInsight fürt, amely ügyfélként is használható, és ezután vizsgálati és a gazdagép a HDInsight-alkalmazások."
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
ms.openlocfilehash: 9c910905b51f2fe92e6e5d47d86a32bd5247c2cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-empty-edge-nodes-on-hadoop-clusters-in-hdinsight"></a><span data-ttu-id="2deca-103">Üres peremhálózati csomópontok használata a hdinsight Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="2deca-103">Use empty edge nodes on Hadoop clusters in HDInsight</span></span>

<span data-ttu-id="2deca-104">Ismerje meg, hogyan tooadd egy üres él csomópont tooan HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="2deca-104">Learn how tooadd an empty edge node tooan HDInsight cluster.</span></span> <span data-ttu-id="2deca-105">Egy üres élcsomópontot hello ugyanazon ügyfél-eszközök telepítve, és mint hello headnodes konfigurálva, de nem Hadoop-szolgáltatást futtató Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2deca-105">An empty edge node is a Linux virtual machine with hello same client tools installed and configured as in hello headnodes, but with no Hadoop services running.</span></span> <span data-ttu-id="2deca-106">Hello fürt eléréséhez, az ügyfél alkalmazások tesztelése és az ügyfélalkalmazások üzemeltető hello élcsomópontot is használhatja.</span><span class="sxs-lookup"><span data-stu-id="2deca-106">You can use hello edge node for accessing hello cluster, testing your client applications, and hosting your client applications.</span></span> 

<span data-ttu-id="2deca-107">Hozzáadhat egy üres biztonsági csomópont tooan meglévő HDInsight-fürtre, tooa új fürt hello fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="2deca-107">You can add an empty edge node tooan existing HDInsight cluster, tooa new cluster when you create hello cluster.</span></span> <span data-ttu-id="2deca-108">Történik, egy üres élcsomópontot hozzáadása Azure Resource Manager-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="2deca-108">Adding an empty edge node is done using Azure Resource Manager template.</span></span>  <span data-ttu-id="2deca-109">hello következő példa bemutatja, hogyan történik a sablon használatával:</span><span class="sxs-lookup"><span data-stu-id="2deca-109">hello following sample demonstrates how it is done using a template:</span></span>

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

<span data-ttu-id="2deca-110">Mintában látható módon hello, opcionálisan hívása egy [parancsfájl-művelet](hdinsight-hadoop-customize-cluster-linux.md) tooperform további konfigurálást, mint például telepítése [Apache Hue](hdinsight-hadoop-hue-linux.md) hello peremhálózati csomópontban.</span><span class="sxs-lookup"><span data-stu-id="2deca-110">As shown in hello sample, you can optionally call a [script action](hdinsight-hadoop-customize-cluster-linux.md) tooperform additional configuration, such as installing [Apache Hue](hdinsight-hadoop-hue-linux.md) in hello edge node.</span></span> <span data-ttu-id="2deca-111">hello parancsfájl parancsfájlművelet nyilvánosan elérhető hello weben kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2deca-111">hello script action script must be publicly accessible on hello web.</span></span>  <span data-ttu-id="2deca-112">Például ha az Azure storage hello parancsfájl, használja a nyilvános tárolók vagy nyilvános blobok.</span><span class="sxs-lookup"><span data-stu-id="2deca-112">For example, if hello script is stored in Azure storage, use either public containers or public blobs.</span></span>

<span data-ttu-id="2deca-113">hello peremhálózati csomópont virtuális gép méretének meg kell felelnie az hello HDInsight fürt munkavégző csomópont virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2deca-113">hello edge node virtual machine size must meet hello HDInsight cluster worker node vm size requirements.</span></span> <span data-ttu-id="2deca-114">Hello ajánlott munkavégző csomópont virtuálisgép-méretek, lásd: [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="2deca-114">For hello recommended worker node vm sizes, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>

<span data-ttu-id="2deca-115">Miután létrehozott egy élcsomópontot, toohello élcsomópontjához SSH csatlakozzon, és futtassa az ügyfelet eszközök tooaccess hello Hadoop-fürt hdinsightban.</span><span class="sxs-lookup"><span data-stu-id="2deca-115">After you have created an edge node, you can connect toohello edge node using SSH, and run client tools tooaccess hello Hadoop cluster in HDInsight.</span></span>

> [!WARNING] 
> <span data-ttu-id="2deca-116">Egy üres élcsomópontot használata a HDInsight jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="2deca-116">Using an empty edge node with HDInsight is currently in preview.</span></span> <span data-ttu-id="2deca-117">Egyéni összetevők hello élcsomópont telepített minden üzleti szempontból ésszerű támogatást kaphatnak a Microsofttól.</span><span class="sxs-lookup"><span data-stu-id="2deca-117">Custom components that are installed on hello edge node receive commercially reasonable support from Microsoft.</span></span> <span data-ttu-id="2deca-118">Emiatt előfordulhat, hogy a felmerülő problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="2deca-118">This might result in resolving problems you encounter.</span></span> <span data-ttu-id="2deca-119">Vagy előfordulhat, hogy a hivatkozott toocommunity erőforrások további segítségért.</span><span class="sxs-lookup"><span data-stu-id="2deca-119">Or you may be referred toocommunity resources for further assistance.</span></span> <span data-ttu-id="2deca-120">hello közé tartoznak a következők a legtöbb aktív helyek kapcsolódnak a Súgó hello Közösségtől hello:</span><span class="sxs-lookup"><span data-stu-id="2deca-120">hello following are some of hello most active sites for getting help from hello community:</span></span>
>
> * [<span data-ttu-id="2deca-121">A HDInsight MSDN fórum</span><span class="sxs-lookup"><span data-stu-id="2deca-121">MSDN forum for HDInsight</span></span>](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)
> * <span data-ttu-id="2deca-122">[http://stackoverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="2deca-122">[http://stackoverflow.com](http://stackoverflow.com).</span></span>
>
> <span data-ttu-id="2deca-123">Ha egy Apache-okat használ, is képes toofind segítséget hello Apache projekt helyeket a [http://apache.org](http://apache.org), például a hello [Hadoop](http://hadoop.apache.org/) hely.</span><span class="sxs-lookup"><span data-stu-id="2deca-123">If you are using an Apache technology, you may be able toofind assistance through hello Apache project sites on [http://apache.org](http://apache.org), such as hello [Hadoop](http://hadoop.apache.org/) site.</span></span>

## <a name="add-an-edge-node-tooan-existing-cluster"></a><span data-ttu-id="2deca-124">Peremhálózati csomópont tooan meglévő fürt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2deca-124">Add an edge node tooan existing cluster</span></span>
<span data-ttu-id="2deca-125">Ebben a szakaszban egy Resource Manager sablon tooadd peremhálózati csomópont tooan meglévő HDInsight-fürtöt használ.</span><span class="sxs-lookup"><span data-stu-id="2deca-125">In this section, you use a Resource Manager template tooadd an edge node tooan existing HDInsight cluster.</span></span>  <span data-ttu-id="2deca-126">hello Resource Manager-sablon található [GitHub](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/).</span><span class="sxs-lookup"><span data-stu-id="2deca-126">hello Resource Manager template can be found in [GitHub](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/).</span></span> <span data-ttu-id="2deca-127">hello Resource Manager-sablon meghívja a parancsfájlművelet https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh helyen. hello parancsfájlok nem hajthatnak végre semmilyen műveletet.</span><span class="sxs-lookup"><span data-stu-id="2deca-127">hello Resource Manager template calls a script action located at https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. hello script doesn't perform any actions.</span></span>  <span data-ttu-id="2deca-128">A Resource Manager-sablon parancsfájlművelet előhívásának toodemonstrate.</span><span class="sxs-lookup"><span data-stu-id="2deca-128">It is toodemonstrate calling script action from a Resource Manager template.</span></span>

<span data-ttu-id="2deca-129">**üres peremhálózati csomópont tooan meglévő fürt tooadd**</span><span class="sxs-lookup"><span data-stu-id="2deca-129">**tooadd an empty edge node tooan existing cluster**</span></span>

1. <span data-ttu-id="2deca-130">HDInsight-fürtök létrehozása, ha még nincs ilyen.</span><span class="sxs-lookup"><span data-stu-id="2deca-130">Create an HDInsight cluster if you don't have one yet.</span></span>  <span data-ttu-id="2deca-131">Lásd: [Hadoop oktatóanyag: Hadoop használatának megkezdésében a HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2deca-131">See [Hadoop tutorial: Get started using Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
2. <span data-ttu-id="2deca-132">Kattintson a következő kép toosign tooAzure a és a nyitott hello Azure Resource Manager sablon hello Azure-portálon hello.</span><span class="sxs-lookup"><span data-stu-id="2deca-132">Click hello following image toosign in tooAzure and open hello Azure Resource Manager template in hello Azure portal.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-add-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy tooAzure"></a>
3. <span data-ttu-id="2deca-133">A következő tulajdonságok hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="2deca-133">Configure hello following properties:</span></span>
   
   * <span data-ttu-id="2deca-134">**Előfizetés**: válassza ki a hello fürt létrehozásához használt Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="2deca-134">**Subscription**: Select an Azure subscription used for creating hello cluster.</span></span>
   * <span data-ttu-id="2deca-135">**Erőforráscsoport**: hello meglévő HDInsight-fürthöz használt válassza hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="2deca-135">**Resource group**: Select hello resource group used for hello existing HDInsight cluster.</span></span>
   * <span data-ttu-id="2deca-136">**Hely**: Válasszon hello meglévő HDInsight-fürt hello helyet.</span><span class="sxs-lookup"><span data-stu-id="2deca-136">**Location**: Select hello location of hello existing HDInsight cluster.</span></span>
   * <span data-ttu-id="2deca-137">**A fürt neve**: Adjon meg egy meglévő HDInsight-fürt hello nevet.</span><span class="sxs-lookup"><span data-stu-id="2deca-137">**Cluster Name**: Enter hello name of an existing HDInsight cluster.</span></span>
   * <span data-ttu-id="2deca-138">**A szegély csomópont méretének**: Válasszon ki egy Virtuálisgép-méretek hello.</span><span class="sxs-lookup"><span data-stu-id="2deca-138">**Edge Node Size**: Select one of hello VM sizes.</span></span> <span data-ttu-id="2deca-139">Virtuálisgép-méretet hello meg kell felelnie az hello munkavégző csomópont virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2deca-139">hello vm size must meet hello worker node vm size requirements.</span></span> <span data-ttu-id="2deca-140">Hello ajánlott munkavégző csomópont virtuálisgép-méretek, lásd: [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="2deca-140">For hello recommended worker node vm sizes, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>
   * <span data-ttu-id="2deca-141">**A szegély csomópont előtag**: hello alapértelmezett értéke **új**.</span><span class="sxs-lookup"><span data-stu-id="2deca-141">**Edge Node Prefix**: hello default value is **new**.</span></span>  <span data-ttu-id="2deca-142">Hello alapértelmezett értéket, hello peremhálózati csomópontnév használata **új edgenode**.</span><span class="sxs-lookup"><span data-stu-id="2deca-142">Using hello default value, hello edge node name is **new-edgenode**.</span></span>  <span data-ttu-id="2deca-143">Testre szabhatja a hello előtag hello portálról.</span><span class="sxs-lookup"><span data-stu-id="2deca-143">You can customize hello prefix from hello portal.</span></span> <span data-ttu-id="2deca-144">Testre szabhatja hello teljes nevét hello sablonból.</span><span class="sxs-lookup"><span data-stu-id="2deca-144">You can also customize hello full name from hello template.</span></span>

4. <span data-ttu-id="2deca-145">Ellenőrizze **toohello feltételek és kikötések fenti elfogadom**, és kattintson a **beszerzési** toocreate hello élcsomópont.</span><span class="sxs-lookup"><span data-stu-id="2deca-145">Check **I agree toohello terms and conditions stated above**, and then click  **Purchase** toocreate hello edge node.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="2deca-146">Győződjön meg arról, hogy tooselect hello Azure erőforráscsoport hello meglévő HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="2deca-146">Make sure tooselect hello Azure resource group for hello existing HDInsight cluster.</span></span>  <span data-ttu-id="2deca-147">Ellenkező esetben akkor a hibaüzenet hello üzenet "nem hajtható végre a kért művelet beágyazott erőforráson.</span><span class="sxs-lookup"><span data-stu-id="2deca-147">Otherwise, you get hello error message "Can not perform requested operation on nested resource.</span></span> <span data-ttu-id="2deca-148">Szülő erőforrás "&lt;ClusterName >" nem található. "</span><span class="sxs-lookup"><span data-stu-id="2deca-148">Parent resource '&lt;ClusterName>' not found."</span></span>

## <a name="add-an-edge-node-when-creating-a-cluster"></a><span data-ttu-id="2deca-149">Adja hozzá egy élcsomópontot, ha a fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="2deca-149">Add an edge node when creating a cluster</span></span>
<span data-ttu-id="2deca-150">Ebben a szakaszban egy Resource Manager sablon toocreate HDInsight-fürt egy élcsomópontot és használja.</span><span class="sxs-lookup"><span data-stu-id="2deca-150">In this section, you use a Resource Manager template toocreate HDInsight cluster with an edge node.</span></span>  <span data-ttu-id="2deca-151">hello Resource Manager-sablon hello található [Azure gyors üzembe helyezés sablontárban](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/).</span><span class="sxs-lookup"><span data-stu-id="2deca-151">hello Resource Manager template can be found in hello [Azure QuickStart Templates gallery](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/).</span></span> <span data-ttu-id="2deca-152">hello Resource Manager-sablon meghívja a parancsfájlművelet https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh helyen. hello parancsfájlok nem hajthatnak végre semmilyen műveletet.</span><span class="sxs-lookup"><span data-stu-id="2deca-152">hello Resource Manager template calls a script action located at https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. hello script doesn't perform any actions.</span></span>  <span data-ttu-id="2deca-153">A Resource Manager-sablon parancsfájlművelet előhívásának toodemonstrate.</span><span class="sxs-lookup"><span data-stu-id="2deca-153">It is toodemonstrate calling script action from a Resource Manager template.</span></span>

<span data-ttu-id="2deca-154">**üres peremhálózati csomópont tooan meglévő fürt tooadd**</span><span class="sxs-lookup"><span data-stu-id="2deca-154">**tooadd an empty edge node tooan existing cluster**</span></span>

1. <span data-ttu-id="2deca-155">HDInsight-fürtök létrehozása, ha még nincs ilyen.</span><span class="sxs-lookup"><span data-stu-id="2deca-155">Create an HDInsight cluster if you don't have one yet.</span></span>  <span data-ttu-id="2deca-156">Lásd: [Hadoop használatának megkezdésében a HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2deca-156">See [Get started using Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
2. <span data-ttu-id="2deca-157">Kattintson a következő kép toosign tooAzure a és a nyitott hello Azure Resource Manager sablon hello Azure-portálon hello.</span><span class="sxs-lookup"><span data-stu-id="2deca-157">Click hello following image toosign in tooAzure and open hello Azure Resource Manager template in hello Azure portal.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy tooAzure"></a>
3. <span data-ttu-id="2deca-158">A következő tulajdonságok hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="2deca-158">Configure hello following properties:</span></span>
   
   * <span data-ttu-id="2deca-159">**Előfizetés**: válassza ki a hello fürt létrehozásához használt Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="2deca-159">**Subscription**: Select an Azure subscription used for creating hello cluster.</span></span>
   * <span data-ttu-id="2deca-160">**Erőforráscsoport**: hozzon létre egy új erőforráscsoportot hello fürt használja.</span><span class="sxs-lookup"><span data-stu-id="2deca-160">**Resource group**: Create a new resource group used for hello cluster.</span></span>
   * <span data-ttu-id="2deca-161">**Hely**: hello erőforráscsoport helyének kiválasztására.</span><span class="sxs-lookup"><span data-stu-id="2deca-161">**Location**: Select a location for hello resource group.</span></span>
   * <span data-ttu-id="2deca-162">**A fürt neve**: hello új fürt toocreate nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="2deca-162">**Cluster Name**: Enter a name for hello new cluster toocreate.</span></span>
   * <span data-ttu-id="2deca-163">**A fürt bejelentkezési felhasználónevét**: hello Hadoop HTTP-felhasználó nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="2deca-163">**Cluster Login User Name**: Enter hello Hadoop HTTP user name.</span></span>  <span data-ttu-id="2deca-164">hello alapértelmezés szerint ez **admin**.</span><span class="sxs-lookup"><span data-stu-id="2deca-164">hello default name is **admin**.</span></span>
   * <span data-ttu-id="2deca-165">**A fürt bejelentkezési jelszó**: hello Hadoop HTTP felhasználói jelszó.</span><span class="sxs-lookup"><span data-stu-id="2deca-165">**Cluster Login Password**: Enter hello Hadoop HTTP user password.</span></span>
   * <span data-ttu-id="2deca-166">**Ssh felhasználónév**: Adja meg a hello SSH-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="2deca-166">**Ssh User Name**: Enter hello SSH user name.</span></span> <span data-ttu-id="2deca-167">hello alapértelmezés szerint ez **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="2deca-167">hello default name is **sshuser**.</span></span>
   * <span data-ttu-id="2deca-168">**Ssh jelszó**: hello SSH felhasználói jelszó.</span><span class="sxs-lookup"><span data-stu-id="2deca-168">**Ssh Password**: Enter hello SSH user password.</span></span>
   * <span data-ttu-id="2deca-169">**Telepítse a parancsfájlművelet**: hello alapértelmezett értéke az oktatóanyag lépéseinek megtartása.</span><span class="sxs-lookup"><span data-stu-id="2deca-169">**Install Script Action**: Keep hello default value for going through this tutorial.</span></span>
     
     <span data-ttu-id="2deca-170">Néhány tulajdonság lett volna hello sablonban szoftveresen kötött: fürt típusa, a fürt feldolgozó csomópontok száma, a peremhálózati csomópont mérete és a peremhálózati csomópont neve.</span><span class="sxs-lookup"><span data-stu-id="2deca-170">Some properties have been hardcoded in hello template: Cluster type, Cluster worker node count, Edge node size, and Edge node name.</span></span>
4. <span data-ttu-id="2deca-171">Ellenőrizze **toohello feltételek és kikötések fenti elfogadom**, és kattintson a **beszerzési** toocreate hello fürt hello élcsomópont.</span><span class="sxs-lookup"><span data-stu-id="2deca-171">Check **I agree toohello terms and conditions stated above**, and then click  **Purchase** toocreate hello cluster with hello edge node.</span></span>

## <a name="access-an-edge-node"></a><span data-ttu-id="2deca-172">Hozzáférés egy élcsomópontot</span><span class="sxs-lookup"><span data-stu-id="2deca-172">Access an edge node</span></span>
<span data-ttu-id="2deca-173">hello élcsomópont ssh-végpont esetében &lt;EdgeNodeName >.&lt; ClusterName >-ssh.azurehdinsight.net:22.</span><span class="sxs-lookup"><span data-stu-id="2deca-173">hello edge node ssh endpoint is &lt;EdgeNodeName>.&lt;ClusterName>-ssh.azurehdinsight.net:22.</span></span>  <span data-ttu-id="2deca-174">Például új-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.</span><span class="sxs-lookup"><span data-stu-id="2deca-174">For example, new-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.</span></span>

<span data-ttu-id="2deca-175">hello élcsomópont hello Azure-portál alkalmazás jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2deca-175">hello edge node appears as an application on hello Azure portal.</span></span>  <span data-ttu-id="2deca-176">hello portál által biztosított információk tooaccess hello hello meg az SSH használatával csomópont él.</span><span class="sxs-lookup"><span data-stu-id="2deca-176">hello portal gives you hello information tooaccess hello edge node using SSH.</span></span>

<span data-ttu-id="2deca-177">**tooverify hello peremhálózati csomópont SSH végpont**</span><span class="sxs-lookup"><span data-stu-id="2deca-177">**tooverify hello edge node SSH endpoint**</span></span>

1. <span data-ttu-id="2deca-178">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2deca-178">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2deca-179">Nyisson meg egy élcsomópontot hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="2deca-179">Open hello HDInsight cluster with an edge node.</span></span>
3. <span data-ttu-id="2deca-180">Kattintson a **alkalmazások** hello fürt paneljén.</span><span class="sxs-lookup"><span data-stu-id="2deca-180">Click **Applications** from hello cluster blade.</span></span> <span data-ttu-id="2deca-181">Hello élcsomópont megjelenik.</span><span class="sxs-lookup"><span data-stu-id="2deca-181">You shall see hello edge node.</span></span>  <span data-ttu-id="2deca-182">hello alapértelmezés szerint ez **új edgenode**.</span><span class="sxs-lookup"><span data-stu-id="2deca-182">hello default name is **new-edgenode**.</span></span>
4. <span data-ttu-id="2deca-183">Kattintson a hello élcsomópont.</span><span class="sxs-lookup"><span data-stu-id="2deca-183">Click hello edge node.</span></span> <span data-ttu-id="2deca-184">Ekkor megjelenik a hello SSH-végpontot.</span><span class="sxs-lookup"><span data-stu-id="2deca-184">You shall see hello SSH endpoint.</span></span>

<span data-ttu-id="2deca-185">**toouse Hive hello peremhálózati csomóponton**</span><span class="sxs-lookup"><span data-stu-id="2deca-185">**toouse Hive on hello edge node**</span></span>

1. <span data-ttu-id="2deca-186">SSH tooconnect toohello élcsomópont használja.</span><span class="sxs-lookup"><span data-stu-id="2deca-186">Use SSH tooconnect toohello edge node.</span></span> <span data-ttu-id="2deca-187">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="2deca-187">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="2deca-188">Toohello élcsomópontjához SSH kapcsolódás után használja a következő parancs tooopen hello Hive konzol hello:</span><span class="sxs-lookup"><span data-stu-id="2deca-188">After you have connected toohello edge node using SSH, use hello following command tooopen hello Hive console:</span></span>
   
        hive
3. <span data-ttu-id="2deca-189">Futtassa a következő parancs tooshow Hive táblák hello fürt hello:</span><span class="sxs-lookup"><span data-stu-id="2deca-189">Run hello following command tooshow Hive tables in hello cluster:</span></span>
   
        show tables;

## <a name="delete-an-edge-node"></a><span data-ttu-id="2deca-190">Egy élcsomópontot törlése</span><span class="sxs-lookup"><span data-stu-id="2deca-190">Delete an edge node</span></span>
<span data-ttu-id="2deca-191">Egy élcsomópontot hello Azure-portálon törölheti.</span><span class="sxs-lookup"><span data-stu-id="2deca-191">You can delete an edge node from hello Azure portal.</span></span>

<span data-ttu-id="2deca-192">**egy élcsomópontot tooaccess**</span><span class="sxs-lookup"><span data-stu-id="2deca-192">**tooaccess an edge node**</span></span>

1. <span data-ttu-id="2deca-193">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2deca-193">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2deca-194">Nyisson meg egy élcsomópontot hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="2deca-194">Open hello HDInsight cluster with an edge node.</span></span>
3. <span data-ttu-id="2deca-195">Kattintson a **alkalmazások** hello fürt paneljén.</span><span class="sxs-lookup"><span data-stu-id="2deca-195">Click **Applications** from hello cluster blade.</span></span> <span data-ttu-id="2deca-196">Peremhálózati lista megjelenik.</span><span class="sxs-lookup"><span data-stu-id="2deca-196">You shall see a list of edge nodes.</span></span>  
4. <span data-ttu-id="2deca-197">Kattintson a jobb gombbal hello élcsomópont toodelete szeretne, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="2deca-197">Right-click hello edge node you want toodelete, and then click **Delete**.</span></span>
5. <span data-ttu-id="2deca-198">Kattintson a **Igen** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="2deca-198">Click **Yes** tooconfirm.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2deca-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2deca-199">Next steps</span></span>
<span data-ttu-id="2deca-200">Ebben a cikkben megtanulta, hogyan tooadd egy élcsomópontot, és hogyan tooaccess hello élcsomópont.</span><span class="sxs-lookup"><span data-stu-id="2deca-200">In this article, you have learned how tooadd an edge node and how tooaccess hello edge node.</span></span> <span data-ttu-id="2deca-201">toolearn több, tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="2deca-201">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="2deca-202">[HDInsight-alkalmazások telepítése](hdinsight-apps-install-applications.md): megtudhatja, hogyan tooinstall egy HDInsight-alkalmazás tooyour fürtök.</span><span class="sxs-lookup"><span data-stu-id="2deca-202">[Install HDInsight applications](hdinsight-apps-install-applications.md): Learn how tooinstall an HDInsight application tooyour clusters.</span></span>
* <span data-ttu-id="2deca-203">[Egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md): megtudhatja, hogyan toodeploy egy közzé nem tett HDInsight-alkalmazás tooHDInsight.</span><span class="sxs-lookup"><span data-stu-id="2deca-203">[Install custom HDInsight applications](hdinsight-apps-install-custom-applications.md): learn how toodeploy an unpublished HDInsight application tooHDInsight.</span></span>
* <span data-ttu-id="2deca-204">[HDInsight-alkalmazások közzététele](hdinsight-apps-publish-applications.md): megtudhatja, hogyan toopublish az egyéni HDInsight-alkalmazások tooAzure piactéren.</span><span class="sxs-lookup"><span data-stu-id="2deca-204">[Publish HDInsight applications](hdinsight-apps-publish-applications.md): Learn how toopublish your custom HDInsight applications tooAzure Marketplace.</span></span>
* <span data-ttu-id="2deca-205">[MSDN: HDInsight-alkalmazások telepítése](https://msdn.microsoft.com/library/mt706515.aspx): megtudhatja, hogyan toodefine HDInsight-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="2deca-205">[MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx): Learn how toodefine HDInsight applications.</span></span>
* <span data-ttu-id="2deca-206">[Parancsfájlművelet Linux-alapú HDInsight-fürtök testreszabása](hdinsight-hadoop-customize-cluster-linux.md): megtudhatja, hogyan toouse parancsfájlművelet tooinstall további alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="2deca-206">[Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md): learn how toouse Script Action tooinstall additional applications.</span></span>
* <span data-ttu-id="2deca-207">[Linux-alapú Hadoop-fürtök létrehozása a Resource Manager-sablonok használatával HDInsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md): megtudhatja, hogyan toocall Resource Manager sablonok toocreate HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="2deca-207">[Create Linux-based Hadoop clusters in HDInsight using Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md): learn how toocall Resource Manager templates toocreate HDInsight clusters.</span></span>

