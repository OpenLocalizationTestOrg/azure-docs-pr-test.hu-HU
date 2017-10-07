---
title: "aaaStatic belső privát IP - Azure VM – klasszikus"
description: "Statikus belső IP-címek (immerzióban) ismertetése és hogyan toomanage őket"
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
ms.openlocfilehash: 5abe1c59f2f3ed19bcf56c269dfe57ac32d4f601
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-a-static-internal-private-ip-address-using-powershell-classic"></a><span data-ttu-id="86f5c-103">Hogyan tooset egy belső statikus magánhálózati IP cím PowerShell (klasszikus) használata</span><span class="sxs-lookup"><span data-stu-id="86f5c-103">How tooset a static internal private IP address using PowerShell (Classic)</span></span>
<span data-ttu-id="86f5c-104">A legtöbb esetben nincs szükség a virtuális gép toospecify statikus belső IP-címnek.</span><span class="sxs-lookup"><span data-stu-id="86f5c-104">In most cases, you won’t need toospecify a static internal IP address for your virtual machine.</span></span> <span data-ttu-id="86f5c-105">A virtuális hálózat virtuális gépek automatikusan fog kapni a belső IP-címnek megadott tartomány.</span><span class="sxs-lookup"><span data-stu-id="86f5c-105">VMs in a virtual network will automatically receive an internal IP address from a range that you specify.</span></span> <span data-ttu-id="86f5c-106">Azonban bizonyos esetekben egy statikus IP-címet ad meg egy adott virtuális gép teljesen logikus.</span><span class="sxs-lookup"><span data-stu-id="86f5c-106">But in certain cases, specifying a static IP address for a particular VM makes sense.</span></span> <span data-ttu-id="86f5c-107">Ha például a virtuális gép folyamatos toorun DNS vagy egy tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="86f5c-107">For example, if your VM is going toorun DNS or will be a domain controller.</span></span> <span data-ttu-id="86f5c-108">A statikus belső IP-cím hello VM akár keresztül stop/deprovision állapotban marad.</span><span class="sxs-lookup"><span data-stu-id="86f5c-108">A static internal IP address stays with hello VM even through a stop/deprovision state.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="86f5c-109">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="86f5c-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="86f5c-110">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="86f5c-110">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="86f5c-111">A Microsoft azt javasolja, hogy az új telepítések esetén használjon hello [Resource Manager üzembe helyezési modellben](virtual-networks-static-private-ip-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="86f5c-111">Microsoft recommends that most new deployments use hello [Resource Manager deployment model](virtual-networks-static-private-ip-arm-ps.md).</span></span>
> 
> 

## <a name="how-tooverify-if-a-specific-ip-address-is-available"></a><span data-ttu-id="86f5c-112">Hogyan tooverify, ha egy adott IP-cím áll rendelkezésre</span><span class="sxs-lookup"><span data-stu-id="86f5c-112">How tooverify if a specific IP address is available</span></span>
<span data-ttu-id="86f5c-113">Ha hello tooverify IP-cím *10.0.0.7* érhető el egy nevű vnetet *TestVnet*, futtassa a hello a következő PowerShell-parancsot, és ellenőrizze a hello értéke *IsAvailable*:</span><span class="sxs-lookup"><span data-stu-id="86f5c-113">tooverify if hello IP address *10.0.0.7* is available in a vnet named *TestVnet*, run hello following PowerShell command and verify hello value for *IsAvailable*:</span></span>

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

> [!NOTE]
> <span data-ttu-id="86f5c-114">Tootest hello parancs fent biztonságos környezetben kövesse hello irányelveinek [hozzon létre egy virtuális hálózatot (klasszikus)](virtual-networks-create-vnet-classic-pportal.md) toocreate egy nevű vnetet *TestVnet* , és ellenőrizze a hello használ  *10.0.0.0/8* címterének.</span><span class="sxs-lookup"><span data-stu-id="86f5c-114">If you want tootest hello command above in a safe environment follow hello guidelines in [Create a virtual network (classic)](virtual-networks-create-vnet-classic-pportal.md) toocreate a vnet named *TestVnet* and ensure it uses hello *10.0.0.0/8* address space.</span></span>
> 
> 

## <a name="how-toospecify-a-static-internal-ip-when-creating-a-vm"></a><span data-ttu-id="86f5c-115">Hogyan toospecify egy statikus belső IP-cím egy virtuális gép létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="86f5c-115">How toospecify a static internal IP when creating a VM</span></span>
<span data-ttu-id="86f5c-116">hello az alábbi PowerShell-parancsfájlt hoz létre egy új felhőalapú szolgáltatás nevű *TestService*, majd lemezkép lekéri az Azure-ból, majd létrehoz egy nevű virtuális gép *TestVM* hello új felhőszolgáltatásban beolvasott hello lemezkép használatával készletek hello VM toobe nevű alhálózat *alhálózat-1*, és beállítja a *10.0.0.7* , egy statikus belső IP-Címek hello VM:</span><span class="sxs-lookup"><span data-stu-id="86f5c-116">hello PowerShell script below creates a new cloud service named *TestService*, then retrieves an image from Azure, then creates a VM named *TestVM* in hello new cloud service using hello retrieved image, sets hello VM toobe in a subnet named *Subnet-1*, and sets *10.0.0.7* as a static internal IP for hello VM:</span></span>

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | Set-AzureSubnet –SubnetNames Subnet-1 `
    | Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
    | New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-tooretrieve-static-internal-ip-information-for-a-vm"></a><span data-ttu-id="86f5c-117">Hogyan tooretrieve statikus belső IP-információit a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="86f5c-117">How tooretrieve static internal IP information for a VM</span></span>
<span data-ttu-id="86f5c-118">tooview hello statikus belső IP-információit hello hello parancsfájl a fenti létrehozott virtuális gép hello a következő PowerShell-parancs futtatása, és tekintse meg az értékeket hello *IP-cím*:</span><span class="sxs-lookup"><span data-stu-id="86f5c-118">tooview hello static internal IP information for hello VM created with hello script above, run hello following PowerShell command and observe hello values for *IpAddress*:</span></span>

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

## <a name="how-tooremove-a-static-internal-ip-from-a-vm"></a><span data-ttu-id="86f5c-119">Hogyan tooremove egy statikus belső IP-cím egy virtuális gépről</span><span class="sxs-lookup"><span data-stu-id="86f5c-119">How tooremove a static internal IP from a VM</span></span>
<span data-ttu-id="86f5c-120">tooremove hello statikus belső IP-címet toohello VM hozzáadott hello parancsfájl fenti, futtassa a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="86f5c-120">tooremove hello static internal IP added toohello VM in hello script above, run hello following PowerShell command:</span></span>

    Get-AzureVM -ServiceName TestService -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM

## <a name="how-tooadd-a-static-internal-ip-tooan-existing-vm"></a><span data-ttu-id="86f5c-121">Hogyan tooadd statikus belső IP-tooan meglévő virtuális gép</span><span class="sxs-lookup"><span data-stu-id="86f5c-121">How tooadd a static internal IP tooan existing VM</span></span>
<span data-ttu-id="86f5c-122">a statikus belső IP toohello runt a fenti hello parancsfájl a következő parancs használatával létrehozott virtuális gép tooadd:</span><span class="sxs-lookup"><span data-stu-id="86f5c-122">tooadd a static internal IP toohello VM created using hello script above, runt he following command:</span></span>

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
    | Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
    | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="86f5c-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="86f5c-123">Next steps</span></span>
[<span data-ttu-id="86f5c-124">Fenntartott IP-cím</span><span class="sxs-lookup"><span data-stu-id="86f5c-124">Reserved IP</span></span>](virtual-networks-reserved-public-ip.md)

[<span data-ttu-id="86f5c-125">Példányszintű nyilvános IP-cím (ILPIP)</span><span class="sxs-lookup"><span data-stu-id="86f5c-125">Instance-Level Public IP (ILPIP)</span></span>](virtual-networks-instance-level-public-ip.md)

[<span data-ttu-id="86f5c-126">Fenntartott IP-cím REST API-k</span><span class="sxs-lookup"><span data-stu-id="86f5c-126">Reserved IP REST APIs</span></span>](https://msdn.microsoft.com/library/azure/dn722420.aspx)

