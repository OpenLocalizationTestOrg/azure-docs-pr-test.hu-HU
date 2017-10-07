---
title: a Windows Azure-ban aaaCustomize |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello egyéni parancsprogramok futtatására szolgáló bővítmény és a Windows-alapú virtuális gépek az Azure Key Vault toocustomize"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c03b2bb6d70875134c63ea2fe4c2e2c1777c2188
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocustomize-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="ae927-103">Hogyan toocustomize Windows virtuális gépként az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="ae927-103">How toocustomize a Windows virtual machine in Azure</span></span>
<span data-ttu-id="ae927-104">tooconfigure virtuális gépek (VM) gyors és konzisztens módon, valamilyen automatizált művelettel általában van szükség.</span><span class="sxs-lookup"><span data-stu-id="ae927-104">tooconfigure virtual machines (VMs) in a quick and consistent manner, some form of automation is typically desired.</span></span> <span data-ttu-id="ae927-105">Egy általános módszer toocustomize egy Windows virtuális gép toouse [egyéni parancsfájl kiterjesztése a Windows](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="ae927-105">A common approach toocustomize a Windows VM is toouse [Custom Script Extension for Windows](extensions-customscript.md).</span></span> <span data-ttu-id="ae927-106">Ezen oktatóanyag segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="ae927-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ae927-107">Hello egyéni parancsprogramok futtatására szolgáló bővítmény tooinstall IIS használata</span><span class="sxs-lookup"><span data-stu-id="ae927-107">Use hello Custom Script Extension tooinstall IIS</span></span>
> * <span data-ttu-id="ae927-108">Egyéni parancsprogramok futtatására szolgáló bővítmény hello használó virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae927-108">Create a VM that uses hello Custom Script Extension</span></span>
> * <span data-ttu-id="ae927-109">Egy futó IIS-webhely megtekintése hello bővítmény alkalmazása után</span><span class="sxs-lookup"><span data-stu-id="ae927-109">View a running IIS site after hello extension is applied</span></span>

<span data-ttu-id="ae927-110">Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="ae927-110">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="ae927-111">Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="ae927-111">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="ae927-112">Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="ae927-112">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="custom-script-extension-overview"></a><span data-ttu-id="ae927-113">Egyéni parancsfájl-bővítmény áttekintése</span><span class="sxs-lookup"><span data-stu-id="ae927-113">Custom script extension overview</span></span>
<span data-ttu-id="ae927-114">Egyéni parancsprogramok futtatására szolgáló bővítmény hello és hajtanak végre a parancsfájlok Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="ae927-114">hello Custom Script Extension downloads and executes scripts on Azure VMs.</span></span> <span data-ttu-id="ae927-115">A bővítmény akkor hasznos, ha a feladás egy vagy több központi telepítés konfigurálása, a szoftver telepítése vagy a más beállításokat / kezelési feladatot.</span><span class="sxs-lookup"><span data-stu-id="ae927-115">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="ae927-116">Parancsfájlok le: az Azure storage vagy a Githubon, vagy megadott toohello az Azure portálon, a bővítmény futási időt.</span><span class="sxs-lookup"><span data-stu-id="ae927-116">Scripts can be downloaded from Azure storage or GitHub, or provided toohello Azure portal at extension run time.</span></span>

<span data-ttu-id="ae927-117">Egyéni parancsprogramok futtatására szolgáló bővítmény hello integrálódik az Azure Resource Manager-sablonok, és is futtathat a hello Azure CLI, PowerShell, Azure-portálon vagy hello Azure virtuális gép REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="ae927-117">hello Custom Script extension integrates with Azure Resource Manager templates, and can also be run using hello Azure CLI, PowerShell, Azure portal, or hello Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="ae927-118">Windows és Linux virtuális gépek hello egyéni parancsprogramok futtatására szolgáló bővítmény használható.</span><span class="sxs-lookup"><span data-stu-id="ae927-118">You can use hello Custom Script Extension with both Windows and Linux VMs.</span></span>


## <a name="create-virtual-machine"></a><span data-ttu-id="ae927-119">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae927-119">Create virtual machine</span></span>
<span data-ttu-id="ae927-120">A virtuális gépek létrehozása előtt hozzon létre egy erőforráscsoportot, a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="ae927-120">Before you can create a VM, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="ae927-121">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupAutomate* a hello *EastUS* helye:</span><span class="sxs-lookup"><span data-stu-id="ae927-121">hello following example creates a resource group named *myResourceGroupAutomate* in hello *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location EastUS
```

<span data-ttu-id="ae927-122">Állítsa a rendszergazda felhasználónevét és jelszavát hello rendelkező virtuális gépek [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="ae927-122">Set an administrator username and password for hello VMs with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="ae927-123">Most hozhat létre a virtuális gép hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="ae927-123">Now you can create hello VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="ae927-124">hello alábbi példa létrehoz hello szükséges virtuális hálózati összetevők hello az operációs rendszer konfigurációs, és létrehoz egy nevű virtuális gép *myVM*:</span><span class="sxs-lookup"><span data-stu-id="ae927-124">hello following example creates hello required virtual network components, hello OS configuration, and then creates a VM named *myVM*:</span></span>

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleWWW  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName myResourceGroupAutomate -Location EastUS -VM $vmConfig
```

<span data-ttu-id="ae927-125">Hello erőforrások és a létrehozott virtuális gép toobe néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="ae927-125">It takes a few minutes for hello resources and VM toobe created.</span></span>


## <a name="automate-iis-install"></a><span data-ttu-id="ae927-126">Az IIS-telepítés automatizálása</span><span class="sxs-lookup"><span data-stu-id="ae927-126">Automate IIS install</span></span>
<span data-ttu-id="ae927-127">Használjon [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello egyéni parancsprogramok futtatására szolgáló bővítmény.</span><span class="sxs-lookup"><span data-stu-id="ae927-127">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello Custom Script Extension.</span></span> <span data-ttu-id="ae927-128">bővítmény futtatása hello `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webkiszolgálót, majd a frissítések hello *Default.htm* lap tooshow hello hello virtuális gép állomásnevét:</span><span class="sxs-lookup"><span data-stu-id="ae927-128">hello extension runs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webserver and then updates hello *Default.htm* page tooshow hello hostname of hello VM:</span></span>

```powershell
Set-AzureRmVMExtension -ResourceGroupName myResourceGroupAutomate `
    -ExtensionName IIS `
    -VMName myVM `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
```


## <a name="test-web-site"></a><span data-ttu-id="ae927-129">Teszt webhely</span><span class="sxs-lookup"><span data-stu-id="ae927-129">Test web site</span></span>
<span data-ttu-id="ae927-130">Hello nyilvános IP-cím a terheléselosztó az beszerzése [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="ae927-130">Obtain hello public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="ae927-131">hello alábbi példa beszerzi hello IP-címet *myPublicIP* korábban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="ae927-131">hello following example obtains hello IP address for *myPublicIP* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

<span data-ttu-id="ae927-132">Majd tooa webböngészőben hello nyilvános IP-címet adhat meg.</span><span class="sxs-lookup"><span data-stu-id="ae927-132">You can then enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="ae927-133">hello webhely jelenik meg, beleértve a virtuális gép hello hello állomásnevét adott hello terheléselosztó forgalom tooas hello a következő példa az elosztott:</span><span class="sxs-lookup"><span data-stu-id="ae927-133">hello website is displayed, including hello hostname of hello VM that hello load balancer distributed traffic tooas in hello following example:</span></span>

![Futó IIS-webhely](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a><span data-ttu-id="ae927-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ae927-135">Next steps</span></span>

<span data-ttu-id="ae927-136">Ebben az oktatóanyagban automatikus hello IIS telepítése egy virtuális Gépre.</span><span class="sxs-lookup"><span data-stu-id="ae927-136">In this tutorial, you automated hello IIS install on a VM.</span></span> <span data-ttu-id="ae927-137">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="ae927-137">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ae927-138">Hello egyéni parancsprogramok futtatására szolgáló bővítmény tooinstall IIS használata</span><span class="sxs-lookup"><span data-stu-id="ae927-138">Use hello Custom Script Extension tooinstall IIS</span></span>
> * <span data-ttu-id="ae927-139">Egyéni parancsprogramok futtatására szolgáló bővítmény hello használó virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae927-139">Create a VM that uses hello Custom Script Extension</span></span>
> * <span data-ttu-id="ae927-140">Egy futó IIS-webhely megtekintése hello bővítmény alkalmazása után</span><span class="sxs-lookup"><span data-stu-id="ae927-140">View a running IIS site after hello extension is applied</span></span>

<span data-ttu-id="ae927-141">Hogyan előzetes toohello következő útmutató toolearn toocreate egyéni Virtuálisgép-lemezképek.</span><span class="sxs-lookup"><span data-stu-id="ae927-141">Advance toohello next tutorial toolearn how toocreate custom VM images.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ae927-142">Egyéni virtuálisgép-rendszerképek létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae927-142">Create custom VM images</span></span>](./tutorial-custom-images.md)
