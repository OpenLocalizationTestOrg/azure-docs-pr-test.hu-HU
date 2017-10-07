---
title: "az Azure-ban kezelt Virtuálisgép-lemezképet a virtuális gép aaaCreate |} Microsoft Docs"
description: "Hozzon létre egy Windows rendszerű virtuális gép egy általánosított felügyelt Virtuálisgép-lemezkép az Azure PowerShell, hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: cynthn
ms.openlocfilehash: 5036ef1533c144a9a328e94599b359e0166f337d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-from-a-managed-image"></a><span data-ttu-id="c8dae-103">Egy felügyelt képre a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8dae-103">Create a VM from a managed image</span></span>

<span data-ttu-id="c8dae-104">Létrehozhat több virtuális gép az Azure-ban kezelt Virtuálisgép-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="c8dae-104">You can create multiple VMs from a managed VM image in Azure.</span></span> <span data-ttu-id="c8dae-105">Egy felügyelt Virtuálisgép-lemezkép hello információ szükséges toocreate egy virtuális Gépet, beleértve a hello operációsrendszer- és adatlemezek tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c8dae-105">A managed VM image contains hello information necessary toocreate a VM, including hello OS and data disks.</span></span> <span data-ttu-id="c8dae-106">hello hello kép, beleértve az operációs rendszer hello lemezek és bármely adatlemezek alkotják, felügyelt lemezként tárolt virtuális merevlemezeket.</span><span class="sxs-lookup"><span data-stu-id="c8dae-106">hello VHDs that make up hello image, including both hello OS disks and any data disks, are stored as managed disks.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="c8dae-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c8dae-107">Prerequisites</span></span>

<span data-ttu-id="c8dae-108">Már szüksége toohave [felügyelt Virtuálisgép-lemezkép létrehozása](capture-image-resource.md) létrehozásához toouse hello új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="c8dae-108">You need toohave already [created a managed VM image](capture-image-resource.md) toouse for creating hello new VM.</span></span> 

<span data-ttu-id="c8dae-109">Győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute és AzureRM.Network PowerShell modulok legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="c8dae-109">Make sure that you have hello latest versions of hello AzureRM.Compute and AzureRM.Network PowerShell modules.</span></span> <span data-ttu-id="c8dae-110">Nyissa meg rendszergazdaként egy PowerShell-parancssorba, majd futtassa a következő parancs tooinstall hello őket.</span><span class="sxs-lookup"><span data-stu-id="c8dae-110">Open a PowerShell prompt as an Administrator and run hello following command tooinstall them.</span></span>

```powershell
Install-Module AzureRM.Compute,AzureRM.Network
```
<span data-ttu-id="c8dae-111">További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c8dae-111">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>



## <a name="collect-information-about-hello-image"></a><span data-ttu-id="c8dae-112">Hello lemezképére vonatkozó információ gyűjtése</span><span class="sxs-lookup"><span data-stu-id="c8dae-112">Collect information about hello image</span></span>

<span data-ttu-id="c8dae-113">Először igazolnia toogather alapszintű hello kép információra van szüksége, és hozzon létre egy változót hello lemezkép.</span><span class="sxs-lookup"><span data-stu-id="c8dae-113">First we need toogather basic information about hello image and create a variable for hello image.</span></span> <span data-ttu-id="c8dae-114">Ez a példa egy felügyelt Virtuálisgép-lemezkép nevű **myImage** , amely a hello **myResourceGroup** hello erőforráscsoportja **nyugati középső Régiójában** helyét.</span><span class="sxs-lookup"><span data-stu-id="c8dae-114">This example uses a managed VM image named **myImage** that is in hello **myResourceGroup** resource group in hello **West Central US** location.</span></span> 

```powershell
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## <a name="create-a-virtual-network"></a><span data-ttu-id="c8dae-115">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8dae-115">Create a virtual network</span></span>
<span data-ttu-id="c8dae-116">Hozzon létre hello vNet és alhálózat a hello [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c8dae-116">Create hello vNet and subnet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="c8dae-117">Hozzon létre hello alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="c8dae-117">Create hello subnet.</span></span> <span data-ttu-id="c8dae-118">Ez a példa-alhálózatot hoz létre nevű **mySubnet** a címelőtagot hello **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="c8dae-118">This example creates a subnet named **mySubnet** with hello address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="c8dae-119">Hello virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c8dae-119">Create hello virtual network.</span></span> <span data-ttu-id="c8dae-120">Ebben a példában egy virtuális hálózatot nevű hoz létre **myVnet** a címelőtagot hello **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="c8dae-120">This example creates a virtual network named **myVnet** with hello address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="c8dae-121">Hozzon létre egy nyilvános IP-cím és a hálózati kapcsolat</span><span class="sxs-lookup"><span data-stu-id="c8dae-121">Create a public IP address and network interface</span></span>

<span data-ttu-id="c8dae-122">hello virtuális gép virtuális hálózati hello kommunikációs tooenable, van szüksége egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="c8dae-122">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="c8dae-123">Hozzon létre egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="c8dae-123">Create a public IP address.</span></span> <span data-ttu-id="c8dae-124">Ez a példa létrehoz egy nyilvános IP-cím nevű **myPip**.</span><span class="sxs-lookup"><span data-stu-id="c8dae-124">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="c8dae-125">Hozzon létre hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="c8dae-125">Create hello NIC.</span></span> <span data-ttu-id="c8dae-126">Ez a példa létrehoz egy hálózati adapter nevű **myNic**.</span><span class="sxs-lookup"><span data-stu-id="c8dae-126">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="c8dae-127">Hello hálózati biztonsági csoport és egy RDP-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8dae-127">Create hello network security group and an RDP rule</span></span>

<span data-ttu-id="c8dae-128">toobe képes toolog tooyour a virtuális gép RDP, kell toohave hálózati biztonsági szabály (NSG), amely lehetővé teszi a hozzáférést RDP a 3389-es porton.</span><span class="sxs-lookup"><span data-stu-id="c8dae-128">toobe able toolog in tooyour VM using RDP, you need toohave a network security rule (NSG) that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="c8dae-129">Ez a példa létrehoz egy NSG nevű **myNsg** nevű szabályt tartalmaz, amelyek **myRdpRule** RDP-forgalmát, amely engedélyezi a 3389-es porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="c8dae-129">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="c8dae-130">Az NSG-k kapcsolatos további információkért lásd: [portok tooa VM megnyitása az Azure-ban a PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c8dae-130">For more information about NSGs, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-hello-virtual-network"></a><span data-ttu-id="c8dae-131">Hozzon létre egy változót hello virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="c8dae-131">Create a variable for hello virtual network</span></span>

<span data-ttu-id="c8dae-132">Hozzon létre egy változót, virtuális hálózat hello befejeződött.</span><span class="sxs-lookup"><span data-stu-id="c8dae-132">Create a variable for hello completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-hello-credentials-for-hello-vm"></a><span data-ttu-id="c8dae-133">A virtuális gép hello hello hitelesítő adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="c8dae-133">Get hello credentials for hello VM</span></span>

<span data-ttu-id="c8dae-134">hello következő parancsmag megnyílik egy ablak, ha beírja egy új felhasználói nevet és jelszót toouse hello helyi rendszergazda fiókot távolról a virtuális gép hello eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="c8dae-134">hello following cmdlet will open a window where you will enter a new user name and password toouse as hello local administrator account for remotely accessing hello VM.</span></span> 

```powershell
$cred = Get-Credential
```

## <a name="set-variables-for-hello-vm-name-computer-name-and-hello-size-of-hello-vm"></a><span data-ttu-id="c8dae-135">Set változói hello virtuális gép neve, a számítógép nevét, és hello hello virtuális gép méretét</span><span class="sxs-lookup"><span data-stu-id="c8dae-135">Set variables for hello VM name, computer name and hello size of hello VM</span></span>

1. <span data-ttu-id="c8dae-136">Hozzon létre változókat hello virtuális gép nevét és a számítógép nevét.</span><span class="sxs-lookup"><span data-stu-id="c8dae-136">Create variables for hello VM name and computer name.</span></span> <span data-ttu-id="c8dae-137">Ez a példa beállítja hello virtuális gép neve, mint **myVM** és számítógépnévvel hello **Sajátgép**.</span><span class="sxs-lookup"><span data-stu-id="c8dae-137">This example sets hello VM name as **myVM** and hello computer name as **myComputer**.</span></span>

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    ```
2. <span data-ttu-id="c8dae-138">Virtuális gép hello hello méretének beállítása.</span><span class="sxs-lookup"><span data-stu-id="c8dae-138">Set hello size of hello virtual machine.</span></span> <span data-ttu-id="c8dae-139">Ez a példa létrehoz **Standard_DS1_v2** méretű virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c8dae-139">This example creates **Standard_DS1_v2** sized VM.</span></span> <span data-ttu-id="c8dae-140">Lásd: hello [Virtuálisgép-méretek](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) dokumentációjában talál további információt.</span><span class="sxs-lookup"><span data-stu-id="c8dae-140">See hello [VM sizes](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) documentation for more information.</span></span>

    ```powershell
    $vmSize = "Standard_DS1_v2"
    ```

3. <span data-ttu-id="c8dae-141">Adja hozzá a hello virtuális gép neve és mérete toohello Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c8dae-141">Add hello VM name and size toohello VM configuration.</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-hello-vm-image-as-source-image-for-hello-new-vm"></a><span data-ttu-id="c8dae-142">Set hello Virtuálisgép-lemezkép forrás képként hello az új virtuális gép</span><span class="sxs-lookup"><span data-stu-id="c8dae-142">Set hello VM image as source image for hello new VM</span></span>

<span data-ttu-id="c8dae-143">Állítsa be a hello forráslemezkép hello felügyelt Virtuálisgép-lemezkép hello Azonosítóját használja.</span><span class="sxs-lookup"><span data-stu-id="c8dae-143">Set hello source image using hello ID of hello managed VM image.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-hello-os-configuration-and-add-hello-nic"></a><span data-ttu-id="c8dae-144">Állítsa be a hello operációs rendszer konfigurációja, és adja hozzá a hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="c8dae-144">Set hello OS configuration and add hello NIC.</span></span>

<span data-ttu-id="c8dae-145">Adja meg a hello tárolási típust (PremiumLRS vagy StandardLRS) és hello hello operációsrendszer-lemez méretét.</span><span class="sxs-lookup"><span data-stu-id="c8dae-145">Enter hello storage type (PremiumLRS or StandardLRS) and hello size of hello OS disk.</span></span> <span data-ttu-id="c8dae-146">Ez a példa beállítja hello fiók típusa túl**PremiumLRS**, lemez mérete túl hello**128 GB-os** és a lemez gyorsítótár túl**ReadWrite**.</span><span class="sxs-lookup"><span data-stu-id="c8dae-146">This example sets hello account type too**PremiumLRS**, hello disk size too**128 GB** and disk caching too**ReadWrite**.</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm  -StorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-hello-vm"></a><span data-ttu-id="c8dae-147">Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8dae-147">Create hello VM</span></span>

<span data-ttu-id="c8dae-148">Hozzon létre új virtuális gép hello konfigurációval rendelkezik beépített és hello tárolt hello **$vm** változó.</span><span class="sxs-lookup"><span data-stu-id="c8dae-148">Create hello new Vm using hello configuration that we have built and stored in hello **$vm** variable.</span></span>

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="c8dae-149">Győződjön meg arról, hogy létrejött a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="c8dae-149">Verify that hello VM was created</span></span>
<span data-ttu-id="c8dae-150">Amikor végzett, láthatja az újonnan létrehozott virtuális gép hello hello [Azure-portálon](https://portal.azure.com) alatt **Tallózás** > **virtuális gépek**, vagy a következő hello használatával PowerShell-parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="c8dae-150">When complete, you should see hello newly created VM in hello [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="c8dae-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c8dae-151">Next steps</span></span>
<span data-ttu-id="c8dae-152">toomanage az új virtuális gépet az Azure PowerShell, lásd: [létrehozása és kezelése Windows virtuális gépek hello Azure PowerShell modul](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c8dae-152">toomanage your new virtual machine with Azure PowerShell, see [Create and manage Windows VMs with hello Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

