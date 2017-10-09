---
title: "aaaManage Azure fenntartott IP-cím (klasszikus) - PowerShell |} Microsoft Docs"
description: "Fenntartott IP-címek (klasszikus) megérteni, hogyan toomanage őket a PowerShell használatával."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 34652a55-3ab8-4c2d-8fb2-43684033b191
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
ms.openlocfilehash: c0a77b2ab8b1ab9bef6015c903eb735ea4358a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reserved-ip-addresses-classic"></a><span data-ttu-id="87c26-103">Fenntartott IP-címek (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="87c26-103">Reserved IP addresses (Classic)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="87c26-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="87c26-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="87c26-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="87c26-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="87c26-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="87c26-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="87c26-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="87c26-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="87c26-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="87c26-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

<span data-ttu-id="87c26-109">IP-címek az Azure-ban két kategóriába sorolhatók: dinamikus és fenntartott.</span><span class="sxs-lookup"><span data-stu-id="87c26-109">IP addresses in Azure fall into two categories: dynamic and reserved.</span></span> <span data-ttu-id="87c26-110">Azure által kezelt nyilvános IP-címek alapértelmezés szerint dinamikus.</span><span class="sxs-lookup"><span data-stu-id="87c26-110">Public IP addresses managed by Azure are dynamic by default.</span></span> <span data-ttu-id="87c26-111">Adott azt jelenti, hogy egy adott felhőalapú szolgáltatás (VIP) használt IP-cím hello vagy a virtuális gép vagy szerepkör példánya közvetlenül (ILPIP) módosítható idő tootime, ha az erőforrások állítsa le vagy leállt (felszabadított) tooaccess.</span><span class="sxs-lookup"><span data-stu-id="87c26-111">That means that hello IP address used for a given cloud service (VIP) or tooaccess a VM or role instance directly (ILPIP) can change from time tootime, when resources are shut down or stopped (deallocated).</span></span>

<span data-ttu-id="87c26-112">tooprevent IP-címek módosítása, foglalhat IP-címet.</span><span class="sxs-lookup"><span data-stu-id="87c26-112">tooprevent IP addresses from changing, you can reserve an IP address.</span></span> <span data-ttu-id="87c26-113">Fenntartott IP-címek csak virtuális IP-címhez, győződjön meg arról, hogy hello IP-cím hello felhőszolgáltatás marad hello azonos, még akkor is, mint az erőforrások állnak le, vagy nem (felszabadított) használható.</span><span class="sxs-lookup"><span data-stu-id="87c26-113">Reserved IPs can be used only as a VIP, ensuring that hello IP address for hello cloud service remains hello same, even as resources are shut down or stopped (deallocated).</span></span> <span data-ttu-id="87c26-114">Ezenkívül alakíthatja ki meglévő dinamikus IP-címek egy VIP tooa fenntartott IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="87c26-114">Furthermore, you can convert existing dynamic IPs used as a VIP tooa reserved IP address.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="87c26-115">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="87c26-115">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="87c26-116">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="87c26-116">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="87c26-117">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="87c26-117">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="87c26-118">Ismerje meg, hogy egy statikus nyilvános IP cím használatával tooreserve hello [Resource Manager üzembe helyezési modellben](virtual-network-ip-addresses-overview-arm.md).</span><span class="sxs-lookup"><span data-stu-id="87c26-118">Learn how tooreserve a static public IP address using hello [Resource Manager deployment model](virtual-network-ip-addresses-overview-arm.md).</span></span>

<span data-ttu-id="87c26-119">toolearn további információ az IP-címek az Azure-ban, olvassa el a hello [IP-címek](virtual-network-ip-addresses-overview-classic.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="87c26-119">toolearn more about IP addresses in Azure, read hello [IP addresses](virtual-network-ip-addresses-overview-classic.md) article.</span></span>

## <a name="when-do-i-need-a-reserved-ip"></a><span data-ttu-id="87c26-120">Mikor kell a foglalt IP-cím?</span><span class="sxs-lookup"><span data-stu-id="87c26-120">When do I need a reserved IP?</span></span>
* <span data-ttu-id="87c26-121">**Azt szeretné, amely hello IP tooensure az előfizetésében foglalt**.</span><span class="sxs-lookup"><span data-stu-id="87c26-121">**You want tooensure that hello IP is reserved in your subscription**.</span></span> <span data-ttu-id="87c26-122">Ha olyan IP-cím kiadása nem történik tooreserve semmilyen körülmények között az előfizetésből, foglalt nyilvános IP-címhez kell használnia.</span><span class="sxs-lookup"><span data-stu-id="87c26-122">If you want tooreserve an IP address that is not released from your subscription under any circumstance, you should use a reserved public IP.</span></span>  
* <span data-ttu-id="87c26-123">**Azt szeretné, hogy az IP-toostay a felhőalapú szolgáltatással is keresztben leállt, vagy állapota (VM) felszabadítása**.</span><span class="sxs-lookup"><span data-stu-id="87c26-123">**You want your IP toostay with your cloud service even across stopped or deallocated state (VMs)**.</span></span> <span data-ttu-id="87c26-124">Ha azt szeretné, hogy a szolgáltatás toobe elérésük IP-címet, amely nem változik, akkor is, ha a virtuális gépek hello a felhőalapú szolgáltatás le van állítva vagy állítsa le a (felszabadított).</span><span class="sxs-lookup"><span data-stu-id="87c26-124">If you want your service toobe accessed by using an IP address that doesn't change, even when VMs in hello cloud service are shut down or stop (deallocated).</span></span>
* <span data-ttu-id="87c26-125">**Azt szeretné, hogy a kimenő forgalom az Azure-ból egy előre jelezhető IP-címet használ tooensure**.</span><span class="sxs-lookup"><span data-stu-id="87c26-125">**You want tooensure that outbound traffic from Azure uses a predictable IP address**.</span></span> <span data-ttu-id="87c26-126">Előfordulhat, hogy a helyszíni konfigurált tűzfal tooallow csak érkező forgalom bizonyos IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="87c26-126">You may have your on-premises firewall configured tooallow only traffic from specific IP addresses.</span></span> <span data-ttu-id="87c26-127">Olyan IP-címet lefoglalásával hello forrás IP-cím tudja cím, és nem szükséges a tűzfalszabályok tooan IP-módosítása miatt tooupdate.</span><span class="sxs-lookup"><span data-stu-id="87c26-127">By reserving an IP, you know hello source IP address, and don't need tooupdate your firewall rules due tooan IP change.</span></span>

## <a name="faq"></a><span data-ttu-id="87c26-128">GYIK</span><span class="sxs-lookup"><span data-stu-id="87c26-128">FAQ</span></span>
1. <span data-ttu-id="87c26-129">Használható az összes Azure-szolgáltatások egy fenntartott IP-cím?</span><span class="sxs-lookup"><span data-stu-id="87c26-129">Can I use a reserved IP for all Azure services?</span></span> <br>
    <span data-ttu-id="87c26-130">Nem.</span><span class="sxs-lookup"><span data-stu-id="87c26-130">No.</span></span> <span data-ttu-id="87c26-131">Fenntartott IP-címek csak a virtuális gépek és virtuális IP-címhez keresztül közzétett felhő példány szerepköreit használható.</span><span class="sxs-lookup"><span data-stu-id="87c26-131">Reserved IPs can only be used for VMs and cloud service instance roles exposed through a VIP.</span></span>
2. <span data-ttu-id="87c26-132">Hány foglalt IP-cím is van?</span><span class="sxs-lookup"><span data-stu-id="87c26-132">How many reserved IPs can I have?</span></span> <br>
    <span data-ttu-id="87c26-133">További információkért lásd: hello [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.</span><span class="sxs-lookup"><span data-stu-id="87c26-133">For details, see hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>
3. <span data-ttu-id="87c26-134">Nem járnak a foglalt IP-cím?</span><span class="sxs-lookup"><span data-stu-id="87c26-134">Is there a charge for reserved IPs?</span></span> <br>
    <span data-ttu-id="87c26-135">Egyes esetekben.</span><span class="sxs-lookup"><span data-stu-id="87c26-135">Sometimes.</span></span> <span data-ttu-id="87c26-136">Díjszabása, lásd: hello [fenntartott IP cím díjszabás](http://go.microsoft.com/fwlink/?LinkID=398482) lap.</span><span class="sxs-lookup"><span data-stu-id="87c26-136">For pricing details, see hello [Reserved IP Address Pricing Details](http://go.microsoft.com/fwlink/?LinkID=398482) page.</span></span>
4. <span data-ttu-id="87c26-137">Hogyan foglaljon le egy IP-címet?</span><span class="sxs-lookup"><span data-stu-id="87c26-137">How do I reserve an IP address?</span></span> <br>
    <span data-ttu-id="87c26-138">A PowerShell segítségével, hello [Azure szolgáltatásfelügyelet REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx), vagy hello [Azure-portálon](https://portal.azure.com) tooreserve Azure-régióban IP-címet.</span><span class="sxs-lookup"><span data-stu-id="87c26-138">You can use PowerShell, hello [Azure Management REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx), or hello [Azure portal](https://portal.azure.com) tooreserve an IP address in an Azure region.</span></span> <span data-ttu-id="87c26-139">A fenntartott IP-cím társítva tooyour előfizetés.</span><span class="sxs-lookup"><span data-stu-id="87c26-139">A reserved IP address is associated tooyour subscription.</span></span>
5. <span data-ttu-id="87c26-140">Használhatok egy fenntartott IP-cím affinitáscsoport-alapú Vnetekhez?</span><span class="sxs-lookup"><span data-stu-id="87c26-140">Can I use a reserved IP with affinity group-based VNets?</span></span> <br>
    <span data-ttu-id="87c26-141">Nem.</span><span class="sxs-lookup"><span data-stu-id="87c26-141">No.</span></span> <span data-ttu-id="87c26-142">Fenntartott IP-címek csak a regionális Vnetek támogatottak.</span><span class="sxs-lookup"><span data-stu-id="87c26-142">Reserved IPs are only supported in regional VNets.</span></span> <span data-ttu-id="87c26-143">Fenntartott IP-címek nem támogatottak a Vnetek társított affinitáscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="87c26-143">Reserved IPs are not supported for VNets that are associated with affinity groups.</span></span> <span data-ttu-id="87c26-144">Egy VNet társít egy régiót vagy affinitáscsoportot kapcsolatos további információkért lásd: hello [regionális Vnetek és Affinitáscsoportok](virtual-networks-migrate-to-regional-vnet.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="87c26-144">For more information about associating a VNet with a region or affinity group, see hello [About Regional VNets and Affinity Groups](virtual-networks-migrate-to-regional-vnet.md) article.</span></span>

## <a name="manage-reserved-vips"></a><span data-ttu-id="87c26-145">Fenntartott virtuális IP-címek kezelése</span><span class="sxs-lookup"><span data-stu-id="87c26-145">Manage reserved VIPs</span></span>

<span data-ttu-id="87c26-146">Győződjön meg arról, hogy telepítette és konfigurálta a PowerShell hello lépések végrehajtásával a hello [telepítse és konfigurálja a PowerShell](/powershell/azure/overview) cikk.</span><span class="sxs-lookup"><span data-stu-id="87c26-146">Ensure you have installed and configured PowerShell by completing hello steps in hello [Install and configure PowerShell](/powershell/azure/overview) article.</span></span> 

<span data-ttu-id="87c26-147">Fenntartott IP-címek használata előtt hozzá kell adnia azt tooyour előfizetés.</span><span class="sxs-lookup"><span data-stu-id="87c26-147">Before you can use reserved IPs, you must add it tooyour subscription.</span></span> <span data-ttu-id="87c26-148">toocreate foglalt IP-cím hello készletből nyilvános IP-címek hello elérhető *USA középső RÉGIÓJA* helyet, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="87c26-148">toocreate a reserved IP from hello pool of public IP addresses available in hello *Central US* location, run hello following command:</span></span>

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
```

<span data-ttu-id="87c26-149">Figyelje meg, azonban, hogy nem adható meg mi IP folyamatban van fenntartva.</span><span class="sxs-lookup"><span data-stu-id="87c26-149">Notice, however, that you cannot specify what IP is being reserved.</span></span> <span data-ttu-id="87c26-150">tooview milyen IP-címek fenntartott az előfizetéshez, futtassa a következő PowerShell-paranccsal, hello és figyelje meg a hello értékeinek *ReservedIPName* és *cím*:</span><span class="sxs-lookup"><span data-stu-id="87c26-150">tooview what IP addresses are reserved in your subscription, run hello following PowerShell command, and notice hello values for *ReservedIPName* and *Address*:</span></span>

```powershell
Get-AzureReservedIP
```

<span data-ttu-id="87c26-151">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="87c26-151">Expected output:</span></span>

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

>[!NOTE]
><span data-ttu-id="87c26-152">Amikor egy fenntartott IP-cím létrehozása a PowerShell használatával, nem adhat meg egy erőforrás csoport toocreate hello szolgáltatás számára fenntartott IP-Címmel.</span><span class="sxs-lookup"><span data-stu-id="87c26-152">When you create a reserved IP address with PowerShell, you cannot specify a resource group toocreate hello reserved IP in.</span></span> <span data-ttu-id="87c26-153">Még egy erőforráscsoportot az Azure helyek *alapértelmezett-hálózat* automatikusan.</span><span class="sxs-lookup"><span data-stu-id="87c26-153">Azure places it into a resource group named *Default-Networking* automatically.</span></span> <span data-ttu-id="87c26-154">Ha hoz létre hello használó hello szolgáltatás számára fenntartott IP [Azure-portálon](http://portal.azure.com), megadhatja, hogy bármely erőforráscsoport választja.</span><span class="sxs-lookup"><span data-stu-id="87c26-154">If you create hello reserved IP using hello [Azure portal](http://portal.azure.com), you can specify any resource group you choose.</span></span> <span data-ttu-id="87c26-155">Eltérő erőforráscsoportban létrehozásakor hello szolgáltatás számára fenntartott IP *alapértelmezett-hálózat* azonban amikor hivatkozik hello foglalt IP-cím-parancsok, mint `Get-AzureReservedIP` és `Remove-AzureReservedIP`, hello neve hivatkozniakell*Csoportnevet erőforrás csoportnév fenntartott-ip*.</span><span class="sxs-lookup"><span data-stu-id="87c26-155">If you create hello reserved IP in a resource group other than *Default-Networking* however, whenever you reference hello reserved IP with commands such as `Get-AzureReservedIP` and `Remove-AzureReservedIP`, you must reference hello name *Group resource-group-name reserved-ip-name*.</span></span>  <span data-ttu-id="87c26-156">Például, ha egy nevű fenntartott IP-cím létrehozása *myReservedIP* erőforráscsoportban nevű *myResourceGroup*, hello szolgáltatás számára fenntartott IP mint hello nevét kell hivatkoznia *csoport myResourceGroup myReservedIP*.</span><span class="sxs-lookup"><span data-stu-id="87c26-156">For example, if you create a reserved IP named *myReservedIP* in a resource group named *myResourceGroup*, you must reference hello name of hello reserved IP as *Group myResourceGroup myReservedIP*.</span></span>   

<span data-ttu-id="87c26-157">Egy IP-cím foglalt, ha a leválasztást társított tooyour előfizetés amíg nem törli azokat.</span><span class="sxs-lookup"><span data-stu-id="87c26-157">Once an IP is reserved, it remains associated tooyour subscription until you delete it.</span></span> <span data-ttu-id="87c26-158">toodelete egy fenntartott IP-cím, a következő PowerShell-parancs futtatása hello:</span><span class="sxs-lookup"><span data-stu-id="87c26-158">toodelete a reserved IP, run hello following PowerShell command:</span></span>

```powershell
Remove-AzureReservedIP -ReservedIPName "MyReservedIP"
```

## <a name="reserve-hello-ip-address-of-an-existing-cloud-service"></a><span data-ttu-id="87c26-159">Foglaljon le egy meglévő felhőszolgáltatáshoz hello IP-címe</span><span class="sxs-lookup"><span data-stu-id="87c26-159">Reserve hello IP address of an existing cloud service</span></span>
<span data-ttu-id="87c26-160">Hello IP-címét egy meglévő felhőszolgáltatáshoz foglalhat hello hozzáadásával `-ServiceName` paraméter.</span><span class="sxs-lookup"><span data-stu-id="87c26-160">You can reserve hello IP address of an existing cloud service by adding hello `-ServiceName` parameter.</span></span> <span data-ttu-id="87c26-161">egy felhőalapú szolgáltatás tooreserve hello IP-címét *TestService* a hello *USA középső RÉGIÓJA* helyet, futtassa a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="87c26-161">tooreserve hello IP address of a cloud service *TestService* in hello *Central US* location, run hello following PowerShell command:</span></span>

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService
```

## <a name="associate-a-reserved-ip-tooa-new-cloud-service"></a><span data-ttu-id="87c26-162">A fenntartott IP-tooa új felhőalapú szolgáltatás társítása</span><span class="sxs-lookup"><span data-stu-id="87c26-162">Associate a reserved IP tooa new cloud service</span></span>
<span data-ttu-id="87c26-163">hello következő parancsfájlt hoz létre egy új fenntartott IP-cím, majd hozzárendeli tooa nevű új felhőalapú szolgáltatás *TestService*.</span><span class="sxs-lookup"><span data-stu-id="87c26-163">hello following script creates a new reserved IP, then associates it tooa new cloud service named *TestService*.</span></span>

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"
```

> [!NOTE]
> <span data-ttu-id="87c26-164">Ha egy fenntartott IP-toouse létrehozásához egy felhőalapú szolgáltatás, továbbra is hivatkozunk toohello VM használatával *VIP:&lt;portszám >* bejövő kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="87c26-164">When you create a reserved IP toouse with a cloud service, you still refer toohello VM by using *VIP:&lt;port number>* for inbound communication.</span></span> <span data-ttu-id="87c26-165">Ha olyan IP-címet nem jelenti azt toohello virtuális gép közvetlenül is elérheti.</span><span class="sxs-lookup"><span data-stu-id="87c26-165">Reserving an IP does not mean you can connect toohello VM directly.</span></span> <span data-ttu-id="87c26-166">hello foglalt IP-cím hozzá van rendelve toohello felhő adott virtuális gép már alkalmazva van hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="87c26-166">hello reserved IP is assigned toohello cloud service that hello VM has been deployed to.</span></span> <span data-ttu-id="87c26-167">Ha közvetlenül tooconnect tooa virtuális gép IP-címekként, rendelkezik tooconfigure olyan példányszintű nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="87c26-167">If you want tooconnect tooa VM by IP directly, you have tooconfigure an instance-level public IP.</span></span> <span data-ttu-id="87c26-168">Olyan példányszintű nyilvános IP-címet egy olyan nyilvános IP-cím (más néven egy ILPIP) közvetlenül hozzárendelt tooyour VM típusú.</span><span class="sxs-lookup"><span data-stu-id="87c26-168">An instance-level public IP is a type of public IP (called an ILPIP) that is assigned directly tooyour VM.</span></span> <span data-ttu-id="87c26-169">Nem lehet lefoglalni.</span><span class="sxs-lookup"><span data-stu-id="87c26-169">It cannot be reserved.</span></span> <span data-ttu-id="87c26-170">További információkért olvassa el a hello [példányszintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="87c26-170">For more information, read hello [Instance-level Public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) article.</span></span>
> 

## <a name="remove-a-reserved-ip-from-a-running-deployment"></a><span data-ttu-id="87c26-171">A foglalt IP-cím eltávolítása a futó központi telepítés</span><span class="sxs-lookup"><span data-stu-id="87c26-171">Remove a reserved IP from a running deployment</span></span>
<span data-ttu-id="87c26-172">fenntartott IP-cím tooremove tooa új felhőalapú szolgáltatás, futtassa a következő PowerShell-paranccsal hello hozzáadva:</span><span class="sxs-lookup"><span data-stu-id="87c26-172">tooremove a reserved IP added tooa new cloud service, run hello following PowerShell command:</span></span>

```powershell
Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService
```

> [!NOTE]
> <span data-ttu-id="87c26-173">A foglalt IP-cím eltávolítása a futó központi telepítés nem távolítja el hello foglalás az előfizetésből.</span><span class="sxs-lookup"><span data-stu-id="87c26-173">Removing a reserved IP from a running deployment does not remove hello reservation from your subscription.</span></span> <span data-ttu-id="87c26-174">Egyszerűen a előfizetés egy másik erőforrás használja hello IP toobe szabadít fel.</span><span class="sxs-lookup"><span data-stu-id="87c26-174">It simply frees hello IP toobe used by another resource in your subscription.</span></span>
> 

## <a name="associate-a-reserved-ip-tooa-running-deployment"></a><span data-ttu-id="87c26-175">A fenntartott IP-tooa-telepítést futtató társítása</span><span class="sxs-lookup"><span data-stu-id="87c26-175">Associate a reserved IP tooa running deployment</span></span>
<span data-ttu-id="87c26-176">hello következő parancsokat egy felhőalapú szolgáltatás létrehozása nevű *TestService2* nevű új virtuális gépen *TestVM2*.</span><span class="sxs-lookup"><span data-stu-id="87c26-176">hello following commands create a cloud service named *TestService2* with a new VM named *TestVM2*.</span></span> <span data-ttu-id="87c26-177">hello meglévő foglalt IP-cím, nevű *MyReservedIP* és társított toohello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="87c26-177">hello existing reserved IP named *MyReservedIP* is then associated toohello cloud service.</span></span>

```powershell
$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService2 -Location "Central US"

Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2
```

## <a name="associate-a-reserved-ip-tooa-cloud-service-by-using-a-service-configuration-file"></a><span data-ttu-id="87c26-178">Társítson egy fenntartott IP-tooa felhőalapú szolgáltatás a szolgáltatás konfigurációs fájl segítségével</span><span class="sxs-lookup"><span data-stu-id="87c26-178">Associate a reserved IP tooa cloud service by using a service configuration file</span></span>
<span data-ttu-id="87c26-179">A fenntartott IP-tooa felhőalapú szolgáltatás szolgáltatás (szolgáltatáskonfigurációs SÉMA) konfigurációs fájl használatával is társíthat.</span><span class="sxs-lookup"><span data-stu-id="87c26-179">You can also associate a reserved IP tooa cloud service by using a service configuration (CSCFG) file.</span></span> <span data-ttu-id="87c26-180">hello következő minta xml bemutatja, hogyan tooconfigure egy felhőalapú szolgáltatás toouse fenntartott virtuális IP-címhez nevű *MyReservedIP*:</span><span class="sxs-lookup"><span data-stu-id="87c26-180">hello following sample xml shows how tooconfigure a cloud service toouse a reserved VIP named *MyReservedIP*:</span></span>

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a><span data-ttu-id="87c26-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="87c26-181">Next steps</span></span>
* <span data-ttu-id="87c26-182">Megértéséhez hogyan [IP-címzés](virtual-network-ip-addresses-overview-classic.md) hello klasszikus üzembe helyezési modellel működik.</span><span class="sxs-lookup"><span data-stu-id="87c26-182">Understand how [IP addressing](virtual-network-ip-addresses-overview-classic.md) works in hello classic deployment model.</span></span>
* <span data-ttu-id="87c26-183">További tudnivalók [magánhálózati IP-címek fenntartott](virtual-networks-reserved-private-ip.md).</span><span class="sxs-lookup"><span data-stu-id="87c26-183">Learn about [reserved private IP addresses](virtual-networks-reserved-private-ip.md).</span></span>
* <span data-ttu-id="87c26-184">További tudnivalók [példány szint nyilvános IP (ILPIP) címek](virtual-networks-instance-level-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="87c26-184">Learn about [Instance Level Public IP (ILPIP) addresses](virtual-networks-instance-level-public-ip.md).</span></span>

