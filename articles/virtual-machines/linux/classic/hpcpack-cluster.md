---
title: "aaaLinux a HPC Pack-fürtben lévő virtuális gépek számítási |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate használja egy HPC Pack fürtön és az Azure-ban a Linux nagy teljesítményű számítástechnikai (HPC) munkaterhelések"
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
ms.openlocfilehash: 9ed20d6cd69a6472a00666caf8965e9d022698a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="1642e-103">Az Azure-beli HPC Pack-fürtök linuxos számítási csomópontjaival kapcsolatos alapvető tudnivalók</span><span class="sxs-lookup"><span data-stu-id="1642e-103">Get started with Linux compute nodes in an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="1642e-104">Állítson be egy [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) Azure egy központi fut a Windows Server és több csomópontot tartalmazó fürt számítási csomópontjain futó támogatott Linux-disztribúció.</span><span class="sxs-lookup"><span data-stu-id="1642e-104">Set up a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) cluster in Azure that contains a head node running Windows Server and several compute nodes running a supported Linux distribution.</span></span> <span data-ttu-id="1642e-105">Adatokba beállítások toomove hello Linux csomópontokat és hello Windows átjárócsomópont hello fürt között.</span><span class="sxs-lookup"><span data-stu-id="1642e-105">Explore options toomove data among hello Linux nodes and hello Windows head node of hello cluster.</span></span> <span data-ttu-id="1642e-106">Ismerje meg, hogyan toosubmit Linux HPC feladatok toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="1642e-106">Learn how toosubmit Linux HPC jobs toohello cluster.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="1642e-107">Magas szinten, hello alábbi ábrán látható hello HPC Pack fürt létrehozása és használata.</span><span class="sxs-lookup"><span data-stu-id="1642e-107">At a high level, hello following diagram shows hello HPC Pack cluster you create and work with.</span></span>

![Linux-csomópontok HPC Pack fürt][scenario]

<span data-ttu-id="1642e-109">Az egyéb beállítások toorun Linux HPC munkaterhelések az Azure, lásd: [kötegelt és nagy teljesítményű számítástechnikai rendszerek technikai erőforrások](../../../batch/big-compute-resources.md).</span><span class="sxs-lookup"><span data-stu-id="1642e-109">For other options toorun Linux HPC workloads in Azure, see [Technical resources for batch and high-performance computing](../../../batch/big-compute-resources.md).</span></span>

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a><span data-ttu-id="1642e-110">HPC Pack-fürt üzembe helyezése a Linux számítási csomópont</span><span class="sxs-lookup"><span data-stu-id="1642e-110">Deploy an HPC Pack cluster with Linux compute nodes</span></span>
<span data-ttu-id="1642e-111">Ez a cikk bemutatja két lehetőség közül választhat toodeploy egy HPC Pack fürt Azure Linux számítási csomópontokkal.</span><span class="sxs-lookup"><span data-stu-id="1642e-111">This article shows you two options toodeploy an HPC Pack cluster in Azure with Linux compute nodes.</span></span> <span data-ttu-id="1642e-112">Mindkét módszer a Windows Server Piactéri rendszerkép HPC Pack toocreate hello átjárócsomópont használja.</span><span class="sxs-lookup"><span data-stu-id="1642e-112">Both methods use a Marketplace image of Windows Server with HPC Pack toocreate hello head node.</span></span> 

* <span data-ttu-id="1642e-113">**Az Azure Resource Manager-sablon** -hello Azure piactér egy sablon, vagy a következő gyorsindítási sablonon hello Közösségtől hello fürt hello Resource Manager üzembe helyezési modellel tooautomate létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1642e-113">**Azure Resource Manager template** - Use a template from hello Azure Marketplace, or a quickstart template from hello community, tooautomate creation of hello cluster in hello Resource Manager deployment model.</span></span> <span data-ttu-id="1642e-114">Például hello [HPC Pack fürt Linux munkaterhelések](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) hello Azure piactér-sablon hoz létre egy teljes HPC Pack fürt infrastruktúra Linux HPC a munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="1642e-114">For example, hello [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) template in hello Azure Marketplace creates a complete HPC Pack cluster infrastructure for Linux HPC workloads.</span></span>
* <span data-ttu-id="1642e-115">**PowerShell parancsfájl** -használata hello [Microsoft HPC Pack IaaS telepítési parancsfájl](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**New-HpcIaaSCluster.ps1**) tooautomate a teljes fürtre történő telepítés hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="1642e-115">**PowerShell script** - Use hello [Microsoft HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**New-HpcIaaSCluster.ps1**) tooautomate a complete cluster deployment in hello classic deployment model.</span></span> <span data-ttu-id="1642e-116">Az Azure PowerShell-parancsfájl HPC Pack VM-lemezkép hello Azure piactér a gyors telepítés használ, és számos átfogó konfigurációs paraméterek toodeploy Linux számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="1642e-116">This Azure PowerShell script uses an HPC Pack VM image in hello Azure Marketplace for fast deployment and provides a comprehensive set of configuration parameters toodeploy Linux compute nodes.</span></span>

<span data-ttu-id="1642e-117">További információ a HPC Pack fürt telepítési lehetőségek az Azure-ban: [toocreate lehetőségek és az Azure-ban a Microsoft HPC Pack nagy teljesítményű számítástechnikai rendszerek (HPC) fürt felügyeletére](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1642e-117">For more information about HPC Pack cluster deployment options in Azure, see [Options toocreate and manage a high-performance computing (HPC) cluster in Azure with Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

### <a name="prerequisites"></a><span data-ttu-id="1642e-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1642e-118">Prerequisites</span></span>
* <span data-ttu-id="1642e-119">**Azure-előfizetés** -előfizetés vagy hello Azure globális vagy Azure Kína szolgáltatás használhatja.</span><span class="sxs-lookup"><span data-stu-id="1642e-119">**Azure subscription** - You can use a subscription in either hello Azure Global or Azure China service.</span></span> <span data-ttu-id="1642e-120">Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/) néhány percig.</span><span class="sxs-lookup"><span data-stu-id="1642e-120">If you don't have an account, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.</span></span>
* <span data-ttu-id="1642e-121">**Magok kvóta** -szükség lehet tooincrease hello beállított kvótát mag, különösen akkor, ha úgy dönt, toodeploy multicore Virtuálisgép-méretek a több fürtcsomóponton.</span><span class="sxs-lookup"><span data-stu-id="1642e-121">**Cores quota** - You might need tooincrease hello quota of cores, especially if you choose toodeploy several cluster nodes with multicore VM sizes.</span></span> <span data-ttu-id="1642e-122">tooincrease kvóta, nyissa meg az online támogatás ügyfélkérés díjmentesen.</span><span class="sxs-lookup"><span data-stu-id="1642e-122">tooincrease a quota, open an online customer support request at no charge.</span></span>
* <span data-ttu-id="1642e-123">**Linux-disztribúció** -jelenleg HPC Pack támogatja a következő Linux terjesztésekről, a számítási csomópontok hello.</span><span class="sxs-lookup"><span data-stu-id="1642e-123">**Linux distributions** - Currently HPC Pack supports hello following Linux distributions for compute nodes.</span></span> <span data-ttu-id="1642e-124">Ezek terjesztéseket piactér verzióját használják, ahol az rendelkezésre áll, vagy adja meg a saját.</span><span class="sxs-lookup"><span data-stu-id="1642e-124">You can use Marketplace versions of these distributions where available, or supply your own.</span></span>
  
  * <span data-ttu-id="1642e-125">**CentOS-alapú**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1-es HPC</span><span class="sxs-lookup"><span data-stu-id="1642e-125">**CentOS-based**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC</span></span>
  * <span data-ttu-id="1642e-126">**Red Hat Enterprise Linux**: 6.7, 6.8, 7.2</span><span class="sxs-lookup"><span data-stu-id="1642e-126">**Red Hat Enterprise Linux**: 6.7, 6.8, 7.2</span></span>
  * <span data-ttu-id="1642e-127">**SUSE Linux Enterprise Server**: SLES 12, a SLES 12 (prémium), a SLES 12 SP1, a SLES 12 SP1 (prémium) SLES 12 a HPC, SLES 12 a HPC (prémium)</span><span class="sxs-lookup"><span data-stu-id="1642e-127">**SUSE Linux Enterprise Server**: SLES 12, SLES 12 (Premium), SLES 12 SP1, SLES 12 SP1 (Premium), SLES 12 for HPC, SLES 12 for HPC (Premium)</span></span>
  * <span data-ttu-id="1642e-128">**Ubuntu Server**: 14.04 LTS, 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="1642e-128">**Ubuntu Server**: 14.04 LTS, 16.04 LTS</span></span>
    
    > [!TIP]
    > <span data-ttu-id="1642e-129">toouse hello Azure RDMA hálózati egy Virtuálisgép-méretek hello RDMA-kompatibilisek-e, adjon meg a hello Azure Piactérről származó SUSE Linux Enterprise Server 12 HPC vagy CentOS-alapú HPC-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="1642e-129">toouse hello Azure RDMA network with one of hello RDMA-capable VM sizes, specify a SUSE Linux Enterprise Server 12 HPC or CentOS-based HPC image from hello Azure Marketplace.</span></span> <span data-ttu-id="1642e-130">További információkért lásd: [nagy teljesítményű számítási Virtuálisgép-méretek](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1642e-130">For more information, see [High performance compute VM sizes](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
    > 
    > 

<span data-ttu-id="1642e-131">További Előfeltételek toodeploy hello fürt hello HPC Pack IaaS telepítési parancsfájl használatával:</span><span class="sxs-lookup"><span data-stu-id="1642e-131">Additional prerequisites toodeploy hello cluster by using hello HPC Pack IaaS deployment script:</span></span>

* <span data-ttu-id="1642e-132">**Ügyfélszámítógép** -ügyfél Windows-alapú számítógép toorun hello fürt telepítési parancsfájl van szüksége.</span><span class="sxs-lookup"><span data-stu-id="1642e-132">**Client computer** - You need a Windows-based client computer toorun hello cluster deployment script.</span></span>
* <span data-ttu-id="1642e-133">**Az Azure PowerShell** - [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview) (0.8.10 verzió vagy újabb) az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="1642e-133">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="1642e-134">**HPC Pack IaaS telepítési parancsfájl** - töltse le és csomagolja ki a legújabb verziójának hello hello hello parancsfájlt [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="1642e-134">**HPC Pack IaaS deployment script** - Download and unpack hello latest version of hello script from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="1642e-135">Hello verziója hello parancsfájl futtatásával ellenőrizheti `.\New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="1642e-135">You can check hello version of hello script by running `.\New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="1642e-136">Ez a cikk 4.4.1 verzió vagy újabb hello parancsfájl alapul.</span><span class="sxs-lookup"><span data-stu-id="1642e-136">This article is based on version 4.4.1 or later of hello script.</span></span>

### <a name="deployment-option-1-use-a-resource-manager-template"></a><span data-ttu-id="1642e-137">1. telepítési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1642e-137">Deployment option 1.</span></span> <span data-ttu-id="1642e-138">A Resource Manager-sablon használata</span><span class="sxs-lookup"><span data-stu-id="1642e-138">Use a Resource Manager template</span></span>
1. <span data-ttu-id="1642e-139">Nyissa meg toohello [HPC Pack fürt Linux munkaterhelések](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) sablon hello Azure piactéren, és kattintson **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="1642e-139">Go toohello [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) template in hello Azure Marketplace, and click **Deploy**.</span></span>
2. <span data-ttu-id="1642e-140">Az Azure-portálon hello, hello tekintse át, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="1642e-140">In hello Azure portal, review hello information and then click **Create**.</span></span>
   
    ![Portál létrehozásához][portal]
3. <span data-ttu-id="1642e-142">A hello **alapjai** panelen adjon meg egy nevet hello fürtté is a hello átjárócsomópont virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="1642e-142">On hello **Basics** blade, enter a name for hello cluster, which also names hello head node VM.</span></span> <span data-ttu-id="1642e-143">Válasszon egy meglévő erőforráscsoportot, vagy olyan helyre, amely elérhető tooyou csoport hello központi telepítés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1642e-143">You can choose an existing resource group or create a group for hello deployment in a location that's available tooyou.</span></span> <span data-ttu-id="1642e-144">hello hely befolyásolja hello rendelkezésre állását, valamint az egyes Virtuálisgép-méretek más Azure-szolgáltatásokkal (lásd: [régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services/)).</span><span class="sxs-lookup"><span data-stu-id="1642e-144">hello location affects hello availability of certain VM sizes and other Azure services (see [Products available by region](https://azure.microsoft.com/regions/services/)).</span></span>
4. <span data-ttu-id="1642e-145">A hello **csomópont beállítások Head** panelen első üzembe helyezés esetén általában elfogadhatja hello alapértelmezett beállításokat.</span><span class="sxs-lookup"><span data-stu-id="1642e-145">On hello **Head node settings** blade, for a first deployment, you can generally accept hello default settings.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="1642e-146">Hello **utáni konfigurációs parancsprogram URL-Címének** van egy nem kötelező beállítás toospecify egy nyilvánosan elérhető Windows PowerShell-parancsfájlt, amelyet a hello átjárócsomópont VM toorun után fut-e.</span><span class="sxs-lookup"><span data-stu-id="1642e-146">hello **Post-configuration script URL** is an optional setting toospecify a publicly available Windows PowerShell script that you want toorun on hello head node VM after it is running.</span></span> 
   > 
   > 
5. <span data-ttu-id="1642e-147">A hello **számítási csomópont beállítások** panelen hello csomópontok, hello száma és mérete hello csomópontok egy elnevezési mintát választ, és Linux terjesztési toodeploy hello.</span><span class="sxs-lookup"><span data-stu-id="1642e-147">On hello **Compute node settings** blade, select a naming pattern for hello nodes, hello number and size of hello nodes, and hello Linux distribution toodeploy.</span></span>
6. <span data-ttu-id="1642e-148">A hello **infrastruktúrabeállításai** panelen írja be a hello virtuális hálózat és az Active Directory tartomány, a tartomány és a virtuális gép rendszergazdai hitelesítő adatokat és a egy elnevezési mintája hello storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="1642e-148">On hello **Infrastructure settings** blade, enter names for hello virtual network and Active Directory domain, domain and VM administrator credentials, and a naming pattern for hello storage accounts.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1642e-149">HPC Pack hello Active Directory tartományi tooauthenticate fürt felhasználók használja.</span><span class="sxs-lookup"><span data-stu-id="1642e-149">HPC Pack uses hello Active Directory domain tooauthenticate cluster users.</span></span> 
   > 
   > 
7. <span data-ttu-id="1642e-150">Miután hello Érvényesítési tesztek futtatásához, és áttekintheti a hello használati feltételeket, kattintson **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="1642e-150">After hello validation tests run and you review hello terms of use, click **Purchase**.</span></span>

### <a name="deployment-option-2-use-hello-iaas-deployment-script"></a><span data-ttu-id="1642e-151">Telepítési beállítás 2.</span><span class="sxs-lookup"><span data-stu-id="1642e-151">Deployment option 2.</span></span> <span data-ttu-id="1642e-152">Hello IaaS telepítési parancsfájl használata</span><span class="sxs-lookup"><span data-stu-id="1642e-152">Use hello IaaS deployment script</span></span>
<span data-ttu-id="1642e-153">Az alábbiakban további Előfeltételek toodeploy hello fürt hello HPC Pack IaaS telepítési parancsfájl használatával:</span><span class="sxs-lookup"><span data-stu-id="1642e-153">Following are additional prerequisites toodeploy hello cluster by using hello HPC Pack IaaS deployment script:</span></span>

* <span data-ttu-id="1642e-154">**Ügyfélszámítógép** -ügyfél Windows-alapú számítógép toorun hello fürt telepítési parancsfájl van szüksége.</span><span class="sxs-lookup"><span data-stu-id="1642e-154">**Client computer** - You need a Windows-based client computer toorun hello cluster deployment script.</span></span>
* <span data-ttu-id="1642e-155">**Az Azure PowerShell** - [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview) (0.8.10 verzió vagy újabb) az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="1642e-155">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="1642e-156">**HPC Pack IaaS telepítési parancsfájl** - töltse le és csomagolja ki a legújabb verziójának hello hello hello parancsfájlt [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="1642e-156">**HPC Pack IaaS deployment script** - Download and unpack hello latest version of hello script from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="1642e-157">Hello verziója hello parancsfájl futtatásával ellenőrizheti `.\New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="1642e-157">You can check hello version of hello script by running `.\New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="1642e-158">Ez a cikk 4.4.1 verzió vagy újabb hello parancsfájl alapul.</span><span class="sxs-lookup"><span data-stu-id="1642e-158">This article is based on version 4.4.1 or later of hello script.</span></span>

<span data-ttu-id="1642e-159">**Konfigurációs XML-fájl**</span><span class="sxs-lookup"><span data-stu-id="1642e-159">**XML configuration file**</span></span>

<span data-ttu-id="1642e-160">hello HPC Pack IaaS telepítési parancsfájl egy konfigurációs XML-fájl bemeneti toodescribe hello HPC-fürt használja.</span><span class="sxs-lookup"><span data-stu-id="1642e-160">hello HPC Pack IaaS deployment script uses an XML configuration file as input toodescribe hello  HPC cluster.</span></span> <span data-ttu-id="1642e-161">hello következő minta konfigurációs fájlt adja meg egy HPC Pack átjárócsomópont és a számítási csomópontok két mérete A7 CentOS 7.0 Linux kisebb fürt.</span><span class="sxs-lookup"><span data-stu-id="1642e-161">hello following sample configuration file specifies a small cluster consisting of an HPC Pack head node and two size A7 CentOS 7.0 Linux compute nodes.</span></span> 

<span data-ttu-id="1642e-162">A környezet és a kívánt fürtkonfigurációt szükség szerint módosítsa a hello fájlt, és mentse a neve, pl. HPCDemoConfig.xml.</span><span class="sxs-lookup"><span data-stu-id="1642e-162">Modify hello file as needed for your environment and desired cluster configuration, and save it with a name such as HPCDemoConfig.xml.</span></span> <span data-ttu-id="1642e-163">Például toosupply kell, az előfizetés nevét és egy egyedi tárfiók neve vagy a felhőszolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="1642e-163">For example, you need toosupply your subscription name and a unique storage account name and cloud service name.</span></span> <span data-ttu-id="1642e-164">Emellett érdemes lehet egy másik toochoose támogatott Linux kép hello számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="1642e-164">Additionally, you might want toochoose a different supported Linux image for hello compute nodes.</span></span> <span data-ttu-id="1642e-165">Hello elemek hello konfigurációs fájlban kapcsolatos további információkért lásd: hello Manual.rtf fájlban hello parancsfájl és [HPC-fürt létrehozása hello HPC Pack IaaS telepítési parancsfájl](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1642e-165">For more information about hello elements in hello configuration file, see hello Manual.rtf file in hello script folder and [Create an HPC cluster with hello HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

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

<span data-ttu-id="1642e-166">**toorun hello HPC Pack IaaS telepítési parancsfájl**</span><span class="sxs-lookup"><span data-stu-id="1642e-166">**toorun hello HPC Pack IaaS deployment script**</span></span>

1. <span data-ttu-id="1642e-167">Nyissa meg a Windows PowerShell hello ügyfélszámítógépen rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1642e-167">Open Windows PowerShell on hello client computer as an administrator.</span></span>
2. <span data-ttu-id="1642e-168">Változás directory toohello mappát, ahol hello parancsfájl telepítve (E:\IaaSClusterScript ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="1642e-168">Change directory toohello folder where hello script is installed (E:\IaaSClusterScript in this example).</span></span>
   
    ```powershell
    cd E:\IaaSClusterScript
    ```
3. <span data-ttu-id="1642e-169">Futtassa a következő parancs toodeploy hello HPC Pack fürt hello.</span><span class="sxs-lookup"><span data-stu-id="1642e-169">Run hello following command toodeploy hello HPC Pack cluster.</span></span> <span data-ttu-id="1642e-170">Ebben a példában feltételezzük, hogy hello a konfigurációs fájl található E:\HPCDemoConfig.xml</span><span class="sxs-lookup"><span data-stu-id="1642e-170">This example assumes that hello configuration file is located in E:\HPCDemoConfig.xml</span></span>
   
    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```
   
    <span data-ttu-id="1642e-171">a.</span><span class="sxs-lookup"><span data-stu-id="1642e-171">a.</span></span> <span data-ttu-id="1642e-172">Mivel hello **AdminPassword** nincs megadva hello megelőző parancs, a felhasználó jelszava felszólító tooenter hello *MyAdminName*.</span><span class="sxs-lookup"><span data-stu-id="1642e-172">Because hello **AdminPassword** is not specified in hello preceding command, you are prompted tooenter hello password for user *MyAdminName*.</span></span>
   
    <span data-ttu-id="1642e-173">b.</span><span class="sxs-lookup"><span data-stu-id="1642e-173">b.</span></span> <span data-ttu-id="1642e-174">hello parancsfájl ezután elindítja a toovalidate hello konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="1642e-174">hello script then starts toovalidate hello configuration file.</span></span> <span data-ttu-id="1642e-175">Attól függően, hogy a hálózati kapcsolat hello tooseveral percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="1642e-175">It can take up tooseveral minutes depending on hello network connection.</span></span>
   
    ![Ellenőrzés][validate]
   
    <span data-ttu-id="1642e-177">c.</span><span class="sxs-lookup"><span data-stu-id="1642e-177">c.</span></span> <span data-ttu-id="1642e-178">Érvényesítést átadni, miután hello parancsfájl hello fürt erőforrások toocreate sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="1642e-178">After validations pass, hello script lists hello cluster resources toocreate.</span></span> <span data-ttu-id="1642e-179">Adja meg *Y* toocontinue.</span><span class="sxs-lookup"><span data-stu-id="1642e-179">Enter *Y* toocontinue.</span></span>
   
    ![Erőforrások][resources]
   
    <span data-ttu-id="1642e-181">d.</span><span class="sxs-lookup"><span data-stu-id="1642e-181">d.</span></span> <span data-ttu-id="1642e-182">hello parancsfájl toodeploy hello HPC Pack fürt elindul, és további manuális lépések nélkül hello konfigurálása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="1642e-182">hello script starts toodeploy hello HPC Pack cluster and completes hello configuration without further manual steps.</span></span> <span data-ttu-id="1642e-183">hello parancsfájlt néhány percig.</span><span class="sxs-lookup"><span data-stu-id="1642e-183">hello script can run for several minutes.</span></span>
   
    ![Üzembe helyezés][deploy]
   
   > [!NOTE]
   > <span data-ttu-id="1642e-185">Ebben a példában hello parancsfájl naplófájlt állítja elő automatikusan óta hello **- LogFile** paraméter nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="1642e-185">In this example, hello script generates a log file automatically since hello **-LogFile** parameter isn't specified.</span></span> <span data-ttu-id="1642e-186">hello naplók valós időben nem írt, de összegyűjtött hello érvényesítési és hello telepítési hello végén.</span><span class="sxs-lookup"><span data-stu-id="1642e-186">hello logs aren't written in real time, but are collected at hello end of hello validation and hello deployment.</span></span> <span data-ttu-id="1642e-187">Ha hello PowerShell folyamat hello parancsfájl futása közben leállt, néhány naplók elvesznek.</span><span class="sxs-lookup"><span data-stu-id="1642e-187">If hello PowerShell process is stopped while hello script is running, some logs are lost.</span></span>
   > 
   > 

## <a name="connect-toohello-head-node"></a><span data-ttu-id="1642e-188">Csatlakozás toohello átjárócsomópont</span><span class="sxs-lookup"><span data-stu-id="1642e-188">Connect toohello head node</span></span>
<span data-ttu-id="1642e-189">Az Azure hello HPC Pack fürt telepítése után [csatlakozzon a távoli asztal](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello átjárócsomópont VM használatával hello hello fürt telepítésekor megadott tartományi hitelesítő adatok (például *hpc\\ clusteradmin*).</span><span class="sxs-lookup"><span data-stu-id="1642e-189">After you deploy hello HPC Pack cluster in Azure, [connect by Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello head node VM using hello domain credentials you provided when you deployed hello cluster (for example, *hpc\\clusteradmin*).</span></span> <span data-ttu-id="1642e-190">Hello átjárócsomópont származó hello fürt kezelése.</span><span class="sxs-lookup"><span data-stu-id="1642e-190">You manage hello cluster from hello head node.</span></span>

<span data-ttu-id="1642e-191">Indítsa el a HPC Fürtkezelő toocheck hello állapot hello HPC Pack fürt hello központi csomópontján.</span><span class="sxs-lookup"><span data-stu-id="1642e-191">On hello head node, start HPC Cluster Manager toocheck hello status of hello HPC Pack cluster.</span></span> <span data-ttu-id="1642e-192">Kezelheti és figyelheti a Linux számítási csomópontok hello használata Windows ugyanúgy számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="1642e-192">You can manage and monitor Linux compute nodes hello same way you work with Windows compute nodes.</span></span> <span data-ttu-id="1642e-193">Például lásd: hello Linux csomópontok felsorolt **erőforrás-kezelés** (ezek a csomópontok telepített hello **LinuxNode** sablon).</span><span class="sxs-lookup"><span data-stu-id="1642e-193">For example, you see hello Linux nodes listed in **Resource Management** (these nodes are deployed with hello **LinuxNode** template).</span></span>

![Csomópont-felügyelet][management]

<span data-ttu-id="1642e-195">Hello Linux csomópontjának hello is látni **Hőtérkép** nézet.</span><span class="sxs-lookup"><span data-stu-id="1642e-195">You also see hello Linux nodes in hello **Heat Map** view.</span></span>

![Hőtérkép][heatmap]

## <a name="how-toomove-data-in-a-cluster-with-linux-nodes"></a><span data-ttu-id="1642e-197">Hogyan toomove adatok Linux csomópontot tartalmazó fürtben</span><span class="sxs-lookup"><span data-stu-id="1642e-197">How toomove data in a cluster with Linux nodes</span></span>
<span data-ttu-id="1642e-198">Számos választási lehetőségek toomove adatokat Linux csomópontokat és hello Windows átjárócsomópont hello fürt között van.</span><span class="sxs-lookup"><span data-stu-id="1642e-198">You have several choices toomove data among Linux nodes and hello Windows head node of hello cluster.</span></span> <span data-ttu-id="1642e-199">Az alábbiakban a három általános módszer, részletesebben a következő részekben hello:</span><span class="sxs-lookup"><span data-stu-id="1642e-199">Here are three common methods, described in more detail in hello following sections:</span></span>

* <span data-ttu-id="1642e-200">**Az Azure File** -egy felügyelt SMB toostore fájlmegosztásadatokra-fájlok az Azure storage tesz elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="1642e-200">**Azure File** - Exposes a managed SMB file share toostore data files in Azure storage.</span></span> <span data-ttu-id="1642e-201">Windows-csomópontok és a Linux-csomópontok is csatlakoztathatja egy Azure fájlmegosztás olyan meghajtó vagy a mappát a hello azonos időben, még akkor is, ha azok a különböző virtuális hálózatokon van telepítve.</span><span class="sxs-lookup"><span data-stu-id="1642e-201">Windows nodes and Linux nodes can mount an Azure File share as a drive or folder at hello same time, even if they're deployed in different virtual networks.</span></span>
* <span data-ttu-id="1642e-202">**Átjárócsomópont SMB-fájlmegosztás** -csatlakoztatja a szabványos Windows megosztott mappába hello átjárócsomópont Linux csomópontján.</span><span class="sxs-lookup"><span data-stu-id="1642e-202">**Head node SMB share** - Mounts a standard Windows shared folder of hello head node on Linux nodes.</span></span>
* <span data-ttu-id="1642e-203">**HEAD csomópont NFS-kiszolgáló** -fájlmegosztási megoldást kínál a Windows és Linux használnak.</span><span class="sxs-lookup"><span data-stu-id="1642e-203">**Head node NFS server**  - Provides a file-sharing solution for a mixed Windows and Linux environment.</span></span>

### <a name="azure-file-storage"></a><span data-ttu-id="1642e-204">Az Azure File storage</span><span class="sxs-lookup"><span data-stu-id="1642e-204">Azure File storage</span></span>
<span data-ttu-id="1642e-205">Hello [Azure File](https://azure.microsoft.com/services/storage/files/) szolgáltatás elérhetővé teszi a fájlmegosztások hello standard SMB 2.1-es protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="1642e-205">hello [Azure File](https://azure.microsoft.com/services/storage/files/) service exposes file shares using hello standard SMB 2.1 protocol.</span></span> <span data-ttu-id="1642e-206">Az Azure virtuális gépek és felhőszolgáltatások megosztott csatlakoztatott megosztásokon keresztül alkalmazások összetevői között, és a helyszíni alkalmazások férhetnek hozzá olyan megosztáson található fájladatokat hello fájl storage API-JÁN keresztül.</span><span class="sxs-lookup"><span data-stu-id="1642e-206">Azure VMs and cloud services can share file data across application components via mounted shares, and on-premises applications can access file data in a share through hello File storage API.</span></span> 

<span data-ttu-id="1642e-207">Egy Azure-fájl megosztása, és csatlakoztassa azt hello átjárócsomópont részletes lépéseket toocreate, lásd: [Ismerkedés az Azure File storage on Windows](../../../storage/files/storage-how-to-use-files-windows.md).</span><span class="sxs-lookup"><span data-stu-id="1642e-207">For detailed steps toocreate an Azure File share and mount it on hello head node, see [Get started with Azure File storage on Windows](../../../storage/files/storage-how-to-use-files-windows.md).</span></span> <span data-ttu-id="1642e-208">toomount hello Azure fájlmegosztás hello Linux csomópont, lásd: [hogyan toouse Linux Azure File storage](../../../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1642e-208">toomount hello Azure File share on hello Linux nodes, see [How toouse Azure File storage with Linux](../../../storage/files/storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="1642e-209">tooset létre tárolásakor kapcsolatokat, lásd: [Persisting kapcsolatok tooMicrosoft Azure fájlok](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).</span><span class="sxs-lookup"><span data-stu-id="1642e-209">tooset up persisting connections, see [Persisting connections tooMicrosoft Azure Files](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).</span></span>

<span data-ttu-id="1642e-210">A következő példa hello Azure-fájlmegosztás létrehozása a storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="1642e-210">In hello following example, create an Azure File share on a storage account.</span></span> <span data-ttu-id="1642e-211">toomount hello megosztása csomóponton hello központi, nyissa meg egy parancssort, és írja be a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="1642e-211">toomount hello share on hello head node, open a Command Prompt and enter hello following commands:</span></span>

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

<span data-ttu-id="1642e-212">Ebben a példában allvhdsje a tárfiók nevére, storageaccountkey a tárfiók kulcsára, pedig rdma hello Azure fájlmegosztás neve.</span><span class="sxs-lookup"><span data-stu-id="1642e-212">In this example, allvhdsje is your storage account name, storageaccountkey is your storage account key, and rdma is hello Azure File share name.</span></span> <span data-ttu-id="1642e-213">hello Azure fájlmegosztás Z:, hello átjárócsomópontjához csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="1642e-213">hello Azure File share is mounted as Z: on hello head node.</span></span>

<span data-ttu-id="1642e-214">toomount hello Azure fájlmegosztás Linux csomópont, futtassa a **clusrun** hello átjárócsomópont parancs.</span><span class="sxs-lookup"><span data-stu-id="1642e-214">toomount hello Azure File share on Linux nodes, run a **clusrun** command on hello head node.</span></span> <span data-ttu-id="1642e-215">**[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)**  van egy hasznos HPC Pack eszköz toocarry több csomóponton felügyeleti feladatokat.</span><span class="sxs-lookup"><span data-stu-id="1642e-215">**[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)** is a useful HPC Pack tool toocarry out administrative tasks on multiple nodes.</span></span> <span data-ttu-id="1642e-216">(Lásd még: [Clusrun Linux csomópontok](#Clusrun-for-Linux-nodes) ebben a cikkben.)</span><span class="sxs-lookup"><span data-stu-id="1642e-216">(See also [Clusrun for Linux nodes](#Clusrun-for-Linux-nodes) in this article.)</span></span>

<span data-ttu-id="1642e-217">Nyisson meg egy Windows PowerShell-ablakot, és írja be a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="1642e-217">Open a Windows PowerShell window and enter hello following commands:</span></span>

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

<span data-ttu-id="1642e-218">hello első parancs létrehoz egy hello LinuxNodes csoport minden csomópontján /rdma nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="1642e-218">hello first command creates a folder named /rdma on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="1642e-219">hello második parancs hello Azure File megosztás allvhdsjw.file.core.windows.net/rdma hello /rdma mappa dir és fájl mód bits set too777 alakzatot csatlakoztatja.</span><span class="sxs-lookup"><span data-stu-id="1642e-219">hello second command mounts hello Azure File share allvhdsjw.file.core.windows.net/rdma onto hello /rdma folder with dir and file mode bits set too777.</span></span> <span data-ttu-id="1642e-220">A második parancs hello allvhdsje a tárfiók neve pedig storageaccountkey a tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="1642e-220">In hello second command, allvhdsje is your storage account name and storageaccountkey is your storage account key.</span></span>

> [!NOTE]
> <span data-ttu-id="1642e-221">hello "\\`" hello második parancs a szimbólum értéke egy escape szimbólum PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1642e-221">hello “\\`” symbol in hello second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="1642e-222">"\\`,"azt jelenti, hogy hello"," (vessző karakter) hello parancs részét képezi.</span><span class="sxs-lookup"><span data-stu-id="1642e-222">“\\`,” means that hello “,” (comma character) is a part of hello command.</span></span>
> 
> 

### <a name="head-node-share"></a><span data-ttu-id="1642e-223">Átjárócsomópont megosztás</span><span class="sxs-lookup"><span data-stu-id="1642e-223">Head node share</span></span>
<span data-ttu-id="1642e-224">Azt is megteheti csatlakoztatása Linux csomópontok hello átjárócsomópont megosztott mappába.</span><span class="sxs-lookup"><span data-stu-id="1642e-224">Alternatively, mount a shared folder of hello head node on Linux nodes.</span></span> <span data-ttu-id="1642e-225">A megosztás hello legegyszerűbb módja tooshare fájlokat, de az hello átjárócsomópont és az összes Linux csomópontra hello kell telepíteni ugyanazt a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="1642e-225">A share provides hello simplest way tooshare files, but hello head node and all Linux nodes must be deployed in hello same virtual network.</span></span> <span data-ttu-id="1642e-226">Az alábbiakban hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="1642e-226">Here are hello steps.</span></span>

1. <span data-ttu-id="1642e-227">Hozzon létre egy mappát a hello átjárócsomópont, és megoszthatják tooEveryone az olvasási/írási engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="1642e-227">Create a folder on hello head node and share it tooEveryone with Read/Write permissions.</span></span> <span data-ttu-id="1642e-228">Például megosztás D:\OpenFOAM hello átjárócsomópont, \\CentOS7RDMA-HN\OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="1642e-228">For example, share D:\OpenFOAM on hello head node as \\CentOS7RDMA-HN\OpenFOAM.</span></span> <span data-ttu-id="1642e-229">A HN CentOS7RDMA ez hello átjárócsomópont hello állomásnevét.</span><span class="sxs-lookup"><span data-stu-id="1642e-229">Here CentOS7RDMA-HN is hello hostname of hello head node.</span></span>
   
    ![Fájlmegosztási engedélyek][fileshareperms]
   
    ![Fájlmegosztás][filesharing]
2. <span data-ttu-id="1642e-232">Nyisson meg egy Windows PowerShell-ablakot, és futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="1642e-232">Open a Windows PowerShell window and run hello following commands:</span></span>
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

<span data-ttu-id="1642e-233">hello első parancs létrehoz egy hello LinuxNodes csoport minden csomópontján /openfoam nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="1642e-233">hello first command creates a folder named /openfoam on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="1642e-234">hello második parancs csatlakoztatja hello megosztott mappa //CentOS7RDMA-HN/OpenFOAM alakzatot dir és fájl mód bits set too777 hello mappában.</span><span class="sxs-lookup"><span data-stu-id="1642e-234">hello second command mounts hello shared folder //CentOS7RDMA-HN/OpenFOAM onto hello folder with dir and file mode bits set too777.</span></span> <span data-ttu-id="1642e-235">hello hello parancs levő felhasználónevet és jelszót kell hello felhasználónevét és jelszavát hello átjárócsomópont fürt felhasználója.</span><span class="sxs-lookup"><span data-stu-id="1642e-235">hello username and password in hello command should be hello username and password of a cluster user on hello head node.</span></span> <span data-ttu-id="1642e-236">(Lásd: [hozzáadását vagy eltávolítását fürt felhasználók](https://technet.microsoft.com/library/ff919330.aspx).)</span><span class="sxs-lookup"><span data-stu-id="1642e-236">(See [Add or remove cluster users](https://technet.microsoft.com/library/ff919330.aspx).)</span></span>

> [!NOTE]
> <span data-ttu-id="1642e-237">hello "\\`" hello második parancs a szimbólum értéke egy escape szimbólum PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1642e-237">hello “\\`” symbol in hello second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="1642e-238">"\\`,"azt jelenti, hogy hello"," (vessző karakter) hello parancs részét képezi.</span><span class="sxs-lookup"><span data-stu-id="1642e-238">“\\`,” means that hello “,” (comma character) is a part of hello command.</span></span>
> 
> 

### <a name="nfs-server"></a><span data-ttu-id="1642e-239">NFS-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="1642e-239">NFS server</span></span>
<span data-ttu-id="1642e-240">hello NFS szolgáltatás lehetővé teszi a tooshare, és telepítse át a fájlok hello SMB protokollt használó hello Windows Server 2012 operációs rendszert futtató és hello NFS-protokollt használó Linux-alapú számítógépek között.</span><span class="sxs-lookup"><span data-stu-id="1642e-240">hello NFS service enables you tooshare and migrate files between computers running hello Windows Server 2012 operating system using hello SMB protocol and Linux-based computers using hello NFS protocol.</span></span> <span data-ttu-id="1642e-241">hello NFS-kiszolgáló és az összes csomóponton rendelkezik telepített hello toobe ugyanazt a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="1642e-241">hello NFS server and all other nodes have toobe deployed in hello same virtual network.</span></span> <span data-ttu-id="1642e-242">Linux-csomópontokkal SMB-megosztáson képest jobb kompatibilitást biztosít.</span><span class="sxs-lookup"><span data-stu-id="1642e-242">It provides better compatibility with Linux nodes compared with an SMB share.</span></span> <span data-ttu-id="1642e-243">Például fájlhivatkozások támogatja.</span><span class="sxs-lookup"><span data-stu-id="1642e-243">For example, it supports file links.</span></span>

1. <span data-ttu-id="1642e-244">tooinstall, és állítsa be az NFS-kiszolgáló kövesse hello [Server, a rendszer első Fájlmegosztására-végpontok](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).</span><span class="sxs-lookup"><span data-stu-id="1642e-244">tooinstall and set up an NFS server, follow hello steps in [Server for Network File System First Share End-to-End](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).</span></span>
   
    <span data-ttu-id="1642e-245">Hozzon létre például egy NFS-megosztás nfs nevű hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="1642e-245">For example, create an NFS share named nfs with hello following properties:</span></span>
   
    ![NFS-engedélyezés][nfsauth]
   
    ![NFS-megosztás engedélyeinek][nfsshare]
   
    ![NFS-NTFS-engedélyek][nfsperm]
   
    ![NFS-felügyeleti tulajdonságai][nfsmanage]
2. <span data-ttu-id="1642e-250">Nyisson meg egy Windows PowerShell-ablakot, és futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="1642e-250">Open a Windows PowerShell window and run hello following commands:</span></span>
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
   
    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```
   
   <span data-ttu-id="1642e-251">hello első parancs létrehoz egy hello LinuxNodes csoport minden csomópontján /nfsshared nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="1642e-251">hello first command creates a folder named /nfsshared on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="1642e-252">hello második parancs csatlakoztatások hello NFS megosztása CentOS7RDMA-HN: / alakzatot nfs hello mappát.</span><span class="sxs-lookup"><span data-stu-id="1642e-252">hello second command mounts hello NFS share CentOS7RDMA-HN:/nfs onto hello folder.</span></span> <span data-ttu-id="1642e-253">Itt CentOS7RDMA-HN: / nfs hello távoli elérési útja az NFS-megosztások.</span><span class="sxs-lookup"><span data-stu-id="1642e-253">Here CentOS7RDMA-HN:/nfs is hello remote path of your NFS share.</span></span>

## <a name="how-toosubmit-jobs"></a><span data-ttu-id="1642e-254">Hogyan toosubmit feladatok</span><span class="sxs-lookup"><span data-stu-id="1642e-254">How toosubmit jobs</span></span>
<span data-ttu-id="1642e-255">Van több módon toosubmit feladatok toohello HPC Pack fürt:</span><span class="sxs-lookup"><span data-stu-id="1642e-255">There are several ways toosubmit jobs toohello HPC Pack cluster:</span></span>

* <span data-ttu-id="1642e-256">HPC Fürtkezelő vagy a HPC Job Manager eszköz grafikus felhasználói felületen</span><span class="sxs-lookup"><span data-stu-id="1642e-256">HPC Cluster Manager or HPC Job Manager GUI</span></span>
* <span data-ttu-id="1642e-257">HPC-webportál</span><span class="sxs-lookup"><span data-stu-id="1642e-257">HPC web portal</span></span>
* <span data-ttu-id="1642e-258">REST API</span><span class="sxs-lookup"><span data-stu-id="1642e-258">REST API</span></span>

<span data-ttu-id="1642e-259">HPC Pack grafikus eszközöket és hello HPC-webportál az Azure-ban toohello fürt vannak hello ugyanaz, mint Windows számítási csomópontok feladat elküldése.</span><span class="sxs-lookup"><span data-stu-id="1642e-259">Job submission toohello cluster in Azure via HPC Pack GUI tools and hello HPC web portal are hello same as for Windows compute nodes.</span></span> <span data-ttu-id="1642e-260">Lásd: [HPC Pack Feladatkezelő](https://technet.microsoft.com/library/ff919691.aspx) és [hogyan toosubmit feladatokat a helyszíni ügyfél-számítógépről](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1642e-260">See [HPC Pack Job Manager](https://technet.microsoft.com/library/ff919691.aspx) and [How toosubmit jobs from an on-premises client computer](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="1642e-261">toosubmit feladatok keresztül hello REST API-t, tekintse meg a túl[létrehozása és elküldése a feladatokat a szerint hello REST API-t a Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span><span class="sxs-lookup"><span data-stu-id="1642e-261">toosubmit jobs via hello REST API, refer too[Creating and Submitting Jobs by Using hello REST API in Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span></span> <span data-ttu-id="1642e-262">a Linux ügyfélben toosubmit feladatok is tekintse meg a toohello Python minta hello [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).</span><span class="sxs-lookup"><span data-stu-id="1642e-262">toosubmit jobs from a Linux client, also refer toohello Python sample in hello [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).</span></span>

## <a name="clusrun-for-linux-nodes"></a><span data-ttu-id="1642e-263">Linux-csomópontok Clusrun</span><span class="sxs-lookup"><span data-stu-id="1642e-263">Clusrun for Linux nodes</span></span>
<span data-ttu-id="1642e-264">HPC Pack hello [clusrun](https://technet.microsoft.com/library/cc947685.aspx) használt tooexecute parancsok Linux csomópontokon keresztül, a parancssor vagy a HPC Cluster Manager eszköz lehet.</span><span class="sxs-lookup"><span data-stu-id="1642e-264">hello HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) tool can be used tooexecute commands on Linux nodes either through a Command Prompt or HPC Cluster Manager.</span></span> <span data-ttu-id="1642e-265">A következő példák alapvető.</span><span class="sxs-lookup"><span data-stu-id="1642e-265">Following are some basic examples.</span></span>

* <span data-ttu-id="1642e-266">Az aktuális felhasználónevek megjelenítése hello fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="1642e-266">Show current user names on all nodes in hello cluster.</span></span>
  
    ```command
    clusrun whoami
    ```
* <span data-ttu-id="1642e-267">Hello telepítése **gdb** hibakereső eszközt a **yum** az összes csomóponton hello linuxnodes csoportban, és indítsa újra hello csomópontok 10 perc múlva.</span><span class="sxs-lookup"><span data-stu-id="1642e-267">Install hello **gdb** debugger tool with **yum** on all nodes in hello linuxnodes group and then restart hello nodes after 10 minutes.</span></span>
  
    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```
* <span data-ttu-id="1642e-268">Hozzon létre egy héjparancsfájlt, minden egyes számának 1 és 10 minden egyes Linux csomóponton egy második hello fürt megjelenítése, majd futtassa és hello csomópontok eredményének megjelenítése azonnal.</span><span class="sxs-lookup"><span data-stu-id="1642e-268">Create a shell script displaying each number 1 through 10 for one second on each Linux node in hello cluster, run it, and show output from hello nodes immediately.</span></span>
  
    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

> [!NOTE]
> <span data-ttu-id="1642e-269">Toouse szükség lehet egyes escape-karakter **clusrun** parancsok.</span><span class="sxs-lookup"><span data-stu-id="1642e-269">You might need toouse certain escape characters in **clusrun** commands.</span></span> <span data-ttu-id="1642e-270">Ebben a példában látható, használjon ^ egy parancssor tooescape hello a ">" szimbólumot.</span><span class="sxs-lookup"><span data-stu-id="1642e-270">As shown in this example, use ^ in a Command Prompt tooescape hello ">" symbol.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="1642e-271">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1642e-271">Next steps</span></span>
* <span data-ttu-id="1642e-272">Próbálkozzon a hello fürt tooa több csomópontot vertikális felskálázásával, vagy a Linux-munkaterhelések futtatásával hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="1642e-272">Try scaling up hello cluster tooa larger number of nodes, or try running a Linux workload on hello cluster.</span></span> <span data-ttu-id="1642e-273">Egy vonatkozó példáért lásd: [NAMD futtassa a Microsoft HPC Pack Linux számítási csomópontok az Azure-ban](hpcpack-cluster-namd.md).</span><span class="sxs-lookup"><span data-stu-id="1642e-273">For an example, see [Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure](hpcpack-cluster-namd.md).</span></span>
* <span data-ttu-id="1642e-274">Próbálja meg a fürt [RDMA-kompatibilisek-e, a számítási igényű virtuális gépek](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toorun MPI munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="1642e-274">Try a cluster with [RDMA-capable, compute-intensive VMs](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toorun MPI workloads.</span></span> <span data-ttu-id="1642e-275">Egy vonatkozó példáért lásd: [OpenFOAM futtassa a Microsoft HPC Pack egy Linux RDMA a fürtön, az Azure-ban](hpcpack-cluster-openfoam.md).</span><span class="sxs-lookup"><span data-stu-id="1642e-275">For an example, see [Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure](hpcpack-cluster-openfoam.md).</span></span>
* <span data-ttu-id="1642e-276">Ha érdekli a helyszíni HPC Pack fürtben lévő Linux-csomópont működik, tekintse meg a hello [TechNet útmutatást](https://technet.microsoft.com/library/mt595803.aspx).</span><span class="sxs-lookup"><span data-stu-id="1642e-276">If you are interested in working with Linux nodes in an on-premises HPC Pack cluster, see hello [TechNet guidance](https://technet.microsoft.com/library/mt595803.aspx).</span></span>

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
