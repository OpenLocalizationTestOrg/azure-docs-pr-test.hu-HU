---
title: "Linux virtuális gépek számítási HPC Pack-fürtben lévő |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre és HPC Pack-fürt használatára az Azure-ban a Linux nagy teljesítményű számítástechnikai (HPC) munkaterhelések"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 4d080fdd-5ffe-4f54-a78d-4c818f6eb3fb
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/12/2016
ms.author: danlep
ms.openlocfilehash: 809d3944311badf265117d353b65642e044d900c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="effb0-103">Az Azure-beli HPC Pack-fürtök linuxos számítási csomópontjaival kapcsolatos alapvető tudnivalók</span><span class="sxs-lookup"><span data-stu-id="effb0-103">Get started with Linux compute nodes in an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="effb0-104">Állítson be egy [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) Azure egy központi fut a Windows Server és több csomópontot tartalmazó fürt számítási csomópontjain futó támogatott Linux-disztribúció.</span><span class="sxs-lookup"><span data-stu-id="effb0-104">Set up a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) cluster in Azure that contains a head node running Windows Server and several compute nodes running a supported Linux distribution.</span></span> <span data-ttu-id="effb0-105">Megismerkedhet a beállítások áthelyezni az adatokat a központi Windows-csomópont, a fürt és a Linux-csomópontok között.</span><span class="sxs-lookup"><span data-stu-id="effb0-105">Explore options to move data among the Linux nodes and the Windows head node of the cluster.</span></span> <span data-ttu-id="effb0-106">Útmutató a fürt Linux HPC feladatok elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="effb0-106">Learn how to submit Linux HPC jobs to the cluster.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="effb0-107">Magas szinten a következő ábrán a HPC Pack fürt létrehozása és használata.</span><span class="sxs-lookup"><span data-stu-id="effb0-107">At a high level, the following diagram shows the HPC Pack cluster you create and work with.</span></span>

![Linux-csomópontok HPC Pack fürt][scenario]

<span data-ttu-id="effb0-109">Egyéb lehetőségek az alkalmazásokat és szolgáltatásokat futtathatnak Linux HPC az Azure-ban, lásd: [kötegelt és nagy teljesítményű számítástechnikai rendszerek technikai erőforrások](../../../batch/big-compute-resources.md).</span><span class="sxs-lookup"><span data-stu-id="effb0-109">For other options to run Linux HPC workloads in Azure, see [Technical resources for batch and high-performance computing](../../../batch/big-compute-resources.md).</span></span>

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a><span data-ttu-id="effb0-110">HPC Pack-fürt üzembe helyezése a Linux számítási csomópont</span><span class="sxs-lookup"><span data-stu-id="effb0-110">Deploy an HPC Pack cluster with Linux compute nodes</span></span>
<span data-ttu-id="effb0-111">Ez a cikk bemutatja, két lehetőség HPC Pack-fürt üzembe helyezése az Azure-ban Linux számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="effb0-111">This article shows you two options to deploy an HPC Pack cluster in Azure with Linux compute nodes.</span></span> <span data-ttu-id="effb0-112">Mindkét módszer a Windows Server Piactéri rendszerkép HPC Pack az átjárócsomópont létrehozásához használja.</span><span class="sxs-lookup"><span data-stu-id="effb0-112">Both methods use a Marketplace image of Windows Server with HPC Pack to create the head node.</span></span> 

* <span data-ttu-id="effb0-113">**Az Azure Resource Manager-sablon** -sablont az Azure piactérről, vagy a következő gyorsindítási sablonon közösségi használatával automatizálhatja a Resource Manager üzembe helyezési modellben a fürt létrehozását.</span><span class="sxs-lookup"><span data-stu-id="effb0-113">**Azure Resource Manager template** - Use a template from the Azure Marketplace, or a quickstart template from the community, to automate creation of the cluster in the Resource Manager deployment model.</span></span> <span data-ttu-id="effb0-114">Például a [HPC Pack fürt Linux munkaterhelések](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) sablon az Azure piactéren létrehoz egy teljes HPC Pack fürt infrastruktúra Linux HPC a munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="effb0-114">For example, the [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) template in the Azure Marketplace creates a complete HPC Pack cluster infrastructure for Linux HPC workloads.</span></span>
* <span data-ttu-id="effb0-115">**PowerShell parancsfájl** -használja a [Microsoft HPC Pack IaaS telepítési parancsfájl](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**New-HpcIaaSCluster.ps1**) a teljes fürtre történő telepítés, a klasszikus üzembe helyezési modellel automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="effb0-115">**PowerShell script** - Use the [Microsoft HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**New-HpcIaaSCluster.ps1**) to automate a complete cluster deployment in the classic deployment model.</span></span> <span data-ttu-id="effb0-116">Az Azure PowerShell-parancsfájl HPC Pack VM-lemezkép használja az Azure piactéren a gyors telepítése során, és a konfigurációs paraméterek központi telepítése Linux számítási csomópontok széles választékát nyújtja.</span><span class="sxs-lookup"><span data-stu-id="effb0-116">This Azure PowerShell script uses an HPC Pack VM image in the Azure Marketplace for fast deployment and provides a comprehensive set of configuration parameters to deploy Linux compute nodes.</span></span>

<span data-ttu-id="effb0-117">További információ a HPC Pack fürt telepítési lehetőségek az Azure-ban: [beállítások létrehozását és kezelését egy nagy teljesítményű számítástechnikai rendszerek (HPC) fürtön, az Azure-ban a Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="effb0-117">For more information about HPC Pack cluster deployment options in Azure, see [Options to create and manage a high-performance computing (HPC) cluster in Azure with Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

### <a name="prerequisites"></a><span data-ttu-id="effb0-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="effb0-118">Prerequisites</span></span>
* <span data-ttu-id="effb0-119">**Azure-előfizetés** -előfizetés használhatja az Azure globális vagy Azure Kína szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="effb0-119">**Azure subscription** - You can use a subscription in either the Azure Global or Azure China service.</span></span> <span data-ttu-id="effb0-120">Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/) néhány percig.</span><span class="sxs-lookup"><span data-stu-id="effb0-120">If you don't have an account, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.</span></span>
* <span data-ttu-id="effb0-121">**Magok kvóta** -előfordulhat, hogy növelnie kell a beállított kvótát mag, különösen akkor, ha úgy dönt, hogy a Virtuálisgép-méretek multicore több fürtcsomóponton telepíteni.</span><span class="sxs-lookup"><span data-stu-id="effb0-121">**Cores quota** - You might need to increase the quota of cores, especially if you choose to deploy several cluster nodes with multicore VM sizes.</span></span> <span data-ttu-id="effb0-122">A kvóta növeléséhez, nyissa meg az online támogatás ügyfélkérés díjmentesen.</span><span class="sxs-lookup"><span data-stu-id="effb0-122">To increase a quota, open an online customer support request at no charge.</span></span>
* <span data-ttu-id="effb0-123">**Linux-disztribúció** -HPC Pack jelenleg a következő Linux terjesztésekről támogatja a számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="effb0-123">**Linux distributions** - Currently HPC Pack supports the following Linux distributions for compute nodes.</span></span> <span data-ttu-id="effb0-124">Ezek terjesztéseket piactér verzióját használják, ahol az rendelkezésre áll, vagy adja meg a saját.</span><span class="sxs-lookup"><span data-stu-id="effb0-124">You can use Marketplace versions of these distributions where available, or supply your own.</span></span>
  
  * <span data-ttu-id="effb0-125">**CentOS-alapú**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1-es HPC</span><span class="sxs-lookup"><span data-stu-id="effb0-125">**CentOS-based**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC</span></span>
  * <span data-ttu-id="effb0-126">**Red Hat Enterprise Linux**: 6.7, 6.8, 7.2</span><span class="sxs-lookup"><span data-stu-id="effb0-126">**Red Hat Enterprise Linux**: 6.7, 6.8, 7.2</span></span>
  * <span data-ttu-id="effb0-127">**SUSE Linux Enterprise Server**: SLES 12, a SLES 12 (prémium), a SLES 12 SP1, a SLES 12 SP1 (prémium) SLES 12 a HPC, SLES 12 a HPC (prémium)</span><span class="sxs-lookup"><span data-stu-id="effb0-127">**SUSE Linux Enterprise Server**: SLES 12, SLES 12 (Premium), SLES 12 SP1, SLES 12 SP1 (Premium), SLES 12 for HPC, SLES 12 for HPC (Premium)</span></span>
  * <span data-ttu-id="effb0-128">**Ubuntu Server**: 14.04 LTS, 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="effb0-128">**Ubuntu Server**: 14.04 LTS, 16.04 LTS</span></span>
    
    > [!TIP]
    > <span data-ttu-id="effb0-129">Az RDMA-kompatibilisek-e Virtuálisgép-méretek egyike az Azure RDMA hálózati használatához adja meg a SUSE Linux Enterprise Server 12 HPC vagy HPC CentOS-alapú lemezkép az Azure piactérről.</span><span class="sxs-lookup"><span data-stu-id="effb0-129">To use the Azure RDMA network with one of the RDMA-capable VM sizes, specify a SUSE Linux Enterprise Server 12 HPC or CentOS-based HPC image from the Azure Marketplace.</span></span> <span data-ttu-id="effb0-130">További információkért lásd: [nagy teljesítményű számítási Virtuálisgép-méretek](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="effb0-130">For more information, see [High performance compute VM sizes](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
    > 
    > 

<span data-ttu-id="effb0-131">A fürt a HPC Pack IaaS telepítési parancsfájl használatával történő központi telepítéséről további Előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="effb0-131">Additional prerequisites to deploy the cluster by using the HPC Pack IaaS deployment script:</span></span>

* <span data-ttu-id="effb0-132">**Ügyfélszámítógép** – a fürt üzembe helyezési parancsfájl futtatásához Windows rendszerű számítógépre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="effb0-132">**Client computer** - You need a Windows-based client computer to run the cluster deployment script.</span></span>
* <span data-ttu-id="effb0-133">**Az Azure PowerShell** - [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview) (0.8.10 verzió vagy újabb) az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="effb0-133">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="effb0-134">**HPC Pack IaaS telepítési parancsfájl** - töltse le és csomagolja ki a parancsfájlt a legújabb verzióját a [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="effb0-134">**HPC Pack IaaS deployment script** - Download and unpack the latest version of the script from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="effb0-135">A parancsfájl verziója futtatásával ellenőrizheti `.\New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="effb0-135">You can check the version of the script by running `.\New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="effb0-136">Ez a cikk verzió 4.4.1 vagy később, a parancsfájl alapján.</span><span class="sxs-lookup"><span data-stu-id="effb0-136">This article is based on version 4.4.1 or later of the script.</span></span>

### <a name="deployment-option-1-use-a-resource-manager-template"></a><span data-ttu-id="effb0-137">1. telepítési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="effb0-137">Deployment option 1.</span></span> <span data-ttu-id="effb0-138">A Resource Manager-sablon használata</span><span class="sxs-lookup"><span data-stu-id="effb0-138">Use a Resource Manager template</span></span>
1. <span data-ttu-id="effb0-139">Lépjen a [HPC Pack fürt Linux munkaterhelések](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) sablon az Azure piactéren, és kattintson a **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="effb0-139">Go to the [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) template in the Azure Marketplace, and click **Deploy**.</span></span>
2. <span data-ttu-id="effb0-140">Az Azure portálon, tekintse át az adatokat, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="effb0-140">In the Azure portal, review the information and then click **Create**.</span></span>
   
    ![Portál létrehozásához][portal]
3. <span data-ttu-id="effb0-142">Az a **alapjai** panelen adjon meg egy nevet a fürt, amely az átjárócsomóponthoz VM is nevezi.</span><span class="sxs-lookup"><span data-stu-id="effb0-142">On the **Basics** blade, enter a name for the cluster, which also names the head node VM.</span></span> <span data-ttu-id="effb0-143">Válasszon egy meglévő erőforráscsoportot, vagy olyan helyre, amely elérhető a telepítési csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="effb0-143">You can choose an existing resource group or create a group for the deployment in a location that's available to you.</span></span> <span data-ttu-id="effb0-144">A hely van hatással, egyes Virtuálisgép-méretek és más Azure-szolgáltatásokkal (lásd: [régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services/)).</span><span class="sxs-lookup"><span data-stu-id="effb0-144">The location affects the availability of certain VM sizes and other Azure services (see [Products available by region](https://azure.microsoft.com/regions/services/)).</span></span>
4. <span data-ttu-id="effb0-145">Az a **csomópont beállítások Head** panelen első üzembe helyezés esetén általában elfogadhatja az alapértelmezett beállításokat.</span><span class="sxs-lookup"><span data-stu-id="effb0-145">On the **Head node settings** blade, for a first deployment, you can generally accept the default settings.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="effb0-146">A **utáni konfigurációs parancsprogram URL-Címének** nem kötelező beállítás megadásához egy nyilvánosan elérhető Windows PowerShell-parancsfájlt, amely után fut-e a virtuális gép központi csomóponton futtatni szeretné.</span><span class="sxs-lookup"><span data-stu-id="effb0-146">The **Post-configuration script URL** is an optional setting to specify a publicly available Windows PowerShell script that you want to run on the head node VM after it is running.</span></span> 
   > 
   > 
5. <span data-ttu-id="effb0-147">Az a **számítási csomópont beállítások** panelen válassza egy elnevezési mintát a csomópontok, a szám és a csomópontokon, és a Linux-disztribúció méretét.</span><span class="sxs-lookup"><span data-stu-id="effb0-147">On the **Compute node settings** blade, select a naming pattern for the nodes, the number and size of the nodes, and the Linux distribution to deploy.</span></span>
6. <span data-ttu-id="effb0-148">A a **infrastruktúrabeállításai** panelen írja be a virtuális hálózat és az Active Directory tartományhoz, a tartomány és a virtuális gép rendszergazdai hitelesítő adatokat és a egy elnevezési mintája a storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="effb0-148">On the **Infrastructure settings** blade, enter names for the virtual network and Active Directory domain, domain and VM administrator credentials, and a naming pattern for the storage accounts.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="effb0-149">HPC Pack az Active Directory-tartomány használatával hitelesíti a felhasználókat a fürt.</span><span class="sxs-lookup"><span data-stu-id="effb0-149">HPC Pack uses the Active Directory domain to authenticate cluster users.</span></span> 
   > 
   > 
7. <span data-ttu-id="effb0-150">Miután az érvényesítési tesztek futtatásához, és tekintse át a használati feltételeket, kattintson a **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="effb0-150">After the validation tests run and you review the terms of use, click **Purchase**.</span></span>

### <a name="deployment-option-2-use-the-iaas-deployment-script"></a><span data-ttu-id="effb0-151">Telepítési beállítás 2.</span><span class="sxs-lookup"><span data-stu-id="effb0-151">Deployment option 2.</span></span> <span data-ttu-id="effb0-152">Az infrastruktúra-szolgáltatási telepítési parancsfájl használata</span><span class="sxs-lookup"><span data-stu-id="effb0-152">Use the IaaS deployment script</span></span>
<span data-ttu-id="effb0-153">A fürt a HPC Pack IaaS telepítési parancsfájl használatával történő központi telepítéséről további Előfeltételek a következők:</span><span class="sxs-lookup"><span data-stu-id="effb0-153">Following are additional prerequisites to deploy the cluster by using the HPC Pack IaaS deployment script:</span></span>

* <span data-ttu-id="effb0-154">**Ügyfélszámítógép** – a fürt üzembe helyezési parancsfájl futtatásához Windows rendszerű számítógépre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="effb0-154">**Client computer** - You need a Windows-based client computer to run the cluster deployment script.</span></span>
* <span data-ttu-id="effb0-155">**Az Azure PowerShell** - [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview) (0.8.10 verzió vagy újabb) az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="effb0-155">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="effb0-156">**HPC Pack IaaS telepítési parancsfájl** - töltse le és csomagolja ki a parancsfájlt a legújabb verzióját a [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="effb0-156">**HPC Pack IaaS deployment script** - Download and unpack the latest version of the script from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="effb0-157">A parancsfájl verziója futtatásával ellenőrizheti `.\New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="effb0-157">You can check the version of the script by running `.\New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="effb0-158">Ez a cikk verzió 4.4.1 vagy később, a parancsfájl alapján.</span><span class="sxs-lookup"><span data-stu-id="effb0-158">This article is based on version 4.4.1 or later of the script.</span></span>

<span data-ttu-id="effb0-159">**Konfigurációs XML-fájl**</span><span class="sxs-lookup"><span data-stu-id="effb0-159">**XML configuration file**</span></span>

<span data-ttu-id="effb0-160">A HPC Pack IaaS telepítési parancsfájl bemeneti XML-konfigurációs fájl használatával írják le a HPC-fürtben.</span><span class="sxs-lookup"><span data-stu-id="effb0-160">The HPC Pack IaaS deployment script uses an XML configuration file as input to describe the  HPC cluster.</span></span> <span data-ttu-id="effb0-161">A következő minta konfigurációs fájlt egy HPC Pack átjárócsomópont és a számítási csomópontok két mérete A7 CentOS 7.0 Linux kis fürt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="effb0-161">The following sample configuration file specifies a small cluster consisting of an HPC Pack head node and two size A7 CentOS 7.0 Linux compute nodes.</span></span> 

<span data-ttu-id="effb0-162">A környezet és a kívánt fürtkonfigurációt szükség szerint módosítsa a fájl, és mentse a neve, pl. HPCDemoConfig.xml.</span><span class="sxs-lookup"><span data-stu-id="effb0-162">Modify the file as needed for your environment and desired cluster configuration, and save it with a name such as HPCDemoConfig.xml.</span></span> <span data-ttu-id="effb0-163">Például hogy kell az előfizetés nevet és egy egyedi tárfiók neve és a felhőalapú szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="effb0-163">For example, you need to supply your subscription name and a unique storage account name and cloud service name.</span></span> <span data-ttu-id="effb0-164">Emellett érdemes a számítási csomópontok különböző támogatott Linux képfájl kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="effb0-164">Additionally, you might want to choose a different supported Linux image for the compute nodes.</span></span> <span data-ttu-id="effb0-165">A konfigurációs fájlban elemeivel kapcsolatos további információkért tekintse meg a Manual.rtf a parancsfájl mappában és [HPC-fürt létrehozása a HPC Pack IaaS telepítési parancsfájl](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="effb0-165">For more information about the elements in the configuration file, see the Manual.rtf file in the script folder and [Create an HPC cluster with the HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>centos7rdmavnetje</VNetName>
    <SubnetName>CentOS7RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS7RDMA-HN</VMName>
    <ServiceName>centos7rdma-je</ServiceName>
  <VMSize>ExtraLarge</VMSize>
  <EnableRESTAPI />
  <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS7RDMA-LN%1%</VMNamePattern>
    <ServiceName>centos7rdma-je</ServiceName>
    <VMSize>A7</VMSize>
    <NodeCount>2</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

<span data-ttu-id="effb0-166">**A HPC Pack IaaS telepítési parancsfájl futtatásához**</span><span class="sxs-lookup"><span data-stu-id="effb0-166">**To run the HPC Pack IaaS deployment script**</span></span>

1. <span data-ttu-id="effb0-167">Rendszergazdaként nyissa meg a Windows PowerShell az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="effb0-167">Open Windows PowerShell on the client computer as an administrator.</span></span>
2. <span data-ttu-id="effb0-168">Módosítsa a mappát, ahol a parancsfájl könyvtárat telepítve (E:\IaaSClusterScript ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="effb0-168">Change directory to the folder where the script is installed (E:\IaaSClusterScript in this example).</span></span>
   
    ```powershell
    cd E:\IaaSClusterScript
    ```
3. <span data-ttu-id="effb0-169">A következő parancsot a HPC Pack fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="effb0-169">Run the following command to deploy the HPC Pack cluster.</span></span> <span data-ttu-id="effb0-170">Ebben a példában feltételezzük, hogy a konfigurációs fájlban található E:\HPCDemoConfig.xml</span><span class="sxs-lookup"><span data-stu-id="effb0-170">This example assumes that the configuration file is located in E:\HPCDemoConfig.xml</span></span>
   
    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```
   
    <span data-ttu-id="effb0-171">a.</span><span class="sxs-lookup"><span data-stu-id="effb0-171">a.</span></span> <span data-ttu-id="effb0-172">Mivel a **AdminPassword** nincs megadva az előző parancsban szereplő kéri a jelszó megadására, a felhasználó *MyAdminName*.</span><span class="sxs-lookup"><span data-stu-id="effb0-172">Because the **AdminPassword** is not specified in the preceding command, you are prompted to enter the password for user *MyAdminName*.</span></span>
   
    <span data-ttu-id="effb0-173">b.</span><span class="sxs-lookup"><span data-stu-id="effb0-173">b.</span></span> <span data-ttu-id="effb0-174">A parancsfájl ezután elindítja a konfigurációs fájl ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="effb0-174">The script then starts to validate the configuration file.</span></span> <span data-ttu-id="effb0-175">Ez attól függően, hogy a hálózati kapcsolat több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="effb0-175">It can take up to several minutes depending on the network connection.</span></span>
   
    ![Ellenőrzés][validate]
   
    <span data-ttu-id="effb0-177">c.</span><span class="sxs-lookup"><span data-stu-id="effb0-177">c.</span></span> <span data-ttu-id="effb0-178">Érvényesítést átadni, miután a parancsfájl létrehozása a fürt erőforrásait sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="effb0-178">After validations pass, the script lists the cluster resources to create.</span></span> <span data-ttu-id="effb0-179">Adja meg *Y* folytatja.</span><span class="sxs-lookup"><span data-stu-id="effb0-179">Enter *Y* to continue.</span></span>
   
    ![Erőforrások][resources]
   
    <span data-ttu-id="effb0-181">d.</span><span class="sxs-lookup"><span data-stu-id="effb0-181">d.</span></span> <span data-ttu-id="effb0-182">A parancsfájl a HPC Pack fürt telepítése elindul, és a konfiguráció további manuális lépések nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="effb0-182">The script starts to deploy the HPC Pack cluster and completes the configuration without further manual steps.</span></span> <span data-ttu-id="effb0-183">A parancsprogram futását néhány percig.</span><span class="sxs-lookup"><span data-stu-id="effb0-183">The script can run for several minutes.</span></span>
   
    ![Üzembe helyezés][deploy]
   
   > [!NOTE]
   > <span data-ttu-id="effb0-185">Ebben a példában a parancsfájl naplófájlt állítja elő automatikusan, mivel a **- LogFile** paraméter nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="effb0-185">In this example, the script generates a log file automatically since the **-LogFile** parameter isn't specified.</span></span> <span data-ttu-id="effb0-186">A naplók valós időben nem írt, de az érvényesítési és a telepítés végén gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="effb0-186">The logs aren't written in real time, but are collected at the end of the validation and the deployment.</span></span> <span data-ttu-id="effb0-187">Ha a PowerShell-folyamat leáll, a parancsfájl futása közben, néhány naplók elvesznek.</span><span class="sxs-lookup"><span data-stu-id="effb0-187">If the PowerShell process is stopped while the script is running, some logs are lost.</span></span>
   > 
   > 

## <a name="connect-to-the-head-node"></a><span data-ttu-id="effb0-188">A átjárócsomópontjához csatlakozik</span><span class="sxs-lookup"><span data-stu-id="effb0-188">Connect to the head node</span></span>
<span data-ttu-id="effb0-189">Miután telepítette az Azure, a HPC Pack fürt [csatlakozzon a távoli asztal](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) az átjárócsomóponthoz VM tartományba megadott hitelesítő adatok, amikor központilag telepítette a fürt (például *hpc\\clusteradmin*).</span><span class="sxs-lookup"><span data-stu-id="effb0-189">After you deploy the HPC Pack cluster in Azure, [connect by Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) to the head node VM using the domain credentials you provided when you deployed the cluster (for example, *hpc\\clusteradmin*).</span></span> <span data-ttu-id="effb0-190">A fürt kezelése a központi csomópontból.</span><span class="sxs-lookup"><span data-stu-id="effb0-190">You manage the cluster from the head node.</span></span>

<span data-ttu-id="effb0-191">Az átjárócsomópont indítsa el a HPC Pack fürt állapotának ellenőrzése a HPC Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="effb0-191">On the head node, start HPC Cluster Manager to check the status of the HPC Pack cluster.</span></span> <span data-ttu-id="effb0-192">Kezelheti, és a figyelő Linux számítási csomópontok használata Windows ugyanúgy számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="effb0-192">You can manage and monitor Linux compute nodes the same way you work with Windows compute nodes.</span></span> <span data-ttu-id="effb0-193">Például tekintse meg a Linux-csomópontok felsorolt **erőforrás-kezelés** (ezek a csomópontok telepítik a **LinuxNode** sablon).</span><span class="sxs-lookup"><span data-stu-id="effb0-193">For example, you see the Linux nodes listed in **Resource Management** (these nodes are deployed with the **LinuxNode** template).</span></span>

![Csomópont-felügyelet][management]

<span data-ttu-id="effb0-195">Linux csomópontján is megjelenik a **Hőtérkép** nézet.</span><span class="sxs-lookup"><span data-stu-id="effb0-195">You also see the Linux nodes in the **Heat Map** view.</span></span>

![Hőtérkép][heatmap]

## <a name="how-to-move-data-in-a-cluster-with-linux-nodes"></a><span data-ttu-id="effb0-197">Adatok áthelyezése Linux csomópontot tartalmazó fürtben</span><span class="sxs-lookup"><span data-stu-id="effb0-197">How to move data in a cluster with Linux nodes</span></span>
<span data-ttu-id="effb0-198">Számos választási lehetőség áll áthelyezni az adatokat a Linux-csomópontok és a központi Windows-csomópont, a fürt között.</span><span class="sxs-lookup"><span data-stu-id="effb0-198">You have several choices to move data among Linux nodes and the Windows head node of the cluster.</span></span> <span data-ttu-id="effb0-199">Az alábbiakban a három általános módszer, a következő szakaszokban részletesebben:</span><span class="sxs-lookup"><span data-stu-id="effb0-199">Here are three common methods, described in more detail in the following sections:</span></span>

* <span data-ttu-id="effb0-200">**Az Azure File** -közzétesz egy kezelt SMB-fájlmegosztás adatfájlok tárolására az Azure-tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="effb0-200">**Azure File** - Exposes a managed SMB file share to store data files in Azure storage.</span></span> <span data-ttu-id="effb0-201">Csomópontok Windows és Linux-csomópontjai is csatlakoztathatja egy Azure fájlmegosztás meghajtó vagy mappa egyszerre, még akkor is, ha azok a különböző virtuális hálózatokon van telepítve.</span><span class="sxs-lookup"><span data-stu-id="effb0-201">Windows nodes and Linux nodes can mount an Azure File share as a drive or folder at the same time, even if they're deployed in different virtual networks.</span></span>
* <span data-ttu-id="effb0-202">**Átjárócsomópont SMB-fájlmegosztás** -csatlakoztatja a központi csomópont szokásos Windows megosztott mappa Linux csomópontján.</span><span class="sxs-lookup"><span data-stu-id="effb0-202">**Head node SMB share** - Mounts a standard Windows shared folder of the head node on Linux nodes.</span></span>
* <span data-ttu-id="effb0-203">**HEAD csomópont NFS-kiszolgáló** -fájlmegosztási megoldást kínál a Windows és Linux használnak.</span><span class="sxs-lookup"><span data-stu-id="effb0-203">**Head node NFS server**  - Provides a file-sharing solution for a mixed Windows and Linux environment.</span></span>

### <a name="azure-file-storage"></a><span data-ttu-id="effb0-204">Az Azure File storage</span><span class="sxs-lookup"><span data-stu-id="effb0-204">Azure File storage</span></span>
<span data-ttu-id="effb0-205">A [Azure File](https://azure.microsoft.com/services/storage/files/) szolgáltatás elérhetővé teszi a standard SMB 2.1 protokollal fájlmegosztások.</span><span class="sxs-lookup"><span data-stu-id="effb0-205">The [Azure File](https://azure.microsoft.com/services/storage/files/) service exposes file shares using the standard SMB 2.1 protocol.</span></span> <span data-ttu-id="effb0-206">Azure virtuális gépek és felhőszolgáltatások megosztott csatlakoztatott megosztásokon keresztül alkalmazások összetevői között, és a helyszíni alkalmazások férhetnek hozzá olyan megosztáson található fájladatokat a File storage API használatával.</span><span class="sxs-lookup"><span data-stu-id="effb0-206">Azure VMs and cloud services can share file data across application components via mounted shares, and on-premises applications can access file data in a share through the File storage API.</span></span> 

<span data-ttu-id="effb0-207">Azure-fájlmegosztás létrehozása, és csatlakoztassa azt az átjárócsomópont részletes lépéseiért lásd: [Ismerkedés az Azure File storage on Windows](../../../storage/files/storage-how-to-use-files-windows.md).</span><span class="sxs-lookup"><span data-stu-id="effb0-207">For detailed steps to create an Azure File share and mount it on the head node, see [Get started with Azure File storage on Windows](../../../storage/files/storage-how-to-use-files-windows.md).</span></span> <span data-ttu-id="effb0-208">Azure fájlmegosztás csatlakoztatása a Linux-csomóponton, lásd: [Azure File storage használata Linux](../../../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="effb0-208">To mount the Azure File share on the Linux nodes, see [How to use Azure File storage with Linux](../../../storage/files/storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="effb0-209">Hozza létre tárolásakor kapcsolatokat, lásd: [Persisting kapcsolatok a Microsoft Azure fájlok](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).</span><span class="sxs-lookup"><span data-stu-id="effb0-209">To set up persisting connections, see [Persisting connections to Microsoft Azure Files](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).</span></span>

<span data-ttu-id="effb0-210">A storage-fiókok Azure-fájlmegosztás létrehozása a következő példában.</span><span class="sxs-lookup"><span data-stu-id="effb0-210">In the following example, create an Azure File share on a storage account.</span></span> <span data-ttu-id="effb0-211">A megosztás csatlakoztatásához a head csomóponton, nyisson meg egy parancssort, és írja be a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="effb0-211">To mount the share on the head node, open a Command Prompt and enter the following commands:</span></span>

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

<span data-ttu-id="effb0-212">Ebben a példában allvhdsje a tárfiók nevének storageaccountkey a tárfiók kulcsára, amely rdma az Azure fájlmegosztás neve.</span><span class="sxs-lookup"><span data-stu-id="effb0-212">In this example, allvhdsje is your storage account name, storageaccountkey is your storage account key, and rdma is the Azure File share name.</span></span> <span data-ttu-id="effb0-213">Az Azure fájlmegosztás az átjárócsomóponthoz, Z: van csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="effb0-213">The Azure File share is mounted as Z: on the head node.</span></span>

<span data-ttu-id="effb0-214">Azure fájlmegosztás csatlakoztatása Linux csomóponton, futtassa a **clusrun** az átjárócsomópont parancs.</span><span class="sxs-lookup"><span data-stu-id="effb0-214">To mount the Azure File share on Linux nodes, run a **clusrun** command on the head node.</span></span> <span data-ttu-id="effb0-215">**[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)**  az HPC Pack eszköz több csomóponton felügyeleti feladatokat hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="effb0-215">**[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)** is a useful HPC Pack tool to carry out administrative tasks on multiple nodes.</span></span> <span data-ttu-id="effb0-216">(Lásd még: [Clusrun Linux csomópontok](#Clusrun-for-Linux-nodes) ebben a cikkben.)</span><span class="sxs-lookup"><span data-stu-id="effb0-216">(See also [Clusrun for Linux nodes](#Clusrun-for-Linux-nodes) in this article.)</span></span>

<span data-ttu-id="effb0-217">Nyisson meg egy Windows PowerShell-ablakot, és írja be a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="effb0-217">Open a Windows PowerShell window and enter the following commands:</span></span>

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

<span data-ttu-id="effb0-218">Az első parancs létrehoz egy /rdma a LinuxNodes csoport minden csomópontján nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="effb0-218">The first command creates a folder named /rdma on all nodes in the LinuxNodes group.</span></span> <span data-ttu-id="effb0-219">A második parancs csatlakoztatja az Azure File megosztás allvhdsjw.file.core.windows.net/rdma alakzatot dir /rdma mappát, majd a fájl mód bitet 777.</span><span class="sxs-lookup"><span data-stu-id="effb0-219">The second command mounts the Azure File share allvhdsjw.file.core.windows.net/rdma onto the /rdma folder with dir and file mode bits set to 777.</span></span> <span data-ttu-id="effb0-220">A második parancs a allvhdsje a tárfiók neve pedig storageaccountkey a tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="effb0-220">In the second command, allvhdsje is your storage account name and storageaccountkey is your storage account key.</span></span>

> [!NOTE]
> <span data-ttu-id="effb0-221">A "\\`" szimbólumra a második parancs a értéke egy escape szimbólum PowerShell.</span><span class="sxs-lookup"><span data-stu-id="effb0-221">The “\\`” symbol in the second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="effb0-222">"\\`," azt jelenti, hogy a "," (vessző karakter) a parancs része.</span><span class="sxs-lookup"><span data-stu-id="effb0-222">“\\`,” means that the “,” (comma character) is a part of the command.</span></span>
> 
> 

### <a name="head-node-share"></a><span data-ttu-id="effb0-223">Átjárócsomópont megosztás</span><span class="sxs-lookup"><span data-stu-id="effb0-223">Head node share</span></span>
<span data-ttu-id="effb0-224">Másik lehetőségként csatlakoztatása egy megosztott mappába a központi csomópont Linux csomópontok.</span><span class="sxs-lookup"><span data-stu-id="effb0-224">Alternatively, mount a shared folder of the head node on Linux nodes.</span></span> <span data-ttu-id="effb0-225">Megosztás biztosít a legegyszerűbben úgy osszanak meg fájlokat, de az átjárócsomópont és az összes Linux csomópontra az azonos virtuális hálózatban kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="effb0-225">A share provides the simplest way to share files, but the head node and all Linux nodes must be deployed in the same virtual network.</span></span> <span data-ttu-id="effb0-226">A lépések a következők.</span><span class="sxs-lookup"><span data-stu-id="effb0-226">Here are the steps.</span></span>

1. <span data-ttu-id="effb0-227">Hozzon létre egy mappát az átjárócsomóponthoz, és ossza meg az olvasási/írási engedéllyel rendelkező összes felhasználó.</span><span class="sxs-lookup"><span data-stu-id="effb0-227">Create a folder on the head node and share it to Everyone with Read/Write permissions.</span></span> <span data-ttu-id="effb0-228">Például, megosztás, az átjárócsomópont D:\OpenFOAM \\CentOS7RDMA-HN\OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="effb0-228">For example, share D:\OpenFOAM on the head node as \\CentOS7RDMA-HN\OpenFOAM.</span></span> <span data-ttu-id="effb0-229">A HN CentOS7RDMA Ez az átjárócsomópont állomásnevét.</span><span class="sxs-lookup"><span data-stu-id="effb0-229">Here CentOS7RDMA-HN is the hostname of the head node.</span></span>
   
    ![Fájlmegosztási engedélyek][fileshareperms]
   
    ![Fájlmegosztás][filesharing]
2. <span data-ttu-id="effb0-232">Nyisson meg egy Windows PowerShell-ablakot, és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="effb0-232">Open a Windows PowerShell window and run the following commands:</span></span>
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

<span data-ttu-id="effb0-233">Az első parancs létrehoz egy /openfoam a LinuxNodes csoport minden csomópontján nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="effb0-233">The first command creates a folder named /openfoam on all nodes in the LinuxNodes group.</span></span> <span data-ttu-id="effb0-234">A második parancs csatlakoztatja a megosztott mappa //CentOS7RDMA-HN/OpenFOAM alakzatot dir mappát, majd fájl mód bitet 777.</span><span class="sxs-lookup"><span data-stu-id="effb0-234">The second command mounts the shared folder //CentOS7RDMA-HN/OpenFOAM onto the folder with dir and file mode bits set to 777.</span></span> <span data-ttu-id="effb0-235">A felhasználónevet és jelszót a parancsban a felhasználónevet és jelszót egy fürt átjárócsomópontjához felhasználójának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="effb0-235">The username and password in the command should be the username and password of a cluster user on the head node.</span></span> <span data-ttu-id="effb0-236">(Lásd: [hozzáadását vagy eltávolítását fürt felhasználók](https://technet.microsoft.com/library/ff919330.aspx).)</span><span class="sxs-lookup"><span data-stu-id="effb0-236">(See [Add or remove cluster users](https://technet.microsoft.com/library/ff919330.aspx).)</span></span>

> [!NOTE]
> <span data-ttu-id="effb0-237">A "\\`" szimbólumra a második parancs a értéke egy escape szimbólum PowerShell.</span><span class="sxs-lookup"><span data-stu-id="effb0-237">The “\\`” symbol in the second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="effb0-238">"\\`," azt jelenti, hogy a "," (vessző karakter) a parancs része.</span><span class="sxs-lookup"><span data-stu-id="effb0-238">“\\`,” means that the “,” (comma character) is a part of the command.</span></span>
> 
> 

### <a name="nfs-server"></a><span data-ttu-id="effb0-239">NFS-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="effb0-239">NFS server</span></span>
<span data-ttu-id="effb0-240">Az NFS-szolgáltatás lehetővé teszi a megosztás és az SMB protokollt használó Windows Server 2012 operációs rendszert futtató számítógépekhez és az NFS-protokollt használó Linux-alapú számítógépek közötti áttelepítése.</span><span class="sxs-lookup"><span data-stu-id="effb0-240">The NFS service enables you to share and migrate files between computers running the Windows Server 2012 operating system using the SMB protocol and Linux-based computers using the NFS protocol.</span></span> <span data-ttu-id="effb0-241">Az NFS-kiszolgáló és a többi csomópontnak kell ugyanabban a virtuális hálózatban üzembe helyezhető.</span><span class="sxs-lookup"><span data-stu-id="effb0-241">The NFS server and all other nodes have to be deployed in the same virtual network.</span></span> <span data-ttu-id="effb0-242">Linux-csomópontokkal SMB-megosztáson képest jobb kompatibilitást biztosít.</span><span class="sxs-lookup"><span data-stu-id="effb0-242">It provides better compatibility with Linux nodes compared with an SMB share.</span></span> <span data-ttu-id="effb0-243">Például fájlhivatkozások támogatja.</span><span class="sxs-lookup"><span data-stu-id="effb0-243">For example, it supports file links.</span></span>

1. <span data-ttu-id="effb0-244">Telepítse és állítsa be az NFS-kiszolgáló, kövesse a lépéseket a [Server, a rendszer első Fájlmegosztására-végpontok](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).</span><span class="sxs-lookup"><span data-stu-id="effb0-244">To install and set up an NFS server, follow the steps in [Server for Network File System First Share End-to-End](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).</span></span>
   
    <span data-ttu-id="effb0-245">Hozzon létre például egy NFS-megosztás nevű nfs a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="effb0-245">For example, create an NFS share named nfs with the following properties:</span></span>
   
    ![NFS-engedélyezés][nfsauth]
   
    ![NFS-megosztás engedélyeinek][nfsshare]
   
    ![NFS-NTFS-engedélyek][nfsperm]
   
    ![NFS-felügyeleti tulajdonságai][nfsmanage]
2. <span data-ttu-id="effb0-250">Nyisson meg egy Windows PowerShell-ablakot, és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="effb0-250">Open a Windows PowerShell window and run the following commands:</span></span>
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
   
    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```
   
   <span data-ttu-id="effb0-251">Az első parancs létrehoz egy /nfsshared a LinuxNodes csoport minden csomópontján nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="effb0-251">The first command creates a folder named /nfsshared on all nodes in the LinuxNodes group.</span></span> <span data-ttu-id="effb0-252">A második parancs csatlakoztatja az NFS-megosztás CentOS7RDMA-HN: / nfs alakzatot a mappát.</span><span class="sxs-lookup"><span data-stu-id="effb0-252">The second command mounts the NFS share CentOS7RDMA-HN:/nfs onto the folder.</span></span> <span data-ttu-id="effb0-253">Itt CentOS7RDMA-HN: / nfs értéke az NFS-megosztás távoli elérési útját.</span><span class="sxs-lookup"><span data-stu-id="effb0-253">Here CentOS7RDMA-HN:/nfs is the remote path of your NFS share.</span></span>

## <a name="how-to-submit-jobs"></a><span data-ttu-id="effb0-254">Hogyan feladatok elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="effb0-254">How to submit jobs</span></span>
<span data-ttu-id="effb0-255">Többféleképpen is lehet a HPC Pack fürt feladatok elküldéséhez:</span><span class="sxs-lookup"><span data-stu-id="effb0-255">There are several ways to submit jobs to the HPC Pack cluster:</span></span>

* <span data-ttu-id="effb0-256">HPC Fürtkezelő vagy a HPC Job Manager eszköz grafikus felhasználói felületen</span><span class="sxs-lookup"><span data-stu-id="effb0-256">HPC Cluster Manager or HPC Job Manager GUI</span></span>
* <span data-ttu-id="effb0-257">HPC-webportál</span><span class="sxs-lookup"><span data-stu-id="effb0-257">HPC web portal</span></span>
* <span data-ttu-id="effb0-258">REST API</span><span class="sxs-lookup"><span data-stu-id="effb0-258">REST API</span></span>

<span data-ttu-id="effb0-259">Feladat elküldése a fürthöz az Azure-ban HPC Pack grafikus eszközöket és a HPC-webportál keresztül ugyanaz, mint a Windows számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="effb0-259">Job submission to the cluster in Azure via HPC Pack GUI tools and the HPC web portal are the same as for Windows compute nodes.</span></span> <span data-ttu-id="effb0-260">Lásd: [HPC Pack Feladatkezelő](https://technet.microsoft.com/library/ff919691.aspx) és [egy helyszíni ügyfélszámítógépről feladatok küldéséhez hogyan](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="effb0-260">See [HPC Pack Job Manager](https://technet.microsoft.com/library/ff919691.aspx) and [How to submit jobs from an on-premises client computer](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="effb0-261">A REST API-n keresztül feladatok küldéséhez, tekintse meg [létrehozása és elküldése feladatok a REST API használatával a Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span><span class="sxs-lookup"><span data-stu-id="effb0-261">To submit jobs via the REST API, refer to [Creating and Submitting Jobs by Using the REST API in Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span></span> <span data-ttu-id="effb0-262">A Linux-ügyfélben feladatok küldéséhez, is tekintse meg a Python minta a [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).</span><span class="sxs-lookup"><span data-stu-id="effb0-262">To submit jobs from a Linux client, also refer to the Python sample in the [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).</span></span>

## <a name="clusrun-for-linux-nodes"></a><span data-ttu-id="effb0-263">Linux-csomópontok Clusrun</span><span class="sxs-lookup"><span data-stu-id="effb0-263">Clusrun for Linux nodes</span></span>
<span data-ttu-id="effb0-264">A HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) parancsok végrehajtása Linux csomópontokon keresztül, a parancssor vagy a HPC Cluster Manager eszköz használható.</span><span class="sxs-lookup"><span data-stu-id="effb0-264">The HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) tool can be used to execute commands on Linux nodes either through a Command Prompt or HPC Cluster Manager.</span></span> <span data-ttu-id="effb0-265">A következő példák alapvető.</span><span class="sxs-lookup"><span data-stu-id="effb0-265">Following are some basic examples.</span></span>

* <span data-ttu-id="effb0-266">Az aktuális felhasználónevek megjelenítése a fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="effb0-266">Show current user names on all nodes in the cluster.</span></span>
  
    ```command
    clusrun whoami
    ```
* <span data-ttu-id="effb0-267">Telepítse a **gdb** hibakereső eszközt a **yum** összes a linuxnodes csomópontja csoportban, majd indítsa újra a csomópontok 10 perc múlva.</span><span class="sxs-lookup"><span data-stu-id="effb0-267">Install the **gdb** debugger tool with **yum** on all nodes in the linuxnodes group and then restart the nodes after 10 minutes.</span></span>
  
    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```
* <span data-ttu-id="effb0-268">Hozzon létre egy héjparancsfájlt, a fürt minden egyes számának 1 és 10 minden egyes Linux csomóponton egy második megjelenítése, majd futtassa és kimeneti csomópontjából azonnal megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="effb0-268">Create a shell script displaying each number 1 through 10 for one second on each Linux node in the cluster, run it, and show output from the nodes immediately.</span></span>
  
    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

> [!NOTE]
> <span data-ttu-id="effb0-269">Szükség lehet egyes escape karakter használandó **clusrun** parancsok.</span><span class="sxs-lookup"><span data-stu-id="effb0-269">You might need to use certain escape characters in **clusrun** commands.</span></span> <span data-ttu-id="effb0-270">Ebben a példában látható, használjon ^ parancssorban karaktert a ">" szimbólumot.</span><span class="sxs-lookup"><span data-stu-id="effb0-270">As shown in this example, use ^ in a Command Prompt to escape the ">" symbol.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="effb0-271">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="effb0-271">Next steps</span></span>
* <span data-ttu-id="effb0-272">Próbálkozzon a fürt a csomópontok nagyobb számot vertikális felskálázásával, vagy a Linux-munkaterhelések futtatásával a fürtön.</span><span class="sxs-lookup"><span data-stu-id="effb0-272">Try scaling up the cluster to a larger number of nodes, or try running a Linux workload on the cluster.</span></span> <span data-ttu-id="effb0-273">Egy vonatkozó példáért lásd: [NAMD futtassa a Microsoft HPC Pack Linux számítási csomópontok az Azure-ban](hpcpack-cluster-namd.md).</span><span class="sxs-lookup"><span data-stu-id="effb0-273">For an example, see [Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure](hpcpack-cluster-namd.md).</span></span>
* <span data-ttu-id="effb0-274">Próbálja meg a fürt [RDMA-kompatibilisek-e, a számítási igényű virtuális gépek](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) MPI munkaterhelések futtatásához.</span><span class="sxs-lookup"><span data-stu-id="effb0-274">Try a cluster with [RDMA-capable, compute-intensive VMs](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) to run MPI workloads.</span></span> <span data-ttu-id="effb0-275">Egy vonatkozó példáért lásd: [OpenFOAM futtassa a Microsoft HPC Pack egy Linux RDMA a fürtön, az Azure-ban](hpcpack-cluster-openfoam.md).</span><span class="sxs-lookup"><span data-stu-id="effb0-275">For an example, see [Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure](hpcpack-cluster-openfoam.md).</span></span>
* <span data-ttu-id="effb0-276">Ha érdekli a helyszíni HPC Pack fürtben lévő Linux-csomópont működik, tekintse meg a [TechNet útmutatást](https://technet.microsoft.com/library/mt595803.aspx).</span><span class="sxs-lookup"><span data-stu-id="effb0-276">If you are interested in working with Linux nodes in an on-premises HPC Pack cluster, see the [TechNet guidance](https://technet.microsoft.com/library/mt595803.aspx).</span></span>

<!--Image references-->
[scenario]:media/hpcpack-cluster/scenario.png
[portal]:media/hpcpack-cluster/portal.png
[validate]:media/hpcpack-cluster/validate.png
[resources]:media/hpcpack-cluster/resources.png
[deploy]:media/hpcpack-cluster/deploy.png
[management]:media/hpcpack-cluster/management.png
[heatmap]:media/hpcpack-cluster/heatmap.png
[fileshareperms]:media/hpcpack-cluster/fileshare1.png
[filesharing]:media/hpcpack-cluster/fileshare2.png
[nfsauth]:media/hpcpack-cluster/nfsauth.png
[nfsshare]:media/hpcpack-cluster/nfsshare.png
[nfsperm]:media/hpcpack-cluster/nfsperm.png
[nfsmanage]:media/hpcpack-cluster/nfsmanage.png
