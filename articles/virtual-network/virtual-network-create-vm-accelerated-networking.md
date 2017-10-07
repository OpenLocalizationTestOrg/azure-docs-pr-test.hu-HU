---
title: "egy Azure virtuális gép a hálózat elérését gyorsítja fel aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy virtuális gép a hálózat elérését gyorsítja fel."
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
ms.openlocfilehash: 241362891bfe083ab32c2f558be54f63f87c0693
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-with-accelerated-networking"></a><span data-ttu-id="643d0-103">A hálózat elérését gyorsítja fel a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="643d0-103">Create a virtual machine with Accelerated Networking</span></span>

<span data-ttu-id="643d0-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toocreate egy Azure virtuális gép (VM) a hálózat elérését gyorsítja fel.</span><span class="sxs-lookup"><span data-stu-id="643d0-104">In this tutorial, you learn how toocreate an Azure Virtual Machine (VM) with Accelerated Networking.</span></span> <span data-ttu-id="643d0-105">Gyorsított hálózatkezelés a Windows és a nyilvános előzetes verziójában az adott Linux terjesztésekről GA.</span><span class="sxs-lookup"><span data-stu-id="643d0-105">Accelerated Networking is GA for Windows and in a Public Preview for specific Linux distributions.</span></span> <span data-ttu-id="643d0-106">Gyorsított hálózatkezelés lehetővé teszi, hogy az egygyökerű i/o-virtualizálás (SR-IOV) tooa VM, a hálózati teljesítmény nagy mértékben javítva.</span><span class="sxs-lookup"><span data-stu-id="643d0-106">Accelerated networking enables single root I/O virtualization (SR-IOV) tooa VM, greatly improving its networking performance.</span></span> <span data-ttu-id="643d0-107">A nagy teljesítményű elérési út nincs hatással a hello gazdagép hello datapath csökkenti a késést, a jitter és a CPU-felhasználás legnagyobb igénybevételt jelentő hálózati munkaterhelését hello támogatott Virtuálisgép-típusokon való használatra.</span><span class="sxs-lookup"><span data-stu-id="643d0-107">This high-performance path bypasses hello host from hello datapath reducing latency, jitter, and CPU utilization, for use with hello most demanding network workloads on supported VM types.</span></span> <span data-ttu-id="643d0-108">hello rendelkező és anélküli gyorsított hálózatkezelés a képen látható kommunikációs következő két virtuális gép (VM) között:</span><span class="sxs-lookup"><span data-stu-id="643d0-108">hello following picture shows communication between two virtual machines (VM) with and without accelerated networking:</span></span>

![Összehasonlítása](./media/virtual-network-create-vm-accelerated-networking/image1.png)

<span data-ttu-id="643d0-110">Gyorsított hálózatkezelés nélkül, az összes hálózati forgalom mindkét virtuális gép hello be kell járnia hello gazdagépről, illetve hello virtuális kapcsolót.</span><span class="sxs-lookup"><span data-stu-id="643d0-110">Without accelerated networking, all networking traffic in and out of hello VM must traverse hello host and hello virtual switch.</span></span> <span data-ttu-id="643d0-111">hello virtuális kapcsoló betartatja az összes, például hálózati biztonsági csoportok, hozzáférhet szabálygyűjteményeket, elkülönítési és egyéb hálózati szempontból virtualizált toonetwork adatforgalmának.</span><span class="sxs-lookup"><span data-stu-id="643d0-111">hello virtual switch provides all policy enforcement, such as network security groups, access control lists, isolation, and other network virtualized services toonetwork traffic.</span></span> <span data-ttu-id="643d0-112">További információk a virtuális kapcsolók, olvassa el a hello toolearn [Hyper-V hálózatvirtualizálás és a virtuális kapcsoló](https://technet.microsoft.com/library/jj945275.aspx) cikk.</span><span class="sxs-lookup"><span data-stu-id="643d0-112">toolearn more about virtual switches, read hello [Hyper-V network virtualization and virtual switch](https://technet.microsoft.com/library/jj945275.aspx) article.</span></span>

<span data-ttu-id="643d0-113">Gyorsított hálózattal, a hálózati forgalom érkezik hello VM hálózati illesztőt (NIC), és ezután kerül toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="643d0-113">With accelerated networking, network traffic arrives at hello VM's network interface (NIC), and is then forwarded toohello VM.</span></span> <span data-ttu-id="643d0-114">Minden hálózati házirendek, amelyek a virtuális kapcsoló hello nélkül gyorsított hálózatkezelés kiszervezve, és a hardver alkalmazza a vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="643d0-114">All network policies that hello virtual switch applies without accelerated networking are offloaded and applied in hardware.</span></span> <span data-ttu-id="643d0-115">A hardver lehetővé teszi, hogy hello NIC tooforward házirend alkalmazása hálózati forgalom közvetlenül toohello VM kihagyásával hello gazdagépről, illetve hello virtuális kapcsolót, minden hello házirenddel azt hello fogadó megőrzésével.</span><span class="sxs-lookup"><span data-stu-id="643d0-115">Applying policy in hardware enables hello NIC tooforward network traffic directly toohello VM, bypassing hello host and hello virtual switch, while maintaining all hello policy it applied in hello host.</span></span>

<span data-ttu-id="643d0-116">hello gyorsított hálózatkezelés előnyei csak akkor jutnak érvényre toohello, hogy engedélyezve van a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="643d0-116">hello benefits of accelerated networking only apply toohello VM that it is enabled on.</span></span> <span data-ttu-id="643d0-117">A hello legjobb eredmény érdekében ideális tooenable legalább két virtuális gépeken Ez a szolgáltatás kapcsolódó toohello azonos Azure Virtual Network (VNet).</span><span class="sxs-lookup"><span data-stu-id="643d0-117">For hello best results, it is ideal tooenable this feature on at least two VMs connected toohello same Azure Virtual Network (VNet).</span></span> <span data-ttu-id="643d0-118">Ha kommunikál a Vnetek vagy az összekötő a helyszíni, ez a szolgáltatás rendelkezik gyakorolt minimális hatás mellett toooverall késés.</span><span class="sxs-lookup"><span data-stu-id="643d0-118">When communicating across VNets or connecting on-premises, this feature has minimal impact toooverall latency.</span></span>

> [!WARNING]
> <span data-ttu-id="643d0-119">A nyilvános előzetes verzió nem lehet azonos szintű rendelkezésre állást és megbízhatóságot, szolgáltatásokat, amelyek hello Linux általában elérhető kiadásai.</span><span class="sxs-lookup"><span data-stu-id="643d0-119">This Linux Public Preview may not have hello same level of availability and reliability as features that are in general availability release.</span></span> <span data-ttu-id="643d0-120">hello szolgáltatás nem támogatott, van, korlátozott képességek, és előfordulhat, hogy nem érhető el az összes Azure helyét.</span><span class="sxs-lookup"><span data-stu-id="643d0-120">hello feature is not supported, may have constrained capabilities, and may not be available in all Azure locations.</span></span> <span data-ttu-id="643d0-121">A legfrissebb értesítések hello rendelkezésre állás és a szolgáltatás állapotának tekintse meg hello Azure Virtual Network frissítések lap.</span><span class="sxs-lookup"><span data-stu-id="643d0-121">For hello most up-to-date notifications on availability and status of this feature, check hello Azure Virtual Network updates page.</span></span>

## <a name="benefits"></a><span data-ttu-id="643d0-122">Előnyök</span><span class="sxs-lookup"><span data-stu-id="643d0-122">Benefits</span></span>
* <span data-ttu-id="643d0-123">**Késés csökkentése / magasabb csomag / másodperc (pps):** eltávolításával hello virtuális kapcsoló a hello datapath hello idejének csomagok töltött eltávolítja hello gazdagép a házirend-feldolgozás, és növeli hello belül hello VM feldolgozható csomagok száma.</span><span class="sxs-lookup"><span data-stu-id="643d0-123">**Lower Latency / Higher packets per second (pps):** Removing hello virtual switch from hello datapath removes hello time packets spend in hello host for policy processing and increases hello number of packets that can be processed inside hello VM.</span></span>
* <span data-ttu-id="643d0-124">**Alacsonyabb jitter:** virtuális kapcsoló feldolgozása, amelyet a toobe alkalmazott házirend hello mennyiségét és hello hello CPU azon munkaterhelés, amely hello feldolgozási függ.</span><span class="sxs-lookup"><span data-stu-id="643d0-124">**Reduced jitter:** Virtual switch processing depends on hello amount of policy that needs toobe applied and hello workload of hello CPU that is doing hello processing.</span></span> <span data-ttu-id="643d0-125">Hello házirend kényszerítési toohello hardver kiszervezésével megszüntetéséhez, hogy a variancia csomagok továbbítása közvetlen toohello VM hello állomás tooVM kommunikáció és az összes szoftver megszakítások és a környezet eltávolítása vált.</span><span class="sxs-lookup"><span data-stu-id="643d0-125">Offloading hello policy enforcement toohello hardware removes that variability by delivering packets directly toohello VM, removing hello host tooVM communication and all software interrupts and context switches.</span></span>
* <span data-ttu-id="643d0-126">**Csökkent a CPU-felhasználás:** Bypassing hello virtuális kapcsoló hello fogadó vezet a hálózati forgalom feldolgozása tooless CPU-használatot.</span><span class="sxs-lookup"><span data-stu-id="643d0-126">**Decreased CPU utilization:** Bypassing hello virtual switch in hello host leads tooless CPU utilization for processing network traffic.</span></span>

## <span data-ttu-id="643d0-127"><a name="Limitations"></a>Korlátozások</span><span class="sxs-lookup"><span data-stu-id="643d0-127"><a name="Limitations"></a>Limitations</span></span>
<span data-ttu-id="643d0-128">a következő korlátozások hello létezik ez a funkció használata esetén:</span><span class="sxs-lookup"><span data-stu-id="643d0-128">hello following limitations exist when using this capability:</span></span>

* <span data-ttu-id="643d0-129">**A hálózati illesztő létrehozása:** gyorsított hálózat csak akkor engedélyezhető, az új hálózati</span><span class="sxs-lookup"><span data-stu-id="643d0-129">**Network interface creation:** Accelerated networking can only be enabled for a new NIC.</span></span> <span data-ttu-id="643d0-130">Nem engedélyezhető az egy meglévő hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="643d0-130">It cannot be enabled for an existing NIC.</span></span>
* <span data-ttu-id="643d0-131">**Virtuális gép létrehozása:** A hálózati adapter engedélyezve gyorsított hálózattal csak a virtuális gép kerül létrehozásra csatolt tooa méretű lehet, ha hello lehet.</span><span class="sxs-lookup"><span data-stu-id="643d0-131">**VM creation:** A NIC with accelerated networking enabled can only be attached tooa VM when hello VM is created.</span></span> <span data-ttu-id="643d0-132">hello hálózati adapter nem lehet meglévő virtuális gép csatlakoztatott tooan.</span><span class="sxs-lookup"><span data-stu-id="643d0-132">hello NIC cannot be attached tooan existing VM.</span></span>
* <span data-ttu-id="643d0-133">**Régiók:** Windows-alapú virtuális gépek gyorsított hálózattal érhető el az Azure-régióban.</span><span class="sxs-lookup"><span data-stu-id="643d0-133">**Regions:** Windows VMs with accelerated networking are offered in most Azure regions.</span></span> <span data-ttu-id="643d0-134">Linux virtuális gépek gyorsított hálózattal több régióba érhető el.</span><span class="sxs-lookup"><span data-stu-id="643d0-134">Linux VMs with accelerated networking are offered in multiple regions.</span></span> <span data-ttu-id="643d0-135">Ez a funkció érhető el a hello régiók növekszik.</span><span class="sxs-lookup"><span data-stu-id="643d0-135">hello regions this capability is available in is expanding.</span></span> <span data-ttu-id="643d0-136">Ellenőrizze a hello Azure virtuális hálózat frissítések blog alatt hello legújabb információkat.</span><span class="sxs-lookup"><span data-stu-id="643d0-136">Please see hello Azure Virtual Networking Updates blog below for hello latest information.</span></span>   
* <span data-ttu-id="643d0-137">**Támogatott operációs rendszerekről:** Windows: Microsoft Windows Server 2012 R2 Datacenter és a Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="643d0-137">**Supported operating systems:** Windows: Microsoft Windows Server 2012 R2 Datacenter and Windows Server 2016.</span></span> <span data-ttu-id="643d0-138">Linux: Ubuntu Server 16.04 kernel 4.4.0-77 vagy annál újabb LTS, SLES 12 SP2, RHEL 7.3 és CentOS 7.3 (közzétett "Engedélyezetlen Wave szoftver").</span><span class="sxs-lookup"><span data-stu-id="643d0-138">Linux: Ubuntu Server 16.04 LTS with kernel 4.4.0-77 or higher, SLES 12 SP2, RHEL 7.3 and CentOS 7.3 (Published by “Rogue Wave Software”).</span></span>
* <span data-ttu-id="643d0-139">**Virtuálisgép-méret:** általános célú és nyolc vagy több maggal rendelkező számítási optimalizált példány mérete.</span><span class="sxs-lookup"><span data-stu-id="643d0-139">**VM Size:** General purpose and compute-optimized instance sizes with eight or more cores.</span></span> <span data-ttu-id="643d0-140">További információkért lásd: hello [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) és [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gép méretének cikkeket.</span><span class="sxs-lookup"><span data-stu-id="643d0-140">For more information, see hello [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) and [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM sizes articles.</span></span> <span data-ttu-id="643d0-141">hello támogatott Virtuálisgép-példány méretek készlete bont ki a jövőbeli hello.</span><span class="sxs-lookup"><span data-stu-id="643d0-141">hello set of supported VM instance sizes will expand in hello future.</span></span>
* <span data-ttu-id="643d0-142">**Telepítés csak Azure Resource Managerrel (ARM) keresztül:** az elérését gyorsítja fel hálózati nincs elérhető a telepítéshez a címterület-kezelés/RDFE keresztül.</span><span class="sxs-lookup"><span data-stu-id="643d0-142">**Deployment through Azure Resource Manager (ARM) only:** Accelerated Networking is not available for deployment through ASM/RDFE.</span></span>

<span data-ttu-id="643d0-143">Módosítások toothese korlátozások hello keresztül történik bejelentés [Azure virtuális hálózat frissítése](https://azure.microsoft.com/updates/accelerated-networking-in-preview) lap.</span><span class="sxs-lookup"><span data-stu-id="643d0-143">Changes toothese limitations are announced through hello [Azure Virtual Networking updates](https://azure.microsoft.com/updates/accelerated-networking-in-preview) page.</span></span>

## <a name="create-a-windows-vm"></a><span data-ttu-id="643d0-144">Windows rendszerű virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="643d0-144">Create a Windows VM</span></span>
<span data-ttu-id="643d0-145">Hello Azure-portálon vagy az Azure [PowerShell](#windows-powershell) toocreate hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="643d0-145">You can use hello Azure portal or Azure [PowerShell](#windows-powershell) toocreate hello VM.</span></span>

### <span data-ttu-id="643d0-146"><a name="windows-portal"></a>Portál</span><span class="sxs-lookup"><span data-stu-id="643d0-146"><a name="windows-portal"></a>Portal</span></span>

1. <span data-ttu-id="643d0-147">Egy webböngészőben nyissa meg a hello Azure [portal](https://portal.azure.com) és jelentkezzen be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="643d0-147">From an Internet browser, open hello Azure [portal](https://portal.azure.com) and sign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="643d0-148">Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="643d0-148">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="643d0-149">Hello portálon kattintson **+ új** > **számítási** > **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="643d0-149">In hello portal, click **+ New** > **Compute** > **Windows Server 2016 Datacenter**.</span></span> 
3. <span data-ttu-id="643d0-150">A hello **Windows Server 2016 Datacenter** panel, amely akkor jelenik meg, hagyja *erőforrás-kezelő* a kijelölt **telepítési modell kiválasztása**, és kattintson a **létrehozása** .</span><span class="sxs-lookup"><span data-stu-id="643d0-150">In hello **Windows Server 2016 Datacenter** blade that appears, leave *Resource Manager* selected under **Select a deployment model**, and click **Create**.</span></span>
4. <span data-ttu-id="643d0-151">A hello **alapjai** panel, amelyen megjelenik, írja be a következő értékek hello, hagyja az alapértelmezett beállítások fennmaradó hello vagy válassza ki vagy adja meg a saját értékeit, és kattintson hello **OK** gombra:</span><span class="sxs-lookup"><span data-stu-id="643d0-151">In hello **Basics** blade that appears, enter hello following values, leave hello remaining default options or select or enter your own values, and click hello **OK** button:</span></span>

    |<span data-ttu-id="643d0-152">Beállítás</span><span class="sxs-lookup"><span data-stu-id="643d0-152">Setting</span></span>|<span data-ttu-id="643d0-153">Érték</span><span class="sxs-lookup"><span data-stu-id="643d0-153">Value</span></span>|
    |---|---|
    |<span data-ttu-id="643d0-154">Név</span><span class="sxs-lookup"><span data-stu-id="643d0-154">Name</span></span>|<span data-ttu-id="643d0-155">MyVm</span><span class="sxs-lookup"><span data-stu-id="643d0-155">MyVm</span></span>|
    |<span data-ttu-id="643d0-156">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="643d0-156">Resource group</span></span>|<span data-ttu-id="643d0-157">Hagyja **hozzon létre új** kiválasztva, és írja be *MyResourceGroup*</span><span class="sxs-lookup"><span data-stu-id="643d0-157">Leave **Create new** selected and enter *MyResourceGroup*</span></span>|
    |<span data-ttu-id="643d0-158">Hely</span><span class="sxs-lookup"><span data-stu-id="643d0-158">Location</span></span>|<span data-ttu-id="643d0-159">USA nyugati régiója, 2.</span><span class="sxs-lookup"><span data-stu-id="643d0-159">West US 2</span></span>|

    <span data-ttu-id="643d0-160">Ha új tooAzure, további információ [erőforráscsoportok](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [előfizetések](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), és [helyek](https://azure.microsoft.com/regions) (amely is hivatkozunk tooas régiók).</span><span class="sxs-lookup"><span data-stu-id="643d0-160">If you're new tooAzure, learn more about [Resource groups](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [subscriptions](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), and [locations](https://azure.microsoft.com/regions) (which are also referred tooas regions).</span></span>
5. <span data-ttu-id="643d0-161">A hello **méret kiválasztása** panel, amelyen megjelenik, írja be *8* a hello **minimális magszámra** mezőbe, majd kattintson az **összes**.</span><span class="sxs-lookup"><span data-stu-id="643d0-161">In hello **Choose a size** blade that appears, enter *8* in hello **Minimum cores** box, then click **View all**.</span></span>
6. <span data-ttu-id="643d0-162">Kattintson a **DS4_V2 szabványos**, vagy bármely támogatott virtuális gép, majd kattintson a hello **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="643d0-162">Click **DS4_V2 Standard**, or any supported VM, then click hello **Select** button.</span></span>
7. <span data-ttu-id="643d0-163">Hello a **beállítások** panelen megjelenő összes beállításokat hagyja-van, kivéve, kattintson **engedélyezve** alatt **elérését gyorsítja fel a hálózat**, majd kattintson a hello **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="643d0-163">In hello **Settings** blade that appears, leave all settings as-is, except click **Enabled** under **Accelerated networking**, then click hello **OK** button.</span></span> <span data-ttu-id="643d0-164">**Megjegyzés:** , ha előző lépésben kiválasztott hello nem szereplő értékek a virtuális gép mérete, az operációs rendszer vagy a hely [korlátozások](#Limitations) című szakaszát, **gyorsított hálózatkezelés**nem látható.</span><span class="sxs-lookup"><span data-stu-id="643d0-164">**Note:** If, in previous steps, you selected values for VM size, operating system, or location that aren't listed in hello [Limitations](#Limitations) section of this article, **Accelerated networking** isn't visible.</span></span>
8. <span data-ttu-id="643d0-165">A hello **összegzés** panel, amelyen megjelenik, kattintson a hello **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="643d0-165">In hello **Summary** blade that appears, click hello **OK** button.</span></span> <span data-ttu-id="643d0-166">Hello virtuális gép létrehozása Azure elindul.</span><span class="sxs-lookup"><span data-stu-id="643d0-166">Azure starts creating hello VM.</span></span> <span data-ttu-id="643d0-167">Virtuális gép létrehozásának folyamata eltart néhány percig.</span><span class="sxs-lookup"><span data-stu-id="643d0-167">VM creation takes a few minutes.</span></span>
9. <span data-ttu-id="643d0-168">tooinstall hello a Windows hálózati illesztőprogram gyorsított, teljes hello szükséges lépések hello [konfigurálása Windows](#configure-windows) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="643d0-168">tooinstall hello accelerated networking driver for Windows, complete hello steps in hello [Configure Windows](#configure-windows) section of this article.</span></span>

### <span data-ttu-id="643d0-169"><a name="windows-powershell"></a>PowerShell</span><span class="sxs-lookup"><span data-stu-id="643d0-169"><a name="windows-powershell"></a>PowerShell</span></span>
1. <span data-ttu-id="643d0-170">Hello hello Azure PowerShell legújabb verziójának telepítéséhez [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul.</span><span class="sxs-lookup"><span data-stu-id="643d0-170">Install hello latest version of hello Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module.</span></span> <span data-ttu-id="643d0-171">Ha új tooAzure PowerShell, olvassa el a hello [Azure PowerShell áttekintése](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk.</span><span class="sxs-lookup"><span data-stu-id="643d0-171">If you're new tooAzure PowerShell, read hello [Azure PowerShell overview](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) article.</span></span>
2. <span data-ttu-id="643d0-172">Indítsa el a Windows hello gombra kattintva indítsa el a egy PowerShell-munkamenetben írja be **powershell**, majd kattintson a **PowerShell** hello a keresési eredmények.</span><span class="sxs-lookup"><span data-stu-id="643d0-172">Start a PowerShell session by clicking hello Windows Start button, typing **powershell**, then clicking **PowerShell** from hello search results.</span></span>
3. <span data-ttu-id="643d0-173">A PowerShell-ablakban írja be a hello `login-azurermaccount` parancs toosign be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="643d0-173">In your PowerShell window, enter hello `login-azurermaccount` command toosign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="643d0-174">Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="643d0-174">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
4. <span data-ttu-id="643d0-175">A böngészőben másolja a következő parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="643d0-175">In your browser, copy hello following script:</span></span>
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

    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
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

    # Create hello virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    #
    ```
5. <span data-ttu-id="643d0-176">A PowerShell-ablakban kattintson a jobb gombbal a toopaste hello parancsfájlt, és futtatnia kell elindítani.</span><span class="sxs-lookup"><span data-stu-id="643d0-176">In your PowerShell window, right-click toopaste hello script and start executing it.</span></span> <span data-ttu-id="643d0-177">Felhasználónév és jelszó megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="643d0-177">You are prompted for a username and password.</span></span> <span data-ttu-id="643d0-178">Használja ezeket a hitelesítő adatok toolog toohello VM, hello következő lépésben tooit kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="643d0-178">Use these credentials toolog in toohello VM when connecting tooit in hello next step.</span></span> <span data-ttu-id="643d0-179">Ha hello parancsfájl futása sikertelen, és azt végrehajtása előtt módosította hello parancsfájlban értékét, ellenőrizze a virtuális gép méretét, operációs rendszer használt hello értékeket, és helyet hello szereplő [korlátozások](#Limitations) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="643d0-179">If hello script fails, and you changed values in hello script before executing it, confirm hello values you used for VM size, operating system, and location are listed in hello [Limitations](#Limitations) section of this article.</span></span>
6. <span data-ttu-id="643d0-180">tooinstall hello a Windows hálózati illesztőprogram gyorsított, teljes hello szükséges lépések hello [konfigurálása Windows](#configure-windows) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="643d0-180">tooinstall hello accelerated networking driver for Windows, complete hello steps in hello [Configure Windows](#configure-windows) section of this article.</span></span>

### <span data-ttu-id="643d0-181"><a name="configure-windows"></a>A Windows beállítása</span><span class="sxs-lookup"><span data-stu-id="643d0-181"><a name="configure-windows"></a>Configure Windows</span></span>
<span data-ttu-id="643d0-182">Miután létrehozta a virtuális gép hello az Azure-ban, telepítenie kell a Windows hello gyorsított hálózati illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="643d0-182">Once you create hello VM in Azure, you must install hello accelerated networking driver for Windows.</span></span> <span data-ttu-id="643d0-183">Hello lépések elvégzése előtt kell korábban létrehozott egy Windows virtuális gép vagy hello segítségével gyorsított hálózatkezelés [portal](#windows-portal) vagy [PowerShell](#windows-powershell) ebben a cikkben ismertetett visszaállítási lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="643d0-183">Before completing hello following steps, you must have created a Windows VM with accelerated networking using either hello [portal](#windows-portal) or [PowerShell](#windows-powershell) steps in this article.</span></span> 

1. <span data-ttu-id="643d0-184">Egy webböngészőben nyissa meg a hello Azure [portal](https://portal.azure.com) és jelentkezzen be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="643d0-184">From an Internet browser, open hello Azure [portal](https://portal.azure.com) and sign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="643d0-185">Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="643d0-185">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="643d0-186">Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *MyVm*.</span><span class="sxs-lookup"><span data-stu-id="643d0-186">In hello box that contains hello text *Search resources* at hello top of hello Azure portal, type *MyVm*.</span></span> <span data-ttu-id="643d0-187">Ha **MyVm** jelenik meg a hello találatok, kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="643d0-187">When **MyVm** appears in hello search results, click it.</span></span>
3. <span data-ttu-id="643d0-188">A hello **MyVm** panel, amelyen megjelenik, kattintson a hello **Connect** hello bal felső sarkában hello panel gombjára.</span><span class="sxs-lookup"><span data-stu-id="643d0-188">In hello **MyVm** blade that appears, click hello **Connect** button in hello top left corner of hello blade.</span></span> <span data-ttu-id="643d0-189">**Megjegyzés:** Ha **létrehozása** hello alatt látható **Connect** gomb, Azure még nem fejeződött be hello virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="643d0-189">**Note:** If **Creating** is visible under hello **Connect** button, Azure has not yet finished creating hello VM.</span></span> <span data-ttu-id="643d0-190">Kattintson a **Connect** csak után többé nem láthatja **létrehozása** alatt hello **Connect** gombra.</span><span class="sxs-lookup"><span data-stu-id="643d0-190">Click **Connect** only after you no longer see **Creating** under hello **Connect** button.</span></span>
4. <span data-ttu-id="643d0-191">A böngésző toodownload hello engedélyezése **MyVm.rdp** fájlt.</span><span class="sxs-lookup"><span data-stu-id="643d0-191">Allow your browser toodownload hello **MyVm.rdp** file.</span></span>  <span data-ttu-id="643d0-192">Ha le, kattintson a hello fájl tooopen azt.</span><span class="sxs-lookup"><span data-stu-id="643d0-192">Once downloaded, click hello file tooopen it.</span></span> 
5. <span data-ttu-id="643d0-193">Kattintson a hello **Connect** hello gombjára **távoli asztali kapcsolat** panelen, amely tájékoztat arról, hogy hello hello távoli kapcsolat közzétevője nem azonosítható.</span><span class="sxs-lookup"><span data-stu-id="643d0-193">Click hello **Connect** button in hello **Remote Desktop Connection** box that appears, notifying you that hello publisher of hello remote connection can't be identified.</span></span>
6. <span data-ttu-id="643d0-194">A hello **Windows biztonsági** meg, kattintson a **több lehetőséget**, majd kattintson a **használjon más fiókot**.</span><span class="sxs-lookup"><span data-stu-id="643d0-194">In hello **Windows Security** box that appears, click **More choices**, then click **Use a different account**.</span></span> <span data-ttu-id="643d0-195">Írja be a hello felhasználónév és a 4. lépésben megadott jelszót, majd kattintson a hello **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="643d0-195">Enter hello username and password you entered in step 4, then click hello **OK** button.</span></span>
7. <span data-ttu-id="643d0-196">Kattintson a hello **Igen** gomb hello távoli asztali kapcsolat-be, amely tájékoztat arról, hogy hello hello távoli számítógép nem azonosítható.</span><span class="sxs-lookup"><span data-stu-id="643d0-196">Click hello **Yes** button in hello Remote Desktop Connection box that appears, notifying you that hello identity of hello remote computer cannot be verified.</span></span>
8. <span data-ttu-id="643d0-197">Kattintson a jobb gombbal a hello Windows Start gombra, és kattintson a **Eszközkezelő**.</span><span class="sxs-lookup"><span data-stu-id="643d0-197">Right-click hello Windows Start button and click **Device Manager**.</span></span> <span data-ttu-id="643d0-198">Bontsa ki a hello **hálózati adapterek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="643d0-198">Expand hello **Network adapters** node.</span></span> <span data-ttu-id="643d0-199">Győződjön meg arról, hogy hello **Mellanox ConnectX-3 virtuális függvény Ethernet-Adapter** jelenik meg, ahogy az alábbi képen hello:</span><span class="sxs-lookup"><span data-stu-id="643d0-199">Confirm that hello **Mellanox ConnectX-3 Virtual Function Ethernet Adapter** appears, as shown in hello following picture:</span></span>
   
    ![Eszközkezelő](./media/virtual-network-create-vm-accelerated-networking/image2.png)

9. <span data-ttu-id="643d0-201">Gyorsított hálózatkezelés engedélyezve van a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="643d0-201">Accelerated Networking is now enabled for your VM.</span></span>

## <a name="create-a-linux-vm"></a><span data-ttu-id="643d0-202">Linux rendszerű virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="643d0-202">Create a Linux VM</span></span>
<span data-ttu-id="643d0-203">Hello Azure-portálon vagy az Azure [PowerShell](#linux-powershell) toocreate egy Ubuntu vagy SLES virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="643d0-203">You can use hello Azure portal or Azure [PowerShell](#linux-powershell) toocreate an Ubuntu or SLES VM.</span></span> <span data-ttu-id="643d0-204">Az RHEL és CentOS virtuális gépeket egy másik munkafolyamat van.</span><span class="sxs-lookup"><span data-stu-id="643d0-204">For RHEL and CentOS VMs there is a different workflow.</span></span>  <span data-ttu-id="643d0-205">Tekintse meg az alábbi hello utasításokat.</span><span class="sxs-lookup"><span data-stu-id="643d0-205">Please see hello instructions below.</span></span>

### <span data-ttu-id="643d0-206"><a name="linux-portal"></a>Portál</span><span class="sxs-lookup"><span data-stu-id="643d0-206"><a name="linux-portal"></a>Portal</span></span>
1. <span data-ttu-id="643d0-207">Hello gyorsított közötti hálózati Linux Preview 1-5. lépéseket a hello befejezése a register [hozzon létre egy Linux virtuális Gépet - PowerShell](#linux-powershell) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="643d0-207">Register for hello accelerated networking for Linux preview by completing steps 1-5 of hello [Create a Linux VM - PowerShell](#linux-powershell) section of this article.</span></span>  <span data-ttu-id="643d0-208">Hello Preview hello portálon nem regisztrálható.</span><span class="sxs-lookup"><span data-stu-id="643d0-208">You cannot register for hello preview in hello portal.</span></span>
2. <span data-ttu-id="643d0-209">Teljes lépések 1-8 a hello [hozzon létre egy Windows virtuális Gépet - portál](#windows-portal) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="643d0-209">Complete steps 1-8 in hello [Create a Windows VM - portal](#windows-portal) section of this article.</span></span> <span data-ttu-id="643d0-210">A 2. lépésben, kattintson a **Ubuntu Server 16.04 LTS** helyett **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="643d0-210">In step 2, click **Ubuntu Server 16.04 LTS** instead of **Windows Server 2016 Datacenter**.</span></span> <span data-ttu-id="643d0-211">Ebben az oktatóanyagban válassza a toouse jelszót, nem pedig egy SSH-kulccsal, abban az esetben, ha az üzemi környezetek is használhatja.</span><span class="sxs-lookup"><span data-stu-id="643d0-211">For this tutorial, choose toouse a password, rather than an SSH key, though for production deployments, you can use either.</span></span> <span data-ttu-id="643d0-212">Ha **hálózat az elérését gyorsítja fel** nem jelenik meg, amikor befejezte a hello 7. lépés [hozzon létre egy Windows virtuális Gépet - portál](#windows-portal) szakasz ezen cikk valószínű hello a következő okok valamelyike miatt:</span><span class="sxs-lookup"><span data-stu-id="643d0-212">If **Accelerated networking** does not appear when you complete step 7 of hello [Create a Windows VM - portal](#windows-portal) section of this article, it's likely for one of hello following reasons:</span></span>
    - <span data-ttu-id="643d0-213">Még nem regisztrált hello Preview.</span><span class="sxs-lookup"><span data-stu-id="643d0-213">You are not yet registered for hello preview.</span></span> <span data-ttu-id="643d0-214">Győződjön meg arról, hogy a regisztrációs állapotát **regisztrált**, hello 4. lépésben leírtak szerint [hozzon létre egy Linux virtuális Gépet - Powershell](#linux-powershell) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="643d0-214">Confirm that your registration state is **Registered**, as explained in step 4 of hello [Create a Linux VM - Powershell](#linux-powershell) section of this article.</span></span> <span data-ttu-id="643d0-215">**Megjegyzés:** Ha Ön részt vett a hello gyorsított hálózatkezelés a Windows virtuális gépek preview (fennállt nem hosszabb szükséges tooregister toouse az elérését gyorsítja fel Windows virtuális gépek hálózati), akkor nem automatikusan regisztrálva vannak hello gyorsított a hálózati Linux virtuális gépek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="643d0-215">**Note:** If you participated in hello Accelerated networking for Windows VMs preview (it's no longer necessary tooregister toouse Accelerated networking for Windows VMs), you are not automatically registered for hello Accelerated networking for Linux VMs preview.</span></span> <span data-ttu-id="643d0-216">Regisztrálnia kell a hello gyorsított hálózati a Linux virtuális gépek előzetes tooparticipate azt.</span><span class="sxs-lookup"><span data-stu-id="643d0-216">You must register for hello Accelerated networking for Linux VMs preview tooparticipate in it.</span></span>
    - <span data-ttu-id="643d0-217">Nincs kiválasztva a virtuális gép méretét, a operációs rendszer vagy a hely hello felsorolt [korlátozások](#limitations) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="643d0-217">You have not selected a VM size, operating system, or location listed in hello [Limitations](#limitations) section of this article.</span></span>
3. <span data-ttu-id="643d0-218">tooinstall hello az elérését gyorsítja fel hálózati illesztőprogram Linux, teljes hello szükséges lépések hello [konfigurálása Linux](#configure-linux) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="643d0-218">tooinstall hello accelerated networking driver for Linux, complete hello steps in hello [Configure Linux](#configure-linux) section of this article.</span></span>

### <span data-ttu-id="643d0-219"><a name="linux-powershell"></a>PowerShell</span><span class="sxs-lookup"><span data-stu-id="643d0-219"><a name="linux-powershell"></a>PowerShell</span></span>

>[!WARNING]
><span data-ttu-id="643d0-220">Ha a Linux virtuális gépek létrehozása egy előfizetésben gyorsított hálózattal, majd próbálja meg toocreate hello gyorsított hálózatkezelés a Windows virtuális gép ugyanahhoz az előfizetéshez, hello Windows virtuális gép létrehozása sikertelen lehet.</span><span class="sxs-lookup"><span data-stu-id="643d0-220">If you create Linux VMs with accelerated networking in a subscription, and then attempt toocreate a Windows VM with accelerated networking in hello same subscription, hello Windows VM creation may fail.</span></span> <span data-ttu-id="643d0-221">Ez az előzetes ajánlott, hogy tesztelje a Linux és a Windows virtuális gépek külön előfizetésekhez gyorsított hálózattal.</span><span class="sxs-lookup"><span data-stu-id="643d0-221">During this preview, it's recommended that you test Linux and Windows VMs with accelerated networking in separate subscriptions.</span></span>
>

1. <span data-ttu-id="643d0-222">Hello hello Azure PowerShell legújabb verziójának telepítéséhez [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul.</span><span class="sxs-lookup"><span data-stu-id="643d0-222">Install hello latest version of hello Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module.</span></span> <span data-ttu-id="643d0-223">Ha új tooAzure PowerShell, olvassa el a hello [Azure PowerShell áttekintése](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk.</span><span class="sxs-lookup"><span data-stu-id="643d0-223">If you're new tooAzure PowerShell, read hello [Azure PowerShell overview](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) article.</span></span>
2. <span data-ttu-id="643d0-224">Indítsa el a Windows hello gombra kattintva indítsa el a egy PowerShell-munkamenetben írja be **powershell**, majd kattintson a **PowerShell** hello a keresési eredmények.</span><span class="sxs-lookup"><span data-stu-id="643d0-224">Start a PowerShell session by clicking hello Windows Start button, typing **powershell**, then clicking **PowerShell** from hello search results.</span></span>
3. <span data-ttu-id="643d0-225">A PowerShell-ablakban írja be a hello `login-azurermaccount` parancs toosign be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="643d0-225">In your PowerShell window, enter hello `login-azurermaccount` command toosign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="643d0-226">Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="643d0-226">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
4. <span data-ttu-id="643d0-227">Regisztrálás az Azure-hálózat elérését gyorsítja fel hello preview hello lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="643d0-227">Register for hello accelerated networking for Azure preview by completing hello following steps:</span></span>
    - <span data-ttu-id="643d0-228">Az e-mailek küldése túl[ axnpreview@microsoft.com ](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) az Azure-előfizetése Azonosítóját és a tervezett használattól.</span><span class="sxs-lookup"><span data-stu-id="643d0-228">Send an email too[axnpreview@microsoft.com](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) with your Azure subscription ID and intended use.</span></span> <span data-ttu-id="643d0-229">Várjon, amíg egy e-mailes megerősítés tudnivalók az előfizetés engedélyezése a Microsoft.</span><span class="sxs-lookup"><span data-stu-id="643d0-229">Please wait for an email confirmation from Microsoft about your subscription being enabled.</span></span>
    - <span data-ttu-id="643d0-230">Adja meg a következő parancs tooconfirm hello Preview regisztrálta hello:</span><span class="sxs-lookup"><span data-stu-id="643d0-230">Enter hello following command tooconfirm you are registered for hello preview:</span></span>
    
        ```powershell
        Get-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingForLinux -ProviderNamespace Microsoft.Network
        ```

        <span data-ttu-id="643d0-231">Ne folytassa az 5. lépés-ig **regisztrált** hello kimeneti hello előző parancs megadása után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="643d0-231">Do not continue with step 5 until **Registered** appears in hello output after you enter hello previous command.</span></span> <span data-ttu-id="643d0-232">A kimeneti hello hasonló kimenetet a folytatás előtt kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="643d0-232">Your output must look like hello following output before continuing:</span></span>
    
        ```powershell
        FeatureName                        ProviderName      RegistrationState
        -----------                        ------------      -----------------
        AllowAcceleratedNetworkingForLinux Microsoft.Network Registered
        ```
        
      >[!NOTE]
      ><span data-ttu-id="643d0-233">Ha Ön részt vett a hello gyorsított hálózatkezelés a Windows virtuális gépek preview (fennállt nem hosszabb szükséges tooregister toouse az elérését gyorsítja fel Windows virtuális gépek hálózati), akkor nem automatikusan regisztrálva vannak hello gyorsított Linux virtuális gépek Preview hálózat.</span><span class="sxs-lookup"><span data-stu-id="643d0-233">If you participated in hello Accelerated networking for Windows VMs preview (it's no longer necessary tooregister toouse Accelerated networking for Windows VMs), you are not automatically registered for hello Accelerated networking for Linux VMs preview.</span></span> <span data-ttu-id="643d0-234">Regisztrálnia kell a hello gyorsított hálózati a Linux virtuális gépek előzetes tooparticipate azt.</span><span class="sxs-lookup"><span data-stu-id="643d0-234">You must register for hello Accelerated networking for Linux VMs preview tooparticipate in it.</span></span>
      >
5. <span data-ttu-id="643d0-235">A böngészőben a következő parancsfájl és Ubuntu vagy SLES tetszés szerint hello másolja.</span><span class="sxs-lookup"><span data-stu-id="643d0-235">In your browser, copy hello following script substituting Ubuntu or SLES as desired.</span></span>  <span data-ttu-id="643d0-236">Ebben az esetben Redhat és CentOS van egy másik munkafolyamat, az alábbiakban leírt:</span><span class="sxs-lookup"><span data-stu-id="643d0-236">Again, Redhat and CentOS have a different workflow outlined below:</span></span>

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

    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Create a new Storage account and define hello new VM’s OSDisk name and its URI
    # Must end with ".vhd" extension
    $OSDiskName = "MyOsDiskName.vhd"
    # Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.
    $OSDiskSAName = "thestorageaccountname"  
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $RgName -Name $OSDiskSAName -Type "Standard_GRS" -Location $Location
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName
 
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
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

    # Create hello virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    ```
    
6. <span data-ttu-id="643d0-237">A PowerShell-ablakban kattintson a jobb gombbal a toopaste hello parancsfájlt, és futtatnia kell elindítani.</span><span class="sxs-lookup"><span data-stu-id="643d0-237">In your PowerShell window, right-click toopaste hello script and start executing it.</span></span> <span data-ttu-id="643d0-238">Felhasználónév és jelszó megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="643d0-238">You are prompted for a username and password.</span></span> <span data-ttu-id="643d0-239">Használja ezeket a hitelesítő adatok toolog toohello VM, hello következő lépésben tooit kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="643d0-239">Use these credentials toolog in toohello VM when connecting tooit in hello next step.</span></span> <span data-ttu-id="643d0-240">Ha hello parancsfájl futása sikertelen, ellenőrizze, hogy:</span><span class="sxs-lookup"><span data-stu-id="643d0-240">If hello script fails, confirm that:</span></span>
    - <span data-ttu-id="643d0-241">Regisztrált hello Preview, a 4. lépésben leírtak szerint</span><span class="sxs-lookup"><span data-stu-id="643d0-241">You are registered for hello preview, as explained in step 4</span></span>
    - <span data-ttu-id="643d0-242">Ha módosította virtuális gép méretét, operációsrendszer-típusra vagy hello parancsfájlban értékei előtt futtatnia kell, hogy hello értékek szerepelnek hello [korlátozások](#Limitations) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="643d0-242">If you changed VM size, operating system type, or location values in hello script before executing it, that hello values are listed in hello [Limitations](#Limitations) section of this article.</span></span>
7. <span data-ttu-id="643d0-243">tooinstall hello az elérését gyorsítja fel hálózati illesztőprogram Linux, teljes hello szükséges lépések hello [konfigurálása Linux](#configure-linux) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="643d0-243">tooinstall hello accelerated networking driver for Linux, complete hello steps in hello [Configure Linux](#configure-linux) section of this article.</span></span>

### <span data-ttu-id="643d0-244"><a name="configure-linux"></a>Linux konfigurálása</span><span class="sxs-lookup"><span data-stu-id="643d0-244"><a name="configure-linux"></a>Configure Linux</span></span>

<span data-ttu-id="643d0-245">Miután létrehozta a virtuális gép hello az Azure-ban, Linux hello gyorsított hálózati illesztőprogram kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="643d0-245">Once you create hello VM in Azure, you must install hello accelerated networking driver for Linux.</span></span> <span data-ttu-id="643d0-246">Hello lépések elvégzése előtt kell korábban létrehozott egy Linux virtuális gép vagy hello segítségével gyorsított hálózatkezelés [portal](#linux-portal) vagy [PowerShell](#linux-powershell) ebben a cikkben ismertetett visszaállítási lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="643d0-246">Before completing hello following steps, you must have created a Linux VM with accelerated networking using either hello [portal](#linux-portal) or [PowerShell](#linux-powershell) steps in this article.</span></span> 

1. <span data-ttu-id="643d0-247">Egy webböngészőben nyissa meg a hello Azure [portal](https://portal.azure.com) és jelentkezzen be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="643d0-247">From an Internet browser, open hello Azure [portal](https://portal.azure.com) and sign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="643d0-248">Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="643d0-248">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="643d0-249">Hello portálon toohello jobb oldalán hello hello tetején *keresési erőforrások* kattintson a hello **> _** ikon toostart a felhő rendszerhéjakba (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="643d0-249">At hello top of hello portal, toohello right of hello *Search resources* bar, click hello **>_** icon toostart a Bash cloud shell (Preview).</span></span> <span data-ttu-id="643d0-250">hello Bash felhő rendszerhéj ablaktáblán megjelenik hello alsó hello portál, és néhány másodpercen belül megadja egy  **username@Azure:~ $** kérdés.</span><span class="sxs-lookup"><span data-stu-id="643d0-250">hello Bash cloud shell pane appears at hello bottom of hello portal and after a few seconds, presents a **username@Azure:~$** prompt.</span></span> <span data-ttu-id="643d0-251">Abban az esetben, ha a számítógép, hanem hello felhő rendszerhéj a virtuális gép SSH toohello is, ebben az oktatóanyagban hello utasítások feltételezik, hogy hello felhő rendszerhéj használata.</span><span class="sxs-lookup"><span data-stu-id="643d0-251">Though you can SSH toohello VM from your computer, rather than hello cloud shell, hello instructions in this tutorial assume you're using hello cloud shell.</span></span>
3. <span data-ttu-id="643d0-252">Hello portál hello tetején hello mezőben tartalmazó hello szöveg *keresési erőforrások*, típus *MyVm*.</span><span class="sxs-lookup"><span data-stu-id="643d0-252">At hello top of hello portal, in hello box that contains hello text *Search resources*, type *MyVm*.</span></span> <span data-ttu-id="643d0-253">Ha **MyVm** jelenik meg a hello találatok, kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="643d0-253">When **MyVm** appears in hello search results, click it.</span></span>
4. <span data-ttu-id="643d0-254">A hello **MyVm** panel, amelyen megjelenik, kattintson a hello **Connect** hello bal felső sarkában hello panel gombjára.</span><span class="sxs-lookup"><span data-stu-id="643d0-254">In hello **MyVm** blade that appears, click hello **Connect** button in hello top left corner of hello blade.</span></span> <span data-ttu-id="643d0-255">**Megjegyzés:** Ha **létrehozása** hello alatt látható **Connect** gomb, Azure még nem fejeződött be hello virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="643d0-255">**Note:** If **Creating** is visible under hello **Connect** button, Azure has not yet finished creating hello VM.</span></span> <span data-ttu-id="643d0-256">Kattintson a **Connect** csak után többé nem láthatja **létrehozása** alatt hello **Connect** gombra.</span><span class="sxs-lookup"><span data-stu-id="643d0-256">Click **Connect** only after you no longer see **Creating** under hello **Connect** button.</span></span>
5. <span data-ttu-id="643d0-257">Azure megnyitása felszólító tooenter hello `ssh adminuser@<ipaddress>`.</span><span class="sxs-lookup"><span data-stu-id="643d0-257">Azure opens a box telling you tooenter hello `ssh adminuser@<ipaddress>`.</span></span> <span data-ttu-id="643d0-258">Adja meg a parancs hello felhő rendszerhéj (vagy másolási megjelent hello listából, 4. lépés:, és illessze be azt a toohello felhő rendszerhéj), majd az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="643d0-258">Enter this command in hello cloud shell (or copy it from hello box that appeared in step 4 and paste it in toohello cloud shell), then press Enter.</span></span>
6. <span data-ttu-id="643d0-259">Adjon meg **Igen** kérni, ha azt szeretné, hogy a toocontinue csatlakozni, majd nyomja le az Enter toohello kérdést.</span><span class="sxs-lookup"><span data-stu-id="643d0-259">Enter **yes** toohello question asking you if you want toocontinue connecting, then press Enter.</span></span>
7. <span data-ttu-id="643d0-260">Adja meg a hello virtuális gép létrehozásakor megadott hello jelszó.</span><span class="sxs-lookup"><span data-stu-id="643d0-260">Enter hello password you entered when creating hello VM.</span></span> <span data-ttu-id="643d0-261">Egyszer a bejelentkezés sikerült toohello VM, megjelenik egy adminuser@MyVm:~ $ kérdés.</span><span class="sxs-lookup"><span data-stu-id="643d0-261">Once successfully logged in toohello VM, you see an adminuser@MyVm:~$ prompt.</span></span> <span data-ttu-id="643d0-262">Most jelentkezett toohello VM hello felhő rendszerhéj kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="643d0-262">You are now logged in toohello VM through hello cloud shell session.</span></span> <span data-ttu-id="643d0-263">**Megjegyzés:** felhő rendszerhéj munkamenetek időkorlátja lejárt 10 perc inaktivitás után.</span><span class="sxs-lookup"><span data-stu-id="643d0-263">**Note:** Cloud shell sessions time out after 10 minutes of inactivity.</span></span>

<span data-ttu-id="643d0-264">Ezen a ponton hello utasításokat hello telepítési módjától függően változhat.</span><span class="sxs-lookup"><span data-stu-id="643d0-264">At this point, hello instructions vary based on hello distribution you are using.</span></span> 

#### <a name="ubuntusles"></a><span data-ttu-id="643d0-265">Ubuntu/SLES</span><span class="sxs-lookup"><span data-stu-id="643d0-265">Ubuntu/SLES</span></span>

1. <span data-ttu-id="643d0-266">Hello kérdés esetén `uname -r` , majd erősítse meg a hello verzió:</span><span class="sxs-lookup"><span data-stu-id="643d0-266">At hello prompt, enter `uname -r` and confirm hello version for:</span></span>

    * <span data-ttu-id="643d0-267">Ubuntu "4.4.0-77-generic," vagy nagyobb</span><span class="sxs-lookup"><span data-stu-id="643d0-267">Ubuntu is "4.4.0-77-generic," or greater</span></span>
    * <span data-ttu-id="643d0-268">SLES "4.4.59-92.20-default" vagy nagyobb</span><span class="sxs-lookup"><span data-stu-id="643d0-268">SLES is "4.4.59-92.20-default" or greater</span></span>

2. <span data-ttu-id="643d0-269">Hozzon létre egy szabványos hálózati vNIC hello és gyorsított hello hálózati vNIC közötti kötés futó hello parancsoknál, amelyek.</span><span class="sxs-lookup"><span data-stu-id="643d0-269">Create a bond between hello standard networking vNIC and hello accelerated networking vNIC by running hello commands that follow.</span></span> <span data-ttu-id="643d0-270">Hálózati forgalom hello magasabb gyorsított hálózati vNIC, hajt végre, amíg hello kötés biztosítja, hogy hálózati adatforgalom nem szakad meg bizonyos konfigurációs változások során használja.</span><span class="sxs-lookup"><span data-stu-id="643d0-270">Network traffic uses hello higher performing accelerated networking vNIC, while hello bond ensures that networking traffic is not interrupted across certain configuration changes.</span></span>
          
     ```bash
     wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/configure_hv_sriov.sh
     chmod +x ./configure_hv_sriov.sh
     sudo ./configure_hv_sriov.sh
     ```
3. <span data-ttu-id="643d0-271">Hello a parancsfájl futtatását, miután a hello VM 60 másodperc felfüggesztése után újraindul.</span><span class="sxs-lookup"><span data-stu-id="643d0-271">After running hello script, hello VM will restart after a 60 second pause.</span></span>
4. <span data-ttu-id="643d0-272">Virtuális gép újraindul, hello egyszer tooit újracsatlakozás újra 5-7 lépések elvégzésével.</span><span class="sxs-lookup"><span data-stu-id="643d0-272">Once hello VM is restarted, reconnect tooit by completing steps 5-7 again.</span></span>
5. <span data-ttu-id="643d0-273">Futtassa a hello `ifconfig` parancsot, és győződjön meg róla, hogy bond0 merült fel, és a rendszer hello felület azt jelzi, fel.</span><span class="sxs-lookup"><span data-stu-id="643d0-273">Run hello `ifconfig` command and confirm that bond0 has come up and hello interface is showing as UP.</span></span> 
 
 >[!NOTE]
      ><span data-ttu-id="643d0-274">Gyorsított hálózatkezelés használó alkalmazások kell kommunikálhassanak hello *bond0* nem felület *eth0*.</span><span class="sxs-lookup"><span data-stu-id="643d0-274">Applications using accelerated networking must communicate over hello *bond0* interface, not *eth0*.</span></span>  <span data-ttu-id="643d0-275">hello kapcsolat neve előtt gyorsított hálózatkezelés eléri az általánosan rendelkezésre álló módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="643d0-275">hello interface name may change before accelerated networking reaches general availability.</span></span>

#### <a name="rhelcentos"></a><span data-ttu-id="643d0-276">RHEL vagy CentOS</span><span class="sxs-lookup"><span data-stu-id="643d0-276">RHEL/CentOS</span></span>

<span data-ttu-id="643d0-277">A Red Hat Enterprise Linux vagy a CentOS 7.3 VM létrehozásához meg néhány további lépések tooload hello legújabb illesztőprogramokkal SR-iov-t és a hello virtuális funkciójú (VF) illesztőprogram hello hálózati kártya számára szükséges.</span><span class="sxs-lookup"><span data-stu-id="643d0-277">Creating a Red Hat Enterprise Linux or CentOS 7.3 VM requires some extra steps tooload hello latest drivers needed for SR-IOV and hello Virtual Function (VF) driver for hello network card.</span></span> <span data-ttu-id="643d0-278">hello hello utasításokat első fázisa előkészít egy olyanra, amely lehet használt toomake egy vagy több virtuális gépeken, amelyek előre betöltött hello illesztőprogramok.</span><span class="sxs-lookup"><span data-stu-id="643d0-278">hello first phase of hello instructions prepares an image that can be used toomake one or more virtual machines that have hello drivers pre-loaded.</span></span>

##### <a name="phase-one-prepare-a-red-hat-enterprise-linux-or-centos-73-base-image"></a><span data-ttu-id="643d0-279">1. fázis: készítse elő a Red Hat Enterprise Linux vagy a CentOS 7.3 alapjául szolgáló lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="643d0-279">Phase one: prepare a Red Hat Enterprise Linux or CentOS 7.3 base image.</span></span> 

1.  <span data-ttu-id="643d0-280">Egy nem - PORTPROFIL CentOS 7.3 virtuális Gépet az Azure telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="643d0-280">Provision a non-SRIOV CentOS 7.3 VM on Azure</span></span>

2.  <span data-ttu-id="643d0-281">Telepíteni azokat 4.2.2:</span><span class="sxs-lookup"><span data-stu-id="643d0-281">Install LIS 4.2.2:</span></span>
    
    ```bash
    wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
    tar -xvf lis-rpms-4.2.2-2.tar.gz
    cd LISISO && sudo ./install.sh
    ```

3.  <span data-ttu-id="643d0-282">Konfigurációs fájljainak letöltése</span><span class="sxs-lookup"><span data-stu-id="643d0-282">Download config files</span></span>
    
    ```bash
    cd /etc/udev/rules.d/  
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/60-hyperv-vf-name.rules 
    cd /usr/sbin/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/hv_vf_name 
    sudo chmod +x hv_vf_name
    cd /etc/sysconfig/network-scripts/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/ifcfg-vf1   
    ```

4.  <span data-ttu-id="643d0-283">Ez a virtuális gép kiosztásának megszüntetése</span><span class="sxs-lookup"><span data-stu-id="643d0-283">Deprovision this VM</span></span>

    ```bash
    sudo waagent -deprovision+user 
    ```

5.  <span data-ttu-id="643d0-284">Azure-portálon állítsa le a virtuális gép; Nyissa meg a tooVM "lemez", hello OSDisk URI Azonosítójú virtuális merevlemez rögzítése és.</span><span class="sxs-lookup"><span data-stu-id="643d0-284">From Azure portal, stop this VM; and go tooVM’s "Disks", capture hello OSDisk’s VHD URI.</span></span> <span data-ttu-id="643d0-285">Ezt az URI hello alapjául szolgáló lemezképhez VHD-t és a tárfiók tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="643d0-285">This URI contains hello base image’s VHD name and its storage account.</span></span> 
 
##### <a name="phase-two-provision-new-vms-on-azure"></a><span data-ttu-id="643d0-286">2. fázis: Azure-alapú virtuális gépek új kiépítése</span><span class="sxs-lookup"><span data-stu-id="643d0-286">Phase two: Provision new VMs on Azure</span></span>

1.  <span data-ttu-id="643d0-287">A New-AzureRMVMConfig alapuló új virtuális gépek kiépítése alapjául szolgáló lemezképhez AcceleratedNetworking hello virtuális engedélyezve van az első lépése, rögzített virtuális merevlemez használatával hello:</span><span class="sxs-lookup"><span data-stu-id="643d0-287">Provision new VMs based with New-AzureRMVMConfig using hello base image VHD captured in phase one, with AcceleratedNetworking enabled on hello vNIC:</span></span>

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
    
    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
     -Name MyNic `
     -ResourceGroupName $RgName `
     -Location $Location `
     -SubnetId $Vnet.Subnets[0].Id `
     -PublicIpAddressId $Pip.Id `
     -EnableAcceleratedNetworking
    
    # Specify hello base image's VHD URI (from phase one step 5). 
    # Note: hello storage account of this base image vhd should have "Storage service encryption" disabled
    # See more from here: https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption
    # This is just an example URI, you will need tooreplace this when running this script
    $sourceUri="https://myexamplesa.blob.core.windows.net/vhds/CentOS73-Base-Test120170629111341.vhd" 

    # Specify a URI for hello location from which hello new image binary large object (BLOB) is copied toostart hello virtual machine. 
    # Must end with ".vhd" extension
    $OsDiskName = "MyOsDiskName.vhd" 
    $destOsDiskUri = "https://myexamplesa.blob.core.windows.net/vhds/" + $OsDiskName
    
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
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
    
    # Create hello virtual machine.    
    New-AzureRmVM `
     -ResourceGroupName $RgName `
     -Location $Location `
     -VM $VmConfig
    ```

2.  <span data-ttu-id="643d0-288">Után indítsa el a virtuális gépek, "lspci" hello VF eszköz ellenőrzi, és ellenőrizze hello Mellanox bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="643d0-288">After VMs boot up, check hello VF device by "lspci" and check hello Mellanox entry.</span></span> <span data-ttu-id="643d0-289">Például azt kell látnia hello lspci kimeneti ezt az elemet:</span><span class="sxs-lookup"><span data-stu-id="643d0-289">For example, we should see this item in hello lspci output:</span></span>
    
    ```
    0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
    ```
    
3.  <span data-ttu-id="643d0-290">Hello ragasztás parancsfájl által futtatása:</span><span class="sxs-lookup"><span data-stu-id="643d0-290">Run hello bonding script by:</span></span>

    ```bash
    sudo bondvf.sh
    ```

4.  <span data-ttu-id="643d0-291">Indítsa újra új virtuális gépek hello:</span><span class="sxs-lookup"><span data-stu-id="643d0-291">Reboot hello new VMs:</span></span>

    ```bash
    sudo reboot
    ```

<span data-ttu-id="643d0-292">virtuális gép hello bond0 konfigurálva és hello az elérését gyorsítja fel hálózati elérési út engedélyezve kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="643d0-292">hello VM should start with bond0 configured and hello Accelerated Networking path enabled.</span></span>  <span data-ttu-id="643d0-293">Futtatás `ifconfig` tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="643d0-293">Run `ifconfig` tooconfirm.</span></span>
