---
title: "a virtuális hálózat - Azure fürtök aaaCreate HBase |} Microsoft Docs"
description: "Ismerkedés az Azure HDInsight HBase használatával. Ismerje meg, hogyan toocreate HDInsight HBase clusters Azure virtuális hálózaton."
keywords: 
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 8de8e446-f818-4e61-8fad-e9d38421e80d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 097338a5a650bb607a9f6f9ddb59bb88d098b56f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hbase-clusters-on-hdinsight-in-azure-virtual-network"></a><span data-ttu-id="fbc1f-104">Hozzon létre HBase-fürtökkel a HDInsight az Azure-beli virtuális hálózathoz</span><span class="sxs-lookup"><span data-stu-id="fbc1f-104">Create HBase clusters on HDInsight in Azure Virtual Network</span></span>
<span data-ttu-id="fbc1f-105">Ismerje meg, hogyan toocreate Azure HDInsight HBase clusters a egy [Azure Virtual Network][1].</span><span class="sxs-lookup"><span data-stu-id="fbc1f-105">Learn how toocreate Azure HDInsight HBase clusters in an [Azure Virtual Network][1].</span></span>

<span data-ttu-id="fbc1f-106">A virtuális hálózati integráció, HBase-fürtökkel telepített toohello azonos virtuális hálózat az alkalmazások, ezért lehet, hogy alkalmazásokat közvetlenül kommunikálhatnak a HBase.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-106">With virtual network integration, HBase clusters can be deployed toohello same virtual network as your applications so that applications can communicate with HBase directly.</span></span> <span data-ttu-id="fbc1f-107">hello előnyöket nyújtja:</span><span class="sxs-lookup"><span data-stu-id="fbc1f-107">hello benefits include:</span></span>

* <span data-ttu-id="fbc1f-108">Közvetlen kapcsolatot hello webes alkalmazás toohello csomópontokból hello HBase-fürtöt, amely lehetővé teszi a kommunikációt keresztül HBase Java távoli eljáráshívásnak (RPC) API-k hívása.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-108">Direct connectivity of hello web application toohello nodes of hello HBase cluster, which enables communication via HBase Java remote procedure call (RPC) APIs.</span></span>
* <span data-ttu-id="fbc1f-109">Javítja a teljesítményt a forgalom nem rendelkezik több átjárók és terheléselosztók lépjen.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-109">Improved performance by not having your traffic go over multiple gateways and load-balancers.</span></span>
* <span data-ttu-id="fbc1f-110">hello képességét tooprocess bizalmas adatokat anélkül, hogy egy nyilvános végpontot több biztonságos módon.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-110">hello ability tooprocess sensitive information in a more secure manner without exposing a public endpoint.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="fbc1f-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fbc1f-111">Prerequisites</span></span>
<span data-ttu-id="fbc1f-112">Ez az oktatóanyag elkezdéséhez a következő elemek hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="fbc1f-112">Before you begin this tutorial, you must have hello following items:</span></span>

* <span data-ttu-id="fbc1f-113">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-113">**An Azure subscription**.</span></span> <span data-ttu-id="fbc1f-114">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="fbc1f-114">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="fbc1f-115">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-115">**A workstation with Azure PowerShell**.</span></span> <span data-ttu-id="fbc1f-116">Lásd: [telepítése és használata az Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).</span><span class="sxs-lookup"><span data-stu-id="fbc1f-116">See [Install and use Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).</span></span>

## <a name="create-hbase-cluster-into-virtual-network"></a><span data-ttu-id="fbc1f-117">HBase-fürt a virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbc1f-117">Create HBase cluster into virtual network</span></span>
<span data-ttu-id="fbc1f-118">Ebben a szakaszban egy Linux-alapú HBase fürt létrehozása hello függő Azure Storage-fiók egy Azure-beli virtuális hálózat használatával egy [Azure Resource Manager sablon](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="fbc1f-118">In this section, you create a Linux-based HBase cluster with hello dependent Azure Storage account in an Azure virtual network using an [Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span> <span data-ttu-id="fbc1f-119">Egyéb Fürtlétrehozási módszerekhez és hello beállításainak ismertetése, tanulmányozza a [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="fbc1f-119">For other cluster creation methods and understanding hello settings, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="fbc1f-120">Egy sablon toocreate Hadoop használatáról további információt a HDInsight-fürtök, a következő témakörben: [létrehozása Hadoop-fürtök Azure Resource Manager-sablonok használata a hdinsightban](hdinsight-hadoop-create-windows-clusters-arm-templates.md)</span><span class="sxs-lookup"><span data-stu-id="fbc1f-120">For more information about using a template toocreate Hadoop clusters in HDInsight, see [Create Hadoop clusters in HDInsight using Azure Resource Manager templates](hdinsight-hadoop-create-windows-clusters-arm-templates.md)</span></span>

> [!NOTE]
> <span data-ttu-id="fbc1f-121">A rendszer néhány tulajdonságokat kódolt hello sablonba.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-121">Some properties are hard-coded into hello template.</span></span> <span data-ttu-id="fbc1f-122">Példa:</span><span class="sxs-lookup"><span data-stu-id="fbc1f-122">For example:</span></span>
>
> * <span data-ttu-id="fbc1f-123">**Hely**: USA keleti régiója 2. régiója</span><span class="sxs-lookup"><span data-stu-id="fbc1f-123">**Location**: East US 2</span></span>
> * <span data-ttu-id="fbc1f-124">**Fürt verziója**: 3.5</span><span class="sxs-lookup"><span data-stu-id="fbc1f-124">**Cluster version**: 3.5</span></span>
> * <span data-ttu-id="fbc1f-125">**A fürt feldolgozó csomópontok száma**: 2. régiója</span><span class="sxs-lookup"><span data-stu-id="fbc1f-125">**Cluster worker node count**: 2</span></span>
> * <span data-ttu-id="fbc1f-126">**Alapértelmezett tárfiók**: egy egyedi karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fbc1f-126">**Default storage account**: a unique string</span></span>
> * <span data-ttu-id="fbc1f-127">**Virtuális hálózati név**: &lt;fürt neve >-hálózatok</span><span class="sxs-lookup"><span data-stu-id="fbc1f-127">**Virtual network name**: &lt;Cluster Name>-vnet</span></span>
> * <span data-ttu-id="fbc1f-128">**Virtuális hálózat címtere**: 10.0.0.0/16</span><span class="sxs-lookup"><span data-stu-id="fbc1f-128">**Virtual network address space**: 10.0.0.0/16</span></span>
> * <span data-ttu-id="fbc1f-129">**Alhálózati név**: Alhalozat_1</span><span class="sxs-lookup"><span data-stu-id="fbc1f-129">**Subnet name**: subnet1</span></span>
> * <span data-ttu-id="fbc1f-130">**Alhálózati címtartományt**: 10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="fbc1f-130">**Subnet address range**: 10.0.0.0/24</span></span>
>
> <span data-ttu-id="fbc1f-131">&lt;A fürt neve > helyére hello fürtnév hello sablon használata esetén adja meg.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-131">&lt;Cluster Name> is replaced with hello cluster name you provide when using hello template.</span></span>
>
>

1. <span data-ttu-id="fbc1f-132">Kattintson a következő kép tooopen hello sablon hello Azure-portálon hello.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-132">Click hello following image tooopen hello template in hello Azure portal.</span></span> <span data-ttu-id="fbc1f-133">hello sablon található [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).</span><span class="sxs-lookup"><span data-stu-id="fbc1f-133">hello template is located in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).</span></span>

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-provision-vnet/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. <span data-ttu-id="fbc1f-134">A hello **egyéni telepítési** panelen adja meg a következő tulajdonságai hello:</span><span class="sxs-lookup"><span data-stu-id="fbc1f-134">From hello **Custom deployment** blade, enter hello following properties:</span></span>

   * <span data-ttu-id="fbc1f-135">**Előfizetés**: válassza ki a használt Azure-előfizetést toocreate hello HDInsight-fürtöt, a függő tárfiókot hello és hello Azure-beli virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-135">**Subscription**: Select an Azure subscription used toocreate hello HDInsight cluster, hello dependent Storage account and hello Azure virtual network.</span></span>
   * <span data-ttu-id="fbc1f-136">**Erőforráscsoport**: válasszon **hozzon létre új**, és adjon meg egy új erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-136">**Resource group**: Select **Create new**, and specify a new resource group name.</span></span>
   * <span data-ttu-id="fbc1f-137">**Hely**: hello erőforráscsoport helyének kiválasztására.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-137">**Location**: Select a location for hello resource group.</span></span>
   * <span data-ttu-id="fbc1f-138">**ClusterName**: Adjon meg egy nevet hello Hadoop-fürt toobe létrehozni.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-138">**ClusterName**: Enter a name for hello Hadoop cluster toobe created.</span></span>
   * <span data-ttu-id="fbc1f-139">**A fürt bejelentkezési nevet és jelszót**: hello alapértelmezett bejelentkezési név az **admin**.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-139">**Cluster login name and password**: hello default login name is **admin**.</span></span>
   * <span data-ttu-id="fbc1f-140">**SSH-felhasználónév és jelszó**: hello alapértelmezett felhasználónév az **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-140">**SSH username and password**: hello default username is **sshuser**.</span></span>  <span data-ttu-id="fbc1f-141">Ezt át lehet nevezni.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-141">You can rename it.</span></span>
   * <span data-ttu-id="fbc1f-142">**Elfogadom toohello feltételek mellett hello fenti**: (válassza ki)</span><span class="sxs-lookup"><span data-stu-id="fbc1f-142">**I agree toohello terms and hello conditions stated above**: (Select)</span></span>
3. <span data-ttu-id="fbc1f-143">Kattintson a **Purchase** (Vásárlás) gombra.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-143">Click **Purchase**.</span></span> <span data-ttu-id="fbc1f-144">Vesz igénybe körülbelül 20 percet toocreate egy fürt.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-144">It takes about around 20 minutes toocreate a cluster.</span></span> <span data-ttu-id="fbc1f-145">Hello fürt létrehozása után kattintson a fürt paneljén hello hello portál tooopen azt.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-145">Once hello cluster is created, you can click hello cluster blade in hello portal tooopen it.</span></span>

<span data-ttu-id="fbc1f-146">Hello az oktatóanyag befejezése után érdemes toodelete hello fürt.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-146">After you complete hello tutorial, you might want toodelete hello cluster.</span></span> <span data-ttu-id="fbc1f-147">A HDInsight az Azure Storage szolgáltatásban tárolja az adatokat, így biztonságosan törölhet olyan fürtöket, amelyek nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-147">With HDInsight, your data is stored in Azure Storage, so you can safely delete a cluster when it is not in use.</span></span> <span data-ttu-id="fbc1f-148">Ráadásul a HDInsight-fürtök akkor is díjkötelesek, amikor éppen nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-148">You are also charged for an HDInsight cluster, even when it is not in use.</span></span> <span data-ttu-id="fbc1f-149">Mivel hello díjak hello fürt sokszor több mint hello a tárolási, érdemes gazdasági toodelete fürtök amikor nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-149">Since hello charges for hello cluster are many times more than hello charges for storage, it makes economic sense toodelete clusters when they are not in use.</span></span> <span data-ttu-id="fbc1f-150">A fürtök törlésével a hello utasításokért lásd: [kezelése Hadoop-fürtöket a HDInsight használatával hello Azure-portálon](hdinsight-administer-use-management-portal.md#delete-clusters).</span><span class="sxs-lookup"><span data-stu-id="fbc1f-150">For hello instructions of deleting a cluster, see [Manage Hadoop clusters in HDInsight by using hello Azure portal](hdinsight-administer-use-management-portal.md#delete-clusters).</span></span>

<span data-ttu-id="fbc1f-151">az új HBase-fürtöt használata toobegin található hello eljárásokkal [HBase a Hadoop HDInsight használatának megkezdésében](hdinsight-hbase-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="fbc1f-151">toobegin working with your new HBase cluster, you can use hello procedures found in [Get started using HBase with Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started.md).</span></span>

## <a name="connect-toohello-hbase-cluster-using-hbase-java-rpc-apis"></a><span data-ttu-id="fbc1f-152">Csatlakozás toohello HBase-fürtöt HBase Java RPC API-k használatával</span><span class="sxs-lookup"><span data-stu-id="fbc1f-152">Connect toohello HBase cluster using HBase Java RPC APIs</span></span>
1. <span data-ttu-id="fbc1f-153">Hozzon létre egy infrastruktúra (IaaS) szolgáltatás virtuális gépként történő hello azonos Azure virtuális hálózatban, és hello ugyanazon az alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-153">Create an infrastructure as a service (IaaS) virtual machine into hello same Azure virtual network and hello same subnet.</span></span> <span data-ttu-id="fbc1f-154">Egy új infrastruktúra-szolgáltatási virtuális gép létrehozása, lásd: [hozzon létre egy virtuális gép futó Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="fbc1f-154">For instructions on creating a new IaaS virtual machine, see [Create a Virtual Machine Running Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md).</span></span> <span data-ttu-id="fbc1f-155">Hello lépések ebben a dokumentumban, a következő értékek hello hálózati konfiguráció hello kell használni:</span><span class="sxs-lookup"><span data-stu-id="fbc1f-155">When following hello steps in this document, you must use hello following values for hello Network configuration:</span></span>

   * <span data-ttu-id="fbc1f-156">**Virtuális hálózati**: &lt;fürt neve >-hálózatok</span><span class="sxs-lookup"><span data-stu-id="fbc1f-156">**Virtual network**: &lt;Cluster name>-vnet</span></span>
   * <span data-ttu-id="fbc1f-157">**Alhálózati**: Alhalozat_1</span><span class="sxs-lookup"><span data-stu-id="fbc1f-157">**Subnet**: subnet1</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="fbc1f-158">Cserélje le &lt;fürtnév > hello nevű előző lépésekben hello HDInsight-fürt létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-158">Replace &lt;Cluster name> with hello name you used when creating hello HDInsight cluster in previous steps.</span></span>
   >
   >

   <span data-ttu-id="fbc1f-159">Használja ezeket az értékeket, hello virtuális gép el van helyezve a hello azonos virtuális hálózati és alhálózati hello HDInsight-fürttel.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-159">Using these values, hello virtual machine is placed in hello same virtual network and subnet as hello HDInsight cluster.</span></span> <span data-ttu-id="fbc1f-160">Ez a konfiguráció lehetővé teszi, hogy azok toodirectly kommunikálnak egymással.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-160">This configuration allows them toodirectly communicate with each other.</span></span> <span data-ttu-id="fbc1f-161">Nincs olyan módon toocreate egy üres élcsomópontot a HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-161">There is a way toocreate an HDInsight cluster with an empty edge node.</span></span> <span data-ttu-id="fbc1f-162">hello élcsomópont használt toomanage hello tárolófürt is lehet.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-162">hello edge node can be used toomanage hello cluster.</span></span>  <span data-ttu-id="fbc1f-163">További információkért lásd: [üres peremhálózati csomópontok használata a Hdinsightban](hdinsight-apps-use-edge-node.md).</span><span class="sxs-lookup"><span data-stu-id="fbc1f-163">For more information, see [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md).</span></span>

2. <span data-ttu-id="fbc1f-164">Ha egy Java-alkalmazás tooconnect tooHBase távolról, hello teljesen minősített tartománynevét (FQDN) kell használnia.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-164">When using a Java application tooconnect tooHBase remotely, you must use hello fully qualified domain name (FQDN).</span></span> <span data-ttu-id="fbc1f-165">toodetermine, ha előbb telepítik azokra hello kapcsolatspecifikus DNS-utótagja hello HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-165">toodetermine this, you must get hello connection-specific DNS suffix of hello HBase cluster.</span></span> <span data-ttu-id="fbc1f-166">toodo, használható hello a következő módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="fbc1f-166">toodo that, you can use one of hello following methods:</span></span>

   * <span data-ttu-id="fbc1f-167">Egy webes böngésző toomake egy Ambari hívást használhatja:</span><span class="sxs-lookup"><span data-stu-id="fbc1f-167">Use a Web browser toomake an Ambari call:</span></span>

     <span data-ttu-id="fbc1f-168">Keresse meg a toohttps: / /&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / üzemeltető? minimal_response = true.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-168">Browse toohttps://&lt;ClusterName>.azurehdinsight.net/api/v1/clusters/&lt;ClusterName>/hosts?minimal_response=true.</span></span> <span data-ttu-id="fbc1f-169">A JSON-fájl, a DNS-utótagok hello változik.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-169">It turns a JSON file with hello DNS suffixes.</span></span>
   * <span data-ttu-id="fbc1f-170">Hello Ambari webhelyet használja:</span><span class="sxs-lookup"><span data-stu-id="fbc1f-170">Use hello Ambari website:</span></span>

     1. <span data-ttu-id="fbc1f-171">Keresse meg a túl https://&lt;ClusterName >. azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-171">Browse too https://&lt;ClusterName>.azurehdinsight.net.</span></span>
     2. <span data-ttu-id="fbc1f-172">Kattintson a **állomások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-172">Click **Hosts** from hello top menu.</span></span>
   * <span data-ttu-id="fbc1f-173">Használja a Curl toomake REST-hívást:</span><span class="sxs-lookup"><span data-stu-id="fbc1f-173">Use Curl toomake REST calls:</span></span>

    ```bash
        curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest
    ```

     <span data-ttu-id="fbc1f-174">Hello JavaScript Object Notation (JSON) visszaadott adatok hello "gazdaszámítógép_neve" bejegyzés található.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-174">In hello JavaScript Object Notation (JSON) data returned, find hello "host_name" entry.</span></span> <span data-ttu-id="fbc1f-175">Hello FQDN hello fürt hello csomópontján tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-175">It contains hello FQDN for hello nodes in hello cluster.</span></span> <span data-ttu-id="fbc1f-176">Példa:</span><span class="sxs-lookup"><span data-stu-id="fbc1f-176">For example:</span></span>

         ...
         "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
         ...

     <span data-ttu-id="fbc1f-177">hello hello tartomány neve hello fürtnév kezdetű része hello DNS-utótagot.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-177">hello portion of hello domain name beginning with hello cluster name is hello DNS suffix.</span></span> <span data-ttu-id="fbc1f-178">Például mycluster.b1.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-178">For example, mycluster.b1.cloudapp.net.</span></span>
   * <span data-ttu-id="fbc1f-179">Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="fbc1f-179">Use Azure PowerShell</span></span>

     <span data-ttu-id="fbc1f-180">A következő Azure PowerShell parancsfájl tooregister hello használata hello **Get-ClusterDetail** függvény, amely lehet használt tooreturn hello DNS-utótag:</span><span class="sxs-lookup"><span data-stu-id="fbc1f-180">Use hello following Azure PowerShell script tooregister hello **Get-ClusterDetail** function, which can be used tooreturn hello DNS suffix:</span></span>

    ```powershell
        function Get-ClusterDetail(
            [String]
            [Parameter( Position=0, Mandatory=$true )]
            $ClusterDnsName,
            [String]
            [Parameter( Position=1, Mandatory=$true )]
            $Username,
            [String]
            [Parameter( Position=2, Mandatory=$true )]
            $Password,
            [String]
            [Parameter( Position=3, Mandatory=$true )]
            $PropertyName
            )
        {
        <#
            .SYNOPSIS
            Displays information toofacilitate an HDInsight cluster-to-cluster scenario within hello same virtual network.
            .Description
            This command shows hello following 4 properties of an HDInsight cluster:
            1. ZookeeperQuorum (supports only HBase type cluster)
                Shows hello value of HBase property "hbase.zookeeper.quorum".
            2. ZookeeperClientPort (supports only HBase type cluster)
                Shows hello value of HBase property "hbase.zookeeper.property.clientPort".
            3. HBaseRestServers (supports only HBase type cluster)
                Shows a list of host FQDNs that run hello HBase REST server.
            4. FQDNSuffix (supports all cluster types)
                Shows hello FQDN suffix of hosts in hello cluster.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
            This command shows hello value of HBase property "hbase.zookeeper.quorum".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
            This command shows hello value of HBase property "hbase.zookeeper.property.clientPort".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
            This command shows a list of host FQDNs that run hello HBase REST server.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
            This command shows hello FQDN suffix of hosts in hello cluster.
        #>

            $DnsSuffix = ".azurehdinsight.net"

            $ClusterFQDN = $ClusterDnsName + $DnsSuffix
            $webclient = new-object System.Net.WebClient
            $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

            if($PropertyName -eq "ZookeeperQuorum")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
            }
            if($PropertyName -eq "ZookeeperClientPort")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
            }
            if($PropertyName -eq "HBaseRestServers")
            {
                $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                $Response1 = $webclient.DownloadString($Url1)
                $JsonObject1 = $Response1 | ConvertFrom-Json
                $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                $Response2 = $webclient.DownloadString($Url2)
                $JsonObject2 = $Response2 | ConvertFrom-Json
                foreach ($host_component in $JsonObject2.host_components)
                {
                    $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                    Write-host $ConnectionString
                }
            }
            if($PropertyName -eq "FQDNSuffix")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                $pos = $FQDN.IndexOf(".")
                $Suffix = $FQDN.Substring($pos + 1)
                Write-host $Suffix
            }
        }
    ```

     <span data-ttu-id="fbc1f-181">Hello Azure PowerShell parancsfájl futtatását, miután használata hello következő tooreturn hello DNS-utótag parancs hello segítségével **Get-ClusterDetail** függvény.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-181">After running hello Azure PowerShell script, use hello following command tooreturn hello DNS suffix by using hello **Get-ClusterDetail** function.</span></span> <span data-ttu-id="fbc1f-182">Ez a parancs használata esetén adja meg a HDInsight HBase fürt nevét, a felügyeleti neve és a rendszergazdai jelszó.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-182">Specify your HDInsight HBase cluster name, admin name, and admin password when using this command.</span></span>

    ```powershell
        Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>
    ```

     <span data-ttu-id="fbc1f-183">Ez a parancs visszaadja a hello DNS-utótagot.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-183">This command returns hello DNS suffix.</span></span> <span data-ttu-id="fbc1f-184">Például **yourclustername.b4.internal.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-184">For example, **yourclustername.b4.internal.cloudapp.net**.</span></span>


<!--
3.    Change hello primary DNS suffix configuration of hello virtual machine. This enables hello virtual machine tooautomatically resolve hello host name of hello HBase cluster without explicit specification of hello suffix. For example, hello *workernode0* host name will be correctly resolved tooworkernode0 of hello HBase cluster.

    toomake hello configuration change:

    1. RDP into hello virtual machine.
    2. Open **Local Group Policy Editor**. hello executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** toohello value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot hello virtual machine.
-->

<span data-ttu-id="fbc1f-185">amely a virtuális gép hello tooverify is kommunikálni hello HBase-fürtöt, hello paranccsal `ping headnode0.<dns suffix>` hello virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-185">tooverify that hello virtual machine can communicate with hello HBase cluster, use hello command `ping headnode0.<dns suffix>` from hello virtual machine.</span></span> <span data-ttu-id="fbc1f-186">Például a ping headnode0.mycluster.b1.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-186">For example, ping headnode0.mycluster.b1.cloudapp.net.</span></span>

<span data-ttu-id="fbc1f-187">toouse ezt az információt Java-alkalmazások, a lépésekkel hello a [Maven használata toobuild Java-alkalmazások a HBase a HDInsight (Hadoop) használó](hdinsight-hbase-build-java-maven.md) toocreate kérelmet.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-187">toouse this information in a Java application, you can follow hello steps in [Use Maven toobuild Java applications that use HBase with HDInsight (Hadoop)](hdinsight-hbase-build-java-maven.md) toocreate an application.</span></span> <span data-ttu-id="fbc1f-188">Csatlakozzon a távoli kiszolgáló tooa a HBase, előbb módosítsa a hello toohave hello alkalmazás **hbase-site.xml** Zookeeper a példa toouse hello FQDN fájlt.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-188">toohave hello application connect tooa remote HBase server, modify hello **hbase-site.xml** file in this example toouse hello FQDN for Zookeeper.</span></span> <span data-ttu-id="fbc1f-189">Példa:</span><span class="sxs-lookup"><span data-stu-id="fbc1f-189">For example:</span></span>

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [!NOTE]
> <span data-ttu-id="fbc1f-190">További információ a névfeloldás az Azure virtuális hálózataihoz egyebek között toouse a saját DNS-kiszolgáló lásd: [Name (DNS) névfeloldási](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="fbc1f-190">For more information about name resolution in Azure virtual networks, including how toouse your own DNS server, see [Name Resolution (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="fbc1f-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fbc1f-191">Next steps</span></span>
<span data-ttu-id="fbc1f-192">Ebben az oktatóanyagban, megtudta, hogyan toocreate HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="fbc1f-192">In this tutorial, you learned how toocreate an HBase cluster.</span></span> <span data-ttu-id="fbc1f-193">toolearn több, lásd:</span><span class="sxs-lookup"><span data-stu-id="fbc1f-193">toolearn more, see:</span></span>

* [<span data-ttu-id="fbc1f-194">Első lépései a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="fbc1f-194">Get started with HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="fbc1f-195">Üres peremhálózati csomópontok használata a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="fbc1f-195">Use empty edge nodes in HDInsight</span></span>](hdinsight-apps-use-edge-node.md)
* [<span data-ttu-id="fbc1f-196">HBase-replikálás konfigurálása a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="fbc1f-196">Configure HBase replication in HDInsight</span></span>](hdinsight-hbase-replication.md)
* [<span data-ttu-id="fbc1f-197">Hdinsight Hadoop-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbc1f-197">Create Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="fbc1f-198">A HBase a Hadoop HDInsight használatának megkezdése</span><span class="sxs-lookup"><span data-stu-id="fbc1f-198">Get started using HBase with Hadoop in HDInsight</span></span>](hdinsight-hbase-tutorial-get-started.md)
* [<span data-ttu-id="fbc1f-199">A HBase a hdinsight eszközben Twitter sentiment elemzése</span><span class="sxs-lookup"><span data-stu-id="fbc1f-199">Analyze Twitter sentiment with HBase in HDInsight</span></span>](hdinsight-hbase-analyze-twitter-sentiment.md)
* <span data-ttu-id="fbc1f-200">[Virtual Network áttekintése][vnet-overview]</span><span class="sxs-lookup"><span data-stu-id="fbc1f-200">[Virtual Network Overview][vnet-overview]</span></span>

[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: /powershell/azureps-cmdlets-docs


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Hello új HBase fürtöt rendelkezés részletei"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Parancsfájlművelet toocustomize HBase-fürtöt használja"

[azure-preview-portal]: https://portal.azure.com
