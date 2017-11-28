---
title: "egy általánosított virtuális Merevlemezt tooAzure PowerShell parancsfájl minta aaaUpload |} Microsoft Docs"
description: "PowerShell parancsfájl tooupload egy általánosított virtuális Merevlemezt tooAzure sample, és új virtuális gép létrehozása hello resource manager üzembe helyezési modellben és a felügyelt lemezek."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/18/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 72b4ff09eb7a6ba1604b9d5cbac51e60eded7545
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sample-script-tooupload-a-vhd-tooazure-and-create-a-new-vm"></a><span data-ttu-id="453fb-103">A minta tooupload egy virtuális merevlemez tooAzure parancsfájl, és hozzon létre egy új virtuális Gépet</span><span class="sxs-lookup"><span data-stu-id="453fb-103">Sample script tooupload a VHD tooAzure and create a new VM</span></span>

<span data-ttu-id="453fb-104">Ez a parancsfájl telik el egy általánosított virtuális Gépet a helyi .vhd-fájllá és feltölti azt tooAzure, hoz létre egy felügyelt lemezképet és hello toocreate egy új virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="453fb-104">This script takes a local .vhd file from a generalized VM and uploads it tooAzure, creates a Managed Disk image and uses hello toocreate a new VM.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="453fb-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="453fb-105">Sample script</span></span>

```powershell
# Provide values for hello variables
$resourceGroup = 'myResourceGroup'
$location = 'EastUS'
$storageaccount = 'mystorageaccount'
$storageType = 'Standard_LRS'
$containername = 'mycontainer'
$localPath = 'C:\Users\Public\Documents\Hyper-V\VHDs\generalized.vhd'
$vmName = 'myVM'
$imageName = 'myImage'
$vhdName = 'myUploadedVhd.vhd'
$diskSizeGB = '128'
$subnetName = 'mySubnet'
$vnetName = 'myVnet'
$ipName = 'myPip'
$nicName = 'myNic'
$nsgName = 'myNsg'
$ruleName = 'myRdpRule'
$vmName = 'myVM'
$computerName = 'myComputerName'
$vmSize = 'Standard_DS1_v2'

# Get hello username and password toobe used for hello administrators account on hello VM. 
# This is used when connecting toohello VM using RDP.

$cred = Get-Credential

# Upload hello VHD
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccount -Location $location `
    -SkuName $storageType -Kind "Storage"
$urlOfUploadedImageVhd = ('https://' + $storageaccount + '.blob.core.windows.net/' + $containername + '/' + $vhdName)
Add-AzureRmVhd -ResourceGroupName $resourceGroup -Destination $urlOfUploadedImageVhd `
    -LocalFilePath $localPath

# Note: Uploading hello VHD may take awhile!

# Create a managed image from hello uploaded VHD 
$imageConfig = New-AzureRmImageConfig -Location $location
$imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized `
    -BlobUri $urlOfUploadedImageVhd
$image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $resourceGroup -Image $imageConfig
 
# Create hello networking resources
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $resourceGroup -Location $location `
    -AllocationMethod Dynamic
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroup -Location $location `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description 'Allow RDP' -Access Allow `
    -Protocol Tcp -Direction Inbound -Priority 110 -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName

# Start building hello VM configuration
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

# Set hello VM image as source image for hello new VM
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id

# Finish hello VM configuration and add hello NIC.
$vm = Set-AzureRmVMOSDisk -VM $vm  -DiskSizeInGB $diskSizeGB -CreateOption FromImage -Caching ReadWrite
$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

# Create hello VM
New-AzureRmVM -VM $vm -ResourceGroupName $resourceGroup -Location $location

# Verify that hello VM was created
$vmList = Get-AzureRmVM -ResourceGroupName $resourceGroup
$vmList.Name


```


<!-- 
[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-iis/create-windows-vm-iis.ps1 "Create VM IIS")] -->

## <a name="clean-up-deployment"></a><span data-ttu-id="453fb-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="453fb-106">Clean up deployment</span></span> 

<span data-ttu-id="453fb-107">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="453fb-107">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="453fb-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="453fb-108">Script explanation</span></span>

<span data-ttu-id="453fb-109">A parancsfájl a következő parancsok toocreate hello telepítési hello.</span><span class="sxs-lookup"><span data-stu-id="453fb-109">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="453fb-110">Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="453fb-110">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="453fb-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="453fb-111">Command</span></span>                                                                                                             | <span data-ttu-id="453fb-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="453fb-112">Notes</span></span>                                                                                                                                                                                |
|---------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="453fb-113">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="453fb-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup)                           | <span data-ttu-id="453fb-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="453fb-114">Creates a resource group in which all resources are stored.</span></span>                                                                                                                          |
| [<span data-ttu-id="453fb-115">Új-AzureRmStorageAccount</span><span class="sxs-lookup"><span data-stu-id="453fb-115">New-AzureRmStorageAccount</span></span>](/powershell/module/azurerm.resources/new-azurermstorageaccount)                         | <span data-ttu-id="453fb-116">Tárfiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="453fb-116">Creates a storage account.</span></span>                                                                                                                                                           |
| [<span data-ttu-id="453fb-117">Adja hozzá AzureRmVhd</span><span class="sxs-lookup"><span data-stu-id="453fb-117">Add-AzureRmVhd</span></span>](/powershell/module/azurerm.resources/add-azurermvhd)                                               | <span data-ttu-id="453fb-118">Egy helyszíni virtuális gép tooa blobból az Azure-ban a felhőalapú társzolgáltatás fiókja a virtuális merevlemez feltöltése.</span><span class="sxs-lookup"><span data-stu-id="453fb-118">Uploads a virtual hard disk from an on-premises virtual machine tooa blob in a cloud storage account in Azure.</span></span>                                                                       |
| [<span data-ttu-id="453fb-119">Új AzureRmImageConfig</span><span class="sxs-lookup"><span data-stu-id="453fb-119">New-AzureRmImageConfig</span></span>](/powershell/module/azurerm.resources/new-azurermimageconfig)                               | <span data-ttu-id="453fb-120">A konfigurálható lemezkép-objektumot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="453fb-120">Creates a configurable image object.</span></span>                                                                                                                                                 |
| [<span data-ttu-id="453fb-121">Set-AzureRmImageOsDisk</span><span class="sxs-lookup"><span data-stu-id="453fb-121">Set-AzureRmImageOsDisk</span></span>](/powershell/module/azurerm.resources/set-azurermimageosdisk)                               | <span data-ttu-id="453fb-122">Hello operációs rendszer lemez tulajdonságainak megadása egy kép objektum.</span><span class="sxs-lookup"><span data-stu-id="453fb-122">Sets hello operating system disk properties on an image object.</span></span>                                                                                                                        |
| [<span data-ttu-id="453fb-123">Új AzureRmImage</span><span class="sxs-lookup"><span data-stu-id="453fb-123">New-AzureRmImage</span></span>](/powershell/module/azurerm.resources/new-azurermimage)                                           | <span data-ttu-id="453fb-124">Létrehoz egy új lemezképet.</span><span class="sxs-lookup"><span data-stu-id="453fb-124">Creates a new image.</span></span>                                                                                                                                                                 |
| [<span data-ttu-id="453fb-125">Új AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="453fb-125">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.resources/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="453fb-126">Létrehoz egy alhálózati konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="453fb-126">Creates a subnet configuration.</span></span> <span data-ttu-id="453fb-127">Ebben a konfigurációban használatos hello virtuális hálózat létrehozásának folyamatát.</span><span class="sxs-lookup"><span data-stu-id="453fb-127">This configuration is used with hello virtual network creation process.</span></span>                                                                                |
| [<span data-ttu-id="453fb-128">Új-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="453fb-128">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.resources/new-azurermvirtualnetwork)                         | <span data-ttu-id="453fb-129">Virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="453fb-129">Creates a virtual network.</span></span>                                                                                                                                                           |
| [<span data-ttu-id="453fb-130">Új AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="453fb-130">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.resources/new-azurermpublicipaddress)                       | <span data-ttu-id="453fb-131">Létrehoz egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="453fb-131">Creates a public IP address.</span></span>                                                                                                                                                         |
| [<span data-ttu-id="453fb-132">Új AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="453fb-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.resources/new-azurermnetworkinterface)                     | <span data-ttu-id="453fb-133">Létrehoz egy adott hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="453fb-133">Creates a network interface.</span></span>                                                                                                                                                         |
| [<span data-ttu-id="453fb-134">Új AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="453fb-134">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.resources/new-azurermnetworksecurityruleconfig)   | <span data-ttu-id="453fb-135">Létrehoz egy hálózati biztonsági csoport szabály konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="453fb-135">Creates a network security group rule configuration.</span></span> <span data-ttu-id="453fb-136">Ez a konfiguráció használt toocreate egy NSG-szabály esetén hello NSG jön létre.</span><span class="sxs-lookup"><span data-stu-id="453fb-136">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span>                                                       |
| [<span data-ttu-id="453fb-137">Új AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="453fb-137">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.resources/new-azurermnetworksecuritygroup)             | <span data-ttu-id="453fb-138">Hálózati biztonsági csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="453fb-138">Creates a network security group.</span></span>                                                                                                                                                    |
| [<span data-ttu-id="453fb-139">Get-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="453fb-139">Get-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.resources/get-azurermvirtualnetwork)                         | <span data-ttu-id="453fb-140">Lekérdezi a virtuális hálózat erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="453fb-140">Gets a virtual network in a resource group.</span></span>                                                                                                                                          |
| [<span data-ttu-id="453fb-141">Új AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="453fb-141">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.resources/new-azurermvmconfig)                                     | <span data-ttu-id="453fb-142">Létrehoz egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="453fb-142">Creates a VM configuration.</span></span> <span data-ttu-id="453fb-143">Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="453fb-143">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="453fb-144">hello konfigurációs szolgál a virtuális gép létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="453fb-144">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="453fb-145">Set-AzureRmVMSourceImage</span><span class="sxs-lookup"><span data-stu-id="453fb-145">Set-AzureRmVMSourceImage</span></span>](/powershell/module/azurerm.resources/set-azurermvmsourceimage)                           | <span data-ttu-id="453fb-146">Adja meg a virtuális gép kép.</span><span class="sxs-lookup"><span data-stu-id="453fb-146">Specifies an image for a virtual machine.</span></span>                                                                                                                                            |
| [<span data-ttu-id="453fb-147">Set-AzureRmVMOSDisk</span><span class="sxs-lookup"><span data-stu-id="453fb-147">Set-AzureRmVMOSDisk</span></span>](/powershell/module/azurerm.resources/set-azurermvmosdisk)                                     | <span data-ttu-id="453fb-148">Hello operációs rendszer lemez tulajdonságainak megadása a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="453fb-148">Sets hello operating system disk properties on a virtual machine.</span></span>                                                                                                                      |
| [<span data-ttu-id="453fb-149">Set-AzureRmVMOperatingSystem</span><span class="sxs-lookup"><span data-stu-id="453fb-149">Set-AzureRmVMOperatingSystem</span></span>](/powershell/module/azurerm.resources/set-azurermvmoperatingsystem)                   | <span data-ttu-id="453fb-150">Hello operációs rendszer lemez tulajdonságainak megadása a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="453fb-150">Sets hello operating system disk properties on a virtual machine.</span></span>                                                                                                                      |
| [<span data-ttu-id="453fb-151">Adja hozzá AzureRmVMNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="453fb-151">Add-AzureRmVMNetworkInterface</span></span>](/powershell/module/azurerm.resources/add-azurermvmnetworkinterface)                 | <span data-ttu-id="453fb-152">A hálózati illesztő tooa virtuális gépek hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="453fb-152">Adds a network interface tooa virtual machine.</span></span>                                                                                                                                       |
| [<span data-ttu-id="453fb-153">Új AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="453fb-153">New-AzureRmVM</span></span>](/powershell/module/azurerm.resources/new-azurermvm)                                                 | <span data-ttu-id="453fb-154">Hozzon létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="453fb-154">Create a virtual machine.</span></span>                                                                                                                                                            |
| [<span data-ttu-id="453fb-155">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="453fb-155">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup)                     | <span data-ttu-id="453fb-156">Eltávolítja az erőforráscsoportot és belül található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="453fb-156">Removes a resource group and all resources contained within.</span></span>                                                                                                                         |

## <a name="next-steps"></a><span data-ttu-id="453fb-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="453fb-157">Next steps</span></span>

<span data-ttu-id="453fb-158">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="453fb-158">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="453fb-159">További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="453fb-159">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
