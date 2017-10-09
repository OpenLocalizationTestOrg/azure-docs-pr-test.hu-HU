---
title: "aaaAzure virtuális hálózatok és a Windows virtuális gépek |} Microsoft Docs"
description: "Az oktatóanyag - kezelése az Azure virtuális hálózatok és a Windows virtuális gépek az Azure PowerShell használatával"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: ed77d9d5873e849fcb2aaf15e41899d7ad8c781a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a><span data-ttu-id="10724-103">Egy Azure virtuális hálózatot és az Azure PowerShell használatával Windows virtuális gépek kezelése</span><span class="sxs-lookup"><span data-stu-id="10724-103">Manage Azure Virtual Networks and Windows Virtual Machines with Azure PowerShell</span></span>

<span data-ttu-id="10724-104">Az Azure virtuális gépek Azure hálózatkezelés belső és külső hálózati kommunikációhoz használni.</span><span class="sxs-lookup"><span data-stu-id="10724-104">Azure virtual machines use Azure networking for internal and external network communication.</span></span> <span data-ttu-id="10724-105">Ebben az oktatóanyagban hozzon létre több virtuális gép (VM) virtuális hálózatban, és konfigurálja őket közötti hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="10724-105">In this tutorial, you create multiple virtual machines (VMs) in a virtual network and configure network connectivity between them.</span></span> <span data-ttu-id="10724-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="10724-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="10724-107">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="10724-107">Create a virtual network</span></span>
> * <span data-ttu-id="10724-108">Virtuális hálózati alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="10724-108">Create virtual network subnets</span></span>
> * <span data-ttu-id="10724-109">Szabályozza a hálózati forgalmat hálózati biztonsági csoportokkal</span><span class="sxs-lookup"><span data-stu-id="10724-109">Control network traffic with Network Security Groups</span></span>
> * <span data-ttu-id="10724-110">A művelet forgalmi szabályok megtekintése</span><span class="sxs-lookup"><span data-stu-id="10724-110">View traffic rules in action</span></span>

<span data-ttu-id="10724-111">Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="10724-111">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="10724-112">Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="10724-112">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="10724-113">Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="10724-113">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="create-vnet"></a><span data-ttu-id="10724-114">VNet létrehozása</span><span class="sxs-lookup"><span data-stu-id="10724-114">Create VNet</span></span>

<span data-ttu-id="10724-115">A virtuális hálózat a saját hálózati hello felhőben megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="10724-115">A VNet is a representation of your own network in hello cloud.</span></span> <span data-ttu-id="10724-116">A virtuális hálózat hello Azure felhőben dedikált tooyour előfizetés logikai elkülönítése.</span><span class="sxs-lookup"><span data-stu-id="10724-116">A VNet is a logical isolation of hello Azure cloud dedicated tooyour subscription.</span></span> <span data-ttu-id="10724-117">Egy Vneten belül található alhálózatok felderítéséhez, a kapcsolat toothose alhálózatok és kapcsolatok hello virtuális gépek toohello alhálózatok szabályait.</span><span class="sxs-lookup"><span data-stu-id="10724-117">Within a VNet, you find subnets, rules for connectivity toothose subnets, and connections from hello VMs toohello subnets.</span></span>

<span data-ttu-id="10724-118">Bármely más Azure-erőforrások létrehozása előtt kell toocreate nevű erőforráscsoport [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="10724-118">Before you can create any other Azure resources, you need toocreate a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="10724-119">hello alábbi példa létrehoz egy erőforráscsoportot *myRGNetwork* a hello *EastUS* helye:</span><span class="sxs-lookup"><span data-stu-id="10724-119">hello following example creates a resource group named *myRGNetwork* in hello *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

<span data-ttu-id="10724-120">Egy alhálózat egy Vnetet gyermek erőforrása, és segítségével meghatározhatja a címterek belül használja az IP-cím előtagokat CIDR-blokkja részeit.</span><span class="sxs-lookup"><span data-stu-id="10724-120">A subnet is a child resource of a VNet, and helps define segments of address spaces within a CIDR block, using IP address prefixes.</span></span> <span data-ttu-id="10724-121">Hálózati adapter toosubnets, és a csatlakoztatott tooVMs biztosítható a kapcsolat a különböző munkaterhelések lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="10724-121">NICs can be added toosubnets, and connected tooVMs, providing connectivity for various workloads.</span></span>

<span data-ttu-id="10724-122">Hozzon létre egy alhálózat [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="10724-122">Create a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="10724-123">Hozzon létre egy VNETET nevű *myVNet* használatával *myFrontendSubnet* rendelkező [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="10724-123">Create a VNET named *myVNet* using *myFrontendSubnet* with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet
```

## <a name="create-front-end-vm"></a><span data-ttu-id="10724-124">Előtér-virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="10724-124">Create front-end VM</span></span>

<span data-ttu-id="10724-125">A virtuális gép toocommunicate a Vneten belül, a virtuális hálózati kártya (NIC) szükséges.</span><span class="sxs-lookup"><span data-stu-id="10724-125">For a VM toocommunicate in a VNet, it needs a virtual network interface (NIC).</span></span> <span data-ttu-id="10724-126">Hello *myFrontendVM* hello elérése internet, így kell továbbá egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="10724-126">hello *myFrontendVM* is accessed from hello internet, so it also needs a public IP address.</span></span> 

<span data-ttu-id="10724-127">Hozzon létre egy nyilvános IP-cím [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span><span class="sxs-lookup"><span data-stu-id="10724-127">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

<span data-ttu-id="10724-128">Hozzon létre egy hálózati Adaptert a [új AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="10724-128">Create a NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span></span>


```powershell
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

<span data-ttu-id="10724-129">Hello felhasználónév és jelszó szükséges a virtuális gép hello hello rendszergazdai fiók beállítása az [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="10724-129">Set hello username and password needed for hello administrator account on hello VM with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="10724-130">Hozzon létre virtuális gépek hello [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Hozzáadása AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), és [új AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="10724-130">Create hello VMs with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), and [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> 

```powershell
$frontendVM = New-AzureRmVMConfig `
    -VMName myFrontendVM `
    -VMSize Standard_D1
$frontendVM = Set-AzureRmVMOperatingSystem `
    -VM $frontendVM `
    -Windows `
    -ComputerName myFrontendVM `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$frontendVM = Set-AzureRmVMSourceImage `
    -VM $frontendVM `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
$frontendVM = Set-AzureRmVMOSDisk `
    -VM $frontendVM `
    -Name myFrontendOSDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
$frontendVM = Add-AzureRmVMNetworkInterface `
    -VM $frontendVM `
    -Id $frontendNic.Id
New-AzureRmVM `
    -ResourceGroupName myRGNetwork `
    -Location EastUS `
    -VM $frontendVM
```

## <a name="install-web-server"></a><span data-ttu-id="10724-131">Webalkalmazás-kiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="10724-131">Install web server</span></span>

<span data-ttu-id="10724-132">Telepítheti az IIS *myFrontendVM* távoli asztali kapcsolat segítségével.</span><span class="sxs-lookup"><span data-stu-id="10724-132">You can install IIS on *myFrontendVM* by using a remote desktop session.</span></span> <span data-ttu-id="10724-133">Tooget hello nyilvános IP-címe hello VM tooaccess kell azt.</span><span class="sxs-lookup"><span data-stu-id="10724-133">You need tooget hello public IP address of hello VM tooaccess it.</span></span>

<span data-ttu-id="10724-134">Hello nyilvános IP-címe kaphat *myFrontendVM* rendelkező [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="10724-134">You can get hello public IP address of *myFrontendVM* with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="10724-135">hello alábbi példa beszerzi hello IP-címet *myPublicIPAddress* korábban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="10724-135">hello following example obtains hello IP address for *myPublicIPAddress* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myRGNetwork `
    -Name myPublicIPAddress | select IpAddress
```

<span data-ttu-id="10724-136">Jegyezze fel ezt az IP-címet, hogy használhassa a későbbi lépésekben.</span><span class="sxs-lookup"><span data-stu-id="10724-136">Take note of this IP Address so you can use it in future steps.</span></span>

<span data-ttu-id="10724-137">Használjon hello következő parancsot a távoli asztali munkamenetet a toocreate *myFrontendVM*.</span><span class="sxs-lookup"><span data-stu-id="10724-137">Use hello following command toocreate a remote desktop session with *myFrontendVM*.</span></span> <span data-ttu-id="10724-138">Cserélje le  *<publicIPAddress>*  korábban feljegyzett hello címmel.</span><span class="sxs-lookup"><span data-stu-id="10724-138">Replace *<publicIPAddress>* with hello address that you previously recorded.</span></span> <span data-ttu-id="10724-139">Amikor a rendszer kéri, adja meg a hello virtuális gép létrehozásakor használt hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="10724-139">When prompted, enter hello credentials used when you created hello VM.</span></span>

```
mstsc /v:<publicIpAddress>
``` 

<span data-ttu-id="10724-140">Most, hogy már bejelentkezett túl*myFrontendVM*, használhatja a PowerShell tooinstall IIS egysoros és hello helyi tűzfal szabály tooallow webes forgalom engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="10724-140">Now that you have logged in too*myFrontendVM*, you can use a single line of PowerShell tooinstall IIS and enable hello local firewall rule tooallow web traffic.</span></span> <span data-ttu-id="10724-141">Nyisson meg egy PowerShell-parancssorba, és futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="10724-141">Open a PowerShell prompt and run hello following command:</span></span>

<span data-ttu-id="10724-142">Használjon [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) toorun hello egyéni parancsprogramok futtatására szolgáló bővítmény, amely telepíti az IIS-webkiszolgáló hello:</span><span class="sxs-lookup"><span data-stu-id="10724-142">Use [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) toorun hello custom script extension that installs hello IIS webserver:</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

<span data-ttu-id="10724-143">Most hello nyilvános IP cím toobrowse toohello VM toosee hello IIS-webhely is használhatja.</span><span class="sxs-lookup"><span data-stu-id="10724-143">Now you can use hello public IP address toobrowse toohello VM toosee hello IIS site.</span></span>

![Alapértelmezett IIS-webhely](./media/tutorial-virtual-network/iis.png)

## <a name="manage-internal-traffic"></a><span data-ttu-id="10724-145">Belső forgalom kezelése</span><span class="sxs-lookup"><span data-stu-id="10724-145">Manage internal traffic</span></span>

<span data-ttu-id="10724-146">A hálózati biztonsági csoport (NSG) szabályokat, amelyek engedélyezik vagy megtagadják a hálózati forgalom csatlakoztatott tooresources tooa hálózatok listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="10724-146">A network security group (NSG) contains a list of security rules that allow or deny network traffic tooresources connected tooa VNet.</span></span> <span data-ttu-id="10724-147">NSG-ket lehet társított toosubnets, vagy az egyes hálózati adapterek csatlakoztatva tooVMs.</span><span class="sxs-lookup"><span data-stu-id="10724-147">NSGs can be associated toosubnets or individual NICs attached tooVMs.</span></span> <span data-ttu-id="10724-148">Megnyitása vagy bezárása hozzáférés tooVMs portokon keresztül történik NSG-szabályok használatával.</span><span class="sxs-lookup"><span data-stu-id="10724-148">Opening or closing access tooVMs through ports is done using NSG rules.</span></span> <span data-ttu-id="10724-149">Létrehozási *myFrontendVM*, 3389-es port bejövő automatikusan nyitották meg az RDP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="10724-149">When you created *myFrontendVM*, inbound port 3389 was automatically opened for RDP connectivity.</span></span>

<span data-ttu-id="10724-150">A virtuális gépek belső kommunikációs egy NSG használatával konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="10724-150">Internal communication of VMs can be configured using an NSG.</span></span> <span data-ttu-id="10724-151">Ebben a szakaszban megismerheti, hogyan toocreate egy további alhálózati hello a hálózatot, és rendelje hozzá egy NSG tooit tooallow kapcsolatot *myFrontendVM* túl*myBackendVM* 1433-as portot.</span><span class="sxs-lookup"><span data-stu-id="10724-151">In this section, you learn how toocreate an additional subnet in hello network and assign an NSG tooit tooallow a connection from *myFrontendVM* too*myBackendVM* on port 1433.</span></span> <span data-ttu-id="10724-152">hello alhálózati majd toohello VM van hozzárendelve, létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="10724-152">hello subnet is then assigned toohello VM when it is created.</span></span>

<span data-ttu-id="10724-153">Belső forgalom korlátozhatja túl*myBackendVM* csak *myFrontendVM* hozzon létre egy NSG-t hello háttér-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="10724-153">You can limit internal traffic too*myBackendVM* from only *myFrontendVM* by creating an NSG for hello back-end subnet.</span></span> <span data-ttu-id="10724-154">hello következő például egy szabályt hoz létre NSG nevű *myBackendNSGRule* rendelkező [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):</span><span class="sxs-lookup"><span data-stu-id="10724-154">hello following example creates an NSG rule named *myBackendNSGRule* with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):</span></span>

```powershell
$nsgBackendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myBackendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix 10.0.0.0/24 `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 1433 `
  -Access Allow
```

<span data-ttu-id="10724-155">Adja hozzá a hálózati biztonsági csoport nevű *myBackendNSG* rendelkező [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span><span class="sxs-lookup"><span data-stu-id="10724-155">Add a network security group named *myBackendNSG* with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span></span>

```powershell
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```
## <a name="add-back-end-subnet"></a><span data-ttu-id="10724-156">Háttér-alhálózat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="10724-156">Add back-end subnet</span></span>

<span data-ttu-id="10724-157">Hozzáadás *myBackEndSubnet* túl*myVNet* rendelkező [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="10724-157">Add *myBackEndSubnet* too*myVNet* with [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
Add-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -VirtualNetwork $vnet `
  -AddressPrefix 10.0.1.0/24 `
  -NetworkSecurityGroup $nsgBackend
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Name myVNet
```

## <a name="create-back-end-vm"></a><span data-ttu-id="10724-158">Háttér-virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="10724-158">Create back-end VM</span></span>

<span data-ttu-id="10724-159">hello legegyszerűbb módja toocreate hello háttér-virtuális gép van egy SQL Server-lemezkép használatával.</span><span class="sxs-lookup"><span data-stu-id="10724-159">hello easiest way toocreate hello back-end VM is by using a SQL Server image.</span></span> <span data-ttu-id="10724-160">Ez az oktatóanyag csak hoz létre a virtuális gép hello hello adatbázis-kiszolgáló, de nem adja meg a hello adatbázis elérésével kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="10724-160">This tutorial only creates hello VM with hello database server, but doesn't provide information about accessing hello database.</span></span>

<span data-ttu-id="10724-161">Hozzon létre *myBackendNic*:</span><span class="sxs-lookup"><span data-stu-id="10724-161">Create *myBackendNic*:</span></span>

```powershell
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

<span data-ttu-id="10724-162">Állítsa be a hello felhasználónév és a virtuális gép és a Get-Credential hello hello rendszergazdai fiók szükséges jelszót:</span><span class="sxs-lookup"><span data-stu-id="10724-162">Set hello username and password needed for hello administrator account on hello VM with Get-Credential:</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="10724-163">Hozzon létre *myBackendVM*:</span><span class="sxs-lookup"><span data-stu-id="10724-163">Create *myBackendVM*:</span></span>

```powershell
$backendVM = New-AzureRmVMConfig `
  -VMName myBackendVM `
  -VMSize Standard_D1
$backendVM = Set-AzureRmVMOperatingSystem `
  -VM $backendVM `
  -Windows `
  -ComputerName myBackendVM `
  -Credential $cred `
  -ProvisionVMAgent `
  -EnableAutoUpdate
$backendVM = Set-AzureRmVMSourceImage `
  -VM $backendVM `
  -PublisherName MicrosoftSQLServer `
  -Offer SQL2016SP1-WS2016 `
  -Skus Enterprise `
  -Version latest
$backendVM = Set-AzureRmVMOSDisk `
  -VM $backendVM `
  -Name myBackendOSDisk `
  -DiskSizeInGB 128 `
  -CreateOption FromImage `
  -Caching ReadWrite
$backendVM = Add-AzureRmVMNetworkInterface `
  -VM $backendVM `
  -Id $backendNic.Id
New-AzureRmVM `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -VM $backendVM
```

<span data-ttu-id="10724-164">hello kép használt SQL Server telepítve van, de nem szerepel ebben az oktatóanyagban.</span><span class="sxs-lookup"><span data-stu-id="10724-164">hello image that is used has SQL Server installed, but is not used in this tutorial.</span></span> <span data-ttu-id="10724-165">Fontos a része tooshow, hogyan konfigurálhat egy virtuális gép toohandle webes forgalom és a virtuális gép toohandle adatbázisának kezelése.</span><span class="sxs-lookup"><span data-stu-id="10724-165">It is included tooshow you how you can configure a VM toohandle web traffic and a VM toohandle database management.</span></span>

## <a name="next-steps"></a><span data-ttu-id="10724-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10724-166">Next steps</span></span>

<span data-ttu-id="10724-167">Ebben az oktatóanyagban létre, és biztonságos kapcsolódó toovirtual gépként Azure hálózatokhoz.</span><span class="sxs-lookup"><span data-stu-id="10724-167">In this tutorial, you created and secured Azure networks as related toovirtual machines.</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="10724-168">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="10724-168">Create a virtual network</span></span>
> * <span data-ttu-id="10724-169">Virtuális hálózati alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="10724-169">Create virtual network subnets</span></span>
> * <span data-ttu-id="10724-170">Szabályozza a hálózati forgalmat hálózati biztonsági csoportokkal</span><span class="sxs-lookup"><span data-stu-id="10724-170">Control network traffic with Network Security Groups</span></span>
> * <span data-ttu-id="10724-171">A művelet forgalmi szabályok megtekintése</span><span class="sxs-lookup"><span data-stu-id="10724-171">View traffic rules in action</span></span>

<span data-ttu-id="10724-172">A virtuális gépek az Azure backup használatával biztonságossá tétele adatok figyelésével kapcsolatos útmutató toolearn következő toohello előzetes.</span><span class="sxs-lookup"><span data-stu-id="10724-172">Advance toohello next tutorial toolearn about monitoring securing data on virtual machines using Azure backup.</span></span> <span data-ttu-id="10724-173">.</span><span class="sxs-lookup"><span data-stu-id="10724-173">.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="10724-174">Készítsen biztonsági másolatot a Windows virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="10724-174">Back up Windows virtual machines in Azure</span></span>](./tutorial-backup-vms.md)
