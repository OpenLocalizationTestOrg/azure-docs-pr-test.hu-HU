---
title: "aaaCreate és kezelése Windows virtuális gépek hello Azure PowerShell modul |} Microsoft Docs"
description: "Az oktatóanyag - létrehozása és kezelése Windows virtuális gépek hello Azure PowerShell modul"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 20adcb673ef4de683e6ad82d048a9625a1dc838c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-windows-vms-with-hello-azure-powershell-module"></a><span data-ttu-id="728b5-103">Létrehozása és kezelése Windows virtuális gépek hello Azure PowerShell modul</span><span class="sxs-lookup"><span data-stu-id="728b5-103">Create and Manage Windows VMs with hello Azure PowerShell module</span></span>

<span data-ttu-id="728b5-104">Az Azure virtuális gépek adjon meg egy teljes mértékben konfigurálhatók és rugalmas számítási környezet.</span><span class="sxs-lookup"><span data-stu-id="728b5-104">Azure virtual machines provide a fully configurable and flexible computing environment.</span></span> <span data-ttu-id="728b5-105">Ez az oktatóanyag ismerteti alapszintű Azure-beli virtuális gép telepítési elemek, például Virtuálisgép-méret kiválasztása, Virtuálisgép-lemezkép kiválasztása és a virtuális gépek telepítése.</span><span class="sxs-lookup"><span data-stu-id="728b5-105">This tutorial covers basic Azure virtual machine deployment items such as selecting a VM size, selecting a VM image, and deploying a VM.</span></span> <span data-ttu-id="728b5-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="728b5-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="728b5-107">Hozzon létre, és csatlakozzon a virtuális gép tooa</span><span class="sxs-lookup"><span data-stu-id="728b5-107">Create and connect tooa VM</span></span>
> * <span data-ttu-id="728b5-108">Válassza ki és használja a Virtuálisgép-rendszerképek</span><span class="sxs-lookup"><span data-stu-id="728b5-108">Select and use VM images</span></span>
> * <span data-ttu-id="728b5-109">Megtekintése és használata a megadott Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="728b5-109">View and use specific VM sizes</span></span>
> * <span data-ttu-id="728b5-110">Virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="728b5-110">Resize a VM</span></span>
> * <span data-ttu-id="728b5-111">Megtekinteni és megérteni a virtuális gép állapota</span><span class="sxs-lookup"><span data-stu-id="728b5-111">View and understand VM state</span></span>

<span data-ttu-id="728b5-112">Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="728b5-112">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="728b5-113">Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="728b5-113">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="728b5-114">Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="728b5-114">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="728b5-115">Erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="728b5-115">Create resource group</span></span>

<span data-ttu-id="728b5-116">Hozzon létre egy erőforráscsoportot hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsot.</span><span class="sxs-lookup"><span data-stu-id="728b5-116">Create a resource group with hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command.</span></span> 

<span data-ttu-id="728b5-117">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="728b5-117">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="728b5-118">Egy erőforráscsoportot a virtuális gépek előtt létre kell hozni.</span><span class="sxs-lookup"><span data-stu-id="728b5-118">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="728b5-119">Ebben a példában az erőforráscsoport neve *myResourceGroupVM* jön létre az hello *EastUS* régióban.</span><span class="sxs-lookup"><span data-stu-id="728b5-119">In this example, a resource group named *myResourceGroupVM* is created in hello *EastUS* region.</span></span> 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupVM -Location EastUS
```

<span data-ttu-id="728b5-120">hello erőforráscsoport létrehozása vagy módosítása egy virtuális Gépet, amelynek ez az oktatóanyag teljes látható során van megadva.</span><span class="sxs-lookup"><span data-stu-id="728b5-120">hello resource group is specified when creating or modifying a VM, which can be seen throughout this tutorial.</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="728b5-121">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="728b5-121">Create virtual machine</span></span>

<span data-ttu-id="728b5-122">A virtuális gép csatlakoztatott tooa virtuális hálózaton kell lennie.</span><span class="sxs-lookup"><span data-stu-id="728b5-122">A virtual machine must be connected tooa virtual network.</span></span> <span data-ttu-id="728b5-123">Hello virtuális gép egy nyilvános IP-címet a hálózati kártya keresztül kommunikálnak.</span><span class="sxs-lookup"><span data-stu-id="728b5-123">You communicate with hello virtual machine using a public IP address through a network interface card.</span></span>

### <a name="create-virtual-network"></a><span data-ttu-id="728b5-124">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="728b5-124">Create virtual network</span></span>

<span data-ttu-id="728b5-125">Hozzon létre egy alhálózat [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="728b5-125">Create a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="728b5-126">A virtuális hálózat létrehozása [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="728b5-126">Create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 ` 
  -Subnet $subnetConfig
```
### <a name="create-public-ip-address"></a><span data-ttu-id="728b5-127">Nyilvános IP-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="728b5-127">Create public IP address</span></span>

<span data-ttu-id="728b5-128">Hozzon létre egy nyilvános IP-cím [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span><span class="sxs-lookup"><span data-stu-id="728b5-128">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress ` 
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS ` 
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

### <a name="create-network-interface-card"></a><span data-ttu-id="728b5-129">Hálózati kártya létrehozása</span><span class="sxs-lookup"><span data-stu-id="728b5-129">Create network interface card</span></span>

<span data-ttu-id="728b5-130">Hozzon létre egy hálózati kártya [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="728b5-130">Create a network interface card with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span></span>

```powershell
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroupVM  `
  -Location EastUS `
  -Name myNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

### <a name="create-network-security-group"></a><span data-ttu-id="728b5-131">Hálózati biztonsági csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="728b5-131">Create network security group</span></span>

<span data-ttu-id="728b5-132">Egy Azure [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) (NSG) szabályozza a bejövő és kimenő forgalmat egy vagy több virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="728b5-132">An Azure [network security group](../../virtual-network/virtual-networks-nsg.md) (NSG) controls inbound and outbound traffic for one or many virtual machines.</span></span> <span data-ttu-id="728b5-133">Hálózati biztonsági csoportszabályok engedélyezheti vagy letilthatja a hálózati forgalmat egy adott portot vagy porttartományt.</span><span class="sxs-lookup"><span data-stu-id="728b5-133">Network security group rules allow or deny network traffic on a specific port or port range.</span></span> <span data-ttu-id="728b5-134">Ezek a szabályok is hozzáadhat egy forráscímelőtag, hogy csak egy előre meghatározott forrás származó forgalmat a virtuális gépek kommunikálhatnak.</span><span class="sxs-lookup"><span data-stu-id="728b5-134">These rules can also include a source address prefix so that only traffic originating at a predefined source can communicate with a virtual machine.</span></span> <span data-ttu-id="728b5-135">tooaccess hello IIS webkiszolgálót, amely telepíti, hozzá kell adnia egy NSG bejövő szabályt.</span><span class="sxs-lookup"><span data-stu-id="728b5-135">tooaccess hello IIS webserver that you are installing, you must add an inbound NSG rule.</span></span>

<span data-ttu-id="728b5-136">toocreate NSG bejövő szabály, használjon [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="728b5-136">toocreate an inbound NSG rule, use [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="728b5-137">hello következő például egy szabályt hoz létre NSG nevű *myNSGRule* portot nyit meg, amely *3389-es* hello virtuális géphez:</span><span class="sxs-lookup"><span data-stu-id="728b5-137">hello following example creates an NSG rule named *myNSGRule* that opens port *3389* for hello virtual machine:</span></span>

```powershell
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389 `
  -Access Allow
```

<span data-ttu-id="728b5-138">Hozzon létre hello NSG-t használó *myNSGRule* rendelkező [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span><span class="sxs-lookup"><span data-stu-id="728b5-138">Create hello NSG using *myNSGRule* with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupVM `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRule
```

<span data-ttu-id="728b5-139">Hello NSG toohello alhálózat hozzáadása a virtuális hálózat hello [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="728b5-139">Add hello NSG toohello subnet in hello virtual network with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
Set-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -VirtualNetwork $vnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="728b5-140">Frissítés hello virtuális hálózat [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="728b5-140">Update hello virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-machine"></a><span data-ttu-id="728b5-141">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="728b5-141">Create virtual machine</span></span>

<span data-ttu-id="728b5-142">A virtuális gép létrehozásakor több lehetőség is elérhető, például az operációs rendszer lemezképét, a lemez méretezés és a felügyeleti hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="728b5-142">When creating a virtual machine, several options are available such as operating system image, disk sizing, and administrative credentials.</span></span> <span data-ttu-id="728b5-143">Ebben a példában egy virtuális gép létrehozása nevű *myVM* futó hello legújabb verzióját a Windows Server 2016 Datacenter.</span><span class="sxs-lookup"><span data-stu-id="728b5-143">In this example, a virtual machine is created with a name of *myVM* running hello latest version of Windows Server 2016 Datacenter.</span></span>

<span data-ttu-id="728b5-144">Állítsa be a hello felhasználónév és jelszó szükséges hello rendszergazdai fiók hello virtuális gépen a [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="728b5-144">Set hello username and password needed for hello administrator account on hello virtual machine with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="728b5-145">Hozzon létre hello hello virtuális gép kezdeti konfigurálásához [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):</span><span class="sxs-lookup"><span data-stu-id="728b5-145">Create hello initial configuration for hello virtual machine with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1
```

<span data-ttu-id="728b5-146">Adja hozzá a hello operációs rendszer információt toohello virtuálisgép-konfigurációnak [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):</span><span class="sxs-lookup"><span data-stu-id="728b5-146">Add hello operating system information toohello virtual machine configuration with [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):</span></span>

```powershell
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM `
    -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

<span data-ttu-id="728b5-147">Adja hozzá a hello kép információk toohello virtuálisgép-konfiguráció [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):</span><span class="sxs-lookup"><span data-stu-id="728b5-147">Add hello image information toohello virtual machine configuration with [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

<span data-ttu-id="728b5-148">Adja hozzá a hello operációs rendszer lemez beállítások toohello virtuálisgép-konfigurációnak [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):</span><span class="sxs-lookup"><span data-stu-id="728b5-148">Add hello operating system disk settings toohello virtual machine configuration with [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
```

<span data-ttu-id="728b5-149">Hozzáadás hello hálózati kártya korábban létrehozott toohello virtuálisgép-konfigurációnak [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="728b5-149">Add hello network interface card that you previously created toohello virtual machine configuration with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

<span data-ttu-id="728b5-150">Hozzon létre virtuális gép hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="728b5-150">Create hello virtual machine with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span>

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroupVM -Location EastUS -VM $vm
```

## <a name="connect-toovm"></a><span data-ttu-id="728b5-151">Csatlakozás tooVM</span><span class="sxs-lookup"><span data-stu-id="728b5-151">Connect tooVM</span></span>

<span data-ttu-id="728b5-152">Hello telepítés befejeződése után létre kell hoznia egy távoli asztali kapcsolat hello virtuális géppel.</span><span class="sxs-lookup"><span data-stu-id="728b5-152">After hello deployment has completed, create a remote desktop connection with hello virtual machine.</span></span>

<span data-ttu-id="728b5-153">Futtassa a következő parancsok tooreturn hello nyilvános IP-cím hello virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="728b5-153">Run hello following commands tooreturn hello public IP address of hello virtual machine.</span></span> <span data-ttu-id="728b5-154">Jegyezze fel ezt az IP-címet, a böngésző tootest webes kapcsolatot egy későbbi lépésben kapcsolatba léphet a tooit.</span><span class="sxs-lookup"><span data-stu-id="728b5-154">Take note of this IP Address so you can connect tooit with your browser tootest web connectivity in a future step.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupVM  | Select IpAddress
```

<span data-ttu-id="728b5-155">Használjon hello következő parancsot a toocreate egy távoli asztali munkamenet hello virtuális géppel.</span><span class="sxs-lookup"><span data-stu-id="728b5-155">Use hello following command toocreate a remote desktop session with hello virtual machine.</span></span> <span data-ttu-id="728b5-156">Cserélje le hello IP-cím hello *publicIPAddress* a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="728b5-156">Replace hello IP address with hello *publicIPAddress* of your virtual machine.</span></span> <span data-ttu-id="728b5-157">Amikor a rendszer kéri, adja meg a hello virtuális gép létrehozásakor használt hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="728b5-157">When prompted, enter hello credentials used when creating hello virtual machine.</span></span>

```powershell
mstsc /v:<publicIpAddress>
```

## <a name="understand-vm-images"></a><span data-ttu-id="728b5-158">Virtuálisgép-rendszerképek ismertetése</span><span class="sxs-lookup"><span data-stu-id="728b5-158">Understand VM images</span></span>

<span data-ttu-id="728b5-159">hello Azure piactér lehet használt toocreate egy új virtuális gép több virtuálisgép-lemezképeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="728b5-159">hello Azure marketplace includes many virtual machine images that can be used toocreate a new virtual machine.</span></span> <span data-ttu-id="728b5-160">Hello előző lépésekben a virtuális gép létrehozása hello Windows Server 2016-Datacenter rendszerképet használ.</span><span class="sxs-lookup"><span data-stu-id="728b5-160">In hello previous steps, a virtual machine was created using hello Windows Server 2016-Datacenter image.</span></span> <span data-ttu-id="728b5-161">Ebben a lépésben hello PowerShell modul az használt toosearch hello piactér más Windows-lemezképek, amely is alapjaként új virtuális gépek esetén.</span><span class="sxs-lookup"><span data-stu-id="728b5-161">In this step, hello PowerShell module is used toosearch hello marketplace for other Windows images, which can also as a base for new VMs.</span></span> <span data-ttu-id="728b5-162">Ezt a folyamatot, hogy a rendszer hello közzétevő, az ajánlat és hello kép neve (Sku) áll.</span><span class="sxs-lookup"><span data-stu-id="728b5-162">This process consists of finding hello publisher, offer, and hello image name (Sku).</span></span> 

<span data-ttu-id="728b5-163">Használjon hello [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) parancs tooreturn kép közzétevők listájának.</span><span class="sxs-lookup"><span data-stu-id="728b5-163">Use hello [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) command tooreturn a list of image publishers.</span></span>  

```powersehll
Get-AzureRmVMImagePublisher -Location "EastUS"
```

<span data-ttu-id="728b5-164">Használjon hello [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) tooreturn kép ajánlatok listáját.</span><span class="sxs-lookup"><span data-stu-id="728b5-164">Use hello [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) tooreturn a list of image offers.</span></span> <span data-ttu-id="728b5-165">Ezzel a paranccsal hello visszaadott lista a megadott közzétevő hello szűrve van.</span><span class="sxs-lookup"><span data-stu-id="728b5-165">With this command, hello returned list is filtered on hello specified publisher.</span></span> 

```powershell
Get-AzureRmVMImageOffer -Location "EastUS" -PublisherName "MicrosoftWindowsServer"
```

```powershell
Offer             PublisherName          Location
-----             -------------          -------- 
Windows-HUB       MicrosoftWindowsServer EastUS 
WindowsServer     MicrosoftWindowsServer EastUS   
WindowsServer-HUB MicrosoftWindowsServer EastUS   
```

<span data-ttu-id="728b5-166">Hello [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) parancs majd szűrést hello közzétevő és ajánlat tooreturn kép neveinek listáját.</span><span class="sxs-lookup"><span data-stu-id="728b5-166">hello [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) command will then filter on hello publisher and offer name tooreturn a list of image names.</span></span>

```powershell
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

```powershell
Skus                            Offer         PublisherName          Location
----                            -----         -------------          --------
2008-R2-SP1                     WindowsServer MicrosoftWindowsServer EastUS  
2008-R2-SP1-BYOL                WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter                 WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter-BYOL            WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter              WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter-BYOL         WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter                 WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core     WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-with-Containers WindowsServer MicrosoftWindowsServer EastUS  
2016-Nano-Server                WindowsServer MicrosoftWindowsServer EastUS
```

<span data-ttu-id="728b5-167">Ezek az információk használt toodeploy azokba rendelkező virtuális gépeknél lehet.</span><span class="sxs-lookup"><span data-stu-id="728b5-167">This information can be used toodeploy a VM with a specific image.</span></span> <span data-ttu-id="728b5-168">Ez a példa hello Virtuálisgép-objektum hello lemezképnév beállítása.</span><span class="sxs-lookup"><span data-stu-id="728b5-168">This example sets hello image name on hello VM object.</span></span> <span data-ttu-id="728b5-169">Tekintse meg az előző példák toohello ebben az oktatóanyagban a teljes telepítési lépéseket.</span><span class="sxs-lookup"><span data-stu-id="728b5-169">Refer toohello previous examples in this tutorial for complete deployment steps.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter-with-Containers `
    -Version latest
```

## <a name="understand-vm-sizes"></a><span data-ttu-id="728b5-170">Virtuálisgép-méretek ismertetése</span><span class="sxs-lookup"><span data-stu-id="728b5-170">Understand VM sizes</span></span>

<span data-ttu-id="728b5-171">A virtuális gép méretét hello számítási erőforrásokat, elérhető toohello virtuális gépen végrehajtott például a CPU, grafikus Processzor és memória mennyisége határozza meg.</span><span class="sxs-lookup"><span data-stu-id="728b5-171">A virtual machine size determines hello amount of compute resources such as CPU, GPU, and memory that are made available toohello virtual machine.</span></span> <span data-ttu-id="728b5-172">Virtuális gépek kell toobe létrehozott méretű megfelelő hello várható terhelése.</span><span class="sxs-lookup"><span data-stu-id="728b5-172">Virtual machines need toobe created with a size appropriate for hello expect work load.</span></span> <span data-ttu-id="728b5-173">Ha növeli a munkaterhelés, egy meglévő virtuális gépet is átméretezhetők.</span><span class="sxs-lookup"><span data-stu-id="728b5-173">If workload increases, an existing virtual machine can be resized.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="728b5-174">Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="728b5-174">VM Sizes</span></span>

<span data-ttu-id="728b5-175">a következő táblázat hello méretek kategorizálja a használati esetek.</span><span class="sxs-lookup"><span data-stu-id="728b5-175">hello following table categorizes sizes into use cases.</span></span>  

| <span data-ttu-id="728b5-176">Típus</span><span class="sxs-lookup"><span data-stu-id="728b5-176">Type</span></span>                     | <span data-ttu-id="728b5-177">Méretek</span><span class="sxs-lookup"><span data-stu-id="728b5-177">Sizes</span></span>           |    <span data-ttu-id="728b5-178">Leírás</span><span class="sxs-lookup"><span data-stu-id="728b5-178">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="728b5-179">Általános célú</span><span class="sxs-lookup"><span data-stu-id="728b5-179">General purpose</span></span>         |<span data-ttu-id="728b5-180">DSv2, Dv2, DS, D, Av2, A0-7</span><span class="sxs-lookup"><span data-stu-id="728b5-180">DSv2, Dv2, DS, D, Av2, A0-7</span></span>| <span data-ttu-id="728b5-181">Elosztott terhelésű CPU-memória.</span><span class="sxs-lookup"><span data-stu-id="728b5-181">Balanced CPU-to-memory.</span></span> <span data-ttu-id="728b5-182">Épp ezért tökéletes választás a fejlesztési / tesztelési és kis toomedium alkalmazások és adatok megoldások.</span><span class="sxs-lookup"><span data-stu-id="728b5-182">Ideal for dev / test and small toomedium applications and data solutions.</span></span>  |
| <span data-ttu-id="728b5-183">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="728b5-183">Compute optimized</span></span>      | <span data-ttu-id="728b5-184">FS, F</span><span class="sxs-lookup"><span data-stu-id="728b5-184">Fs, F</span></span>             | <span data-ttu-id="728b5-185">Magas CPU-memória.</span><span class="sxs-lookup"><span data-stu-id="728b5-185">High CPU-to-memory.</span></span> <span data-ttu-id="728b5-186">Jó közepes forgalom alkalmazásokat, hálózati berendezéseket és kötegelt folyamatok.</span><span class="sxs-lookup"><span data-stu-id="728b5-186">Good for medium traffic applications, network appliances, and batch processes.</span></span>        |
| <span data-ttu-id="728b5-187">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="728b5-187">Memory optimized</span></span>       | <span data-ttu-id="728b5-188">GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="728b5-188">GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="728b5-189">Magas memória-core.</span><span class="sxs-lookup"><span data-stu-id="728b5-189">High memory-to-core.</span></span> <span data-ttu-id="728b5-190">A relációs adatbázisok, közepes méretű toolarge gyorsítótárak és memórián belüli analytics nagy.</span><span class="sxs-lookup"><span data-stu-id="728b5-190">Great for relational databases, medium toolarge caches, and in-memory analytics.</span></span>                 |
| <span data-ttu-id="728b5-191">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="728b5-191">Storage optimized</span></span>       | <span data-ttu-id="728b5-192">Ls</span><span class="sxs-lookup"><span data-stu-id="728b5-192">Ls</span></span>                | <span data-ttu-id="728b5-193">Magas lemez-adatátviteli és I/O-műveleti jellemzők.</span><span class="sxs-lookup"><span data-stu-id="728b5-193">High disk throughput and IO.</span></span> <span data-ttu-id="728b5-194">Ideális Big Data-, SQL- és NoSQL-adatbázisok esetén.</span><span class="sxs-lookup"><span data-stu-id="728b5-194">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| <span data-ttu-id="728b5-195">GPU</span><span class="sxs-lookup"><span data-stu-id="728b5-195">GPU</span></span>           | <span data-ttu-id="728b5-196">PORTOK HV, NC</span><span class="sxs-lookup"><span data-stu-id="728b5-196">NV, NC</span></span>            | <span data-ttu-id="728b5-197">Nagy mennyiségű grafikus megjelenítési és videó szerkesztése szánt speciális virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="728b5-197">Specialized VMs targeted for heavy graphic rendering and video editing.</span></span>       |
| <span data-ttu-id="728b5-198">Kiemelkedő teljesítmény</span><span class="sxs-lookup"><span data-stu-id="728b5-198">High performance</span></span> | <span data-ttu-id="728b5-199">H, A8-11</span><span class="sxs-lookup"><span data-stu-id="728b5-199">H, A8-11</span></span>          | <span data-ttu-id="728b5-200">A leghatékonyabb CPU rendelkező virtuális gépek nagy átviteli további hálózati adapterek (RDMA).</span><span class="sxs-lookup"><span data-stu-id="728b5-200">Our most powerful CPU VMs with optional high-throughput network interfaces (RDMA).</span></span> 


### <a name="find-available-vm-sizes"></a><span data-ttu-id="728b5-201">Elérhető Virtuálisgép-méretek keresése</span><span class="sxs-lookup"><span data-stu-id="728b5-201">Find available VM sizes</span></span>

<span data-ttu-id="728b5-202">virtuális gép listájának toosee méretének egy adott régióban, használja a hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) parancsot.</span><span class="sxs-lookup"><span data-stu-id="728b5-202">toosee a list of VM sizes available in a particular region, use hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) command.</span></span>

```powershell
Get-AzureRmVMSize -Location EastUS
```

## <a name="resize-a-vm"></a><span data-ttu-id="728b5-203">Virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="728b5-203">Resize a VM</span></span>

<span data-ttu-id="728b5-204">A virtuális gépek telepítése után az átméretezett tooincrease kell, vagy erőforrás-elosztás csökkentése.</span><span class="sxs-lookup"><span data-stu-id="728b5-204">After a VM has been deployed, it can be resized tooincrease or decrease resource allocation.</span></span>

<span data-ttu-id="728b5-205">A virtuális gépek átméretezésével előtt ellenőrizze, hogy hello kívánt mérete hello aktuális Virtuálisgép-fürt érhető el.</span><span class="sxs-lookup"><span data-stu-id="728b5-205">Before resizing a VM, check if hello desired size is available on hello current VM cluster.</span></span> <span data-ttu-id="728b5-206">Hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) parancs méretek listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="728b5-206">hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) command returns a list of sizes.</span></span> 

```powershell
Get-AzureRmVMSize -ResourceGroupName myResourceGroupVM -VMName myVM 
```

<span data-ttu-id="728b5-207">Hello szükséges méret érhető el, ha hello VM átméretezhető az alkalmazás bekapcsolja a állapotból, azonban hello művelet során újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="728b5-207">If hello desired size is available, hello VM can be resized from a powered-on state, however it is rebooted during hello operation.</span></span>

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM 
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
```

<span data-ttu-id="728b5-208">Hello méretének igény nincs fürtön hello aktuális hello virtuális gép igényeinek felszabadítása előtt hello átméretezése művelet toobe akkor fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="728b5-208">If hello desired size is not on hello current cluster, hello VM needs toobe deallocated before hello resize operation can occur.</span></span> <span data-ttu-id="728b5-209">Vegye figyelembe, hello VM regisztráló vissza, bármely hello ideiglenes lemezen lévő adatok törlődnek, és a hello nyilvános IP-cím módosítása, kivéve, ha a statikus IP-cím használatban van.</span><span class="sxs-lookup"><span data-stu-id="728b5-209">Note, when hello VM is powered back on, any data on hello temp disk are removed, and hello public IP address change unless a static IP address is being used.</span></span> 

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
Start-AzureRmVM -ResourceGroupName myResourceGroupVM  -Name $vm.name
```

## <a name="vm-power-states"></a><span data-ttu-id="728b5-210">Virtuális gép energiaállapotokat</span><span class="sxs-lookup"><span data-stu-id="728b5-210">VM power states</span></span>

<span data-ttu-id="728b5-211">Egy Azure virtuális gép sok energiaállapotát egyike lehet.</span><span class="sxs-lookup"><span data-stu-id="728b5-211">An Azure VM can have one of many power states.</span></span> <span data-ttu-id="728b5-212">Ebben az állapotban hello hello virtuális gép aktuális állapotát jeleníti meg hello hipervizor hello tükrözik.</span><span class="sxs-lookup"><span data-stu-id="728b5-212">This state represents hello current state of hello VM from hello standpoint of hello hypervisor.</span></span> 

### <a name="power-states"></a><span data-ttu-id="728b5-213">Energiaállapotokat</span><span class="sxs-lookup"><span data-stu-id="728b5-213">Power states</span></span>

| <span data-ttu-id="728b5-214">Energiaállapot</span><span class="sxs-lookup"><span data-stu-id="728b5-214">Power State</span></span> | <span data-ttu-id="728b5-215">Leírás</span><span class="sxs-lookup"><span data-stu-id="728b5-215">Description</span></span>
|----|----|
| <span data-ttu-id="728b5-216">Indulás alatt</span><span class="sxs-lookup"><span data-stu-id="728b5-216">Starting</span></span> | <span data-ttu-id="728b5-217">Azt jelzi, hogy hello virtuális gép elindul.</span><span class="sxs-lookup"><span data-stu-id="728b5-217">Indicates hello virtual machine is being started.</span></span> |
| <span data-ttu-id="728b5-218">Fut</span><span class="sxs-lookup"><span data-stu-id="728b5-218">Running</span></span> | <span data-ttu-id="728b5-219">Azt jelzi, hogy hello a virtuális gép futása.</span><span class="sxs-lookup"><span data-stu-id="728b5-219">Indicates that hello virtual machine is running.</span></span> |
| <span data-ttu-id="728b5-220">Leállítás</span><span class="sxs-lookup"><span data-stu-id="728b5-220">Stopping</span></span> | <span data-ttu-id="728b5-221">Azt jelzi, hogy hello a virtuális gép leáll.</span><span class="sxs-lookup"><span data-stu-id="728b5-221">Indicates that hello virtual machine is being stopped.</span></span> | 
| <span data-ttu-id="728b5-222">Leállítva</span><span class="sxs-lookup"><span data-stu-id="728b5-222">Stopped</span></span> | <span data-ttu-id="728b5-223">Azt jelzi, hogy hello a virtuális gép le van állítva.</span><span class="sxs-lookup"><span data-stu-id="728b5-223">Indicates that hello virtual machine is stopped.</span></span> <span data-ttu-id="728b5-224">Vegye figyelembe, hogy a virtuális gépek hello leállt állapotban továbbra is fel Önnek költségeket.</span><span class="sxs-lookup"><span data-stu-id="728b5-224">Note that virtual machines in hello stopped state still incur compute charges.</span></span>  |
| <span data-ttu-id="728b5-225">Felszabadítása</span><span class="sxs-lookup"><span data-stu-id="728b5-225">Deallocating</span></span> | <span data-ttu-id="728b5-226">Azt jelzi, hogy hello a virtuális gép felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="728b5-226">Indicates that hello virtual machine is being deallocated.</span></span> |
| <span data-ttu-id="728b5-227">Felszabadítása</span><span class="sxs-lookup"><span data-stu-id="728b5-227">Deallocated</span></span> | <span data-ttu-id="728b5-228">Azt jelzi, hogy hello a virtuális gép teljesen eltávolítja a hello hipervizor, de továbbra is elérhetők a hello vezérlő vezérlősík.</span><span class="sxs-lookup"><span data-stu-id="728b5-228">Indicates that hello virtual machine is completely removed from hello hypervisor but still available in hello control plane.</span></span> <span data-ttu-id="728b5-229">Hello Deallocated állapotban lévő virtuális gépek nem számítunk költségeket.</span><span class="sxs-lookup"><span data-stu-id="728b5-229">Virtual machines in hello Deallocated state do not incur compute charges.</span></span> |
| - | <span data-ttu-id="728b5-230">Azt jelzi, hogy hello power hello virtuális gép állapota ismeretlen.</span><span class="sxs-lookup"><span data-stu-id="728b5-230">Indicates that hello power state of hello virtual machine is unknown.</span></span> |

### <a name="find-power-state"></a><span data-ttu-id="728b5-231">Energiaállapot keresése</span><span class="sxs-lookup"><span data-stu-id="728b5-231">Find power state</span></span>

<span data-ttu-id="728b5-232">egy adott virtuális Gépet, használjon hello tooretrieve hello állapotának [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) parancsot.</span><span class="sxs-lookup"><span data-stu-id="728b5-232">tooretrieve hello state of a particular VM, use hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) command.</span></span> <span data-ttu-id="728b5-233">Lehet, hogy toospecify egy érvényes nevet a virtuális gép és erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="728b5-233">Be sure toospecify a valid name for a virtual machine and resource group.</span></span> 

```powershell
Get-AzureRmVM `
    -ResourceGroupName myResourceGroup `
    -Name myVM `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

<span data-ttu-id="728b5-234">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="728b5-234">Output:</span></span>

```powershell
Status
------
PowerState/running
```

## <a name="management-tasks"></a><span data-ttu-id="728b5-235">Felügyeleti feladatok</span><span class="sxs-lookup"><span data-stu-id="728b5-235">Management tasks</span></span>

<span data-ttu-id="728b5-236">A virtuális gépek hello életciklusa során szükség lehet a toorun felügyeleti feladatokhoz, mint az indítása, leállítása vagy egy virtuális gép törlése.</span><span class="sxs-lookup"><span data-stu-id="728b5-236">During hello lifecycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="728b5-237">Emellett érdemes lehet toocreate parancsfájlok tooautomate ismétlődő vagy összetett feladatokat.</span><span class="sxs-lookup"><span data-stu-id="728b5-237">Additionally, you may want toocreate scripts tooautomate repetitive or complex tasks.</span></span> <span data-ttu-id="728b5-238">Az Azure PowerShell használatával számos gyakori felügyeleti feladatokat is futtatható hello parancssorból vagy parancsfájlokban.</span><span class="sxs-lookup"><span data-stu-id="728b5-238">Using Azure PowerShell, many common management tasks can be run from hello command line or in scripts.</span></span>

### <a name="stop-virtual-machine"></a><span data-ttu-id="728b5-239">Virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="728b5-239">Stop virtual machine</span></span>

<span data-ttu-id="728b5-240">Állítsa le és a virtuális gép felszabadítása [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="728b5-240">Stop and deallocate a virtual machine with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
```

<span data-ttu-id="728b5-241">Ha azt szeretné, hogy tookeep hello virtuális gép kiosztott állapotú, használja a hello - StayProvisioned paramétert.</span><span class="sxs-lookup"><span data-stu-id="728b5-241">If you want tookeep hello virtual machine in a provisioned state, use hello -StayProvisioned parameter.</span></span>

### <a name="start-virtual-machine"></a><span data-ttu-id="728b5-242">Virtuális gép elindítása</span><span class="sxs-lookup"><span data-stu-id="728b5-242">Start virtual machine</span></span>

```powershell
Start-AzureRmVM -ResourceGroupName myResourceGroupVM -Name myVM
```

### <a name="delete-resource-group"></a><span data-ttu-id="728b5-243">Erőforráscsoport törlése</span><span class="sxs-lookup"><span data-stu-id="728b5-243">Delete resource group</span></span>

<span data-ttu-id="728b5-244">Erőforráscsoport törlésekor a belül található összes erőforrást is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="728b5-244">Deleting a resource group also deletes all resources contained within.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroupVM -Force
```

## <a name="next-steps"></a><span data-ttu-id="728b5-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="728b5-245">Next steps</span></span>

<span data-ttu-id="728b5-246">Ebben az oktatóprogramban megismerte alapvető virtuális gép létrehozása és kezelése például hogyan:</span><span class="sxs-lookup"><span data-stu-id="728b5-246">In this tutorial, you learned about basic VM creation and management such as how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="728b5-247">Hozzon létre, és csatlakozzon a virtuális gép tooa</span><span class="sxs-lookup"><span data-stu-id="728b5-247">Create and connect tooa VM</span></span>
> * <span data-ttu-id="728b5-248">Válassza ki és használja a Virtuálisgép-rendszerképek</span><span class="sxs-lookup"><span data-stu-id="728b5-248">Select and use VM images</span></span>
> * <span data-ttu-id="728b5-249">Megtekintése és használata a megadott Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="728b5-249">View and use specific VM sizes</span></span>
> * <span data-ttu-id="728b5-250">Virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="728b5-250">Resize a VM</span></span>
> * <span data-ttu-id="728b5-251">Megtekinteni és megérteni a virtuális gép állapota</span><span class="sxs-lookup"><span data-stu-id="728b5-251">View and understand VM state</span></span>

<span data-ttu-id="728b5-252">Virtuális gép lemezeivel kapcsolatos útmutató toolearn következő toohello előzetes.</span><span class="sxs-lookup"><span data-stu-id="728b5-252">Advance toohello next tutorial toolearn about VM disks.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="728b5-253">Hozzon létre és kezelheti a virtuális gépek lemezei</span><span class="sxs-lookup"><span data-stu-id="728b5-253">Create and Manage VM disks</span></span>](./tutorial-manage-data-disk.md)
