---
title: "gyors üzembe helyezés – aaaAzure létrehozása Windows virtuális gép PowerShell |} Microsoft Docs"
description: "Gyorsan további toocreate egy Windows virtuális gépek a PowerShell használatával"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5e92435bf7ee443a01c158fed91c356363e2f425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell"></a><span data-ttu-id="374d1-103">Windows rendszerű virtuális gép létrehozása PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="374d1-103">Create a Windows virtual machine with PowerShell</span></span>

<span data-ttu-id="374d1-104">hello Azure PowerShell modul használt toocreate és hello PowerShell parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="374d1-104">hello Azure PowerShell module is used toocreate and manage Azure resources from hello PowerShell command line or in scripts.</span></span> <span data-ttu-id="374d1-105">Ez az útmutató adatokat PowerShell toocreate és a Windows Server 2016 futó Azure virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="374d1-105">This guide details using PowerShell toocreate and Azure virtual machine running Windows Server 2016.</span></span> <span data-ttu-id="374d1-106">Központi telepítés befejezése után azt csatlakoztassa toohello a kiszolgálót, és telepítse az IIS.</span><span class="sxs-lookup"><span data-stu-id="374d1-106">Once deployment is complete, we connect toohello server and install IIS.</span></span>  

<span data-ttu-id="374d1-107">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="374d1-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="374d1-108">A gyors üzembe helyezési hello Azure PowerShell 3,6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="374d1-108">This quick start requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="374d1-109">Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="374d1-109">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="374d1-110">Ha tooinstall vagy frissítés van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="374d1-110">If you need tooinstall or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="374d1-111">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="374d1-111">Log in tooAzure</span></span>

<span data-ttu-id="374d1-112">Jelentkezzen be Azure előfizetés hello tooyour `Login-AzureRmAccount` parancsot, és kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="374d1-112">Log in tooyour Azure subscription with hello `Login-AzureRmAccount` command and follow hello on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a><span data-ttu-id="374d1-113">Erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="374d1-113">Create resource group</span></span>

<span data-ttu-id="374d1-114">Hozzon létre egy Azure-erőforráscsoportot a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="374d1-114">Create an Azure resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="374d1-115">Az erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="374d1-115">A resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-networking-resources"></a><span data-ttu-id="374d1-116">Hálózati erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="374d1-116">Create networking resources</span></span>

### <a name="create-a-virtual-network-subnet-and-a-public-ip-address"></a><span data-ttu-id="374d1-117">Hozzon létre egy virtuális hálózatot, egy alhálózatot és egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="374d1-117">Create a virtual network, subnet, and a public IP address.</span></span> 
<span data-ttu-id="374d1-118">Ezeket az erőforrásokat használt tooprovide hálózati kapcsolat toohello virtuális gépet, és csatlakoztassa toohello internet.</span><span class="sxs-lookup"><span data-stu-id="374d1-118">These resources are used tooprovide network connectivity toohello virtual machine and connect it toohello internet.</span></span>

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location EastUS `
    -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location EastUS `
    -AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a><span data-ttu-id="374d1-119">Hozzon létre egy hálózati biztonsági csoportot és egy hálózati biztonsági csoportszabályt.</span><span class="sxs-lookup"><span data-stu-id="374d1-119">Create a network security group and a network security group rule.</span></span> 
<span data-ttu-id="374d1-120">hello hálózati biztonsági csoport hello virtuális gépet a bejövő és kimenő szabályok használatával titkosítja.</span><span class="sxs-lookup"><span data-stu-id="374d1-120">hello network security group secures hello virtual machine using inbound and outbound rules.</span></span> <span data-ttu-id="374d1-121">Ebben az esetben létrejön egy bejövő szabály a 3389-es porthoz, amely lehetővé teszi a bejövő távoli asztali kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="374d1-121">In this case, an inbound rule is created for port 3389, which allows incoming remote desktop connections.</span></span> <span data-ttu-id="374d1-122">Szeretnénk továbbá egy bejövő forgalomra vonatkozó szabály toocreate port 80, amely lehetővé teszi a bejövő webes forgalomnak.</span><span class="sxs-lookup"><span data-stu-id="374d1-122">We also want toocreate an inbound rule for port 80, which allows incoming web traffic.</span></span>

```powershell
# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
    -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
    -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location EastUS `
    -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP,$nsgRuleWeb
```

### <a name="create-a-network-card-for-hello-virtual-machine"></a><span data-ttu-id="374d1-123">A hálózati kártya hello virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="374d1-123">Create a network card for hello virtual machine.</span></span> 
<span data-ttu-id="374d1-124">Hozzon létre egy hálózati kártya [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="374d1-124">Create a network card with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) for hello virtual machine.</span></span> <span data-ttu-id="374d1-125">hello hálózati kártya hello virtuális gép tooa alhálózat, a hálózati biztonsági csoport és a nyilvános IP-cím csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="374d1-125">hello network card connects hello virtual machine tooa subnet, network security group, and public IP address.</span></span>

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a><span data-ttu-id="374d1-126">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="374d1-126">Create virtual machine</span></span>

<span data-ttu-id="374d1-127">Hozzon létre egy virtuálisgép-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="374d1-127">Create a virtual machine configuration.</span></span> <span data-ttu-id="374d1-128">A konfiguráció hello virtuális gépek például a virtuálisgép-lemezkép mérete és hitelesítési konfiguráció telepítésekor használt hello beállításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="374d1-128">This configuration includes hello settings that are used when deploying hello virtual machine such as a virtual machine image, size, and authentication configuration.</span></span> <span data-ttu-id="374d1-129">Ennek a lépésnek a futtatásakor a rendszer a hitelesítő adatok megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="374d1-129">When running this step, you are prompted for credentials.</span></span> <span data-ttu-id="374d1-130">megadott hello értékek hello felhasználónevet és jelszót hello virtuális géphez van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="374d1-130">hello values that you enter are configured as hello user name and password for hello virtual machine.</span></span>

```powershell
# Define a credential object
$cred = Get-Credential

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
```

<span data-ttu-id="374d1-131">Hozzon létre virtuális gép hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="374d1-131">Create hello virtual machine with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span>

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location EastUS -VM $vmConfig
```

## <a name="connect-toovirtual-machine"></a><span data-ttu-id="374d1-132">Csatlakoztassa a gépet toovirtual</span><span class="sxs-lookup"><span data-stu-id="374d1-132">Connect toovirtual machine</span></span>

<span data-ttu-id="374d1-133">Hello telepítés befejeződése után létre kell hoznia egy távoli asztali kapcsolat hello virtuális géppel.</span><span class="sxs-lookup"><span data-stu-id="374d1-133">After hello deployment has completed, create a remote desktop connection with hello virtual machine.</span></span>

<span data-ttu-id="374d1-134">Használjon hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) tooreturn hello nyilvános IP-cím hello virtuális gép parancsot.</span><span class="sxs-lookup"><span data-stu-id="374d1-134">Use hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) command tooreturn hello public IP address of hello virtual machine.</span></span> <span data-ttu-id="374d1-135">Jegyezze fel ezt az IP-címet, a böngésző tootest webes kapcsolatot egy későbbi lépésben kapcsolatba léphet a tooit.</span><span class="sxs-lookup"><span data-stu-id="374d1-135">Take note of this IP Address so you can connect tooit with your browser tootest web connectivity in a future step.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

<span data-ttu-id="374d1-136">Használjon hello következő parancsot a toocreate egy távoli asztali munkamenet hello virtuális géppel.</span><span class="sxs-lookup"><span data-stu-id="374d1-136">Use hello following command toocreate a remote desktop session with hello virtual machine.</span></span> <span data-ttu-id="374d1-137">Cserélje le hello IP-cím hello *publicIPAddress* a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="374d1-137">Replace hello IP address with hello *publicIPAddress* of your virtual machine.</span></span> <span data-ttu-id="374d1-138">Amikor a rendszer kéri, adja meg a hello virtuális gép létrehozásakor használt hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="374d1-138">When prompted, enter hello credentials used when creating hello virtual machine.</span></span>

```bash 
mstsc /v:<publicIpAddress>
```

## <a name="install-iis-via-powershell"></a><span data-ttu-id="374d1-139">IIS telepítése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="374d1-139">Install IIS via PowerShell</span></span>

<span data-ttu-id="374d1-140">Most, hogy az Azure virtuális gép toohello már bejelentkezett, használjon PowerShell tooinstall IIS egysoros, és hello helyi tűzfal szabály tooallow webes forgalom engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="374d1-140">Now that you have logged in toohello Azure VM, you can use a single line of PowerShell tooinstall IIS and enable hello local firewall rule tooallow web traffic.</span></span> <span data-ttu-id="374d1-141">Nyisson meg egy PowerShell-parancssorba, és futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="374d1-141">Open a PowerShell prompt and run hello following command:</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-hello-iis-welcome-page"></a><span data-ttu-id="374d1-142">Nézet hello IIS-kezdőlap</span><span class="sxs-lookup"><span data-stu-id="374d1-142">View hello IIS welcome page</span></span>

<span data-ttu-id="374d1-143">A telepített IIS-t, és most nyissa meg a virtuális gép hello Internet a 80-as porton az a choice tooview hello alapértelmezett IIS üdvözlőlap webböngésző is használhatja.</span><span class="sxs-lookup"><span data-stu-id="374d1-143">With IIS installed and port 80 now open on your VM from hello Internet, you can use a web browser of your choice tooview hello default IIS welcome page.</span></span> <span data-ttu-id="374d1-144">Lehet, hogy toouse hello *publicIpAddress* toovisit hello alapértelmezett oldal fent részletes ismertetését lásd.</span><span class="sxs-lookup"><span data-stu-id="374d1-144">Be sure toouse hello *publicIpAddress* you documented above toovisit hello default page.</span></span> 

![Alapértelmezett IIS-webhely](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="374d1-146">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="374d1-146">Clean up resources</span></span>

<span data-ttu-id="374d1-147">Ha már nincs szükség, használhatja a hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások parancsot.</span><span class="sxs-lookup"><span data-stu-id="374d1-147">When no longer needed, you can use hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="374d1-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="374d1-148">Next steps</span></span>

<span data-ttu-id="374d1-149">Ebben a rövid útmutatóban üzembe helyezett egy egyszerű virtuális gépet, egy hálózati biztonsági csoport szabályát, valamint telepített egy webkiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="374d1-149">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="374d1-150">További információ az Azure virtuális gépeken, toolearn toohello oktatóanyag Windows virtuális gépek továbbra is.</span><span class="sxs-lookup"><span data-stu-id="374d1-150">toolearn more about Azure virtual machines, continue toohello tutorial for Windows VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="374d1-151">Windowsos virtuális gépek az Azure-ban – oktatóanyagok</span><span class="sxs-lookup"><span data-stu-id="374d1-151">Azure Windows virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
