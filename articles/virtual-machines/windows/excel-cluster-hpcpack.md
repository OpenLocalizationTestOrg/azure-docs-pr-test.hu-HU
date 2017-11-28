---
title: "az Excel és SOA fürt aaaHPC Pack |} Microsoft Docs"
description: "Ismerkedés a nagyméretű Excel és a SOA munkaterhelések egy HPC Pack fürtben futó Azure-ban"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,hpc-pack
ms.assetid: cb6a9abe-caf3-44da-b911-849a50f6cfb3
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/01/2017
ms.author: danlep
ms.openlocfilehash: 55b4b2c25fe65d06b75025cc23c3c13b8b764238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="63125-103">Ismerkedés az Excel és a SOA munkaterhelések egy HPC Pack fürtben futó Azure-ban</span><span class="sxs-lookup"><span data-stu-id="63125-103">Get started running Excel and SOA workloads on an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="63125-104">Ez a cikk bemutatja, hogyan toodeploy a Microsoft HPC Pack 2012 R2 fürt Azure virtuális gépeken futó Azure gyors üzembe helyezés sablonná és opcionálisan egy Azure PowerShell telepítési parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="63125-104">This article shows you how toodeploy a Microsoft HPC Pack 2012 R2 cluster on Azure virtual machines by using an Azure quickstart template, or optionally an Azure PowerShell deployment script.</span></span> <span data-ttu-id="63125-105">hello fürt HPC Pack használ az Azure piactér virtuális gép tervezett képek toorun Microsoft Excel vagy szolgáltatásorientált architektúra (SOA) munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="63125-105">hello cluster uses Azure Marketplace VM images designed toorun Microsoft Excel or service-oriented architecture (SOA) workloads with HPC Pack.</span></span> <span data-ttu-id="63125-106">Hello fürt toorun Excel HPC és SOA szolgáltatások egy helyszíni ügyfélszámítógépről is használhatja.</span><span class="sxs-lookup"><span data-stu-id="63125-106">You can use hello cluster toorun Excel HPC and SOA services from an on-premises client computer.</span></span> <span data-ttu-id="63125-107">hello Excel HPC-szolgáltatások közé tartoznak az Excel-munkafüzet kiürítéséhez és a felhasználó által definiált függvények Excel vagy a felhasználó által megadott függvények.</span><span class="sxs-lookup"><span data-stu-id="63125-107">hello Excel HPC services include Excel workbook offloading and Excel user-defined functions, or UDFs.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="63125-108">Ez a cikk a HPC Pack 2012 R2 alapul funkciók, sablonok és parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="63125-108">This article is based on features, templates, and scripts for HPC Pack 2012 R2.</span></span> <span data-ttu-id="63125-109">Ebben a forgatókönyvben jelenleg nem támogatott a HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="63125-109">This scenario is not currently supported in HPC Pack 2016.</span></span>
>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="63125-110">Magas szinten, hello alábbi ábrán látható hello HPC Pack fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="63125-110">At a high level, hello following diagram shows hello HPC Pack cluster you create.</span></span>

![Excel munkaterheket futtatnak csomópontok HPC-fürt][scenario]

## <a name="prerequisites"></a><span data-ttu-id="63125-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="63125-112">Prerequisites</span></span>
* <span data-ttu-id="63125-113">**Ügyfélszámítógép** -ügyfél Windows-alapú számítógép toosubmit minta Excel és SOA feladatok toohello fürt van szüksége.</span><span class="sxs-lookup"><span data-stu-id="63125-113">**Client computer** - You need a Windows-based client computer toosubmit sample Excel and SOA jobs toohello cluster.</span></span> <span data-ttu-id="63125-114">A Windows számítógép toorun hello Azure PowerShell fürt telepítési parancsfájlt is (ha az adott központi telepítési módszer választása) kell.</span><span class="sxs-lookup"><span data-stu-id="63125-114">You also need a Windows computer toorun hello Azure PowerShell cluster deployment script (if you choose that deployment method).</span></span>
* <span data-ttu-id="63125-115">**Azure-előfizetés** – Ha nem rendelkezik Azure-előfizetéssel, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/) néhány percig.</span><span class="sxs-lookup"><span data-stu-id="63125-115">**Azure subscription** - If you don't have an Azure subscription, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.</span></span>
* <span data-ttu-id="63125-116">**Magok kvóta** -szükség lehet tooincrease hello beállított kvótát mag, különösen akkor, ha több fürtcsomóponton multicore Virtuálisgép-méretek és telepít.</span><span class="sxs-lookup"><span data-stu-id="63125-116">**Cores quota** - You might need tooincrease hello quota of cores, especially if you deploy several cluster nodes with multicore VM sizes.</span></span> <span data-ttu-id="63125-117">Egy Azure gyors üzembe helyezés sablont használ, ha hello magok kvóta az erőforrás-kezelőben / Azure-régióban van.</span><span class="sxs-lookup"><span data-stu-id="63125-117">If you are using an Azure quickstart template, hello cores quota in Resource Manager is per Azure region.</span></span> <span data-ttu-id="63125-118">Ebben az esetben szükség lehet egy adott régióban tooincrease hello kvótát.</span><span class="sxs-lookup"><span data-stu-id="63125-118">In that case, you might need tooincrease hello quota in a specific region.</span></span> <span data-ttu-id="63125-119">Lásd: [Azure-előfizetésre vonatkozó korlátok, kvóták és megkötések](../../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="63125-119">See [Azure subscription limits, quotas, and constraints](../../azure-subscription-service-limits.md).</span></span> <span data-ttu-id="63125-120">a kvóta tooincrease [nyissa meg az online támogatás ügyfélkérés](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) díjmentesen.</span><span class="sxs-lookup"><span data-stu-id="63125-120">tooincrease a quota, [open an online customer support request](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) at no charge.</span></span>
* <span data-ttu-id="63125-121">**A Microsoft Office-licencet** – Ha a számítási csomópontok piactér HPC Pack 2012 R2 Virtuálisgép-lemezkép használatával a Microsoft Excel, a 30 napos próbaverzióját Microsoft Excel Professional Plus 2013 telepítve van.</span><span class="sxs-lookup"><span data-stu-id="63125-121">**Microsoft Office license** - If you deploy compute nodes using a Marketplace HPC Pack 2012 R2 VM image with Microsoft Excel, a 30-day evaluation version of Microsoft Excel Professional Plus 2013 is installed.</span></span> <span data-ttu-id="63125-122">Hello próbaidőszak után munkaterheléseknek tooprovide egy érvényes Microsoft Office licenc tooactivate Excel toocontinue toorun.</span><span class="sxs-lookup"><span data-stu-id="63125-122">After hello evaluation period, you need tooprovide a valid Microsoft Office license tooactivate Excel toocontinue toorun workloads.</span></span> <span data-ttu-id="63125-123">Lásd: [Excel-aktiválási](#excel-activation) című cikkben.</span><span class="sxs-lookup"><span data-stu-id="63125-123">See [Excel activation](#excel-activation) later in this article.</span></span> 

## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="63125-124">1. lépés</span><span class="sxs-lookup"><span data-stu-id="63125-124">Step 1.</span></span> <span data-ttu-id="63125-125">Az Azure-ban egy HPC Pack fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="63125-125">Set up an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="63125-126">Hello HPC Pack 2012 R2-fürt két beállítások tooset megmutatjuk: első, egy sablon Azure gyors üzembe helyezés és hello Azure-portálon; és a második, az Azure PowerShell telepítési parancsfájlt használ.</span><span class="sxs-lookup"><span data-stu-id="63125-126">We show two options tooset up hello HPC Pack 2012 R2 cluster: first, using an Azure quickstart template and hello Azure portal; and second, using an Azure PowerShell deployment script.</span></span>

### <a name="option-1-use-a-quickstart-template"></a><span data-ttu-id="63125-127">1. lehetőség.</span><span class="sxs-lookup"><span data-stu-id="63125-127">Option 1.</span></span> <span data-ttu-id="63125-128">A következő gyorsindítási sablonon használata</span><span class="sxs-lookup"><span data-stu-id="63125-128">Use a quickstart template</span></span>
<span data-ttu-id="63125-129">Használja az Azure gyors üzembe helyezés sablon tooquickly HPC Pack-fürt üzembe helyezése a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="63125-129">Use an Azure quickstart template tooquickly deploy an HPC Pack cluster in hello Azure portal.</span></span> <span data-ttu-id="63125-130">Hello sablon hello portál megnyitásakor egy egyszerű felhasználói felület, ahol hello-beállítások megadása a fürt kap.</span><span class="sxs-lookup"><span data-stu-id="63125-130">When you open hello template in hello portal, you get a simple UI where you enter hello settings for your cluster.</span></span> <span data-ttu-id="63125-131">Az alábbiakban hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="63125-131">Here are hello steps.</span></span> 

> [!TIP]
> <span data-ttu-id="63125-132">Ha azt szeretné, egy [Azure piactér sablon](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) , amely kifejezetten a Excel munkaterhelések hasonló fürt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="63125-132">If you want, use an [Azure Marketplace template](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) that creates a similar cluster specifically for Excel workloads.</span></span> <span data-ttu-id="63125-133">hello lépések némileg eltérő hello következő.</span><span class="sxs-lookup"><span data-stu-id="63125-133">hello steps differ slightly from hello following.</span></span>
> 
> 

1. <span data-ttu-id="63125-134">A Microsoft hello [sablonlap HPC-fürt létrehozása a Githubon](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster).</span><span class="sxs-lookup"><span data-stu-id="63125-134">Visit hello [Create HPC Cluster template page on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster).</span></span> <span data-ttu-id="63125-135">Ha azt szeretné, tekintse át a hello sablon és hello forráskód adatait.</span><span class="sxs-lookup"><span data-stu-id="63125-135">If you want, review information about hello template and hello source code.</span></span>
2. <span data-ttu-id="63125-136">Kattintson a **tooAzure telepítése** toostart a központi telepítés sablonnal hello hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="63125-136">Click **Deploy tooAzure** toostart a deployment with hello template in hello Azure portal.</span></span>
   
   ![Sablon tooAzure telepítése][github]
3. <span data-ttu-id="63125-138">Hello portálon hajtsa végre az ezen lépések tooenter hello paraméterek hello HPC-fürt sablon.</span><span class="sxs-lookup"><span data-stu-id="63125-138">In hello portal, follow these steps tooenter hello parameters for hello HPC cluster template.</span></span>
   
   <span data-ttu-id="63125-139">a.</span><span class="sxs-lookup"><span data-stu-id="63125-139">a.</span></span> <span data-ttu-id="63125-140">A hello **paraméterek** lapon adja meg, vagy módosítsa hello sablon paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="63125-140">On hello **Parameters** page, enter or modify values for hello template parameters.</span></span> <span data-ttu-id="63125-141">(Kattintson a hello ikon következő tooeach beállítás súgójában talál.) A következő képernyő hello mintaértékek láthatók.</span><span class="sxs-lookup"><span data-stu-id="63125-141">(Click hello icon next tooeach setting for help information.) Sample values are shown in hello following screen.</span></span> <span data-ttu-id="63125-142">Ez a példa egy nevű fürtöt hoz létre *hpc01* a hello *hpc.local* tartomány egy átjárócsomóponttal és 2 álló számítási csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="63125-142">This example creates a cluster named *hpc01* in hello *hpc.local* domain consisting of a head node and 2 compute nodes.</span></span> <span data-ttu-id="63125-143">hello számítási csomópontok HPC Pack VM-lemezkép, amely tartalmazza a Microsoft Excel készített.</span><span class="sxs-lookup"><span data-stu-id="63125-143">hello compute nodes are created from an HPC Pack VM image that includes Microsoft Excel.</span></span>
   
   ![Adja meg a paraméterek][parameters-new-portal]
   
   > [!NOTE]
   > <span data-ttu-id="63125-145">virtuális gép automatikusan létrejön a hello hello átjárócsomópont [legújabb Piactéri lemezképhez](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) HPC Pack 2012 R2 Windows Server 2012 R2 rendszeren.</span><span class="sxs-lookup"><span data-stu-id="63125-145">hello head node VM is created automatically from hello [latest Marketplace image](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) of HPC Pack 2012 R2 on Windows Server 2012 R2.</span></span> <span data-ttu-id="63125-146">Jelenleg hello kép HPC Pack 2012 R2 Update 3 alapul.</span><span class="sxs-lookup"><span data-stu-id="63125-146">Currently hello image is based on HPC Pack 2012 R2 Update 3.</span></span>
   > 
   > <span data-ttu-id="63125-147">A számítási csomópont virtuális gépek jönnek létre a kiválasztott hello számítási csomópont termékcsalád hello legújabb lemezképről.</span><span class="sxs-lookup"><span data-stu-id="63125-147">Compute node VMs are created from hello latest image of hello selected compute node family.</span></span> <span data-ttu-id="63125-148">Jelölje be hello **ComputeNodeWithExcel** hello legújabb HPC Pack számítási csomópont-kép próbaverziójáról a Microsoft Excel Professional Plus 2013 beállítást.</span><span class="sxs-lookup"><span data-stu-id="63125-148">Select hello **ComputeNodeWithExcel** option for hello latest HPC Pack compute node image that includes an evaluation version of Microsoft Excel Professional Plus 2013.</span></span> <span data-ttu-id="63125-149">az általános SOA munkamenetekhez, illetve az Excel UDF-ben történő kiszervezésével a toodeploy válasszon hello **Átjárócsomópontján** (nélkül telepített Excel) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="63125-149">toodeploy a cluster for general SOA sessions or for Excel UDF offloading, choose hello **ComputeNode** option (without Excel installed).</span></span>
   > 
   > 
   
   <span data-ttu-id="63125-150">b.</span><span class="sxs-lookup"><span data-stu-id="63125-150">b.</span></span> <span data-ttu-id="63125-151">Válasszon hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="63125-151">Choose hello subscription.</span></span>
   
   <span data-ttu-id="63125-152">c.</span><span class="sxs-lookup"><span data-stu-id="63125-152">c.</span></span> <span data-ttu-id="63125-153">Például a hello fürt erőforráscsoport létrehozása *hpc01RG*.</span><span class="sxs-lookup"><span data-stu-id="63125-153">Create a resource group for hello cluster, such as *hpc01RG*.</span></span>
   
   <span data-ttu-id="63125-154">d.</span><span class="sxs-lookup"><span data-stu-id="63125-154">d.</span></span> <span data-ttu-id="63125-155">Válasszon egy helyet hello erőforráscsoport, például az USA középső RÉGIÓJA.</span><span class="sxs-lookup"><span data-stu-id="63125-155">Choose a location for hello resource group, such as Central US.</span></span>
   
   <span data-ttu-id="63125-156">e.</span><span class="sxs-lookup"><span data-stu-id="63125-156">e.</span></span> <span data-ttu-id="63125-157">A hello **jogi feltételeket** lapján tekintse át a hello feltételeket.</span><span class="sxs-lookup"><span data-stu-id="63125-157">On hello **Legal terms** page, review hello terms.</span></span> <span data-ttu-id="63125-158">Ha elfogadja, kattintson a **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="63125-158">If you agree, click **Purchase**.</span></span> <span data-ttu-id="63125-159">Ha elkészült, hello sablon hello beállításértékek, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="63125-159">Then, when you are finished setting hello values for hello template, click **Create**.</span></span>
4. <span data-ttu-id="63125-160">Hello telepítési befejeződésekor (általában tart körülbelül 30 percig), hello fürt tanúsítványfájl exportálása hello fürt átjárócsomópontjából.</span><span class="sxs-lookup"><span data-stu-id="63125-160">When hello deployment completes (it typically takes around 30 minutes), export hello cluster certificate file from hello cluster head node.</span></span> <span data-ttu-id="63125-161">Egy későbbi lépésben importálja ezt a nyilvános tanúsítványt hello tooprovide hello kiszolgálóoldali ügyfélszámítógépeket biztonságos HTTP-kötés.</span><span class="sxs-lookup"><span data-stu-id="63125-161">In a later step, you import this public certificate on hello client computer tooprovide hello server-side authentication for secure HTTP binding.</span></span>
   
   <span data-ttu-id="63125-162">a.</span><span class="sxs-lookup"><span data-stu-id="63125-162">a.</span></span> <span data-ttu-id="63125-163">Nyissa meg toohello irányítópultot, válassza hello átjárócsomópont hello Azure-portálon, és **Connect** hello lap tooconnect távoli asztali kapcsolattal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="63125-163">In hello Azure portal, go toohello dashboard, select hello head node, and click **Connect** at hello top of hello page tooconnect using Remote Desktop.</span></span>
   
    <!-- ![Connect toohello head node][connect] -->
   
   <span data-ttu-id="63125-164">b.</span><span class="sxs-lookup"><span data-stu-id="63125-164">b.</span></span> <span data-ttu-id="63125-165">Eljárásokkal szabványos Tanúsítványkezelő tooexport hello átjárócsomópont tanúsítványban (az található a Cert: \LocalMachine\My) hello titkos kulcs nélkül.</span><span class="sxs-lookup"><span data-stu-id="63125-165">Use standard procedures in Certificate Manager tooexport hello head node certificate (located under Cert:\LocalMachine\My) without hello private key.</span></span> <span data-ttu-id="63125-166">Ebben a példában exportálása *CN = hpc01.eastus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="63125-166">In this example, export *CN = hpc01.eastus.cloudapp.azure.com*.</span></span>
   
   ![Hello tanúsítvány exportálása][cert]

### <a name="option-2-use-hello-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="63125-168">2. lehetőség.</span><span class="sxs-lookup"><span data-stu-id="63125-168">Option 2.</span></span> <span data-ttu-id="63125-169">Hello HPC Pack IaaS telepítési parancsfájl használata</span><span class="sxs-lookup"><span data-stu-id="63125-169">Use hello HPC Pack IaaS Deployment script</span></span>
<span data-ttu-id="63125-170">HPC Pack IaaS telepítési parancsfájl hello biztosít egy másik sokoldalú módon toodeploy egy HPC Pack fürthöz.</span><span class="sxs-lookup"><span data-stu-id="63125-170">hello HPC Pack IaaS deployment script provides another versatile way toodeploy an HPC Pack cluster.</span></span> <span data-ttu-id="63125-171">A fürt létrehoz hello klasszikus üzembe helyezési modellel, mivel hello a sablon által hello Azure Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="63125-171">It creates a cluster in hello classic deployment model, whereas hello template uses hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="63125-172">Hello parancsfájlt is kompatibilis Azure globális hello vagy Azure Kína szolgáltatás előfizetés.</span><span class="sxs-lookup"><span data-stu-id="63125-172">Also, hello script is compatible with a subscription in either hello Azure Global or Azure China service.</span></span>

<span data-ttu-id="63125-173">**További Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="63125-173">**Additional prerequisites**</span></span>

* <span data-ttu-id="63125-174">**Az Azure PowerShell** - [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview) (0.8.10 verzió vagy újabb) az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="63125-174">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="63125-175">**HPC Pack IaaS telepítési parancsfájl** - töltse le és csomagolja ki a legújabb verziójának hello hello hello parancsfájlt [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="63125-175">**HPC Pack IaaS deployment script** - Download and unpack hello latest version of hello script from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="63125-176">Verzióellenőrzés hello hello parancsfájl futtatásával `New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="63125-176">Check hello version of hello script by running `New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="63125-177">Ez a cikk verzióján 4.5.0 vagy későbbi hello parancsfájl alapul.</span><span class="sxs-lookup"><span data-stu-id="63125-177">This article is based on version 4.5.0 or later of hello script.</span></span>

<span data-ttu-id="63125-178">**Hello konfigurációs fájl létrehozása**</span><span class="sxs-lookup"><span data-stu-id="63125-178">**Create hello configuration file**</span></span>

 <span data-ttu-id="63125-179">hello HPC Pack IaaS telepítési parancsfájl egy konfigurációs XML-fájl hello infrastruktúra hello HPC-fürt bemenetként használja.</span><span class="sxs-lookup"><span data-stu-id="63125-179">hello HPC Pack IaaS deployment script uses an XML configuration file as input that describes hello infrastructure of hello HPC cluster.</span></span> <span data-ttu-id="63125-180">a fürt egy átjárócsomóponttal és 18 álló számítási csomópontjain hello számítási csomópont lemezkép, amely tartalmazza a Microsoft Excel alapján létre toodeploy értékek környezetnek a következő minta konfigurációs fájl hello helyettesítse.</span><span class="sxs-lookup"><span data-stu-id="63125-180">toodeploy a cluster consisting of a head node and 18 compute nodes created from hello compute node image that includes Microsoft Excel, substitute values for your environment into hello following sample configuration file.</span></span> <span data-ttu-id="63125-181">Hello konfigurációs fájllal kapcsolatos további információkért lásd: hello Manual.rtf fájlban hello parancsfájl és [HPC-fürt létrehozása hello HPC Pack IaaS telepítési parancsfájl](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="63125-181">For more information about hello configuration file, see hello Manual.rtf file in hello script folder and [Create an HPC cluster with hello HPC Pack IaaS deployment script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

<span data-ttu-id="63125-182">**Hello konfigurációs fájllal kapcsolatos megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="63125-182">**Notes about hello configuration file**</span></span>

* <span data-ttu-id="63125-183">Hello **VMName** hello központi csomópont **kell** kell hello ugyanaz, mint a hello **szolgáltatásnév**, vagy SOA-feladatok sikertelenek toorun.</span><span class="sxs-lookup"><span data-stu-id="63125-183">hello **VMName** of hello head node **MUST** be hello same as hello **ServiceName**, or SOA jobs fail toorun.</span></span>
* <span data-ttu-id="63125-184">Győződjön meg arról, hogy a megadott **EnableWebPortal** , hogy hello átjárócsomópont tanúsítvány jön létre, és exportálja.</span><span class="sxs-lookup"><span data-stu-id="63125-184">Make sure you specify **EnableWebPortal** so that hello head node certificate is generated and exported.</span></span>
* <span data-ttu-id="63125-185">hello fájl konfiguráció utáni PowerShell-parancsfájl hello átjárócsomópont futó PostConfig.ps1 határozza meg.</span><span class="sxs-lookup"><span data-stu-id="63125-185">hello file specifies a post-configuration PowerShell script PostConfig.ps1 that runs on hello head node.</span></span> <span data-ttu-id="63125-186">a következő mintaparancsfájl hello hello Azure tárolási kapcsolati karakterlánc konfigurálása, hello számítási csomópont szerepkör eltávolítása hello átjárócsomópont és összes csomópont online elérését, amikor központilag telepítették őket.</span><span class="sxs-lookup"><span data-stu-id="63125-186">hello following sample script configures hello Azure storage connection string, removes hello compute node role from hello head node, and brings all nodes online when they are deployed.</span></span> 

```
    # add hello HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set hello Azure storage connection string for hello cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove hello compute node role for head node toomake sure hello Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in hello deployment including hello head node and compute nodes, which should match hello number specified in hello XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

<span data-ttu-id="63125-187">**Hello parancsfájl futtatása**</span><span class="sxs-lookup"><span data-stu-id="63125-187">**Run hello script**</span></span>

1. <span data-ttu-id="63125-188">Nyissa meg rendszergazdaként egy PowerShell-konzolban hello hello ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="63125-188">Open hello PowerShell console on hello client computer as an administrator.</span></span>
2. <span data-ttu-id="63125-189">Directory toohello parancsfájl mappa módosítása (E:\IaaSClusterScript ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="63125-189">Change directory toohello script folder (E:\IaaSClusterScript in this example).</span></span>
   
   ```
   cd E:\IaaSClusterScript
   ```
3. <span data-ttu-id="63125-190">toodeploy hello HPC Pack fürthöz, futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="63125-190">toodeploy hello HPC Pack cluster, run hello following command.</span></span> <span data-ttu-id="63125-191">Ez a példa feltételezi, hogy hello a konfigurációs fájl E:\HPCDemoConfig.xml található.</span><span class="sxs-lookup"><span data-stu-id="63125-191">This example assumes that hello configuration file is located in E:\HPCDemoConfig.xml.</span></span>
   
   ```
   .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
   ```

<span data-ttu-id="63125-192">hello HPC Pack telepítési parancsfájlt futtatja egy kis ideig.</span><span class="sxs-lookup"><span data-stu-id="63125-192">hello HPC Pack deployment script runs for some time.</span></span> <span data-ttu-id="63125-193">Egy dolog hello parancsprogram tooexport és hello fürt tanúsítvány letöltése és mentheti hello ügyfélszámítógépen hello aktuális felhasználó Dokumentumok mappájának.</span><span class="sxs-lookup"><span data-stu-id="63125-193">One thing hello script does is tooexport and download hello cluster certificate and save it in hello current user’s Documents folder on hello client computer.</span></span> <span data-ttu-id="63125-194">hello parancsfájl állít elő, egy üzenet hasonló toohello következő.</span><span class="sxs-lookup"><span data-stu-id="63125-194">hello script generates a message similar toohello following.</span></span> <span data-ttu-id="63125-195">A következő lépésben hello tanúsítványt hello megfelelő tanúsítványtárolójában kell importálnia.</span><span class="sxs-lookup"><span data-stu-id="63125-195">In a following step, you import hello certificate in hello appropriate certificate store.</span></span>    

    You have enabled REST API or web portal on HPC Pack head node. Please import hello following certificate in hello Trusted Root Certification Authorities certificate store on hello computer where you are submitting job or accessing hello HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a><span data-ttu-id="63125-196">2. lépés</span><span class="sxs-lookup"><span data-stu-id="63125-196">Step 2.</span></span> <span data-ttu-id="63125-197">Excel-munkafüzetek kiszervezése és a felhasználó által megadott függvények futtatása egy helyszíni ügyfélről</span><span class="sxs-lookup"><span data-stu-id="63125-197">Offload Excel workbooks and run UDFs from an on-premises client</span></span>
### <a name="excel-activation"></a><span data-ttu-id="63125-198">Excel-aktiválás</span><span class="sxs-lookup"><span data-stu-id="63125-198">Excel activation</span></span>
<span data-ttu-id="63125-199">A termelési számítási feladatokhoz hello ComputeNodeWithExcel Virtuálisgép-lemezkép használatakor szüksége tooprovide egy érvényes Microsoft Office licenc kulcs tooactivate Excel hello számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="63125-199">When using hello ComputeNodeWithExcel VM image for production workloads, you need tooprovide a valid Microsoft Office license key tooactivate Excel on hello compute nodes.</span></span> <span data-ttu-id="63125-200">Ellenkező esetben Excel hello próbaverzióját 30 nap múlva lejár, és hello COMException (0x800AC472) Excel-munkafüzetek futtatása meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="63125-200">Otherwise, hello evaluation version of Excel expires after 30 days, and running Excel workbooks will fail with hello COMException (0x800AC472).</span></span> 

<span data-ttu-id="63125-201">Az értékelés időpontjához további 30 napra is állíthatnak alaphelyzetbe Excel: toohello átjárócsomópont és clusrun bejelentkezés `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` összes Excel a számítási csomópontok HPC Cluster Manager keresztül.</span><span class="sxs-lookup"><span data-stu-id="63125-201">You can rearm Excel for another 30 days of evaluation time: Log on toohello head node and clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` on all Excel compute nodes via HPC Cluster Manager.</span></span> <span data-ttu-id="63125-202">Legfeljebb kétszer is újra.</span><span class="sxs-lookup"><span data-stu-id="63125-202">You can rearm a maximum of two times.</span></span> <span data-ttu-id="63125-203">Ezt követően meg kell adnia egy érvényes Office licenckulcs.</span><span class="sxs-lookup"><span data-stu-id="63125-203">After that, you must provide a valid Office license key.</span></span>

<span data-ttu-id="63125-204">hello Office Professional Plus 2013 telepítve van a Virtuálisgép-lemezkép hello mennyiségi kiadását az általános mennyiségi licenc kulcsot (GVLK).</span><span class="sxs-lookup"><span data-stu-id="63125-204">hello Office Professional Plus 2013 installed on hello VM image is a volume edition with a Generic Volume License Key (GVLK).</span></span> <span data-ttu-id="63125-205">Kulcskezelő szolgáltatás (KMS) keresztül is aktiválhatja vagy Active Directory-alapú aktiválás (AD-BA), vagy a többször használható aktiválási kulcs (MAK).</span><span class="sxs-lookup"><span data-stu-id="63125-205">You can activate it via Key Management Service (KMS)/Active Directory-Based Activation (AD-BA) or Multiple Activation Key (MAK).</span></span> 

    * <span data-ttu-id="63125-206">toouse KMS/AD-BA, a meglévő KMS-kiszolgáló használata, vagy állítson be egy új hello Microsoft Office 2013 mennyiségi licenc csomag használatával.</span><span class="sxs-lookup"><span data-stu-id="63125-206">toouse KMS/AD-BA, use an existing KMS server or set up a new one by using hello Microsoft Office 2013 Volume License Pack.</span></span> <span data-ttu-id="63125-207">(Ha kívánja, hello kiszolgáló beállítása hello központi csomóponton.) Ezután aktiválja hello KMS-állomás kulcsát hello interneten keresztül vagy telefonon keresztül.</span><span class="sxs-lookup"><span data-stu-id="63125-207">(If you want to, set up hello server on hello head node.) Then, activate hello KMS host key via hello Internet or telephone.</span></span> <span data-ttu-id="63125-208">Majd clusrun `ospp.vbs` tooset hello KMS-kiszolgáló és a portot, és Office aktiválja az összes hello Excel számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="63125-208">Then clusrun `ospp.vbs` tooset hello KMS server and port and activate Office on all hello Excel compute nodes.</span></span> 

    * <span data-ttu-id="63125-209">MAK-ot, első clusrun toouse `ospp.vbs` tooinput hello kulcs, és majd aktiválása az összes hello Excel számítási csomópontok hello interneten vagy telefonon keresztül.</span><span class="sxs-lookup"><span data-stu-id="63125-209">toouse MAK, first clusrun `ospp.vbs` tooinput hello key and then activate all hello Excel compute nodes via hello Internet or telephone.</span></span> 

> [!NOTE]
> <span data-ttu-id="63125-210">Ez a Virtuálisgép-lemezkép nem használható Office Professional Plus 2013 kereskedelmi termékkulcsokat.</span><span class="sxs-lookup"><span data-stu-id="63125-210">Retail product keys for Office Professional Plus 2013 cannot be used with this VM image.</span></span> <span data-ttu-id="63125-211">Ha van érvényes kulcsok és a telepítési adathordozó Office vagy az Excel kiadás kivételével az Office Professional Plus 2013 mennyiségi kiadását, használhatja őket helyette.</span><span class="sxs-lookup"><span data-stu-id="63125-211">If you have valid keys and installation media for Office or Excel editions other than this Office Professional Plus 2013 volume edition, you can use them instead.</span></span> <span data-ttu-id="63125-212">Először távolítsa el a mennyiségi kiadását, és telepítse, hogy rendelkezik hello verziót.</span><span class="sxs-lookup"><span data-stu-id="63125-212">First uninstall this volume edition and install hello edition that you have.</span></span> <span data-ttu-id="63125-213">hello Excel számítási csomópont rögzíthető, a testre szabott VM kép toouse léptékű központi telepítés újratelepítése.</span><span class="sxs-lookup"><span data-stu-id="63125-213">hello reinstalled Excel compute node can be captured as a customized VM image toouse in a deployment at scale.</span></span>
> 
> 

### <a name="offload-excel-workbooks"></a><span data-ttu-id="63125-214">Excel-munkafüzetek kiszervezése</span><span class="sxs-lookup"><span data-stu-id="63125-214">Offload Excel workbooks</span></span>
<span data-ttu-id="63125-215">Kövesse ezeket a lépéseket toooffload egy Excel-munkafüzet futtatása az Azure-ban hello HPC Pack fürtön.</span><span class="sxs-lookup"><span data-stu-id="63125-215">Follow these steps toooffload an Excel workbook so that it runs on hello HPC Pack cluster in Azure.</span></span> <span data-ttu-id="63125-216">toodo, rendelkeznie kell az Excel 2010 vagy 2013 hello ügyfélszámítógépen már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="63125-216">toodo this, you must have Excel 2010 or 2013 already installed on hello client computer.</span></span>

1. <span data-ttu-id="63125-217">Használja az 1. lépés toodeploy egy HPC Pack fürt hello Excel hello-beállítások számítási csomópont kép.</span><span class="sxs-lookup"><span data-stu-id="63125-217">Use one of hello options in Step 1 toodeploy an HPC Pack cluster with hello Excel compute node image.</span></span> <span data-ttu-id="63125-218">Szerezze be a hello fürt tanúsítványfájlt (.cer) és a fürt felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="63125-218">Obtain hello cluster certificate file (.cer) and cluster username and password.</span></span>
2. <span data-ttu-id="63125-219">Hello ügyfélszámítógépen hello fürt tanúsítványt a Cert: \CurrentUser\Root importálni.</span><span class="sxs-lookup"><span data-stu-id="63125-219">On hello client computer, import hello cluster certificate under Cert:\CurrentUser\Root.</span></span>
3. <span data-ttu-id="63125-220">Győződjön meg arról, hogy telepítve van a Excel.</span><span class="sxs-lookup"><span data-stu-id="63125-220">Make sure Excel is installed.</span></span> <span data-ttu-id="63125-221">Hozzon létre egy Excel.exe.config fájlt hello hello tartalmát a következő mappában, amelyben Excel.exe hello ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="63125-221">Create an Excel.exe.config file with hello following contents in hello same folder as Excel.exe on hello client computer.</span></span> <span data-ttu-id="63125-222">Ez a lépés biztosítja, hogy hello HPC Pack 2012 R2 Excel COM beépülő modul sikeresen betöltődik.</span><span class="sxs-lookup"><span data-stu-id="63125-222">This step ensures that hello HPC Pack 2012 R2 Excel COM add-in loads successfully.</span></span>
   
    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
4. <span data-ttu-id="63125-223">Hello ügyfél toosubmit feladatok toohello HPC Pack fürt beállítása.</span><span class="sxs-lookup"><span data-stu-id="63125-223">Set up hello client toosubmit jobs toohello HPC Pack cluster.</span></span> <span data-ttu-id="63125-224">Egy elem toodownload hello teljes [HPC Pack 2012 R2 Update 3 telepítési](http://www.microsoft.com/download/details.aspx?id=49922) és hello HPC Pack ügyfél telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="63125-224">One option is toodownload hello full [HPC Pack 2012 R2 Update 3 installation](http://www.microsoft.com/download/details.aspx?id=49922) and install hello HPC Pack client.</span></span> <span data-ttu-id="63125-225">Azt is megteheti, töltse le és telepítse a hello [HPC Pack 2012 R2 Update 3 ügyfél segédprogramok](https://www.microsoft.com/download/details.aspx?id=49923) és a megfelelő Visual C++ 2010 újraterjeszthető csomag a számítógép hello ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).</span><span class="sxs-lookup"><span data-stu-id="63125-225">Alternatively, download and install hello [HPC Pack 2012 R2 Update 3 client utilities](https://www.microsoft.com/download/details.aspx?id=49923) and hello appropriate Visual C++ 2010 redistributable for your computer ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).</span></span>
5. <span data-ttu-id="63125-226">A jelen példában használjuk ConvertiblePricing_Complete.xlsb nevű minta Excel-munkafüzet.</span><span class="sxs-lookup"><span data-stu-id="63125-226">In this example, we use a sample Excel workbook named ConvertiblePricing_Complete.xlsb.</span></span> <span data-ttu-id="63125-227">Letöltheti a [Itt](https://www.microsoft.com/en-us/download/details.aspx?id=2939).</span><span class="sxs-lookup"><span data-stu-id="63125-227">You can download it [here](https://www.microsoft.com/en-us/download/details.aspx?id=2939).</span></span>
6. <span data-ttu-id="63125-228">Hello Excel munkafüzet tooa munkamappa D:\Excel\Run például másolja.</span><span class="sxs-lookup"><span data-stu-id="63125-228">Copy hello Excel workbook tooa working folder such as D:\Excel\Run.</span></span>
7. <span data-ttu-id="63125-229">Nyissa meg a hello Excel-munkafüzet.</span><span class="sxs-lookup"><span data-stu-id="63125-229">Open hello Excel workbook.</span></span> <span data-ttu-id="63125-230">A hello **Develop** menüszalag, kattintson a **COM-bővítmények** , és győződjön meg arról, hogy hello HPC Pack Excel COM-bővítmény sikeresen be van töltve.</span><span class="sxs-lookup"><span data-stu-id="63125-230">On hello **Develop** ribbon, click **COM Add-Ins** and confirm that hello HPC Pack Excel COM add-in is loaded successfully.</span></span>
   
   ![Excel-bővítmény HPC Pack][addin]
8. <span data-ttu-id="63125-232">Szerkesztés hello VBA makró Excel HPCControlMacros hello módosításával sorok megjegyzésként, ahogy az a következő parancsfájl hello.</span><span class="sxs-lookup"><span data-stu-id="63125-232">Edit hello VBA macro HPCControlMacros in Excel by changing hello commented lines as shown in hello following script.</span></span> <span data-ttu-id="63125-233">Helyettesítse be a környezetének megfelelő értékeket.</span><span class="sxs-lookup"><span data-stu-id="63125-233">Substitute appropriate values for your environment.</span></span>
   
   ![HPC Pack Excel makró][macro]
   
   ```
   'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
   Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"
   
   'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
   Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.Initialize ActiveWorkbook
   HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles
   
   'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
   HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
   HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
   ```
9. <span data-ttu-id="63125-235">Hello Excel munkafüzet tooan feltöltés címtár D:\Excel\Upload például másolja.</span><span class="sxs-lookup"><span data-stu-id="63125-235">Copy hello Excel workbook tooan upload directory such as D:\Excel\Upload.</span></span> <span data-ttu-id="63125-236">Ez a könyvtár hello HPC_DependsFiles állandó hello VBA makró van megadva.</span><span class="sxs-lookup"><span data-stu-id="63125-236">This directory is specified in hello HPC_DependsFiles constant in hello VBA macro.</span></span>
10. <span data-ttu-id="63125-237">toorun hello munkafüzet hello fürtön, az Azure-ban kattintson hello **fürt** hello munkalapon gombra.</span><span class="sxs-lookup"><span data-stu-id="63125-237">toorun hello workbook on hello cluster in Azure, click hello **Cluster** button on hello worksheet.</span></span>

### <a name="run-excel-udfs"></a><span data-ttu-id="63125-238">Excel univerzális Lemezformátumokat futtatása</span><span class="sxs-lookup"><span data-stu-id="63125-238">Run Excel UDFs</span></span>
<span data-ttu-id="63125-239">Excel univerzális Lemezformátumokat toorun hajtsa végre az előző lépésekben hello 1 – 3 tooset hello ügyfélszámítógépet.</span><span class="sxs-lookup"><span data-stu-id="63125-239">toorun Excel UDFs, follow hello preceding steps 1 – 3 tooset up hello client computer.</span></span> <span data-ttu-id="63125-240">Excel univerzális Lemezformátumokat toohave hello Excel alkalmazás telepítve van a számítási csomópontok nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="63125-240">For Excel UDFs, you don't need toohave hello Excel application installed on compute nodes.</span></span> <span data-ttu-id="63125-241">Amikor a fürt létrehozása számítási csomópontot, válassza a normál számítási csomópont lemezkép helyett hello számítási csomópont rendszerképének Excel.</span><span class="sxs-lookup"><span data-stu-id="63125-241">So, when creating your cluster compute nodes, you could choose a normal compute node image instead of hello compute node image with Excel.</span></span>

> [!NOTE]
> <span data-ttu-id="63125-242">Létrejön egy 34 karakteres korlátot, az Excel 2010 hello és 2013 fürt összekötő párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="63125-242">There is a 34 character limit in hello Excel 2010 and 2013 cluster connector dialog box.</span></span> <span data-ttu-id="63125-243">A párbeszédpanel bezárásához toospecify hello futtató fürtöt hello felhasználó által megadott függvények használhatja.</span><span class="sxs-lookup"><span data-stu-id="63125-243">You use this dialog box toospecify hello cluster that runs hello UDFs.</span></span> <span data-ttu-id="63125-244">Ha hosszabb hello teljes fürt neve (például hpcexcelhn01.southeastasia.cloudapp.azure.com), nem fér el hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="63125-244">If hello full cluster name is longer (for example, hpcexcelhn01.southeastasia.cloudapp.azure.com), it does not fit in hello dialog box.</span></span> <span data-ttu-id="63125-245">hello megoldás, tooset például gépre kiterjedő változó *CCP_IAASHN* hello hosszú fürtnév hello értékű.</span><span class="sxs-lookup"><span data-stu-id="63125-245">hello workaround is tooset a machine-wide variable such as *CCP_IAASHN* with hello value of hello long cluster name.</span></span> <span data-ttu-id="63125-246">Ezután írja be a *CCP_IAASHN %* hello párbeszédpanelen hello fürt átjárócsomópontjához neveként.</span><span class="sxs-lookup"><span data-stu-id="63125-246">Then, enter *%CCP_IAASHN%* in hello dialog box as hello cluster head node name.</span></span> 
> 
> 

<span data-ttu-id="63125-247">Hello fürt sikeres telepítése után folytassa a következő lépéseket toorun hello minta beépített Excel UDF.</span><span class="sxs-lookup"><span data-stu-id="63125-247">After hello cluster is successfully deployed, continue with hello following steps toorun a sample built-in Excel UDF.</span></span> <span data-ttu-id="63125-248">A testre szabott Excel univerzális Lemezformátumokat tapasztalja [erőforrások](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) toobuild hello XLL-EK, és telepítheti azokat a hello IaaS-fürtön.</span><span class="sxs-lookup"><span data-stu-id="63125-248">For customized Excel UDFs, see these [resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) toobuild hello XLLs and deploy them on hello IaaS cluster.</span></span>

1. <span data-ttu-id="63125-249">Nyisson meg egy új Excel-munkafüzet.</span><span class="sxs-lookup"><span data-stu-id="63125-249">Open a new Excel workbook.</span></span> <span data-ttu-id="63125-250">A hello **Develop** menüszalag, kattintson a **bővítmények**. A hello párbeszédpanelen kattintson **Tallózás**toohello %CCP_HOME%Bin\XLL32 mappa keresse meg és válassza ki a hello minta ClusterUDF32.xll.</span><span class="sxs-lookup"><span data-stu-id="63125-250">On hello **Develop** ribbon, click **Add-Ins**. Then, in hello dialog box, click **Browse**, navigate toohello %CCP_HOME%Bin\XLL32 folder, and select hello sample ClusterUDF32.xll.</span></span> <span data-ttu-id="63125-251">Ha hello ClusterUDF32 nem létezik hello ügyfélszámítógépen, másolásához hello átjárócsomópont hello %CCP_HOME%Bin\XLL32 mappából.</span><span class="sxs-lookup"><span data-stu-id="63125-251">If hello ClusterUDF32 doesn't exist on hello client machine, copy it from hello %CCP_HOME%Bin\XLL32 folder on hello head node.</span></span>
   
   ![Válassza ki a hello UDF-ben][udf]
2. <span data-ttu-id="63125-253">Kattintson a **fájl** > **beállítások** > **speciális**.</span><span class="sxs-lookup"><span data-stu-id="63125-253">Click **File** > **Options** > **Advanced**.</span></span> <span data-ttu-id="63125-254">A **képletek**, ellenőrizze **engedélyezése a felhasználó által definiált XLL funkciók toorun a számítási fürt**.</span><span class="sxs-lookup"><span data-stu-id="63125-254">Under **Formulas**, check **Allow user-defined XLL functions toorun a compute cluster**.</span></span> <span data-ttu-id="63125-255">Kattintson a **beállítások** , és írja be a teljes fürt neve hello a **fürt átjárócsomópontjához neve**.</span><span class="sxs-lookup"><span data-stu-id="63125-255">Then click **Options** and enter hello full cluster name in **Cluster head node name**.</span></span> <span data-ttu-id="63125-256">(Részben ismertetett beállításértékeket korábban a beviteli mezőbe az korlátozott too34 karakterből állhat, így előfordulhat, hogy nem felelnek meg egy hosszú neve.</span><span class="sxs-lookup"><span data-stu-id="63125-256">(As noted previously this input box is limited too34 characters, so a long cluster name may not fit.</span></span> <span data-ttu-id="63125-257">Használhatja a gépre kiterjedő változó nevét egy hosszú fürtnév.)</span><span class="sxs-lookup"><span data-stu-id="63125-257">You may use a machine-wide variable here for a long cluster name.)</span></span>
   
   ![Az UDF hello konfigurálása][options]
3. <span data-ttu-id="63125-259">toorun hello UDF számítási fürtön hello, hello cellába, amelynek értéke =XllGetComputerNameC() kattintson, és nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="63125-259">toorun hello UDF calculation on hello cluster, click hello cell with value =XllGetComputerNameC() and press Enter.</span></span> <span data-ttu-id="63125-260">hello függvény egyszerűen hello számítási csomópont, mely hello UDF fut. hello nevét kéri le.</span><span class="sxs-lookup"><span data-stu-id="63125-260">hello function simply retrieves hello name of hello compute node on which hello UDF runs.</span></span> <span data-ttu-id="63125-261">A hello először futtatja a hitelesítő adatok párbeszédpanel hello felhasználónév és jelszó tooconnect toohello IaaS fürt megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="63125-261">For hello first run, a credentials dialog box prompts for hello username and password tooconnect toohello IaaS cluster.</span></span>
   
   ![Futtassa az UDF-ben][run]
   
   <span data-ttu-id="63125-263">Ha sok cellát toocalculate, nyomja le az Alt-Shift-Ctrl + F9 toorun hello számítás összes cellákon.</span><span class="sxs-lookup"><span data-stu-id="63125-263">When there are many cells toocalculate, press Alt-Shift-Ctrl + F9 toorun hello calculation on all cells.</span></span>

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a><span data-ttu-id="63125-264">3. lépés</span><span class="sxs-lookup"><span data-stu-id="63125-264">Step 3.</span></span> <span data-ttu-id="63125-265">A SOA munkaterhelések futtatásához egy helyszíni ügyfélről</span><span class="sxs-lookup"><span data-stu-id="63125-265">Run a SOA workload from an on-premises client</span></span>
<span data-ttu-id="63125-266">általános SOA alkalmazások toorun hello HPC Pack IaaS fürtön, először hello módszerek valamelyikével 1. lépés toodeploy hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="63125-266">toorun general SOA applications on hello HPC Pack IaaS cluster, first use one of hello methods in Step 1 toodeploy hello cluster.</span></span> <span data-ttu-id="63125-267">Adjon meg általános számítási csomópont lemezképet ebben az esetben, mert Excel hello számítási csomóponton nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="63125-267">Specify a generic compute node image in this case, because you do not need Excel on hello compute nodes.</span></span> <span data-ttu-id="63125-268">Ezután kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="63125-268">Then follow these steps.</span></span>

1. <span data-ttu-id="63125-269">Hello fürt tanúsítvány beolvasása, után importálja a Cert: \CurrentUser\Root hello ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="63125-269">After retrieving hello cluster certificate, import it on hello client computer under Cert:\CurrentUser\Root.</span></span>
2. <span data-ttu-id="63125-270">Telepítse a hello [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) és [HPC Pack 2012 R2 Update 3 ügyfél segédprogramok](https://www.microsoft.com/download/details.aspx?id=49923).</span><span class="sxs-lookup"><span data-stu-id="63125-270">Install hello [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) and [HPC Pack 2012 R2 Update 3 client utilities](https://www.microsoft.com/download/details.aspx?id=49923).</span></span> <span data-ttu-id="63125-271">Ezek az eszközök lehetővé teszik a toodevelop és SOA ügyfélalkalmazások futtatását.</span><span class="sxs-lookup"><span data-stu-id="63125-271">These tools enable you toodevelop and run SOA client applications.</span></span>
3. <span data-ttu-id="63125-272">Töltse le a hello HelloWorldR2 [példakód](https://www.microsoft.com/download/details.aspx?id=41633).</span><span class="sxs-lookup"><span data-stu-id="63125-272">Download hello HelloWorldR2 [sample code](https://www.microsoft.com/download/details.aspx?id=41633).</span></span> <span data-ttu-id="63125-273">Nyissa meg a Visual Studio 2010 HelloWorldR2.sln hello vagy 2012.</span><span class="sxs-lookup"><span data-stu-id="63125-273">Open hello HelloWorldR2.sln in Visual Studio 2010 or 2012.</span></span> <span data-ttu-id="63125-274">(Ez a minta nincs jelenleg kompatibilis a Visual Studio legújabb verziói.)</span><span class="sxs-lookup"><span data-stu-id="63125-274">(This sample is not currently compatible with more recent versions of Visual Studio.)</span></span>
4. <span data-ttu-id="63125-275">Először a hello EchoService projekt felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="63125-275">Build hello EchoService project first.</span></span> <span data-ttu-id="63125-276">Ezt követően a hello szolgáltatás toohello IaaS-fürt központi telepítése a hello ugyanúgy telepítheti tooan a helyi fürthöz.</span><span class="sxs-lookup"><span data-stu-id="63125-276">Then, deploy hello service toohello IaaS cluster in hello same way you deploy tooan on-premises cluster.</span></span> <span data-ttu-id="63125-277">Részletes útmutató: a HelloWordR2 Readme.doc hello.</span><span class="sxs-lookup"><span data-stu-id="63125-277">For detailed steps, see hello Readme.doc in HelloWordR2.</span></span> <span data-ttu-id="63125-278">Módosíthatja, és hello HellWorldR2 és más projektek hozhat létre a következő szakasz toogenerate hello SOA ügyfélalkalmazások Azure IaaS fürtökön futó hello leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="63125-278">Modify and build hello HellWorldR2 and other projects as described in hello following section toogenerate hello SOA client applications that run on an Azure IaaS cluster.</span></span>

### <a name="use-http-binding-with-azure-storage-queue"></a><span data-ttu-id="63125-279">Http-kötés használata az Azure storage üzenetsorába</span><span class="sxs-lookup"><span data-stu-id="63125-279">Use Http binding with Azure storage queue</span></span>
<span data-ttu-id="63125-280">egy Azure storage üzenetsorába toouse Http kötés módosításokat néhány toohello mintakód.</span><span class="sxs-lookup"><span data-stu-id="63125-280">toouse Http binding with an Azure storage queue, make a few changes toohello sample code.</span></span>

* <span data-ttu-id="63125-281">Frissítse a hello fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="63125-281">Update hello cluster name.</span></span>
  
    ```
  // Before
  const string headnode = "[headnode]";
  // After e.g.
  const string headnode = "hpc01.eastus.cloudapp.azure.com";
  or
  const string headnode = "hpc01.cloudapp.net";
  ```
* <span data-ttu-id="63125-282">Másik lehetőségként SessionStartInfo hello alapértelmezett TransportScheme használja, vagy explicit módon állítsa be tooHttp.</span><span class="sxs-lookup"><span data-stu-id="63125-282">Optionally, use hello default TransportScheme in SessionStartInfo or explicitly set it tooHttp.</span></span>

```
    info.TransportScheme = TransportScheme.Http;
```

* <span data-ttu-id="63125-283">Hello BrokerClient alapértelmezett kötés használja.</span><span class="sxs-lookup"><span data-stu-id="63125-283">Use default binding for hello BrokerClient.</span></span>
  
    ```
  // Before
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
  // After
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
  ```
  
    <span data-ttu-id="63125-284">Vagy állítsa be explicit módon használja a hello basicHttpBinding.</span><span class="sxs-lookup"><span data-stu-id="63125-284">Or set explicitly using hello basicHttpBinding.</span></span>
  
    ```
  BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
  binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
  ```
* <span data-ttu-id="63125-285">Beállíthatja hello UseAzureQueue jelző tootrue is SessionStartInfo.</span><span class="sxs-lookup"><span data-stu-id="63125-285">Optionally, set hello UseAzureQueue flag tootrue in SessionStartInfo.</span></span> <span data-ttu-id="63125-286">Ha nincs beállítva, lesz beállítva tootrue alapértelmezés szerint amikor hello fürtnév Azure tartományutótagok és hello TransportScheme Http.</span><span class="sxs-lookup"><span data-stu-id="63125-286">If not set, it will be set tootrue by default when hello cluster name has Azure domain suffixes and hello TransportScheme is Http.</span></span>
  
    ```
    info.UseAzureQueue = true;
  ```

### <a name="use-http-binding-without-azure-storage-queue"></a><span data-ttu-id="63125-287">Http-kötés nélkül az Azure storage-várólista használata</span><span class="sxs-lookup"><span data-stu-id="63125-287">Use Http binding without Azure storage queue</span></span>
<span data-ttu-id="63125-288">toouse Http-kötés egy Azure storage várólista, explicit módon beállítva hello UseAzureQueue jelző toofalse a hello SessionStartInfo nélkül.</span><span class="sxs-lookup"><span data-stu-id="63125-288">toouse Http binding without an Azure storage queue, explicitly set hello UseAzureQueue flag toofalse in hello SessionStartInfo.</span></span>

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a><span data-ttu-id="63125-289">NetTcp kötés használata</span><span class="sxs-lookup"><span data-stu-id="63125-289">Use NetTcp binding</span></span>
<span data-ttu-id="63125-290">toouse NetTcp hello konfigurálása még kötés, hasonló tooconnecting tooan a helyi fürthöz.</span><span class="sxs-lookup"><span data-stu-id="63125-290">toouse NetTcp binding, hello configuration is similar tooconnecting tooan on-premises cluster.</span></span> <span data-ttu-id="63125-291">Tooopen kell néhány hello átjárócsomópont virtuális gép végpontja.</span><span class="sxs-lookup"><span data-stu-id="63125-291">You need tooopen a few endpoints on hello head node VM.</span></span> <span data-ttu-id="63125-292">Ha hello HPC Pack IaaS telepítési parancsfájl toocreate hello fürt használt, például set hello végpontok hello Azure-portálon az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="63125-292">If you used hello HPC Pack IaaS deployment script toocreate hello cluster, for example, set hello endpoints in hello Azure portal as follows.</span></span>

1. <span data-ttu-id="63125-293">Állítsa le a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="63125-293">Stop hello VM.</span></span>
2. <span data-ttu-id="63125-294">Adja hozzá a hello TCP-portok 9090, 9087, 9091, a munkamenet, hello 9094 Replikaszervező, munkavégző és adatszolgáltatások, illetve Replikaszervező</span><span class="sxs-lookup"><span data-stu-id="63125-294">Add hello TCP ports 9090, 9087, 9091, 9094 for hello Session, Broker, Broker worker, and Data services, respectively</span></span>
   
    ![Végpontok konfigurálása][endpoint-new-portal]
3. <span data-ttu-id="63125-296">Indítsa el a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="63125-296">Start hello VM.</span></span>

<span data-ttu-id="63125-297">hello SOA ügyfélalkalmazás nem kell módosítani a módosítása hello központi toohello IaaS fürt teljes név kivételével.</span><span class="sxs-lookup"><span data-stu-id="63125-297">hello SOA client application requires no changes except altering hello head name toohello IaaS cluster full name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="63125-298">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="63125-298">Next steps</span></span>
* <span data-ttu-id="63125-299">Lásd: [ezeket az erőforrásokat](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) HPC Pack Excel munkaterhelések futtatásával kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="63125-299">See [these resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) for more information about running Excel workloads with HPC Pack.</span></span>
* <span data-ttu-id="63125-300">Lásd: [SOA-szolgáltatások kezelése a Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) központi telepítésére és felügyeletére SOA szolgáltatások HPC Pack bővebben.</span><span class="sxs-lookup"><span data-stu-id="63125-300">See [Managing SOA Services in Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) for more about deploying and managing SOA services with HPC Pack.</span></span>

<!--Image references-->
[scenario]: ./media/excel-cluster-hpcpack/scenario.png
[github]: ./media/excel-cluster-hpcpack/github.png
[template]: ./media/excel-cluster-hpcpack/template.png
[parameters]: ./media/excel-cluster-hpcpack/parameters.png
[parameters-new-portal]: ./media/excel-cluster-hpcpack/parameters-new-portal.png
[create]: ./media/excel-cluster-hpcpack/create.png
[connect]: ./media/excel-cluster-hpcpack/connect.png
[cert]: ./media/excel-cluster-hpcpack/cert.png
[addin]: ./media/excel-cluster-hpcpack/addin.png
[macro]: ./media/excel-cluster-hpcpack/macro.png
[options]: ./media/excel-cluster-hpcpack/options.png
[run]: ./media/excel-cluster-hpcpack/run.png
[endpoint]: ./media/excel-cluster-hpcpack/endpoint.png
[endpoint-new-portal]: ./media/excel-cluster-hpcpack/endpoint-new-portal.png
[udf]: ./media/excel-cluster-hpcpack/udf.png
