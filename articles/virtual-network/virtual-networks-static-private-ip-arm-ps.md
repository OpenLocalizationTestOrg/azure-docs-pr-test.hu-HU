---
title: "virtuális gépek - Azure PowerShell aaaConfigure magánhálózati IP-címek |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure privát IP-címek a virtuális gépek PowerShell használatával."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: d5f18929-15e3-40a2-9ee3-8188bc248ed8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4a3eb67de583e08208fcab40de1c2a8a9b65618c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-powershell"></a><span data-ttu-id="1b2aa-103">PowerShell használatával virtuális gépek magánhálózati IP-címek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1b2aa-103">Configure private IP addresses for a virtual machine using PowerShell</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

<span data-ttu-id="1b2aa-104">Az Azure két üzemi modellel rendelkezik, az Azure Resource Managerrel és a klasszikussal.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="1b2aa-105">A Microsoft azt javasolja, hello Resource Manager üzembe helyezési modellben erőforrásoknak létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="1b2aa-106">hello arról toolearn hello hello két modellek közötti különbséget olvasási [megértéséhez Azure üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span> <span data-ttu-id="1b2aa-107">Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-107">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="1b2aa-108">Emellett [statikus magánhálózati IP-cím hello klasszikus üzembe helyezési modellel kezelése](virtual-networks-static-private-ip-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="1b2aa-108">You can also [manage static private IP address in hello classic deployment model](virtual-networks-static-private-ip-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="1b2aa-109">hello minta az alábbi parancsok várt már létrehozott egy egyszerű környezetben PowerShell fenti hello forgatókönyv alapján.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-109">hello sample PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="1b2aa-110">Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre leírt tesztkörnyezet hello [hozhat létre egy vnetet](virtual-networks-create-vnet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="1b2aa-110">If you want toorun hello commands as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-arm-ps.md).</span></span>

## <a name="create-a-vm-with-a-static-private-ip-address"></a><span data-ttu-id="1b2aa-111">Statikus magánhálózati IP-címmel rendelkező virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="1b2aa-111">Create a VM with a static private IP address</span></span>
<span data-ttu-id="1b2aa-112">toocreate nevű virtuális gép *DNS01* a hello *előtér* egy vnet nevű alhálózat *TestVNet* statikus magánhálózati IP-címe a *192.168.1.101*, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1b2aa-112">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow hello steps below:</span></span>

1. <span data-ttu-id="1b2aa-113">Hello tárfiókot, hely, erőforráscsoport és használt hitelesítő adatok toobe változók megadása.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-113">Set variables for hello storage account, location, resource group, and credentials toobe used.</span></span> <span data-ttu-id="1b2aa-114">A virtuális gép hello kell tooenter egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-114">You will need tooenter a user name and password for hello VM.</span></span> <span data-ttu-id="1b2aa-115">hello fiók- és erőforrás tárolócsoport már léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-115">hello storage account and resource group must already exist.</span></span>

    ```powershell
    $stName  = "vnetstorage"
    $locName = "Central US"
    $rgName  = "TestRG"
    $cred    = Get-Credential -Message "Type hello name and password of hello local administrator account."
    ```

2. <span data-ttu-id="1b2aa-116">Kérje le hello virtuális hálózat és alhálózat toocreate kívánt hello a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-116">Retrieve hello virtual network and subnet you want toocreate hello VM in.</span></span>

    ```powershell
    $vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    $subnet = $vnet.Subnets[0].Id
    ```

3. <span data-ttu-id="1b2aa-117">Szükség esetén hozzon létre egy nyilvános IP cím tooaccess hello VM hello Internet.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-117">If necessary, create a public IP address tooaccess hello VM from hello Internet.</span></span>

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name TestPIP -ResourceGroupName $rgName `
    -Location $locName -AllocationMethod Dynamic
    ```

4. <span data-ttu-id="1b2aa-118">Hozzon létre egy hálózati adapter hello statikus magánhálózati IP-cím azt szeretné, hogy a virtuális gép tooassign toohello használatával.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-118">Create a NIC using hello static private IP address you want tooassign toohello VM.</span></span> <span data-ttu-id="1b2aa-119">Győződjön meg arról, hogy hello IP van hello alhálózati tartományból ad hozzá hello virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-119">Make sure hello IP is from hello subnet range you are adding hello VM to.</span></span> <span data-ttu-id="1b2aa-120">Ez a hello fő lépés ebben a cikkben, amelyen meg hello privát IP-toobe statikus.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-120">This is hello main step for this article, where you set hello private IP toobe static.</span></span>

    ```powershell
    $nic = New-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName $rgName `
    -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id `
    -PrivateIpAddress 192.168.1.101
    ```

5. <span data-ttu-id="1b2aa-121">A fenti létrehozott virtuális gép hálózati hello hello létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-121">Create hello VM using hello NIC created above.</span></span>

    ```powershell
    $vm = New-AzureRmVMConfig -VMName DNS01 -VMSize "Standard_A1"
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName DNS01 `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/WindowsVMosDisk.vhd"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name "windowsvmosdisk" -VhdUri $osDiskUri `
    -CreateOption fromImage
    New-AzureRmVM -ResourceGroupName $rgName -Location $locName -VM $vm 
    ```

    <span data-ttu-id="1b2aa-122">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="1b2aa-122">Expected output:</span></span>
    
        EndTime             : [Date and time]
        Error               : 
        Output              : 
        StartTime           : [Date and time]
        Status              : Succeeded
        TrackingOperationId : [Id]
        RequestId           : [Id]
        StatusCode          : OK 

## <a name="retrieve-static-private-ip-address-information-for-a-network-interface"></a><span data-ttu-id="1b2aa-123">Statikus magánhálózati IP-címadatok a hálózati illesztő beolvasása</span><span class="sxs-lookup"><span data-stu-id="1b2aa-123">Retrieve static private IP address information for a network interface</span></span>
<span data-ttu-id="1b2aa-124">tooview hello statikus magánhálózati IP-címe címadatok hello a virtuális gép létre a fenti hello parancsfájl hello a következő PowerShell-parancs futtatása, és tekintse meg az hello értékeinek *privateipaddress tulajdonságot* és  *Címkiosztási*:</span><span class="sxs-lookup"><span data-stu-id="1b2aa-124">tooview hello static private IP address information for hello VM created with hello script above, run hello following PowerShell command and observe hello values for *PrivateIpAddress* and *PrivateIpAllocationMethod*:</span></span>

```powershell
Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
```

<span data-ttu-id="1b2aa-125">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="1b2aa-125">Expected output:</span></span>

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    Etag                 : W/"[Id]"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"[Id]\"",
                               "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Static",
                               "Subnet": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="remove-a-static-private-ip-address-from-a-network-interface"></a><span data-ttu-id="1b2aa-126">A statikus magánhálózati IP-cím eltávolítása a hálózati illesztő</span><span class="sxs-lookup"><span data-stu-id="1b2aa-126">Remove a static private IP address from a network interface</span></span>
<span data-ttu-id="1b2aa-127">tooremove hello statikus magánhálózati IP-cím hozzá toohello VM felett, a következő PowerShell-parancsok futtatása hello hello parancsfájlban:</span><span class="sxs-lookup"><span data-stu-id="1b2aa-127">tooremove hello static private IP address added toohello VM in hello script above, run hello following PowerShell commands:</span></span>

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Dynamic"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

<span data-ttu-id="1b2aa-128">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="1b2aa-128">Expected output:</span></span>

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    Etag                 : W/"[Id]"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/WindowsVM"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"[Id]\"",
                               "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Dynamic",
                               "Subnet": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="add-a-static-private-ip-address-tooa-network-interface"></a><span data-ttu-id="1b2aa-129">Adja hozzá a statikus magánhálózati IP-cím tooa hálózati kapcsolat</span><span class="sxs-lookup"><span data-stu-id="1b2aa-129">Add a static private IP address tooa network interface</span></span>
<span data-ttu-id="1b2aa-130">egy statikus magánhálózati IP cím toohello a fenti hello parancsfájl használatával létrehozott virtuális gép tooadd futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="1b2aa-130">tooadd a static private IP address toohello VM created using hello script above, run hello following commands:</span></span>

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Static"
$nic.IpConfigurations[0].PrivateIpAddress = "192.168.1.101"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```
## <a name="change-hello-allocation-method-for-a-private-ip-address-assigned-tooa-network-interface"></a><span data-ttu-id="1b2aa-131">A privát IP-cím tooa hálózati illesztő hello módszerének módosítása</span><span class="sxs-lookup"><span data-stu-id="1b2aa-131">Change hello allocation method for a private IP address assigned tooa network interface</span></span>

<span data-ttu-id="1b2aa-132">A magánhálózati IP-cím hozzá van rendelve a hálózati adapter tooa hello statikus vagy dinamikus kiosztási módszerrel.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-132">A private IP address is assigned tooa NIC with hello static or dynamic allocation method.</span></span> <span data-ttu-id="1b2aa-133">Dinamikus IP-címek (felszabadított) állapotát, amely korábban a hello indítása leállítása után módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-133">Dynamic IP addresses can change after starting a VM that was previously in hello stopped (deallocated) state.</span></span> <span data-ttu-id="1b2aa-134">Potenciálisan problémák léphetnek fel, ha a virtuális gép hello igénylő hello szolgáltatást üzemeltető IP-cím, egy leállított (felszabadított) állapot újraindítás után is.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-134">This can potentially cause issues if hello VM is hosting a service that requires hello same IP address, even after restarts from a stopped (deallocated) state.</span></span> <span data-ttu-id="1b2aa-135">Statikus IP-címek vannak maradnak, amíg nem hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-135">Static IP addresses are retained until hello VM is deleted.</span></span> <span data-ttu-id="1b2aa-136">toochange hello elosztási módszert IP-cím, futtassa a következő parancsfájl, amely megváltoztatja a hello elosztási módszert a dinamikus toostatic hello.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-136">toochange hello allocation method of an IP address, run hello following script, which changes hello allocation method from dynamic toostatic.</span></span> <span data-ttu-id="1b2aa-137">Ha statikus kiosztási módszerrel hello hello aktuális magánhálózati IP-cím, módosítsa *statikus* túl*dinamikus* hello parancsfájl végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-137">If hello allocation method for hello current private IP address is static, change *Static* too*Dynamic* before executing hello script.</span></span>

```powershell
$RG = "TestRG"
$NIC_name = "testnic1"

$nic = Get-AzureRmNetworkInterface -ResourceGroupName $RG -Name $NIC_name
$nic.IpConfigurations[0].PrivateIpAllocationMethod = 'Static'
Set-AzureRmNetworkInterface -NetworkInterface $nic 
$IP = $nic.IpConfigurations[0].PrivateIpAddress

Write-Host "hello allocation method is now set to"$nic.IpConfigurations[0].PrivateIpAllocationMethod"for hello IP address" $IP"." -NoNewline
```

<span data-ttu-id="1b2aa-138">Ha nem tudja hello NIC hello nevét, erőforráscsoporton belül a hálózati adapterek listáját megtekintheti a hello a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="1b2aa-138">If you don't know hello name of hello NIC, you can view a list of NICs within a resource group by entering hello following command:</span></span>

```powershell
Get-AzureRmNetworkInterface -ResourceGroupName $RG | Where-Object {$_.ProvisioningState -eq 'Succeeded'} 
```

## <a name="next-steps"></a><span data-ttu-id="1b2aa-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b2aa-139">Next steps</span></span>
* <span data-ttu-id="1b2aa-140">További tudnivalók [foglalt nyilvános IP-cím](virtual-networks-reserved-public-ip.md) címek.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-140">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="1b2aa-141">További tudnivalók [példányszintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) címek.</span><span class="sxs-lookup"><span data-stu-id="1b2aa-141">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="1b2aa-142">Tekintse át a hello [fenntartott IP REST API-k](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="1b2aa-142">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

