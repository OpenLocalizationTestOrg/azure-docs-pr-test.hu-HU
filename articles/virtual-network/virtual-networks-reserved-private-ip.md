---
title: "Statikus belső privát IP - Azure VM – klasszikus"
description: "Statikus belső IP-címek (immerzióban) és a kezelésük módjával ismertetése"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 93444c6f-af1b-41f8-a035-77f5c0302bf0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: jdial
ms.openlocfilehash: cf9ee59ca4e44ed01836c2efb1f4df5f073bf6e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a><span data-ttu-id="5c894-103">Hogyan kell beállítani egy statikus belső magánhálózati IP-cím (klasszikus) PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="5c894-103">How to set a static internal private IP address using PowerShell (Classic)</span></span>
<span data-ttu-id="5c894-104">A legtöbb esetben nem kell a virtuális gép statikus belső IP-címet megadni.</span><span class="sxs-lookup"><span data-stu-id="5c894-104">In most cases, you won’t need to specify a static internal IP address for your virtual machine.</span></span> <span data-ttu-id="5c894-105">A virtuális hálózat virtuális gépek automatikusan fog kapni a belső IP-címnek megadott tartomány.</span><span class="sxs-lookup"><span data-stu-id="5c894-105">VMs in a virtual network will automatically receive an internal IP address from a range that you specify.</span></span> <span data-ttu-id="5c894-106">Azonban bizonyos esetekben egy statikus IP-címet ad meg egy adott virtuális gép teljesen logikus.</span><span class="sxs-lookup"><span data-stu-id="5c894-106">But in certain cases, specifying a static IP address for a particular VM makes sense.</span></span> <span data-ttu-id="5c894-107">Ha például a virtuális Gépet DNS futtatni fogja vagy egy tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="5c894-107">For example, if your VM is going to run DNS or will be a domain controller.</span></span> <span data-ttu-id="5c894-108">Egy statikus belső IP-címet a virtuális gép akár keresztül stop/deprovision állapotban marad.</span><span class="sxs-lookup"><span data-stu-id="5c894-108">A static internal IP address stays with the VM even through a stop/deprovision state.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="5c894-109">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5c894-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="5c894-110">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="5c894-110">This article covers using the classic deployment model.</span></span> <span data-ttu-id="5c894-111">A Microsoft azt javasolja, hogy az új telepítések esetén használja a [Resource Manager üzembe helyezési modellben](virtual-networks-static-private-ip-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="5c894-111">Microsoft recommends that most new deployments use the [Resource Manager deployment model](virtual-networks-static-private-ip-arm-ps.md).</span></span>
> 
> 

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a><span data-ttu-id="5c894-112">Ha rendelkezésére áll-e egy adott IP-cím ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="5c894-112">How to verify if a specific IP address is available</span></span>
<span data-ttu-id="5c894-113">Ha ellenőrizni az IP-cím *10.0.0.7* érhető el egy nevű vnetet *TestVnet*, futtassa a következő PowerShell-parancsot, és ellenőrizze a következő *IsAvailable*:</span><span class="sxs-lookup"><span data-stu-id="5c894-113">To verify if the IP address *10.0.0.7* is available in a vnet named *TestVnet*, run the following PowerShell command and verify the value for *IsAvailable*:</span></span>

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

> [!NOTE]
> <span data-ttu-id="5c894-114">Ha a fenti parancs teszteléséhez biztonságos környezetben útmutatását [hozzon létre egy virtuális hálózatot (klasszikus)](virtual-networks-create-vnet-classic-pportal.md) hozhat létre egy vnetet nevű *TestVnet* , és győződjön meg arról, használja a *10.0.0.0/8*  címterének.</span><span class="sxs-lookup"><span data-stu-id="5c894-114">If you want to test the command above in a safe environment follow the guidelines in [Create a virtual network (classic)](virtual-networks-create-vnet-classic-pportal.md) to create a vnet named *TestVnet* and ensure it uses the *10.0.0.0/8* address space.</span></span>
> 
> 

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a><span data-ttu-id="5c894-115">Egy statikus belső IP-cím megadása a virtuális gép létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="5c894-115">How to specify a static internal IP when creating a VM</span></span>
<span data-ttu-id="5c894-116">Az alábbi PowerShell-parancsfájlt hoz létre egy új felhőalapú szolgáltatás nevű *TestService*, majd lemezkép lekéri az Azure-ból, majd létrehoz egy nevű virtuális gép *TestVM* a beolvasott kép használatával új felhőalapú szolgáltatás, úgy állítja be a Nevű alhálózat virtuális Gépet *alhálózat-1*, és beállítja a *10.0.0.7* , egy statikus belső IP-cím a virtuális gép számára:</span><span class="sxs-lookup"><span data-stu-id="5c894-116">The PowerShell script below creates a new cloud service named *TestService*, then retrieves an image from Azure, then creates a VM named *TestVM* in the new cloud service using the retrieved image, sets the VM to be in a subnet named *Subnet-1*, and sets *10.0.0.7* as a static internal IP for the VM:</span></span>

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | Set-AzureSubnet –SubnetNames Subnet-1 `
    | Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
    | New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a><span data-ttu-id="5c894-117">Hogyan lehet lekérni a virtuális gép statikus belső IP-információit</span><span class="sxs-lookup"><span data-stu-id="5c894-117">How to retrieve static internal IP information for a VM</span></span>
<span data-ttu-id="5c894-118">A fenti parancsfájl létrehozza a virtuális gép statikus belső IP-információit megtekintéséhez futtassa a következő PowerShell-parancsot, és tekintse meg az értékeit *IP-cím*:</span><span class="sxs-lookup"><span data-stu-id="5c894-118">To view the static internal IP information for the VM created with the script above, run the following PowerShell command and observe the values for *IpAddress*:</span></span>

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a><span data-ttu-id="5c894-119">Egy statikus belső IP-cím eltávolítása a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="5c894-119">How to remove a static internal IP from a VM</span></span>
<span data-ttu-id="5c894-120">A statikus belső IP-címet a fenti parancsprogramban VM hozzáadott eltávolításához futtassa a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="5c894-120">To remove the static internal IP added to the VM in the script above, run the following PowerShell command:</span></span>

    Get-AzureVM -ServiceName TestService -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a><span data-ttu-id="5c894-121">Egy statikus belső IP-cím hozzáadása egy meglévő virtuális Gépen</span><span class="sxs-lookup"><span data-stu-id="5c894-121">How to add a static internal IP to an existing VM</span></span>
<span data-ttu-id="5c894-122">Hozzáadása egy belső statikus IP-cím kötése a virtuális gép létre a parancsfájl a fent runt a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="5c894-122">To add a static internal IP to the VM created using the script above, runt he following command:</span></span>

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
    | Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
    | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="5c894-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c894-123">Next steps</span></span>
[<span data-ttu-id="5c894-124">Fenntartott IP-cím</span><span class="sxs-lookup"><span data-stu-id="5c894-124">Reserved IP</span></span>](virtual-networks-reserved-public-ip.md)

[<span data-ttu-id="5c894-125">Példányszintű nyilvános IP-cím (ILPIP)</span><span class="sxs-lookup"><span data-stu-id="5c894-125">Instance-Level Public IP (ILPIP)</span></span>](virtual-networks-instance-level-public-ip.md)

[<span data-ttu-id="5c894-126">Fenntartott IP-cím REST API-k</span><span class="sxs-lookup"><span data-stu-id="5c894-126">Reserved IP REST APIs</span></span>](https://msdn.microsoft.com/library/azure/dn722420.aspx)

