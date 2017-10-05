---
title: "Hozzon létre egy virtuális Gépet egy statikus nyilvános IP-cím - Azure PowerShell |} Microsoft Docs"
description: "Útmutató: virtuális gép létrehozása a PowerShell használatával statikus nyilvános IP-cím."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ad975ab9-d69f-45c1-9e45-0d3f0f51e87e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e4c413d3cb5c242a16f3e534dafe322785a35141
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-powershell"></a><span data-ttu-id="35611-103">Virtuális gép létrehozása a PowerShell használatával statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="35611-103">Create a VM with a static public IP address using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="35611-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="35611-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="35611-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="35611-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="35611-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="35611-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="35611-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="35611-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="35611-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="35611-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="35611-109">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="35611-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="35611-110">Ez a cikk a Microsoft azt javasolja, hogy a klasszikus üzembe helyezési modellel helyett az új telepítések esetén a Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="35611-110">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="step-1---start-your-script"></a><span data-ttu-id="35611-111">1. lépés – a parancsfájl futtatásához</span><span class="sxs-lookup"><span data-stu-id="35611-111">Step 1 - Start your script</span></span>
<span data-ttu-id="35611-112">Letöltheti használt teljes PowerShell-parancsfájl [Itt](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-ps.ps1).</span><span class="sxs-lookup"><span data-stu-id="35611-112">You can download the full PowerShell script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-ps.ps1).</span></span> <span data-ttu-id="35611-113">Módosíthatja a parancsfájlnak a környezetben az alábbi lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="35611-113">Follow the steps below to change the script to work in your environment.</span></span>

<span data-ttu-id="35611-114">A telepítéshez használni kívánt értékek alapján az alábbi változók értékeinek módosítása.</span><span class="sxs-lookup"><span data-stu-id="35611-114">Change the values of the variables below based on the values you want to use for your deployment.</span></span> <span data-ttu-id="35611-115">A következő értékek leképezése a forgatókönyvet, a cikk ezt használja:</span><span class="sxs-lookup"><span data-stu-id="35611-115">The following values map to the scenario used in this article:</span></span>

```powershell
# Set variables resource group
$rgName                = "IaaSStory"
$location              = "West US"

# Set variables for VNet
$vnetName              = "WTestVNet"
$vnetPrefix            = "192.168.0.0/16"
$subnetName            = "FrontEnd"
$subnetPrefix          = "192.168.1.0/24"

# Set variables for storage
$stdStorageAccountName = "iaasstorystorage"

# Set variables for VM
$vmSize                = "Standard_A1"
$diskSize              = 127
$publisher             = "MicrosoftWindowsServer"
$offer                 = "WindowsServer"
$sku                   = "2012-R2-Datacenter"
$version               = "latest"
$vmName                = "WEB1"
$osDiskName            = "osdisk"
$nicName               = "NICWEB1"
$privateIPAddress      = "192.168.1.101"
$pipName               = "PIPWEB1"
$dnsName               = "iaasstoryws1"
```

## <a name="step-2---create-the-necessary-resources-for-your-vm"></a><span data-ttu-id="35611-116">2. lépés - a szükséges erőforrások a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="35611-116">Step 2 - Create the necessary resources for your VM</span></span>
<span data-ttu-id="35611-117">Virtuális gép létrehozása előtt meg kell egy erőforráscsoport, hálózatok, nyilvános IP-cím és a hálózati adapter a virtuális gép által használandó.</span><span class="sxs-lookup"><span data-stu-id="35611-117">Before creating a VM, you need a resource group, VNet, public IP, and NIC to be used by the VM.</span></span>

1. <span data-ttu-id="35611-118">Hozzon létre egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="35611-118">Create a new resource group.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name $rgName -Location $location
    ```

2. <span data-ttu-id="35611-119">A VNet és alhálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="35611-119">Create the VNet and subnet.</span></span>

    ```powershell
    $vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName `
        -AddressPrefix $vnetPrefix -Location $location

    Add-AzureRmVirtualNetworkSubnetConfig -Name $subnetName `
        -VirtualNetwork $vnet -AddressPrefix $subnetPrefix

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

3. <span data-ttu-id="35611-120">A nyilvános IP-erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="35611-120">Create the public IP resource.</span></span> 

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName `
        -AllocationMethod Static -DomainNameLabel $dnsName -Location $location
    ```

4. <span data-ttu-id="35611-121">Hozzon létre a hálózati kártya (NIC) a virtuális géphez létre felett, a nyilvános IP-cím az alhálózat.</span><span class="sxs-lookup"><span data-stu-id="35611-121">Create the network interface (NIC) for the VM in the subnet created above, with the public IP.</span></span> <span data-ttu-id="35611-122">Figyelje meg, az első parancsmag beolvasása a virtuális hálózat az Azure-ból, ez pedig szükséges, mivel egy `Set-AzureRmVirtualNetwork` módosíthatja a meglévő VNet végre lett hajtva.</span><span class="sxs-lookup"><span data-stu-id="35611-122">Notice the first cmdlet retrieving the VNet from Azure, this is necessary since a `Set-AzureRmVirtualNetwork` was executed to change the existing VNet.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
        -Subnet $subnet -Location $location -PrivateIpAddress $privateIPAddress `
        -PublicIpAddress $pip
    ```

5. <span data-ttu-id="35611-123">Hozzon létre egy tárfiókot, a virtuális gép operációs rendszere meghajtó üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="35611-123">Create a storage account to host the VM OS drive.</span></span>

    ```powershell
    $stdStorageAccount = New-AzureRmStorageAccount -Name $stdStorageAccountName `
    -ResourceGroupName $rgName -Type Standard_LRS -Location $location
    ```

## <a name="step-3---create-the-vm"></a><span data-ttu-id="35611-124">3. lépés – a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="35611-124">Step 3 - Create the VM</span></span>
<span data-ttu-id="35611-125">Most, hogy minden szükséges erőforrás van érvényben, létrehozhat egy új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="35611-125">Now that all necessary resources are in place, you can create a new VM.</span></span>

1. <span data-ttu-id="35611-126">A konfigurációs objektumot létrehozni a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="35611-126">Create the configuration object for the VM.</span></span>

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    ```

2. <span data-ttu-id="35611-127">A virtuális gép helyi rendszergazdai fiók hitelesítő adatainak lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="35611-127">Get credentials for the VM local administrator account.</span></span>

    ```powershell
    $cred = Get-Credential -Message "Type the name and password for the local administrator account."
    ```

3. <span data-ttu-id="35611-128">Hozzon létre egy virtuális gép konfigurációs objektuma.</span><span class="sxs-lookup"><span data-stu-id="35611-128">Create a VM configuration object.</span></span>

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```

4. <span data-ttu-id="35611-129">Állítsa be az operációs rendszer lemezképét a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="35611-129">Set the operating system image for the VM.</span></span>

    ```powershell
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher `
        -Offer $offer -Skus $sku -Version $version
    ```

5. <span data-ttu-id="35611-130">Konfigurálhatja az operációsrendszer-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="35611-130">Configure the OS disk.</span></span>

    ```powershell
    $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
    $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
    ```

6. <span data-ttu-id="35611-131">A hálózati adapter hozzáadása a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="35611-131">Add the NIC to the VM.</span></span>

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id -Primary
    ```

7. <span data-ttu-id="35611-132">A virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="35611-132">Create the VM.</span></span>

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName $rgName -Location $location
    ```

8. <span data-ttu-id="35611-133">Mentse a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="35611-133">Save the script file.</span></span>

## <a name="step-4---run-the-script"></a><span data-ttu-id="35611-134">4. lépés: a parancsfájl futtatása</span><span class="sxs-lookup"><span data-stu-id="35611-134">Step 4 - Run the script</span></span>
<span data-ttu-id="35611-135">Szükséges módosítások, és a parancsfájl megismerése után fent megjelenítése, futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="35611-135">After making any necessary changes, and understanding the script show above, run the script.</span></span> 

1. <span data-ttu-id="35611-136">Egy PowerShell-konzolon, vagy a PowerShell ISE a fenti parancsfájl futtatásához.</span><span class="sxs-lookup"><span data-stu-id="35611-136">From a PowerShell console, or PowerShell ISE, run the script above.</span></span>
2. <span data-ttu-id="35611-137">A következő kimeneti üzenetnek kell megjelennie, néhány perc elteltével:</span><span class="sxs-lookup"><span data-stu-id="35611-137">The following output should be displayed after a few minutes:</span></span>
   
        ResourceGroupName : IaaSStory
        Location          : westus
        ProvisioningState : Succeeded
        Tags              : 
        ResourceId        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory
   
        AddressSpace      : Microsoft.Azure.Commands.Network.Models.PSAddressSpace
        DhcpOptions       : Microsoft.Azure.Commands.Network.Models.PSDhcpOptions
        Subnets           : {FrontEnd}
        ProvisioningState : Succeeded
        AddressSpaceText  : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptionsText   : {}
        SubnetsText       : [
                              {
                                "Name": "FrontEnd",
                                "AddressPrefix": "192.168.1.0/24"
                              }
                            ]
        ResourceGroupName : IaaSStory
        Location          : westus
        ResourceGuid      : [Id]
        Tag               : {}
        TagsTable         : 
        Name              : WTestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        Id                : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet
   
        AddressSpace      : Microsoft.Azure.Commands.Network.Models.PSAddressSpace
        DhcpOptions       : Microsoft.Azure.Commands.Network.Models.PSDhcpOptions
        Subnets           : {FrontEnd}
        ProvisioningState : Succeeded
        AddressSpaceText  : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptionsText   : {
                              "DnsServers": []
                            }
        SubnetsText       : [
                              {
                                "Name": "FrontEnd",
                                "Etag": [Id],
                                "Id": "/subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "ProvisioningState": "Succeeded"
                              }
                            ]
        ResourceGroupName : IaaSStory
        Location          : westus
        ResourceGuid      : [Id]
        Tag               : {}
        TagsTable         : 
        Name              : WTestVNet
        Etag              : [Id]
        Id                : /subscriptions/[Subscription Id]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet
   
        TrackingOperationId : [Id]
        RequestId           : [Id]
        Status              : Succeeded
        StatusCode          : OK
        Output              : 
        StartTime           : [Subscription Id]
        EndTime             : [Subscription Id]
        Error               : 
        ErrorText           : 

