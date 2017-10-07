---
title: "aaaAzure példányszintű nyilvános IP-cím (klasszikus) címek |} Microsoft Docs"
description: "Példány szintű nyilvános IP (ILPIP) címek megérteni, hogyan toomanage őket a PowerShell használatával."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 07eef6ec-7dfe-4c4d-a2c2-be0abfb48ec5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
ms.openlocfilehash: 832143ee6fdd80b634e1cebfddc759a8cacda583
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="instance-level-public-ip-classic-overview"></a><span data-ttu-id="1804a-103">Példány nyilvános IP (klasszikus) áttekintése</span><span class="sxs-lookup"><span data-stu-id="1804a-103">Instance level public IP (Classic) overview</span></span>
<span data-ttu-id="1804a-104">Egy szint nyilvános IP (ILPIP) példány egy nyilvános IP-cím rendelhető közvetlenül tooa virtuális gép vagy Felhőszolgáltatás szerepkörpéldányt, nem pedig toohello felhőszolgáltatás, amely a virtuális gép vagy szerepkör példányát találhatók.</span><span class="sxs-lookup"><span data-stu-id="1804a-104">An instance level public IP (ILPIP) is a public IP address that you can assign directly tooa VM or Cloud Services role instance, rather than toohello cloud service that your VM or role instance reside in.</span></span> <span data-ttu-id="1804a-105">Egy ILPIP hello hello virtuális IP-cím (VIP) tooyour felhőszolgáltatás hozzárendelt hely nem veszi.</span><span class="sxs-lookup"><span data-stu-id="1804a-105">An ILPIP doesn’t take hello place of hello virtual IP (VIP) that is assigned tooyour cloud service.</span></span> <span data-ttu-id="1804a-106">Ehelyett használható tooconnect további IP-cím közvetlenül tooyour virtuális gép vagy szerepkör-példányt.</span><span class="sxs-lookup"><span data-stu-id="1804a-106">Rather, it’s an additional IP address that you can use tooconnect directly tooyour VM or role instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1804a-107">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1804a-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="1804a-108">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="1804a-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="1804a-109">A Microsoft azt javasolja, virtuális gépek erőforrás-kezelő létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1804a-109">Microsoft recommends creating VMs through Resource Manager.</span></span> <span data-ttu-id="1804a-110">Győződjön meg arról, hogy hogyan [IP-címek](virtual-network-ip-addresses-overview-classic.md) munkahelyi az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="1804a-110">Make sure you understand how [IP addresses](virtual-network-ip-addresses-overview-classic.md) work in Azure.</span></span>

![Különbség ILPIP és a VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

<span data-ttu-id="1804a-112">1. ábrán látható, VIP hello felhőalapú szolgáltatás hozzáfér, hello során egyes virtuális gépek általában hozzáférnek VIP:&lt;portszámát&gt;.</span><span class="sxs-lookup"><span data-stu-id="1804a-112">As shown in Figure 1, hello cloud service is accessed using a VIP, while hello individual VMs are normally accessed using VIP:&lt;port number&gt;.</span></span> <span data-ttu-id="1804a-113">Egy ILPIP tooa hozzárendelésével speciális virtuális Gépet, virtuális gép használható érhetők el közvetlenül az adott IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="1804a-113">By assigning an ILPIP tooa specific VM, that VM can be accessed directly using that IP address.</span></span>

<span data-ttu-id="1804a-114">Amikor az Azure-ban létrehoz egy felhőalapú szolgáltatás, megfelelő DNS A-rekordokat jönnek létre automatikusan tooallow toohello szolgáltatás egy teljesen minősített tartománynevét (FQDN), a tényleges VIP hello használata helyett.</span><span class="sxs-lookup"><span data-stu-id="1804a-114">When you create a cloud service in Azure, corresponding DNS A records are created automatically tooallow access toohello service through a fully qualified domain name (FQDN), instead of using hello actual VIP.</span></span> <span data-ttu-id="1804a-115">hello egy ILPIP, így access toohello virtuális gép vagy szerepkör-példány a teljes tartománynév alapján hello ILPIP helyett a zavartalan ugyanazt a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="1804a-115">hello same process happens for an ILPIP, allowing access toohello VM or role instance by FQDN instead of hello ILPIP.</span></span> <span data-ttu-id="1804a-116">Például ha nevű felhőalapú szolgáltatás létrehozása *contosoadservice*, és konfigurálja a webes szerepkör nevű *contosoweb* két példánnyal, A következő Azure regiszterekben hello hello példányok rögzíti:</span><span class="sxs-lookup"><span data-stu-id="1804a-116">For instance, if you create a cloud service named *contosoadservice*, and you configure a web role named *contosoweb* with two instances, Azure registers hello following A records for hello instances:</span></span>

* <span data-ttu-id="1804a-117">contosoweb\_IN_0.contosoadservice.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="1804a-117">contosoweb\_IN_0.contosoadservice.cloudapp.net</span></span>
* <span data-ttu-id="1804a-118">contosoweb\_IN_1.contosoadservice.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="1804a-118">contosoweb\_IN_1.contosoadservice.cloudapp.net</span></span> 

> [!NOTE]
> <span data-ttu-id="1804a-119">Minden virtuális gép vagy szerepkör-példány csak egy ILPIP rendelhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="1804a-119">You can assign only one ILPIP for each VM or role instance.</span></span> <span data-ttu-id="1804a-120">Előfizetésenként too5 ILPIPs mentése használhatja.</span><span class="sxs-lookup"><span data-stu-id="1804a-120">You can use up too5 ILPIPs per subscription.</span></span> <span data-ttu-id="1804a-121">ILPIPs több hálózati Adapterrel virtuális gépek esetén nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="1804a-121">ILPIPs are not supported for multi-NIC VMs.</span></span>
> 
> 

## <a name="why-would-i-request-an-ilpip"></a><span data-ttu-id="1804a-122">Miért kellene kérelem egy ILPIP?</span><span class="sxs-lookup"><span data-stu-id="1804a-122">Why would I request an ILPIP?</span></span>
<span data-ttu-id="1804a-123">Ha azt szeretné, hogy toobe képes tooconnect tooyour VM, vagy egy IP-cím szerint szerepkörpéldányt közvetlenül hozzárendelt tooit, ahelyett, hogy az hello felhőalapú szolgáltatás VIP:&lt;portszámát&gt;, egy ILPIP kérhetnek a virtuális gép vagy a szerepkör példánya.</span><span class="sxs-lookup"><span data-stu-id="1804a-123">If you want toobe able tooconnect tooyour VM or role instance by an IP address assigned directly tooit, rather than using hello cloud service VIP:&lt;port number&gt;, request an ILPIP for your VM or your role instance.</span></span>

* <span data-ttu-id="1804a-124">**Aktív FTP** -egy ILPIP tooa VM hozzárendelésével megkaphatja a forgalom a portokon.</span><span class="sxs-lookup"><span data-stu-id="1804a-124">**Active FTP** - By assigning an ILPIP tooa VM, it can receive traffic on any port.</span></span> <span data-ttu-id="1804a-125">Végpontok esetén nincs szükség virtuális gépek tooreceive forgalma hello.</span><span class="sxs-lookup"><span data-stu-id="1804a-125">Endpoints are not required for hello VM tooreceive traffic.</span></span>  <span data-ttu-id="1804a-126">(Https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview) [az FTP protokoll áttekintése] talál információt hello FTP protokollt.</span><span class="sxs-lookup"><span data-stu-id="1804a-126">See (https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview)[FTP Protocol Overview] for details on hello FTP protocol.</span></span>
* <span data-ttu-id="1804a-127">**Kimenő IP** - kimenő forgalmat a virtuális gép hello leképezve toohello ILPIP hello forrásként, és hello ILPIP egyedileg azonosítja a hello VM tooexternal entitásokat.</span><span class="sxs-lookup"><span data-stu-id="1804a-127">**Outbound IP** - Outbound traffic originating from hello VM is mapped toohello ILPIP as hello source and hello ILPIP uniquely identifies hello VM tooexternal entities.</span></span>

> [!NOTE]
> <span data-ttu-id="1804a-128">Elmúlt hello ILPIP-címnek hivatkozott tooas nyilvános IP (PIP)-cím volt.</span><span class="sxs-lookup"><span data-stu-id="1804a-128">In hello past, an ILPIP address was referred tooas a public IP (PIP) address.</span></span>
> 

## <a name="manage-an-ilpip-for-a-vm"></a><span data-ttu-id="1804a-129">A virtuális gép egy ILPIP kezelése</span><span class="sxs-lookup"><span data-stu-id="1804a-129">Manage an ILPIP for a VM</span></span>
<span data-ttu-id="1804a-130">a következő feladatok hello lehetővé teszik toocreate, hozzárendelése és eltávolítása a virtuális gépekről érkező ILPIPs:</span><span class="sxs-lookup"><span data-stu-id="1804a-130">hello following tasks enable you toocreate, assign, and remove ILPIPs from VMs:</span></span>

### <a name="how-toorequest-an-ilpip-during-vm-creation-using-powershell"></a><span data-ttu-id="1804a-131">Hogyan toorequest egy ILPIP virtuális gép létrehozása PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="1804a-131">How toorequest an ILPIP during VM creation using PowerShell</span></span>
<span data-ttu-id="1804a-132">a következő PowerShell-parancsfájl hello létrehoz egy felhőalapú szolgáltatás nevű *FTPService*, lekéri a lemezkép az Azure-ból, létrehoz egy nevű virtuális gép *FTPInstance* beolvasott hello lemezképpel beállítja hello VM toouse egy ILPIP és hello VM toohello új szolgáltatás hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="1804a-132">hello following PowerShell script creates a cloud service named *FTPService*, retrieves an image from Azure, creates a VM named *FTPInstance* using hello retrieved image, sets hello VM toouse an ILPIP, and adds hello VM toohello new service:</span></span>

```powershell
New-AzureService -ServiceName FTPService -Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"} `
New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"
```

### <a name="how-tooretrieve-ilpip-information-for-a-vm"></a><span data-ttu-id="1804a-133">Hogyan tooretrieve ILPIP információkat a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="1804a-133">How tooretrieve ILPIP information for a VM</span></span>
<span data-ttu-id="1804a-134">tooview hello ILPIP információi hello hello, előző parancsfájl létrehozott virtuális gép hello a következő PowerShell-parancs futtatása, és tekintse meg az hello értékeinek *PublicIPAddress* és *PublicIPName*:</span><span class="sxs-lookup"><span data-stu-id="1804a-134">tooview hello ILPIP information for hello VM created with hello previous script, run hello following PowerShell command and observe hello values for *PublicIPAddress* and *PublicIPName*:</span></span>

```powershell
Get-AzureVM -Name FTPInstance -ServiceName FTPService
```

<span data-ttu-id="1804a-135">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="1804a-135">Expected output:</span></span>
 
    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            :   Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

### <a name="how-tooremove-an-ilpip-from-a-vm"></a><span data-ttu-id="1804a-136">Hogyan tooremove egy ILPIP VM</span><span class="sxs-lookup"><span data-stu-id="1804a-136">How tooremove an ILPIP from a VM</span></span>
<span data-ttu-id="1804a-137">tooremove hello ILPIP toohello VM hello előző parancsfájlban, futtassa a következő PowerShell-paranccsal hello hozzáadva:</span><span class="sxs-lookup"><span data-stu-id="1804a-137">tooremove hello ILPIP added toohello VM in hello previous script, run hello following PowerShell command:</span></span>

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Remove-AzurePublicIP | Update-AzureVM
```

### <a name="how-tooadd-an-ilpip-tooan-existing-vm"></a><span data-ttu-id="1804a-138">Hogyan tooadd egy ILPIP tooan meglévő virtuális gép</span><span class="sxs-lookup"><span data-stu-id="1804a-138">How tooadd an ILPIP tooan existing VM</span></span>
<span data-ttu-id="1804a-139">tooadd egy ILPIP toohello előző, hello parancsfájl használatával létrehozott virtuális gép hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1804a-139">tooadd an ILPIP toohello VM created using hello script previous, run hello following command:</span></span>

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Set-AzurePublicIP -PublicIPName ftpip2 | Update-AzureVM
```

## <a name="manage-an-ilpip-for-a-cloud-services-role-instance"></a><span data-ttu-id="1804a-140">A Cloud Services szerepkör példány egy ILPIP kezelése</span><span class="sxs-lookup"><span data-stu-id="1804a-140">Manage an ILPIP for a Cloud Services role instance</span></span>

<span data-ttu-id="1804a-141">tooadd ILPIP tooa Cloud Services szerepkör példánya, teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1804a-141">tooadd an ILPIP tooa Cloud Services role instance, complete hello following steps:</span></span>

1. <span data-ttu-id="1804a-142">Hello .cscfg fájl tölthető hello lépéseit felhőszolgáltatás hello hello elvégzésével [tooConfigure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) cikk.</span><span class="sxs-lookup"><span data-stu-id="1804a-142">Download hello .cscfg file for hello cloud service by completing hello steps in hello [How tooConfigure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) article.</span></span>
2. <span data-ttu-id="1804a-143">Frissítés hello .cscfg fájl hello hozzáadásával `InstanceAddress` elemet.</span><span class="sxs-lookup"><span data-stu-id="1804a-143">Update hello .cscfg file by adding hello `InstanceAddress` element.</span></span> <span data-ttu-id="1804a-144">hello következő minta hozzáadása egy elnevezett ILPIP *MyPublicIP* nevű tooa szerepkörpéldányt *WebRole1*:</span><span class="sxs-lookup"><span data-stu-id="1804a-144">hello following sample adds an ILPIP named *MyPublicIP* tooa role instance named *WebRole1*:</span></span> 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ILPIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
          <ConfigurationSettings>
        <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
          </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <InstanceAddress roleName="WebRole1">
        <PublicIPs>
          <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>
    ```
3. <span data-ttu-id="1804a-145">Hello .cscfg fájl feltöltése a hello lépéseit felhőszolgáltatás hello hello elvégzésével [tooConfigure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) cikk.</span><span class="sxs-lookup"><span data-stu-id="1804a-145">Upload hello .cscfg file for hello cloud service by completing hello steps in hello [How tooConfigure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1804a-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1804a-146">Next steps</span></span>
* <span data-ttu-id="1804a-147">Megértéséhez hogyan [IP-címzés](virtual-network-ip-addresses-overview-classic.md) hello klasszikus üzembe helyezési modellel működik.</span><span class="sxs-lookup"><span data-stu-id="1804a-147">Understand how [IP addressing](virtual-network-ip-addresses-overview-classic.md) works in hello classic deployment model.</span></span>
* <span data-ttu-id="1804a-148">További tudnivalók [fenntartott IP-címek](virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="1804a-148">Learn about [Reserved IPs](virtual-networks-reserved-public-ip.md).</span></span>
