---
title: "Az Excel és SOA HPC Pack fürt |} Microsoft Docs"
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
ms.openlocfilehash: 63babd94fdab15217cfb0757e4cd6efe458a628d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="318c1-103">Ismerkedés az Excel és a SOA munkaterhelések egy HPC Pack fürtben futó Azure-ban</span><span class="sxs-lookup"><span data-stu-id="318c1-103">Get started running Excel and SOA workloads on an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="318c1-104">Ez a cikk bemutatja, hogyan Azure virtuális gépeken futó Microsoft HPC Pack 2012 R2 fürt központi telepítése egy Azure gyors üzembe helyezés sablont, vagy opcionálisan egy Azure PowerShell telepítési parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="318c1-104">This article shows you how to deploy a Microsoft HPC Pack 2012 R2 cluster on Azure virtual machines by using an Azure quickstart template, or optionally an Azure PowerShell deployment script.</span></span> <span data-ttu-id="318c1-105">A fürt úgy tervezték, hogy a Microsoft Excel alkalmazást vagy szolgáltatásorientált architektúra (SOA) munkaterhelések futtatása HPC Pack Azure piactér Virtuálisgép-rendszerképekről használja.</span><span class="sxs-lookup"><span data-stu-id="318c1-105">The cluster uses Azure Marketplace VM images designed to run Microsoft Excel or service-oriented architecture (SOA) workloads with HPC Pack.</span></span> <span data-ttu-id="318c1-106">A fürt használhatja a helyszíni ügyfél-számítógépről Excel HPC és SOA szolgáltatások futtatásához.</span><span class="sxs-lookup"><span data-stu-id="318c1-106">You can use the cluster to run Excel HPC and SOA services from an on-premises client computer.</span></span> <span data-ttu-id="318c1-107">Excel-munkafüzet kiürítéséhez és a felhasználó által definiált függvények Excel vagy a felhasználó által megadott függvények közé tartoznak az Excelhez készült HPC-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="318c1-107">The Excel HPC services include Excel workbook offloading and Excel user-defined functions, or UDFs.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="318c1-108">Ez a cikk a HPC Pack 2012 R2 alapul funkciók, sablonok és parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="318c1-108">This article is based on features, templates, and scripts for HPC Pack 2012 R2.</span></span> <span data-ttu-id="318c1-109">Ebben a forgatókönyvben jelenleg nem támogatott a HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="318c1-109">This scenario is not currently supported in HPC Pack 2016.</span></span>
>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="318c1-110">Magas szinten a következő ábrán a HPC Pack fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="318c1-110">At a high level, the following diagram shows the HPC Pack cluster you create.</span></span>

![Excel munkaterheket futtatnak csomópontok HPC-fürt][scenario]

## <a name="prerequisites"></a><span data-ttu-id="318c1-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="318c1-112">Prerequisites</span></span>
* <span data-ttu-id="318c1-113">**Ügyfélszámítógép** -elküldeni az Excel és SOA mintafeladatok a fürthöz ügyfél Windows-alapú számítógépre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="318c1-113">**Client computer** - You need a Windows-based client computer to submit sample Excel and SOA jobs to the cluster.</span></span> <span data-ttu-id="318c1-114">Szükség (ha az adott központi telepítési módszer választása) az Azure PowerShell-fürt üzembe helyezési parancsfájl futtatásához egy Windows-számítógép.</span><span class="sxs-lookup"><span data-stu-id="318c1-114">You also need a Windows computer to run the Azure PowerShell cluster deployment script (if you choose that deployment method).</span></span>
* <span data-ttu-id="318c1-115">**Azure-előfizetés** – Ha nem rendelkezik Azure-előfizetéssel, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/) néhány percig.</span><span class="sxs-lookup"><span data-stu-id="318c1-115">**Azure subscription** - If you don't have an Azure subscription, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.</span></span>
* <span data-ttu-id="318c1-116">**Magok kvóta** -előfordulhat, hogy növelnie kell a beállított kvótát mag, különösen akkor, ha több fürtcsomóponton az multicore Virtuálisgép-méretek telepít.</span><span class="sxs-lookup"><span data-stu-id="318c1-116">**Cores quota** - You might need to increase the quota of cores, especially if you deploy several cluster nodes with multicore VM sizes.</span></span> <span data-ttu-id="318c1-117">Egy Azure gyors üzembe helyezés sablont használ, ha a magok kvótája a Resource Manager egy Azure-régióban van.</span><span class="sxs-lookup"><span data-stu-id="318c1-117">If you are using an Azure quickstart template, the cores quota in Resource Manager is per Azure region.</span></span> <span data-ttu-id="318c1-118">Ebben az esetben szükség lehet egy adott régióban a kvóta növeléséhez.</span><span class="sxs-lookup"><span data-stu-id="318c1-118">In that case, you might need to increase the quota in a specific region.</span></span> <span data-ttu-id="318c1-119">Lásd: [Azure-előfizetésre vonatkozó korlátok, kvóták és megkötések](../../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="318c1-119">See [Azure subscription limits, quotas, and constraints](../../azure-subscription-service-limits.md).</span></span> <span data-ttu-id="318c1-120">A kvóta növeléséhez [nyissa meg az online támogatás ügyfélkérés](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) díjmentesen.</span><span class="sxs-lookup"><span data-stu-id="318c1-120">To increase a quota, [open an online customer support request](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) at no charge.</span></span>
* <span data-ttu-id="318c1-121">**A Microsoft Office-licencet** – Ha a számítási csomópontok piactér HPC Pack 2012 R2 Virtuálisgép-lemezkép használatával a Microsoft Excel, a 30 napos próbaverzióját Microsoft Excel Professional Plus 2013 telepítve van.</span><span class="sxs-lookup"><span data-stu-id="318c1-121">**Microsoft Office license** - If you deploy compute nodes using a Marketplace HPC Pack 2012 R2 VM image with Microsoft Excel, a 30-day evaluation version of Microsoft Excel Professional Plus 2013 is installed.</span></span> <span data-ttu-id="318c1-122">A próbaidőszak után meg kell adnia a Microsoft Office Excel továbbra is alkalmazásokat és szolgáltatásokat futtathatnak aktiválásához érvényes licenc.</span><span class="sxs-lookup"><span data-stu-id="318c1-122">After the evaluation period, you need to provide a valid Microsoft Office license to activate Excel to continue to run workloads.</span></span> <span data-ttu-id="318c1-123">Lásd: [Excel-aktiválási](#excel-activation) című cikkben.</span><span class="sxs-lookup"><span data-stu-id="318c1-123">See [Excel activation](#excel-activation) later in this article.</span></span> 

## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="318c1-124">1. lépés</span><span class="sxs-lookup"><span data-stu-id="318c1-124">Step 1.</span></span> <span data-ttu-id="318c1-125">Az Azure-ban egy HPC Pack fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="318c1-125">Set up an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="318c1-126">Megmutatjuk, két lehetőség közül választhat a HPC Pack 2012 R2-fürt beállításához: első, egy Azure gyors üzembe helyezés sablont és az Azure-portálon; és a második, az Azure PowerShell telepítési parancsfájlt használ.</span><span class="sxs-lookup"><span data-stu-id="318c1-126">We show two options to set up the HPC Pack 2012 R2 cluster: first, using an Azure quickstart template and the Azure portal; and second, using an Azure PowerShell deployment script.</span></span>

### <a name="option-1-use-a-quickstart-template"></a><span data-ttu-id="318c1-127">1. lehetőség.</span><span class="sxs-lookup"><span data-stu-id="318c1-127">Option 1.</span></span> <span data-ttu-id="318c1-128">A következő gyorsindítási sablonon használata</span><span class="sxs-lookup"><span data-stu-id="318c1-128">Use a quickstart template</span></span>
<span data-ttu-id="318c1-129">Egy Azure gyors üzembe helyezés sablon használatával gyorsan HPC Pack-fürt üzembe helyezése az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="318c1-129">Use an Azure quickstart template to quickly deploy an HPC Pack cluster in the Azure portal.</span></span> <span data-ttu-id="318c1-130">A sablon a portálon megnyitásakor egy egyszerű felhasználói felület, ahol a beállítások megadása a fürt kap.</span><span class="sxs-lookup"><span data-stu-id="318c1-130">When you open the template in the portal, you get a simple UI where you enter the settings for your cluster.</span></span> <span data-ttu-id="318c1-131">A lépések a következők.</span><span class="sxs-lookup"><span data-stu-id="318c1-131">Here are the steps.</span></span> 

> [!TIP]
> <span data-ttu-id="318c1-132">Ha azt szeretné, egy [Azure piactér sablon](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) , amely kifejezetten a Excel munkaterhelések hasonló fürt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="318c1-132">If you want, use an [Azure Marketplace template](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) that creates a similar cluster specifically for Excel workloads.</span></span> <span data-ttu-id="318c1-133">A lépések némileg eltér a következő.</span><span class="sxs-lookup"><span data-stu-id="318c1-133">The steps differ slightly from the following.</span></span>
> 
> 

1. <span data-ttu-id="318c1-134">Látogasson el a [sablonlap HPC-fürt létrehozása a Githubon](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster).</span><span class="sxs-lookup"><span data-stu-id="318c1-134">Visit the [Create HPC Cluster template page on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster).</span></span> <span data-ttu-id="318c1-135">Ha azt szeretné, tekintse át a sablon és a forráskódot.</span><span class="sxs-lookup"><span data-stu-id="318c1-135">If you want, review information about the template and the source code.</span></span>
2. <span data-ttu-id="318c1-136">Kattintson a **az Azure telepítéséhez** a központi telepítés elindítása a sablon az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="318c1-136">Click **Deploy to Azure** to start a deployment with the template in the Azure portal.</span></span>
   
   ![Az Azure-sablon üzembe helyezése][github]
3. <span data-ttu-id="318c1-138">A portál lépések végrehajtásával adja meg a paramétereket a HPC-fürt sablon.</span><span class="sxs-lookup"><span data-stu-id="318c1-138">In the portal, follow these steps to enter the parameters for the HPC cluster template.</span></span>
   
   <span data-ttu-id="318c1-139">a.</span><span class="sxs-lookup"><span data-stu-id="318c1-139">a.</span></span> <span data-ttu-id="318c1-140">Az a **paraméterek** lapon adja meg, vagy módosítsa a sablon paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="318c1-140">On the **Parameters** page, enter or modify values for the template parameters.</span></span> <span data-ttu-id="318c1-141">(Kattintson a beállítások mellett lévő ikonra súgójában talál.) A következő képernyő mintaértékek láthatók.</span><span class="sxs-lookup"><span data-stu-id="318c1-141">(Click the icon next to each setting for help information.) Sample values are shown in the following screen.</span></span> <span data-ttu-id="318c1-142">Ez a példa egy nevű fürtöt hoz létre *hpc01* a a *hpc.local* tartomány egy átjárócsomóponttal és 2 álló számítási csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="318c1-142">This example creates a cluster named *hpc01* in the *hpc.local* domain consisting of a head node and 2 compute nodes.</span></span> <span data-ttu-id="318c1-143">A számítási csomópontok HPC Pack VM-lemezkép, amely tartalmazza a Microsoft Excel készített.</span><span class="sxs-lookup"><span data-stu-id="318c1-143">The compute nodes are created from an HPC Pack VM image that includes Microsoft Excel.</span></span>
   
   ![Adja meg a paraméterek][parameters-new-portal]
   
   > [!NOTE]
   > <span data-ttu-id="318c1-145">A virtuális gép alapján automatikusan létrehozott átjárócsomópont a [legújabb Piactéri lemezképhez](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) HPC Pack 2012 R2 Windows Server 2012 R2 rendszeren.</span><span class="sxs-lookup"><span data-stu-id="318c1-145">The head node VM is created automatically from the [latest Marketplace image](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) of HPC Pack 2012 R2 on Windows Server 2012 R2.</span></span> <span data-ttu-id="318c1-146">Jelenleg a kép HPC Pack 2012 R2 Update 3 alapul.</span><span class="sxs-lookup"><span data-stu-id="318c1-146">Currently the image is based on HPC Pack 2012 R2 Update 3.</span></span>
   > 
   > <span data-ttu-id="318c1-147">A kijelölt számítási csomópont termékcsalád a legújabb lemezképből számítási csomópont virtuális gépek jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="318c1-147">Compute node VMs are created from the latest image of the selected compute node family.</span></span> <span data-ttu-id="318c1-148">Válassza ki a **ComputeNodeWithExcel** beállítás megadása a legújabb HPC Pack számítási csomópont kép, amely tartalmazza a Microsoft Excel Professional Plus 2013 próbaverzióját.</span><span class="sxs-lookup"><span data-stu-id="318c1-148">Select the **ComputeNodeWithExcel** option for the latest HPC Pack compute node image that includes an evaluation version of Microsoft Excel Professional Plus 2013.</span></span> <span data-ttu-id="318c1-149">Általános SOA munkamenetek vagy Excel UDF kiszervezésével fürt központi telepítése, válassza ki a **Átjárócsomópontján** (nélkül telepített Excel) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="318c1-149">To deploy a cluster for general SOA sessions or for Excel UDF offloading, choose the **ComputeNode** option (without Excel installed).</span></span>
   > 
   > 
   
   <span data-ttu-id="318c1-150">b.</span><span class="sxs-lookup"><span data-stu-id="318c1-150">b.</span></span> <span data-ttu-id="318c1-151">Válassza ki az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="318c1-151">Choose the subscription.</span></span>
   
   <span data-ttu-id="318c1-152">c.</span><span class="sxs-lookup"><span data-stu-id="318c1-152">c.</span></span> <span data-ttu-id="318c1-153">Hozzon létre például egy erőforráscsoportot a fürt *hpc01RG*.</span><span class="sxs-lookup"><span data-stu-id="318c1-153">Create a resource group for the cluster, such as *hpc01RG*.</span></span>
   
   <span data-ttu-id="318c1-154">d.</span><span class="sxs-lookup"><span data-stu-id="318c1-154">d.</span></span> <span data-ttu-id="318c1-155">Válassza ki az erőforráscsoportot, például az USA középső RÉGIÓJA helyét.</span><span class="sxs-lookup"><span data-stu-id="318c1-155">Choose a location for the resource group, such as Central US.</span></span>
   
   <span data-ttu-id="318c1-156">e.</span><span class="sxs-lookup"><span data-stu-id="318c1-156">e.</span></span> <span data-ttu-id="318c1-157">Az a **jogi feltételeket** lapon, olvassa el a feltételeket.</span><span class="sxs-lookup"><span data-stu-id="318c1-157">On the **Legal terms** page, review the terms.</span></span> <span data-ttu-id="318c1-158">Ha elfogadja, kattintson a **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="318c1-158">If you agree, click **Purchase**.</span></span> <span data-ttu-id="318c1-159">Ha elkészült, a beállítás a sablon értéke, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="318c1-159">Then, when you are finished setting the values for the template, click **Create**.</span></span>
4. <span data-ttu-id="318c1-160">A telepítés befejezésekor (általában tart körülbelül 30 percig), a fürt tanúsítványfájl exportálása a fürt átjárócsomópontjából.</span><span class="sxs-lookup"><span data-stu-id="318c1-160">When the deployment completes (it typically takes around 30 minutes), export the cluster certificate file from the cluster head node.</span></span> <span data-ttu-id="318c1-161">Egy későbbi lépésben importálni a nyilvános tanúsítvány biztonságos HTTP-kötés a kiszolgálóoldali hitelesítése az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="318c1-161">In a later step, you import this public certificate on the client computer to provide the server-side authentication for secure HTTP binding.</span></span>
   
   <span data-ttu-id="318c1-162">a.</span><span class="sxs-lookup"><span data-stu-id="318c1-162">a.</span></span> <span data-ttu-id="318c1-163">Az Azure portálon, nyissa meg az irányítópultot, válassza ki az átjárócsomóponthoz, és kattintson a **Connect** csatlakozni a távoli asztal segítségével a lap tetején.</span><span class="sxs-lookup"><span data-stu-id="318c1-163">In the Azure portal, go to the dashboard, select the head node, and click **Connect** at the top of the page to connect using Remote Desktop.</span></span>
   
    <!-- ![Connect to the head node][connect] -->
   
   <span data-ttu-id="318c1-164">b.</span><span class="sxs-lookup"><span data-stu-id="318c1-164">b.</span></span> <span data-ttu-id="318c1-165">Standard eljárások segítségével a Tanúsítványkezelő az átjárócsomópont (az található a Cert: \LocalMachine\My) a titkos kulcs nélküli tanúsítvány exportálása.</span><span class="sxs-lookup"><span data-stu-id="318c1-165">Use standard procedures in Certificate Manager to export the head node certificate (located under Cert:\LocalMachine\My) without the private key.</span></span> <span data-ttu-id="318c1-166">Ebben a példában exportálása *CN = hpc01.eastus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="318c1-166">In this example, export *CN = hpc01.eastus.cloudapp.azure.com*.</span></span>
   
   ![A tanúsítvány exportálása][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="318c1-168">2. lehetőség.</span><span class="sxs-lookup"><span data-stu-id="318c1-168">Option 2.</span></span> <span data-ttu-id="318c1-169">A HPC Pack IaaS telepítési parancsfájl használata</span><span class="sxs-lookup"><span data-stu-id="318c1-169">Use the HPC Pack IaaS Deployment script</span></span>
<span data-ttu-id="318c1-170">A HPC Pack IaaS telepítési parancsfájl segítségével egy másik sokoldalú HPC Pack-fürt üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="318c1-170">The HPC Pack IaaS deployment script provides another versatile way to deploy an HPC Pack cluster.</span></span> <span data-ttu-id="318c1-171">A fürt létrehoz a klasszikus üzembe helyezési modellel, mivel a sablon az Azure Resource Manager telepítési modellt használ.</span><span class="sxs-lookup"><span data-stu-id="318c1-171">It creates a cluster in the classic deployment model, whereas the template uses the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="318c1-172">A parancsfájl is kompatibilis Azure globális vagy Azure Kína szolgáltatásban előfizetés.</span><span class="sxs-lookup"><span data-stu-id="318c1-172">Also, the script is compatible with a subscription in either the Azure Global or Azure China service.</span></span>

<span data-ttu-id="318c1-173">**További Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="318c1-173">**Additional prerequisites**</span></span>

* <span data-ttu-id="318c1-174">**Az Azure PowerShell** - [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview) (0.8.10 verzió vagy újabb) az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="318c1-174">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="318c1-175">**HPC Pack IaaS telepítési parancsfájl** - töltse le és csomagolja ki a parancsfájlt a legújabb verzióját a [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="318c1-175">**HPC Pack IaaS deployment script** - Download and unpack the latest version of the script from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="318c1-176">A verziószám a parancsfájl futtatásával `New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="318c1-176">Check the version of the script by running `New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="318c1-177">Ez a cikk verzió 4.5.0 vagy később, a parancsfájl alapján.</span><span class="sxs-lookup"><span data-stu-id="318c1-177">This article is based on version 4.5.0 or later of the script.</span></span>

<span data-ttu-id="318c1-178">**A konfigurációs fájl létrehozása**</span><span class="sxs-lookup"><span data-stu-id="318c1-178">**Create the configuration file**</span></span>

 <span data-ttu-id="318c1-179">A HPC Pack IaaS telepítési parancsfájl egy konfigurációs XML-fájl, amely az infrastruktúra a HPC-fürt bemenetként használja.</span><span class="sxs-lookup"><span data-stu-id="318c1-179">The HPC Pack IaaS deployment script uses an XML configuration file as input that describes the infrastructure of the HPC cluster.</span></span> <span data-ttu-id="318c1-180">Egy átjárócsomóponttal és 18 számítási csomópontokat, amely tartalmazza a Microsoft Excel számítási csomópont lemezkép alapján létre álló fürt központi telepítése, helyettesítse be az értékek környezetnek az alábbi minta konfigurációs fájlba.</span><span class="sxs-lookup"><span data-stu-id="318c1-180">To deploy a cluster consisting of a head node and 18 compute nodes created from the compute node image that includes Microsoft Excel, substitute values for your environment into the following sample configuration file.</span></span> <span data-ttu-id="318c1-181">A konfigurációs fájl kapcsolatos további információkért tekintse meg a Manual.rtf a parancsfájl mappában és [HPC-fürt létrehozása a HPC Pack IaaS telepítési parancsfájl](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="318c1-181">For more information about the configuration file, see the Manual.rtf file in the script folder and [Create an HPC cluster with the HPC Pack IaaS deployment script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

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

<span data-ttu-id="318c1-182">**A konfigurációs fájl kapcsolatos megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="318c1-182">**Notes about the configuration file**</span></span>

* <span data-ttu-id="318c1-183">A **VMName** , az átjárócsomópont **kell** lehet azonos a **szolgáltatásnév**, vagy SOA-feladatok nem indulnak el.</span><span class="sxs-lookup"><span data-stu-id="318c1-183">The **VMName** of the head node **MUST** be the same as the **ServiceName**, or SOA jobs fail to run.</span></span>
* <span data-ttu-id="318c1-184">Győződjön meg arról, hogy a megadott **EnableWebPortal** így átjárócsomópontjához tanúsítvány jön létre, és exportálja.</span><span class="sxs-lookup"><span data-stu-id="318c1-184">Make sure you specify **EnableWebPortal** so that the head node certificate is generated and exported.</span></span>
* <span data-ttu-id="318c1-185">A fájl egy konfigurációt követő PowerShell-parancsfájlt a központi csomóponton futó PostConfig.ps1 határozza meg.</span><span class="sxs-lookup"><span data-stu-id="318c1-185">The file specifies a post-configuration PowerShell script PostConfig.ps1 that runs on the head node.</span></span> <span data-ttu-id="318c1-186">Az alábbi mintaparancsfájl konfigurálása az Azure storage kapcsolati karakterlánc, a számítási csomópont szerepkör eltávolítása az átjárócsomóponthoz, és az összes csomópont online elérését, amikor központilag telepítették őket.</span><span class="sxs-lookup"><span data-stu-id="318c1-186">THe following sample script configures the Azure storage connection string, removes the compute node role from the head node, and brings all nodes online when they are deployed.</span></span> 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
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

<span data-ttu-id="318c1-187">**A parancsfájl futtatása**</span><span class="sxs-lookup"><span data-stu-id="318c1-187">**Run the script**</span></span>

1. <span data-ttu-id="318c1-188">Nyissa meg rendszergazdaként a PowerShell-konzolon, az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="318c1-188">Open the PowerShell console on the client computer as an administrator.</span></span>
2. <span data-ttu-id="318c1-189">Módosítsa a könyvtárat a parancsfájl mappába (E:\IaaSClusterScript ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="318c1-189">Change directory to the script folder (E:\IaaSClusterScript in this example).</span></span>
   
   ```
   cd E:\IaaSClusterScript
   ```
3. <span data-ttu-id="318c1-190">A következő parancsot a HPC Pack fürt telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="318c1-190">To deploy the HPC Pack cluster, run the following command.</span></span> <span data-ttu-id="318c1-191">Ez a példa feltételezi, hogy a konfigurációs fájlban található E:\HPCDemoConfig.xml.</span><span class="sxs-lookup"><span data-stu-id="318c1-191">This example assumes that the configuration file is located in E:\HPCDemoConfig.xml.</span></span>
   
   ```
   .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
   ```

<span data-ttu-id="318c1-192">A HPC Pack telepítési parancsfájlt futtatja egy kis ideig.</span><span class="sxs-lookup"><span data-stu-id="318c1-192">The HPC Pack deployment script runs for some time.</span></span> <span data-ttu-id="318c1-193">A parancsprogram egyetlen művelet exportálni, és töltse le a fürt tanúsítványt, és mentse az ügyfélszámítógépen az aktuális felhasználó Dokumentumok mappájának-hoz.</span><span class="sxs-lookup"><span data-stu-id="318c1-193">One thing the script does is to export and download the cluster certificate and save it in the current user’s Documents folder on the client computer.</span></span> <span data-ttu-id="318c1-194">A parancsfájl egy a következőhöz hasonló üzenetet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="318c1-194">The script generates a message similar to the following.</span></span> <span data-ttu-id="318c1-195">Egy következő lépésben importálja a tanúsítványt a megfelelő tanúsítványtárolójában.</span><span class="sxs-lookup"><span data-stu-id="318c1-195">In a following step, you import the certificate in the appropriate certificate store.</span></span>    

    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a><span data-ttu-id="318c1-196">2. lépés</span><span class="sxs-lookup"><span data-stu-id="318c1-196">Step 2.</span></span> <span data-ttu-id="318c1-197">Excel-munkafüzetek kiszervezése és a felhasználó által megadott függvények futtatása egy helyszíni ügyfélről</span><span class="sxs-lookup"><span data-stu-id="318c1-197">Offload Excel workbooks and run UDFs from an on-premises client</span></span>
### <a name="excel-activation"></a><span data-ttu-id="318c1-198">Excel-aktiválás</span><span class="sxs-lookup"><span data-stu-id="318c1-198">Excel activation</span></span>
<span data-ttu-id="318c1-199">A termelési számítási feladatokhoz a ComputeNodeWithExcel Virtuálisgép-lemezkép használatakor meg kell adnia egy érvényes Microsoft Office licenckulcs aktiválásához Excel a számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="318c1-199">When using the ComputeNodeWithExcel VM image for production workloads, you need to provide a valid Microsoft Office license key to activate Excel on the compute nodes.</span></span> <span data-ttu-id="318c1-200">Ellenkező esetben Excel próbaverzióját 30 nap múlva lejár, és a COMException (0x800AC472) Excel-munkafüzetek futtatása meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="318c1-200">Otherwise, the evaluation version of Excel expires after 30 days, and running Excel workbooks will fail with the COMException (0x800AC472).</span></span> 

<span data-ttu-id="318c1-201">Az értékelés időpontjához további 30 napra is állíthatnak alaphelyzetbe Excel: Jelentkezzen be az átjárócsomópont és clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` összes Excel a számítási csomópontok HPC Cluster Manager keresztül.</span><span class="sxs-lookup"><span data-stu-id="318c1-201">You can rearm Excel for another 30 days of evaluation time: Log on to the head node and clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` on all Excel compute nodes via HPC Cluster Manager.</span></span> <span data-ttu-id="318c1-202">Legfeljebb kétszer is újra.</span><span class="sxs-lookup"><span data-stu-id="318c1-202">You can rearm a maximum of two times.</span></span> <span data-ttu-id="318c1-203">Ezt követően meg kell adnia egy érvényes Office licenckulcs.</span><span class="sxs-lookup"><span data-stu-id="318c1-203">After that, you must provide a valid Office license key.</span></span>

<span data-ttu-id="318c1-204">Az Office Professional Plus 2013 a Virtuálisgép-lemezkép telepítve az általános mennyiségi licenc kulcsot (GVLK) mennyiségi kiadását.</span><span class="sxs-lookup"><span data-stu-id="318c1-204">The Office Professional Plus 2013 installed on the VM image is a volume edition with a Generic Volume License Key (GVLK).</span></span> <span data-ttu-id="318c1-205">Kulcskezelő szolgáltatás (KMS) keresztül is aktiválhatja vagy Active Directory-alapú aktiválás (AD-BA), vagy a többször használható aktiválási kulcs (MAK).</span><span class="sxs-lookup"><span data-stu-id="318c1-205">You can activate it via Key Management Service (KMS)/Active Directory-Based Activation (AD-BA) or Multiple Activation Key (MAK).</span></span> 

    * <span data-ttu-id="318c1-206">Használja a KMS/AD-BA, meglévő KMS-kiszolgáló használata, vagy állítson be egy új Microsoft Office 2013 mennyiségi licenc csomag használatával.</span><span class="sxs-lookup"><span data-stu-id="318c1-206">To use KMS/AD-BA, use an existing KMS server or set up a new one by using the Microsoft Office 2013 Volume License Pack.</span></span> <span data-ttu-id="318c1-207">(Ha kívánja, a kiszolgáló beállítása a head csomóponton.) Ezután aktiválja a KMS-állomás kulcsát az interneten vagy telefonon keresztül.</span><span class="sxs-lookup"><span data-stu-id="318c1-207">(If you want to, set up the server on the head node.) Then, activate the KMS host key via the Internet or telephone.</span></span> <span data-ttu-id="318c1-208">Majd clusrun `ospp.vbs` a KMS-kiszolgáló és a port és az összes Office aktiválása az Excel számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="318c1-208">Then clusrun `ospp.vbs` to set the KMS server and port and activate Office on all the Excel compute nodes.</span></span> 

    * <span data-ttu-id="318c1-209">MAK-ot, első clusrun használandó `ospp.vbs` adja meg a kulcs, és az összes majd aktiválja az Excel számítási csomópontok az interneten vagy telefonon keresztül.</span><span class="sxs-lookup"><span data-stu-id="318c1-209">To use MAK, first clusrun `ospp.vbs` to input the key and then activate all the Excel compute nodes via the Internet or telephone.</span></span> 

> [!NOTE]
> <span data-ttu-id="318c1-210">Ez a Virtuálisgép-lemezkép nem használható Office Professional Plus 2013 kereskedelmi termékkulcsokat.</span><span class="sxs-lookup"><span data-stu-id="318c1-210">Retail product keys for Office Professional Plus 2013 cannot be used with this VM image.</span></span> <span data-ttu-id="318c1-211">Ha van érvényes kulcsok és a telepítési adathordozó Office vagy az Excel kiadás kivételével az Office Professional Plus 2013 mennyiségi kiadását, használhatja őket helyette.</span><span class="sxs-lookup"><span data-stu-id="318c1-211">If you have valid keys and installation media for Office or Excel editions other than this Office Professional Plus 2013 volume edition, you can use them instead.</span></span> <span data-ttu-id="318c1-212">Először távolítsa el a mennyiségi kiadását, és telepítse a verziót, hogy rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="318c1-212">First uninstall this volume edition and install the edition that you have.</span></span> <span data-ttu-id="318c1-213">Az újratelepített Excel számítási csomópont virtuális gép testreszabott egy telepítéseihez léptékű képként rögzíthetők.</span><span class="sxs-lookup"><span data-stu-id="318c1-213">The reinstalled Excel compute node can be captured as a customized VM image to use in a deployment at scale.</span></span>
> 
> 

### <a name="offload-excel-workbooks"></a><span data-ttu-id="318c1-214">Excel-munkafüzetek kiszervezése</span><span class="sxs-lookup"><span data-stu-id="318c1-214">Offload Excel workbooks</span></span>
<span data-ttu-id="318c1-215">Kövesse az alábbi lépéseket, hogy az Azure-ban a HPC Pack fürtön fut egy Excel-munkafüzet-kiszervezés.</span><span class="sxs-lookup"><span data-stu-id="318c1-215">Follow these steps to offload an Excel workbook so that it runs on the HPC Pack cluster in Azure.</span></span> <span data-ttu-id="318c1-216">Ehhez az szükséges, az Excel 2010 vagy 2013 már telepítve van az ügyfélszámítógép kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="318c1-216">To do this, you must have Excel 2010 or 2013 already installed on the client computer.</span></span>

1. <span data-ttu-id="318c1-217">Használja az 1. lépésben lehetőségek HPC Pack-fürt üzembe helyezése a Excel számítási csomópont kép.</span><span class="sxs-lookup"><span data-stu-id="318c1-217">Use one of the options in Step 1 to deploy an HPC Pack cluster with the Excel compute node image.</span></span> <span data-ttu-id="318c1-218">Szerezze be a fürt tanúsítványfájlt (.cer) és a fürt felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="318c1-218">Obtain the cluster certificate file (.cer) and cluster username and password.</span></span>
2. <span data-ttu-id="318c1-219">Az ügyfélszámítógépen a fürt tanúsítványt a Cert: \CurrentUser\Root importálni.</span><span class="sxs-lookup"><span data-stu-id="318c1-219">On the client computer, import the cluster certificate under Cert:\CurrentUser\Root.</span></span>
3. <span data-ttu-id="318c1-220">Győződjön meg arról, hogy telepítve van a Excel.</span><span class="sxs-lookup"><span data-stu-id="318c1-220">Make sure Excel is installed.</span></span> <span data-ttu-id="318c1-221">Hozzon létre egy Excel.exe.config fájlt az ügyfélszámítógépen Excel.exe megegyező mappában található a következő tartalommal.</span><span class="sxs-lookup"><span data-stu-id="318c1-221">Create an Excel.exe.config file with the following contents in the same folder as Excel.exe on the client computer.</span></span> <span data-ttu-id="318c1-222">Ez a lépés biztosítja, hogy a HPC Pack 2012 R2 Excel COM beépülő modul sikeresen betöltődik.</span><span class="sxs-lookup"><span data-stu-id="318c1-222">This step ensures that the HPC Pack 2012 R2 Excel COM add-in loads successfully.</span></span>
   
    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
4. <span data-ttu-id="318c1-223">Állítsa be az ügyfél a HPC Pack fürthöz feladatok küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="318c1-223">Set up the client to submit jobs to the HPC Pack cluster.</span></span> <span data-ttu-id="318c1-224">Egy elem letölteni a teljes [HPC Pack 2012 R2 Update 3 telepítési](http://www.microsoft.com/download/details.aspx?id=49922) és a HPC Pack ügyfél telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="318c1-224">One option is to download the full [HPC Pack 2012 R2 Update 3 installation](http://www.microsoft.com/download/details.aspx?id=49922) and install the HPC Pack client.</span></span> <span data-ttu-id="318c1-225">Azt is megteheti, töltse le és telepítse a [HPC Pack 2012 R2 Update 3 ügyfél segédprogramok](https://www.microsoft.com/download/details.aspx?id=49923) és a megfelelő Visual C++ 2010 terjeszthető változatának a számítógép ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555) ).</span><span class="sxs-lookup"><span data-stu-id="318c1-225">Alternatively, download and install the [HPC Pack 2012 R2 Update 3 client utilities](https://www.microsoft.com/download/details.aspx?id=49923) and the appropriate Visual C++ 2010 redistributable for your computer ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).</span></span>
5. <span data-ttu-id="318c1-226">A jelen példában használjuk ConvertiblePricing_Complete.xlsb nevű minta Excel-munkafüzet.</span><span class="sxs-lookup"><span data-stu-id="318c1-226">In this example, we use a sample Excel workbook named ConvertiblePricing_Complete.xlsb.</span></span> <span data-ttu-id="318c1-227">Letöltheti a [Itt](https://www.microsoft.com/en-us/download/details.aspx?id=2939).</span><span class="sxs-lookup"><span data-stu-id="318c1-227">You can download it [here](https://www.microsoft.com/en-us/download/details.aspx?id=2939).</span></span>
6. <span data-ttu-id="318c1-228">A munkamappa D:\Excel\Run például az Excel-munkafüzet másolja.</span><span class="sxs-lookup"><span data-stu-id="318c1-228">Copy the Excel workbook to a working folder such as D:\Excel\Run.</span></span>
7. <span data-ttu-id="318c1-229">Nyissa meg az Excel-munkafüzet.</span><span class="sxs-lookup"><span data-stu-id="318c1-229">Open the Excel workbook.</span></span> <span data-ttu-id="318c1-230">A a **Develop** menüszalag, kattintson a **COM-bővítmények** és ellenőrizze, hogy a HPC Pack Excel COM-bővítmény sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="318c1-230">On the **Develop** ribbon, click **COM Add-Ins** and confirm that the HPC Pack Excel COM add-in is loaded successfully.</span></span>
   
   ![Excel-bővítmény HPC Pack][addin]
8. <span data-ttu-id="318c1-232">A VBA makró Excel HPCControlMacros szerkesztése a megjegyzésként sorok módosításával, ahogy az az alábbi parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="318c1-232">Edit the VBA macro HPCControlMacros in Excel by changing the commented lines as shown in the following script.</span></span> <span data-ttu-id="318c1-233">Helyettesítse be a környezetének megfelelő értékeket.</span><span class="sxs-lookup"><span data-stu-id="318c1-233">Substitute appropriate values for your environment.</span></span>
   
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
9. <span data-ttu-id="318c1-235">Másolja az Excel-munkafüzet egy feltöltési címtár, például D:\Excel\Upload.</span><span class="sxs-lookup"><span data-stu-id="318c1-235">Copy the Excel workbook to an upload directory such as D:\Excel\Upload.</span></span> <span data-ttu-id="318c1-236">Ez a könyvtár a HPC_DependsFiles állandó a VBA makróban van megadva.</span><span class="sxs-lookup"><span data-stu-id="318c1-236">This directory is specified in the HPC_DependsFiles constant in the VBA macro.</span></span>
10. <span data-ttu-id="318c1-237">A munkafüzet futtatásához a fürtön, az Azure-ban kattintson a **fürt** gomb a munkalapon.</span><span class="sxs-lookup"><span data-stu-id="318c1-237">To run the workbook on the cluster in Azure, click the **Cluster** button on the worksheet.</span></span>

### <a name="run-excel-udfs"></a><span data-ttu-id="318c1-238">Excel univerzális Lemezformátumokat futtatása</span><span class="sxs-lookup"><span data-stu-id="318c1-238">Run Excel UDFs</span></span>
<span data-ttu-id="318c1-239">Excel univerzális Lemezformátumokat futtatásához kövesse az előző lépések 1 – 3 állíthatja be az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="318c1-239">To run Excel UDFs, follow the preceding steps 1 – 3 to set up the client computer.</span></span> <span data-ttu-id="318c1-240">Excel univerzális Lemezformátumokat nincs szükség van a számítási csomópontok telepítve Excel-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="318c1-240">For Excel UDFs, you don't need to have the Excel application installed on compute nodes.</span></span> <span data-ttu-id="318c1-241">Ezért amikor a fürt létrehozása számítási csomópontokat, megadhatja, normál számítási csomópont kép helyett a számítási csomópont rendszerképének Excel.</span><span class="sxs-lookup"><span data-stu-id="318c1-241">So, when creating your cluster compute nodes, you could choose a normal compute node image instead of the compute node image with Excel.</span></span>

> [!NOTE]
> <span data-ttu-id="318c1-242">Létrejön egy 34 karakteres korlátot, az Excel 2010 és 2013 fürt összekötő párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="318c1-242">There is a 34 character limit in the Excel 2010 and 2013 cluster connector dialog box.</span></span> <span data-ttu-id="318c1-243">Ez a párbeszédpanel segítségével adja meg a fürtöt, amely a felhasználó által megadott függvények futtatja.</span><span class="sxs-lookup"><span data-stu-id="318c1-243">You use this dialog box to specify the cluster that runs the UDFs.</span></span> <span data-ttu-id="318c1-244">Ha a teljes fürt neve hosszabb (például hpcexcelhn01.southeastasia.cloudapp.azure.com), nem fér el a párbeszédpanel bezárásához.</span><span class="sxs-lookup"><span data-stu-id="318c1-244">If the full cluster name is longer (for example, hpcexcelhn01.southeastasia.cloudapp.azure.com), it does not fit in the dialog box.</span></span> <span data-ttu-id="318c1-245">A megoldás is, hogy a gépre kiterjedő változó például *CCP_IAASHN* a hosszú fürtnév értékét.</span><span class="sxs-lookup"><span data-stu-id="318c1-245">The workaround is to set a machine-wide variable such as *CCP_IAASHN* with the value of the long cluster name.</span></span> <span data-ttu-id="318c1-246">Ezután írja be a *CCP_IAASHN %* neveként a fürt átjárócsomópontjához párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="318c1-246">Then, enter *%CCP_IAASHN%* in the dialog box as the cluster head node name.</span></span> 
> 
> 

<span data-ttu-id="318c1-247">A fürt sikeres telepítése után folytassa a következő lépéseket egy beépített minta futtatásához Excel UDF.</span><span class="sxs-lookup"><span data-stu-id="318c1-247">After the cluster is successfully deployed, continue with the following steps to run a sample built-in Excel UDF.</span></span> <span data-ttu-id="318c1-248">A testre szabott Excel univerzális Lemezformátumokat tapasztalja [erőforrások](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) a XLL-EK építsenek, és telepítheti őket az IaaS-fürtön.</span><span class="sxs-lookup"><span data-stu-id="318c1-248">For customized Excel UDFs, see these [resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) to build the XLLs and deploy them on the IaaS cluster.</span></span>

1. <span data-ttu-id="318c1-249">Nyisson meg egy új Excel-munkafüzet.</span><span class="sxs-lookup"><span data-stu-id="318c1-249">Open a new Excel workbook.</span></span> <span data-ttu-id="318c1-250">Az a **Develop** menüszalag, kattintson a **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="318c1-250">On the **Develop** ribbon, click **Add-Ins**.</span></span> <span data-ttu-id="318c1-251">Majd kattintson a párbeszédpanelen **Tallózás**, keresse meg a %CCP_HOME%Bin\XLL32 mappát, és válassza ki a minta ClusterUDF32.xll.</span><span class="sxs-lookup"><span data-stu-id="318c1-251">Then, in the dialog box, click **Browse**, navigate to the %CCP_HOME%Bin\XLL32 folder, and select the sample ClusterUDF32.xll.</span></span> <span data-ttu-id="318c1-252">Ha a ClusterUDF32 nem létezik az ügyfélszámítógépen, másolja az átjárócsomópont %CCP_HOME%Bin\XLL32 mappájából.</span><span class="sxs-lookup"><span data-stu-id="318c1-252">If the ClusterUDF32 doesn't exist on the client machine, copy it from the %CCP_HOME%Bin\XLL32 folder on the head node.</span></span>
   
   ![Válassza ki az UDF-ben][udf]
2. <span data-ttu-id="318c1-254">Kattintson a **fájl** > **beállítások** > **speciális**.</span><span class="sxs-lookup"><span data-stu-id="318c1-254">Click **File** > **Options** > **Advanced**.</span></span> <span data-ttu-id="318c1-255">A **képletek**, ellenőrizze **engedélyezése a felhasználó által definiált XLL függvények futtatásához a számítási fürt**.</span><span class="sxs-lookup"><span data-stu-id="318c1-255">Under **Formulas**, check **Allow user-defined XLL functions to run a compute cluster**.</span></span> <span data-ttu-id="318c1-256">Kattintson a **beállítások** és adja meg a teljes fürt nevét a **fürt átjárócsomópontjához neve**.</span><span class="sxs-lookup"><span data-stu-id="318c1-256">Then click **Options** and enter the full cluster name in **Cluster head node name**.</span></span> <span data-ttu-id="318c1-257">(Részben ismertetett beállításértékeket korábban a beviteli mezőbe korlátozódik 34 karakter hosszúságú lehet, ezért előfordulhat, hogy nem felelnek meg egy hosszú neve.</span><span class="sxs-lookup"><span data-stu-id="318c1-257">(As noted previously this input box is limited to 34 characters, so a long cluster name may not fit.</span></span> <span data-ttu-id="318c1-258">Használhatja a gépre kiterjedő változó nevét egy hosszú fürtnév.)</span><span class="sxs-lookup"><span data-stu-id="318c1-258">You may use a machine-wide variable here for a long cluster name.)</span></span>
   
   ![Az UDF konfigurálása][options]
3. <span data-ttu-id="318c1-260">Az UDF számítási futtatása a fürtön, kattintson a cellára, amelynek értéke =XllGetComputerNameC(), és nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="318c1-260">To run the UDF calculation on the cluster, click the cell with value =XllGetComputerNameC() and press Enter.</span></span> <span data-ttu-id="318c1-261">A függvény egyszerűen lekéri a számítási csomópont, amelyen fut az UDF nevét.</span><span class="sxs-lookup"><span data-stu-id="318c1-261">The function simply retrieves the name of the compute node on which the UDF runs.</span></span> <span data-ttu-id="318c1-262">Az első alkalommal történő futtatásakor a hitelesítő adatok párbeszédpanel megadását kéri a felhasználónevet és jelszót csatlakozni az IaaS-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="318c1-262">For the first run, a credentials dialog box prompts for the username and password to connect to the IaaS cluster.</span></span>
   
   ![Futtassa az UDF-ben][run]
   
   <span data-ttu-id="318c1-264">Ha sok cellát kiszámításához, nyomja le az Alt-Shift-Ctrl + F9 a számítás futniuk valamennyi cellájában.</span><span class="sxs-lookup"><span data-stu-id="318c1-264">When there are many cells to calculate, press Alt-Shift-Ctrl + F9 to run the calculation on all cells.</span></span>

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a><span data-ttu-id="318c1-265">3. lépés</span><span class="sxs-lookup"><span data-stu-id="318c1-265">Step 3.</span></span> <span data-ttu-id="318c1-266">A SOA munkaterhelések futtatásához egy helyszíni ügyfélről</span><span class="sxs-lookup"><span data-stu-id="318c1-266">Run a SOA workload from an on-premises client</span></span>
<span data-ttu-id="318c1-267">Általános SOA-alkalmazások futtatása a HPC Pack IaaS fürtön, először a módszerek valamelyikével 1. lépésben a fürt telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="318c1-267">To run general SOA applications on the HPC Pack IaaS cluster, first use one of the methods in Step 1 to deploy the cluster.</span></span> <span data-ttu-id="318c1-268">Adja meg, általános számítási csomópont kép ebben az esetben, mert az Excel nem szükséges a számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="318c1-268">Specify a generic compute node image in this case, because you do not need Excel on the compute nodes.</span></span> <span data-ttu-id="318c1-269">Ezután kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="318c1-269">Then follow these steps.</span></span>

1. <span data-ttu-id="318c1-270">A fürt tanúsítvány beolvasása, után importálja a Cert: \CurrentUser\Root az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="318c1-270">After retrieving the cluster certificate, import it on the client computer under Cert:\CurrentUser\Root.</span></span>
2. <span data-ttu-id="318c1-271">Telepítse a [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) és [HPC Pack 2012 R2 Update 3 ügyfél segédprogramok](https://www.microsoft.com/download/details.aspx?id=49923).</span><span class="sxs-lookup"><span data-stu-id="318c1-271">Install the [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) and [HPC Pack 2012 R2 Update 3 client utilities](https://www.microsoft.com/download/details.aspx?id=49923).</span></span> <span data-ttu-id="318c1-272">Ezek az eszközök lehetővé teszik fejlesztéséhez és SOA ügyfélalkalmazások futtatását.</span><span class="sxs-lookup"><span data-stu-id="318c1-272">These tools enable you to develop and run SOA client applications.</span></span>
3. <span data-ttu-id="318c1-273">Töltse le a HelloWorldR2 [példakód](https://www.microsoft.com/download/details.aspx?id=41633).</span><span class="sxs-lookup"><span data-stu-id="318c1-273">Download the HelloWorldR2 [sample code](https://www.microsoft.com/download/details.aspx?id=41633).</span></span> <span data-ttu-id="318c1-274">Nyissa meg a HelloWorldR2.sln a Visual Studio 2010 vagy a 2012.</span><span class="sxs-lookup"><span data-stu-id="318c1-274">Open the HelloWorldR2.sln in Visual Studio 2010 or 2012.</span></span> <span data-ttu-id="318c1-275">(Ez a minta nincs jelenleg kompatibilis a Visual Studio legújabb verziói.)</span><span class="sxs-lookup"><span data-stu-id="318c1-275">(This sample is not currently compatible with more recent versions of Visual Studio.)</span></span>
4. <span data-ttu-id="318c1-276">Először hozza létre a EchoService projekt.</span><span class="sxs-lookup"><span data-stu-id="318c1-276">Build the EchoService project first.</span></span> <span data-ttu-id="318c1-277">Ugyanúgy telepít a helyi fürthöz, majd telepítse a szolgáltatást az IaaS-fürt.</span><span class="sxs-lookup"><span data-stu-id="318c1-277">Then, deploy the service to the IaaS cluster in the same way you deploy to an on-premises cluster.</span></span> <span data-ttu-id="318c1-278">Részletes útmutató: a HelloWordR2 Readme.doc.</span><span class="sxs-lookup"><span data-stu-id="318c1-278">For detailed steps, see the Readme.doc in HelloWordR2.</span></span> <span data-ttu-id="318c1-279">Módosítsa, majd létre a HellWorldR2 és más projektek a következő szakaszban leírtak a SOA ügyfél Azure IaaS fürtökön futó alkalmazások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="318c1-279">Modify and build the HellWorldR2 and other projects as described in the following section to generate the SOA client applications that run on an Azure IaaS cluster.</span></span>

### <a name="use-http-binding-with-azure-storage-queue"></a><span data-ttu-id="318c1-280">Http-kötés használata az Azure storage üzenetsorába</span><span class="sxs-lookup"><span data-stu-id="318c1-280">Use Http binding with Azure storage queue</span></span>
<span data-ttu-id="318c1-281">Egy Azure storage üzenetsorába használandó Http-kötés, módosításokat néhány példakód.</span><span class="sxs-lookup"><span data-stu-id="318c1-281">To use Http binding with an Azure storage queue, make a few changes to the sample code.</span></span>

* <span data-ttu-id="318c1-282">Frissítse a fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="318c1-282">Update the cluster name.</span></span>
  
    ```
  // Before
  const string headnode = "[headnode]";
  // After e.g.
  const string headnode = "hpc01.eastus.cloudapp.azure.com";
  or
  const string headnode = "hpc01.cloudapp.net";
  ```
* <span data-ttu-id="318c1-283">Szükség esetén használja az alapértelmezett TransportScheme SessionStartInfo, vagy explicit módon állítsa be azt a Http.</span><span class="sxs-lookup"><span data-stu-id="318c1-283">Optionally, use the default TransportScheme in SessionStartInfo or explicitly set it to Http.</span></span>

```
    info.TransportScheme = TransportScheme.Http;
```

* <span data-ttu-id="318c1-284">A BrokerClient alapértelmezett kötés használja.</span><span class="sxs-lookup"><span data-stu-id="318c1-284">Use default binding for the BrokerClient.</span></span>
  
    ```
  // Before
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
  // After
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
  ```
  
    <span data-ttu-id="318c1-285">Vagy állítsa be explicit módon használja a basicHttpBinding.</span><span class="sxs-lookup"><span data-stu-id="318c1-285">Or set explicitly using the basicHttpBinding.</span></span>
  
    ```
  BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
  binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
  ```
* <span data-ttu-id="318c1-286">Beállíthatja a UseAzureQueue jelző SessionStartInfo igaz értékű.</span><span class="sxs-lookup"><span data-stu-id="318c1-286">Optionally, set the UseAzureQueue flag to true in SessionStartInfo.</span></span> <span data-ttu-id="318c1-287">Ha nincs megadva, akkor úgy lesz beállítva, amikor a fürt neve van Azure-tartomány utótag, és a TransportScheme Http alapértelmezés szerint true.</span><span class="sxs-lookup"><span data-stu-id="318c1-287">If not set, it will be set to true by default when the cluster name has Azure domain suffixes and the TransportScheme is Http.</span></span>
  
    ```
    info.UseAzureQueue = true;
  ```

### <a name="use-http-binding-without-azure-storage-queue"></a><span data-ttu-id="318c1-288">Http-kötés nélkül az Azure storage-várólista használata</span><span class="sxs-lookup"><span data-stu-id="318c1-288">Use Http binding without Azure storage queue</span></span>
<span data-ttu-id="318c1-289">Explicit módon használja a Http-kötés nélkül az Azure storage üzenetsorába UseAzureQueue jelzőt a SessionStartInfo a FALSE értékre kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="318c1-289">To use Http binding without an Azure storage queue, explicitly set the UseAzureQueue flag to false in the SessionStartInfo.</span></span>

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a><span data-ttu-id="318c1-290">NetTcp kötés használata</span><span class="sxs-lookup"><span data-stu-id="318c1-290">Use NetTcp binding</span></span>
<span data-ttu-id="318c1-291">NetTcp kötés használatához a konfigurációs hasonlít a csatlakozás a helyi fürthöz.</span><span class="sxs-lookup"><span data-stu-id="318c1-291">To use NetTcp binding, the configuration is similar to connecting to an on-premises cluster.</span></span> <span data-ttu-id="318c1-292">Nyissa meg az átjárócsomóponthoz VM néhány végpontja van szüksége.</span><span class="sxs-lookup"><span data-stu-id="318c1-292">You need to open a few endpoints on the head node VM.</span></span> <span data-ttu-id="318c1-293">Ha a HPC Pack IaaS telepítési parancsfájlt a fürt létrehozásához használt, például a végpontok Azure-portálon az alábbiak szerint állíthatja.</span><span class="sxs-lookup"><span data-stu-id="318c1-293">If you used the HPC Pack IaaS deployment script to create the cluster, for example, set the endpoints in the Azure portal as follows.</span></span>

1. <span data-ttu-id="318c1-294">Állítsa le a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="318c1-294">Stop the VM.</span></span>
2. <span data-ttu-id="318c1-295">Adja hozzá a TCP-portok 9090, 9087, 9091, a munkamenet 9094 Replikaszervező, munkavégző és adatszolgáltatások, illetve Replikaszervező</span><span class="sxs-lookup"><span data-stu-id="318c1-295">Add the TCP ports 9090, 9087, 9091, 9094 for the Session, Broker, Broker worker, and Data services, respectively</span></span>
   
    ![Végpontok konfigurálása][endpoint-new-portal]
3. <span data-ttu-id="318c1-297">Indítsa el a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="318c1-297">Start the VM.</span></span>

<span data-ttu-id="318c1-298">A SOA ügyfélalkalmazás nem kell módosítani a módosítása a központi nevét az IaaS-fürt teljes név kivételével.</span><span class="sxs-lookup"><span data-stu-id="318c1-298">The SOA client application requires no changes except altering the head name to the IaaS cluster full name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="318c1-299">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="318c1-299">Next steps</span></span>
* <span data-ttu-id="318c1-300">Lásd: [ezeket az erőforrásokat](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) HPC Pack Excel munkaterhelések futtatásával kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="318c1-300">See [these resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) for more information about running Excel workloads with HPC Pack.</span></span>
* <span data-ttu-id="318c1-301">Lásd: [SOA-szolgáltatások kezelése a Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) központi telepítésére és felügyeletére SOA szolgáltatások HPC Pack bővebben.</span><span class="sxs-lookup"><span data-stu-id="318c1-301">See [Managing SOA Services in Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) for more about deploying and managing SOA services with HPC Pack.</span></span>

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
