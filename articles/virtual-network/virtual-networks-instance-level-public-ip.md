---
title: "Az Azure példányszintű nyilvános IP-cím (klasszikus) címek |} Microsoft Docs"
description: "Példány szintű nyilvános IP (ILPIP) címek és a kezelésük módjával PowerShell használatával."
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
ms.openlocfilehash: 773043f2841ec7539b0d49357dec6bcb9f4f78a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="instance-level-public-ip-classic-overview"></a><span data-ttu-id="8c00c-103">Példány nyilvános IP (klasszikus) áttekintése</span><span class="sxs-lookup"><span data-stu-id="8c00c-103">Instance level public IP (Classic) overview</span></span>
<span data-ttu-id="8c00c-104">Egy szint nyilvános IP (ILPIP) példány nyilvános IP-címnek, amelyeket hozzárendelhet közvetlenül egy virtuális gép vagy Felhőszolgáltatás szerepkörpéldányt, nem pedig a felhőszolgáltatásba, amelyek tárolása a virtuális gép vagy szerepkör példányát.</span><span class="sxs-lookup"><span data-stu-id="8c00c-104">An instance level public IP (ILPIP) is a public IP address that you can assign directly to a VM or Cloud Services role instance, rather than to the cloud service that your VM or role instance reside in.</span></span> <span data-ttu-id="8c00c-105">Egy ILPIP az a virtuális IP-cím (VIP) a felhőalapú szolgáltatáshoz hozzárendelt hely nem veszi.</span><span class="sxs-lookup"><span data-stu-id="8c00c-105">An ILPIP doesn’t take the place of the virtual IP (VIP) that is assigned to your cloud service.</span></span> <span data-ttu-id="8c00c-106">Ehelyett olyan további IP-cím segítségével csatlakozzon közvetlenül a virtuális gép vagy szerepkör-példányához.</span><span class="sxs-lookup"><span data-stu-id="8c00c-106">Rather, it’s an additional IP address that you can use to connect directly to your VM or role instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8c00c-107">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8c00c-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="8c00c-108">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="8c00c-108">This article covers using the classic deployment model.</span></span> <span data-ttu-id="8c00c-109">A Microsoft azt javasolja, virtuális gépek erőforrás-kezelő létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8c00c-109">Microsoft recommends creating VMs through Resource Manager.</span></span> <span data-ttu-id="8c00c-110">Győződjön meg arról, hogy hogyan [IP-címek](virtual-network-ip-addresses-overview-classic.md) munkahelyi az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="8c00c-110">Make sure you understand how [IP addresses](virtual-network-ip-addresses-overview-classic.md) work in Azure.</span></span>

![Különbség ILPIP és a VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

<span data-ttu-id="8c00c-112">Az 1. ábrán látható, a felhőalapú szolgáltatás érhető el egy VIP használatával az egyes virtuális gépek általában hozzáférnek VIP:&lt;portszámát&gt;.</span><span class="sxs-lookup"><span data-stu-id="8c00c-112">As shown in Figure 1, the cloud service is accessed using a VIP, while the individual VMs are normally accessed using VIP:&lt;port number&gt;.</span></span> <span data-ttu-id="8c00c-113">Egy ILPIP rendel egy adott virtuális gép virtuális gép elérése közvetlenül az adott IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="8c00c-113">By assigning an ILPIP to a specific VM, that VM can be accessed directly using that IP address.</span></span>

<span data-ttu-id="8c00c-114">Amikor az Azure-ban létrehoz egy felhőalapú szolgáltatás, megfelelő DNS A-rekordokat jönnek létre automatikusan egy teljesen minősített tartománynevét (FQDN), a szolgáltatás eléréséhez a tényleges VIP használata helyett.</span><span class="sxs-lookup"><span data-stu-id="8c00c-114">When you create a cloud service in Azure, corresponding DNS A records are created automatically to allow access to the service through a fully qualified domain name (FQDN), instead of using the actual VIP.</span></span> <span data-ttu-id="8c00c-115">Ugyanazt a folyamatot a egy ILPIP, amely hozzáférést biztosít a virtuális gép vagy szerepkör példány helyett a ILPIP teljes tartománynév alapján történik.</span><span class="sxs-lookup"><span data-stu-id="8c00c-115">The same process happens for an ILPIP, allowing access to the VM or role instance by FQDN instead of the ILPIP.</span></span> <span data-ttu-id="8c00c-116">Például ha nevű felhőalapú szolgáltatás létrehozása *contosoadservice*, és konfigurálja a webes szerepkör nevű *contosoweb* két példánnyal, Azure regisztrálja a következő A rekordok a példányok:</span><span class="sxs-lookup"><span data-stu-id="8c00c-116">For instance, if you create a cloud service named *contosoadservice*, and you configure a web role named *contosoweb* with two instances, Azure registers the following A records for the instances:</span></span>

* <span data-ttu-id="8c00c-117">contosoweb\_IN_0.contosoadservice.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="8c00c-117">contosoweb\_IN_0.contosoadservice.cloudapp.net</span></span>
* <span data-ttu-id="8c00c-118">contosoweb\_IN_1.contosoadservice.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="8c00c-118">contosoweb\_IN_1.contosoadservice.cloudapp.net</span></span> 

> [!NOTE]
> <span data-ttu-id="8c00c-119">Minden virtuális gép vagy szerepkör-példány csak egy ILPIP rendelhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="8c00c-119">You can assign only one ILPIP for each VM or role instance.</span></span> <span data-ttu-id="8c00c-120">Előfizetésenként legfeljebb 5 ILPIPs is használhatja.</span><span class="sxs-lookup"><span data-stu-id="8c00c-120">You can use up to 5 ILPIPs per subscription.</span></span> <span data-ttu-id="8c00c-121">ILPIPs több hálózati Adapterrel virtuális gépek esetén nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="8c00c-121">ILPIPs are not supported for multi-NIC VMs.</span></span>
> 
> 

## <a name="why-would-i-request-an-ilpip"></a><span data-ttu-id="8c00c-122">Miért kellene kérelem egy ILPIP?</span><span class="sxs-lookup"><span data-stu-id="8c00c-122">Why would I request an ILPIP?</span></span>
<span data-ttu-id="8c00c-123">Ha szeretne csatlakozni a virtuális gép vagy szerepkör példányához által közvetlenül hozzárendelt IP-címet, ahelyett, hogy az a felhőalapú szolgáltatás VIP:&lt;portszámát&gt;, egy ILPIP kérhetnek a virtuális gép vagy a szerepkör példánya.</span><span class="sxs-lookup"><span data-stu-id="8c00c-123">If you want to be able to connect to your VM or role instance by an IP address assigned directly to it, rather than using the cloud service VIP:&lt;port number&gt;, request an ILPIP for your VM or your role instance.</span></span>

* <span data-ttu-id="8c00c-124">**Aktív FTP** -egy ILPIP rendel a virtuális gépek, megkaphatja a forgalom a portokon.</span><span class="sxs-lookup"><span data-stu-id="8c00c-124">**Active FTP** - By assigning an ILPIP to a VM, it can receive traffic on any port.</span></span> <span data-ttu-id="8c00c-125">Végpontok esetén nincs szükség a virtuális gép forgalom fogadására.</span><span class="sxs-lookup"><span data-stu-id="8c00c-125">Endpoints are not required for the VM to receive traffic.</span></span>  <span data-ttu-id="8c00c-126">Az FTP protokoll részleteit itt találja: (https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview) [az FTP protokoll áttekintése].</span><span class="sxs-lookup"><span data-stu-id="8c00c-126">See (https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview)[FTP Protocol Overview] for details on the FTP protocol.</span></span>
* <span data-ttu-id="8c00c-127">**Kimenő IP** - kimenő forgalmat a virtuális gép forrásaként ILPIP van leképezve, és a ILPIP egyedileg azonosítja a virtuális Gépet, a külső részére.</span><span class="sxs-lookup"><span data-stu-id="8c00c-127">**Outbound IP** - Outbound traffic originating from the VM is mapped to the ILPIP as the source and the ILPIP uniquely identifies the VM to external entities.</span></span>

> [!NOTE]
> <span data-ttu-id="8c00c-128">A múltban ILPIP címnek néven ismert nyilvános IP (PIP)-cím.</span><span class="sxs-lookup"><span data-stu-id="8c00c-128">In the past, an ILPIP address was referred to as a public IP (PIP) address.</span></span>
> 

## <a name="manage-an-ilpip-for-a-vm"></a><span data-ttu-id="8c00c-129">A virtuális gép egy ILPIP kezelése</span><span class="sxs-lookup"><span data-stu-id="8c00c-129">Manage an ILPIP for a VM</span></span>
<span data-ttu-id="8c00c-130">A következő feladatok lehetővé teszik a létrehozása, hozzárendelése és ILPIPs eltávolítása a virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="8c00c-130">The following tasks enable you to create, assign, and remove ILPIPs from VMs:</span></span>

### <a name="how-to-request-an-ilpip-during-vm-creation-using-powershell"></a><span data-ttu-id="8c00c-131">Arról, hogyan kérhet egy ILPIP virtuális gép létrehozása PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="8c00c-131">How to request an ILPIP during VM creation using PowerShell</span></span>
<span data-ttu-id="8c00c-132">A következő PowerShell-parancsfájl létrehoz egy felhőalapú szolgáltatás nevű *FTPService*, lekéri a lemezkép az Azure-ból, létrehoz egy nevű virtuális gép *FTPInstance* a beolvasott kép használatával beállítja a virtuális gép egy ILPIP használatára, és hozzáadja a virtuális Gépet az új szolgáltatáshoz:</span><span class="sxs-lookup"><span data-stu-id="8c00c-132">The following PowerShell script creates a cloud service named *FTPService*, retrieves an image from Azure, creates a VM named *FTPInstance* using the retrieved image, sets the VM to use an ILPIP, and adds the VM to the new service:</span></span>

```powershell
New-AzureService -ServiceName FTPService -Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"} `
New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"
```

### <a name="how-to-retrieve-ilpip-information-for-a-vm"></a><span data-ttu-id="8c00c-133">Hogyan lehet lekérni a virtuális gépek ILPIP információk</span><span class="sxs-lookup"><span data-stu-id="8c00c-133">How to retrieve ILPIP information for a VM</span></span>
<span data-ttu-id="8c00c-134">Az előző parancsfájl létrehozza a virtuális gép ILPIP információk megtekintéséhez futtassa a következő PowerShell-parancsot, és tekintse meg az értékeit *PublicIPAddress* és *PublicIPName*:</span><span class="sxs-lookup"><span data-stu-id="8c00c-134">To view the ILPIP information for the VM created with the previous script, run the following PowerShell command and observe the values for *PublicIPAddress* and *PublicIPName*:</span></span>

```powershell
Get-AzureVM -Name FTPInstance -ServiceName FTPService
```

<span data-ttu-id="8c00c-135">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="8c00c-135">Expected output:</span></span>
 
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

### <a name="how-to-remove-an-ilpip-from-a-vm"></a><span data-ttu-id="8c00c-136">Egy virtuális Gépet egy ILPIP eltávolítása</span><span class="sxs-lookup"><span data-stu-id="8c00c-136">How to remove an ILPIP from a VM</span></span>
<span data-ttu-id="8c00c-137">Az előző parancsprogramban VM hozzáadott ILPIP eltávolításához futtassa a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="8c00c-137">To remove the ILPIP added to the VM in the previous script, run the following PowerShell command:</span></span>

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Remove-AzurePublicIP | Update-AzureVM
```

### <a name="how-to-add-an-ilpip-to-an-existing-vm"></a><span data-ttu-id="8c00c-138">Egy ILPIP hozzáadása egy meglévő virtuális Gépen</span><span class="sxs-lookup"><span data-stu-id="8c00c-138">How to add an ILPIP to an existing VM</span></span>
<span data-ttu-id="8c00c-139">Egy ILPIP hozzáadása a virtuális gép létrehozása az előző parancsfájl használatával, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="8c00c-139">To add an ILPIP to the VM created using the script previous, run the following command:</span></span>

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Set-AzurePublicIP -PublicIPName ftpip2 | Update-AzureVM
```

## <a name="manage-an-ilpip-for-a-cloud-services-role-instance"></a><span data-ttu-id="8c00c-140">A Cloud Services szerepkör példány egy ILPIP kezelése</span><span class="sxs-lookup"><span data-stu-id="8c00c-140">Manage an ILPIP for a Cloud Services role instance</span></span>

<span data-ttu-id="8c00c-141">A Cloud Services szerepkör példánya egy ILPIP hozzáadásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8c00c-141">To add an ILPIP to a Cloud Services role instance, complete the following steps:</span></span>

1. <span data-ttu-id="8c00c-142">Töltse le a .cscfg fájlban, a felhőszolgáltatás; Ehhez hajtsa végre a lépéseket a [felhőalapú szolgáltatások konfigurálása](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) cikk.</span><span class="sxs-lookup"><span data-stu-id="8c00c-142">Download the .cscfg file for the cloud service by completing the steps in the [How to Configure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) article.</span></span>
2. <span data-ttu-id="8c00c-143">Frissítse a .cscfg fájlban adja hozzá a `InstanceAddress` elemet.</span><span class="sxs-lookup"><span data-stu-id="8c00c-143">Update the .cscfg file by adding the `InstanceAddress` element.</span></span> <span data-ttu-id="8c00c-144">Az alábbi minta hozzáadása egy elnevezett ILPIP *MyPublicIP* egy szerepkörpéldányt nevű *WebRole1*:</span><span class="sxs-lookup"><span data-stu-id="8c00c-144">The following sample adds an ILPIP named *MyPublicIP* to a role instance named *WebRole1*:</span></span> 

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
3. <span data-ttu-id="8c00c-145">Töltse fel a .cscfg fájlban, a felhőszolgáltatás; Ehhez hajtsa végre a lépéseket a [felhőalapú szolgáltatások konfigurálása](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) cikk.</span><span class="sxs-lookup"><span data-stu-id="8c00c-145">Upload the .cscfg file for the cloud service by completing the steps in the [How to Configure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c00c-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8c00c-146">Next steps</span></span>
* <span data-ttu-id="8c00c-147">Megértéséhez hogyan [IP-címzés](virtual-network-ip-addresses-overview-classic.md) a klasszikus üzembe helyezési modellel működik.</span><span class="sxs-lookup"><span data-stu-id="8c00c-147">Understand how [IP addressing](virtual-network-ip-addresses-overview-classic.md) works in the classic deployment model.</span></span>
* <span data-ttu-id="8c00c-148">További tudnivalók [fenntartott IP-címek](virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="8c00c-148">Learn about [Reserved IPs](virtual-networks-reserved-public-ip.md).</span></span>
