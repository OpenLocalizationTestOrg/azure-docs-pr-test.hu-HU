---
title: "Hozzon létre egy Azure virtuális gép a hálózat elérését gyorsítja fel |} Microsoft Docs"
description: "Útmutató: virtuális gép létrehozása a hálózat elérését gyorsítja fel."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 449425189a3b42dcb2c31316c1c8e38fac69d761
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-virtual-machine-with-accelerated-networking"></a><span data-ttu-id="0aa5b-103">A hálózat elérését gyorsítja fel a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="0aa5b-103">Create a virtual machine with Accelerated Networking</span></span>

<span data-ttu-id="0aa5b-104">Ebben az oktatóanyagban elsajátíthatja, hogyan hozzon létre egy Azure virtuális gép (VM) a hálózat elérését gyorsítja fel.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-104">In this tutorial, you learn how to create an Azure Virtual Machine (VM) with Accelerated Networking.</span></span> <span data-ttu-id="0aa5b-105">Gyorsított hálózatkezelés a Windows és a nyilvános előzetes verziójában az adott Linux terjesztésekről GA.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-105">Accelerated Networking is GA for Windows and in a Public Preview for specific Linux distributions.</span></span> <span data-ttu-id="0aa5b-106">Gyorsított hálózatkezelés lehetővé teszi, hogy az egygyökerű i/o-virtualizálás (SR-IOV) egy virtuális géphez, a hálózati teljesítmény nagy mértékben javítva.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-106">Accelerated networking enables single root I/O virtualization (SR-IOV) to a VM, greatly improving its networking performance.</span></span> <span data-ttu-id="0aa5b-107">A nagy teljesítményű elérési út nincs hatással a gazdagép a datapath, így csökkentve a késést, a jitter és a CPU felhasználását, a legnagyobb igénybevételt jelentő munkaterheléseit támogatott Virtuálisgép-típusokon való használatra.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-107">This high-performance path bypasses the host from the datapath reducing latency, jitter, and CPU utilization, for use with the most demanding network workloads on supported VM types.</span></span> <span data-ttu-id="0aa5b-108">Az alábbi képen látható két virtuális gép (VM) közötti kommunikációt, és anélkül gyorsított hálózat:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-108">The following picture shows communication between two virtual machines (VM) with and without accelerated networking:</span></span>

![Összehasonlítása](./media/virtual-network-create-vm-accelerated-networking/image1.png)

<span data-ttu-id="0aa5b-110">Gyorsított hálózatkezelés nélkül, az összes hálózati forgalom mindkét a virtuális Gépet a virtuális kapcsoló és a gazdagép be kell járnia.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-110">Without accelerated networking, all networking traffic in and out of the VM must traverse the host and the virtual switch.</span></span> <span data-ttu-id="0aa5b-111">A virtuális kapcsoló betartatja az összes, például a hálózati biztonsági csoportok, hozzáférés-vezérlési listák, elkülönítési és egyéb hálózati szempontból virtualizált szolgáltatások a hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-111">The virtual switch provides all policy enforcement, such as network security groups, access control lists, isolation, and other network virtualized services to network traffic.</span></span> <span data-ttu-id="0aa5b-112">Virtuális kapcsolók kapcsolatos további tudnivalókért olvassa el a [Hyper-V hálózatvirtualizálás és a virtuális kapcsoló](https://technet.microsoft.com/library/jj945275.aspx) cikk.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-112">To learn more about virtual switches, read the [Hyper-V network virtualization and virtual switch](https://technet.microsoft.com/library/jj945275.aspx) article.</span></span>

<span data-ttu-id="0aa5b-113">Gyorsított hálózattal, a hálózati forgalom érkezik a virtuális hálózati adapter (NIC), és a virtuális gép ezután kerül.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-113">With accelerated networking, network traffic arrives at the VM's network interface (NIC), and is then forwarded to the VM.</span></span> <span data-ttu-id="0aa5b-114">Minden hálózati házirendek, a virtuális kapcsolót alkalmazó gyorsított hálózatkezelés nélkül kiszervezett és hardver alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-114">All network policies that the virtual switch applies without accelerated networking are offloaded and applied in hardware.</span></span> <span data-ttu-id="0aa5b-115">A hardver-házirend alkalmazását lehetővé teszi, hogy a hálózati adapter a hálózati forgalom erre a virtuális Gépet, a gazdagép és a virtuális kapcsolót, akkor alkalmazza, a fogadó minden házirend fenntartásával kihagyásával.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-115">Applying policy in hardware enables the NIC to forward network traffic directly to the VM, bypassing the host and the virtual switch, while maintaining all the policy it applied in the host.</span></span>

<span data-ttu-id="0aa5b-116">A gyorsított hálózat előnyeit, hogy engedélyezve van a virtuális gép csak vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-116">The benefits of accelerated networking only apply to the VM that it is enabled on.</span></span> <span data-ttu-id="0aa5b-117">A legjobb eredmény elérése érdekében az ideális legalább két virtuális gépeken, az azonos Azure Virtual Network (VNet) csatlakozik a funkció engedélyezése érdekében.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-117">For the best results, it is ideal to enable this feature on at least two VMs connected to the same Azure Virtual Network (VNet).</span></span> <span data-ttu-id="0aa5b-118">Ha kommunikál a Vnetek vagy az összekötő a helyszíni, ez a funkció csak minimális befolyással van általános késleltetésű.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-118">When communicating across VNets or connecting on-premises, this feature has minimal impact to overall latency.</span></span>

> [!WARNING]
> <span data-ttu-id="0aa5b-119">A Linux Public Preview nem rendelkezhet azonos szintű rendelkezésre állást és megbízhatóságot, szolgáltatások, amelyek általában a rendelkezésre állási kiadási.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-119">This Linux Public Preview may not have the same level of availability and reliability as features that are in general availability release.</span></span> <span data-ttu-id="0aa5b-120">A szolgáltatás nem támogatott, van, korlátozott képességeket, és előfordulhat, hogy nem érhető el az összes Azure helyét.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-120">The feature is not supported, may have constrained capabilities, and may not be available in all Azure locations.</span></span> <span data-ttu-id="0aa5b-121">A rendelkezésre állás és a szolgáltatás állapotát a legfrissebb értesítések tekintse meg az Azure Virtual Network frissítések lap.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-121">For the most up-to-date notifications on availability and status of this feature, check the Azure Virtual Network updates page.</span></span>

## <a name="benefits"></a><span data-ttu-id="0aa5b-122">Előnyök</span><span class="sxs-lookup"><span data-stu-id="0aa5b-122">Benefits</span></span>
* <span data-ttu-id="0aa5b-123">**Késés csökkentése / magasabb csomag / másodperc (pps):** a virtuális kapcsoló eltávolítása a datapath eltávolítja a csomagok kiosztására szánt idejét a gazdagép a házirend-feldolgozás, a virtuális Gépen belül feldolgozható csomagok száma.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-123">**Lower Latency / Higher packets per second (pps):** Removing the virtual switch from the datapath removes the time packets spend in the host for policy processing and increases the number of packets that can be processed inside the VM.</span></span>
* <span data-ttu-id="0aa5b-124">**Alacsonyabb jitter:** virtuális kapcsoló feldolgozása függ a házirendet, szükség van, és a CPU-t, amely a feldolgozási terhelését.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-124">**Reduced jitter:** Virtual switch processing depends on the amount of policy that needs to be applied and the workload of the CPU that is doing the processing.</span></span> <span data-ttu-id="0aa5b-125">A házirendek betartatását, hogy a hardver kiszervezésével megszüntetéséhez, hogy a variancia közvetlenül a virtuális gép eltávolítása a gazdagép VM kommunikáció és az összes szoftver megszakítások és a környezet kapcsolók kézbesíti a csomagokat.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-125">Offloading the policy enforcement to the hardware removes that variability by delivering packets directly to the VM, removing the host to VM communication and all software interrupts and context switches.</span></span>
* <span data-ttu-id="0aa5b-126">**Csökkent a CPU-felhasználás:** kevesebb hálózati forgalom feldolgozása a CPU-használatot a virtuális kapcsoló a gazdagép megkerülésével vezet.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-126">**Decreased CPU utilization:** Bypassing the virtual switch in the host leads to less CPU utilization for processing network traffic.</span></span>

## <span data-ttu-id="0aa5b-127"><a name="Limitations"></a>Korlátozások</span><span class="sxs-lookup"><span data-stu-id="0aa5b-127"><a name="Limitations"></a>Limitations</span></span>
<span data-ttu-id="0aa5b-128">A következő korlátozások vonatkoznak az e funkció használata esetén:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-128">The following limitations exist when using this capability:</span></span>

* <span data-ttu-id="0aa5b-129">**A hálózati illesztő létrehozása:** gyorsított hálózat csak akkor engedélyezhető, az új hálózati</span><span class="sxs-lookup"><span data-stu-id="0aa5b-129">**Network interface creation:** Accelerated networking can only be enabled for a new NIC.</span></span> <span data-ttu-id="0aa5b-130">Nem engedélyezhető az egy meglévő hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-130">It cannot be enabled for an existing NIC.</span></span>
* <span data-ttu-id="0aa5b-131">**Virtuális gép létrehozása:** A hálózati adapter engedélyezve gyorsított hálózattal csak akkor csatolható a virtuális géphez a virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-131">**VM creation:** A NIC with accelerated networking enabled can only be attached to a VM when the VM is created.</span></span> <span data-ttu-id="0aa5b-132">A hálózati adapter nem lehet csatolni, egy meglévő virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-132">The NIC cannot be attached to an existing VM.</span></span>
* <span data-ttu-id="0aa5b-133">**Régiók:** Windows-alapú virtuális gépek gyorsított hálózattal érhető el az Azure-régióban.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-133">**Regions:** Windows VMs with accelerated networking are offered in most Azure regions.</span></span> <span data-ttu-id="0aa5b-134">Linux virtuális gépek gyorsított hálózattal több régióba érhető el.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-134">Linux VMs with accelerated networking are offered in multiple regions.</span></span> <span data-ttu-id="0aa5b-135">Ez a funkció érhető el a régiók növekszik.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-135">The regions this capability is available in is expanding.</span></span> <span data-ttu-id="0aa5b-136">Tekintse meg az Azure virtuális hálózat frissítések blog alatt a legfrissebb információkat.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-136">Please see the Azure Virtual Networking Updates blog below for the latest information.</span></span>   
* <span data-ttu-id="0aa5b-137">**Támogatott operációs rendszerekről:** Windows: Microsoft Windows Server 2012 R2 Datacenter és a Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-137">**Supported operating systems:** Windows: Microsoft Windows Server 2012 R2 Datacenter and Windows Server 2016.</span></span> <span data-ttu-id="0aa5b-138">Linux: Ubuntu Server 16.04 kernel 4.4.0-77 vagy annál újabb LTS, SLES 12 SP2, RHEL 7.3 és CentOS 7.3 (közzétett "Engedélyezetlen Wave szoftver").</span><span class="sxs-lookup"><span data-stu-id="0aa5b-138">Linux: Ubuntu Server 16.04 LTS with kernel 4.4.0-77 or higher, SLES 12 SP2, RHEL 7.3 and CentOS 7.3 (Published by “Rogue Wave Software”).</span></span>
* <span data-ttu-id="0aa5b-139">**Virtuálisgép-méret:** általános célú és nyolc vagy több maggal rendelkező számítási optimalizált példány mérete.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-139">**VM Size:** General purpose and compute-optimized instance sizes with eight or more cores.</span></span> <span data-ttu-id="0aa5b-140">További információkért lásd: a [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) és [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gép méretének cikkeket.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-140">For more information, see the [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) and [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM sizes articles.</span></span> <span data-ttu-id="0aa5b-141">A Virtuálisgép-példány támogatott méretek készletét a jövőben bont ki.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-141">The set of supported VM instance sizes will expand in the future.</span></span>
* <span data-ttu-id="0aa5b-142">**Telepítés csak Azure Resource Managerrel (ARM) keresztül:** az elérését gyorsítja fel hálózati nincs elérhető a telepítéshez a címterület-kezelés/RDFE keresztül.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-142">**Deployment through Azure Resource Manager (ARM) only:** Accelerated Networking is not available for deployment through ASM/RDFE.</span></span>

<span data-ttu-id="0aa5b-143">Ezek a korlátozások módosításai keresztül történik bejelentés a [Azure virtuális hálózat frissítése](https://azure.microsoft.com/updates/accelerated-networking-in-preview) lap.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-143">Changes to these limitations are announced through the [Azure Virtual Networking updates](https://azure.microsoft.com/updates/accelerated-networking-in-preview) page.</span></span>

## <a name="create-a-windows-vm"></a><span data-ttu-id="0aa5b-144">Windows rendszerű virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="0aa5b-144">Create a Windows VM</span></span>
<span data-ttu-id="0aa5b-145">Használhatja az Azure-portálon vagy az Azure [PowerShell](#windows-powershell) a virtuális gép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-145">You can use the Azure portal or Azure [PowerShell](#windows-powershell) to create the VM.</span></span>

### <span data-ttu-id="0aa5b-146"><a name="windows-portal"></a>Portál</span><span class="sxs-lookup"><span data-stu-id="0aa5b-146"><a name="windows-portal"></a>Portal</span></span>

1. <span data-ttu-id="0aa5b-147">Egy webböngészőben nyissa meg az Azure [portal](https://portal.azure.com) és jelentkezzen be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="0aa5b-147">From an Internet browser, open the Azure [portal](https://portal.azure.com) and sign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="0aa5b-148">Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="0aa5b-148">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="0aa5b-149">Kattintson a portál **+ új** > **számítási** > **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-149">In the portal, click **+ New** > **Compute** > **Windows Server 2016 Datacenter**.</span></span> 
3. <span data-ttu-id="0aa5b-150">Az a **Windows Server 2016 Datacenter** panel, amely akkor jelenik meg, hagyja *erőforrás-kezelő* a kijelölt **telepítési modell kiválasztása**, és kattintson a **létrehozása** .</span><span class="sxs-lookup"><span data-stu-id="0aa5b-150">In the **Windows Server 2016 Datacenter** blade that appears, leave *Resource Manager* selected under **Select a deployment model**, and click **Create**.</span></span>
4. <span data-ttu-id="0aa5b-151">Az a **alapjai** panel, amelyen megjelenik, adja meg a következő értékeket, hagyja a további alapértelmezett beállításokat, vagy válassza ki vagy adja meg a saját értékeit, majd kattintson a **OK** gombra:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-151">In the **Basics** blade that appears, enter the following values, leave the remaining default options or select or enter your own values, and click the **OK** button:</span></span>

    |<span data-ttu-id="0aa5b-152">Beállítás</span><span class="sxs-lookup"><span data-stu-id="0aa5b-152">Setting</span></span>|<span data-ttu-id="0aa5b-153">Érték</span><span class="sxs-lookup"><span data-stu-id="0aa5b-153">Value</span></span>|
    |---|---|
    |<span data-ttu-id="0aa5b-154">Név</span><span class="sxs-lookup"><span data-stu-id="0aa5b-154">Name</span></span>|<span data-ttu-id="0aa5b-155">MyVm</span><span class="sxs-lookup"><span data-stu-id="0aa5b-155">MyVm</span></span>|
    |<span data-ttu-id="0aa5b-156">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="0aa5b-156">Resource group</span></span>|<span data-ttu-id="0aa5b-157">Hagyja **hozzon létre új** kiválasztva, és írja be *MyResourceGroup*</span><span class="sxs-lookup"><span data-stu-id="0aa5b-157">Leave **Create new** selected and enter *MyResourceGroup*</span></span>|
    |<span data-ttu-id="0aa5b-158">Hely</span><span class="sxs-lookup"><span data-stu-id="0aa5b-158">Location</span></span>|<span data-ttu-id="0aa5b-159">USA nyugati régiója, 2.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-159">West US 2</span></span>|

    <span data-ttu-id="0aa5b-160">Ha most ismerkedik az Azure-ba, további információ [erőforráscsoportok](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [előfizetések](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), és [helyek](https://azure.microsoft.com/regions) (amely is nevezzük régiók).</span><span class="sxs-lookup"><span data-stu-id="0aa5b-160">If you're new to Azure, learn more about [Resource groups](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [subscriptions](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), and [locations](https://azure.microsoft.com/regions) (which are also referred to as regions).</span></span>
5. <span data-ttu-id="0aa5b-161">Az a **méret kiválasztása** panel, amelyen megjelenik, írja be *8* a a **minimális magszámra** mezőbe, majd kattintson az **összes**.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-161">In the **Choose a size** blade that appears, enter *8* in the **Minimum cores** box, then click **View all**.</span></span>
6. <span data-ttu-id="0aa5b-162">Kattintson a **DS4_V2 szabványos**, vagy bármely támogatott virtuális gép, majd kattintson a **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-162">Click **DS4_V2 Standard**, or any supported VM, then click the **Select** button.</span></span>
7. <span data-ttu-id="0aa5b-163">A a **beállítások** panelen megjelenő összes beállításokat hagyja-van, kivéve, kattintson **engedélyezve** alatt **elérését gyorsítja fel a hálózat**, majd kattintson a **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-163">In the **Settings** blade that appears, leave all settings as-is, except click **Enabled** under **Accelerated networking**, then click the **OK** button.</span></span> <span data-ttu-id="0aa5b-164">**Megjegyzés:** , ha előző lépésben kiválasztott értékek a virtuális gép mérete, az operációs rendszer vagy helyre, amely nem szerepel a [korlátozások](#Limitations) című szakaszát, **elérését gyorsítja fel a hálózat** nem látható.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-164">**Note:** If, in previous steps, you selected values for VM size, operating system, or location that aren't listed in the [Limitations](#Limitations) section of this article, **Accelerated networking** isn't visible.</span></span>
8. <span data-ttu-id="0aa5b-165">Az a **összegzés** panel, amelyen megjelenik, kattintson a **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-165">In the **Summary** blade that appears, click the **OK** button.</span></span> <span data-ttu-id="0aa5b-166">Azure elindítja a virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-166">Azure starts creating the VM.</span></span> <span data-ttu-id="0aa5b-167">Virtuális gép létrehozásának folyamata eltart néhány percig.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-167">VM creation takes a few minutes.</span></span>
9. <span data-ttu-id="0aa5b-168">A Windows a gyorsított hálózati illesztőprogram telepítéséhez hajtsa végre a lépéseit a [konfigurálása Windows](#configure-windows) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-168">To install the accelerated networking driver for Windows, complete the steps in the [Configure Windows](#configure-windows) section of this article.</span></span>

### <span data-ttu-id="0aa5b-169"><a name="windows-powershell"></a>PowerShell</span><span class="sxs-lookup"><span data-stu-id="0aa5b-169"><a name="windows-powershell"></a>PowerShell</span></span>
1. <span data-ttu-id="0aa5b-170">Az Azure PowerShell legújabb verziójának telepítéséhez [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-170">Install the latest version of the Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module.</span></span> <span data-ttu-id="0aa5b-171">Ha most ismerkedik az Azure PowerShell, olvassa el a [Azure PowerShell áttekintése](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-171">If you're new to Azure PowerShell, read the [Azure PowerShell overview](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) article.</span></span>
2. <span data-ttu-id="0aa5b-172">A PowerShell-munkamenet indításához kattintson a Start gombra, írja be **powershell**, majd kattintson a **PowerShell** a keresési eredmények.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-172">Start a PowerShell session by clicking the Windows Start button, typing **powershell**, then clicking **PowerShell** from the search results.</span></span>
3. <span data-ttu-id="0aa5b-173">A PowerShell-ablakban írja be a `login-azurermaccount` parancs futtatásával jelentkezzen be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="0aa5b-173">In your PowerShell window, enter the `login-azurermaccount` command to sign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="0aa5b-174">Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="0aa5b-174">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
4. <span data-ttu-id="0aa5b-175">A böngészőben másolja a következő:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-175">In your browser, copy the following script:</span></span>
    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Windows `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id 

    # Create the virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    #
    ```
5. <span data-ttu-id="0aa5b-176">A PowerShell-ablakban kattintson a jobb gombbal illessze be a parancsfájlt, és futtatnia kell elindítani.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-176">In your PowerShell window, right-click to paste the script and start executing it.</span></span> <span data-ttu-id="0aa5b-177">Felhasználónév és jelszó megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-177">You are prompted for a username and password.</span></span> <span data-ttu-id="0aa5b-178">Ezek a hitelesítő adatok használatát a bejelentkezéshez a virtuális géphez, amikor csatlakozik a következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-178">Use these credentials to log in to the VM when connecting to it in the next step.</span></span> <span data-ttu-id="0aa5b-179">Ha a parancsfájl futása sikertelen, és azt végrehajtása előtt megváltoztatta a parancsfájlban szereplő értékeket, ellenőrizze a virtuális gép méretét, operációs rendszer használt értékeket, és a helyen találhatók a [korlátozások](#Limitations) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-179">If the script fails, and you changed values in the script before executing it, confirm the values you used for VM size, operating system, and location are listed in the [Limitations](#Limitations) section of this article.</span></span>
6. <span data-ttu-id="0aa5b-180">A Windows a gyorsított hálózati illesztőprogram telepítéséhez hajtsa végre a lépéseit a [konfigurálása Windows](#configure-windows) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-180">To install the accelerated networking driver for Windows, complete the steps in the [Configure Windows](#configure-windows) section of this article.</span></span>

### <span data-ttu-id="0aa5b-181"><a name="configure-windows"></a>A Windows beállítása</span><span class="sxs-lookup"><span data-stu-id="0aa5b-181"><a name="configure-windows"></a>Configure Windows</span></span>
<span data-ttu-id="0aa5b-182">Miután létrehozta a virtuális Gépet az Azure-ban, telepítenie kell a Windows a gyorsított hálózati illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-182">Once you create the VM in Azure, you must install the accelerated networking driver for Windows.</span></span> <span data-ttu-id="0aa5b-183">Az alábbi lépések elvégzése előtt kell korábban létrehozott egy Windows virtuális gép gyorsított hálózat használatával a [portal](#windows-portal) vagy [PowerShell](#windows-powershell) ebben a cikkben ismertetett visszaállítási lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-183">Before completing the following steps, you must have created a Windows VM with accelerated networking using either the [portal](#windows-portal) or [PowerShell](#windows-powershell) steps in this article.</span></span> 

1. <span data-ttu-id="0aa5b-184">Egy webböngészőben nyissa meg az Azure [portal](https://portal.azure.com) és jelentkezzen be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="0aa5b-184">From an Internet browser, open the Azure [portal](https://portal.azure.com) and sign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="0aa5b-185">Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="0aa5b-185">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="0aa5b-186">A mezőbe a szöveget tartalmazó *keresési erőforrások* az Azure portál felső részén írja be a *MyVm*.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-186">In the box that contains the text *Search resources* at the top of the Azure portal, type *MyVm*.</span></span> <span data-ttu-id="0aa5b-187">Ha **MyVm** jelenik meg a keresési eredmények között kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-187">When **MyVm** appears in the search results, click it.</span></span>
3. <span data-ttu-id="0aa5b-188">Az a **MyVm** panel, amelyen megjelenik, kattintson a **Connect** a panel bal felső sarkában található gombra.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-188">In the **MyVm** blade that appears, click the **Connect** button in the top left corner of the blade.</span></span> <span data-ttu-id="0aa5b-189">**Megjegyzés:** Ha **létrehozása** alatt látható a **Connect** gomb, Azure még nem fejeződött be a virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-189">**Note:** If **Creating** is visible under the **Connect** button, Azure has not yet finished creating the VM.</span></span> <span data-ttu-id="0aa5b-190">Kattintson a **Connect** csak után többé nem láthatja **létrehozása** alatt a **Connect** gombra.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-190">Click **Connect** only after you no longer see **Creating** under the **Connect** button.</span></span>
4. <span data-ttu-id="0aa5b-191">Engedélyezze a böngészőben a letöltés a **MyVm.rdp** fájlt.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-191">Allow your browser to download the **MyVm.rdp** file.</span></span>  <span data-ttu-id="0aa5b-192">Miután letöltötte, kattintson a fájl megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-192">Once downloaded, click the file to open it.</span></span> 
5. <span data-ttu-id="0aa5b-193">Kattintson a **Connect** gombra a **távoli asztali kapcsolat** panelen, amely tájékoztat arról, hogy a távoli kapcsolat közzétevője nem azonosítható.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-193">Click the **Connect** button in the **Remote Desktop Connection** box that appears, notifying you that the publisher of the remote connection can't be identified.</span></span>
6. <span data-ttu-id="0aa5b-194">Az a **Windows biztonsági** meg, kattintson a **több lehetőséget**, majd kattintson a **használjon más fiókot**.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-194">In the **Windows Security** box that appears, click **More choices**, then click **Use a different account**.</span></span> <span data-ttu-id="0aa5b-195">Írja be a felhasználónevét és a 4. lépésben megadott jelszót, majd kattintson a **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-195">Enter the username and password you entered in step 4, then click the **OK** button.</span></span>
7. <span data-ttu-id="0aa5b-196">Kattintson a **Igen** gombra a távoli asztali kapcsolat mezőbe, amely tájékoztat arról, hogy a távoli számítógép identitása nem ellenőrizhető.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-196">Click the **Yes** button in the Remote Desktop Connection box that appears, notifying you that the identity of the remote computer cannot be verified.</span></span>
8. <span data-ttu-id="0aa5b-197">Kattintson a jobb gombbal a Start gombra, és kattintson a **Eszközkezelő**.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-197">Right-click the Windows Start button and click **Device Manager**.</span></span> <span data-ttu-id="0aa5b-198">Bontsa ki a **hálózati adapterek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-198">Expand the **Network adapters** node.</span></span> <span data-ttu-id="0aa5b-199">Ellenőrizze, hogy a **Mellanox ConnectX-3 virtuális függvény Ethernet-Adapter** az alábbi ábrán látható módon jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-199">Confirm that the **Mellanox ConnectX-3 Virtual Function Ethernet Adapter** appears, as shown in the following picture:</span></span>
   
    ![Eszközkezelő](./media/virtual-network-create-vm-accelerated-networking/image2.png)

9. <span data-ttu-id="0aa5b-201">Gyorsított hálózatkezelés engedélyezve van a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-201">Accelerated Networking is now enabled for your VM.</span></span>

## <a name="create-a-linux-vm"></a><span data-ttu-id="0aa5b-202">Linux rendszerű virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="0aa5b-202">Create a Linux VM</span></span>
<span data-ttu-id="0aa5b-203">Használhatja az Azure-portálon vagy az Azure [PowerShell](#linux-powershell) egy Ubuntu vagy SLES virtuális gép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-203">You can use the Azure portal or Azure [PowerShell](#linux-powershell) to create an Ubuntu or SLES VM.</span></span> <span data-ttu-id="0aa5b-204">Az RHEL és CentOS virtuális gépeket egy másik munkafolyamat van.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-204">For RHEL and CentOS VMs there is a different workflow.</span></span>  <span data-ttu-id="0aa5b-205">Ellenőrizze az alábbi utasításokat.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-205">Please see the instructions below.</span></span>

### <span data-ttu-id="0aa5b-206"><a name="linux-portal"></a>Portál</span><span class="sxs-lookup"><span data-stu-id="0aa5b-206"><a name="linux-portal"></a>Portal</span></span>
1. <span data-ttu-id="0aa5b-207">Linux preview; Ehhez hajtsa végre az 1-5 lépések a gyorsított hálózati regisztrálható a [hozzon létre egy Linux virtuális Gépet - PowerShell](#linux-powershell) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-207">Register for the accelerated networking for Linux preview by completing steps 1-5 of the [Create a Linux VM - PowerShell](#linux-powershell) section of this article.</span></span>  <span data-ttu-id="0aa5b-208">A portál előnézethez nem regisztrálható.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-208">You cannot register for the preview in the portal.</span></span>
2. <span data-ttu-id="0aa5b-209">Végezze el a lépéseket 1-8 a [hozzon létre egy Windows virtuális Gépet - portál](#windows-portal) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-209">Complete steps 1-8 in the [Create a Windows VM - portal](#windows-portal) section of this article.</span></span> <span data-ttu-id="0aa5b-210">A 2. lépésben, kattintson a **Ubuntu Server 16.04 LTS** helyett **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-210">In step 2, click **Ubuntu Server 16.04 LTS** instead of **Windows Server 2016 Datacenter**.</span></span> <span data-ttu-id="0aa5b-211">A jelen oktatóanyag esetében jelölje ki a jelszót, nem pedig egy SSH-kulcsot, abban az esetben, ha az üzemi környezetek is használhatja.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-211">For this tutorial, choose to use a password, rather than an SSH key, though for production deployments, you can use either.</span></span> <span data-ttu-id="0aa5b-212">Ha **elérését gyorsítja fel a hálózat** nem jelenik meg, amikor befejezte a 7. lépés a [hozzon létre egy Windows virtuális Gépet - portál](#windows-portal) szakasz ezen cikk valószínű, a következő okok valamelyike:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-212">If **Accelerated networking** does not appear when you complete step 7 of the [Create a Windows VM - portal](#windows-portal) section of this article, it's likely for one of the following reasons:</span></span>
    - <span data-ttu-id="0aa5b-213">Meg vannak még nincs regisztrálva az előzetes verziójára.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-213">You are not yet registered for the preview.</span></span> <span data-ttu-id="0aa5b-214">Győződjön meg arról, hogy a regisztrációs állapotát **regisztrált**, 4. lépésében leírtak szerint a [hozzon létre egy Linux virtuális Gépet - Powershell](#linux-powershell) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-214">Confirm that your registration state is **Registered**, as explained in step 4 of the [Create a Linux VM - Powershell](#linux-powershell) section of this article.</span></span> <span data-ttu-id="0aa5b-215">**Megjegyzés:** Amennyiben Ön részt vett a gyorsított hálózatkezelés a Windows virtuális gépek preview (már nem szükséges Windows hálózat virtuális gépek gyorsított használandó regisztrálása), akkor nem automatikusan regisztrálva vannak a Linux virtuális gépek hálózati gyorsított előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-215">**Note:** If you participated in the Accelerated networking for Windows VMs preview (it's no longer necessary to register to use Accelerated networking for Windows VMs), you are not automatically registered for the Accelerated networking for Linux VMs preview.</span></span> <span data-ttu-id="0aa5b-216">Regisztrálnia kell a gyorsított hálózati Linux virtuális gépek Preview részt.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-216">You must register for the Accelerated networking for Linux VMs preview to participate in it.</span></span>
    - <span data-ttu-id="0aa5b-217">Nem jelölt ki egy virtuális gép méretét, az operációs rendszer, vagy a hely szerepel a [korlátozások](#limitations) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-217">You have not selected a VM size, operating system, or location listed in the [Limitations](#limitations) section of this article.</span></span>
3. <span data-ttu-id="0aa5b-218">Gyorsított hálózati illesztőprogram telepítése Linux, lépéseinek végrehajtásához a [konfigurálása Linux](#configure-linux) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-218">To install the accelerated networking driver for Linux, complete the steps in the [Configure Linux](#configure-linux) section of this article.</span></span>

### <span data-ttu-id="0aa5b-219"><a name="linux-powershell"></a>PowerShell</span><span class="sxs-lookup"><span data-stu-id="0aa5b-219"><a name="linux-powershell"></a>PowerShell</span></span>

>[!WARNING]
><span data-ttu-id="0aa5b-220">Ha a Linux virtuális gépek létrehozása egy előfizetésben gyorsított hálózattal, és próbálja meg egy Windows virtuális gép létrehozása az ugyanahhoz az előfizetéshez gyorsított hálózattal, a Windows virtuális gépek létrehozása sikertelen lehet.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-220">If you create Linux VMs with accelerated networking in a subscription, and then attempt to create a Windows VM with accelerated networking in the same subscription, the Windows VM creation may fail.</span></span> <span data-ttu-id="0aa5b-221">Ez az előzetes ajánlott, hogy tesztelje a Linux és a Windows virtuális gépek külön előfizetésekhez gyorsított hálózattal.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-221">During this preview, it's recommended that you test Linux and Windows VMs with accelerated networking in separate subscriptions.</span></span>
>

1. <span data-ttu-id="0aa5b-222">Az Azure PowerShell legújabb verziójának telepítéséhez [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-222">Install the latest version of the Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module.</span></span> <span data-ttu-id="0aa5b-223">Ha most ismerkedik az Azure PowerShell, olvassa el a [Azure PowerShell áttekintése](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-223">If you're new to Azure PowerShell, read the [Azure PowerShell overview](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) article.</span></span>
2. <span data-ttu-id="0aa5b-224">A PowerShell-munkamenet indításához kattintson a Start gombra, írja be **powershell**, majd kattintson a **PowerShell** a keresési eredmények.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-224">Start a PowerShell session by clicking the Windows Start button, typing **powershell**, then clicking **PowerShell** from the search results.</span></span>
3. <span data-ttu-id="0aa5b-225">A PowerShell-ablakban írja be a `login-azurermaccount` parancs futtatásával jelentkezzen be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="0aa5b-225">In your PowerShell window, enter the `login-azurermaccount` command to sign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="0aa5b-226">Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="0aa5b-226">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
4. <span data-ttu-id="0aa5b-227">Regisztráció az Azure-gyorsított hálózati preview az alábbi lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-227">Register for the accelerated networking for Azure preview by completing the following steps:</span></span>
    - <span data-ttu-id="0aa5b-228">E-mail küldése [ axnpreview@microsoft.com ](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) az Azure-előfizetése Azonosítóját és a tervezett használattól.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-228">Send an email to [axnpreview@microsoft.com](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) with your Azure subscription ID and intended use.</span></span> <span data-ttu-id="0aa5b-229">Várjon, amíg egy e-mailes megerősítés tudnivalók az előfizetés engedélyezése a Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-229">Please wait for an email confirmation from Microsoft about your subscription being enabled.</span></span>
    - <span data-ttu-id="0aa5b-230">Adja meg annak ellenőrzéséhez, hogy az előzetes regisztrálta a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-230">Enter the following command to confirm you are registered for the preview:</span></span>
    
        ```powershell
        Get-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingForLinux -ProviderNamespace Microsoft.Network
        ```

        <span data-ttu-id="0aa5b-231">Ne folytassa az 5. lépés-ig **regisztrált** kimenet jelenik meg, miután megadta az előző parancsot.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-231">Do not continue with step 5 until **Registered** appears in the output after you enter the previous command.</span></span> <span data-ttu-id="0aa5b-232">A kimeneti folytatása előtt a következő kimeneti hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-232">Your output must look like the following output before continuing:</span></span>
    
        ```powershell
        FeatureName                        ProviderName      RegistrationState
        -----------                        ------------      -----------------
        AllowAcceleratedNetworkingForLinux Microsoft.Network Registered
        ```
        
      >[!NOTE]
      ><span data-ttu-id="0aa5b-233">Amennyiben Ön részt vett a gyorsított hálózatkezelés a Windows virtuális gépek preview (már nem szükséges Windows virtuális gépek hálózati gyorsított használandó regisztrálása), akkor nincsenek automatikusan regisztrálva a gyorsított hálózatkezelési a Linux virtuális gépek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-233">If you participated in the Accelerated networking for Windows VMs preview (it's no longer necessary to register to use Accelerated networking for Windows VMs), you are not automatically registered for the Accelerated networking for Linux VMs preview.</span></span> <span data-ttu-id="0aa5b-234">Regisztrálnia kell a gyorsított hálózati Linux virtuális gépek Preview részt.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-234">You must register for the Accelerated networking for Linux VMs preview to participate in it.</span></span>
      >
5. <span data-ttu-id="0aa5b-235">A böngészőben másolja a következő Ubuntu vagy SLES és a kívánt módon működjenek.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-235">In your browser, copy the following script substituting Ubuntu or SLES as desired.</span></span>  <span data-ttu-id="0aa5b-236">Ebben az esetben Redhat és CentOS van egy másik munkafolyamat, az alábbiakban leírt:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-236">Again, Redhat and CentOS have a different workflow outlined below:</span></span>

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Create a new Storage account and define the new VM’s OSDisk name and its URI
    # Must end with ".vhd" extension
    $OSDiskName = "MyOsDiskName.vhd"
    # Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.
    $OSDiskSAName = "thestorageaccountname"  
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $RgName -Name $OSDiskSAName -Type "Standard_GRS" -Location $Location
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName
 
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Linux `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName Canonical `
      -Offer UbuntuServer `
      -Skus 16.04-LTS `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk -Name $OSDiskName `
      -VhdUri $OSDiskUri `
      -CreateOption FromImage 

    # Create the virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    ```
    
6. <span data-ttu-id="0aa5b-237">A PowerShell-ablakban kattintson a jobb gombbal illessze be a parancsfájlt, és futtatnia kell elindítani.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-237">In your PowerShell window, right-click to paste the script and start executing it.</span></span> <span data-ttu-id="0aa5b-238">Felhasználónév és jelszó megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-238">You are prompted for a username and password.</span></span> <span data-ttu-id="0aa5b-239">Ezek a hitelesítő adatok használatát a bejelentkezéshez a virtuális géphez, amikor csatlakozik a következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-239">Use these credentials to log in to the VM when connecting to it in the next step.</span></span> <span data-ttu-id="0aa5b-240">Ha a parancsfájl futása sikertelen, ellenőrizze, hogy:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-240">If the script fails, confirm that:</span></span>
    - <span data-ttu-id="0aa5b-241">Regisztrálva az előzetes a 4. lépésben leírtak szerint</span><span class="sxs-lookup"><span data-stu-id="0aa5b-241">You are registered for the preview, as explained in step 4</span></span>
    - <span data-ttu-id="0aa5b-242">Ha megváltoztatta virtuális gép méretét, operációs rendszer típusa vagy a parancsfájl értékei előtt futtatnia kell, hogy az értékek láthatók a [korlátozások](#Limitations) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-242">If you changed VM size, operating system type, or location values in the script before executing it, that the values are listed in the [Limitations](#Limitations) section of this article.</span></span>
7. <span data-ttu-id="0aa5b-243">Gyorsított hálózati illesztőprogram telepítése Linux, lépéseinek végrehajtásához a [konfigurálása Linux](#configure-linux) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-243">To install the accelerated networking driver for Linux, complete the steps in the [Configure Linux](#configure-linux) section of this article.</span></span>

### <span data-ttu-id="0aa5b-244"><a name="configure-linux"></a>Linux konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0aa5b-244"><a name="configure-linux"></a>Configure Linux</span></span>

<span data-ttu-id="0aa5b-245">Miután létrehozta a virtuális Gépet az Azure-ban, telepítenie kell a gyorsított hálózati illesztőprogram Linux.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-245">Once you create the VM in Azure, you must install the accelerated networking driver for Linux.</span></span> <span data-ttu-id="0aa5b-246">Az alábbi lépések elvégzése előtt kell korábban létrehozott Linux virtuális gép gyorsított hálózat használatával a [portal](#linux-portal) vagy [PowerShell](#linux-powershell) ebben a cikkben ismertetett visszaállítási lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-246">Before completing the following steps, you must have created a Linux VM with accelerated networking using either the [portal](#linux-portal) or [PowerShell](#linux-powershell) steps in this article.</span></span> 

1. <span data-ttu-id="0aa5b-247">Egy webböngészőben nyissa meg az Azure [portal](https://portal.azure.com) és jelentkezzen be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="0aa5b-247">From an Internet browser, open the Azure [portal](https://portal.azure.com) and sign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="0aa5b-248">Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="0aa5b-248">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="0aa5b-249">A bal felső, a portál jobb oldalán a *keresési erőforrások* sávban kattintson a **> _** ikonra kattintva indítsa el a felhő rendszerhéjakba (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="0aa5b-249">At the top of the portal, to the right of the *Search resources* bar, click the **>_** icon to start a Bash cloud shell (Preview).</span></span> <span data-ttu-id="0aa5b-250">Megadja a lap alján, a portál, és néhány másodpercen belül megjelenik a Bash felhő rendszerhéj ablaktábla a  **username@Azure:~ $** kérdés.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-250">The Bash cloud shell pane appears at the bottom of the portal and after a few seconds, presents a **username@Azure:~$** prompt.</span></span> <span data-ttu-id="0aa5b-251">Bár a számítógépre, nem pedig a felhő rendszerhéj a virtuális gép is SSH, ebben az oktatóanyagban az utasítások feltételezik, hogy a felhő rendszerhéj használata.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-251">Though you can SSH to the VM from your computer, rather than the cloud shell, the instructions in this tutorial assume you're using the cloud shell.</span></span>
3. <span data-ttu-id="0aa5b-252">A portál felső részén a mezőbe, amely tartalmazza a szöveg *keresési erőforrások*, típus *MyVm*.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-252">At the top of the portal, in the box that contains the text *Search resources*, type *MyVm*.</span></span> <span data-ttu-id="0aa5b-253">Ha **MyVm** jelenik meg a keresési eredmények között kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-253">When **MyVm** appears in the search results, click it.</span></span>
4. <span data-ttu-id="0aa5b-254">Az a **MyVm** panel, amelyen megjelenik, kattintson a **Connect** a panel bal felső sarkában található gombra.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-254">In the **MyVm** blade that appears, click the **Connect** button in the top left corner of the blade.</span></span> <span data-ttu-id="0aa5b-255">**Megjegyzés:** Ha **létrehozása** alatt látható a **Connect** gomb, Azure még nem fejeződött be a virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-255">**Note:** If **Creating** is visible under the **Connect** button, Azure has not yet finished creating the VM.</span></span> <span data-ttu-id="0aa5b-256">Kattintson a **Connect** csak után többé nem láthatja **létrehozása** alatt a **Connect** gombra.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-256">Click **Connect** only after you no longer see **Creating** under the **Connect** button.</span></span>
5. <span data-ttu-id="0aa5b-257">Azure megnyitása felszólító adja meg a `ssh adminuser@<ipaddress>`.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-257">Azure opens a box telling you to enter the `ssh adminuser@<ipaddress>`.</span></span> <span data-ttu-id="0aa5b-258">Adja meg a parancs a felhő rendszerhéj (vagy azt a listából, hogy megjelent 4. lépés:, és illessze be a felhő rendszerhéj másolás), majd az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-258">Enter this command in the cloud shell (or copy it from the box that appeared in step 4 and paste it in to the cloud shell), then press Enter.</span></span>
6. <span data-ttu-id="0aa5b-259">Adjon meg **Igen** kéri, hogy ha folytatja a kapcsolódás, majd nyomja le az Enter a kérdésre.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-259">Enter **yes** to the question asking you if you want to continue connecting, then press Enter.</span></span>
7. <span data-ttu-id="0aa5b-260">Adja meg a virtuális gép létrehozásakor megadott jelszó.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-260">Enter the password you entered when creating the VM.</span></span> <span data-ttu-id="0aa5b-261">Egyszer sikeresen bejelentkezett a virtuális gép, megjelenik egy adminuser@MyVm:~ $ kérdés.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-261">Once successfully logged in to the VM, you see an adminuser@MyVm:~$ prompt.</span></span> <span data-ttu-id="0aa5b-262">Most jelentkezett a virtuális gép a felhőben rendszerhéj-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-262">You are now logged in to the VM through the cloud shell session.</span></span> <span data-ttu-id="0aa5b-263">**Megjegyzés:** felhő rendszerhéj munkamenetek időkorlátja lejárt 10 perc inaktivitás után.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-263">**Note:** Cloud shell sessions time out after 10 minutes of inactivity.</span></span>

<span data-ttu-id="0aa5b-264">Ezen a ponton utasításokat a telepítési módjától függően változhat.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-264">At this point, the instructions vary based on the distribution you are using.</span></span> 

#### <a name="ubuntusles"></a><span data-ttu-id="0aa5b-265">Ubuntu/SLES</span><span class="sxs-lookup"><span data-stu-id="0aa5b-265">Ubuntu/SLES</span></span>

1. <span data-ttu-id="0aa5b-266">A parancssorba írja be a `uname -r` , majd erősítse meg a verzió:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-266">At the prompt, enter `uname -r` and confirm the version for:</span></span>

    * <span data-ttu-id="0aa5b-267">Ubuntu "4.4.0-77-generic," vagy nagyobb</span><span class="sxs-lookup"><span data-stu-id="0aa5b-267">Ubuntu is "4.4.0-77-generic," or greater</span></span>
    * <span data-ttu-id="0aa5b-268">SLES "4.4.59-92.20-default" vagy nagyobb</span><span class="sxs-lookup"><span data-stu-id="0aa5b-268">SLES is "4.4.59-92.20-default" or greater</span></span>

2. <span data-ttu-id="0aa5b-269">Az alábbi parancsok futtatásával hozzon létre egy kötés a szabványos hálózati virtuális hálózati és a gyorsított hálózati vNIC között.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-269">Create a bond between the standard networking vNIC and the accelerated networking vNIC by running the commands that follow.</span></span> <span data-ttu-id="0aa5b-270">A hálózati forgalom használ a magasabb végrehajtása az elérését gyorsítja fel hálózati vNIC, amíg a kötés biztosítja, hogy hálózati adatforgalom nem szakad meg bizonyos konfigurációs változások során.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-270">Network traffic uses the higher performing accelerated networking vNIC, while the bond ensures that networking traffic is not interrupted across certain configuration changes.</span></span>
          
     ```bash
     wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/configure_hv_sriov.sh
     chmod +x ./configure_hv_sriov.sh
     sudo ./configure_hv_sriov.sh
     ```
3. <span data-ttu-id="0aa5b-271">A parancsfájl futtatása után a virtuális gép újraindul, 60 másodperc felfüggesztése után.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-271">After running the script, the VM will restart after a 60 second pause.</span></span>
4. <span data-ttu-id="0aa5b-272">Miután a virtuális gép újraindul, kapcsolódjon újra azt újra 5-7 lépések elvégzésével.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-272">Once the VM is restarted, reconnect to it by completing steps 5-7 again.</span></span>
5. <span data-ttu-id="0aa5b-273">Futtassa a `ifconfig` parancsot, és győződjön meg róla, hogy bond0 merült fel, és a rendszer azt jelzi, fel a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-273">Run the `ifconfig` command and confirm that bond0 has come up and the interface is showing as UP.</span></span> 
 
 >[!NOTE]
      ><span data-ttu-id="0aa5b-274">Gyorsított hálózatkezelés használó alkalmazások kell protokollt használó kommunikációra a *bond0* nem felület *eth0*.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-274">Applications using accelerated networking must communicate over the *bond0* interface, not *eth0*.</span></span>  <span data-ttu-id="0aa5b-275">A kapcsolat neve előtt gyorsított hálózatkezelés eléri az általánosan rendelkezésre álló módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-275">The interface name may change before accelerated networking reaches general availability.</span></span>

#### <a name="rhelcentos"></a><span data-ttu-id="0aa5b-276">RHEL vagy CentOS</span><span class="sxs-lookup"><span data-stu-id="0aa5b-276">RHEL/CentOS</span></span>

<span data-ttu-id="0aa5b-277">A Red Hat Enterprise Linux vagy a CentOS 7.3 VM létrehozásához meg néhány további lépést betölteni a legújabb illesztőprogramokkal SR-iov-t és a virtuális funkciójú (VF) illesztőprogram hálózati kártya számára szükséges.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-277">Creating a Red Hat Enterprise Linux or CentOS 7.3 VM requires some extra steps to load the latest drivers needed for SR-IOV and the Virtual Function (VF) driver for the network card.</span></span> <span data-ttu-id="0aa5b-278">Az első fázisban a utasítás előkészíti egy olyanra, amely egy vagy több virtuális gépeken, amelyek az előre betöltött illesztőprogramokat is használhatók.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-278">The first phase of the instructions prepares an image that can be used to make one or more virtual machines that have the drivers pre-loaded.</span></span>

##### <a name="phase-one-prepare-a-red-hat-enterprise-linux-or-centos-73-base-image"></a><span data-ttu-id="0aa5b-279">1. fázis: készítse elő a Red Hat Enterprise Linux vagy a CentOS 7.3 alapjául szolgáló lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-279">Phase one: prepare a Red Hat Enterprise Linux or CentOS 7.3 base image.</span></span> 

1.  <span data-ttu-id="0aa5b-280">Egy nem - PORTPROFIL CentOS 7.3 virtuális Gépet az Azure telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="0aa5b-280">Provision a non-SRIOV CentOS 7.3 VM on Azure</span></span>

2.  <span data-ttu-id="0aa5b-281">Telepíteni azokat 4.2.2:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-281">Install LIS 4.2.2:</span></span>
    
    ```bash
    wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
    tar -xvf lis-rpms-4.2.2-2.tar.gz
    cd LISISO && sudo ./install.sh
    ```

3.  <span data-ttu-id="0aa5b-282">Konfigurációs fájljainak letöltése</span><span class="sxs-lookup"><span data-stu-id="0aa5b-282">Download config files</span></span>
    
    ```bash
    cd /etc/udev/rules.d/  
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/60-hyperv-vf-name.rules 
    cd /usr/sbin/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/hv_vf_name 
    sudo chmod +x hv_vf_name
    cd /etc/sysconfig/network-scripts/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/ifcfg-vf1   
    ```

4.  <span data-ttu-id="0aa5b-283">Ez a virtuális gép kiosztásának megszüntetése</span><span class="sxs-lookup"><span data-stu-id="0aa5b-283">Deprovision this VM</span></span>

    ```bash
    sudo waagent -deprovision+user 
    ```

5.  <span data-ttu-id="0aa5b-284">Azure-portálon állítsa le a virtuális gép; Ugrás a virtuális gép "lemez", az OSDisk URI Azonosítójú virtuális merevlemez rögzítése.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-284">From Azure portal, stop this VM; and go to VM’s "Disks", capture the OSDisk’s VHD URI.</span></span> <span data-ttu-id="0aa5b-285">Ezt az URI és az alapjául szolgáló lemezképhez VHD-t a tárfiók tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-285">This URI contains the base image’s VHD name and its storage account.</span></span> 
 
##### <a name="phase-two-provision-new-vms-on-azure"></a><span data-ttu-id="0aa5b-286">2. fázis: Azure-alapú virtuális gépek új kiépítése</span><span class="sxs-lookup"><span data-stu-id="0aa5b-286">Phase two: Provision new VMs on Azure</span></span>

1.  <span data-ttu-id="0aa5b-287">Kiépítés új virtuális gépek és az alapjául szolgáló lemezképhez engedélyezve a virtuális hálózati AcceleratedNetworking, szakaszban rögzített virtuális merevlemez használatával új AzureRMVMConfig alapján:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-287">Provision new VMs based with New-AzureRMVMConfig using the base image VHD captured in phase one, with AcceleratedNetworking enabled on the vNIC:</span></span>

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
     -Name $RgName `
     -Location $Location

    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
     -Name MySubnet `
     -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
     -ResourceGroupName $RgName `
     -Location $Location `
     -Name MyVnet `
     -AddressPrefix 10.0.0.0/16 `
     -Subnet $Subnet
    
    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
     -Name MyPublicIp `
     -ResourceGroupName $RgName `
     -Location $Location `
     -AllocationMethod Static
    
    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
     -Name MyNic `
     -ResourceGroupName $RgName `
     -Location $Location `
     -SubnetId $Vnet.Subnets[0].Id `
     -PublicIpAddressId $Pip.Id `
     -EnableAcceleratedNetworking
    
    # Specify the base image's VHD URI (from phase one step 5). 
    # Note: The storage account of this base image vhd should have "Storage service encryption" disabled
    # See more from here: https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption
    # This is just an example URI, you will need to replace this when running this script
    $sourceUri="https://myexamplesa.blob.core.windows.net/vhds/CentOS73-Base-Test120170629111341.vhd" 

    # Specify a URI for the location from which the new image binary large object (BLOB) is copied to start the virtual machine. 
    # Must end with ".vhd" extension
    $OsDiskName = "MyOsDiskName.vhd" 
    $destOsDiskUri = "https://myexamplesa.blob.core.windows.net/vhds/" + $OsDiskName
    
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential
    
    # Create a custom virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
     -VMName MyVM -VMSize Standard_DS4_v2 | `
    Set-AzureRmVMOperatingSystem `
     -Linux `
     -ComputerName myVM `
     -Credential $Cred | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk `
     -Name $OsDiskName `
     -SourceImageUri $sourceUri `
     -VhdUri $destOsDiskUri `
     -CreateOption FromImage `
     -Linux
    
    # Create the virtual machine.    
    New-AzureRmVM `
     -ResourceGroupName $RgName `
     -Location $Location `
     -VM $VmConfig
    ```

2.  <span data-ttu-id="0aa5b-288">Után indítsa el a virtuális gépek, ellenőrizze a VF eszköz által "lspci", és ellenőrizze a Mellanox bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-288">After VMs boot up, check the VF device by "lspci" and check the Mellanox entry.</span></span> <span data-ttu-id="0aa5b-289">Például azt kell látnia a lspci kimeneti ezt az elemet:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-289">For example, we should see this item in the lspci output:</span></span>
    
    ```
    0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
    ```
    
3.  <span data-ttu-id="0aa5b-290">A ragasztás parancsprogrammal szerint:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-290">Run the bonding script by:</span></span>

    ```bash
    sudo bondvf.sh
    ```

4.  <span data-ttu-id="0aa5b-291">Indítsa újra az új virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="0aa5b-291">Reboot the new VMs:</span></span>

    ```bash
    sudo reboot
    ```

<span data-ttu-id="0aa5b-292">A virtuális gép konfigurált bond0 és az elérését gyorsítja fel hálózati elérési utat engedélyezve kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-292">The VM should start with bond0 configured and the Accelerated Networking path enabled.</span></span>  <span data-ttu-id="0aa5b-293">Futtatás `ifconfig` megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0aa5b-293">Run `ifconfig` to confirm.</span></span>
