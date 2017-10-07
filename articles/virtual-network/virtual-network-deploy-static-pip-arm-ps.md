---
title: "a virtuális gép statikus nyilvános IP-címmel – Azure PowerShell aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy virtuális Gépet egy statikus nyilvános IP-cím PowerShell használatával."
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
ms.openlocfilehash: 0d2b88319cb114b8616f60dbee41e8fdc6d8b1b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-powershell"></a><span data-ttu-id="582b1-103">Virtuális gép létrehozása a PowerShell használatával statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="582b1-103">Create a VM with a static public IP address using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="582b1-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="582b1-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="582b1-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="582b1-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="582b1-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="582b1-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="582b1-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="582b1-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="582b1-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="582b1-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="582b1-109">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="582b1-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="582b1-110">Ez a cikk a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello klasszikus üzembe helyezési modellel hello Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="582b1-110">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="step-1---start-your-script"></a><span data-ttu-id="582b1-111">1. lépés – a parancsfájl futtatásához</span><span class="sxs-lookup"><span data-stu-id="582b1-111">Step 1 - Start your script</span></span>
<span data-ttu-id="582b1-112">Letöltheti a hello használt teljes PowerShell parancsfájl [Itt](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-ps.ps1).</span><span class="sxs-lookup"><span data-stu-id="582b1-112">You can download hello full PowerShell script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-ps.ps1).</span></span> <span data-ttu-id="582b1-113">Kövesse az alábbi toochange hello parancsfájl toowork környezetében hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="582b1-113">Follow hello steps below toochange hello script toowork in your environment.</span></span>

<span data-ttu-id="582b1-114">Hello értékek módosítása hello változók alábbi hello értékek alapján az üzembe helyezéshez szeretné toouse.</span><span class="sxs-lookup"><span data-stu-id="582b1-114">Change hello values of hello variables below based on hello values you want toouse for your deployment.</span></span> <span data-ttu-id="582b1-115">hello értékek térkép toohello forgatókönyv a cikk ezt használja a következő:</span><span class="sxs-lookup"><span data-stu-id="582b1-115">hello following values map toohello scenario used in this article:</span></span>

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

## <a name="step-2---create-hello-necessary-resources-for-your-vm"></a><span data-ttu-id="582b1-116">2. lépés - a virtuális gép számára szükséges erőforrások hello létrehozása</span><span class="sxs-lookup"><span data-stu-id="582b1-116">Step 2 - Create hello necessary resources for your VM</span></span>
<span data-ttu-id="582b1-117">Virtuális gép létrehozása előtt kell egy erőforráscsoport, hálózatok, nyilvános IP-, és a hálózati adapter toobe hello virtuális gép által használt.</span><span class="sxs-lookup"><span data-stu-id="582b1-117">Before creating a VM, you need a resource group, VNet, public IP, and NIC toobe used by hello VM.</span></span>

1. <span data-ttu-id="582b1-118">Hozzon létre egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="582b1-118">Create a new resource group.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name $rgName -Location $location
    ```

2. <span data-ttu-id="582b1-119">Hozzon létre hello VNet és alhálózat.</span><span class="sxs-lookup"><span data-stu-id="582b1-119">Create hello VNet and subnet.</span></span>

    ```powershell
    $vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName `
        -AddressPrefix $vnetPrefix -Location $location

    Add-AzureRmVirtualNetworkSubnetConfig -Name $subnetName `
        -VirtualNetwork $vnet -AddressPrefix $subnetPrefix

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

3. <span data-ttu-id="582b1-120">Hello nyilvános IP-erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="582b1-120">Create hello public IP resource.</span></span> 

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName `
        -AllocationMethod Static -DomainNameLabel $dnsName -Location $location
    ```

4. <span data-ttu-id="582b1-121">Hozzon létre hello nyilvános IP-cím a fenti létrehozott hello alhálózatban lévő virtuális gép hello hello hálózati illesztőt (NIC).</span><span class="sxs-lookup"><span data-stu-id="582b1-121">Create hello network interface (NIC) for hello VM in hello subnet created above, with hello public IP.</span></span> <span data-ttu-id="582b1-122">Figyelje meg, hello Azure hello VNet lekérése első parancsmag, ez az szükséges, mivel egy `Set-AzureRmVirtualNetwork` lett hajtva toochange hello meglévő virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="582b1-122">Notice hello first cmdlet retrieving hello VNet from Azure, this is necessary since a `Set-AzureRmVirtualNetwork` was executed toochange hello existing VNet.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
        -Subnet $subnet -Location $location -PrivateIpAddress $privateIPAddress `
        -PublicIpAddress $pip
    ```

5. <span data-ttu-id="582b1-123">Hozzon létre egy tárolási fiók toohost hello virtuális gép operációs rendszere meghajtó.</span><span class="sxs-lookup"><span data-stu-id="582b1-123">Create a storage account toohost hello VM OS drive.</span></span>

    ```powershell
    $stdStorageAccount = New-AzureRmStorageAccount -Name $stdStorageAccountName `
    -ResourceGroupName $rgName -Type Standard_LRS -Location $location
    ```

## <a name="step-3---create-hello-vm"></a><span data-ttu-id="582b1-124">3. lépés – hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="582b1-124">Step 3 - Create hello VM</span></span>
<span data-ttu-id="582b1-125">Most, hogy minden szükséges erőforrás van érvényben, létrehozhat egy új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="582b1-125">Now that all necessary resources are in place, you can create a new VM.</span></span>

1. <span data-ttu-id="582b1-126">Hello konfigurációs objektumot a hello virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="582b1-126">Create hello configuration object for hello VM.</span></span>

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    ```

2. <span data-ttu-id="582b1-127">A virtuális gép helyi rendszergazdai fiók hello beszerezni a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="582b1-127">Get credentials for hello VM local administrator account.</span></span>

    ```powershell
    $cred = Get-Credential -Message "Type hello name and password for hello local administrator account."
    ```

3. <span data-ttu-id="582b1-128">Hozzon létre egy virtuális gép konfigurációs objektuma.</span><span class="sxs-lookup"><span data-stu-id="582b1-128">Create a VM configuration object.</span></span>

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```

4. <span data-ttu-id="582b1-129">Állítsa be a virtuális gép hello hello operációs rendszer lemezképét.</span><span class="sxs-lookup"><span data-stu-id="582b1-129">Set hello operating system image for hello VM.</span></span>

    ```powershell
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher `
        -Offer $offer -Skus $sku -Version $version
    ```

5. <span data-ttu-id="582b1-130">Az operációs rendszer hello lemez konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="582b1-130">Configure hello OS disk.</span></span>

    ```powershell
    $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
    $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
    ```

6. <span data-ttu-id="582b1-131">Adja hozzá a hello NIC toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="582b1-131">Add hello NIC toohello VM.</span></span>

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id -Primary
    ```

7. <span data-ttu-id="582b1-132">Hello virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="582b1-132">Create hello VM.</span></span>

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName $rgName -Location $location
    ```

8. <span data-ttu-id="582b1-133">Hello parancsfájlokat mentse.</span><span class="sxs-lookup"><span data-stu-id="582b1-133">Save hello script file.</span></span>

## <a name="step-4---run-hello-script"></a><span data-ttu-id="582b1-134">4. lépés: hello parancsfájl futtatása</span><span class="sxs-lookup"><span data-stu-id="582b1-134">Step 4 - Run hello script</span></span>
<span data-ttu-id="582b1-135">Szükséges módosítások, és hello parancsfájl megismerése után fent megjelenítése, hello parancsprogrammal.</span><span class="sxs-lookup"><span data-stu-id="582b1-135">After making any necessary changes, and understanding hello script show above, run hello script.</span></span> 

1. <span data-ttu-id="582b1-136">Egy PowerShell-konzolon, vagy a PowerShell ISE futtassa a fenti hello-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="582b1-136">From a PowerShell console, or PowerShell ISE, run hello script above.</span></span>
2. <span data-ttu-id="582b1-137">a következő kimeneti hello üzenetnek kell megjelennie, néhány perc elteltével:</span><span class="sxs-lookup"><span data-stu-id="582b1-137">hello following output should be displayed after a few minutes:</span></span>
   
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

