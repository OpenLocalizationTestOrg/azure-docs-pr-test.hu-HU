---
title: "Az Azure fenntartott IP-címek (klasszikus) - PowerShell kezelése |} Microsoft Docs"
description: "Fenntartott IP-címek (klasszikus) és a kezelésük módjával PowerShell használatával."
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
ms.openlocfilehash: 5e9c83cebec96c6bc8afd53b0c637d7af899746f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="reserved-ip-addresses-classic"></a><span data-ttu-id="12b26-103">Fenntartott IP-címek (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="12b26-103">Reserved IP addresses (Classic)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="12b26-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="12b26-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="12b26-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="12b26-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="12b26-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="12b26-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="12b26-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="12b26-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="12b26-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="12b26-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

<span data-ttu-id="12b26-109">IP-címek az Azure-ban két kategóriába sorolhatók: dinamikus és fenntartott.</span><span class="sxs-lookup"><span data-stu-id="12b26-109">IP addresses in Azure fall into two categories: dynamic and reserved.</span></span> <span data-ttu-id="12b26-110">Azure által kezelt nyilvános IP-címek alapértelmezés szerint dinamikus.</span><span class="sxs-lookup"><span data-stu-id="12b26-110">Public IP addresses managed by Azure are dynamic by default.</span></span> <span data-ttu-id="12b26-111">Ez azt jelenti, hogy az IP-címet használja, egy adott felhőalapú szolgáltatás (VIP) vagy a virtuális gépek eléréséhez, vagy közvetlenül szerepkörpéldányt (ILPIP) időről időre, ha az módosíthatja erőforrások állítsa le vagy leállt (felszabadított).</span><span class="sxs-lookup"><span data-stu-id="12b26-111">That means that the IP address used for a given cloud service (VIP) or to access a VM or role instance directly (ILPIP) can change from time to time, when resources are shut down or stopped (deallocated).</span></span>

<span data-ttu-id="12b26-112">IP-címek módosítása érdekében foglalhat IP-címet.</span><span class="sxs-lookup"><span data-stu-id="12b26-112">To prevent IP addresses from changing, you can reserve an IP address.</span></span> <span data-ttu-id="12b26-113">Fenntartott IP-címek csak virtuális IP-címhez, győződjön meg arról, hogy a felhőalapú szolgáltatás IP-címe ugyanaz marad, akár erőforrások állítsa le vagy leállt (felszabadított) használható.</span><span class="sxs-lookup"><span data-stu-id="12b26-113">Reserved IPs can be used only as a VIP, ensuring that the IP address for the cloud service remains the same, even as resources are shut down or stopped (deallocated).</span></span> <span data-ttu-id="12b26-114">Továbbá meglévő dinamikus IP-címek fenntartott IP-címekre VIP használt válthat.</span><span class="sxs-lookup"><span data-stu-id="12b26-114">Furthermore, you can convert existing dynamic IPs used as a VIP to a reserved IP address.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="12b26-115">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="12b26-115">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="12b26-116">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="12b26-116">This article covers using the classic deployment model.</span></span> <span data-ttu-id="12b26-117">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="12b26-117">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="12b26-118">Megtudhatja, hogyan lefoglalni egy statikus nyilvános IP cím használatával a [Resource Manager üzembe helyezési modellben](virtual-network-ip-addresses-overview-arm.md).</span><span class="sxs-lookup"><span data-stu-id="12b26-118">Learn how to reserve a static public IP address using the [Resource Manager deployment model](virtual-network-ip-addresses-overview-arm.md).</span></span>

<span data-ttu-id="12b26-119">IP-címek az Azure-ban kapcsolatos további tudnivalókért olvassa el a [IP-címek](virtual-network-ip-addresses-overview-classic.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="12b26-119">To learn more about IP addresses in Azure, read the [IP addresses](virtual-network-ip-addresses-overview-classic.md) article.</span></span>

## <a name="when-do-i-need-a-reserved-ip"></a><span data-ttu-id="12b26-120">Mikor kell a foglalt IP-cím?</span><span class="sxs-lookup"><span data-stu-id="12b26-120">When do I need a reserved IP?</span></span>
* <span data-ttu-id="12b26-121">**Ahhoz, hogy az IP-cím az előfizetésében foglalt**.</span><span class="sxs-lookup"><span data-stu-id="12b26-121">**You want to ensure that the IP is reserved in your subscription**.</span></span> <span data-ttu-id="12b26-122">Ha semmilyen körülmények között az előfizetésből nem kiadott IP-címet lefoglalni kívánt, foglalt nyilvános IP-címhez kell használnia.</span><span class="sxs-lookup"><span data-stu-id="12b26-122">If you want to reserve an IP address that is not released from your subscription under any circumstance, you should use a reserved public IP.</span></span>  
* <span data-ttu-id="12b26-123">**Azt szeretné, hogy az IP-marad a felhőalapú szolgáltatással, még akkor is keresztben leállt, vagy állapota (VM) felszabadítása**.</span><span class="sxs-lookup"><span data-stu-id="12b26-123">**You want your IP to stay with your cloud service even across stopped or deallocated state (VMs)**.</span></span> <span data-ttu-id="12b26-124">Ha azt szeretné, hogy a szolgáltatás elérése IP-cím használatával, amely nem változik, még ha a felhőszolgáltatásban található virtuális gépek a állnak le, vagy állítsa le a (felszabadított).</span><span class="sxs-lookup"><span data-stu-id="12b26-124">If you want your service to be accessed by using an IP address that doesn't change, even when VMs in the cloud service are shut down or stop (deallocated).</span></span>
* <span data-ttu-id="12b26-125">**Győződjön meg arról, hogy az Azure-ból kimenő forgalom használ egy előre jelezhető IP-címet szeretne**.</span><span class="sxs-lookup"><span data-stu-id="12b26-125">**You want to ensure that outbound traffic from Azure uses a predictable IP address**.</span></span> <span data-ttu-id="12b26-126">Előfordulhat, hogy a helyi tűzfal konfigurálva ahhoz, hogy csak adott IP-címekről érkező forgalmat.</span><span class="sxs-lookup"><span data-stu-id="12b26-126">You may have your on-premises firewall configured to allow only traffic from specific IP addresses.</span></span> <span data-ttu-id="12b26-127">Olyan IP-címet lefoglalásával tudja a forrás IP-címet, és nem szükséges frissíteni a tűzfalszabályok egy IP-módosítása miatt.</span><span class="sxs-lookup"><span data-stu-id="12b26-127">By reserving an IP, you know the source IP address, and don't need to update your firewall rules due to an IP change.</span></span>

## <a name="faq"></a><span data-ttu-id="12b26-128">GYIK</span><span class="sxs-lookup"><span data-stu-id="12b26-128">FAQ</span></span>
1. <span data-ttu-id="12b26-129">Használható az összes Azure-szolgáltatások egy fenntartott IP-cím?</span><span class="sxs-lookup"><span data-stu-id="12b26-129">Can I use a reserved IP for all Azure services?</span></span> <br>
    <span data-ttu-id="12b26-130">Nem.</span><span class="sxs-lookup"><span data-stu-id="12b26-130">No.</span></span> <span data-ttu-id="12b26-131">Fenntartott IP-címek csak a virtuális gépek és virtuális IP-címhez keresztül közzétett felhő példány szerepköreit használható.</span><span class="sxs-lookup"><span data-stu-id="12b26-131">Reserved IPs can only be used for VMs and cloud service instance roles exposed through a VIP.</span></span>
2. <span data-ttu-id="12b26-132">Hány foglalt IP-cím is van?</span><span class="sxs-lookup"><span data-stu-id="12b26-132">How many reserved IPs can I have?</span></span> <br>
    <span data-ttu-id="12b26-133">További információkért lásd: a [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.</span><span class="sxs-lookup"><span data-stu-id="12b26-133">For details, see the [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>
3. <span data-ttu-id="12b26-134">Nem járnak a foglalt IP-cím?</span><span class="sxs-lookup"><span data-stu-id="12b26-134">Is there a charge for reserved IPs?</span></span> <br>
    <span data-ttu-id="12b26-135">Egyes esetekben.</span><span class="sxs-lookup"><span data-stu-id="12b26-135">Sometimes.</span></span> <span data-ttu-id="12b26-136">Díjszabása, tekintse meg a [fenntartott IP cím díjszabás](http://go.microsoft.com/fwlink/?LinkID=398482) lap.</span><span class="sxs-lookup"><span data-stu-id="12b26-136">For pricing details, see the [Reserved IP Address Pricing Details](http://go.microsoft.com/fwlink/?LinkID=398482) page.</span></span>
4. <span data-ttu-id="12b26-137">Hogyan foglaljon le egy IP-címet?</span><span class="sxs-lookup"><span data-stu-id="12b26-137">How do I reserve an IP address?</span></span> <br>
    <span data-ttu-id="12b26-138">Használhatja a PowerShell, a [Azure szolgáltatásfelügyelet REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx), vagy a [Azure-portálon](https://portal.azure.com) Azure-régióban IP-címet lefoglalni.</span><span class="sxs-lookup"><span data-stu-id="12b26-138">You can use PowerShell, the [Azure Management REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx), or the [Azure portal](https://portal.azure.com) to reserve an IP address in an Azure region.</span></span> <span data-ttu-id="12b26-139">A fenntartott IP-cím az előfizetéshez tartozik.</span><span class="sxs-lookup"><span data-stu-id="12b26-139">A reserved IP address is associated to your subscription.</span></span>
5. <span data-ttu-id="12b26-140">Használhatok egy fenntartott IP-cím affinitáscsoport-alapú Vnetekhez?</span><span class="sxs-lookup"><span data-stu-id="12b26-140">Can I use a reserved IP with affinity group-based VNets?</span></span> <br>
    <span data-ttu-id="12b26-141">Nem.</span><span class="sxs-lookup"><span data-stu-id="12b26-141">No.</span></span> <span data-ttu-id="12b26-142">Fenntartott IP-címek csak a regionális Vnetek támogatottak.</span><span class="sxs-lookup"><span data-stu-id="12b26-142">Reserved IPs are only supported in regional VNets.</span></span> <span data-ttu-id="12b26-143">Fenntartott IP-címek nem támogatottak a Vnetek társított affinitáscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="12b26-143">Reserved IPs are not supported for VNets that are associated with affinity groups.</span></span> <span data-ttu-id="12b26-144">Egy VNet társít egy régiót vagy affinitáscsoportot kapcsolatos további információkért tekintse meg a [regionális Vnetek és Affinitáscsoportok](virtual-networks-migrate-to-regional-vnet.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="12b26-144">For more information about associating a VNet with a region or affinity group, see the [About Regional VNets and Affinity Groups](virtual-networks-migrate-to-regional-vnet.md) article.</span></span>

## <a name="manage-reserved-vips"></a><span data-ttu-id="12b26-145">Fenntartott virtuális IP-címek kezelése</span><span class="sxs-lookup"><span data-stu-id="12b26-145">Manage reserved VIPs</span></span>

<span data-ttu-id="12b26-146">Győződjön meg arról, hogy telepítette és konfigurálta a PowerShell; Ehhez hajtsa végre a lépéseket a [telepítse és konfigurálja a PowerShell](/powershell/azure/overview) cikk.</span><span class="sxs-lookup"><span data-stu-id="12b26-146">Ensure you have installed and configured PowerShell by completing the steps in the [Install and configure PowerShell](/powershell/azure/overview) article.</span></span> 

<span data-ttu-id="12b26-147">Fenntartott IP-címek használata előtt hozzá kell adnia az előfizetése.</span><span class="sxs-lookup"><span data-stu-id="12b26-147">Before you can use reserved IPs, you must add it to your subscription.</span></span> <span data-ttu-id="12b26-148">A foglalt IP-cím létrehozása a nyilvános IP-címkészletet érhető el a *USA középső RÉGIÓJA* helyét, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="12b26-148">To create a reserved IP from the pool of public IP addresses available in the *Central US* location, run the following command:</span></span>

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
```

<span data-ttu-id="12b26-149">Figyelje meg, azonban, hogy nem adható meg mi IP folyamatban van fenntartva.</span><span class="sxs-lookup"><span data-stu-id="12b26-149">Notice, however, that you cannot specify what IP is being reserved.</span></span> <span data-ttu-id="12b26-150">Milyen IP-cím van lefoglalva az előfizetés megtekintéséhez futtassa a következő PowerShell-parancsot, és figyelje meg értékeit *ReservedIPName* és *cím*:</span><span class="sxs-lookup"><span data-stu-id="12b26-150">To view what IP addresses are reserved in your subscription, run the following PowerShell command, and notice the values for *ReservedIPName* and *Address*:</span></span>

```powershell
Get-AzureReservedIP
```

<span data-ttu-id="12b26-151">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="12b26-151">Expected output:</span></span>

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
><span data-ttu-id="12b26-152">Amikor egy fenntartott IP-cím a PowerShell használatával hoz létre, nem adható meg egy erőforráscsoportot a foglalt IP-cím létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="12b26-152">When you create a reserved IP address with PowerShell, you cannot specify a resource group to create the reserved IP in.</span></span> <span data-ttu-id="12b26-153">Még egy erőforráscsoportot az Azure helyek *alapértelmezett-hálózat* automatikusan.</span><span class="sxs-lookup"><span data-stu-id="12b26-153">Azure places it into a resource group named *Default-Networking* automatically.</span></span> <span data-ttu-id="12b26-154">A fenntartott IP-történő létrehozásakor a [Azure-portálon](http://portal.azure.com), megadhatja, hogy bármely erőforráscsoport választja.</span><span class="sxs-lookup"><span data-stu-id="12b26-154">If you create the reserved IP using the [Azure portal](http://portal.azure.com), you can specify any resource group you choose.</span></span> <span data-ttu-id="12b26-155">Eltérő erőforráscsoportban létrehozásakor a foglalt IP-cím *alapértelmezett-hálózat* azonban amikor hivatkozik a foglalt IP-cím parancsok, mint `Get-AzureReservedIP` és `Remove-AzureReservedIP`, a nevet kell hivatkoznia  *Erőforrás-csoportnév fenntartott-ip-név csoport*.</span><span class="sxs-lookup"><span data-stu-id="12b26-155">If you create the reserved IP in a resource group other than *Default-Networking* however, whenever you reference the reserved IP with commands such as `Get-AzureReservedIP` and `Remove-AzureReservedIP`, you must reference the name *Group resource-group-name reserved-ip-name*.</span></span>  <span data-ttu-id="12b26-156">Például, ha egy nevű fenntartott IP-cím létrehozása *myReservedIP* erőforráscsoportban nevű *myResourceGroup*, kell hivatkoznia, mint a foglalt IP-cím neve *csoport myResourceGroup myReservedIP*.</span><span class="sxs-lookup"><span data-stu-id="12b26-156">For example, if you create a reserved IP named *myReservedIP* in a resource group named *myResourceGroup*, you must reference the name of the reserved IP as *Group myResourceGroup myReservedIP*.</span></span>   

<span data-ttu-id="12b26-157">Ha egy IP-cím foglalt, marad az előfizetéshez társított amíg nem törli azokat.</span><span class="sxs-lookup"><span data-stu-id="12b26-157">Once an IP is reserved, it remains associated to your subscription until you delete it.</span></span> <span data-ttu-id="12b26-158">A foglalt IP-cím törlése, futtassa a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="12b26-158">To delete a reserved IP, run the following PowerShell command:</span></span>

```powershell
Remove-AzureReservedIP -ReservedIPName "MyReservedIP"
```

## <a name="reserve-the-ip-address-of-an-existing-cloud-service"></a><span data-ttu-id="12b26-159">Foglaljon le egy meglévő felhőszolgáltatáshoz IP-címe</span><span class="sxs-lookup"><span data-stu-id="12b26-159">Reserve the IP address of an existing cloud service</span></span>
<span data-ttu-id="12b26-160">Az IP-címét egy meglévő felhőszolgáltatáshoz foglalhat hozzáadásával a `-ServiceName` paraméter.</span><span class="sxs-lookup"><span data-stu-id="12b26-160">You can reserve the IP address of an existing cloud service by adding the `-ServiceName` parameter.</span></span> <span data-ttu-id="12b26-161">A felhőalapú szolgáltatások IP-cím lefoglalása *TestService* a a *USA középső RÉGIÓJA* helyét, a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="12b26-161">To reserve the IP address of a cloud service *TestService* in the *Central US* location, run the following PowerShell command:</span></span>

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService
```

## <a name="associate-a-reserved-ip-to-a-new-cloud-service"></a><span data-ttu-id="12b26-162">Új felhőalapú szolgáltatás foglalt IP-cím hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="12b26-162">Associate a reserved IP to a new cloud service</span></span>
<span data-ttu-id="12b26-163">Az alábbi parancsfájlt hoz létre egy új fenntartott IP-cím, majd társítja azt nevű új felhőalapú szolgáltatás *TestService*.</span><span class="sxs-lookup"><span data-stu-id="12b26-163">The following script creates a new reserved IP, then associates it to a new cloud service named *TestService*.</span></span>

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"
```

> [!NOTE]
> <span data-ttu-id="12b26-164">Amikor létrehoz egy fenntartott IP-cím egy felhőalapú szolgáltatás használata, továbbra is hivatkozik a virtuális gép használatával *VIP:&lt;portszám >* bejövő kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="12b26-164">When you create a reserved IP to use with a cloud service, you still refer to the VM by using *VIP:&lt;port number>* for inbound communication.</span></span> <span data-ttu-id="12b26-165">Ha olyan IP-címet nem jelenti azt közvetlenül csatlakozzon a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="12b26-165">Reserving an IP does not mean you can connect to the VM directly.</span></span> <span data-ttu-id="12b26-166">A foglalt IP-cím hozzá van rendelve a felhőszolgáltatás, amely a virtuális gép már alkalmazva van.</span><span class="sxs-lookup"><span data-stu-id="12b26-166">The reserved IP is assigned to the cloud service that the VM has been deployed to.</span></span> <span data-ttu-id="12b26-167">Ha azt szeretné, a virtuális gépek IP-címekként közvetlenül kapcsolódni, hogy konfigurálnia olyan példányszintű nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="12b26-167">If you want to connect to a VM by IP directly, you have to configure an instance-level public IP.</span></span> <span data-ttu-id="12b26-168">Olyan példányszintű nyilvános IP-címet egy olyan nyilvános IP-cím (más néven egy ILPIP) közvetlenül a virtuális Géphez rendelt típusú.</span><span class="sxs-lookup"><span data-stu-id="12b26-168">An instance-level public IP is a type of public IP (called an ILPIP) that is assigned directly to your VM.</span></span> <span data-ttu-id="12b26-169">Nem lehet lefoglalni.</span><span class="sxs-lookup"><span data-stu-id="12b26-169">It cannot be reserved.</span></span> <span data-ttu-id="12b26-170">További információkért olvassa el a [példányszintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="12b26-170">For more information, read the [Instance-level Public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) article.</span></span>
> 

## <a name="remove-a-reserved-ip-from-a-running-deployment"></a><span data-ttu-id="12b26-171">A foglalt IP-cím eltávolítása a futó központi telepítés</span><span class="sxs-lookup"><span data-stu-id="12b26-171">Remove a reserved IP from a running deployment</span></span>
<span data-ttu-id="12b26-172">A foglalt IP-cím hozzá egy új felhőalapú szolgáltatás eltávolításához futtassa a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="12b26-172">To remove a reserved IP added to a new cloud service, run the following PowerShell command:</span></span>

```powershell
Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService
```

> [!NOTE]
> <span data-ttu-id="12b26-173">Egy futó telepítésből egy fenntartott IP-cím eltávolítása nem törli a Foglalás az előfizetésből.</span><span class="sxs-lookup"><span data-stu-id="12b26-173">Removing a reserved IP from a running deployment does not remove the reservation from your subscription.</span></span> <span data-ttu-id="12b26-174">Egyszerűen az előfizetésében szereplő más erőforrás által használandó IP szabadít fel.</span><span class="sxs-lookup"><span data-stu-id="12b26-174">It simply frees the IP to be used by another resource in your subscription.</span></span>
> 

## <a name="associate-a-reserved-ip-to-a-running-deployment"></a><span data-ttu-id="12b26-175">Egy futó telepítéshez foglalt IP-cím hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="12b26-175">Associate a reserved IP to a running deployment</span></span>
<span data-ttu-id="12b26-176">A következő parancsok nevű felhőalapú szolgáltatás létrehozása *TestService2* nevű új virtuális gépen *TestVM2*.</span><span class="sxs-lookup"><span data-stu-id="12b26-176">The following commands create a cloud service named *TestService2* with a new VM named *TestVM2*.</span></span> <span data-ttu-id="12b26-177">A meglévő foglalt IP-cím, nevű *MyReservedIP* áll majd a felhőszolgáltatáshoz társított.</span><span class="sxs-lookup"><span data-stu-id="12b26-177">The existing reserved IP named *MyReservedIP* is then associated to the cloud service.</span></span>

```powershell
$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService2 -Location "Central US"

Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2
```

## <a name="associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a><span data-ttu-id="12b26-178">A foglalt IP-cím egy felhőszolgáltatásra történő társíthatók a szolgáltatás konfigurációs fájl használatával</span><span class="sxs-lookup"><span data-stu-id="12b26-178">Associate a reserved IP to a cloud service by using a service configuration file</span></span>
<span data-ttu-id="12b26-179">A foglalt IP-cím egy felhőszolgáltatásra történő szolgáltatás (szolgáltatáskonfigurációs SÉMA) konfigurációs fájl használatával is társíthat.</span><span class="sxs-lookup"><span data-stu-id="12b26-179">You can also associate a reserved IP to a cloud service by using a service configuration (CSCFG) file.</span></span> <span data-ttu-id="12b26-180">Az alábbi minta XML-kód bemutatja, hogyan nevű fenntartott virtuális IP-címhez használandó felhőszolgáltatás konfigurálása *MyReservedIP*:</span><span class="sxs-lookup"><span data-stu-id="12b26-180">The following sample xml shows how to configure a cloud service to use a reserved VIP named *MyReservedIP*:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="12b26-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="12b26-181">Next steps</span></span>
* <span data-ttu-id="12b26-182">Megértéséhez hogyan [IP-címzés](virtual-network-ip-addresses-overview-classic.md) a klasszikus üzembe helyezési modellel működik.</span><span class="sxs-lookup"><span data-stu-id="12b26-182">Understand how [IP addressing](virtual-network-ip-addresses-overview-classic.md) works in the classic deployment model.</span></span>
* <span data-ttu-id="12b26-183">További tudnivalók [magánhálózati IP-címek fenntartott](virtual-networks-reserved-private-ip.md).</span><span class="sxs-lookup"><span data-stu-id="12b26-183">Learn about [reserved private IP addresses](virtual-networks-reserved-private-ip.md).</span></span>
* <span data-ttu-id="12b26-184">További tudnivalók [példány szint nyilvános IP (ILPIP) címek](virtual-networks-instance-level-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="12b26-184">Learn about [Instance Level Public IP (ILPIP) addresses](virtual-networks-instance-level-public-ip.md).</span></span>

