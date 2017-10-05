---
title: "A Windows Azure-ban testreszabása |} Microsoft Docs"
description: "Útmutató az egyéni parancsprogramok futtatására szolgáló bővítmény és a Key Vault segítségével testre szabhatja a Windows-alapú virtuális gépek Azure-ban"
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
ms.openlocfilehash: 3be58bf8afbcff018b2b0d69a0e08c2c9ab1fca7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-customize-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="3eba7-103">A Windows Azure virtuális gép testreszabása</span><span class="sxs-lookup"><span data-stu-id="3eba7-103">How to customize a Windows virtual machine in Azure</span></span>
<span data-ttu-id="3eba7-104">Virtuális gépek (VM) gyors és egységes módon konfigurálásához valamilyen automatizált művelettel általában van szükség.</span><span class="sxs-lookup"><span data-stu-id="3eba7-104">To configure virtual machines (VMs) in a quick and consistent manner, some form of automation is typically desired.</span></span> <span data-ttu-id="3eba7-105">A Windows virtuális gépek testreszabása általános gyakorlatként javasolt, hogy használja [egyéni parancsfájl kiterjesztése a Windows](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="3eba7-105">A common approach to customize a Windows VM is to use [Custom Script Extension for Windows](extensions-customscript.md).</span></span> <span data-ttu-id="3eba7-106">Ezen oktatóanyag segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="3eba7-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3eba7-107">Az egyéni parancsprogramok futtatására szolgáló bővítmény használatával az IIS telepítése</span><span class="sxs-lookup"><span data-stu-id="3eba7-107">Use the Custom Script Extension to install IIS</span></span>
> * <span data-ttu-id="3eba7-108">Az egyéni parancsprogramok futtatására szolgáló bővítmény használó virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="3eba7-108">Create a VM that uses the Custom Script Extension</span></span>
> * <span data-ttu-id="3eba7-109">Egy futó IIS-webhely megtekintheti a bővítmény alkalmazása után</span><span class="sxs-lookup"><span data-stu-id="3eba7-109">View a running IIS site after the extension is applied</span></span>

<span data-ttu-id="3eba7-110">Az oktatóanyaghoz az Azure PowerShell-modul 3.6-os vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="3eba7-110">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="3eba7-111">A verzió azonosításához futtassa a következőt: ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="3eba7-111">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="3eba7-112">Ha frissítenie kell, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="3eba7-112">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="custom-script-extension-overview"></a><span data-ttu-id="3eba7-113">Egyéni parancsfájl-bővítmény áttekintése</span><span class="sxs-lookup"><span data-stu-id="3eba7-113">Custom script extension overview</span></span>
<span data-ttu-id="3eba7-114">Az egyéni parancsprogramok futtatására szolgáló bővítmény és hajtanak végre a parancsfájlok Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="3eba7-114">The Custom Script Extension downloads and executes scripts on Azure VMs.</span></span> <span data-ttu-id="3eba7-115">A bővítmény akkor hasznos, ha a feladás egy vagy több központi telepítés konfigurálása, a szoftver telepítése vagy a más beállításokat / kezelési feladatot.</span><span class="sxs-lookup"><span data-stu-id="3eba7-115">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="3eba7-116">Parancsfájlok le: az Azure storage vagy a Githubon, vagy megadott futásidejű bővítmény: az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="3eba7-116">Scripts can be downloaded from Azure storage or GitHub, or provided to the Azure portal at extension run time.</span></span>

<span data-ttu-id="3eba7-117">Az egyéni parancsprogramok futtatására szolgáló bővítmény integrálódik az Azure Resource Manager-sablonok, és is futtathat az Azure parancssori felület, PowerShell, Azure-portálon vagy az Azure virtuális gép REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="3eba7-117">The Custom Script extension integrates with Azure Resource Manager templates, and can also be run using the Azure CLI, PowerShell, Azure portal, or the Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="3eba7-118">Az egyéni parancsprogramok futtatására szolgáló bővítmény Windows és Linux virtuális gépek egyaránt használható.</span><span class="sxs-lookup"><span data-stu-id="3eba7-118">You can use the Custom Script Extension with both Windows and Linux VMs.</span></span>


## <a name="create-virtual-machine"></a><span data-ttu-id="3eba7-119">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="3eba7-119">Create virtual machine</span></span>
<span data-ttu-id="3eba7-120">A virtuális gépek létrehozása előtt hozzon létre egy erőforráscsoportot, a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="3eba7-120">Before you can create a VM, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="3eba7-121">Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupAutomate* a a *EastUS* helye:</span><span class="sxs-lookup"><span data-stu-id="3eba7-121">The following example creates a resource group named *myResourceGroupAutomate* in the *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location EastUS
```

<span data-ttu-id="3eba7-122">Állítsa a Rendszergazda felhasználónévvel és jelszóval rendelkező virtuális gépek [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="3eba7-122">Set an administrator username and password for the VMs with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="3eba7-123">Most a virtuális Géphez a hozhat létre [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="3eba7-123">Now you can create the VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="3eba7-124">Az alábbi példa hoz létre a szükséges virtuális hálózati összetevők, az operációs rendszer konfigurációja, és létrehoz egy nevű virtuális gép *myVM*:</span><span class="sxs-lookup"><span data-stu-id="3eba7-124">The following example creates the required virtual network components, the OS configuration, and then creates a VM named *myVM*:</span></span>

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

<span data-ttu-id="3eba7-125">Az erőforrások és a virtuális gép, létre kell néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="3eba7-125">It takes a few minutes for the resources and VM to be created.</span></span>


## <a name="automate-iis-install"></a><span data-ttu-id="3eba7-126">Az IIS-telepítés automatizálása</span><span class="sxs-lookup"><span data-stu-id="3eba7-126">Automate IIS install</span></span>
<span data-ttu-id="3eba7-127">Használjon [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) az egyéni parancsprogramok futtatására szolgáló bővítmény telepítése.</span><span class="sxs-lookup"><span data-stu-id="3eba7-127">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) to install the Custom Script Extension.</span></span> <span data-ttu-id="3eba7-128">A bővítmény fut `powershell Add-WindowsFeature Web-Server` az IIS webkiszolgálót, és a frissítések telepítése a *Default.htm* a virtuális gép állomásnevét lapjait:</span><span class="sxs-lookup"><span data-stu-id="3eba7-128">The extension runs `powershell Add-WindowsFeature Web-Server` to install the IIS webserver and then updates the *Default.htm* page to show the hostname of the VM:</span></span>

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


## <a name="test-web-site"></a><span data-ttu-id="3eba7-129">Teszt webhely</span><span class="sxs-lookup"><span data-stu-id="3eba7-129">Test web site</span></span>
<span data-ttu-id="3eba7-130">Szerezze be a terheléselosztó a nyilvános IP-címe [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="3eba7-130">Obtain the public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="3eba7-131">Az alábbi példa beolvassa az IP-címek *myPublicIP* korábban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="3eba7-131">The following example obtains the IP address for *myPublicIP* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

<span data-ttu-id="3eba7-132">Beírhatja a nyilvános IP-címet a webböngésző.</span><span class="sxs-lookup"><span data-stu-id="3eba7-132">You can then enter the public IP address in to a web browser.</span></span> <span data-ttu-id="3eba7-133">A webhely jelenik meg, beleértve az állomásnevet, a virtuális gép, amelyek a terheléselosztó felé irányuló forgalom az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="3eba7-133">The website is displayed, including the hostname of the VM that the load balancer distributed traffic to as in the following example:</span></span>

![Futó IIS-webhely](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a><span data-ttu-id="3eba7-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3eba7-135">Next steps</span></span>

<span data-ttu-id="3eba7-136">Ebben az oktatóanyagban az IIS telepítése a virtuális gép automatikus.</span><span class="sxs-lookup"><span data-stu-id="3eba7-136">In this tutorial, you automated the IIS install on a VM.</span></span> <span data-ttu-id="3eba7-137">Megtudta, hogyan, hogy:</span><span class="sxs-lookup"><span data-stu-id="3eba7-137">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3eba7-138">Az egyéni parancsprogramok futtatására szolgáló bővítmény használatával az IIS telepítése</span><span class="sxs-lookup"><span data-stu-id="3eba7-138">Use the Custom Script Extension to install IIS</span></span>
> * <span data-ttu-id="3eba7-139">Az egyéni parancsprogramok futtatására szolgáló bővítmény használó virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="3eba7-139">Create a VM that uses the Custom Script Extension</span></span>
> * <span data-ttu-id="3eba7-140">Egy futó IIS-webhely megtekintheti a bővítmény alkalmazása után</span><span class="sxs-lookup"><span data-stu-id="3eba7-140">View a running IIS site after the extension is applied</span></span>

<span data-ttu-id="3eba7-141">Továbblépés a következő oktatóanyag áttekintésével megismerheti, hogyan hozza létre egyéni Virtuálisgép-lemezképeket.</span><span class="sxs-lookup"><span data-stu-id="3eba7-141">Advance to the next tutorial to learn how to create custom VM images.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3eba7-142">Egyéni virtuálisgép-rendszerképek létrehozása</span><span class="sxs-lookup"><span data-stu-id="3eba7-142">Create custom VM images</span></span>](./tutorial-custom-images.md)
